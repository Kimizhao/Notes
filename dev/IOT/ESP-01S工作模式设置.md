### ESP-01S工作模式设置

#### STA模式

-> AT+CWMODE=1 //设置为 STA 模式

-> AT+CWJAP="Alpine Home WiFi","20201218!@#" //链接 Server 端 ESP8266 模块的 Wi-Fi 

-> AT+CWAUTOCONN=1 //上电自动连接 AP 

-> AT+CIFSR //查看模组 MAC 地址和 IP 地址 

-> AT+CIPMUX=1 //启动多连接 

-> AT+CIPSERVER=1 //建立server

-> AT+CIPSTART=0,"TCP","192.168.0.100",5000 // 连接 server 端

-> AT+CIPSEND=5 //发送数据



#### AP模式

设置模块为AP

```
AT+CWMODE=2
```

设置模块的热点参数，参数为模块的SSID、PASSWORD、信道号和加密方式

```
AT+CWLAP="esp01s","1234567890",11,3
```

使能多连接

```
AT+CIPMUX=1
```

设置TCP服务器

```
AT+CIPSERVER=1,8888
连接上
0,CONNECT
```

发送模式，后面跟只能发送5个字符

```
AT+CIPSEND=0,5
OK
>
```



参考：

esp8266_start_guide_1_.pdf

https://blog.csdn.net/qq_34020487/article/details/100904978

https://blog.csdn.net/liuxianfei0810/article/details/107210821