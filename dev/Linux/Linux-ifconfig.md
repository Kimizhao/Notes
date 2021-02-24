### Linux-ifconfig

#### 修改MAC地址

1.用命令行临时解决：

```
#sudo ifconfig eth0 down
#sudo ifconfig eth0 hw ether AA:BB:CC:DD:EE:FF
#sudo ifconfig eth0 up
```

2.启动自行修改

```
#sudo vi /etc/network/interfaces
```

在`eth0`的配置中加入如下

```
hwaddress ether AA:BB:CC:DD:EE:FF
```