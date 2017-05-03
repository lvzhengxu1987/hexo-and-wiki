---
title: mysql 常用sql操作
date: 2017-05-02 01:43:10
tags: mysql
categories: 数据库
keywords: mysql 数据库
---

查询mysql表中记录总数

``` sql
SELECT COUNT(1) FROM tablename WHERE someCondition
```


一次同时查询多个表的记录条数

``` sql
SELECT
  (SELECT COUNT(*) FROM table1 WHERE someCondition) as table1Count,
  (SELECT COUNT(*) FROM table2 WHERE someCondition) as table2Count,
  (SELECT COUNT(*) FROM table3 WHERE someCondition) as table3Count
-- ---------------------------------------------------------------------
SELECT COUNT(*) FROM table1 WHERE someCondition
UNION
SELECT COUNT(*) FROM table2 WHERE someCondition
UNION
SELECT COUNT(*) FROM table3 WHERE someCondition
-- ---------------------------------------------------------------------
SELECT
    COUNT(table1.*) as t1,
    COUNT(table2.*) as t2,
    COUNT(table3.*) as t3
FROM table1
    LEFT JOIN tabel2 ON condition
    LEFT JOIN tabel3 ON condition
```

Duplicating a MySQL table, indexes and data

``` sql
CREATE TABLE new_table LIKE old_table; INSERT new_table SELECT * FROM old_table;

MyISAM 

CREATE TABLE copy LIKE original;
ALTER TABLE copy DISABLE KEYS
INSERT INTO copy SELECT * FROM original;
ALTER TABLE copy ENABLE KEYS;

InnoDB

SET unique_checks=0; SET foreign_key_checks=0; 
..insert sql code here..
SET unique_checks=1; SET foreign_key_checks=1;
```

