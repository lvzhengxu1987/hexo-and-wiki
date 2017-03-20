---
title: iptables
date: 2016-06-19 15:34:21
tags: iptables
categories: iptables
keywords: iptables
---
目录

1. <a href="#iptables-简介">iptables简介</a>
2. <a href="#iptables-预置的table和chain" style="line-height:50%;">iptables 预置的table和chain</a>
3. <a href="#iptables-命令格式">iptables 命令格式</a>
4. <a href="#iptables-应用方向">iptables 应用方向</a>
5. <a href="#iptables-原理">iptables 原理</a>
6. <a href="#iptables优缺点">iptables优缺点</a>
7. <a href="#iptables的下一代-nftables">iptables的下一代 nftables</a>

## iptables 简介
iptables，一个运行在用户空间的应用软件，iptables设定控制规则，通过Linux内核netfilter模块，来管理网络数据包的流动与转送。在大部分的Linux系统上面，iptables是使用/usr/sbin/iptables来操作，使用方法可以通过 man iptables 指令获取。通常iptables都需要内核层级的模块来配合运作，Xtables是主要在内核层级里面iptables API运作功能的模块。因相关动作上的需要，iptables的操作需要用到超级用户的权限。
目前iptables系在2.4、2.6及3.0的内核底下运作，旧版的Linux内核（2.2）使用ipchains及ipwadm（Linux 2.0）来达成类似的功能，2014年1月19日起分发的新版Linux内核（3.13后）则使用nftables取而代之。


## iptables 预置的table和chain
Table 包括 filter, nat, mangle, raw四种内建表

* filter:This is the default table (if no -t option is passed). It contains the built-in chains  INPUT  (for packets  destined to local sockets), FORWARD (for packets being routed through the box), and OUTPUT (for locally-generated packets).
这是iptables默认使用的表(如果没有使用-t option 选项),它包括这内建的chains INPUT(针对那些到达本地sockets的数据包),FORWARD(为了将数据包从此台机器转发)，OUTPUT(针对本地数据包发送出去)
* nat:This table is consulted when a packet that creates a new connection is encountered.  It consists of three  built-ins:  PREROUTING  (for altering packets as soon as they come in), OUTPUT (for altering locally-generated packets before routing), and POSTROUTING (for altering packets as they are about to go out).
nat 当一个链接创建时会查看此表规则，它有三个内建表：PREROUTING (在数据包进来前的预处理)，OUTPUT(本地数据包在路由前进行修改)，POSTROUTING(当数据包准备出去时进行修改)

* mangle:
    This  table  is  used  for  specialized packet alteration.  Until kernel 2.4.17 it had two built-in chains: PREROUTING (for altering incoming packets before routing) and OUTPUT (for altering locally-generated  packets before routing).  Since kernel 2.4.18, three other built-in chains are also supported: INPUT (for packets coming into the box itself), FORWARD (for altering packets being  routed through the box), and POSTROUTING (for altering packets as they are about to go out).
    Mangle这个表是用来指定的数据包修改，在内核版本2.4.17之前，它有两个内建chain，一个是PREOUTING(为了那些incoming的数据包还有没进行路由前)，OUTPUT(为了那些本地包还没有路由前)，自从2.4.18之后的版本，另外三个内建的chain也被支持了，INPUT(为了那些进入本机的数据包)，FORWARD(为了那些)

## iptables 命令格式
iptables  常用参数及常用命令
iptables [-t table] {-A|-C|-D} chain rule-specification
iptables [-t table] -I chain [rulenum] rule-specification
iptables [-t table] -R chain rulenum rule-specification
iptables [-t table] -D chain rule-specification
iptables [-t table] -D chain rulenum
iptables [-t table] -S [chain [rulenum]]
iptables [-t table] {-F|-L|-Z} [chain [rulenum]] [options...]
iptables [-t table] -N chain
iptables [-t table] -X [chain]
iptables [-t table] -P chain target
iptables [-t table] -E old-chain-name new-chain-name
rule-specification = [matches...] [target]
match = -m matchname [per-match-options]
target = -j targetname [per-target-options]

-A：在某chain添加過濾規則
-I：把過濾規則插入chain內特定位置
-R：置換chain內某位置的過濾規則
-D：刪除chain內某道過濾規則

