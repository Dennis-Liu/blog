## 常用命令
获取镜像
>docker pull ubuntu  

查找镜像
>docker search httpd

删除镜像
>docker rmi hello-world
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

路径映射
>docker run -p 80:80 -v /data:/data -d nginx:latest

创建容器时忘了添加参数
>docker update --restart=always 容器名字/ID  
