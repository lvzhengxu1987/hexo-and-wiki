---
title: bash shell
date: 2016-06-19 01:57:27
tags: shell
categories: linux_commands
keywords: shell
---
目录
<a href="#id0001">1. shell中的变量</a>
<a href="#id0002">2. shell的基本控制语句</a>
<a href="#id0003">3. 二元比较</a>
<a href="#id0004">4. 文件类型的测试操作</a>
<a href="#id0005">5. 字符串操作</a>
<a href="#id0006">6. 参数替换和扩展</a>
<a href="#id0007">7. shell function</a>
<a href="#id0008">8. shell 中一个重定向的问题</a>
<a href="#id0009">9.常用的小脚本</a>
<a href="#id0010">10.脚本常见问题</a>
<!--more-->

summary:shell和c、c++、java等语言类似，有自己的变量、控制结构、各种比较运算符、数值运算操作、字符串运算操作，并且支持正则，可以自定义函数，也可以和其他已有命令搭配使用等等。

## <a name="id0001" style="color: black">1.shell中的变量</a>
##### <a href="#id0001-01">01. 内置系统变量</a>
##### <a href="#id0001-02">02. 环境变量</a>
##### <a href="#id0001-03">03. 用户自定义变量</a>
shell中变量的引用一般是$var和${var} 2种方式.变量类型：一般有环境变量，shell系统内置变量，用户自定义变量,环境变量是系统已经设置完成的变量,比如LANG,PWD之类的.
##### <a name="id0001-01" style="color: black">shell系统内置变量</a>

|变      量| 含  义   |
|----------|:--------:|
|$0      |脚本名称|
|$n      |传给脚本/函数的第n个参数|
|$$      |脚本的PID|
|$!      |上一个被执行的命令的PID(后台运行的进程)|
|$?      |上一个命令的退出状态(管道命令使用${PIPESTATUS})|
|$#      |传递给脚本/函数的参数个数|
|$@      |传递给脚本/函数的所有参数(识别每个参数,所有的位置参数(每个都作为独立的字符串)),必须被引用起来,否则默认为"$@"|
|$*      |传递给脚本/函数的所有参数(把所有参数当成一个字符串,所有的位置参数作为单个字符串 )|
|$_      |上一个命令的最后一个参数|
|$-      |传递到脚本中的标志(使用set)|


##### <a name="id0001-02" style="color: black">shell环境变量</a>
RANDOM变量,可以产生范围是0--32767的随机数字.
SCONDS变量,脚本已经运行的时间（秒）
$BASH           ==> 这个变量将指向Bash的二进制执行文件的位置.
$BASH_ENV      ==>这个环境变量将指向一个Bash启动文件,这个启动文件将在调用一个脚本时被读取.
$BASH_SUBSHELL ==>这个变量将提醒subshell的层次,这是一个在version3才被添加到Bash中的新特性.
$BASH_VERSINFO[n] ==>记录Bash安装信息的一个6元素的数组.与下边的$BASH_VERSION很像,但这个更加详细

```
# Bash version info:

for n in 0 1 2 3 4 5
do
  echo "BASH_VERSINFO[$n] = ${BASH_VERSINFO[$n]}"
done  

# BASH_VERSINFO[0] = 3                      # 主版本号
# BASH_VERSINFO[1] = 00                     # 次版本号
# BASH_VERSINFO[2] = 14                     # Patch 次数.
# BASH_VERSINFO[3] = 1                      # Build version.
# BASH_VERSINFO[4] = release                # Release status.
# BASH_VERSINFO[5] = i386-redhat-linux-gnu  # Architecture
```

