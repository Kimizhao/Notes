### ZooKeeper-Replicated

[复制模式](https://zookeeper.apache.org/doc/r3.7.0/zookeeperStarted.html#sc_RunningReplicatedZooKeeper)



#### 先决条件

- 安装 Java。
- [下载](https://zookeeper.apache.org/releases.html)



#### 集群规划

| 服务器  | IP              | 主机名称 | 用户   |      |      |
| ------- | --------------- | -------- | ------ | ---- | ---- |
| hadoop1 | 192.168.100.181 | hadoop1  | hadoop |      |      |
| hadoop2 | 192.168.100.182 | hadoop2  | hadoop |      |      |
| hadoop3 | 192.168.100.183 | hadoop3  | hadoop |      |      |



安装用户：hadoop

安装目录：`/home/hadoop/apps`

数据目录：`/home/hadoop/data`

注：apps和data文件夹需要自己单独创建



在[独立模式](https://zookeeper.apache.org/doc/r3.7.0/zookeeperStarted.html#sc_InstallingSingleMode)下运行 ZooKeeper 方便了评估、开发和测试。但是在生产环境中，您应该以复制模式运行 ZooKeeper。同一应用程序中的复制服务器组称为仲裁，在复制模式下，仲裁中的所有服务器都具有相同配置文件的副本。



**注意：**对于复制模式，至少需要三台服务器，并且强烈建议您使用奇数个服务器。如果您只有两个服务器，那么您就处于这样一种情况: 如果其中一个服务器出现故障，那么就没有足够的计算机来形成多数仲裁。两台服务器的稳定性本质上不如一台服务器，因为存在两个单点故障。



下载后解压包，包放在目录`/home/hadoop/apps/`

```
$tar -zxvf apache-zookeeper-3.7.0-bin.tar.gz
```

进入目录`conf`

```
$cp zoo_sample.cfg zoo.cfg
```

复制模式所需的 `conf/zoo.cfg` 文件与独立模式中使用的文件类似，但有一些不同之处。下面是一个例子:

```
tickTime=2000
dataDir=/home/hadoop/data/
clientPort=2181
initLimit=5
syncLimit=2
server.1=hadoop1:2888:3888
server.2=hadoop2:2888:3888
server.3=hadoop3:2888:3888
```



设置服务器ID

```
$ mkdir ~/data/zookeeper
$ echo 1 > ~/data/zookeeper/myid
$ cat ~/data/zookeeper/myid
```

解释：

当服务器启动时，它通过在数据目录中查找文件 myid 来知道它是哪个服务器。这个文件包含了以 ASCII 表示的服务器号。



复制配置好的信息到另外两台机器

```
$ scp    -r   apache-zookeeper-3.7.0-bin   hadoop@hadoop2:~/apps
$ ssh  hadoop@hadoop2 "mkdir  -p ~/data/zookeeper;echo 2 >~/data/zookeeper/myid"
$ scp    -r   apache-zookeeper-3.7.0-bin   hadoop@hadoop3:~/apps
$ ssh  hadoop@hadoop3 "mkdir  -p ~/data/zookeeper ;echo 3 >~/data/zookeeper/myid"
```



启动zookeeper

分别启动三台

```
$ bin/zkServer.sh start
```



```
# 进入zookeeper控制台
zkCli.sh

# 清理hbase数据
deleteall /hbase

# 重启整个框架，hbase-site.xml配置才会生效
```





新

设置服务器ID

```
$ mkdir ~/apache-zk/data
$ echo 61 > ~/apache-zk/data/myid
$ cat ~/apache-zk/data/myid
```

