---
title: cpp
date: 2016-06-19 05:42:29
tags: cpp
categories: 编程语言
keywords: cpp
---

<a href="#id0001">template</a>
<a href="#id0002">reference 引用</a> 
<a href="#id0003">overload</a>
<a href="#id0004">C++的类型转换</a>
<a href="#id0005">friend</a>
<a href="#id0006">c++的class OBJECT{}里的内容(函数定义)</a>


C++在兼容c的同时，增加了面向对象、继承、多态，函数重载和操作符重载等等很多新特性。
C++中的const,static在外面有一个统一的结论文件,可参考.C++作为一个面向对象编程语言,既兼容了c语言特点,又含有面向对象的特点,同时因为兼容c语法,使得它的代码不是那么容易看懂,甚至多了许多晦涩难明的语法.

### <a name="id0001" style="color: black">0001.template</a>
在使用template时,有个问题需要注意下,一个template function的定义和实现最好写在一起,否则在编译时,就找不到他的实现,具体的原因如下英文描述001.A template is not a class or a function. A template is a "pattern" that the compiler uses to generate a family of classes or functions.  002.In order for the compiler to generate the code, it must see both the template definition (not just declaration) and the specific types/whatever used to "fill in" the template. For example, if you're trying to use a Foo, the compiler must see both the Foo template and the fact that you're trying to make a specific Foo.  003.Your compiler probably doesn't remember the details of one .cpp file while it is compiling another .cpp file. It could, but most do not and if you are reading this FAQ, it almost definitely does not. BTW this is called the "separate compilation model."

<!--more-->
  解决办法
  http://stackoverflow.com/questions/3008541/template-class-symbols-not-found 一个最直接的解决办法 Move the implementation inside the header.

### <a name="id0002" style="color: black">引用 reference</a>
  引用在处理变量时,定义时须同时初始化.引用在初始化时不能赋值NULL,换句话说,引用 是某个变量的别名,和指针不同的是,引用必须初始化.

### <a name="id0003" style="color: black">overload</a>
  Restrictions
  0001.The operators :: (scope resolution), . (member access), .* (member access 
  through pointer to member), and ?: (ternary conditional) cannot be overloaded
  0002.New operators such as **, <>, or &| cannot be created
  0003.The overloads of operators &&, ||, and , (comma) lose their special 
  properties: short-circuit evaluation and sequencing. 

### <a name="id0004" style="color: black">C++的类型转换</a>
### <a name="id0005" style="color: black">friend</a>
  friend注意 You don't put the friend keyword when defining the function, only when declaring it.  

### <a name="id0006" style="color: black">c++的class OBJECT{}里的内容(函数定义)</a>
  在测试代码时,发现一个问题,如果操作符重载

## CPP资源
[CPP 介绍及API](http://www.cplusplus.com/info/)
[The GNU C++ Library](https://gcc.gnu.org/onlinedocs/libstdc++/)
[C Programming and C++ Programming](http://www.cprogramming.com/)
[The Definitive C++ Book Guide and List](http://stackoverflow.com/questions/388242/the-definitive-c-book-guide-and-list)
[GCC online documentation](https://gcc.gnu.org/onlinedocs/)
[C++参考手册](http://zh.cppreference.com/w/%E9%A6%96%E9%A1%B5)

[boost 官网](http://www.boost.org/)
[boost info](http://stackoverflow.com/tags/boost/info)
[Open Standards](http://www.open-std.org/)

[C++学习的方法以及四大名著（荐）](http://www.cnblogs.com/pugang/archive/2012/08/17/2643710.html)
[Linux C++ 调试神技--如何将Linux C++ 可执行文件逆向工程到Intel格式汇编](http://www.cnblogs.com/pugang/p/4035314.html)
