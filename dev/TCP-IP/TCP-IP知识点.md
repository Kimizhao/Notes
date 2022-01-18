# TCP-IP知识点



## Keep-alive

[Keepalive](http://tldp.org/HOWTO/html_single/TCP-Keepalive-HOWTO/)

http://msdn.microsoft.com/en-us/library/ee470551(v=VS.85).aspx

[如何开启KeepAlive?](https://www.cnblogs.com/havenshen/p/3850167.html)

KeepAlive并不是默认开启的，在Linux系统上没有一个全局的选项去开启TCP的KeepAlive。需要开启KeepAlive的应用必须在TCP的socket中单独开启。Linux Kernel有三个选项影响到KeepAlive的行为：

1.net.ipv4.tcpkeepaliveintvl = 75

2.net.ipv4.tcpkeepaliveprobes = 9

3.net.ipv4.tcpkeepalivetime = 7200

tcpkeepalivetime的单位是秒，表示TCP链接在多少秒之后没有数据报文传输启动探测报文; tcpkeepaliveintvl单位是也秒,表示前一个探测报文和后一个探测报文之间的时间间隔，tcpkeepaliveprobes表示探测的次数。

TCP socket也有三个选项和内核对应，通过setsockopt系统调用针对单独的socket进行设置：

TCPKEEPCNT: 覆盖 tcpkeepaliveprobes

TCPKEEPIDLE: 覆盖 tcpkeepalivetime

TCPKEEPINTVL: 覆盖 tcpkeepalive_intvl

举个例子，以我的系统默认设置为例，kernel默认设置的tcpkeepalivetime是7200s, 如果我在应用程序中针对socket开启了KeepAlive,然后设置的TCP_KEEPIDLE为60，那么TCP协议栈在发现TCP链接空闲了60s没有数据传输的时候就会发送第一个探测报文。



## Nagle算法

[Nagle算法](https://baike.baidu.com/item/Nagle算法/5645172)



## Wireshark

[Wireshark常用过滤使用方法](https://www.cnblogs.com/nmap/p/6291683.html)

