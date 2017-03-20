---
title: make cmake scons
date: 2016-06-18 21:32:17
tags: [make,cmake,scons,linux]
categories: tools
keywords: make cmake scons
---

## Make
make命令通过读取Makefile这个文件，或者通过 -f参数指定的文件。来实现编译的。

| make变量名称  | 含义          |
| ------------- |:-------------:|
| $@            | 目标的文件名 |
| $*            | 目标的主文件名,不包含扩展名|
| $<            | 第一个条件的文件名|
| $^            | 所有条件的文件名,并以空格分开,且排除了所有重复的条件|
| $+            | 与$^类似,没有排除重复的条件|
| $?            | 时间戳在目标之后的条件,并以空格分开|
<!--more-->

---

| make赋值      | 含义          |
|:------------- |:-------------:|
|=|基本的赋值|
|:=|覆盖之前的值|
|?=|如果没有被赋值过就赋予等号后面的值|
|+=|添加等号后面的值|

---

|make 常用变量| 含义  |
|:------------- |:-------------|
|CC|C compiler command|
|CFLAGS|C compiler flags|
|LDFLAGS|linker flags, e.g. -L&lt;lib dir&gt; if you have libraries in a nonstandard directory &lt;lib dir&gt;|
|LIBS|libraries to pass to the linker, e.g. -l&lt;library&gt;|
|CPPFLAGS|(Objective) C/C++ preprocessor flags, e.g. -I&lt;include dir&gt; if you have headers in a nonstandard directory &lt;include dir&gt;|
|CXX|C++ compiler command|
|CXXFLAGS|C++ compiler flags|
|CPP|C preprocessor|
|CXXCPP|C++ preprocessor|
|F77|Fortran 77 compiler command|
|FFLAGS|Fortran 77 compiler flags|

---


* make中创建的变量,在引用的时候要用$(),${}
* make一般情况下使用的创建规则目标、条件和命令,在命令前面一定要有一个tab键,这个是make规定死的,无法改变,条件可能是若干个,命令可以是若干个
* 可以用命令make -p 查看隐式规则
* Makefile中在命令前有时有@、-、+,@符号的作用是不再输出当前命令的数据内容到屏幕上,-符号是忽略当前命令的错误并继续执行,+符号是只显示命令，但并不执行,在makefile中可以使用\来断行

make 一个模版
``` makefile
CC=gcc
CPP=g++

CFLAGS=-g -Wall -O0 -pg
CXXFLAGS= -O2 -pg

C_DEBUGFLAGS=-g -Wall -O0 -pg -DDEBUG
CXX_DEBUGFLAGS=-g -Wall -O0 -pg -DDEBUG

LIBS=-lpthread -luuid -lrt

all: client debug_client tags
#default:client debug_client tags

tags:
    ctags -R *

client:client_radius_thread.o do_work.o md5.o RadiusClient.o
    $(CPP) $(CXXFLAGS) -o $@ $^  $(LIBS)

debug_client:client_radius_thread.cppdg do_work.cppdg md5.cppdg RadiusClient.cppdg
    $(CPP) $(CXX_DEBUGFLAGS) -o $@ $^  $(LIBS)

%.cppdg:%.cpp
    $(CPP) $(CXX_DEBUGFLAGS) -c $< -o $@

%.cdg:%.c
    $(CC) $(C_DEBUGFLAGS) -c $< -o $@

clean:
    -rm -f *.o *.cppdg *.cdg tags cscope.out
    -rm client debug_client

#.PHONY : all
```

## cmake

## scons
[scons 入门使用](http://www.ibm.com/developerworks/cn/linux/l-cn-scons/)
一个简短的hello world scon示例
```
Program('do_client', ['client_radius_thread.c', 'do_work.cpp', 'md5.cpp','RadiusClient.cpp'],
		LIBS = ['pthread', 'uuid'],
		LIBPATH = ['/usr/lib', '/usr/local/lib'],
		CCFLAGS = '-DHELLOSCONS')
```


参考网址
[GNU make Document](https://www.gnu.org/software/make/manual/html_node/index.html#toc-Integrating-GNU-make)

[cmake step by step](https://tuannguyen68.gitbooks.io/learning-cmake-a-beginner-s-guide/content/chap1/chap1.html)
[cmake tutorial](https://cmake.org/cmake-tutorial/)
[cmake 学习笔记(一)](http://blog.csdn.net/dbzhang800/article/details/6314073)
[cmake 学习笔记](http://blog.csdn.net/dbzhang800/article/category/894128)
[在 linux 下使用 CMake 构建应用程序](http://www.ibm.com/developerworks/cn/linux/l-cn-cmake/)
