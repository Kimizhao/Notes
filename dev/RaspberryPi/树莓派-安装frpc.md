# 树莓派安装frpc



```shell
$ wget https://github.com/fatedier/frp/releases/download/v0.32.0/frp_0.32.0_linux_arm.tar.gz
$ tar -zxvf frp_0.32.0_linux_arm.tar.gz
$ cd frp_0.32.0_linux_arm/systemd/
$ sudo cp frpc.service /etc/systemd/system/
$ cd ../
$ sudo cp frpc /usr/bin/
$ sudo mkdir /etc/frp/
$ sudo cp frpc.ini /etc/frp/
$ sudo systemctl daemon-reload
$ sudo systemctl enable frpc
$ sudo systemctl start frpc
$ sudo systemctl status frpc
```



frpc.ini

```shell
[common]
server_addr = 47.96.0.107
server_port = 7000

admin_addr = 0.0.0.0
admin_port = 7400
admin_user = admin
admin_pwd = admin

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000
[xrdp]
type = tcp
local_ip = 127.0.0.1
local_port = 3389
remote_port = 6001
```



安装XRDP服务

```
sudo apt-get install xrdp
```

