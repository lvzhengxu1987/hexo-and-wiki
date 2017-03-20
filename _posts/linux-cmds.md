---
title: linux-常用命令
date: 2016-07-16 19:34:56
tags: [find,ps]
categories: linux_commands
keywords: find
---

##### find
``find path -name "*.[c|cpp|h]" -type f| xargs grep --color=auto  "contents"``
与ack合用,安装ack``curl http://beyondgrep.com/ack-2.14-single-file > ~/bin/ack && chmod 0755 !#:3``
也可以下载[文件](/site_files/files/ack)到/bin/目录下，再修改权限即可
``find path -name "*.[c|cpp|h]" -type f| xargs ack "contents"``
<!--more-->
##### ps
``` sh
#信息来源与man手册
#To see every process on the system using standard syntax:
ps -e
ps -ef
ps -eF
ps -ely
#To see every process on the system using BSD syntax:
ps ax
ps axu
#To print a process tree:
ps -ejH
ps axjf
#To get info about threads:
ps -eLf
ps axms
#To get security info:
ps -eo euser,ruser,suser,fuser,f,comm,label
ps axZ
ps -eM
#To see every process running as root (real & effective ID) in user format:
ps -U root -u root u
#To see every process with a user-defined format:
ps -eo pid,tid,class,rtprio,ni,pri,psr,pcpu,stat,wchan:14,comm
ps axo stat,euid,ruid,tty,tpgid,sess,pgrp,ppid,pid,pcpu,comm
ps -eopid,tt,user,fname,tmout,f,wchan
#Print only the process IDs of syslogd:
ps -C syslogd -o pid=
#Print only the name of PID 42:
ps -p 42 -o comm=
```
