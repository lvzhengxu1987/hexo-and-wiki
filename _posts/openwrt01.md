---
title: openwrt基础总结
date: 2016-06-19 06:04:54
tags: [openwrt,uhttpd,dnsmasq,iptables,策略路由]
categories: openwrt
keywords: openwrt uhttpd dnsmasq iptables 策略路由
---

openwrt 从去年年末一直到今年年中，这么长的一段时间一直在弄openwrt，从开始的编译openwrt windows用于测试的版本到使用lua语言在里面编写openwrt相关cgi程序。学到很多知识。

在编译openwrt时，一定要注意你的路由设备的flash大小，然后在编译的时候要对一些功能进行取舍，毕竟编译的bin包超出了flash的大小，那这个bin包绝对是无法烧写上去。
<!--more-->
首先是openwrt的双网卡，这个双网卡跟家里的路由器是同一种概念，其实openwrt也好，家用路由器也好，用的都是NAT技术，简单来说，家用路由器和openwrt都是把家里的网络划分成两个网口，一个是LAN口，一个是WAN口，LAN口对内，就是我们常见的ip段，192.168.1.1，而WAN口常见的就是网络运营商给的那个账号和密码喽，路由器内部用过NAT技术将LAN口接收的数据转发至WAN口，这样我们家里的那个小局域网里面的网络设备就可以方位互联网啦。

openwrt中有一个uhttpd这个程序，它是一个很简单的http服务器，他的原理大致如链接中的文章所示。[uhttpd的实现框架](http://www.cnblogs.com/zmkeil/archive/2013/05/14/3078766.html),[Luci实现框架](http://www.cnblogs.com/zmkeil/archive/2013/05/14/3078774.html),[uhttpd源码分析](http://wanghuanming.com/2014/08/uhttpd-code-analysis/).

上面说了一下uhttpd，我在openwrt中主要是用了uhttpd的cgi程序，在cgi程序中，第一段代码必须是echo "Content-type: text/html";echo ""，第一个echo代表这cgi返回的html头信息，第二个echo代表这回车换行，这个其实是http请求中通用的返回头，cgi也要遵守这个规定，刚开始写cgi程序时，不知道这个规则，导致走了很莫名其妙的错误，不知道愿意，也就没地方下手去解决问题了。在熟悉cgi规则后，使用lua语言编写了启动openvpn，设置iptables，设置dnsmasq，设置策略路由等等功能。既熟悉了这些技术，也熟悉了lua语言。

在openwrt中有个dnsmasq，它既是dns服务，也是dhcp服务，作为dns服务时，使用/etc/dnsmasq.conf这个配置文件中的内容，主要是server=114.114.114.114这种配置来指定dns服务器，作为dhcp服务时，可以读取/etc/ethers文件，如果/etc/ethers中有mac ip这么一对儿记录时，就能根据dhcp请求服务，获取ip。dhcp协议本身并不复杂，一般情况下，首先是没有ip的机器广播整个局域网(客户机发DHCPDISCOVER广播包)，接下来dhcp服务器发出相应包(服务器发DHCPOFFER广播包),接下来，客户机发出请求获取IP的包(客户机发DHCPREQUEST广播包),最后dhcp服务器将分配好的ip发送给客户机(服务器发DHCPACK/DHCPNAK广播包).到此dhcp结束。当dnsmasq作为dns时，它监听着53端口，若有dns数据包请求，就把数据包转发到server指定的ip上。

在linux系统中有个策略路由，它和路由策略是完全不一样的。策略路由可以根据源路由来分配数据包的走向，也可以根据目的路由来决定数据包走向，可以在数据包级别做负载均衡等等，在我的openwrt中，我就是使用策略路由+iptables+dhcp组合功能，先是使用dhcp分配ip给客户机，然后使用iptables将我分配的ip指向到固定网卡上，使用 ip rule命令将源ip指向到某个路由表中。接下来在那个路由表中添加0.0.0.0的路由项和default的路由项，这样就可以让某个源ip上送的数据只走那个指定的路由表了！

在linux系统中，iptables是很出名的，iptables本身功能很强大，但是它也有限制，iptables不能干涉路由的选择。它是在路由处理过程中添加了hooks，在每一个hook上进行处理，但是它不能干涉路由的处理。iptables 是users层的工具，而真正在起作用的是内核层的Netfilter。
