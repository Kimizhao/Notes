# Docker 安装 MySQL

## Docker 安装 MySQL 5.7 版本

```shell
# docker 中下载 mysql
docker pull mysql:5.7

#启动
docker run --name mysql57 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123qwe! -d mysql:5.7

#进入容器
docker exec -it mysql57 bash

#登录mysql
mysql -u root -p
```



```
# pwd
/opt
# mkdir -p docker_v/mysql/conf
# cd docker_v/mysql/conf
# touch my.cnf
# docker run -p 3306:3306 --name mysql -v /opt/docker_v/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```



## Docker 安装 MySQL 8 版本

```sh
# docker 中下载 mysql
docker pull mysql

#启动
docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql

#进入容器
docker exec -it mysql bash

#登录mysql
mysql -u root -p
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';

#添加远程登录用户
CREATE USER 'test'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
GRANT ALL PRIVILEGES ON *.* TO 'test'@'%';
```