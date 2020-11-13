本地win10, cmd输入
>ssh-keygen -t rsa -b 4096  

记事本打开
>.ssh\id_rsa.pub  

复制内容到远程linux的
>~/.ssh/authorized_keys  

保存

?>注意config要在.ssh\id_rsa.pub相同目录
