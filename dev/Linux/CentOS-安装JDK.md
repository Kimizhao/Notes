### CentOS-安装JDK

#### 下载JDK

http://www.oracle.com/technetwork/java/javase/downloads/index.html



#### 解压安装JDK

```
$tar zxvf jdk-8u281-linux-x64.tar.gz

# 移动到目录
$mv -f jdk1.8.0_281 /usr/share/jdk/
```



#### **配置JDK环境变量**

修改配置文件

```
$sudo vim /etc/profile
```

文末添加

```
export JAVA_HOME=/usr/share/jdk/jdk1.8.0_281
export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar
export PATH=$PATH:${JAVA_HOME}/bin
```



`wq!`保存退出。



配置生效

```
$sudo source /etc/profile
```



#### 测试安装情况

```
$java -version
java version "1.8.0_281"
Java(TM) SE Runtime Environment (build 1.8.0_281-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.281-b09, mixed mode)
```

