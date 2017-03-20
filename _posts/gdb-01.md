---
title: gdb调试-基本用法
date: 2016-06-20 07:48:30
tags: gdb
categories: 调试 
keywords: gdb
---

## GDB调试-调试入门
使用GDB调试C，或者C++代码时，首先在代码编译时，一定要加入``-g``选项.
使用GDB调试程序的步骤：
一、``gdb  run_exe``
二、设置执行程序参数 ``set args xxx xxx xxx``
三、设置breakpoint，使用命令``b filename:line``或者``b line``
四、执行``run``命令，直到程序运行到断点处。
五、使用print、where、info等命令查看断点处的信息
六、使用q退出gdb.

使用GDB的关键是breakpoint等等。

##### 关于gdb打断点时,有的函数名重复冲突时的解决办法

