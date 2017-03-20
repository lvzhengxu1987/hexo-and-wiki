---
title: sed
date: 2016-04-17 18:53:31
tags: [sed,linux]
categories: linux_commands 
---

sed [-nefri] [动作]
<pre>简单的选项与参数：
-n ：使用silent模式。一般情况下，所有来自STDIN的数据都会被列出到终端上
     但加上-n参数后,则只有经过sed特殊处理的那一行(或者动作)才会被列出来
-e ：直接在命令列模式上进行 sed 的动作编辑；
-f ：直接将 sed 的动作写在一个文件内， -f filename 则可以运行 filename 内的 sed 动作；
-r ：sed 的动作支持的是延伸型正规表示法的语法。(默认是基础正规表示法语法)
-i ：直接修改读取的文件内容，而不是输出到终端</pre>

<pre>动作说明： [n1[,n2]] function
n1, n2 ：不见得会存在，一般代表『选择进行动作的行数』，举例来说，如果我的动作是需要在10到 20行之间进行的，则『 10,20[动作行为] 』

function：
a ：追加， a 的后面可以接字串，而这些字串会在新的一行出现(当前匹配的下一行)
c ：更改， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！
d ：删除， 因为是删除啊，所以 d 后面通常不接任何东西；
i ：插入， i 的后面可以接字串，而这些字串会在新的一行出现(当前匹配的上一行)；
p ：打印， 亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行；
s ：替换， 可以直接进行替换的工作！通常这个 s 的动作可以搭配正规表示法！例如 1,20s/old/new/g </pre>
<!--more-->

## sed是行模式的字节流处理工具,其核心处理机制依赖正则表达式

下面的是sed中使用的正则表达符号,可以看出和正则表达式中的内容是吻合的.
``^`` 表示一行的开头。如：```/^#/``` 以#开头的匹配。
``$`` 表示一行的结尾。如：```/}$/```  以}结尾的匹配。
``\<`` 表示词首。 如 ```\<abc``` 表示以 abc 为首的詞
``\>`` 表示词尾。 如 ```abc\>```  表示以 abc 結尾的詞。
``.`` 表示任何单个字符。
``*`` 表示某个字符出现了0次或多次。
``[ ]`` 字符集合。 如：[abc]表示匹配a或b或c，还有[a-zA-Z]表示匹配所有的26个字符。如果其中有^表示反，如[^a]表示非a的字符

## sed 命令的一些形式

<b>替换</b> [address] s<font size="3" color="red">/</font>pattern<font  size="3" color="red">/</font>replacement<font size="3" color="red">/</font>flags
flags一般是 n，g，p，w file 这几种.其中n是一个数字，就是进行匹配后的第n个进行处理；g就是全部；p是打印的意思；w file就是把结果写到file中。
<b>删除</b> [address]d
<b>追加</b> [line-address]a text 或  [line-address]r file
&nbsp;&nbsp;&nbsp;&nbsp;追加只能作用于单行内容，在匹配的单行之后增加text的内容；a后面可以跟制表符（需要转义斜杠如：tab键要输入\\t而不是\t），这样制表符也会追加到text之前，而text中的制表符不需要转义；r命令后面是跟一个文件名file，这样会把file中的内容追加到下一行中，想追加多行内容时可以使用此方法。
<b>插入</b> [line-address]i text
&nbsp;&nbsp;&nbsp;&nbsp;插入只能作用于单行内容，在匹配的单行之前插入text的内容，i命令后面可以跟制表符（需要转义斜杠如：tab键要输入\\\\t而不是\\t），这样制表符也会追加到text之前，而 text中的制表符不需要转义
<b>更改</b> [address]c text
&nbsp;&nbsp;&nbsp;&nbsp;更改命令可以作用于单行，也可以作用于一个范围的行；c命令后面可以跟制表符（需要转义斜杠如：tab键要输入\\\\t而不是\t），这样制表符也会追加到text之前，而text中的制表符不需要转义。

<font color="red">以下截图均是没有加 -i 参数，也就是说把处理后的结果打印在屏幕上，没有直接修改文件</font>
sed 替换命令效果
![img](/site_files/sed-io-replace.jpg)
<img src="/site_files/sed-io-replace.jpg" />
sed 删除命令效果
<img src="/site_files/sed-io-delete.jpg" />
sed 追加命令效果
<img src="/site_files/sed-io-append.jpg" />
sed 插入命令效果
<img src="/site_files/sed-io-insert.jpg" />
sed 更改命令效果
<img src="/site_files/sed-io-change.jpg" />

