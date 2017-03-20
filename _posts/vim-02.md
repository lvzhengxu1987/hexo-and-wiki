---
title: vim-编辑器基本操作
date: 2016-06-20 02:16:15
tags: vim
categories: tools
keywords: vim
---

VIM基本操作

vim的3种模式,分别是命令模式,插入模式,底行命令模式,进入插入模式按i键,进入底行命令模式按:键(shift+;),3种模式间的切换均在以命令模式为基础的情况下来操作,如果不能确定当前模式是否为命令模式,可以多按几下Esc键.

###### 常用的移动操作(命令模式下)
```
光标移动  h 左, j 下, k 上, l 右.
单词移动  w 向右一个单词;b 向左一个单词;e 移至单词尾部.
行头和行尾的移动  移动至行头,按数字0或^;移动至行尾,按$.
当前屏幕位置移动 H  移动至当前屏幕首行首字符.M  移动至当前屏幕中间那一行首字符.L  移动至当前屏幕最下面那一行首字符.
屏幕移动 ctrl(键)+f  向下一屏;ctrl+b  向上一屏;ctrl+d  向下半屏;ctrl+u  向上半屏;ctrl+e 向下一行;ctrl+y  向上一行;
整个文件行首与行尾移动,G(shift+g)   移动至文件最尾端,gg(连按两次) 移动至文件开始的位置
```
<!--more-->

###### 常用的修改操作
```
i 在光标前插入
I 在当前行首插入
a 在光标后插入
A 在当前行尾插入
o 在当前行之下一新行插入
O 在当前行之上新开一行插入
R 替换(覆盖)当前光标位置及后面的若干文本
J 合并光标所在行及下一行为一行
```

###### 常用的copy动作
```
yy copy一行，粘贴内容放入缓冲区中，按p键粘贴
yw copy一个单词，按p键粘贴
vim在命令行模式下按住v键进入visual模式，上下左右移动进行copy，按p键粘贴
```

###### 常用的删除操作
```
dd 删除一行，删除内容放入缓冲区中，可以按p键粘贴
dw 删除一个单词，按p键粘贴
vim在命令行模式下按住v键进入visual模式，上下左右移动进行del，按p键粘贴
```

###### 常用的恢复操作
```
u  回退当前操作,未被保存
U  回退当前行所有新的操作，不管是否保存
```

###### 常用的redo操作
```
CTRL-R 如果发现有些操作被误回退，可以使用CTRL 和 R键进行redo。
```

###### vim中的number操作
```
vim中的命令，比如copy，delete，undo和redo，都可以在命令前面加上一个具体的number，或者输入一个范围，来指定操作的信息。
```

###### 常用的搜索和替换操作
```
在命令模式下,可以使用#和*锁定当前单词,向上或向下搜索该单词, 并把光标移至对应位置
在命令模式下 /string 在当前位置向下寻找string,并将光标移至对应位置
         ?string 在当前位置向上寻找string,并将光标移至对应位置
         n 继续执行/string或?string
         N 反向继续执行/string或?string
在命令模式下,进入底行命令模式
s/string1/string2/        将当前行第一个string1替换成string2
s/string1/string2/g       将当前行所有的string1换成string2
1,$ s/string1/string2/    将文件中所有行的第一个string1换成string2
% s/string1/string2/      将文件中所有行的第一个string1换成string2
1,$ s/string1/string2/g   将文件中所有行中所有的string1换成string2
% s/string1/string2/g     将文件中所有行中所有的string1换成string2
```

##### <a name="id0002" style="color: black">vim 使用正则表达式</a>
正则表达式可查看正则这篇文章[regular](/2016/06/20/regular-01/)

  
另外，如果要查找字符 *、.、/等，则需要在前面用 \ 符号，表示这不是元字符，而只是普通字符而已。

接下来就是vim一些常用的正则操作
* 查找所有以char开头，之后是一个以上的空白,  最后是一个标识符和分号
  ``/char\s\\+[A-Za-z\_]\\w\*;``
* 查找如 17:37:01 格式的时间字符串
  ``/\d\d:\d\d:\d\d``
* 删除只有空白的行
  ``:g/^\s*$/d``            
* 将所有的four替换成4，但是fourteen中的four不替换
  ``:s/\<four\\>/4/g``
