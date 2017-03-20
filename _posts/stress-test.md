---
title: 压力测试
date: 2016-07-17 01:01:59
tags: [nmon,压力测试]
categories: 测试
keywords: nmon
---

#### 压力测试
测试理论:
构造测试模型
准备测试用数据

测试方法:
首先使用工具进行某个交易场景的一个交易多次发送，获取当前场景下，一个交易请求成功返回的耗时平均值作为参考值。
然后使用多线程方式，以并发方式在同一时间点向服务器发送大量交易请求，然后等待交易结果返回，在每个线程中设置时间点，计算每个动作完成后，各个时间耗费多少。最后汇总，打印报表信息。
通过计算每个线程做完工作的总耗时，还有一共有多少个线程处在这一个交易场景中，可以计算出所有线程的耗时时间，可以计算出耗时平均值，可以获取线程中耗时最大者，也可以获取线程中耗时最小者，还有这些线程中，动作成功完成的数量和动作失败的数量。
<!--more-->

计算每个步骤的使用时间可以使用一下代码
``` c
#define GET_T(v)  memset(&(v),0x00, sizeof(struct timeval));gettimeofday(&(v),NULL);
struct timeval v1,v2;
GET_T(v1);
......
GET_T(v2);
debuglog("xxx step cost time %-6f\n",(1000000 * (tv2.tv_sec-tv1.tv_sec)+ tv2.tv_usec-tv1.tv_usec)/(double)1000000);
```
压力测试从时间上分为：
1.短时间段的压力测试，例如 5分钟或10分钟段的压力测试，主要是调试bug。
2.较长时间段的压力测试，例如 半小时或一小时的压力测试，这个是基本的压力测试时间。看看程序是否会出故障
3.整天的压力测试。这个是检查程序在长时间的高负载情况下，程序的稳定性。

压力测试从测试场景分为：
1.单一场景。比如只测试登录动作，所有的交易压力都是登录动作，只测试登录。
2.多个交易顺序场景。
3.多个交易随机场景。

压力测试报告
不知道....

##### 压力测试使用工具
1. 客户端工具:使用手写的多线程工具进行并发交易请求,在编译c代码时，可以添加-pg选项，当程序运行一段时间后，就会生成gmon.out,使用gprof命令可以显示 function call graph profile data,这部分显示数据对分析程序性能有不少帮助，可以找到很多程序可以优化的地方。
2. 服务器检测工具:nmon
3. 服务器压力数据报表工具:nmon_analyser

##### 监控服务器的工具
Munin Nagios Zabbix nmon icinga
top htop 等等


#### 参考链接
[Linux监控工具munin的安装和配置](http://www.cnblogs.com/rond/p/3757804.html)
[Munin installation](http://guide.munin-monitoring.org/en/latest/installation/index.html)
[Nmon工具的使用以及通过nmon_analyse生成分析报表 ](http://phpseyo.iteye.com/blog/1958502)
[Linux下Nagios的安装与配置](http://www.cnblogs.com/mchina/archive/2013/02/20/2883404.html)
[Linux下Nagios的安装与配置](https://linux.cn/article-2436-1.html)
[netdata](https://github.com/firehol/netdata/wiki)
[nmon使用方法](http://www.ibm.com/developerworks/aix/library/au-nmon_analyser/index.html)
[nmon各个版本下载，注意适用的服务器](http://nmon.sourceforge.net/pmwiki.php?n=Site.Download)
