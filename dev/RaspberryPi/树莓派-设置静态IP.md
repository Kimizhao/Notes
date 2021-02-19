### 树莓派设置静态IP

设置树莓派IP地址

以前修改`/etc/network/interfaces`，即可。

```
auto lo 

iface lo inet loopback 
iface eth0 inet static 

address 192.168.1.222 
netmask 255.255.255.0 
gateway 192.168.1.1 
```

`sudo vim /etc/network/interfaces`

```
pi@raspberrypi:~ $ sudo vim /etc/network/interfaces
# interfaces(5) file used by ifup(8) and ifdown(8)

# Please note that this file is written to be used with dhcpcd
# For static IP, consult /etc/dhcpcd.conf and 'man dhcpcd.conf'

# Include files from /etc/network/interfaces.d:
source-directory /etc/network/interfaces.d
```

打开 `interfaces` 文件发现，开头注释里提示要修改静态IP地址，需要修改的是 /etc/dhcpcd.conf 也就是 DHCP 的配置文件。
执行命令

```
sudo nano /etc/dhcpcd.conf
```

在`dhcpcd.conf`文件后面添加如下内容：

```
# It is possible to fall back to a static IP if DHCP fails:
# define static profile
#profile static_eth0
#static ip_address=192.168.1.23/24
#static routers=192.168.1.1
#static domain_name_servers=192.168.1.1

# fallback to static profile on eth0
#interface eth0
#fallback static_eth0

interface eth0
static ip_address=192.168.0.101/24
static routers=192.168.0.1
static domain_name_servers=192.168.0.1 8.8.8.8

interface wlan0
static ip_address=192.168.0.102/24
static routers=192.168.0.1
static domain_name_servers=192.168.0.1 8.8.8.8
```

其中，`eth0`是有线的配置，`wlan0`是无线配置
`ip_address`为静态IP，后面要接/24
`routers`是网关
`static domain_name_servers`是DNS
然后再在命令行执行

```shell
sudo reboot
```

重启树莓派。