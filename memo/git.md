## 搭建Git服务器  
### 第一步，安装
>sudo apt-get install git  

### 第二步，创建一个git用户，用来运行git服务
>sudo adduser git  

### 第三步，创建证书登录：  

收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。  

### 第四步，初始化Git仓库：  
>sudo git init --bare sample.git  

Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：  
>sudo chown -R git:git sample.git  

### 第五步，克隆远程仓库：  
>git clone git@server:/srv/sample.git  