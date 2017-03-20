---
title: 网络基础知识
date: 2016-06-11 21:20:32
tags: network
categories: 网络
---
1. <a href="#id0001">网络报文头部</a>
2. <a href="#id0002">TCP报文字段解释</a>
3. <a href="#id0003">NAT 地址转换</a>

<!--more-->

<SCRIPT language=javascript src="/site_scripts/image.js"></script>
## <a name="id0001" style="color: black">报文头部图片</a>
### IP报文格式
<img src="/site_files/2015-09-10_110808.png"/>

### TCP报文格式
TCP报文总览<br>
<img src="/site_files/tcp-general.png" /><br>
TCP头部详细信息<br>
<img src="/site_files/TCP-Header.png" /><br>

### UDP报文格式
<img src="/site_files/udp-general.png" />

## <a name="id0002" style="color: black">TCP报文字段解释</a>
源端口、目的端口：16位长。标识出远端和本地的端口号。<br>
顺序号：32位长。表明了发送的数据报的顺序。<br>
确认号：32位长。希望收到的下一个数据报的序列号。<br>
TCP协议数据报头长读：Data Offset – 4 位。表明TCP头中包含多少个4字节。TCP头部长度的计算公式是Data Offset * 4，得出的结果就是tcp 头长度，单位是bytes(字节)。接下来的6位未用，Reserved，预留以备用。必须设置为0。<br>
ACK：ACK位置1表明确认号是合法的。如果ACK为0，那么数据报不包含确认信息，确认字段被省略。<br>
PSH：表示是带有PUSH标志的数据。接收方因此请求数据报一到便可送往应用程序而不必等到缓冲区装满时才传送。<br>
RST：用于复位由于主机崩溃或其它原因而出现的错误的连接。还可以用于拒绝非法的数据报或拒绝连接请求。<br>
SYN：用于建立连接。<br>
FIN：用于释放连接。<br>
窗口大小：16位长。窗口大小字段表示在确认了字节之后还可以发送多少个字节。<br>
校验和：16位长。是为了确保高可靠性而设置的。它校验头部、数据和伪TCP头部之和。<br>
可选项：0个或多个32位字。包括最大TCP载荷，窗口比例、选择重发数据报等选项。<br>
最大TCP载荷：允许每台主机设定其能够接受的最大的TCP载荷能力。在建立连接期间，双方均声明其最大载荷能力，并选取其中较小的作为标准。如果一台主机未使用该选项，那么其载荷能力缺省设置为536字节。<br>
窗口比例：允许发送方和接收方商定一个合适的窗口比例因子。这一因子使滑动窗口最大能够达到232字节。<br>
TCP协议数据报头选择重发数据报：这个选项允许接收方请求发送指定的一个或多个数据报。<br>
在协议中DATA又可以称为payload(有效载荷,可以查看这个单词原意)<br>

### TCP创建连接、数据传输、断开连接
<img src="/site_files/tcp-connect-disconnect.png" />

### TCP ACK
Rules for Generating ACK (1)<br>
1. When one end sends a data segment to the other end, it must include an ACK.  That gives the next sequence number it expects to receive. (Piggyback)<br>
2. The receiver needs to delay sending (until another segment arrives or 500ms) an ACK segment if there is only one outstanding in-order segment. It prevents ACK segments from creating extra traffic.<br>
3. There should not be more than 2 in-order unacknowledged segments at any time. It prevent the unnecessary retransmission<br>
4. When a segment arrives with an out-of-order sequence number that is higher than expected, the receiver immediately sends an ACK segment announcing the sequence number of the next expected segment. (for fast retransmission)<br>
5. When a missing segment arrives, the receiver sends an ACK segment to announce the next sequence number expected.<br>
6. If a duplicate segment arrives, the receiver immediately sends an ACK.<br>

## <a name="id0003" style="color: black">NAT 地址转换</a>


