---
title: mysql-优化
date: 2016-07-16 16:39:29
tags: [mysql,优化]
categories: 数据库
keywords: mysql优化
---

#### 修改mysql连接数
``` sql
--查看并修改最大连接数, 这种方式有一种缺陷，
--那就是这个配置生效后，如果mysql服务重新启动，
--这个配置就会丢失，如果要设置的话，就需要重新
--进入mysql中执行这些命令
show variables like 'max_connections'; 
set global max_connections=1000;

--另一种方式是，修改mysql的配置文件 my.cnf,
--找到mysqld编辑它，找到mysqld启动的那两行，在后面加上参数 ：
max_connections=1500
```
#### 去掉mysql域名解析功能
当远程访问mysql时，mysql会解析域名，会导致访问速度很慢，加上下面这个配置可解决此问题 
解决办法,在mysqld下面添加这行代码，然后重启mysql
[mysqld] 
skip-name-resolve 
但是，这样会引起一个问题：连接mysql时，不能使用 localhost连接了，而是要使用IP地址的；如果是按localhost对用户赋权限的话，用户登录权限也要修改一下的。 

