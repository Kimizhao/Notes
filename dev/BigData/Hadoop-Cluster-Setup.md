### Hadoop-Cluster-Setup

https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/ClusterSetup.html

### 集群设置

#### 目的



本文档描述了如何安装和配置 Hadoop 集群，包括从几个节点到拥有数千个节点的超大型集群。要使用 Hadoop，您可能首先希望将其安装在单台机器上(请参阅[单节点设置](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html))。

这个文档不包括高级主题，如[安全](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SecureMode.html)或高可用性。

#### 先决条件

- 安装 Java。请参阅[Hadoop Wiki](https://cwiki.apache.org/confluence/display/HADOOP/Hadoop+Java+Versions)已知的好版本
- 从Apache镜像下载一个稳定版本的Hadoop



#### 集群规划

| 服务器  | IP              | 主机名称 | 用户   | HDFS                       | YARN                        |
| ------- | --------------- | -------- | ------ | -------------------------- | --------------------------- |
| hadoop1 | 192.168.100.181 | hadoop1  | hadoop | NameNode,DataNode          | NodeManager                 |
| hadoop2 | 192.168.100.182 | hadoop2  | hadoop | DataNode                   | NodeManager                 |
| hadoop3 | 192.168.100.183 | hadoop3  | hadoop | DataNode,SecondaryNameNode | NodeManager                 |
| hadoop4 | 192.168.100.184 | hadoop4  | hadoop | DataNode                   | ResourceManager,NodeManager |

#### 环境准备

1. 软件版本

hadoop-3.2.2

[CentOS-7-x86_64](http://mirrors.aliyun.com/centos/7/isos/x86_64/)

1. host和主机名配置

host配置

```shell
$vim /etc/hosts
```

```
192.168.100.181 hadoop1
192.168.100.182 hadoop2
192.168.100.183 hadoop3
192.168.100.184 hadoop4
```

主机名

```shell
$hostnamectl set-hostname hadoop1
```

依次修改其他服务器。

3. JDK安装

版本：jdk1.8.0_281



#### 免密登陆

检查是否可以免密登陆

```shell
$ssh localhost
```

如果不行，运行一下命令

```shell
  $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
  $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  $ chmod 0600 ~/.ssh/authorized_keys
```

每台相互间免密，在每台机器上执行这个命令：

```shell
$ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop1
$ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop2
$ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop3
$ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop4
```

4台都执行，会自动把密钥加载到 authorized_keys 文件中

关闭四台服务器的防火墙和~~SELINUX~~

查看防火墙状态

```shell
$systemctl status firewalld.service
```

关闭运行的防火墙

```shell
$systemctl stop firewalld.service
```

禁止防火墙服务器（永久关闭）

```shell
$systemctl disable firewalld.service
```



#### 时间同步

安装ntp

```shell
$yum install ntp
```

配置阿里的ntp服务器

```shell
$ntpdate -u ntp1.aliyun.com
```

验证是否成功

```
$date
```

```
[root@hadoop1 ~]# date
2021年 04月 09日 星期五 15:27:24 CST
[root@hadoop1 ~]# ntpdate -u ntp1.aliyun.com
 9 Apr 17:33:52 ntpdate[4286]: step time server 120.25.115.20 offset 7574.673324 sec
[root@hadoop1 ~]# date
2021年 04月 09日 星期五 17:33:54 CST
```



#### Hadoop安装

##### 1.规划

安装用户：hadoop

安装目录：/home/hadoop/apps

数据目录：/home/hadoop/data

注：apps和data文件夹需要自己单独创建

##### 2.解压包

```shell
[hadoop@hadoop1 apps]$ ls
hadoop-3.2.2  hadoop-3.2.2.tar.gz
[hadoop@hadoop1 apps]$ tar -zxvf hadoop-3.2.2.tar.gz
```

##### 3.修改配置文件

配置文件目录：/home/hadoop/apps/hadoop-3.2.2/etc/hadoop

```
[hadoop@hadoop1 hadoop]$ ls
capacity-scheduler.xml      hadoop-user-functions.sh.example  kms-log4j.properties        ssl-client.xml.example
configuration.xsl           hdfs-site.xml                     kms-site.xml                ssl-server.xml.example
container-executor.cfg      httpfs-env.sh                     log4j.properties            user_ec_policies.xml.template
core-site.xml               httpfs-log4j.properties           mapred-env.cmd              workers
hadoop-env.cmd              httpfs-signature.secret           mapred-env.sh               yarn-env.cmd
hadoop-env.sh               httpfs-site.xml                   mapred-queues.xml.template  yarn-env.sh
hadoop-metrics2.properties  kms-acls.xml                      mapred-site.xml             yarnservice-log4j.properties
hadoop-policy.xml           kms-env.sh                        shellprofile.d              yarn-site.xml
```

###### 3.1 hadoop-env.sh

```
[hadoop@hadoop1 hadoop]$ vim hadoop-env.sh 
```

修改JAVA_HOME

```
export JAVA_HOME=/usr/share/jdk/jdk1.8.0_281
```

###### 3.2 core-site.xml

```shell
[hadoop@hadoop1 hadoop]$ vim core-site.xml 
```

fs.defaultFS ： 这个属性用来指定namenode的hdfs协议的文件系统通信地址，可以指定一个主机+端口，也可以指定为一个namenode服务（这个服务内部可以有多台namenode实现ha的namenode服务

hadoop.tmp.dir : hadoop集群在工作的时候存储的一些临时文件的目录。

```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://hadoop1:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/home/hadoop/data/hadoopdata</value>
    </property>
</configuration>
```

###### 3.3 hdfs-site.xml

```shell
[hadoop@hadoop1 hadoop]$ vim hdfs-site.xml
```

 dfs.namenode.name.dir：namenode数据的存放地点。也就是namenode元数据存放的地方，记录了hdfs系统中文件的元数据。

 dfs.datanode.data.dir： datanode数据的存放地点。也就是block块存放的目录了。

dfs.replication：hdfs的副本数设置。也就是上传一个文件，其分割为block块后，每个block的冗余副本个数，默认配置是3。

dfs.secondary.http.address：secondarynamenode 运行节点的信息，和 namenode 不同节点

```
<configuration>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/home/hadoop/data/hadoopdata/name</value>
        <description>为了保证元数据的安全一般配置多个不同目录</description>
    </property>

    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/home/hadoop/data/hadoopdata/data</value>
        <description>datanode 的数据存储目录</description>
    </property>

    <property>
        <name>dfs.replication</name>
        <value>2</value>
        <description>HDFS 的数据块的副本存储个数, 默认是3</description>
    </property>

    <property>
        <name>dfs.secondary.http.address</name>
        <value>hadoop3:50090</value>
        <description>secondarynamenode 运行节点的信息，和 namenode 不同节点</description>
    </property>
</configuration>
```

###### 3.4 mapred-site.xml

```shell
[hadoop@hadoop1 hadoop]$ vim mapred-site.xml
```

mapreduce.framework.name：指定mr框架为yarn方式,Hadoop二代MP也基于资源管理系统Yarn来运行 。

```
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

###### 3.5 yarn-site.xml

```shell
[hadoop@hadoop1 hadoop]$ vim yarn-site.xml
```

 yarn.resourcemanager.hostname：yarn总管理器的IPC通讯地址

 yarn.nodemanager.aux-services：

```shell
<configuration>

<!-- Site specific YARN configuration properties -->

    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>hadoop4</value>
    </property>

    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
        <description>YARN 集群为 MapReduce 程序提供的 shuffle 服务</description>
    </property>
    <property>
        <name>yarn.application.classpath</name>
        <value>/home/hadoop/apps/hadoop-3.2.2/etc/hadoop:/home/hadoop/apps/hadoop-3.2.2/share/hadoop/common/lib/*:/home/hadoop/apps/hadoop-3.2.2/share/hadoop/common/*:/home/hadoop/apps/hadoop-3.2.2/share/hadoop/hdfs:/home/hadoop/apps/hadoop-3.2.2/share/hadoop/hdfs/lib/*:/home/hadoop/apps/hadoop-3.2.2/share/hadoop/hdfs/*:/home/hadoop/apps/hadoop-3.2.2/share/hadoop/mapreduce/lib/*:/home/hadoop/apps/hadoop-3.2.2/share/hadoop/mapreduce/*:/home/hadoop/apps/hadoop-3.2.2/share/hadoop/yarn:/home/hadoop/apps/hadoop-3.2.2/share/hadoop/yarn/lib/*:/home/hadoop/apps/hadoop-3.2.2/share/hadoop/yarn/*</value>
        <description>输入刚才返回的Hadoop classpath路径</description>
    </property>
</configuration>
```

###### 3.6 workers

3.0开始，slaves更名为workers

```shell
[hadoop@hadoop1 hadoop]$ vim workers 
```

添加

```
hadoop1
hadoop2
hadoop3
hadoop4
```

##### 4.包分发到其他节点

可通过scp命令分发，比如：

```
[hadoop@hadoop1 hadoop]$ scp -r ~/apps/hadoop-3.2.2/ hadoop2:~/apps/
[hadoop@hadoop1 hadoop]$ scp -r ~/apps/hadoop-3.2.2/ hadoop3:~/apps/
[hadoop@hadoop1 hadoop]$ scp -r ~/apps/hadoop-3.2.2/ hadoop4:~/apps/
```

##### 5.配置Hadoop环境变量

使用普通用户进行安装。 vi ~/.bashrc 用户变量

```
[hadoop@hadoop1 ~]$ vim .bashrc
```

```
export HADOOP_HOME=/home/hadoop/apps/hadoop-3.2.2
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:
```

使环境变量生效

```
[hadoop@hadoop1 bin]$ source ~/.bashrc 
```

##### 6.查看hadoop版本

```
[hadoop@hadoop1 hadoop]$ hadoop version
Hadoop 3.2.2
Source code repository Unknown -r 7a3bc90b05f257c8ace2f76d74264906f0f7a932
Compiled by hexiaoqiao on 2021-01-03T09:26Z
Compiled with protoc 2.5.0
From source with checksum 5a8f564f46624254b27f6a33126ff4
This command was run using /home/hadoop/apps/hadoop-3.2.2/share/hadoop/common/hadoop-common-3.2.2.jar
```

##### 7.Hadoop初始化

注意：HDFS初始化只能在主节点上进行

```
[hadoop@hadoop1 ~]$ hadoop namenode -format
```

##### 8.启动

###### 8.1 启动HDFS

注意：不管在集群中的那个节点都可以

```
[hadoop@hadoop1 ~]$ start-dfs.sh
```

###### 8.2 启动YARN

注意：只能在主节点中进行启动

```
[hadoop@hadoop4 ~]$ start-yarn.sh
```

##### 9.查看4台服务器的进程

jps查看。



##### 10. 网页界面

一旦 Hadoop 集群启动并运行，请检查组件的 web-ui，如下所述:

| 守护进程                                         | 网页界面                     | 注释                  |
| ------------------------------------------------ | ---------------------------- | --------------------- |
| NameNode                                         | http://192.168.100.181:9870/ | 默认 HTTP 端口是9870  |
| ResourceManager                                  | http://192.168.100.184:8088/ | 默认 HTTP 端口是8088  |
| MapReduce JobHistory Server MapReduce JobHistory |                              | 默认 HTTP 端口是19888 |



#### Hadoop简单使用

##### 创建文件夹

```
[hadoop@hadoop1 ~]$ hadoop fs -mkdir -p /test/input
```

##### 查看创建的文件夹

```
[hadoop@hadoop1 ~]$ hadoop fs -ls /test
```

##### 上传文件

创建一个文件words.txt

```
[hadoop@hadoop1 ~]$ vi words.txt
hello zhangsan
hello lisi
hello wangwu
```

```
[hadoop@hadoop1 ~]$ hadoop fs -put ~/words.txt /test/input
```

##### 下载文件

```
[hadoop@hadoop1 ~]$ hadoop fs -get /test/input/words.txt ~/data
```

##### 运行一个mapreduce的例子程序： wordcount

```
$hadoop jar /home/hadoop/apps/hadoop-3.2.2/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.2.jar wordcount /test/input /test/output
```

查看结果：

```
[hadoop@hadoop1 ~]$ hadoop fs -cat /test/output/part-r-00000
hello   3
lisi    1
wangwu  1
zhangsan        1
```



#### 错误1：

运行mapreduce子程序后，错误: [找不到或无法加载主类 org.apache.hadoop.mapreduce.v2.app.MRAppMaster](https://blog.csdn.net/qq_41684957/article/details/81710190)



#### 错误2：

resourcemanager未启动

在hadoop4运行:$ yarn --daemon start resourcemanager

http://192.168.100.184:8088/cluster/cluster



#### 参考：

1.[Hadoop学习之路（四）Hadoop集群搭建和简单应用](https://www.cnblogs.com/qingyunzong/p/8496127.html#_labelTop)

2.[超详细的Hadoop集群部署](https://blog.csdn.net/Alex_81D/article/details/102964892)

3.[hadoop分布式集群搭建](http://www.ityouknow.com/hadoop/2017/07/24/hadoop-cluster-setup.html)

4.[Hadoop-3.2.2(HA)完全分布式部署](https://www.jianshu.com/p/7fb3839d3ca5)

5.Cloudera（英语：Cloudera, Inc.）是一家位于美国的软件公司，向企业客户提供基于Apache Hadoop的软件、支持、服务以及培训。