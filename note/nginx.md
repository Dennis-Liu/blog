### win10配置nginx和php  
下载稳定版的nginx
http://nginx.org/download

修改php.ini
>cgi.fix_pathinfo=1

修改nginx.conf
>location / ->root E:/site/html;  
>location / ->index index.php index.html index.htm;  
>location = /50x.html->root E:/site/html;  
```conf
location ~ \.php$ {
    root D:\Dennis\\nginx-1.18.0\html;
    fastcgi_pass   127.0.0.1:9090;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
```

准备启动脚本  
phpcgi.bat
```bat
@echo off
echo Starting PHP FastCGI…
"D:\Program Files\php-7.1.0-nts-Win32-VC14-x64\php-cgi.exe" -b 127.0.0.1:9090 -c "D:\Program\ Files\php-7.1.0-nts-Win32-VC14-x64\php.ini"
```
nginx.bat
```bat
@echo off
echo Starting nginx…
"D:\Program Files\nginx-1.11.8\nginx.exe" -p "E:\site"
```
stop.bat
```bat
echo Stopping nginx…
taskkill /F /IM nginx.exe > nul
echo Stopping PHP FastCGI…
taskkill /F /IM php-cgi.exe > nul
pause
```

### ubuntu配置nginx和php  
修改php.ini  
>cgi.fix_pathinfo=1  

添加soap.conf  
```conf
server {
    listen       8080; # 监听端口
    server_name  localhost; # 服务名
    charset utf-8; # 字符集，可处理中文乱码
    access_log  /var/log/nginx/default.access.log;
    error_log /var/log/nginx/default.error.log;
    root   /var/www/html;
    location / {
        index  index.html index.htm;
    }
    location ~ \.php$ {
        fastcgi_pass   unix:/run/php/php7.4-fpm.sock;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
}
```
报错nginx: [error] open() "/run/nginx.pid" failed (2: No such file or directory),确保nginx.conf里配置了pid的路径，然后去/usr/sbin下执行  
>./nginx

报错nginx connect() failed (111: Connection refused) while connecting to upstream.  执行
>netstat -ant | grep 9000  

确保php-fpm已经启动。如果php-fpm已经启动但是搜不到9000端口，则去/etc/php/7.4/fpm/pool.d/www.conf 查看listen属性发现listen = /run/php/php7.4-fpm.sock
因为nginx和php有两种链接方式，一种是  
>fastcgi_pass 127.0.0.1:9000;  

另一种是这个  
>fastcgi_pass unix:/run/php/php7.0-fpm.sock;  

所以配置第二种方式



### nginx配置文件目录可以被访问
```conf
server {
    listen       8088; # 监听端口
    server_name  10.126.0.80; # 服务名
    charset utf-8; # 字符集，可处理中文乱码
    location / {
        autoindex on; # 开启目录浏览功能
        autoindex_exact_size off; # 详细文件大小统计，让文件大小显示MB，GB单位，默认为b
        autoindex_localtime on; # 服务器本地时区显示文件修改日期
        root /var/autotest/opt/dennis/failCaseScreenshot/; # 指定实际目录绝对路径
    }
}
```

### 然后执行reload
>nginx -s reload  