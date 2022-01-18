# Docker 安装 MongoDB

一般安装

```shell
# 拉取镜像
docker pull mongo:latest

# 运行容器
docker run -itd --name mongo -p 27017:27017 mongo
```



**带密码访问**

```shell
# 运行容器
docker run -itd --name mongo -p 27017:27017 mongo --auth
```

参数说明：

- **-p 27017:27017** ：映射容器服务的 27017 端口到宿主机的 27017 端口。外部可以直接通过 宿主机 ip:27017 访问到 mongo 的服务。
- **--auth**：需要密码才能访问容器服务。



接着使用以下命令添加用户和设置密码，并且尝试连接。

```shell
$ docker exec -it mongo mongo admin
# 创建一个名为 admin，密码为 123456 的用户。
>  db.createUser({ user:'admin',pwd:'123456',roles:[ { role:'userAdminAnyDatabase', db: 'admin'},"readWriteAnyDatabase"]});
# 尝试使用上面创建的用户信息进行连接。
> db.auth('admin', '123456')
```



**带数据挂载**

```shell
docker run -p 27017:27017 -v /data/mongo:/data/db --name mongodb -d mongo
```



**带密码、数据挂载**

```shell
docker run -p 27017:27017 -v ~/data/mongodb:/data/db --name mongodb -d mongo --auth
```

接着使用以下命令添加用户和设置密码，并且尝试连接。

```shell
$ docker exec -it mongo mongo admin
# 创建一个名为 admin，密码为 123qwe! 的用户。
>  db.createUser({ user:'admin',pwd:'123qwe!',roles:[ { role:'userAdminAnyDatabase', db: 'admin'},"readWriteAnyDatabase"]});
# 尝试使用上面创建的用户信息进行连接。
> db.auth('admin', '123qwe!')
```





客户端工具：

MongoBD Compass：https://www.mongodb.com/try/download/compass