**下面是一些例子，多做试验理解**
在每一行最前面加点东西，就是加个#号
``sed 's/^/#/g' pets.txt``
&nbsp;&nbsp;&nbsp;&nbsp;如果加了-i 选项，就不会将结果刷在屏幕上，而就直接会去修改文件本身！例如:sed 's/^/#/g' -i pets.txt
在每一行最后面加点东西
``sed 's/$/ --- /g' pets.txt``

删除文件中的空行
``sed '/^$/d' filename``

正规则表达式是一些很牛的事，比如我们要去掉某html中的tags
&lt;b&gt;This&lt;/b&gt; is what &lt;span style="text-decoration: underline;"&gt;I&lt;/span&gt; meant. Understand?
如果你这样搞的话，就会有问题
``$ sed 's/<.\*>//g' html.txt ``
结果是：Understand?
要解决上面的那个问题，就得像下面这样。
其中的'[^>]' 指定了除了>的字符重复0次或多次。
``$ sed 's/<[^>]*>//g' html.txt``
结果是：This is what I meant. Understand?

指定替换某行内容,代码中的3可以替换成任意一行
``sed "3s/my/your/g" pets.txt``

指定替换某一段内容
``sed "3,6s/my/your/g" pets.txt``

只替换每一行的第一个s
``sed 's/s/S/1' my.txt``

只替换每一行的第二个s
``sed 's/s/S/2' my.txt``

只替换每一行第三个和以后的s
``sed 's/s/S/3g' my.txt``

如果我们需要一次替换多个模式，可参看下面的示例：（第一个模式把第一行到第三行的my替换成your，第二个则把第3行以后的This替换成了That）
``sed '1,3s/my/your/g; 3,$s/This/That/g' my.txt``
上面的命令等价于：（注：下面使用的是sed的-e命令行参数）
``sed -e '1,3s/my/your/g' -e '3,$s/This/That/g' my.txt``

sed &操作符
我们可以使用&来当做被匹配的变量，然后可以在其左右加一些东西。如下所示：
``sed 's/my/[&]/g' my.txt``

圆括号匹配
使用圆括号匹配的示例：（圆括号括起来的正则表达式所匹配的字符串会可以当成变量来使用，sed中使用的是\1,\2…）
``$ sed 's/This is my \([^,]*\),.*is \(.\*\)/\1:\2/g' my.txt``
cat:betty
dog:frank
fish:george
goat:adam
上面这个例子中的正则表达式有点复杂，解开如下（去掉转义字符）：
正则为：This is my ([^,]*),.*is (.*)
匹配为：This is my (cat),……….is (betty)

##### sed如何引用环境变量
在sed使用过程中，如果要用到环境变量的话，可以这样：``sed 's/aaa/'"$LC_TIME"'/' filename``,在环境变量处，先用``'``结束之前命令，然后再用``""``包含环境变量名称,然后再用'接着后续sed命令
[参考链接 How do I read environment variables with sed](http://www.linuxtopia.org/online_books/linux_tool_guides/the_sed_faq/sedfaq4_018.html)
##### sed中关于替换变量与被替换变量及分隔符之间的一个处理
当sed要替换的字符串或者被替换的字符串中有``/``这种字符的时候，比如``sed 's/abc/"b/c"/' filename``,注意这个"b/c",这个命令就不好用了，会报错 ``sed: -e expression #1, char 10: unknown option to `s'``,此时使用``sed 's@abc@"b/c"@' filename``，其实就是将``/``换成``@``，然后依旧可以使用.替换符可以是``@#%!|/``，在替换或者被替换的一个字符串的字符如果和sed命令中的分隔符一样的话，那么 sed 命令就会失败，这个情况需要特别注意.
[参考链接 sed-scripting-environment-variable-substitutio](http://stackoverflow.com/questions/584894/sed-scripting-environment-variable-substitution)
##### 如何判断sed命令中的分隔符与替换或被替换变量中的字符是否冲突
1. 如果熟悉替换或者被替换的字符串的内容，那么在写命令时，只要不使用特定的字符作为分隔符，这个问题就可以解决。
2. 如果不清楚要操作的字符串内容的话，那只能先扫描一遍字符串内容，然后根据具体情况再选用特定分隔符了。效率方面自然就很一般了。

sed中Hold Space
g: 将hold space中的内容拷贝到pattern space中,原来pattern space里的内容清除
G: 将hold space中的内容append到pattern space\n后
h: 将pattern space中的内容拷贝到hold space中,原来的hold space里的内容被清除
H: 将pattern space中的内容append到hold space\n后
x: 交换pattern space和hold space的内容

#### 参考网址
[sed:概念介绍](http://blog.csdn.net/itsenlin/article/details/20784191)
[sed:常用命令](http://blog.csdn.net/itsenlin/article/details/20814051)
[sed:模式空间](http://blog.csdn.net/itsenlin/article/details/21129405)
