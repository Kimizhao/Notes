### Ubuntu 18.04 LTS时钟同步

```shell
$ ntpdate ntp1.aliyun.com
```



系统服务配置

```shell
# 查看是否启动
$ sudo systemctl status systemd-timesyncd.service
# 未启动重启时间同步
$ sudo systemctl restart systemd-timesyncd.service

$ sudo systemctl status systemd-timesyncd.service
● systemd-timesyncd.service - Network Time Synchronization
   Loaded: loaded (/lib/systemd/system/systemd-timesyncd.service; disabled; vendor preset: enabled)
  Drop-In: /lib/systemd/system/systemd-timesyncd.service.d
           └─disable-with-time-daemon.conf
   Active: active (running) since 五 2021-06-04 16:21:19 CST; 3s ago
     Docs: man:systemd-timesyncd.service(8)
 Main PID: 16230 (systemd-timesyn)
   Status: "Synchronized to time server 91.189.89.198:123 (ntp.ubuntu.com)."
    Tasks: 2
   Memory: 436.0K
      CPU: 18ms
   CGroup: /system.slice/systemd-timesyncd.service
           └─16230 /lib/systemd/systemd-timesyncd
```



新增ntp服务器配置

```shell
$ sudo vim /etc/systemd/timesyncd.conf
```



添加，多个空格隔开。

```
[Time]
NTP=ntp1.aliyun.com
```



查看新增后状态

```shell
$ sudo systemctl status systemd-timesyncd.service
● systemd-timesyncd.service - Network Time Synchronization
   Loaded: loaded (/lib/systemd/system/systemd-timesyncd.service; disabled; vendor preset: enabled)
  Drop-In: /lib/systemd/system/systemd-timesyncd.service.d
           └─disable-with-time-daemon.conf
   Active: active (running) since 五 2021-06-04 16:34:32 CST; 7min ago
     Docs: man:systemd-timesyncd.service(8)
 Main PID: 21433 (systemd-timesyn)
   Status: "Synchronized to time server 120.25.115.20:123 (ntp1.aliyun.com)."
    Tasks: 2
   Memory: 392.0K
      CPU: 16ms
   CGroup: /system.slice/systemd-timesyncd.service
           └─21433 /lib/systemd/systemd-timesyncd

6月 04 16:34:32 ubuntu-Shangqi-N720 systemd[1]: Starting Network Time Synchronization...
6月 04 16:34:32 ubuntu-Shangqi-N720 systemd[1]: Started Network Time Synchronization.
6月 04 16:34:32 ubuntu-Shangqi-N720 systemd-timesyncd[21433]: Synchronized to time server 120.25.115.20:123 (ntp1.aliyun.com).
```

