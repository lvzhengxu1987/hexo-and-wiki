---
title: regular-正则基础
date: 2016-06-20 03:27:02
tags: regular
categories: 正则
keywords: 正则
---

通常，我想在一个目录中找一个文件，例如：在当前目录下有一个a.txt这个文件，如果在linux 环境中，我想找这个文件，我可以使用命令``ls -lrt | grep a.txt``,也可以 ``ls -l a.txt``等等，如果当前目录下有a1.txt,a2.txt....aN.txt等n个以字母a开头的txt文件，此时我如果想一次都给找出来，这时，我该怎么做呢，同样在linux环境下，我可以``ls -lrt | grep a*.txt``,或者 ``ls -l a*.txt``,通过这种方式，就可以将以字母a开头的txt找到了。在上面的两个命令中，*这个字符其实就是正则表达式的一个特殊字符，表示0-无限个任意字符。用这种表达式就可以将这些文件都代替了。
<!--more-->

接下来就是正则表达式里面常用的一些信息。

### 元字符
元字符是具有特殊意义的字符。使用元字符可以表达任意字符、行首、行尾、某几个字符等意义

元字符一览
{% img /site_files/20160704-1458.png %}
{% img /site_files/regular-expressions-cheat-sheet-v2.png %}

图片来源于[百度百科-正则](http://baike.baidu.com/link?url=DeFJcFEo6R0gYlPNcFmor5_S336Q8JqGxK7bWGmPv0O8i5QwJHNlBf6ub9iu5EemGR6pY2-0nyxs1MBRM7U-Ba)，[addedbytes.com](http://www.addedbytes.com/cheat-sheets/download/)
备注:用ctrl+v然后再按Tab键,可以键入tab字符,aix下的sed用\t不好用

******************************************************************************
正则表达式分组
我们已经提到了怎么重复单个字符(直接在字符后面加上限定符就行了);但如果想要重复
多个字符又该怎么办?你可以用小括号来指定子表达式(也叫做分组)，然后你就可以指定
这个子表达式的重复次数了.
(\d{1,3}\.){3}\d{1,3}是一个简单的IP地址匹配表达式。要理解这个表达式，请按下列顺序
分析它：\d{1,3}匹配1到3位的数字，(\d{1,3}\.){3}匹配三位数字加上一个英文句号(这个
整体也就是这个分组)重复3次，最后再加上一个一到三位的数字(\d{1,3})。不幸的是，它也
将匹配256.300.888.999这种不可能存在的IP地址。如果能使用算术比较的话，或许能简单地
解决这个问题，但是正则表达式中并不提供关于数学的任何功能，所以只能使用冗长的分组
字符类来描述一个正确的IP地址：
    ((2[0-4]\d|25[0-5]|[01]?\d\d?)\.){3}(2[0-4]\d|25[0-5]|[01]?\d\d?)

******************************************************************************
关于正则表达式可以看看这个图片
{% img /site_files/20160704-1453.png %}

参考地址:
[learn-regex](https://github.com/dwyl/learn-regex/blob/master/README.md)
[regular-expressions-cheat-sheet](http://www.addedbytes.com/cheat-sheets/download/regular-expressions-cheat-sheet-v2.png)
[regex](http://www.computerhope.com/jargon/r/regex.htm)
[一个正则表达式的解析](http://www.computerhope.com/jargon/r/regex.htm)
[wiki regular](https://en.wikipedia.org/wiki/Regular_expression)
