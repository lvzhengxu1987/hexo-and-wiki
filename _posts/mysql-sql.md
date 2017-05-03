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

```
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

```
CREATE TABLE tbl\_new AS SELECT * FROM tbl\_old;
```

