### [global]部分

把
>workgroup = MYGROUP   
改成
>workgroup = WORKGROUP

把
>security = user  
修改为 
>security = user  
>map to guest =Bad User

#### 然后在文件/etc/samba/smb.conf的最末尾处加入以下内容
```conf
[share]
    comment = share all
    path = /tmp/samba
    browseable = yes
    public = yes
    writable = no
```
####首先测试你配置的smb.conf是否正确，用下面的命令  
>testparm  
#### 启动samba服务
>/etc/init.d/smb start  

如果没有错误，则在你的windows机器上的浏览器中输入 file://IP/share 看是否能访问

