### 由于rabbitMq需要erlang语言的支持，在安装rabbitMq之前需要安装erlang，执行命令：
```linux
apt-get install erlang-nox     # 安装erlang
erl    # 查看relang语言版本，成功执行则说明relang安装成功
```
### 加公钥
```linux
wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc | sudo apt-key add -
```
### 更新软件包
```linux
apt-get update
```
### 安装 RabbitMQ
```linux
apt-get install rabbitmq-server  #安装成功自动启动
```
这里遇到个问题
```linux
root@VM-0-11-ubuntu:/home/ubuntu# apt-get install rabbitmq-server
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libmysqlclient20 linux-modules-4.15.0-29-generic
Use 'apt autoremove' to remove them.
The following additional packages will be installed:
  socat
The following NEW packages will be installed:
  rabbitmq-server socat
0 upgraded, 2 newly installed, 0 to remove and 279 not upgraded.
Need to get 4,993 kB of archives.
After this operation, 6,712 kB of additional disk space will be used.
Do you want to continue? [Y/n] Abort.
```
系统不给输入y的机会，直接默认Abort后自动退出安装，这种情况可以在输入命令时候提前加入 -y
```linux
apt-get install rabbitmq-server -y
```

### 查看 RabbitMq状态
```linux
systemctl status rabbitmq-server   #Active: active (running) 说明处于运行状态

# service rabbitmq-server status 用service指令也可以查看，同systemctl指令
```
### 启动、停止、重启
```linux
service rabbitmq-server start    # 启动
service rabbitmq-server stop     # 停止
service rabbitmq-server restart  # 重启 
```
执行了上面的步骤，rabbitMq已经安装成功。

### 启用 web端可视化操作界面，我们还需要配置Management Plugin插件
```linux
rabbitmq-plugins enable rabbitmq_management   # 启用插件
service rabbitmq-server restart    # 重启
```
此时，应该可以通过 http://localhost:15672 查看，使用默认账户guest/guest 登录。  
注意：RabbitMQ 3.3 及后续版本，guest 只能在服务本机登录。  
瞄了一眼官方文档，说的是默认会创建guest用户，但是只能服务器本机登录，建议创建其他新用户，授权，用来做其他操作。  

#### 查看用户
```linux
rabbitmqctl list_users
```
### 添加管理用户
```linux
rabbitmqctl add_user admin yourpassword   # 增加普通用户
rabbitmqctl set_user_tags admin administrator    # 给普通用户分配管理员角色 
```
ok，你可以在你的浏览器上输入：http://服务器Ip:15672/ 来访问你的rabbitmq监控页面。使用刚刚添加的新用户登录。  

