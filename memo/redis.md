## linux设置redis密码redis  
#### 运行命令  
>redis-cli

#### 查看现有的redis密码(可选操作，可以没有)  
>config get requirepass 

#### 设置redis密码 (****为你要设置的密码)，设置成功的话会返回‘OK’字样  
>config set requirepass ****

#### auth（****为你设置的密码）登陆  
> auth '*****' 

## 项目刚启动的时候redis连接是没问题的，但是在一段 时间后就会出现连接超时的问题
#### 解决办法web服务器设置的timeout要小于Redis服务的timeout并且tcp-keepalive设置为10

```application.yml
  redis:
    #数据库索引
    database: ${REDIS_DB:0}
    host: ${REDIS_HOST:175.24.127.179}
    port: ${REDIS_PORT:6389}
    password: ${REDIS_PWD:root}
    #连接超时时间毫秒
    timeout: 8000
```

```redis.conf
timeout 10
tcp-keepalive 10
```