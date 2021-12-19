# EMQX平台简明教程

​	物联网平台基础架构中 MQTT 代理人。

## 1. 登录

地址：http://192.168.10.68:18083/#/login

用户名：admin

密码：public

![1-emqx-login](..\..\images\emqx\1-emqx-login.png)



## 2. 控制台

![1-emqx-monitor](..\..\images\emqx\1-emqx-monitor.png)



## 3.客户端

MQTT客户端

![3-emqx-client](..\..\images\emqx\3-emqx-client.png)



## 4.主题

系统中活跃的主题

![4-emqx-topic](..\..\images\emqx\4-emqx-topic.png)



## 5.订阅

![5-emqx-sub](..\..\\images\emqx\5-emqx-sub.png)



## 6.工具（常用）

### 6.1 Websocket

发布和订阅，常用于调试设备数据

**连接**

点击“连接”，订阅。

![6-tool-websocket-connect](..\..\images\emqx\6-tool-websocket-connect.png)



**订阅/发布**

![6-tool-websocket-pubsub](..\..\images\emqx\6-tool-websocket-pubsub.png)



新能源主题：

调试车架号: `FLS00000000000022`

1.订阅全部上行数据，主题为：`jtne/upstream/FLS00000000000022/#`



2.发布下行数据，

查询主题：`jtne/dnstream/FLS00000000000022/query`

设置主题：`jtne/dnstream/FLS00000000000022/setting`

控制主题：`jtne/dnstream/FLS00000000000022/control`



