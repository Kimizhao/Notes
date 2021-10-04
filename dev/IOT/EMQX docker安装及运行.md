# EMQX docker安装及运行

## 社区版

### 1.镜像页面地址：

​    https://hub.docker.com/r/emqx/emqx



### 2.拉取镜像命令：

​    docker pull emqx/emqx



### 3.Docker的一些操作命令：

docker后台启动命令：

```shell
docker run -d --name emqx -p 1883:1883 -p 8081:8081 -p 8083:8083 -p 8883:8883 -p 8084:8084 -p 18083:18083 emqx/emqx
```

进入emqx docker命令：

docker exec -it emqx /bin/sh

重启emqx docker命令：

docker restart emqx



### 4.访问EMQX web管理页面：

http://ip:18083  默认账号：admin 密码：public





## 企业版

https://docs.emqx.cn/enterprise/v4.3/getting-started/install-ee.html#freebsd

### 拉取

```
docker pull emqx/emqx-ee:4.3.2
```



### 启动

```
docker run -d \
    --name emqx-ee \
    -p 1883:1883 \
    -p 8083:8083 \
    -p 8883:8883 \
    -p 8084:8084 \
    -p 18083:18083 \
    -v /root/emqx/emqx.lic:/opt/emqx/etc/emqx.lic \
    emqx/emqx-ee:4.3.2-alpine-amd64
```





[Emqx高可用架构](https://www.cnblogs.com/jimoliunian/p/12964975.html)