* 处理文本中的^M,对于换行,window下用回车换行（0A0D）来表示,linux下是回车（0A）来表示。这样,将window上的文件拷到unix上用时,总会有个^M,要替换掉这种字符,%s/^M$//g,其中,^M是由Ctrl+V和Ctrl+M组合输入的,也可以sed -e "s/^M//" file > outfile
*在每一行的头部添加"mv"
 ``:%s/^/mv/g``
* %s/f $/for$/g 将每一行尾部的"f "（f键和空格键）替换为for
* g/^s*$/d 删除所有空行
* %s/ *$//g "去掉行尾空格

正则表达式替换用的变量

在正规表达式中使用 \( 和 \) 符号括起正规表达式，即可在后面使用\1、\2等变量来访问 \( 和 \) 中的内容。

* "查找开头和结尾处a的个数相同的字符串， " 如 aabbbaa，aaacccaaa，但是不匹配 abbbaa
  ``/\(a\+\)[^a]\+\1``
* "将URL替换为&lt;a href="http://url"&gt; http://url </a>的格式
   自己酌情加符号来进行正则匹配  
   ``
   s/\\(http:\/\/[a-zA-Z0-9\.\_\/]\\+\\)/\&lt;a href="\1"\&gt;\1\&lt;\/a\&gt;/ 
   s/\\(http:\/\/[a-zA-Z0-9\.\_=\\\?\/]\\+\\)/\&lt;a href="\1"\&gt;\1\&lt;\/a\&gt;/
   s/\(http:\/\/www.126.com\)/\<a href="\1"\>\1\<\/a\>/
   ``

* "将 data1 data2 (中间有若干空格)修改为 data2 data1
  ``:s/\\(\w\\+\\)\s\\+\\(\w\\+\\)/\2\t\1``                

函数式
在替换命令 s 中可以使用函数表达式来书写替换内容，格式为 :s/替换字符串/\=函数式
在函数式中可以使用 submatch(1)、submatch(2) 等来引用 \1、\2 等的内容，而submatch(0)可以引用匹配的整个内容
:%s/\<id\>/\=line(".")                              
" 将各行的 id 字符串替换为行号
:%s/^\<\w\+\>/\=(line(".")-10) .".". submatch(1)    
" 将每行开头的单词替换为 (行号-10).单词 的格式, " 如第11行的 word 替换成 1. word

VIM编码

Vim 有四个跟字符编码方式有关的选项，encoding、fileencoding、fileencodings、termencoding (这些选项可能的取值请参考 Vim 在线帮助 :help encoding-names)，它们的意义如下:
* encoding: Vim 内部使用的字符编码方式，包括 Vim 的 buffer (缓冲区)、菜单文本、消息文本等。默认是根据你的locale选择.用户手册上建议只在 .vimrc 中改变它的值，事实上似乎也只有在.vimrc 中改变它的值才有意义。你可以用另外一种编码来编辑和保存文件，如你的vim的encoding为utf-8,所编辑的文件采用cp936编码,vim会自动将读入的文件转成utf-8(vim的能读懂的方式），而当你写入文件时,又会自动转回成cp936（文件的保存编码).

* fileencoding: Vim 中当前编辑的文件的字符编码方式，Vim 保存文件时也会将文件保存为这种字符编码方式 (不管是否新文件都如此)。

* fileencodings: Vim自动探测fileencoding的顺序列表，启动时会按照它所列出的字符编码方式逐一探测即将打开的文件的字符编码方式，并且将 fileencoding 设置为最终探测到的字符编码方式。因此最好将Unicode 编码方式放到这个列表的最前面，将拉丁语系编码方式 latin1 放到最后面。

* termencoding: Vim 所工作的终端 (或者 Windows 的 Console 窗口) 的字符编码方式。如果vim所在的term与vim编码相同，则无需设置。如其不然，你可以用vim的termencoding选项将自动转换成term 的编码.这个选项在 Windows 下对我们常用的 GUI 模式的 gVim 无效，而对 Console 模式的Vim 而言就是 Windows 控制台的代码页，并且通常我们不需要改变它。

一般情况下
encoding设置为 utf-8
fileencodings 设置为ucs-bom,utf-8,cp936,gb18030,big5,euc-jp,euc-kr,latin1
fileencoding根据fileencodings检测出来的编码来确定





