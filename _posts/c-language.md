---
title: c-language
date: 2016-08-21 20:46:44
tags: c
categories: program-language
keywords: c
---

### 变量类型 变量操作 变量表达式 程序控制 函数与程序结构 
变量类型包括 char,short,int,long,double float 和unsigned char, unsigned short,unsigned int,unsigned long.
取值范围
<!-- more -->
#### 变量类型

数字变量类型

|变量          |64位占用字节  |取值范围|
|--------------|:------------:|:------:|
|char          |1             |-128~127|
|short         |2             |-32768~32767|
|int           |4             |-2147483648~2147483647|
|long          |8             |-9223372036854775808~9223372036854775807|
|long long     |8             |-9223372036854775808~9223372036854775807|

不是所有的系统都会有long long类型
如果是unsigned type，那么取值类型都是正值且是[正值最大值]*2+1;

#### 变量操作符

#### 变量声明周期

|变量动作      |      含义          |
|--------------|:------------------:|
|变量生命周期|变量声明->变量定义->变量初始化->变量赋值->变量销毁|
|变量声明|    声明并不会为一个变量申请存储空间，如extern int a;|
|变量定义|    变量定义则是一定会为变量申请空间，如 int a;|
|变量初始化|  变量在定义时并为变量进行赋值，如 int a = 10;|
|变量赋值|    这是最常用的了,int a; a = 10;|
|变量销毁|    如果是全局变量，这是在程序退出时，变量销毁，如果是局部变量，那么当那个变量所处的代码块完成后，这个变量便不在有任何意义了。|
### 指针与数组 C struct 

### c语言调用动态库的两种方法
[dlopen(3) - Linux man page,有个示例](http://www.gnu.org/software/libc/manual/html_mono/libc.html)
[dlopen opengroup 标准](http://pubs.opengroup.org/onlinepubs/009695399/functions/dlopen.html)

在c语言中，调用动态库的方式有两种，一种是用-l+库名称、使用头文件预定义函数名称等,然后在代码中使用动态库中的函数;另一种方式是使用dlopen方式，使用函数指针调用动态库中的函数。两种方式都能调用动态库，两种方式的不同点在于使用 -l方式在编译时，就将动态库和可执行程序编译在了一起，在可执行程序加载到内存时，就将动态库放入内存中,使用ldd命令可以看到关联的动态库名称;而dlopen函数只有在程序使用到的时候，才会加载动态库,并且使用ldd命令时，是看不到代码中调用的动态库链接.

### C 资料
[glibc document](http://www.gnu.org/software/libc/documentation.html)
[The GNU C Library](http://www.gnu.org/software/libc/manual/html_mono/libc.html)
