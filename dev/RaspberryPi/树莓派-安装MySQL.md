### 树莓派-安装MySQL

**需求**  在树莓派上安装Mysql
**环境**   Linux raspberrypi 4.9.35-v7+ #1014 SMP Fri Jun 30 14:47:43 BST 2017 armv7l GNU/Linux
**步骤**  1.安装mysql server     

```shell
$ sudo apt-get install mysql-server
```

![Image](..\..\images\raspberrypi-mysql-install-01.png)



 设置root用户密码：123456  执行指令后，一路自动安装了客户端mysql-client-5.5和服务端mysql-server-5.5等等

![Image](..\..\images\raspberrypi-mysql-install-02.png)

**操作**  

```shell
mysql -uroot -p123456
```

**设置远程访问**

$ sudo vim /etc/mysql/my.cnf
注释掉bind-address

![Image](..\..\\images\raspberrypi-mysql-install-03.png)



```
# 重启mysql
pi@raspberrypi:~ $ sudo /etc/init.d/mysql restart
[ ok ] Restarting mysql (via systemctl): mysql.service.

# 设置账号可以远程登录
pi@raspberrypi:~ $ mysql -uroot -p123456
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 39
Server version: 5.5.62-0+deb8u1 (Raspbian)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)

mysql>
```



![Image](..\..\images\raspberrypi-mysql-install-04.png)



![Image](..\..\images\raspberrypi-mysql-install-05.png)



参考：

1.https://www.cnblogs.com/apanly/p/9061803.html