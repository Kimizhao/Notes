# Docker入门手册

## Windows下安装

1. 安装桌面版Docker Desktop：https://www.docker.com/products/docker-desktop
2. 安装Linux内核更新包：https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-kernel



## Docker 镜像常用命令

### 搜索镜像
```shell
$ docker search java
```


### 下载镜像

 ```shell
 $ docker pull java:8
 $ docker pull macro/eureka-server:0.0.1
 ```


### 列出镜像
```shell
$ docker images
```

### 删除镜像

```shell
$ docker rmi java
$ docker rmi -f java 
$ docker rmi -f $(docker images)
```

## Docker 容器常用命令
### 新建并启动容器
```shell
$ docker run -d -p 91:80 nginx
```

### 列出容器

```shell
$ docker ps
```

### 停止容器

```shell
$ docker stop $ContainerId
```

### 强制停止容器

```shell
$ docker kill $ContainerId
```

### 启动已停止的容器

```shell
$ docker start $ContainerId
```

### 进入容器

```shell
$ docker inspect --format "{{.State.Pid}}" $ContainerId
$ nsenter --target "$pid" --mount --uts --ipc --net --pid
```

### 删除容器

```shell
$ docker rm $ContainerId
$ docker rm -f $(docker ps -a -q)
```

### 查看启动错误日志

```shell
$ docker logs $ContainerIdName(或者$ContainerId)
```

### 查看容器的IP地址（172.17.0.*）

```shell
$ docker inspect --format '{{ .NetworkSettings.IPAddress }}' $ContainerId
```

### 同步宿主机时间到容器

```shell
$ docker cp /etc/localtime $ContainerName:/etc/
```

### 在宿主机查看docker使用cpu、内存、网络、io情况

```shell
# 查看指定容器情况：
$ docker stats $ContainerName

# 查看所有容器情况：
$ docker stats -a
```

### 进入docker内部的bash

```shell
# docker exec -it $ContainerName /bin/bash
```



### Dockerfile构建镜像

带参数构建

```bash
docker build --build-arg JAR_FILE=helloworld-0.0.1-SNAPSHOT.jar -t hello:v1 .
```

```dockerfile
FROM java:8
EXPOSE 8080
ARG JAR_FILE
ADD target/${JAR_FILE} /helloworld-0.0.1.jar
ENTRYPOINT ["java", "-jar","/helloworld-0.0.1.jar"]
```

### 使用重启策略

|                  | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| `no`             | 不要自动重启容器。（默认）                                   |
| `on-failure`     | 如果容器由于错误而退出，则重新启动容器，该错误表现为非零退出代码。 |
| `always`         | 如果容器停止，请务必重启容器。如果手动停止，则仅在Docker守护程序重新启动或手动重新启动容器本身时才重新启动。（参见[重启政策详情中](https://docs.docker.com/config/containers/start-containers-automatically/#restart-policy-details)列出的第二个项目） |
| `unless-stopped` | 类似于`always`，除了当容器停止（手动或其他方式）时，即使在Docker守护程序重新启动后也不会重新启动容器。 |

以下示例启动Redis容器并将其配置为始终重新启动，除非明确停止或重新启动Docker。

```
$ docker run -dit --restart unless-stopped redis
```

如果run时没有添加restart 可以通过update命令追加

```
docker update --restart=always web
```



### 参考教程

1. 菜鸟教程：https://www.runoob.com/docker/docker-tutorial.html
2. Docker 入门教程：http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.htmlDocker 
3. 微服务教程：http://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html
4. 加速：Docker windows安装并启用镜像加速https://www.jianshu.com/p/b21c508514ae
5. [Docker自动启动容器](https://www.cnblogs.com/wei9593/p/11192908.html)