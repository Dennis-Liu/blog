### 安装ttyd
```shell
#下载ttyd
wget -O ttyd https://github.com/tsl0922/ttyd/releases/download/1.6.0/ttyd_linux.x86_64
#添加执行权限
chmod +x ttyd
#移动目录
mv ttyd /usr/sbin
```

### 运行
>ttyd -p 1234 login

### nginx反向代理
```conf
location / {
    proxy_http_version 1.1;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_pass http://127.0.0.1:1234;
}
```
