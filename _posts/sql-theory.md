---
title: sql-理论基础
date: 2016-06-20 06:25:01
tags: sql
categories: 数据库
keywords: 数据库
---

#### 数据库左右连接
数据库在通过连接两张或多张表来返回记录时，都会生成一张中间的临时表，然后再将这张临时表返回给用户。
在使用left jion时，on和where条件的区别如下：
1. on条件是在生成临时表时使用的条件，它不管on中的条件是否为真，都会返回左边表中的记录。
2. where条件是在临时表生成好后，再对临时表进行过滤的条件。这时已经没有left join的含义（必须返回左边表的记录）了，条件不为真的就全部过滤掉。
<!--more-->

#### group by 和 having 子句
1. GROUP BY 是分组查询, 一般 GROUP BY 是和聚合函数配合使用
group by 有一个原则,就是 select 后面的所有列中,没有使用聚合函数的列,必须出现在 group by 后面（重要）
2. Having
where 子句的作用是在对查询结果进行分组前，将不符合where条件的行去掉，即在分组之前过滤数据，条件中不能包含聚组函数，使用where条件显示特定的行。having 子句的作用是筛选满足条件的组，即在分组之后过滤数据，条件中经常包含聚组函数，使用having 条件显示特定的组，也可以使用多个分组标准进行分组。having 子句被限制子已经在SELECT语句中定义的列和聚合表达式上。通常，你需要通过在HAVING子句中重复聚合函数表达式来引用聚合值，就如你在SELECT语句中做的那样。例如：``SELECT A COUNT(B) FROM TABLE GROUP BY A HAVING COUNT(B)>2``
