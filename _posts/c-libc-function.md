---
title: c-libc中常用函数
date: 2016-07-17 15:42:51
tags: 
categories: libc
keywords: 
---

##### gettimeofday, settimeofday 
gettimeofday, settimeofday - get / set time 获取系统的时间，以秒和微秒为单位。可以用来计算程序执行时间.
#####  getitimer, setitimer
getitimer, setitimer - get or set value of an interval timer 设置一个内部时间定时器，当定时器时间到了之后，会发送一个信号给process，然后这个定时器会重置并启动。
