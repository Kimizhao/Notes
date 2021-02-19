### Linux-journalctl-查看系统日志

可用于查询由systemd-journald.service（8）编写的systemd（1）日记的内容。

**查看某个服务日志**

```
journalctl -f -u iotlight.demo
```

其中：

 `-f`代表跟踪(follow)服务日志

`-u`代表指定服务iotlight.demo