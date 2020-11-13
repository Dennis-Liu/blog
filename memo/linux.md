批量创建文件夹:
>mkdir {FailedStepScreenshot,xml,SuccessfulStepScreenshot,vendor,features,library}

权限
>setfacl -R -m u:jenkins:rwx /opt  
>chmod jenkins 777 /opt

查看当前文件夹大小
>du -sh

查找
>find / -name 'default'

删除当前文件夹下所有png格式的文件(rm -rf /*会删除整个根目录，且不可逆)
>rm -rf *.png

查找大文件
>find -type f -size +100M  -print0 | xargs -0 du -h

查看端口占用
>netstat -an | grep 7055

清空文件内容
>: > xx.txt

下载
>sz /tmp/phpunitcheck/phpunit_data.csv

进程详细信息
>ll /proc/PID

以 root 帐户执行上一条命令
>sudo !!

切换到上一次访问的目录 
>cd -

将上一条命令中的 foo 替换为 bar，并执行 
>^foo^bar

调出上次命令使用的参数
>先按ESC再按.

查看firefox进程总数
>ps -ef | grep firefox | wc -l

刷票
```shell
For I in $(seq 1 100)
Do
Echo 'X-FORWARDED-FOR:58.'$i'.44.69';
Curl 'http://www.dramastar.org/tp/pnum.php' -H 'X-FORWARDED-FOR:58.'$i'.44.69' --data 'id=1899';
done
```