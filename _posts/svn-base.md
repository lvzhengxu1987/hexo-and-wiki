---
title: svn-base
date: 2016-08-22 01:00:12
tags: svn
categories: 版本管理
keywords: svn,版本管理
---
### svn 常用命令

``svn add``                  添加某个文件
``svn commit``               将修改后的文件更新到svn服务器上
``svn update``               将svn服务器上的最新文件更新到本机上
``svn checkout [-r 版本号]`` 将svn服务器上某个库放置到本机上
``svn export [-r 版本号]``   将svn服务器上某个库放置到本机上，但不带有svn版本控制信息

<!-- more -->
### svn branches tag trunk
trunk是主线，branches是分支，而tag是标记。
 
一种方式：在软件的每一个release中，都是在各自的branch中进行开发，trunk只做发布版本中使用。
这种开发模式当中，trunk是不承担具体开发任务的，一个版本/阶段的开发任务在开始的时候，根据已经release的版本做新的开发分支，并且基于这个分支进行开发。这种方式中，把每个release版本分开，每个版本各自为战，互相关联不大。这样可以开发很多定制版本。每个版本有各自的bug，自己修复自己的。但是如果有个公共bug，那么就要所有版本都要进行更新。

一种方式：所有的开发都是在trunk中进行，当一个版本/release开发告一段落后，代码就会封版。然后打tag。当下一个版本/阶段的开发任务开始，继续在trunk进行开发。此时，如果发现了上一个已发行版本（Released Version）有一些bug，或者一些很急迫的功能要求，而正在开发的版本（Developing Version）无法满足时间要求，这时候就需要在上一个版本上进行修改了。应该基于发行版对应的tag，做相应的分支（branch）进行开发。这种版本模式有些像瀑布模型，一步一步按部就班的来。

不同的svn管理方式也是产品的特性决定的吧。

Trunk would be the main body of development, originating from the start of the project until the present.

Branch will be a copy of code derived from a certain point in the trunk that is used for applying major changes to the code while preserving the integrity of the code in the trunk. If the major changes work according to plan, they are usually merged back into the trunk.

Tag will be a point in time on the trunk or a branch that you wish to preserve. The two main reasons for preservation would be that either this is a major release of the software, whether alpha, beta, RC or RTM, or this is the most stable point of the software before major revisions on the trunk were applied.

参考地址：
[SVN中trunk,branches,tags用法详解](http://www.cnblogs.com/dafozhang/archive/2012/06/28/2567769.html)
[What do "branch", "tag" and "trunk" mean in Subversion repositories?](http://stackoverflow.com/questions/16142/what-do-branch-tag-and-trunk-mean-in-subversion-repositories)
[An Agile Perspective on Branching and Merging](https://www.cmcrossroads.com/article/agile-perspective-branching-and-merging?page=0%2C0)

