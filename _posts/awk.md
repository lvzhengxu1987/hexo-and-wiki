---
title: awk
date: 2016-06-18 17:52:12
tags: [awk,linux]
categories: linux_commands 
---

## 基础
BEGIN和END这两个关键字意味着执行前和执行后的意思

* BEGIN{ 这里面放的是执行前的语句 }
* END {这里面放的是处理完所有的行后要执行的语句 }
* {这里面放的是处理每一行时要执行的语句}
<!--more-->

## awk的内建变量
说到了内建变量,我们可以来看看awk的一些内建变量：

| 变量名称      | 含义           |
| ------------- |:-------------:| 
| $0            | 当前记录(这个变量中存放着整个行的内容) | 
| $1~$n         | 当前记录的第n个字段,字段间由FS分隔| 
| FS            | 输入字段分隔符 默认是空格或Tab| 
| NF            | 当前记录中的字段个数,就是有多少列,一行一行读取时,可以作为一行的字数| 
| NR            | 已经读出的记录数,就是行号,从1开始,如果有多个文件话,这个值就不断地累加|
| FNR           | 当前记录数,与NR不同的是,这个值是各个文件自己的行号|
| RS            | 输入的记录分隔符,默认为换行符|
| OFS           | 输出字段分隔符,默认是空格|
| ORS           | 输出的记录分隔符,默认为换行符|
| FILENAME      | 当前输入文件的名字|


---
### awk 一些常用命令

* 有表头还有条件再加上格式化输出 
``netstat -antup | awk '$3==0 && $6=="LISTEN" || NR==1 {printf "%-20s %-20s %s\n",$4,$5,$6}' ``

* 指定分隔符,下面2条命令等价
``awk 'BEGIN{FS=":"} {print $1,$3,$6}' /etc/passwd``
``awk -F: '{print $1,$3,$6}' /etc/passwd``

* 以\t作为分隔符输出,etc/passwd这个文件之前是以:为分隔符
``awk -F: '{print $1,$3,$6}' OFS="\t" /etc/passwd``

* awk字符串匹配
第一个示例匹配FIN状态， 第二个示例匹配WAIT字样的状态。
``~`` 表示模式开始。``/xxx/``中是模式。这就是一个正则表达式的匹配。
首先是``netstat -antup > file``
``awk '$6 ~ /FIN/ || NR==1 {print NR,$4,$5,$6}' OFS="\t" filename``
``awk '$6 ~ /WAIT/ || NR==1 {print NR,$4,$5,$6}' OFS="\t" filename``
也可以使用 /FIN|WAIT/
``awk '$6 ~ /FIN|WAIT/ || NR==1 {print NR,$4,$5,$6}' OFS="\t" filename``
模式取反
``awk '$6 !~ /FIN|WAIT/ || NR==1 {print NR,$4,$5,$6}' OFS="\t" filename``

* awk也可以像grep搜索字符串
``awk '/LISTEN/' filename``,``awk '!/LISTEN/' filename``

* awk拆分文件,使用重定向
``awk 'NR!=1{print > $6}' filename``
* 复杂的文件拆分
``awk 'NR!=1{if($6 ~ /TIME|ESTABLISHED/) print > "1.txt";
else if($6 ~ /LISTEN/) print > "2.txt";
else print > "3.txt" }' filename``

* awk使用环境变量
使用-v参数和ENVIRON，使用ENVIRON的环境变量需要export
<pre>
$ x=5
$ y=10
$ export x
$ export y
$ echo $x $y
5 10
$ awk -v val=$x '{print $1, $2, $3, $4+val, $5+ENVIRON["y"]}' OFS="\t" file 
</pre>




