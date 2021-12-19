### docker-compose

[Awesome Compose](https://github.com/docker/awesome-compose) 托管在Github上的一些精选列表，适合开发环境，对于生产环境不适用。



安装

https://docs.docker.com/compose/install/

```
# 下载compose，1.29.2替换最新版本
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   633  100   633    0     0    631      0  0:00:01  0:00:01 --:--:--   632
100 12.1M  100 12.1M    0     0   159k      0  0:01:17  0:01:17 --:--:-- 2497k

# 将可执行权限应用于二进制文件
$ sudo chmod +x /usr/local/bin/docker-compose

#查看版本
$ docker-compose --version
docker-compose version 1.29.2, build 5becea4c
```

