---
title: protect-python-code
date: 2016-12-24 21:27:42
tags: [protect-code,python]
categories: protect-code
---
## python 保护你的代码

目录
[为什么做保护](#a0001)
[怎么去做保护](#a0002)
[保护的几种做法](#a0003)
[几种做法的对比](#a0004)
[采用的做法](#a0005)
[参考链接](#a0006)



<span id="a0001">一: 为什么做代码保护</span>
  毕竟是要把代码作为商业软件发行出去，那么必要的保护加密是必须的，防止被别人逆向出来(不被逆向出来是不可能的，只是增加一些难度而已)。
<!--more-->

<span id="a0002">二: 代码保护的方向</span>
  1. 将代码中的变量或者函数名称都换成随机字符，让代码难以辨认
  2. 使用python自带的工具将python代码改成二进制方式
  3. 将python部分核心的代码都改成c，c++等语言，让核心部分变成二进制方式

<span id="a0003">三：代码保护的方法</span>
  3.1. 修改变量与函数的名称，将其统统变成随机字符
  3.2. 使用python自己的编译功能，将python脚本编译成pyc文件。
  3.3. 使用cython将python核心部分功能重写，然后将其编译为.so或.dll文件，再由 import 指令导入。
  3.4. 如果是windows平台，可以使用py2exe将python脚本包装为exe可执行程序,也可以使用PyInstaller。
  3.5. 如果是linux或者mac平台，可以使用PyInstaller将脚本打包为可执行程序。
  3.6. 使用python原始的c/c++嵌套代码方式，重写所有功能,或者全部功能用c,c++等语言重写。
  
<span id="a0004">四:各种方法的优点与缺点</span>
  * 方法3.1将变量与函数的名称修改后，整个代码可读性变差，但是代码结构没有本质的变化，很容易被还原
  * 方法3.2中编译出来的pyc文件无法直接阅读，缺点是使用python的uncompyle6(python2.7)、
decompyle等就可以还原为原来的代码，一目了然。并且pyc文件会保存编译它时的python版本，运行时会检查python版本，如果版本不一致，pyc文件无法运行。
  * 方法3.3需要c编程，编译出的so或dll文件，逆向成本高，而且编写c代码容易出bug，调试比较麻烦。

<span id="a0005">五:采用的方法</span>
linux环境下推荐使用cython方式，可以将整个python代码改成c实现。在docker中，使用Supervisor，可以管理所有程序。 也可以使用pyinstaller，其他做法生成的文件都是很容易被还原的。


<span id="a0006">参考连接</span>
[Protect python code 本文从各个方面阐述保护的方式与原因](http://stackoverflow.com/questions/261638/how-do-i-protect-python-code)
[("Protecting") Python Source Code](https://wiki.python.org/moin/Asking%20for%20Help/How%20do%20you%20protect%20Python%20source%20code%3F)
[Python程序的混淆和加密](http://www.jianshu.com/p/25631aa535c3)
[python 代码在线混淆](http://pyob.oxyry.com/#offline)
[python linux、mac环境打包](http://www.pyinstaller.org/)
[python下编译py成pyc和pyo](http://www.cnblogs.com/dkblog/archive/2009/04/16/1980757.html)
[使用uncompyle2直接反编译python字节码文件pyo/pyc](http://www.cnblogs.com/rainduck/p/3524557.html)
[在线反编译pyc](http://tool.lu/pyc/)
[python 反编译2.6或更低的版本](https://sourceforge.net/projects/decompyle/)
[python 反编译2.7或3之后的版本](https://github.com/wibiti/uncompyle2)
[使用pyinstaller打包python程序](http://blog.float.tw/2013/10/use-pyinstaller-packages-python-programs.html)
[Cython 官网](http://cython.org/)
[cpython 三分钟入门](http://www.oschina.net/question/54100_39042)
[How can I protect my Python code but still make it available to run?](https://www.quora.com/How-can-I-protect-my-Python-code-but-still-make-it-available-to-run)
[Protecting a Python codebase](http://bits.citrusbyte.com/protecting-a-python-codebase/)


* 扩展阅读
[PyConChina ](http://cn.pycon.org/2015/)
[What do the python file extensions, .pyc .pyd .pyo stand for?](http://stackoverflow.com/questions/8822335/what-do-the-python-file-extensions-pyc-pyd-pyo-stand-for)
[Is it possible to install another version of Python to Virtualenv?](http://stackoverflow.com/questions/5506110/is-it-possible-to-install-another-version-of-python-to-virtualenv)