$BASH_VERSION ==>安装在系统上的Bash的版本号.可以使用echo $BASH_VERSION
$DIRSTACK ==>在目录栈中最上边的值(将受到pushd和popd的影响).这个内建的变量与dirs命令是保持一致的,但是dirs命令将显示目录栈的整个内容.
$EDITOR ==>脚本调用的默认编辑器,一般是vi或者是emacs.
$EUID ==>"effective"用户ID号.当前用户被假定的任何id号.可能在su命令中使用.注意:$EUID并不一定与$UID相同.
$HOSTNAME ==>hostname命令将在一个init脚本中,在启动的时候分配一个系统名字.gethostname()函数将用来设置这个$HOSTNAME内部变量
$HOSTTYPE ==>主机类型,就像$MACHTYPE,识别系统的硬件. 例子：bash$ echo $HOSTTYPE
$IFS ==>内部域分隔符.  这个变量用来决定Bash在解释字符串时如何识别域,或者单词边界.  $IFS默认为空白(空格,tab,和新行),但可以修改,比如在分析逗号分隔的数据文件时.  注意:$*使用$IFS中的第一个字符,具体见Example 5-1.注意:$IFS并不像它处理其它字符一样处理空白.

```  
bash$ echo $IFS | cat -vte
$
bash$ bash -c 'set w x y z; IFS=":-;"; echo "$*"'
w:x:y:o
```

$IGNOREEOF ==>忽略EOF: 告诉shell在log out之前要忽略多少文件结束符(control-D).
$LC_COLLATE ==>常在.bashrc或/etc/profile中设置,这个变量用来在文件名扩展和模式匹配校对顺序.如果此变量被错误的设置,那么将会在filename globbing中引起错误的结果.<b>注意</b>:在2.05以后的Bash版本中,filename globbing将不在对[]中的字符区分大小写.比如:ls [A-M]\* 将即匹配File1.txt也会匹配file1.txt.为了恢复[]的习惯用法,设置$LC\_COLLATE的值为c,使用export LC\_COLLATE=c 在/etc/profile或者是~/.bashrc中.
$LC_CTYPE ==>这个内部变量用来控制globbing和模式匹配的字符串解释.
$LINENO ==>这个变量记录它所在的shell脚本中它所在行的行号.这个变量一般用于调试目的.

```  
# *** BEGIN DEBUG BLOCK ***
last_cmd_arg=$_  # Save it.  
echo "At line number $LINENO, variable \"v1\" = $v1"
echo "Last command argument processed = $last_cmd_arg"
# *** END DEBUG BLOCK ***
```

$MACHTYPE 系统类型,提示系统硬件:$ echo $MACHTYPE
$OLDPWD 老的工作目录("OLD-print-working-directory",你所在的之前的目录)
$OSTYPE 操作系统类型.$ echo $OSTYPE
$PATH 指向Bash外部命令所在的位置,一般为/usr/bin,/usr/X11R6/bin,/usr/local/bin等.当给出一个命令时,Bash将自动对$PATH中的目录做一张hash表.$PATH中以":"分隔的目录列表将被存储在环境变量中.一般的,系统存储的$PATH定义在/ect/processed或~/.bashrc中在脚本中,这是一个添加目录到$PATH中的便捷方法.这样在这个脚本退出的时候,$PATH将会恢复(因为这个shell是个子进程,像这样的一个脚本是不会将它的父进程的环境变量修改的) 注意:当前的工作目录"./"在$PATH中一般都没有被添加.
$PPID 一个进程的$PPID就是它的父进程的进程id(pid).(当前运行的脚本的PID 为$$)使用pidof命令对比一下.
$PROMPT_COMMAND 这个变量保存一个在主提示符($PS1)显示之前需要执行的命令.
$PS1 主提示符,具体见命令行上的显示.
$PS2 第2提示符,当你需要额外的输入的时候将会显示,默认为">".
$PS3 第3提示符,在一个select循环中显示(见Example 10-29).
$PS4 第4提示符,当使用-x选项调用脚本时,这个提示符将出现在每行的输出前边.默认为"+".
$PWD 工作目录(你当前所在的目录).与pwd内建命令作用相同. 
$REPLY read命令如果没有给变量,那么输入将保存在$REPLY中.在select菜单中也可用,但是只提供选择的变量的项数,而不是变量本身的值. # REPLY是'read'命令结果保存的默认变量.
$SECONDS 这个脚本已经运行的时间(单位为秒).

