mysql有5个级别的权限，分别是：  
Clobal Level,Database Level,Table level,Column Level,Routine Level。

### Clobal Level：  
它是针对整个mysql数据库服务器的全局权限。对mysql里的某个数据库，或某个数据库的某张表的权限。所有的权限信息都存在mysql.user表中。

全局权限的设置语句：

>GRANT ALL ON *.* to 'root' @ 'localhost'


第一个*代表数据库名，这里是所有的数据库，第二个*代表表名。  
全局权限有ALTER ALTER ROUTINE CREATE ALL CREATE ROUTINE CREATE TEMPORARY TABLES CREATE USER CREATE VIEW DELETE All DROP All EXECUTE FILE All INTO FILE INDEX All INSERT All LOCK TABLES PROCESS All RELOAD All REPLICATION CLIENT SLAVE STATUS REPLICATION SLAVE SELECT SHOW DATABASES SHOW VIEW view SHUTDOWN SUPER UPDATE USAGE


### Database Level:  
数据库级别的权限，通过databasename.* 设置权限。设置语句如下：

>GRANT ALL ON databasename.* to 'root' @ 'localhost'
>
它会被global level的权限给覆盖掉，如有两条如下的权限设置语句：

>GRANT SELECT on test.* to 'root' @ 'localhost' ;    
>REVOKE SELECT ON *.* FROM 'root' @ 'localhost' ;  

'root'@'localhost'将不再对test拥有select权限。
数据库权限有： CREATE USER，FILE，PROCESS，RELOAD，REPLICATION CLIENT，REPLICATION SLAVE，SHOW DATABASES，SHUTDOWN，SUPER USAGE

### Table Level：  
表级别的权限能被全局权限和数据库级别权限覆盖

>GRANT SELECT ON test.test to 'root' @ 'localhost' ;    
>SHOW GRANTS FOR 'root' @ 'localhost' ;    

可以通过use选中某个数据库，直接对table名设置权限

>GRANT SELECT ON test to 'root' @ 'localhost' ;    

表的权限有：ALTER，CREATE，DELETE，DROP，INDEX，INSERT，SELECT UPDATE

### Column Level：   
表的某个列的权限。它会被前面三个权限给覆盖掉

>GRANT SELECT (id) ON test to 'root' @ 'localhost' ;    
>
字段级别的权限有INSERT，SELECT ，UPDATE

### Routine Level：    
是针对函数和存储过程的权限，他会被1,2,3个权限给覆盖掉。

>GRANT EXECUTE ON test.p to 'root' @ 'localhost' ;   
