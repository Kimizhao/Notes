# frp

树莓派安装frpc

```
wget https://github.com/fatedier/frp/releases/download/v0.32.0/frp_0.32.0_linux_arm.tar.gztar -zxvf frp_0.32.0_linux_arm.tar.gzcd frp_0.32.0_linux_arm/cat frpc.servicesudo cp frpc.service /etc/systemd/system/cd ../sudo cp frpc /usr/bin/sudo mkdir /etc/frp/sudo cp frpc.ini /etc/frp/systemctl daemon-reloadsystemctl enable frpcsystemctl start frpcsystemctl status frpc
```

frpc.ini

```
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