``` sh
 #!/bin/bash
 
 TIME_LIMIT=10
 INTERVAL=1
 
 echo
 echo "Hit Control-C to exit before $TIME_LIMIT seconds."
 echo
 
 while [ "$SECONDS" -le "$TIME_LIMIT" ]
 do
   if [ "$SECONDS" -eq 1 ]
   then
     units=second
   else  
     units=seconds
   fi
 
   echo "This script has been running $SECONDS $units."
   #  在一台比较慢的或者是负载很大的机器上,这个脚本可能会跳过几次循环
   #+ 在一个while循环中.
   sleep $INTERVAL
 done
 
 echo -e "\a"  # Beep!
 
 exit 0
```

$SHELLOPTS 这个变量里保存shell允许的选项,这个变量是只读的.

```
  $ echo $SHELLOPTS
  braceexpand:hashall:histexpand:monitor:history:interactive-comments:emacs
```

$SHLVL Shell层次,就是shell层叠的层次,如果是命令行那$SHLVL就是1,如果命令行执行的脚本中,$SHLVL就是2,以此类推.
$TMOUT 如果$TMOUT环境变量被设置为一个非零的时间值,那么在过了这个指定的时间之后,shell提示符将会超时,这会引起一个logout.不知道用处是啥，，完全没有用过.

##### <a name="id0001-03" style="color: black">shell用户自定义变量</a>
用户定义的变量名必须由字母数字及下划线组成，并且变量名的第一个字符不能为数字。

自定义变量分为全局变量和局部变量,shell的变量默认是全局作用的，如果需要在一定范围内生效，则需要加上local限制。例如local a将设置a为局部变量。

如果在赋值后不希望改变变量，使其类似于常数，则可以使用readonly命令将其设为只读。
``` sh
#直接赋值为只读
readonly a="ls"
#先赋值，然后改成只读
b="b"
readonly b

echo $a
a="aaaa"
#a.sh: line 20: a: readonly variable
echo $a #ls
unset a
#a.sh: line 22: unset: a: cannot unset: readonly variable
a="a2"
#a.sh: line 23: a: readonly variable
echo $a #ls
```

## <a name="id0002" style="color: black">2.shell的基本控制语句</a>

```
if [ expr ]; then / elif [ expr ]; then / else / fi

for s in sth ; do / done

while [ expr ] ; do / done

case "var" in
yes|y|...)
  echo "hello world";; 在每个case分支最后一个语句，要用2个分号标注
[nN]*)
  echo "input no";;
 *)
  echo "input error";;
esac
```

在中括号[]中的shell的连接符说明如下: -a 表示AND, -o 表示OR.
在if判断中,可以将多个条件写入一个中括号内,也可以多个中括号来隔开！
而括号与括号之间，则以 && 或 || 来隔开,&&表示AND,||表示OR.
if [ $b -gt 0 -o $c -gt 0 -a $a -gt 0 ]; then
可以改写为
if [ $b -gt 0 ] || [ $c -gt 0 ] && [ $a -gt 0 ]; then

## <a name="id0003" style="color: black">3.二元比较</a>
```
操作         描述
算术比较                        
-eq          等于
==           等于
-ne          不等于
-lt          小于
-le          小于等于
-gt          大于
-ge          大于等于

算术比较     双括号(( ... ))结构
>            大于       (("$a" > "$b")) 
>=           大于等于   (("$a" >= "$b"))
<            小于       (("$a" < "$b"))
<=           小于等于   (("$a" <= "$b"))

字符串比较
-z           字符串为空
-n           字符串不为空
=            等于
==           等于
!=           不等于
\<           小于 (ASCII) *
\>           大于 (ASCII) *
```
在双[[&nbsp;&nbsp;]]中  字符串比较  if[[ "$a"<"Sb" ]],在单方括号情况下,字符<和>前须加转义符号

数字计算
shell中的数字计算比较特殊，未完待续。

* 1.每个命令之间用;隔开
说明：各命令的执行给果，不会影响其它命令的执行。换句话说，各个命令都会执行，但不保证每个命令都执行成功。

* 2.每个命令之间用&&隔开
说明：若前面的命令执行成功，才会去执行后面的命令。这样可以保证所有的命令执行完毕后，执行过程都是成功的。

* 3.每个命令之间用||隔开
说明：||是或的意思，只有前面的命令执行失败后才去执行下一条命令，直到执行成功一条命令为止。 

