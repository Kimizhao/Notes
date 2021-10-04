# Docker入门手册

## Windows下安装

1. 安装桌面版Docker Desktop：https://www.docker.com/products/docker-desktop
2. 安装Linux内核更新包：https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-kernel





## CentOS安装

参考：https://www.runoob.com/docker/centos-docker-install.html



## 使用官方安装脚本自动安装

```
curl -fsSL https://get.docker.com | bash -s docker --mirror aliyun
```



## 手动安装

```
# 添加阿里源
$ sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    
# 安装20.10.7版本
$ sudo yum install docker-ce-20.10.7 docker-ce-cli-20.10.7 containerd.io
```



## **启动Docker**

```
$ sudo systemctl start docker
```



```
$ sudo systemctl enable docker
```



## **将用户加入Docker用户组**

```
# 将test用户加入docker用户组，这样就不需要每次在docker命令前加sudo
sudo usermod -aG docker test
# 更新用户组
newgrp docker
```



## **查看版本**

```
$ docker version
Client: Docker Engine - Community
 Version:           20.10.6
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        370c289
 Built:             Fri Apr  9 22:45:33 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.6
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       8728dd2
  Built:            Fri Apr  9 22:43:57 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.6
  GitCommit:        d71fcd7d8303cbf684402823e425e9dd2e99285d
 runc:
  Version:          1.0.0-rc95
  GitCommit:        b9ee9c6314599f1b4a7f497e1f1f856fe433d3b7
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```



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



## Docker 命令行

### [docker run](https://docs.docker.com/engine/reference/commandline/run/)

在新容器中运行命令

选项

#### 启动一个容器

```shell
$ docker run --name test -itd debian
```

`--name`: 将test名称分配给容器

`--interactive , -i`:Keep STDIN open even if not attached

`--tty` , `-t`: Allocate a pseudo-TTY 分配伪终端

`--detach` , `-d`: Run container in background and print container ID 后台运行容器并打印容器ID



#### 挂载目录到宿主目录

```shell
$ docker  run  -v `pwd`:`pwd` -w `pwd` -i -t  ubuntu pwd
```

The `-v` flag mounts the current working directory into the container. The `-w` lets the command being executed inside the current working directory, by changing into the directory to the value returned by `pwd`. So this combination executes the command using the container, but inside the current working directory. 

`-v`:当前目录挂载到容器。

`-w`:设置当前工作目录，到PWD返回的目录

```shell
$ docker run -v /doesnt/exist:/foo -w /foo -i -t ubuntu bash
```

When the host directory of a bind-mounted volume doesn’t exist, Docker will automatically create this directory on the host for you. In the example above, Docker will create the `/doesnt/exist` folder before starting your container.

如果绑定挂载的宿主目录不存在，Docker将自动创建`/doesnt/exist`目录。



#### 将条目添加到容器HOST主机文件

```shell
$ docker run --add-host=docker:10.180.0.1 --rm -it debian
```

Sometimes you need to connect to the Docker host from within your container. To enable this, pass the Docker host’s IP address to the container using the `--add-host` flag. To find the host’s address, use the `ip addr show` command.

通过add-host标签将容器HOST主机文件添加条目



## Docker 图形化

官方[安装portainer](https://documentation.portainer.io/v2.0/deploy/ceinstalldocker/)

中文[安装链接](http://www.netkiller.cn/virtualization/docker/ch01s02.html#idm108453187840)

Linux下

### Portainer服务端部署

```
$ docker volume create portainer_data
$ docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```

### Portainer代理部署

```
$ docker run -d -p 9001:9001 --name portainer_agent --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker/volumes:/var/lib/docker/volumes portainer/agent
```



### 参考教程

1. 菜鸟教程：https://www.runoob.com/docker/docker-tutorial.html
2. Docker 入门教程：http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.htmlDocker 
3. 微服务教程：http://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html
4. 加速：Docker windows安装并启用镜像加速https://www.jianshu.com/p/b21c508514ae
5. [Docker自动启动容器](https://www.cnblogs.com/wei9593/p/11192908.html)