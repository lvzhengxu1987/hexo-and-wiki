---
title: mysql 存储过程 一个实际案例
date: 2016-07-17 06:54:07
tags: [mysql,procedure]
categories: 数据库
keywords: procedure
---

昨天因为公司的服务器压力过大，所以加了一个班，公司认证服务器的业务逻辑中有一个处理，就是要获取当前所有在线服务器的压力数据，之前的做法是循环发送sql查询语句去数据库那边统计每台服务器的压力数据，然后在认证服务器上把压力数据整合后统一返回请求方，这样做的后果是，有多少台在线服务器，就要频繁建立tcp链接，将sql查询语句的请求发送到数据库服务器上，还要等待返回数据，增加耗时。频繁的sql请求很影响认证服务器的处理速度,造成认证服务器响应速度慢，客户体验差，为了解决这个问题，就准备把统计负载数据这个功能放到数据库服务器自己身上来做了，使用存储过程来统计数据，然后用定时器每2秒定时触发存储过程，更新负载表中的数据.这样做的话，数据库这边有一定的压力，而认证服务器这边只是单纯的查询下表数据，然后就可以返回信息了。当然这种做法也有一定的局限性，在数据库中数据量较大的情况下，如果特别频繁的调用存储过程，会增加数据库的压力，但是如果定时器的间隔时间过长，则显示给用户的统计数据就会有一定的偏差。这之间的权衡.....
<!--more-->
在实现的过程中，发现在存储过程的代码中，在定义游标的过程中，会查询特定的表，在定义游标的sql语句之后，就不要在里面的代码里重复查询那个表了，会造成游标不知所踪。

在定义定时器时，一定要确认
```
mysql> SHOW VARIABLES LIKE '%sche%'; 
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| event_scheduler | OFF   |
+-----------------+-------+
1 row in set (0.00 sec)
```
如果 event_scheduler 是OFF的话，要用 ``SET GLOBAL event_scheduler = 1;  -- 启动定时器`` 打开它，否则定时任务是没法用的，当然``SET GLOBAL event_scheduler = 0;``可以关闭定时器.

建表语句，用来存储负载信息的
```  sql
CREATE TABLE IF NOT EXISTS xxx_server_load (
  -- server_id INT(10) NOT NULL AUTO_INCREMENT,
  server_id  INT(10) UNSIGNED NOT NULL,
  load_num INT(10) NOT NULL,
  PRIMARY KEY (server_id)
) ENGINE=InnoDB;

```

存储过程，从另外几个表中获取数据，来统计负载信息
``` sql
USE `数据库名称`;
DROP procedure IF EXISTS `do_load`;

DELIMITER $$
USE `数据库名称`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `do_load`()
BEGIN
DECLARE finish_flag INT UNSIGNED DEFAULT 0;
DECLARE server_ip   INT UNSIGNED DEFAULT 0;
DECLARE server_load INT UNSIGNED  DEFAULT 0;
DECLARE server_total_load INT UNSIGNED  DEFAULT 0;
DECLARE server_load_percent INT UNSIGNED  DEFAULT 0;

-- 声明游标
DECLARE svr_cursor CURSOR FOR 
	SELECT ip,serverload FROM xxx_servers where status=1;

-- 声明数据处理完毕的handler
DECLARE CONTINUE HANDLER 
FOR NOT FOUND SET finish_flag = 1;

OPEN svr_cursor;

get_svr: LOOP

FETCH svr_cursor INTO server_ip,server_total_load;

IF finish_flag = 1 THEN
LEAVE get_svr;
END IF;

SELECT 
    COUNT(1)
INTO server_load FROM
    (SELECT 
        COUNT(1) AS num
    FROM
        xxx_landlog
    LEFT JOIN xxx_vpslog ON xxx_landlog.vpnsession = xxx_vpslog.vpnsession
    WHERE
        xxx_landlog.sessiontime = 0
            AND xxx_vpslog.serip = server_ip
            AND xxx_vpslog.stoptime IS NULL
    GROUP BY xxx_vpslog.username
    ORDER BY xxx_vpslog.starttime DESC) AS a;

-- SELECT     serverload INTO server_total_load FROM     xxx_servers WHERE    xxx_servers.ip = server_ip;
SELECT (server_load/server_total_load)*100 INTO server_load_percent;
-- SET server_load_percent = (server_load/server_total_load)*100;
-- INSERT INTO xxx_server_load (server_id,load_num) VALUES (server_ip,server_load_percent) ON DUPLICATE KEY UPDATE load_num=server_load_percent;
INSERT INTO xxx_server_load (server_id,load_num) VALUES (server_ip,server_load_percent) ON DUPLICATE KEY UPDATE load_num=server_load_percent;
END LOOP get_svr; 
CLOSE svr_cursor;
END$$

DELIMITER ;
```

创建定时任务
``` sql
CREATE EVENT IF NOT EXISTS event_do_load
  ON SCHEDULE every 2 second
  ON COMPLETION  PRESERVE
  do call do_load;

```

#### 参考地址:
[非常好的存储过程教程](http://www.mysqltutorial.org/mysql-stored-procedure-tutorial.aspx)
