---
title: mysql常用命令及操作方式
date: 2016-06-19 01:18:32
tags: mysql
categories: 数据库
keywords: mysql 数据库
---

目录
* <a href="#Mysql-执行数据库脚本" >Mysql 执行数据库脚本</a>
* <a href="#mysql创建与删除用户" >mysql创建与删除用户</a>
* <a href="#Mysql权限" >Mysql权限问题</a>
* <a href="#Mysql修改密码" >Mysql修改密码</a>
* <a href="#mysql-存储过程和函数" >mysql 存储过程和函数</a>
* <a href="#mysqldump" >mysqldump</a>
* <a href="#mysql权限一览表" >mysql权限一览表</a>


#### Mysql 执行数据库脚本
``` sql 
--mysql执行sql脚本
mysql -h 127.0.0.1 -uroot  -p123456 < setup.sql
--带数据库名称的执行sql脚本
mysql -h 127.0.0.1 -uroot  -p123456 database_name < xxxx.sql
```
<!--more-->
#### mysql创建与删除用户
``` sql
--执行创建用户和删除用户后，最好执行一下FLUSH PRIVILEGES;
--创建一个用户username，位于host，密码是password
--请注意host，如果host如果是%，则代表着在任意ip地址都可以用这个用户登录
--如果host是localhost或者127.0.0.1，那么只能本地登录
--如果host是某个指定的ip，那么就只能由那个ip来的username登陆了
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
-- mysql查看有哪些用户
SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;
select host,user,password from user;
--删除某个用户
DROP USER 'username'@'host';
```

#### Mysql权限
``` sql
--执行授权修正后，执行FLUSH PRIVILEGES;
--增加某些授权给用户
--例子: GRANT ALL ON *.* TO 'pig'@'%';
GRANT privileges ON databasename.tablename TO 'username'@'host'
--删除某个用户的权限
--例子:REVOKE SELECT ON *.* FROM 'pig'@'%';
REVOKE privilege ON databasename.tablename FROM 'username'@'host';
--查看某个用户的权限,mysql user 表是 mysql.user
show grants for 'radius'@'localhost';
--授权给某个用户数据库的权限,其中identified by后面是密码，此语句会按照这个参数修改用户的密码的
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP ON radius.* TO radius@'localhost' identified by 'radpass';
```

#### Mysql修改密码
``` sql
--如果用户密码不对，可以通过一下手段修改mysql密码
$ mysql -uroot -p123456
mysql>use mysql;
mysql>UPDATE user SET password=PASSWORD('radpass') WHERE user='radius';
mysql>FLUSH PRIVILEGES;
--如果root的密码忘记了，此时修改密码会比较麻烦
--1.关闭mysql 服务 service mysql stop
--2.编辑 my.cnf，在mysqld中添加 skip-grant-tables
--3.重新启动数据库 service mysql start
--4.修改root密码 进入数据库 mysql -p 然后
update mysql.user set password=password('rootroot') where user='root' and host='%';
--5.将my.cnf中的skip-grant-tables注释掉
--6.重启mysql service mysql restart
```

#### mysql 存储过程和函数
查看当前存储过程
方法一：``select `name` from MySQL.proc where db = 'your_db_name' and `type` = 'PROCEDURE'``
方法二：查看数据库里所有存储过程+内容 ``show procedure status;``
方法三: 查看当前数据库里存储过程列表 ``select specific_name from mysql.proc;``
方法四：(查看某一个存储过程的具体内容) ``select body from mysql.proc where specific_name = 'your_proc_name';``

查看存储过程或函数的创建实现代码
``show create procedure proc_name;``
``show create function func_name;``

删除存储过程 ``drop procedure your_proc_name;``

#### mysqldump 
``mysqldump -uroot -ppwdqweasd radiuslx svrlist;``
其中radiuslx是database的name;svrlist是table名称,当然也可以是 view 名称等等.
如果想要导出表中部分数据的话，可以加上where条件，也可以加其他限制信息，等等。
例如``mysqldump -uuser -pxx database_name tab_name --where=" dateline > 1277495460" > a.sql``

``mysqldump -uroot -ppwdqweasd radiuslx; ``
如果不加入表名，默认就会导出这个数据库的所有的表


#### mysql权限一览表

权限名称|权限解释
:--------|:--------
ALTER	|Allows use of ALTER TABLE.
ALTER ROUTINE|	Alters or drops stored routines.
CREATE|	Allows use of CREATE TABLE.
CREATE ROUTINE|	Creates stored routines.
CREATE TEMPORARY TABLE|	Allows use of CREATE TEMPORARY TABLE.
CREATE USER|Allows use of CREATE USER, DROP USER, RENAME USER, and REVOKE ALL PRIVILEGES.
CREATE VIEW|Allows use of CREATE VIEW.
DELETE|Allows use of DELETE.
DROP|Allows use of DROP TABLE.
EXECUTE|Allows the user to run stored routines.
FILE|Allows use of SELECT... INTO OUTFILE and LOAD DATA INFILE.
INDEX|Allows use of CREATE INDEX and DROP INDEX.
INSERT|Allows use of INSERT.
LOCK TABLES|Allows use of LOCK TABLES on tables for which the user also has SELECT privileges.
PROCESS|Allows use of SHOW FULL PROCESSLIST.
RELOAD|Allows use of FLUSH.
REPLICATION|Allows the user to ask where slave or master
CLIENT|servers are.
REPLICATION SLAVE|Needed for replication slaves.
SELECT|Allows use of SELECT.
SHOW DATABASES|Allows use of SHOW DATABASES.
SHOW VIEW|Allows use of SHOW CREATE VIEW.
SHUTDOWN|Allows use of mysqladmin shutdown.
SUPER|Allows use of CHANGE MASTER, KILL, PURGE MASTER LOGS, and SET GLOBAL SQL statements. Allows mysqladmin debug command. Allows one extra connection to be made if maximum connections are reached.
UPDATE|Allows use of UPDATE.
USAGE|Allows connection without any specific privileges.
