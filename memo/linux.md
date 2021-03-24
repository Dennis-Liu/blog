批量创建文件夹:
>mkdir {FailedStepScreenshot,xml,SuccessfulStepScreenshot,vendor,features,library}

软链接
>ln -s /var/autotest/var/lib/jenkins/workspace/us_only_dev2_selenium_test/ us_only  


权限
>setfacl -R -m u:jenkins:rwx /opt  
>chmod jenkins 777 /opt  
>chown -R 101:101 behat/

查看当前文件夹大小
>du -sh

查找
>find / -name 'default'

删除当前文件夹下所有png格式的文件(rm -rf /*会删除整个根目录，且不可逆)
>rm -rf *.png

当删除文件太多或目录太多时会报错 -bash: /usr/bin/rm: Argument list too long,解决办法:
>rm -rf */*.xml  

替换成
>find . -name "*.xml" | xargs rm -rf "*.xml"

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

测试端口通不通
>telnet 1.2.3.4 3306  

刷票
```shell
For I in $(seq 1 100)
Do
Echo 'X-FORWARDED-FOR:58.'$i'.44.69';
Curl 'http://www.dramastar.org/tp/pnum.php' -H 'X-FORWARDED-FOR:58.'$i'.44.69' --data 'id=1899';
done
```

两条命令放在一行执行，第一条命令成功再执行第二条  
>cd /xx/xx&&svn update  

两条命令放在一行执行，第一条命令失败再执行第二条  
>cd /yy/yy||git pull  

In Bash, use ctrl-w to delete the last word, and ctrl-u to delete the content from current cursor back to the start of the line. Use alt-b and alt-f to move by word, ctrl-a to move cursor to beginning of line, ctrl-e to move cursor to end of line, ctrl-k to kill to the end of the line, ctrl-l to clear the screen. See man readline for all the default keybindings in Bash. There are a lot. For example alt-. cycles through previous arguments, and alt-* expands a glob.

Go to your home directory with cd. Access files relative to your home directory with the ~ prefix (e.g. ~/.bashrc). In sh scripts refer to the home directory as $HOME.

If you are halfway through typing a command but change your mind, hit alt-# to add a # at the beginning and enter it as a comment (or use ctrl-a, #, enter). You can then return to it later via command history.

pstree -p is a helpful display of the process tree.