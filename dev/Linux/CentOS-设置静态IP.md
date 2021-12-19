### CentOS-设置静态IP

#### 设置静态IP

关于静态IP设置官方已经给出答案有兴趣的可以看[官方WIKI](https://wiki.centos.org/FAQ/CentOS7#head-a21a9e454157700367c9b7e9ccb1ff9954bec881)指导，这里直接给出配置方案，需要配置两个地方，所有操作需要管理员（root）权限！

#### 配置文件

在 /etc/sysconfig/network-scripts 路径下找到 ifcfg-* ，* 代表具体网卡，本文修改的网卡是 ifcfg-enp0s3 ，你的有可能是 ifcfg-eth0 ，除 ONBOOT 和 BOOTPROTO 修改外，其他几项为新增。修改后内容参见下文。

```
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
#BOOTPROTO="dhcp"
BOOTPROTO="static"
IPADDR="192.168.100.201"
NETMASK="255.255.255.0"
GATEWAY="192.168.100.1"
DNS1="114.114.114.114"
DNS2="8.8.8.8"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="enp0s3"
UUID="75218b2b-df44-4a1c-9eca-13c91025414c"
DEVICE="enp0s3"
ONBOOT="yes"
```

#### 重置网络配置

```
service network restart
```





TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=eth0
UUID=7b1db590-b340-48f1-82ef-36325741ec17
DEVICE=eth0
ONBOOT=no
IPADDR=172.19.2.179
PREFIX=24
GATEWAY=172.19.2.254
DNS1=221.13.30.242
IPV6_PRIVACY=no





[hadoop@hadoop003 ~]$ cat /etc/sysconfig/network-scripts/ifcfg-eth0
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=eth0
UUID=7b1db590-b340-48f1-82ef-36325741ec17
DEVICE=eth0
ONBOOT=yes
IPADDR=172.19.2.179
PREFIX=24
GATEWAY=172.19.2.254
DNS1=114.114.114.114
DNS2=8.8.8.8
IPV6_PRIVACY=no
