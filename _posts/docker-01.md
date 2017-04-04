---
title: docker基础
date: 2016-11-29 20:50:54
tags: docker
categories: linux
---

## docker 介绍
docker 是一种针对应用程序这个级别的容器封装，它类似于我们常用的虚拟机，但是和虚拟机又有些不同。
docker同虚拟机相比(vmware,virtualbox),docker 是应用程序级别的容器，传统的虚拟机模拟的是cpu，硬盘，内存等等硬件信息，再依靠虚拟机里面的操作系统，实现了一个模拟的系统；而docker是依赖于操作系统的现有技术(kernel的命名空间，AUFS，cgroups),可以是实际系统，也可以是虚拟机系统中，在里面利用那些技术实现一个或多个应用程序的容器封装。现在开发测试以及部署一个程序时，都可以在一个docker镜像中，就像在一个u盘上一样，可插拔，放到哪里都能运行。在docker中，docker镜像由Dockerfile控制构建规则，然后搭建好的镜像可以在libcontainer中运行，docker容器的概念有些类似于java的虚拟机。可以类比java的jdk与docker的libcontainer。
在docker中运行的一般是一个程序或者多个程序，而且程序一般不以daemon形式运行，并不像虚拟机那样，跑着整个操作系统。
<!--more-->

#### 容器与虚拟机几点不同：[(摘自Docker简介与入门)](https://segmentfault.com/a/1190000000448808)

> 1. 虚拟化技术依赖物理CPU和内存，是硬件级别的；而docker构建在操作系统上，利用操作系统的containerization技术，所以docker甚至可以在虚拟机上运行。
> 2. 虚拟化系统一般都是指操作系统镜像，比较复杂，称为“系统”；而docker开源而且轻量，称为“容器”，单个容器适合部署少量应用，比如部署一个redis、一个memcached。
> 3. 传统的虚拟化技术使用快照来保存状态；而docker在保存状态上不仅更为轻便和低成本，而且引入了类似源代码管理机制，将容器的快照历史版本一一记录，切换成本很低。
> 4. 传统的虚拟化技术在构建系统的时候较为复杂，需要大量的人力；而docker可以通过Dockfile来构建整个容器，重启和构建速度很快。更重要的是Dockfile可以手动编写，这样应用程序开发人员可以通过发布Dockfile来指导系统环境和依赖，这样对于持续交付十分有利。
> 5. Dockerfile可以基于已经构建好的容器镜像，创建新容器。Dockerfile可以通过社区分享和下载，有利于该技术的推广。

#### Docker的主要特性如下: [(摘自Docker简介与入门)](https://segmentfault.com/a/1190000000448808)

> 1. 文件系统隔离：每个进程容器运行在完全独立的根文件系统里。
> 2. 资源隔离：可以使用cgroup为每个进程容器分配不同的系统资源，例如CPU和内存。
> 3. 网络隔离：每个进程容器运行在自己的网络命名空间里，拥有自己的虚拟接口和IP地址。
> 4. 写时复制：采用写时复制方式创建根文件系统，这让部署变得极其快捷，并且节省内存和硬盘空间。
> 5. 日志记录：Docker将会收集和记录每个进程容器的标准流（stdout/stderr/stdin），用于实时检索或批量检索。
> 6. 变更管理：容器文件系统的变更可以提交到新的映像中，并可重复使用以创建更多的容器。无需使用模板或手动配置。
> 7. 交互式Shell：Docker可以分配一个虚拟终端并关联到任何容器的标准输入上，例如运行一个一次性交互shell。

#### docker 生命周期
{% img /site_files/docker-life.png %}

#### docker 服务配置信息
在ubuntu环境中,设置docker DNS，让docker在pull镜像或者访问网络时，可以通过这个dns去解析域名，修改/etc/default/docker,在里面加入`DOCKER_OPTS="--dns 223.5.5.5 --dns 114.114.114.114"`


#### docker images设置
1. 如果 生成image要在国内网络使用的情况下：可以通过ADD命令 来替换image中的resolv.conf 与 update源文件，比如将resolv.conf中默认都是8.8.8.8,image访问网络会很慢的,换成223.5.5.5之后会好很多，假设image是ubuntu的话，替换image中的/etc/apt/sources.list,就可以让ubuntu使用国内的ubuntu更新源服务。这两个对于docker来说，都是加快网络访问的方式.

#### docker 命令
`docker build -t name:version .` 寻找当前目录的Dockerfile并安装Dockerfile的顺序编译docker image。
`docker run -d -t name:version `执行docker image ，生成一个docker contaioner
`docker exec -it "docker container id" /bin/sh` 以某个docker image的id执行交互窗口命令

#### docker 一个简单的Dockerfile格式与其中的命令

## docker 资源链接
##### 学习资源
[Docker 主站点](https://www.docker.io)
[docker 入门到实践](https://yeasy.gitbooks.io/docker_practice/content/)
[各种资源](http://docs.daocloud.io/daocloud-translate/startup-60-tools)
[Linux_namespaces](https://en.wikipedia.org/wiki/Linux_namespaces)
[Cgroups](https://en.wikipedia.org/wiki/Cgroups)
[Aufs](https://en.wikipedia.org/wiki/Aufs)
[OverlayFS](https://en.wikipedia.org/wiki/OverlayFS)
[OverlayFS Doc](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/Documentation/filesystems/overlayfs.txt)
[Linux kernel Namespaces and cgroups](http://www.haifux.org/lectures/299/netLec7.pdf)
[Docker 网络设置](http://www.oschina.net/translate/docker-network-configuration)
##### 资源网站
[dockerfile online manager](http://dockerfile.github.io/)
[docker images manager](https://github.com/shipyard/shipyard)

