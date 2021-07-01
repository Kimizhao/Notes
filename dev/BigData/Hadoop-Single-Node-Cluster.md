# Hadoop-Single-Node-Cluster

## 环境准备

操作系统：Ubuntu 16.04

hadoop版本：3.2.2

JDK版本：1.8.0_281

## 下载

从[官网下载](https://apache.website-solution.net/hadoop/common/hadoop-3.2.2/)，版本为3.2.2，文件名：hadoop-3.2.2.tar.gz

```shell
# 到当前用户目录
$ cd
# 创建apache-hadoop目录
$ mkdir apache-hadoop
# 进入目录
$ cd apache-hadoop
# 下载安装包
$ curl -O https://apache.website-solution.net/hadoop/common/hadoop-3.2.2/hadoop-3.2.2.tar.gz
# 下载速度看网速，完成后解压
$ tar -zxcf hadoop-3.2.2.tar.gz
# 查看解压文件，多了个hadoop-3.2.2文件夹
$ ls
hadoop-3.2.2.tar.gz hadoop-3.2.2
# 进入hadoop-3.2.2目录
$ cd hadoop-3.2.2
```

文件包准备完成。

## 安装

### 配置

1. etc/hadoop/core-site.xml:

```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```



2. etc/hadoop/hdfs-site.xml:

```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
    	<name>dfs.namenode.name.dir</name>
    	<value>/home/hadoop/apache-hadoop/hadoop-3.2.2/data/name</value>
    	<description>为了保证元数据的安全一般配置多个不同目录</description>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/home/hadoop/apache-hadoop/hadoop-3.2.2/data/data</value>
        <description>datanode 的数据存储目录</description>
    </property>
</configuration>
```

如果没有配置namenode和datanode，每次重启都需要格式化。其中目录根据实际情况修改。



3. 编辑`etc/hadoop/hadoop-env.sh`

```bash
# set to the root of your Java installation
export JAVA_HOME=/home/hadoop/jdk/jdk1.8.0_281
```



#### 设置免密

```shell
 $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
 $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
 $ chmod 0600 ~/.ssh/authorized_keys
```



#### 运行

1.格式化

```shell
$ bin/hdfs namenode -format
```



2.启动

```shell
$ sbin/start-dfs.sh
```



### 参考

[Setting up a Single Node Cluster](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html)



2.启动

```shell
$ sbin/start-dfs.sh
Starting namenodes on [localhost]
localhost: Permission denied (publickey,password).
Starting datanodes
localhost: Permission denied (publickey,password).
Starting secondary namenodes [ubuntu-Shangqi-N720]
ubuntu-Shangqi-N720: Permission denied (publickey,password).
$
$ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
Generating public/private rsa key pair.
/home/zzh/.ssh/id_rsa already exists.
Overwrite (y/n)? yes
Your identification has been saved in /home/zzh/.ssh/id_rsa.
Your public key has been saved in /home/zzh/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:l78QS+uaNpc8SuVJtc3vkIHUCIqBxiaHsBCxe+aSW68 zzh@ubuntu-Shangqi-N720
The key's randomart image is:
+---[RSA 2048]----+
|=+ o ..   .      |
|.oo *  o . . o   |
|o  =  . .   + .  |
| .         + =   |
|. o     S B o +  |
| =       * *   + |
|o o     ..*.. o .|
| + .   .oo=. . o |
|. E..  .+=...   .|
+----[SHA256]-----+
$
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
$ chmod 0600 ~/.ssh/authorized_keys
$ sbin/start-dfs.sh
```



**Execution**

http://192.168.10.100:9870/dfshealth.html#tab-overview

![image-20210225175435022](I:\700-repos\Notes\images\hadoop-start-01.png)



**YARN on a Single Node**

http://192.168.10.100:8088/cluster

![image-20210225175517555](I:\700-repos\Notes\images\hadoop-start-02.png)

