---
title: wireshark和tcpdump
date: 2016-06-11 21:26:40
tags: [wireshark,tcpdump]
categories: tools
---

## wireshark

#### 在抓包时，尽量将所有的包都抓取，尽量少设置抓包过滤条件，防止相关的关键包丢失,在分析的时候，再使用更细致的过滤条件去分析问题

<!--more-->
常用wireshark 抓包命令

* 抓取tcp包并且端口为80 ``tcp.port == 80``
* 抓取tcp包，src方和dst方端口都不是22,这样才能不抓取某个固定端口的数据包
  ``tcp.srcport != 22 and  tcp.dstport != 22``
* 抓取ip包，ip src方是10.2.2.2 ``ip.src == 10.2.2.2``,ip dst方是10.2.2.2 ``ip.dst == 10.2.2.2``
* 在wireshark中，在某个包上面右键，选择 "Follow TCP Stream"或者 "Follow UDP Stream"这个选项会将和此包相关联的包一同显示在wireshark上。
## tcpdump 

* tcpdump 抓取网卡eth0, 端口是22的数据包 
  `` tcpdump -i eth0 port  22 -n -nn ``
* tcpdump 抓取网卡eth0, 端口不是22的数据包 
  `` tcpdump -i eth0 port not 22 -n -nn ``
* tcpdump 抓取网卡eth0, ip是1.1.1.1,端口是22的数据包 
  `` tcpdump -i eth0 host 1.1.1.1 and port 22 -n -nn ``
* tcpdump 选项-s.例子-s 0 : 抓取数据包时默认抓取长度为68字节,加上-s 0 后可以抓到完整的数据包.
* tcpdump 有个-w选项，可以 -w dumpfile，将抓取的数据包保存下来，供wireshark分析使用

#### 参考地址
[Wireshark图解教程](http://www.cnblogs.com/observer/archive/2011/11/04/2235219.html)
[一站式学习Wireshark（一）：Wireshark基本用法](http://blog.jobbole.com/70907/)
