## pull docker image
>docker pull kiwitcms/kiwi
## Start Docker compose
Copy below docker-compose.yml and place it in a new directory
```yml
version: '2'

services:
    db:
        container_name: kiwi_db
        image: centos/mariadb-103-centos7
        volumes:
            - db_data:/var/lib/mysql/data
            # workaround for missing charset & collation support
            # https://github.com/sclorg/mariadb-container/pull/125
            - ./99-charset.sh:/usr/share/container-scripts/mysql/init/99-charset.sh:Z
            - ./99-charset.cnf:/usr/share/container-scripts/mysql/cnf/99-charset.cnf:Z
        restart: always
        environment:
            MYSQL_ROOT_PASSWORD: kiwi-1s-aw3s0m3
            MYSQL_DATABASE: kiwi
            MYSQL_USER: kiwi
            MYSQL_PASSWORD: kiwi
            MYSQL_CHARSET: utf8mb4
            MYSQL_COLLATION: utf8mb4_unicode_ci

    web:
        container_name: kiwi_web
        depends_on:
            - db
        restart: always
        image: kiwitcms/kiwi:latest
        ports:
            - 80:8080
            - 443:8443
        volumes:
            - uploads:/Kiwi/uploads:Z
        environment:
            KIWI_DB_HOST: db
            KIWI_DB_PORT: 3306
            KIWI_DB_NAME: kiwi
            KIWI_DB_USER: kiwi
            KIWI_DB_PASSWORD: kiwi

volumes:
    db_data:
    uploads:
```
>cd /opt/kiwi/docker-compose.yml  
>docker-compose up -d

