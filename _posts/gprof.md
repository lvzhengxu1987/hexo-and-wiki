---
title: gprof
date: 2016-07-17 05:20:37
tags: [gprof]
categories: profiler
keywords: gprof
---

#### gprof
首先编译时，在CFLAGS加入 -pg，编译出程序后，在程序运行一段时间后，就会生成 gmon.out这个文件，当程序运行完成后，就可以使用命令``gprof a.out gmon.out ``就可以将程序运行的分析结果打印在屏幕上面了。
