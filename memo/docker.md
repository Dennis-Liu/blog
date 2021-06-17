## 常用命令
获取镜像
>docker pull ubuntu  

查找镜像
>docker search httpd

删除镜像
>docker rmi hello-world

保存镜像
>docker save ID > xxx.tar  
>docker load < xxx.tar  
-------------

启动容器
>docker run -it ubuntu /bin/bash  
>docker run -d -p 5000:5000 training/webapp python app.py  
>docker run -d -p 127.0.0.1:5001:5000 training/webapp python app.py  
>docker-compose up -d 

后台运行
>docker run -itd --name ubuntu-test ubuntu /bin/bash  

进入容器
>docker exec -it 243c32535da7 /bin/bash

删除容器
>docker rm -f 1e560fca3906

查看容器
>docker ps

保存容器
>docker export ID >xxx.tar  
>docker import xxx.tar containr:v1  

然后再  
>docker run -it containr:v1 bash
-------------

网络端口的快捷方式
>docker port wizardly_chandrasekhar 

查看日志
>docker logs --tail 100 kiwi_web

查看容器的进程
>docker top

查看 Docker 的底层信息
>docker inspect

停止容器
>docker stop kiwi_web

重启容器
>docker restart kiwi_web

Docker的数据持久化即使数据不随着container的结束而结束，数据存在于host机器上——要么存在于host的某个指定目录中（使用bind mount），要么使用docker自己管理的volume（/var/lib/docker/volumes下）
>docker run -p 80:80 -v /data:/data -d nginx:latest

创建容器时忘了添加参数
>docker update --restart=always 容器名字/ID  
