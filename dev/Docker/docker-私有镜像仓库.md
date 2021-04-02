### docker-私有镜像仓库

#### 1.1搭建镜像仓库

首先，下载Registry镜像

```shell
docker pull registry
```

然后，运行一个Registry镜像仓库的容器实例

示例：

```
docker run -d -p 5000:5000 --name registry registry:2
```

实际部署：

```
docker run -d -v /home/zzh/yg-dokcer-images/registry:/var/lib/registry -p 50001:5000 --restart=always --name yg-registry registry
```



输入以下网页查看镜像

http://192.168.10.100:50001/v2/_catalog



#### 1.2 上传镜像到私有仓库

使用`HelloWorld`镜像进行测试

```
# 首先拉取一下
docker pull hello-world

# 标记hello-world该镜像需要推送到私有仓库
docker tag hello-world:latest 127.0.0.1:50001/hello-world:latest

# 通过push指令推送到私有仓库
docker push 127.0.0.1:50001/hello-world:latest

# 从私有仓库下载镜像
docker pull 127.0.0.1:50001/hello-world
docker pull 192.168.10.100:50001/hello-world
```



#### 配置非 https 仓库地址

如果你不想使用 127.0.0.1:50001 作为仓库地址，比如想让本网段的其他主机也能把镜像推送到私有仓库。你就得把例如 192.168.10.100:50001 这样的内网地址作为私有仓库地址，这时你会发现无法成功推送镜像。

客户端配置/etc/docker/daemon.json

```
{
  "insecure-registries": [
    "192.168.10.100:50001"
  ]
}
```



### 参考

[你必须知道的Docker镜像仓库的搭建](https://www.cnblogs.com/edisonchou/p/docker_registry_repository_setup_introduction.html)

[基于Docker搭建私有镜像仓库](https://cloud.tencent.com/developer/article/1639614)

[Docker-从入门到实践-私有仓库](https://yeasy.gitbook.io/docker_practice/repository/registry)