备注 
3.1 * 如果在双中括号 [[ ... ]] 测试结构中使用的话, 那么就不需要使用转义符\了.

## <a name="id0004" style="color: black">4.文件类型的测试操作</a>
```
操作    测试条件  
-e     文件是否存在
-f     是一个标准文件
-d     是一个目录
-x     文件具有执行权限
-r     文件具有读权限
-w     文件具有写权限
-h     文件是一个符号链接
-L     文件是一个符号链接
-b     文件是一个块设备
-c     文件是一个字符设备
-p     文件是一个管道
-S     文件是一个socket
-t     文件与一个终端相关联
-N     从这个文件最后一次被读取之后,它被修改过
-O     这个文件的宿主是你
-G     文件的组id与你所属的组相同
-s     文件大小不为0
-g     设置了sgid标记
-u     设置了suid标记
-k     设置了"粘贴位"
F1 -nt F2   文件F1比文件F2新 *
F1 -ot F2   文件F1比文件F2旧 *
F1 -ef F2   文件F1和文件F2都是同一个文件的硬链接 *
```
备注 
4.1  !  "非" (反转上边的测试结果) 

## <a name="id0005" style="color: black">5.字符串操作,其中$substring是一个正则表达式.</a>
```
表达式                            含义
${#string}                        $string的 长度
${string:position}                在$string中, 从位置$position开始从左往右提取子串,直到结束
${string:position:length}         在$string中, 从位置$position开始从左往右提取长度为$length的子串
${string:0-position}              在$string中, 从右边第$position字符开始，从左到右直到结束
${string:0-position:length}       在$string中, 从右边第$position字符开始从左往右提取长度为$length的子串

${string#substring}(1)            从 变量$string的开头, 删除最短匹配$substring的子串
${string##substring}              从 变量$string的开头, 删除最长匹配$substring的子串
${string%substring}               从 变量$string的结尾, 删除最短匹配$substring的子串
${string%%substring}              从 变量$string的结尾, 删除最长匹配$substring的子串

${string/substring/replacement}    使用$replacement, 来代替第一个匹配的$substring
${string//substring/replacement}   使 用$replacement, 代替所有匹配的$substring
${string/#substring/replacement}   如 果$string的前缀匹配$substring, 那么就用$replacement来代替匹配到的$substring
${string/%substring/replacement}   如果$string的后缀匹配$substring, 那么就用$replacement来代替匹配到的$substring


expr match "$string" '$substring'        匹配$string开头的$substring* 的长度
expr "$string" : '$substring'            匹配$string开头的$substring* 的长度
expr index "$string" $substring          在$string中匹配到的$substring的第一个字符出现的位置
expr substr $string $position $length    在$string中 从位置$position开始提取长度为$length的子串
expr match "$string" '\($substring\)'    从$string的 开头位置提取$substring*
expr "$string" : '\($substring\)'        从$string的 开头位置提取$substring*
expr match "$string" '.*\($substring\)'  从$string的 结尾提取$substring*
expr "$string" : '.*\($substring\)'      从$string的 结尾提取$substring*
```
备注
5.1 在opensuse,bash环境下,要写成 ${string#$substring},注意这个$符号,
    在不同的环境中,变现方式有可能不同,这条规则同样适用于替换的那部分.
    有可能在别的环境下,string也需要加$符号,多测试几下.

##  <a name="id0006" style="color: black">6. 参数替换和扩展</a>
```
表达式                     含义
${var}                     变量var的 值, 与$var相同
${var-DEFAULT}             如果var没 有被声明, 那么就以$DEFAULT作为其值 *
${var:-DEFAULT}            如果var没 有被声明, 或者其值为空, 那么就以$DEFAULT作为其值 *
${var=DEFAULT}             如果var没 有被声明, 那么就以$DEFAULT作为其值 *
${var:=DEFAULT}            如果var没 有被声明, 或者其值为空, 那么就以$DEFAULT作为其值 *
${var+OTHER}               如果var声 明了, 那么其值就是$OTHER, 否则就为null字符串
${var:+OTHER}              如 果var被设置了, 那么其值就是$OTHER, 否则就为null字符串
${var?ERR_MSG}             如果var没 被声明, 那么就打印$ERR_MSG *
${var:?ERR_MSG}            如果var没 被设置, 那么就打印$ERR_MSG *
${!varprefix*}             匹配之前所有以varprefix开头进行声明的变量
${!varprefix@}             匹配之前所有以varprefix开头进行声明的变量
```
备注
6.1  * 当然, 如果变量var已经被设置的 话, 那么其值就是$var.


## <a name="id0007" style="color: black">7. shell function</a>

因为 shell script 的运行方式是由上而下,由左而右,因此在shell script,当中的 function 的配置一定要在程序的最前面,function 也是拥有内建变量的,他的内建变量与 shell script 很类似, 函数名称代表示$0,而后续接的变量也是以 $1, $2... 来取代的
dash和bash的区别之一就是：在dash中，shell脚本是没有function关键字的，而bash的shell脚本里面有function关键字.在Ubuntu里，默认的sh是dash，而在CentOS中，默认的sh是bash。

``` sh
#bash 语法
function fun_name() 
{ 
	do sth #程序段 
}

#dash 语法
fun_name()
{
	do sth #程序段
}
```
有个问题需要注意下，脚本中的函数名字不要有中横线 - <br>

## <a name="id0008" style="color: black">8.shell 中一个重定向的问题</a>
  1>/dev/null 2>&1 

## <a name="id0009" style="color: black">9.脚本中常用的小脚本</a>
* dev_inst=\`ip route | grep default | awk '{print $5}'\` #找到默认的能访问外网的设备名称,通过默认路由的方式(有可能是多个)

* 通过while循环读取文件的每一行并进行判断和处理 
```
while read paraa parab
do
  if [ "Xsvr-list-end" == "X"$paraa ]; then 
     break; 
  fi
  echo $parab
  route add -host $parab dev $dev_inst
done < /etc/svrlist
```

## <a name="id0010" style="color: black">10.脚本常见问题</a>

#### shell中调用ftp命令时"Unexpected end of file"
在shell调用ftp等命令时，有时会报错 Unexpected end of file，而从报错信息没法看到报错原因，不管怎么调试，偶尔能好，好用的时候多数都是蒙出来的。一个常见场景如下
``` sh
#!/bin/sh
ftp -nivp <<asdfghjk
    open 1.1.1.1
    user user passwd
    binary
    get getsysinfo.py
    get get_status.py
    bye
    asdfghjk
```

上面这个ftp脚本代码就会报错 Unexpected end of file。解决办法有两种
``` sh
#!/bin/sh
ftp -nivp <<asdfghjk
    open 1.1.1.1 
    user user passwd
    binary
    get getsysinfo.py
    get get_status.py
    bye
asdfghjk
#asdfghjk 必须顶行开始，一个多余字符都不能有
#--------------------------------------------------------------
#!/bin/sh
    ftp -nivp <<-asdfghjk
        open 1.1.1.1
        user user passwd
        binary
        get getsysinfo.py
        get get_status.py
        bye
    [任意tab,可以和ftp -nivp保持一致]asdfghjk
#asdfghjk前面可以有任意个tab，可以和ftp -nivp保持一致，让shell代码更美观，
#必须是tab键，其他字符不行。当使用vim时，vim有的配置将tab键换成4个空格，
#这种情况下，可以用 ctrl+v，再按tab键的方式数据tab字符！
```
[参考链接 unexpected-end-of-file](http://askubuntu.com/questions/485567/unexpected-end-of-file)

### shell资源

[POSIX Shell 标准](http://pubs.opengroup.org/onlinepubs/9699919799/)

[Bash scripting Tutorial](https://linuxconfig.org/bash-scripting-tutorial)
[Getting Started with BASH](http://www.hypexr.org/bash_tutorial.php)
[Advanced Bash-Scripting Guide](http://tldp.org/LDP/abs/html/)
[Bash Reference Manual](https://www.gnu.org/software/bash/manual/html_node/index.html)
[shell变量](http://blog.csdn.net/zhuying_linux/article/details/6633022)
