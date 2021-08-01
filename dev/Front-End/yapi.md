# yapi

## 使用Docker部署YAIP

### 运行MongoDB

```shell
# 创建存储卷
docker volume create mongo-data

# 启动 MongoDB
docker run -d \
  --name mongo-yapi \
  -v mongo-data:/data/db \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=yapi \
  -e MONGO_INITDB_ROOT_PASSWORD=yapi \
  -e MONGO_INITDB_DATABASE=yapi \
  mongo:4.0
```





构建YAPI镜像

Dockerfile

```shell
FROM node:12-alpine as builder
WORKDIR /yapi
RUN apk add --no-cache wget python make
ENV VERSION=1.9.3
RUN wget https://github.com/YMFE/yapi/archive/refs/tags/${VERSION}.zip
RUN unzip v${VERSION}.zip && mv yapi-${VERSION} vendors
RUN cd /yapi/vendors && cp config_example.json ../config.json && npm install --production --registry https://registry.npm.taobao.org

FROM node:12-alpine
MAINTAINER zzh@aidipper.com
ENV TZ="Asia/Shanghai"
WORKDIR /yapi/vendors
COPY --from=builder /yapi/vendors /yapi/vendors
EXPOSE 3000
ENTRYPOINT ["node"]
```



config.json

```
{
  "port": "3000",
  "adminAccount": "admin@aidipper.com",
  "timeout":120000,
  "db": {
    "servername": "192.168.10.74",
    "DATABASE": "yapi",
    "port": 27017,
    "user": "yapi",
    "pass": "yapi",
    "authSource": "admin"
  },
  "mail": {
      "enable": true,
      "host": "smtp.exmail.qq.com",
      "port": 465,
      "from": "xxx",
      "auth": {
          "user": "xxx",
          "pass": "xxx"
      }
  },
  "ldapLogin": {
      "enable": true,
      "server": "ldap://l-ldapt1.com",
      "baseDn": "CN=Admin,CN=Users,DC=test,DC=com",
      "bindPassword": "password123",
      "searchDn": "OU=UserContainer,DC=test,DC=com",
      "searchStandard": "mail",
      "emailPostfix": "@163.com",
      "emailKey": "mail",
      "usernameKey": "name"
   }
}

```



初始化 YAPI 数据库索引及管理员账号

```shell
docker run -it --rm \
  --link mongo-yapi:mongo \
  --entrypoint npm \
  --workdir /yapi/vendors \
  -v $PWD/config.json:/yapi/config.json \
  yapi \
  run install-server
```



log: mongodb load success...
初始化管理员账号成功,账号名："admin@aidipper.com"，密码："ymfe.org"



注意MongoDB版本不能太高。建议4.0版本



启动 Yapi 服务

```
docker run -d \
  --name yapi \
  --link mongo-yapi:mongo \
  --workdir /yapi/vendors \
  -p 3000:3000 \
  -v $PWD/config.json:/yapi/config.json \
  yapi \
  server/app.js
```

注意：需要先删除，初始化时的容器

```
docker rm yapi
```







参考：

1.[顶尖 API 文档管理工具 (YAPI)](https://www.jianshu.com/p/a97d2efb23c5)

2.[docker 部署使用yapi](https://blog.csdn.net/weixin_45444133/article/details/118673316)