iptables 配置可以存放的位置：/etc/rc.d/rc.local ；/etc/sysconfig/iptables
iptables增加nat规则： iptables -t nat -I PREROUTING -p tcp --dport 90 -j DNAT --to 104.250.150.2:90
iptables显示nat规则: iptables -L -t nat -n -v --line-numbers
iptables删除nat某条规则: iptables -t nat -D PREROUTING <num>
拒绝所有请求的80端口 TCP连接 iptables -I INPUT -p TCP --dport 80 -j DROP
只允许46.166.150.22请求80端口TCP连接 iptables -I INPUT -s 46.166.150.22 -p TCP --dport 80 -j ACCEPT

iptables 日志使用规则

## iptables 应用方向
接下来说一下iptables的几个应用方向
1. 防护主机，一般来说，这个功能是iptables的主要功能，此时iptables作为防火墙，将入口ip控制住，不让陌生的ip访问这台机器，也不让某个ip过多次数访问这个系统，这样就能在一定程度上保护主机不被攻击，尤其是密码字典暴力破解。但是像ddos这种攻击，iptables只能在一定程度上能有所抵抗，在海量的ddos面前，我觉得iptables的作用有限。
一个防护的iptables示例，先用-F参数清空filter中的规则，然后添加80和443，TCP状态是syn的端口访问；然后再添加22端口的，目的地址是xx.xx.xx.xx；再添加源地址是127.0.0.1，目的地址是127.0.0.1，端口是9000的规则，还有类似的3306端口规则；然后加上<b>--state RELATED,ESTABLISHED</b>这种规则，这个规则的含义是，只要是允许了的规则的数据包进来了就允许出去，或者是本机主动出去的数据包也让他返回来，没有这个规则的话，经常看到的现象是：外部机器访问本机时，符合规则的数据包可以进来，但是却无法出去，或者本机ping外部机器，用tcpdump只能获取出去的包了；最后一个规则<b>-p all -j REJECT</b>，含义就是如果在它上面的规则都不符合的话，那么这个数据包走到它这里就意味着被拒绝了。
```
iptables -t filter -F
iptables -t filter -I INPUT -p tcp -m state --state NEW  --dport 80 -j ACCEPT
iptables -t filter -I INPUT -p tcp -m state --state NEW  --dport 443 -j ACCEPT
iptables -t filter -I INPUT -p tcp -m state --state NEW  -s xx.xx.xx.xx --dport 22 -j ACCEPT
iptables -t filter -I INPUT -p tcp -m state --state NEW -s 127.0.0.1 -d 127.0.0.1 --dport 9000 -j ACCEPT
iptables -t filter -I INPUT -p tcp -m state --state NEW  -s xx.xx.xx.xx --dport 3306 -j ACCEPT
iptables -t filter -A INPUT -p all -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -t filter -A INPUT -p all -j REJECT
```
2. 数据包转发，主要是nat表，在PREROUTING中设置本地被iptables监听的端口，将符合规则的数据包的目的地址改成转发的目的地址，然后再POSTROUTING中将数据包的源地址换成自己的ip，这样就能把数据包转发走了。

3. 配合VPN软件，此时iptables的filter表中会有一两条规则允许VPN软件的虚拟网卡上面的数据包进行forward转发，然后nat表会有一条规则把虚拟网卡的数据转换成本机ip并发送出去。

4. 配合策略路由，此时iptables和策略路由一起，可以将一个局域网中不同的机器的数据转发到各个不同的外部节点中，这些节点可以是不同的网卡，也可以是不同的外部ip地址等等。

## iptables 原理
* iptables 可以在路由处理过程中加以干预，但是不能决定路由的走向。
* iptables 处理顺序可以看看下面这个图片![ Netfilter-packet-flow](/site_files/Netfilter-packet-flow.svg.png)

## iptables优缺点

## iptables的下一代 nftables
nftables 的使用方法 和 与iptables之间的差别。

参考地址
[防火墙详解 + 配置抗DDOS攻击](http://www.2cto.com/Article/201501/371537.html)
[iptables防DDOS攻击和CC攻击设置](http://sookk8.blog.51cto.com/455855/321242/)
-[iptables防DDOS攻击和CC攻击设置](http://sookk8.blog.51cto.com/455855/321242/)
-[Netfilter机制分析](http://bbs.chinaunix.net/thread-1750611-1-1.html)
-[Netfilter实现机制分析](http://bbs.chinaunix.net/thread-2008344-1-1.html)
