### Linux-系统实用操作

1、查看cpu信息查看所有cpu信息：cat /proc/cpuinfo查看cpu类型： grep "model name" /proc/cpuinfo
2、查看内存信息：查看所有内存信息： cat /proc/meminfo查看内存容量： grep "MemTotal" /proc/meminfo
3、查看linux版本信息：cat /proc/version
4、查看系统信息：uname -a
5、查看cpu的位长：getconf LONG_BIT
6、查看磁盘信息：df -h
7、查看目录大小当前目录大小：du -sh子目录大小： du -sh *
8、查看当前cpu运行状况top
9、查看系统内存信息：free
10、查看usb信息lsusb（/sbin/lsusb)lsusb -tv
11、查看进程信息ps树状显示进程：ps -e -o pid,args --forest
12、监控网络tcpdump
13、查看用户打开的文件句柄的列表lsof查看打开用户目录的进程： lsof ~
14、查看和跟踪系统调用和信号strace
15、查看网卡/网络接口相关信息，包括ip，mac等ifconfig (/sbin/ifconfig)
16、查看路由相关信息ip (/sbin/ip)查看路由信息： /sbin/ip route查看ip地址： /sbin/ip addr



#### 查看文件大小

查看当前目录下的文件夹大小
文件目录的大小和文件夹包含的文件数
	    统计总数大小
	    du -sh xmldb/
	    du -sm * | sort -n //统计当前目录大小 并安大小 排序
	    du -sk * | sort -n
	    du -sk * | grep guojf //看一个人的大小
	    du -m | cut -d "/" -f 2 //看第二个/ 字符前的文字
	    查看此文件夹有多少文件 /*/*/* 有多少文件
	    du xmldb/
	    du xmldb/*/*/* |wc -l
	    40752
	    解释：
	    wc [-lmw]
	    参数说明：
	    -l :多少行
	    -m:多少字符
	    -w:多少字
	**查看文件大小**
	du -sh *
	**查看文件属性**
	df
	ls -l *



#### 计算器bc

```
zhaozh@ubuntu:~/shelltest$ bc
bc 1.06.95
Copyright 1991-1994, 1997, 1998, 2000, 2004, 2006 Free Software Foundation, Inc.
This is free software with ABSOLUTELY NO WARRANTY.
For details type `warranty'.
1+3
4
1/3
0
scale=3
1/3
.333
```



#### 系统磁盘分区建议

```
/ 根目录，唯一必须挂载的目录 2G
/boot  系统启动需要的文件，60MB～120MB之间
swap 最好是物理内存的1.5～2倍 
/usr 10G
/home
/var 2G
/tmp 2G
/bin 2G
```



#### 更改主机名

```
1.启用root用户
运行命令 sudo passwd root 为root用户设置密码
2.以root用户身份登录
1）编辑文件/etc/hosts 将下面的一行
127.0.1.1    xxxxx
替换为
127.0.1.1    newhostname
2) 编辑 /etc/hostname文件 删除该文件的所有内容，添加newhostname
3）运行一下命令 hostname newhostname
3.退出root用户 改用一般用户登录即可
注：
其中 xxxxx为原来的主机名 newhostname为你想修改的主机名
```



#### 显示内核版本

```
/ # cat   /proc/version
Linux version 2.6.35.7-svn255 (psj@ubuntuxmic) (gcc version 4.4.1 (Sourcery G++ Lite 2009q3-67) ) #495 PREEMPT Fri Dec 16 15:43:27 CST 2011
/ # uname -a
Linux localhost 2.6.35.7-svn255 #495 PREEMPT Fri Dec 16 15:43:27 CST 2011 armv7l GNU/Linux
/ # uname -r
2.6.35.7-svn255
/ # uname
Linux
/ # uname --help
BusyBox v1.16.0 (2010-02-26 14:52:34 HKT) multi-call binary.

Usage: uname [-amnrspv]

Print system information

Options:
        -a      Print all
        -m      The machine (hardware) type
        -n      Hostname
        -r      OS release
        -s      OS name (default)
        -p      Processor type
        -v      OS version
```

