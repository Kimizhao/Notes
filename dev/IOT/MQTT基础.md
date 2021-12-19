# MQTT基础

## MQTT协议

### [概览](https://docs.emqx.cn/broker/v4.3/development/protocol.html#概览)

MQTT是一个轻量的发布订阅模式消息传输协议，专门针对低带宽和不稳定网络环境的物联网应用设计。

MQTT官网: [http://mqtt.org(opens new window)](http://mqtt.org/)

MQTT V3.1.1协议规范: [http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html(opens new window)](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html)

### [特点](https://docs.emqx.cn/broker/v4.3/development/protocol.html#特点)

1. 开放消息协议，简单易实现
2. 发布订阅模式，一对多消息发布
3. 基于TCP/IP网络连接
4. 1字节固定报头，2字节心跳报文，报文结构紧凑
5. 消息QoS支持，可靠传输保证

### [应用](https://docs.emqx.cn/broker/v4.3/development/protocol.html#应用)

MQTT协议广泛应用于物联网、移动互联网、智能硬件、车联网、电力能源等领域。

1. 物联网M2M通信，物联网大数据采集
2. Android消息推送，WEB消息推送
3. 移动即时消息，例如Facebook Messenger
4. 智能硬件、智能家具、智能电器
5. 车联网通信，电动车站桩采集
6. 智慧城市、远程医疗、远程教育
7. 电力、石油与能源等行业市场







## 开发库

https://github.com/mqtt/mqtt.org/wiki/libraries

.NET

1.[MQTTnet](https://github.com/chkr1011/MQTTnet)

2.[.net core使用MQTT](https://www.cnblogs.com/luoocean/p/11232752.html)

Java



