### HBase-Quick-Start

官方地址：https://hbase.apache.org/

Book：http://hbase.apache.org/book.html#quickstart

2. Quick Start - Standalone HBase

   2.3. Pseudo-Distributed for Local Testing



### 环境：

hadoop-3.2.2

hbase-2.4.2

apache-zookeeper-3.7.0

CentOS-7-x86_64-DVD-2009



### 开始

#### 1.引言

快速入门将使您在 HBase 的单节点、独立实例上启动并运行。

#### 2.独立HBase

##### 2.1.安装Java环境

##### 2.2.开始使用HBase

###### **过程: 在独立模式下下载、配置和启动 HBase**

1.[下载版本](https://www.apache.org/dyn/closer.lua/hbase/)

2.解压

```
$ tar xzvf hbase-2.4.2-bin.tar.gz
$ cd hbase-2.4.2
```

3.修改环境变量

*conf/hbase-env.sh*

```
# Set environment variables here.
# The java implementation to use.
export JAVA_HOME=/home/zzh/jdk/jdk1.8.0_281/
```

4.启动HBase

```
bin/start-hbase.sh
```

HBase WebUI：http://192.168.10.100:16010/

![image-20210406183502998](..\..\images\hbase-start-01.png)



###### **过程：使用HBase**

1.连接HBase

```java
$ ./bin/hbase shell
2021-04-06 18:41:26,395 WARN  [main] util.NativeCodeLoader: Unable to load native-hadoop library for your                                                                                                                                     platform... using builtin-java classes where applicable
HBase Shell
Use "help" to get list of supported commands.
Use "exit" to quit this interactive shell.
For Reference, please visit: http://hbase.apache.org/2.0/book.html#shell
Version 2.4.2, r3e98c51c512cbd5ef779ae6bcef178ce89c46e37, Mon Mar  8 16:49:11 PST 2021
Took 0.0021 seconds
hbase:001:0>
```

2.显示HBase Shell 帮助文本

输入`help`回车

```java
hbase:002:0> help
HBase Shell, version 2.4.2, r3e98c51c512cbd5ef779ae6bcef178ce89c46e37, Mon Mar  8 16:49:11 PST 2021
Type 'help "COMMAND"', (e.g. 'help "get"' -- the quotes are necessary) for help on a specific command.
Commands are grouped. Type 'help "COMMAND_GROUP"', (e.g. 'help "general"') for help on a command group.

COMMAND GROUPS:
  Group name: general
  Commands: processlist, status, table_help, version, whoami
。。。。
```

3.创建一张表

使用`create`命令创建一张表，需要指定表名和列家族名

```java
hbase:004:0> create 'test','cf'
Created table test
Took 1.2334 seconds
=> Hbase::Table - test
hbase:005:0>
```

4.列出表的信息

使用`list`命令确认表是否存在

```java
hbase:005:0> list 'test'
TABLE
test
1 row(s)
Took 0.0293 seconds
=> ["test"]
```

使用`describe`命令查看详细，包含默认配置

```java
hbase:007:0> describe 'test'
Table test is ENABLED
test
COLUMN FAMILIES DESCRIPTION
{NAME => 'cf', BLOOMFILTER => 'ROW', IN_MEMORY => 'false', VERSIONS => '1', KEEP_DELETED_CELLS => 'FALSE',
 DATA_BLOCK_ENCODING => 'NONE', COMPRESSION => 'NONE', TTL => 'FOREVER', MIN_VERSIONS => '0', BLOCKCACHE =
> 'true', BLOCKSIZE => '65536', REPLICATION_SCOPE => '0'}

1 row(s)
Quota is disabled
Took 0.1902 seconds
```

5.往表中放入数据

使用`put`命令，往表中放入数据

```
hbase:009:0> put 'test', 'row1', 'cf:a', 'value1'
Took 0.1333 seconds
hbase:010:0> put 'test', 'row2', 'cf:b', 'value2'
Took 0.0045 seconds
hbase:011:0> put 'test', 'row3', 'cf:c', 'value3'
Took 0.0063 seconds
```

这里，我们插入三个值，一次一个。第一个 insert 位于 row1，列 cf: a，值为 value1。HBase 中的列由列族前缀 cf 组成，在本例中为 cf，后跟冒号，然后是列限定符后缀，在本例中为 a。

6.一次性扫描表中所有数据。

从 HBase 获取数据的方法之一是**扫描**。使用`scan`命令扫描表中的数据。你可以限制你的扫描，但是现在，所有的数据都被获取。

```
hbase:013:0> scan 'test'
ROW                         COLUMN+CELL
 row1                       column=cf:a, timestamp=2021-04-06T18:54:02.733, value=value1
 row2                       column=cf:b, timestamp=2021-04-06T18:54:09.737, value=value2
 row3                       column=cf:c, timestamp=2021-04-06T18:54:15.259, value=value3
3 row(s)
Took 0.1098 seconds

```

7.获取一行数据

若要一次获得单行数据，请使用 `get` 命令。

```
hbase:015:0> get 'test','row1'
COLUMN                      CELL
 cf:a                       timestamp=2021-04-06T18:54:02.733, value=value1
1 row(s)
Took 0.0131 seconds
```

8.禁用一张表

如果要删除表或更改其设置，以及在其他情况下，需要首先使用 `disable` 命令禁用该表。您可以使用 `enable` 命令重新启用它。

```
hbase:016:0> disable 'test'
Took 0.4059 seconds
hbase:017:0> enable 'test'
Took 0.6621 seconds
```

如果您测试了上面的 `enable` 命令，请再次禁用该表:

```
hbase:019:0> disable 'test'
Took 0.3307 seconds
```

9.删除一张表

要删除(删除)一个表，使用`drop`命令。

```
hbase:020:0> drop 'test'
Took 0.3790 seconds
hbase:021:0>
```

10.退出HBase Shell。

要退出 HBase Shell 并断开与集群的连接，请使用 `quit` 命令。HBase 仍然在后台运行。

```
hbase:022:0> quit
```

###### 过程：停止HBase

1.与 *bin/start-hbase.sh* 脚本方式相同，可以方便地启动所有 HBase 守护进程，即 *bin/stop-HBase.sh* 脚本停止他们。

```
$ ./bin/stop-hbase.sh
stopping hbase...........
```

在发出命令之后，进程可能需要几分钟才能关闭。使用 `jps` 确保 HMaster 和 HRegionServer 进程已关闭。



上面介绍了如何启动和停止 HBase 的独立实例。在接下来的部分中，我们将快速概述 hbase 部署的其他模式。

##### 2.3.用于本地测试的伪分布式

通过快速启动独立模式后，可以重新配置 HBase 以伪分布（pseudo-distributed）模式运行。伪分布模式意味着 HBase 仍然在单个主机上完全运行，但是每个 HBase 守护进程(HMaster、 HRegionServer 和 ZooKeeper)作为单独的进程运行: 在独立（standalone）模式下，所有守护进程在一个 jvm 进程/实例中运行。默认情况下，除非像 quickstart 中描述的那样配置 hbase.rootdir 属性，否则数据仍然存储在/tmp/中。在这个步骤中，我们将数据存储在 HDFS 中，假设您有 HDFS 可用。您可以跳过 HDFS 配置，继续在本地文件系统中存储数据。

> Hadoop 配置
>
> 此过程假定您已经在本地系统和/或远程系统上配置了 Hadoop 和 HDFS，并且它们正在运行和可用。它还假设您正在使用 Hadoop 2。在 Hadoop 文档中设置单节点集群([Setting up a Single Node Cluster](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html))的指南是一个很好的起点。

1.如果HBase正在运行，请停止它

如果您刚刚完成快速启动，HBase 仍在运行，请停止它。此过程将创建一个全新的目录，HBase 将在其中存储数据，因此您以前创建的任何数据库都将丢失。

2.配置 HBase

编辑 hbase-site.xml 配置。首先，添加以下属性，它指示 HBase 在分布式模式下运行，每个守护进程有一个 JVM 实例。

```
<property>
  <name>hbase.cluster.distributed</name>
  <value>true</value>
</property>
```

接下来，添加 hbase.rootdir 的配置，使用 HDFS:///URI 语法指向您的 HDFS 实例的地址。在本例中，HDFS 在本地主机端口8020上运行。

```
<property>
  <name>hbase.rootdir</name>
  <value>hdfs://localhost:9000/hbase</value>
</property>
```

您不需要在 HDFS 中创建目录。HBase 会为你做这件事。如果您创建目录，HBase 将尝试执行迁移，这不是您想要的。

最后，删除 hbase.tmp.dir 和 hbase.unsafe.stream.capabilityenforcement 的现有配置。

3.启动HBase

使用 *bin/start-hbase.sh*命令启动 HBase。如果系统配置正确，jps 命令应该显示正在运行的 HMaster 和 HRegionServer 进程。

```
$ bin/start-hbase.sh
The authenticity of host '127.0.0.1 (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:TmFFx8PYKml4FpY4ArlP1hGtPKcY4A9s2ASdNDrPGsI.
Are you sure you want to continue connecting (yes/no)? yes
127.0.0.1: Warning: Permanently added '127.0.0.1' (ECDSA) to the list of known hosts.
127.0.0.1: running zookeeper, logging to /home/zzh/apache-hbase/hbase-2.4.2/bin/../logs/hbase-zzh-zookeeper-ubuntu-Shangqi-N720.out
running master, logging to /home/zzh/apache-hbase/hbase-2.4.2/bin/../logs/hbase-zzh-master-ubuntu-Shangqi-N720.out
: running regionserver, logging to /home/zzh/apache-hbase/hbase-2.4.2/bin/../logs/hbase-zzh-regionserver-ubuntu-Shangqi-N720.out
$ jps
2456 HRegionServer
2298 HMaster
648 NameNode
1145 SecondaryNameNode
797 DataNode
2178 HQuorumPeer
2586 Jps
```



启动一段时间后，退出，失败日志

```
zookeeper.ClientCnxn: Socket error occurred: localhost/127.0.0.1:2181: 拒绝连接
```

说明hbase内置的zookeeper未启动，然后手动独立安装[zookkeeper](https://zookeeper.apache.org/doc/r3.7.0/zookeeperStarted.html)。然后就成功了。

![image-20210406210128748](..\..\images\hbase-start-02.png)



4.检查 HDFS 中的 HBase 目录。

如果一切正常，HBase 在 HDFS 中创建了它的目录。在上面的配置中，它存储在 HDFS 的/hbase/中。您可以使用 Hadoop 的 bin/目录中的 Hadoop fs 命令来列出这个目录。

```
$ ./bin/hadoop fs -ls /hbase
Found 12 items
drwxr-xr-x   - zzh supergroup          0 2021-04-06 20:57 /hbase/.hbck
drwxr-xr-x   - zzh supergroup          0 2021-04-07 09:02 /hbase/.tmp
drwxr-xr-x   - zzh supergroup          0 2021-04-06 20:57 /hbase/MasterData
drwxr-xr-x   - zzh supergroup          0 2021-04-07 09:02 /hbase/WALs
drwxr-xr-x   - zzh supergroup          0 2021-04-06 20:57 /hbase/archive
drwxr-xr-x   - zzh supergroup          0 2021-04-06 20:57 /hbase/corrupt
drwxr-xr-x   - zzh supergroup          0 2021-04-06 20:57 /hbase/data
-rw-r--r--   3 zzh supergroup         42 2021-04-06 20:57 /hbase/hbase.id
-rw-r--r--   3 zzh supergroup          7 2021-04-06 20:57 /hbase/hbase.version
drwxr-xr-x   - zzh supergroup          0 2021-04-06 20:57 /hbase/mobdir
drwxr-xr-x   - zzh supergroup          0 2021-04-07 09:03 /hbase/oldWALs
drwx--x--x   - zzh supergroup          0 2021-04-06 20:57 /hbase/staging
```

5.创建一个表并用数据填充它。

您可以使用 HBase Shell 创建一个表，填充数据，扫描并从中获取值，使用与 Shell 练习中相同的过程。



#### 参考：

[Ubuntu18.04 hadoop2.7.7+hbase2.0.5单机伪分布式环境搭建](http://news.migage.com/articles/Ubuntu1804+hadoop277hbase205%E5%8D%95%E6%9C%BA%E4%BC%AA%E5%88%86%E5%B8%83%E5%BC%8F%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA_1904855_csdn.html)

https://zookeeper.apache.org/doc/r3.7.0/zookeeperStarted.html