## create the database schema by executing
>docker exec -it kiwi_web /Kiwi/manage.py migrate
## create the first user account
>docker exec -it kiwi_web /Kiwi/manage.py createsuperuser
## Kiwi TCMS configuration settings
>/venv/lib64/python3.6/site-packages/tcms/settings/common.py
```python
# -*- coding: utf-8 -*-

import os
from importlib import import_module

import pkg_resources
from django.contrib.messages import constants as messages
from django.urls import reverse_lazy
from django.utils.translation import gettext_lazy as _

import tcms

# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# ~~ You have to override the following settings in product.py


# Set to False for production
DEBUG = True


# Make this unique, and don't share it with anybody.
SECRET_KEY = 'xxxxxx'  # nosec:B105


# Database settings
DATABASES = {
    'default': {
        'ENGINE': os.environ.get('KIWI_DB_ENGINE', 'django.db.backends.mysql'),
        'NAME': os.environ.get('KIWI_DB_NAME', 'kiwi'),
        'USER': os.environ.get('KIWI_DB_USER', 'kiwi'),
        'PASSWORD': os.environ.get('KIWI_DB_PASSWORD', 'kiwi'),
        'HOST': os.environ.get('KIWI_DB_HOST', ''),
        'PORT': os.environ.get('KIWI_DB_PORT', ''),
        'OPTIONS': {},
    },
}

# handle MariaDB only options
if DATABASES['default']['ENGINE'].find('mysql') > -1:
    DATABASES['default']['OPTIONS'].update({  # pylint: disable=objects-update-used
        'init_command': "SET sql_mode='STRICT_TRANS_TABLES'",
        'charset': 'utf8mb4',
    })


# Administrators error report email settings
ADMINS = [
    # ('Dennis', 'hongwei.liu@dhc.com.cn'),
]


# Email settings
# DEFAULT_FROM_EMAIL must be defined if you want Kiwi TCMS to send emails.
# You also need to configure the email backend. For more information see:
# https://docs.djangoproject.com/en/3.0/topics/email/
# SERVER_EMAIL is used by the logging backend to send exceptions to ADMINS
SERVER_EMAIL = DEFAULT_FROM_EMAIL = 'kiwi@xxx.com'
EMAIL_SUBJECT_PREFIX = '[Kiwi-TCMS] '

#  SMTP specific settings
EMAIL_HOST = 'xxx.smtp.xxx.com'
EMAIL_PORT = 25
#  EMAIL_HOST_USER = 'smtp_username'
#  EMAIL_HOST_PASSWORD = 'smtp_password'


# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# ~~ You may want to override the following settings as well


# Hosts/domain names that are valid for this site; required if DEBUG is False
# See https://docs.djangoproject.com/en/1.11/ref/settings/#allowed-hosts
ALLOWED_HOSTS = ['*']


# default group in which new users will be created
DEFAULT_GROUPS = ['Tester']


# When set to False site administrators will have to manually approve
# new users. You can combine this with tcms.signals.notify_admins() signal
# handler!
AUTO_APPROVE_NEW_USERS = True


# How often will session cookies expire? We set this to 24hrs by default.
# You may override based on your security policies
# https://docs.djangoproject.com/en/2.1/ref/settings/#session-cookie-age
SESSION_COOKIE_AGE = 86400


# Maximum upload file size, default set to 5MB.
FILE_UPLOAD_MAX_SIZE = 5242880


# Controls if django-attachments deletes files from disk
DELETE_ATTACHMENTS_FROM_DISK = True


# Configure a caching backend. ATM only used to cache bug details b/c
# external issue trackers may be slow. If you want to override see:
# https://docs.djangoproject.com/en/2.2/topics/cache/
# https://docs.djangoproject.com/en/2.2/ref/settings/#std:setting-CACHES
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.locmem.LocMemCache',
        'LOCATION': 'kiwitcms',
        'TIMEOUT': 3600,
    }
}

# Absolute path to the directory static files should be collected to.
# Don't put anything in this directory yourself; store your static files
# in apps' "static/" subdirectories and in STATICFILES_DIRS.
# Example: "/var/www/example.com/static/"
STATIC_ROOT = '/Kiwi/static/'


# WARNING: Do not change this unless you know what you are doing !!!
# If you want to allow read-only access to anonymous users you can disable
# global_login_required.GlobalLoginRequiredMiddleware below!
MIDDLEWARE = [
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.locale.LocaleMiddleware',
    'django.middleware.common.CommonMiddleware',
    'tcms.core.middleware.CsrfDisableMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'global_login_required.GlobalLoginRequiredMiddleware',
    'simple_history.middleware.HistoryRequestMiddleware',
    'tcms.core.middleware.CheckSettingsMiddleware',
    'tcms.core.middleware.CheckUnappliedMigrationsMiddleware',
]


# You can also list additional views which will be available to
# anonymous users here. Take care to keep the default ones!
PUBLIC_VIEWS = [
    'modernrpc.views.RPCEntryPoint',
    'django.contrib.auth.views.LoginView',
    'django.contrib.auth.views.LogoutView',
    'django.contrib.auth.views.PasswordResetView',
    'django.contrib.auth.views.PasswordResetDoneView',
    'django.contrib.auth.views.PasswordResetConfirmView',
    'django.contrib.auth.views.PasswordResetCompleteView',
    'tcms.kiwi_auth.views.LoginViewWithCustomTemplate',
    'tcms.kiwi_auth.views.PasswordResetView',
    'tcms.kiwi_auth.views.Register',
    'tcms.kiwi_auth.views.Confirm',
    'tcms.core.views.NavigationView',
    'tcms.core.views.TranslationMode',
]


# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# ~~ DANGER: Don't change the settings below!

SITE_ID = 1

KIWI_VERSION = tcms.__version__

MANAGERS = ADMINS

LOGIN_REDIRECT_URL = reverse_lazy('core-views-index')

# internal
TCMS_ROOT_PATH = os.path.abspath(os.path.join(os.path.dirname(__file__), '..').replace('\\', '/'))

# If you set this to False, Django will make some optimizations so as not
# to load the internationalization machinery.
USE_I18N = True

# If you set this to False, Django will not format dates, numbers and
# calendars according to the current locale.
USE_L10N = True

# Language code for this installation. All choices can be found here:
# See https://code.djangoproject.com/ticket/29713
LANGUAGE_CODE = 'en-us'

LOCALE_PATHS = [
    os.path.join(TCMS_ROOT_PATH, 'locale'),
]

# If you set this to False, Django will not use timezone-aware datetimes.
# See https://docs.djangoproject.com/en/2.2/topics/i18n/timezones/
USE_TZ = os.environ.get('KIWI_USE_TZ', 'False').lower() == 'true'

# Local time zone for this installation. Choices can be found here:
# http://en.wikipedia.org/wiki/List_of_tz_zones_by_name
TIME_ZONE = os.environ.get('KIWI_TIME_ZONE', 'Etc/UTC')

# Absolute filesystem path to the directory that will hold user-uploaded files.
MEDIA_ROOT = '/Kiwi/uploads'

# URL that handles the media served from MEDIA_ROOT. Make sure to use a
# trailing slash.
# Examples: "http://example.com/media/", "http://media.example.com/"
MEDIA_URL = '/uploads/'

# URL prefix for static files.
# Example: "http://example.com/static/", "http://static.example.com/"
STATIC_URL = '/static/'

# Additional locations of static files
STATICFILES_DIRS = [
    # Put strings here, like "/home/html/static" or "C:/www/django/static".
    # Always use forward slashes, even on Windows.
    # Don't forget to use absolute paths, not relative paths.
    os.path.join(TCMS_ROOT_PATH, 'static').replace('\\', '/'),
    os.path.join(TCMS_ROOT_PATH, 'node_modules').replace('\\', '/'),
]

# List of finder classes that know how to find static files in
# various locations.
STATICFILES_FINDERS = [
    'django.contrib.staticfiles.finders.FileSystemFinder',
    'django.contrib.staticfiles.finders.AppDirectoriesFinder',
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
            os.path.join(TCMS_ROOT_PATH, 'templates/').replace('\\', '/'),
        ],
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.static',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                'django.template.context_processors.i18n',

                'tcms.core.context_processors.request_contents_processor',
                'tcms.core.context_processors.settings_processor',
                'tcms.core.context_processors.server_time_processor',
            ],
            'loaders': [
                'django.template.loaders.filesystem.Loader',
                'django.template.loaders.app_directories.Loader',
            ]
        },
    },
]

ROOT_URLCONF = 'tcms.urls'

# Python dotted path to the WSGI application used by Django's runserver.
WSGI_APPLICATION = 'tcms.wsgi.application'

# DANGER: DO NOT EDIT, see git log !!!
# this is consumed by kiwitcms-tenants/django-tenants
TENANT_APPS = [
    'django.contrib.sites',

    'attachments',
    'django_comments',
    'modernrpc',
    'simple_history',

    'tcms.core.contrib.linkreference',
    'tcms.management',
    'tcms.testcases.apps.AppConfig',
    'tcms.testplans.apps.AppConfig',
    'tcms.testruns.apps.AppConfig',
]

# if you wish to disable Kiwi TCMS bug tracker
# define the KIWI_DISABLE_BUGTRACKER ENV variable
if os.environ.get('KIWI_DISABLE_BUGTRACKER') is None:
    TENANT_APPS.append('tcms.bugs.apps.AppConfig')


INSTALLED_APPS = TENANT_APPS + [
    'grappelli',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.messages',
    'django.contrib.sessions',
    'django.contrib.staticfiles',

    'colorfield',
    'django_extensions',
    'vinaigrette',

    'tcms.core',
    'tcms.kiwi_auth',
    'tcms.telemetry',
    'tcms.rpc',
]

for plugin in pkg_resources.iter_entry_points('kiwitcms.plugins'):
    INSTALLED_APPS.append(plugin.module_name)

# this is the main navigation menu
MENU_ITEMS = [
    (_('DASHBOARD'), reverse_lazy('core-views-index')),
    (_('TESTING'), [
        (_('New Test Plan'), reverse_lazy('plans-new')),
        ('-', '-'),
        (_('New Test Case'), reverse_lazy('testcases-new')),
        ('-', '-') if 'tcms.bugs.apps.AppConfig' in INSTALLED_APPS else (),
        (_('New Bug'),
         reverse_lazy('bugs-new')) if 'tcms.bugs.apps.AppConfig' in INSTALLED_APPS else (),
    ]),
    (_('SEARCH'), [
        (_('Search Test Plans'), reverse_lazy('plans-search')),
        (_('Search Test Runs'), reverse_lazy('testruns-search')),
        (_('Search Test Cases'), reverse_lazy('testcases-search')),
        (_('Search Bugs'),
         reverse_lazy('bugs-search')) if 'tcms.bugs.apps.AppConfig' in INSTALLED_APPS else (),
    ]),
    (_('TELEMETRY'), [
        (_('Testing'), [
            (_('Breakdown'), reverse_lazy('testing-breakdown')),
            (_('Status matrix'), reverse_lazy('testing-status-matrix')),
            (_('Execution trends'), reverse_lazy('testing-execution-trends')),
            (_('TestCase health'), reverse_lazy('test-case-health')),
        ]),
        ('More coming soon',
         'http://kiwitcms.org/blog/kiwi-tcms-team/2019/03/03/legacy-reports-become-telemetry/'),
    ]),
    (_('ADMIN'), [
        (_('Users and groups'), '/admin/auth/'),
        ('-', '-'),
        (_('Everything else'), '/admin/'),
    ]),
    (_('PLUGINS'), [
    ]),
]

# last element is always PLUGINS so we can easily extend & override it
for plugin in pkg_resources.iter_entry_points('kiwitcms.plugins'):
    plugin_menu = import_module('%s.menu' % plugin.module_name)
    MENU_ITEMS[-1][1].extend(plugin_menu.MENU_ITEMS)

# redefine the help menu in the navigation bar
HELP_MENU_ITEMS = [
    ('https://kiwitcms.discourse.group', _('Forum')),
    ('https://github.com/kiwitcms/Kiwi/issues/new/choose', _('Report an Issue')),
    ('https://stackoverflow.com/questions/tagged/kiwi-tcms', _('Ask for help on StackOverflow')),
    ('https://opencollective.com/kiwitcms#section-contribute', _('Donate €5 via Open Collective')),
    ('http://kiwitcms.readthedocs.io/en/latest/admin.html', _('Administration Guide')),
    ('http://kiwitcms.readthedocs.io/en/latest/tutorial.html', _('User Guide')),
    ('http://kiwitcms.readthedocs.io/en/latest/api/index.html', _('API Help')),
]

SESSION_SERIALIZER = 'django.contrib.sessions.serializers.JSONSerializer'

SESSION_ENGINE = 'django.contrib.sessions.backends.cached_db'

# WARNING: do not edit. The stock JSONRPC handler does not HTML escape !!!
MODERNRPC_HANDLERS = ['modernrpc.handlers.XMLRPCHandler', 'tcms.handlers.SafeJSONRPCHandler']

# in alphabetic order
MODERNRPC_METHODS_MODULES = [
    'tcms.telemetry.api',
    'tcms.rpc.api.attachment',
    'tcms.rpc.api.auth',
    'tcms.rpc.api.bug',
    'tcms.rpc.api.build',
    'tcms.rpc.api.category',
    'tcms.rpc.api.classification',
    'tcms.rpc.api.component',
    'tcms.rpc.api.plantype',
    'tcms.rpc.api.priority',
    'tcms.rpc.api.product',
    'tcms.rpc.api.tag',
    'tcms.rpc.api.testcase',
    'tcms.rpc.api.testexecution',
    'tcms.rpc.api.testexecutionstatus',
    'tcms.rpc.api.testcasestatus',
    'tcms.rpc.api.testplan',
    'tcms.rpc.api.testrun',
    'tcms.rpc.api.user',
    'tcms.rpc.api.version',
]

if 'tcms.bugs.apps.AppConfig' in INSTALLED_APPS:
    MODERNRPC_METHODS_MODULES.append('tcms.bugs.api')

# This is a list of dotted class names providing integration with
# external bug trackers. Plugins and downstream installation can augment
# this list with their own integration classes!
# https://kiwitcms.readthedocs.io/en/latest/admin.html#configure-external-bug-trackers
EXTERNAL_BUG_TRACKERS = [
    'tcms.issuetracker.types.Bugzilla',
    'tcms.issuetracker.types.JIRA',
    'tcms.issuetracker.types.GitHub',
    'tcms.issuetracker.types.Gitlab',
    'tcms.issuetracker.types.Redmine',
]
if 'tcms.bugs.apps.AppConfig' in INSTALLED_APPS:
    EXTERNAL_BUG_TRACKERS.append('tcms.issuetracker.types.KiwiTCMS')

# Enable the administrator delete permission
# In another word it's set the admin to super user or not.
SET_ADMIN_AS_SUPERUSER = False

# Allows history_change_reason to be a TextField so we can
# support a changelog-like feature! DO NOT EDIT b/c migrations
# depend on this setting!
SIMPLE_HISTORY_HISTORY_CHANGE_REASON_USE_TEXT_FIELD = True

# Default page size when paginating queries
DEFAULT_PAGE_SIZE = 100

# A sample logging configuration. The only tangible logging
# performed by this configuration is to send an email to
# the site admins on every HTTP 500 error when DEBUG=False.
# See http://docs.djangoproject.com/en/dev/topics/logging for
# more details on how to customize your logging configuration.
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'verbose': {
            'format': '%(levelname)s %(asctime)s %(module)s %(process)d %(thread)d %(message)s'
        },
        'simple': {
            'format': '[%(asctime)s] %(levelname)s %(message)s'
        },
        'rpc_log': {
            'format': '[%(asctime)s] %(levelname)s XMLRPC %(process)d "%(message)s"'
        },
    },
    'filters': {
        'require_debug_false': {
            '()': 'django.utils.log.RequireDebugFalse'
        }
    },
    'handlers': {
        'console': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',
            'formatter': 'simple'
        },
        'rpc': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',
            'formatter': 'rpc_log',
        },
        'mail_admins': {
            'level': 'ERROR',
            'filters': ['require_debug_false'],
            'class': 'django.utils.log.AdminEmailHandler'
        },
    },
    'loggers': {
        'django.request': {
            'handlers': ['mail_admins'],
            'level': 'ERROR',
            'propagate': True,
        },
        'kiwi.rpc': {
            'handlers': ['rpc'],
            'level': 'DEBUG',
            'propagate': True,
        },
    }
}

# override default message tags to match Patternfly class names
MESSAGE_TAGS = {
    messages.ERROR: 'danger',
}
```

