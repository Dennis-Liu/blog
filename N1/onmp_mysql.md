## 安装部署Mysql后外部不能访问的问题排查
1、mysql用户权限问题。mysql初始安装后，默认给root用户只分配了localhost和127.0.0.1两个host，即在mysql库的user表中，user为root的两行记录的host字段值分别为localhost和127.0.0.1。这个host字段就是Mysql用于通过客户机IP进行访问控制的。这种情况下，可以新增一条记录或者修改已有记录的host字段两种方式，将root用户的host字段值改为‘%’，或者是特定客户端即得IP，比如192.168.0.11等等。

2、机器防火墙的问题。如果安装Mysql的机器打开了防火墙，并且没有开放Mysql监听端口3306（或用户自己指定的端口），也会导致外部机器不能正常访问。这种情况下，修改防火墙设置即可，比如Ubuntu系统上通常使用iptables防火墙软件，编辑iptables配置增加以下配置项即可：

-A INPUT -m state –state NEW -m tcp -p tcp –dport 3306 -j ACCEPT

更详细的信息可以网上搜索。

3、还有一种情况，就是Mysql服务启动时绑定了127.0.0.1。这种情况可以通过netstat工具查看Mysql绑定的IP和端口，如果绑定了127.0.0.1也会导致外部机器不可访问。这是可以修改mysql的配置文件。比如在Ubuntu上，在使用apt-get安装的情况下，Mysql的配置文件通常在/etc/mysql/mysql.conf.d/mysqld.cnf，把其中一行bind-address = 127.0.0.1注释掉或者改为mysql本机的外部IP即可。
