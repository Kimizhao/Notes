# Portainer简明使用教程

Docker的可视化界面管理工具。

## 1. 登陆

地址：http://192.168.10.100:9901/

账号：ygadmin

密码：ygadmin@yg.cn

![1-login](..\..\images\portainer\1-login.png)



## 2.首页

进入Home。

![2-home](..\..\images\portainer\2-home.png)

目前已有多个Endpoints连接。

local为本机的docker，其余为所管理的他机docker。



## 3.管理Endpoint

### 3.1 进入Endpoint仪表盘

​	在首页点击Endpoint名为“docker-hadoop69”，进入该Endpoint的仪表盘(Dashboard)。

![3.1-endpoint-dashboard](..\..\images\portainer\3.1-endpoint-dashboard.png)

**端点信息总览**

|          |                                                     |      |
| -------- | --------------------------------------------------- | ---- |
| Endpoint | docker-hadoop69 4 3 GB - Standalone 20.10.7 + Agent | 名称 |
| URL      | 192.168.10.69:9001                                  | 地址 |
| Tags     |                                                     | 标签 |

0 **Stacks**

6 **Container**

37 **Images**

0 **Volumes**

3 **Networks**



### 3.2 容器Container管理

![3.2-endpoint-containers](..\..\images\portainer\3.2-endpoint-containers.png)

#### 3.2.1 启停操作

![3.2-endpoint-containers-start-stop](..\..\images\portainer\3.2-endpoint-containers-start-stop.png)

勾选container，可进行，Start(启动)、Stop(停止)、Restart(重启)、Pause(中止)、Remove(移除)、Add(添加)，操作。

#### 3.2.2 查看日志

从Quick actions，进入查看日志。

![3.2-endpoint-containers-quickaction-logs](..\..\images\portainer\3.2-endpoint-containers-quickaction-logs.png)

或点击名称进入Container，点击Logs

![image-20211014111909958](C:\Users\kimiz\AppData\Roaming\Typora\typora-user-images\image-20211014111909958.png)

#### 3.2.3 查阅

![3.2-endpoint-containers-quickaction-inspect](..\..\images\portainer\3.2-endpoint-containers-quickaction-inspect.png)

#### 3.2.3 查看统计

内存使用率

CPU使用率

网络使用率

IO使用率

![3.2-endpoint-containers-quickaction-stats](..\..\images\portainer\3.2-endpoint-containers-quickaction-stats.png)

#### 3.2.4 进入控制台

点击“Console”。进入如下连接控制台页面，选择Command，点击“Connect”。

![3.2-endpoint-containers-quickaction-console](..\..\images\portainer\3.2-endpoint-containers-quickaction-console.png)



### 3.3 镜像Images管理

可对镜像进行，移除，构建、导入、导出操作。

![3.3-endpoint-images](..\..\images\portainer\3.3-endpoint-images.png)



### 3.4 卷Volume管理

对卷进行新增和删除。

![3.4-endpoint-volume](..\..\images\portainer\3.4-endpoint-volume.png)



### 3.5 网络network管理

对网络进行新增和删除。

![3.5-endpoint-networks](..\..\images\portainer\3.5-endpoint-networks.png)



### 3.6 堆管理

对堆进行新增和删除。

![3.6-endpoint-stacks](..\..\images\portainer\3.6-endpoint-stacks.png)