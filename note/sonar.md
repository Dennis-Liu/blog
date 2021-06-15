### 拉取镜像
>docker pull postgres:10  
>docker pull sonarqube:7.9.1-community

### 运行容器
#### 启动postgres
```shell
docker run \
  -d \
  --name postgres10 \
  -p 5432:5432 \
  -e POSTGRES_USER=sonar \
  -e POSTGRES_PASSWORD=123456 \
  postgres:10
```

#### 启动SonarQube
```shell
docker run \
  -d \
  --name sonarqube7.9 \
  -p 9002:9000 \
  --link postgres10 \
  -e SONARQUBE_JDBC_URL=jdbc:postgresql://postgres10:5432/sonar \
  -e SONARQUBE_JDBC_USERNAME=sonar \
  -e SONARQUBE_JDBC_PASSWORD=123456 \
  -e SONARQUBE_WEB_JVM_OPTS=-Xms2048m -Xmx2048m -Xss512k \
  -v sonarqube_conf:/opt/sonarqube/conf \
  -v sonarqube_extensions:/opt/sonarqube/extensions \
  -v sonarqube_logs:/opt/sonarqube/logs \
  -v sonarqube_data:/opt/sonarqube/data \
  sonarqube:7.9.1-community
```

### 检查状态
>docker ps -a  

### 浏览器访问
http://192.168.16.200:9002  
admin/admin
### 迁移/新升级的Sonar没有显示项目或用户
删除/data/sonar/data/es5/* 后重启sonar