## customize api
tcms_api not provide create bug function
>cd /venv/lib64/python3.6/site-packages/tcms/bugs  
>vi api.py

add below function
```python
@permissions_required('bugs.add_bug_tags')
@rpc_method(name='Bug.create')
def create(data):
    text = data['text']
    del data['text']
    bug = Bug.objects.create(**data)
    add_comment([bug], text, bug.reporter)

    return bug
```
add `create` in `__all__` like below
```python
__all__ = (
    'create',
    'add_tag',
    'remove_tag',
    'filter',
    'remove',
)
```
test api
```python
from tcms_api import TCMS

rpc_client = TCMS()
rpc_client.exec.Bug.create({
    'summary':'create bug and assing to testcase owner by api',
    'text': 'create bug and assing to testcase owner by api',
    'product_id': 1,
    'reporter_id': 1,
    'version_id': 1,
    'build_id': 1,
    'assignee_id':rpc_client.exec.User.filter({'username':'autotester'})[0]['id']
})
```


## tcms-api

>pip3 install tcms-api  

face issue
>subprocess.CalledProcessError: Command 'krb5-config --libs gssapi' returned non-zero exit status 127.

install krb5-devel  
>yum install krb5-devel  

face issue
>gssapi/raw/misc.c:35:20: fatal error: Python.h: No such file or directory  
>#include "Python.h"  
>                    ^  
>compilation terminated.  
>error: command 'gcc' failed with exit status 1  

install python3-devel
>yum install python3-devel

## refer site
[kiwitcms](https://kiwitcms.readthedocs.io/en/latest/)
[kiwiapi](https://kiwitcms.readthedocs.io/en/latest/modules/tcms.rpc.api.html)