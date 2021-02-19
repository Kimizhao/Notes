### 树莓派-运行腾讯云智能灯接入lightdemo

#### 创建tencentyun目录

当前目录下

```
mkdir tencentyun && cd tencentyun
```

#### 下载lightdemo例程

从[Github](https://github.com/tencentyun/qcloud-iot-explorer-sdk-embedded-c)下载，或执行下面git命令。

```
git clone --depth=1 https://github.com/tencentyun/qcloud-iot-explorer-sdk-embedded-c.git
```

#### 修改Demo程序

上述 git 命令执行成功后，会生成一个 qcloud-iot-sdk-embedded-c 目录

1.进入 qcloud-iot-explorer-sdk-embedded-c 目录。
2.修改该目录下的 device_info.json 文件。

```
nano device_info.json
```

```
{
    "auth_mode":"KEY",

    "productId":"JPRIGY6599",
    "productSecret":"4uZ+P4dfIHmBo3ytyTFclw==",
    "deviceName":"dev002_pi",

    "key_deviceinfo":{
        "deviceSecret":"4uZ+P4dfIHmBo3ytyTFclw=="
    },

    "cert_deviceinfo":{
        "devCertFile":"YOUR_DEVICE_CERT_FILE_NAME",
        "devPrivateKeyFile":"YOUR_DEVICE_PRIVATE_KEY_FILE_NAME"
    },

    "subDev":{
        "subdev_num":6,
        "subdev_list":
        [
                {"sub_productId": "WPDA0S6S08", "sub_devName": "dev001"},
                {"sub_productId": "WPDA0S6S08", "sub_devName": "dev002_pi"},
                {"sub_productId": "WPDA0S6S08", "sub_devName": "dev003"},
                {"sub_productId": "Y8T6NB8DM0", "sub_devName": "test001"},
                {"sub_productId": "Y8T6NB8DM0", "sub_devName": "test002"},
                {"sub_productId": "Y8T6NB8DM0", "sub_devName": "test003"}
        ]
    },

    "region":"china"
}
```

#### 编译

1. 安装cmake

```
sudo apt-get install cmake
```

​	2.在 qcloud-iot-sdk-embedded-c 目录下执行以下命令进行编译。

```
./cmake_build.sh
```

​	3.编译成功后，会在 output/release/bin 目录下生成 light_data_template_sample 执行文件。

#### 运行Demo程序

1. 进入 output/release/bin 目录。
2. 输入 ./light_data_template_sample。
3. 运行成功后，系统输出示例如下：

```
pi@raspberrypi:~/tencentyun/qcloud-iot-explorer-sdk-embedded-c/output/release/bin $ ./light_data_template_sample
INF|2021-02-18 23:17:37|qcloud_iot_device.c|iot_device_info_set(56): SDK_Ver: 3.1.5, Product_ID: JPRIGY6599, Device_Name: dev002_pi
DBG|2021-02-18 23:17:37|HAL_TLS_mbedtls.c|HAL_TLS_Connect(224): Setting up the SSL/TLS structure...
DBG|2021-02-18 23:17:37|HAL_TLS_mbedtls.c|HAL_TLS_Connect(266): Performing the SSL/TLS handshake...
DBG|2021-02-18 23:17:37|HAL_TLS_mbedtls.c|HAL_TLS_Connect(267): Connecting to /JPRIGY6599.iotcloud.tencentdevices.com/8883...
DBG|2021-02-18 23:17:37|HAL_TLS_mbedtls.c|HAL_TLS_Connect(289): connected with /JPRIGY6599.iotcloud.tencentdevices.com/8883...
INF|2021-02-18 23:17:37|mqtt_client.c|IOT_MQTT_Construct(125): mqtt connect with id: JBiB7 success
DBG|2021-02-18 23:17:37|mqtt_client_subscribe.c|qcloud_iot_mqtt_subscribe(147): topicName=$thing/down/property/JPRIGY6599/dev002_pi|packet_id=8447
DBG|2021-02-18 23:17:37|data_template_client.c|_template_mqtt_event_handler(277): template subscribe success, packet-id=8447
INF|2021-02-18 23:17:37|light_data_template_sample.c|event_handler(344): subscribe success, packet-id=8447
INF|2021-02-18 23:17:37|data_template_client.c|IOT_Template_Construct(885): Sync device data successfully
INF|2021-02-18 23:17:37|light_data_template_sample.c|main(717): Cloud Device Construct Success
DBG|2021-02-18 23:17:37|light_data_template_sample.c|_usr_init(377): add your init code here
INF|2021-02-18 23:17:37|light_data_template_sample.c|_register_data_template_property(486): data template property=power_switch registered.
INF|2021-02-18 23:17:37|light_data_template_sample.c|_register_data_template_property(486): data template property=color registered.
INF|2021-02-18 23:17:37|light_data_template_sample.c|_register_data_template_property(486): data template property=brightness registered.
INF|2021-02-18 23:17:37|light_data_template_sample.c|_register_data_template_property(486): data template property=name registered.
INF|2021-02-18 23:17:37|light_data_template_sample.c|_register_data_template_property(486): data template property=position registered.
INF|2021-02-18 23:17:37|light_data_template_sample.c|main(739): Register data template propertys Success
DBG|2021-02-18 23:17:37|mqtt_client_publish.c|qcloud_iot_mqtt_publish(347): publish packetID=0|topicName=$thing/up/property/JPRIGY6599/dev002_pi|payload={"method":"report_info", "clientToken":"JPRIGY6599-0", "params":{"module_hardinfo":"ESP8266","module_softinfo":"V1.0","fw_ver":"3.1.5","imei":"11-22-33-44","lat":"22.546015","lon":"113.941125","mac":"11:22:33:44:55:66", "device_label":{"append_info":"your self define info"}}}
DBG|2021-02-18 23:17:37|data_template_client.c|_reply_ack_cb(194): replyAck=0
DBG|2021-02-18 23:17:37|data_template_client.c|_reply_ack_cb(197): Received Json Document={"method":"report_info_reply","clientToken":"JPRIGY6599-0","code":0,"status":"success"}
DBG|2021-02-18 23:17:37|mqtt_client_publish.c|qcloud_iot_mqtt_publish(347): publish packetID=0|topicName=$thing/up/property/JPRIGY6599/dev002_pi|payload={"method":"get_status", "clientToken":"JPRIGY6599-1"}
DBG|2021-02-18 23:17:37|data_template_client.c|_get_status_reply_ack_cb(211): replyAck=0
DBG|2021-02-18 23:17:37|data_template_client.c|_get_status_reply_ack_cb(215): Received Json Document={"method":"get_status_reply","clientToken":"JPRIGY6599-1","code":0,"status":"success","data":{"reported":{"brightness":0,"name":"dev002_pi","power_switch":0,"color":0}}}
DBG|2021-02-18 23:17:38|light_data_template_sample.c|main(773): Get data status success
DBG|2021-02-18 23:17:38|mqtt_client_publish.c|qcloud_iot_mqtt_publish(347): publish packetID=0|topicName=$thing/up/property/JPRIGY6599/dev002_pi|payload={"method":"report", "clientToken":"JPRIGY6599-2", "params":{"power_switch":0,"color":0,"brightness":0,"name":"dev002_pi","position":{"longitude":1,"latitude":1}}}
INF|2021-02-18 23:17:38|light_data_template_sample.c|main(817): data template reporte success
INF|2021-02-18 23:17:39|light_data_template_sample.c|OnReportReplyCallback(471): recv report_reply(ack=-1): {"method":"report_reply","clientToken":"JPRIGY6599-2","code":406,"status":"checkReportData fail"}
```



#### 注册系统服务

`sudo touch /etc/systemd/system/iotlight.demo.service`

`sudo vim /etc/systemd/system/iotlight.demo.service`

```
[Unit]
Description=Tencent Cloud light demo service
After=network.target


[Service]
Type=simple
WorkingDirectory=/home/pi/tencentyun/qcloud-iot-explorer-sdk-embedded-c/output/release/bin
SyslogIdentifier=iotlightdemo
User=root
Restart=on-failure
RestartSec=5s
ExecStart=/home/pi/tencentyun/qcloud-iot-explorer-sdk-embedded-c/output/release/bin/light_data_template_sample

[Install]
WantedBy=multi-user.target
```

系统服务重新加载`sudo systemctl daemon-reload`

启用iotlight.demo服务`sudo systemctl enable iotlight.demo`

```
Created symlink /etc/systemd/system/multi-user.target.wants/iotlight.demo.service → /etc/systemd/system/iotlight.demo.service.
```

启动iotlight.demo服务`sudo systemctl start iotlight.demo`

查看iotlight.demo服务`sudo systemctl status iotlight.demo`

```
● iotlight.demo.service - Tencent Cloud light demo service
   Loaded: loaded (/etc/systemd/system/iotlight.demo.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2021-02-18 23:27:14 CST; 6s ago
 Main PID: 5650 (light_data_temp)
   Memory: 256.0K
   CGroup: /system.slice/iotlight.demo.service
           └─5650 /home/pi/tencentyun/qcloud-iot-explorer-sdk-embedded-c/output/release/bin/light_data_template_sample

2月 18 23:27:15 raspberrypi iotlightdemo[5650]: DBG|2021-02-18 23:27:15|mqtt_client_publish.c|qcloud_iot_mqtt_publish(347): publish packetID=0|topicName=$thing/up/property/JPRIG
2月 18 23:27:15 raspberrypi iotlightdemo[5650]: DBG|2021-02-18 23:27:15|data_template_client.c|_reply_ack_cb(194): replyAck=0
2月 18 23:27:15 raspberrypi iotlightdemo[5650]: DBG|2021-02-18 23:27:15|data_template_client.c|_reply_ack_cb(197): Received Json Document={"method":"report_info_reply","clientTo
2月 18 23:27:15 raspberrypi iotlightdemo[5650]: DBG|2021-02-18 23:27:15|mqtt_client_publish.c|qcloud_iot_mqtt_publish(347): publish packetID=0|topicName=$thing/up/property/JPRIG
2月 18 23:27:15 raspberrypi iotlightdemo[5650]: DBG|2021-02-18 23:27:15|data_template_client.c|_get_status_reply_ack_cb(211): replyAck=0
2月 18 23:27:15 raspberrypi iotlightdemo[5650]: DBG|2021-02-18 23:27:15|data_template_client.c|_get_status_reply_ack_cb(215): Received Json Document={"method":"get_status_reply"
2月 18 23:27:15 raspberrypi iotlightdemo[5650]: DBG|2021-02-18 23:27:15|light_data_template_sample.c|main(773): Get data status success
2月 18 23:27:15 raspberrypi iotlightdemo[5650]: DBG|2021-02-18 23:27:15|mqtt_client_publish.c|qcloud_iot_mqtt_publish(347): publish packetID=0|topicName=$thing/up/property/JPRIG
2月 18 23:27:15 raspberrypi iotlightdemo[5650]: INF|2021-02-18 23:27:15|light_data_template_sample.c|main(817): data template reporte success
2月 18 23:27:16 raspberrypi iotlightdemo[5650]: INF|2021-02-18 23:27:16|light_data_template_sample.c|OnReportReplyCallback(471): recv report_reply(ack=-1): {"method":"report_rep
```





#### 参考

智能灯接入指引：https://cloud.tencent.com/document/product/1081/41155