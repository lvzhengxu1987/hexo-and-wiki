---
title: tmux
date: 2016-06-19 06:14:02
tags: [linux,tmux]
categories: tmux
keywords: tmux
---
tmux 操作命令
<!--more-->

|操作命令|含义描述|
|--------|:------:|
|tmux |开启tmux|
|tmux ls |显示已有tmux列表（C-b s）|
|tmux attach-session -t 数字 |选择tmux|
|C-b c|创建一个新的窗口|
|C-b n|切换到下一个窗口|
|C-b p|切换到上一个窗口|
|C-b l|最后一个窗口,和上一个窗口的概念不一样哟,谁试谁知道|
|c-b w|通过上下键选择当前窗口中打开的会话|
|C-b 数字|直接跳到你按的数字所在的窗口|
|C-b &|退出当前窗口|
|C-b d|临时断开会话 断开以后,还可以连上的哟:)|
|C-b "|分割出来一个窗口|
|C-b %|分割出来一个窗口|
|C-b o|在小窗口中切换|
|C-b (方向键)|根据方向键切换小窗口|
|C-b !|关闭所有小窗口|
|C-b x|关闭当前光标处的小窗口|
|C-b t|钟表|
|C-b pageup/pagedown|翻滚整个屏幕,使用后感觉效果不好|
