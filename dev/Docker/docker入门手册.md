# Docker入门手册

## Windows下安装

1. 安装桌面版Docker Desktop：https://www.docker.com/products/docker-desktop
2. 安装Linux内核更新包：https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-kernel

## Docker 镜像常用命令

### 搜索镜像
docker search java
### 下载镜像
- docker pull java:8
- docker pull macro/eureka-server:0.0.1
### 列出镜像
docker images
### 删除镜像
- docker rmi java
- docker rmi -f java 
- docker rmi -f $(docker images)

## Docker 容器常用命令
### 新建并启动容器
docker run -d -p 91:80 nginx
### 列出容器
docker ps
### 停止容器
docker stop $ContainerId
### 强制停止容器
docker kill $ContainerId
### 启动已停止的容器
docker start $ContainerId
### 进入容器
- docker inspect --format "{{.State.Pid}}" $ContainerId
- nsenter --target "$pid" --mount --uts --ipc --net --pid
### 删除容器
- docker rm $ContainerId
- docker rm -f $(docker ps -a -q)
### 查看启动错误日志
docker logs $ContainerIdName(或者$ContainerId)
### 查看容器的IP地址（172.17.0.*）
docker inspect --format '{{ .NetworkSettings.IPAddress }}' $ContainerId
### 同步宿主机时间到容器
docker cp /etc/localtime $ContainerName:/etc/
### 在宿主机查看docker使用cpu、内存、网络、io情况
- 查看指定容器情况：docker stats $ContainerName
- 查看所有容器情况：docker stats -a
### 进入docker内部的bash
docker exec -it $ContainerName /bin/bash

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



### 参考教程

1. 菜鸟教程：https://www.runoob.com/docker/docker-tutorial.html
2. Docker 入门教程：http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.htmlDocker 
3. 微服务教程：http://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html
4. 加速：Docker windows安装并启用镜像加速https://www.jianshu.com/p/b21c508514ae

