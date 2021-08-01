# Flink 笔记

## Flink-理论

## 041.Flink理论 Window API 基本概念

### 窗口

- 一般真是的流都是无界的，怎样处理无界的数据
- 可以把无限的数据流进行切分，得到有限的数据集进行处理--也就是得到有界流
- 窗口（window）就是**将无限流切割为有限流的一种方式**，它会将流数据分发到有限大小的桶(bucket)中进行分析

### window类型

- 时间窗口(Time Window)
  - 滚动时间窗口
  - 滑动时间窗口
  - 会话窗口
- 计数窗口(Count Window)
  - 滚动计数窗口
  - 滑动计数窗口



滚动窗口(Tumbling Windows)

- 将数据依据固定的窗口长度对数据进行切分

- 时间对齐，窗口长度固定，没有重叠

头连尾，尾连头，数据只属于一个，左闭右开。



滑动窗口(Sliding Windows)

- 滑动窗口是**固定窗口**的更广义的一种形式，滑动窗口由固定的窗口长度和滑动间隔组成
- 窗口长度固定，可以有重叠



会话窗口(Session Windows)

- 由一系列事件组合一个指定时间长度的timeout间隙组成，也就是一段时间没有接收到新数据就会生成新的窗口
- 特点：时间无对齐



.timeWindow

.countWindow

```
        dataStream.keyBy(data -> data.getId())
                //.timeWindow(Time.seconds(15))
                //.window(TumblingProcessingTimeWindows.of(Time.seconds(15)))//滚动时间窗口
//        .window(SlidingEventTimeWindows.of(Time.seconds(15), Time.seconds(15)))//滑动时间窗口
                //.window(EventTimeSessionWindows.withGap(Time.minutes(10)))
        //.countWindow(5)//滚动计数
```



### 窗口函数(window function)

定义了要对窗口中收集的数据做的计算操作

可以分为两类：

增量聚合函数（incremental aggregation functions）

- 每条数据到来就进行计算，保持一个简单的状态
- ReduceFunction、AggregateFunction



全窗口函数（full window functions）

- 先把窗口所有数据收集起来，等到计算的时候会遍历所有数据
- ProcessWindowFunction，WindowFunction

更加灵活，适用场景多。



其他可选API。

.trigger

.evictor

.allowedLateness

.sideOutputLateData 侧输出流

![image-20210629235936360](J:\700-repo\github\Kimizhao\Notes\dev\BigData\flink\image-20210629235936360.png)







## Flink-运维



### 系统配置

Apache Flink 提供了很多用来配置其行为以及调节性能的参数。所有这些参数都可以在./conf/flink-conf.yaml文件中定义。



#### Java和类加载

Flink默认会使用PATH环境变量中的Java执行文件来启动JVM进程。如果PATH中们没有配置Java或需要使用其他Java版本，你可以利用配置文件中的JAVA_HOME环境变量或者env.java.home键对应的参数来制定Java安装目录。



#### CPU

Flink不会主动限制自身消耗的CPU资源量，但会采用处理槽来控制可以分配给工作进程（TaskManager）的任务数。每个TaskManager都能提供一定数量的处理槽，这些处理槽由ResourceManager统一注册和管理。JobManager需要请求一个或多个处理槽来执行应用。每个处理槽可以i处理应用的一个“切片”，即应用程序每个算子的一个并行任务。因此，JobManager至少需要获取和应用算子最大并行度等量的处理槽。

TaskManager所提供的处理槽数量是由配置文件中的taskmanager.numberOfTaskSlots项来指定的。默认是每个TaskManager一个处理槽。



#### 内存和网络缓冲

Flink的主进程和工作进程对内存有着不同需求。主进程主要管理计算资源（ResourceManager）和协调应用（JobManager）执行；而工作进程要负责繁重的工作以及处理潜在规模庞大的数据。

主进程对内存的要求并不苛刻，默认JVM堆内存数量只有1GB。通过jobmanager.heap.size配置

工作进程内存要复杂一些，最重要参数是JVM堆内存大小，taskmanager.heap.size配置

除了JVM，还有两个组件同样会消耗很多内存，Flink的网络栈和RocksDB（如果选它作为状态后端）。

网络配置项是taskmanager.network.memory.fraction，它的值决定了JVM为网络缓冲区分配的内存比例，默认配置是使用JVM堆内存的10%。

taskmanager.memory.segment-size，决定了单个网络缓冲区的大小，默认值32KB

taskmanager.memory.min,网络缓冲区的最小，默认64MB

taskmanager.memory.max，网络缓冲区的最大，默认1GB

RocksDB没直观的计算方法，自行尝试。



#### 磁盘存储

出于多种原因，Flink的工作进程需要在本地文件系统上存储数据，其中包括接收应用的JAR包、写日志、以及在配置了RokcsDB状态后端是维护状态。离用io.tmp.dirs配置项，你可以指定一个或多个目录（英文冒号分割）用以在本地文件系统上存储数据。默认情况下，数据会写入默认临时目录（由Java系统变量java.io.tmpdir决定，或Linux及MacOS下的/tmp目录）。io.tmp.dirs参数路径会默认用于Flink大多数有本地存储需求的组件。但这些组件各自的路径也可以分别设置。



配置Blob服务器的本地存储目录

```
blob.storage.directory
```

TaskManager的日志文件目录，默认是安装位置的./log目录

```
env.log.dir
```



RocksDB状态后端会将应用状态维护在本地文件系统中。其维护目录可以利用

state.backend.rocksdb.localdir参数来设置。如果没有显式指定该配置参数，RocksDB会使用io.tmp.dirs的值。



#### 检查点和状态后端

Flink针对状态后端如何将状态写入检查点提供了很多配置项。所有参数都可以在应用代码中显示指定。但你也可以通过Flink配置文件为整个Flink集群提供默认配置，它们会在作业未显示声明时启动作用。

参数定义集群的默认状态后端

```
state.backend
```

异步检查点

```
state.backend.async
```

增量检查点

```
state.backend.incremental
```

如果状态后端不支持某些选项，则可能会自动忽略它们。

写入检查点

```
state.checkpoints.dir
```

保存点的远程存储路径

```
state.savepoints.dir
```



某些状态后端特有的。

RocksDB状态后端

本地文件存储路径

```
state.backend.rocksdb.localdir
```

计时器状态存放在堆中（默认）还是RocksDB中

```
state.backend.rocksdb.timer-service.factory
```



Flink集群默认启用和配置本地恢复

```
state.backend.local-recovery=true
```

本地状态副本的存储位置

```
taskmanager.state.local.root-dirs
```



#### 安全性

数据处理框架属于公司IT基础架构中的敏感组件，我们需要对其采取某些保护措施，以防出现未经授权的使用或数据访问。Apache Flink支持Kerberos身份验证，也可以利用SSL对网络通信进行加密。





### 保存点

保存点和检查点的本质相同，二者都是应用状态的一致性完整快照。但他们的生命周期有所差异。检查点会自动创建，在发生故障时自动加载并由Flink自动删除（取决于应用具体配置）。此外i，除非应用显示制定要保留检查点，否则它们会在应用取消时自动被删除。二保持点则与之相反，它们需要由用户或外部服务手动触发，且永远不会被Flink自动删除。

每个保持点都对应一个持久化数据存储上的目录。它由一个包含了所有任务状态数据文件的子目录和一个包含了全部数据文件绝对路径的二进制元数据文件组成。由于元数据文件中存储的是绝对路径，所以将保存点移动到其他路径会使其失效。下面展示了一个保存点的目录结构：

```
# 保存点根路径
/savepoints/

# 某一具体保存点的路径
/savepoints/savepoint-:shortjobid-:savepointid/

# 某一保存点的二进制元数据文件
/savepoints/savepoint-:shortjobid-:savepointid/_metadata

# 存储的算子状态
/savepoints/savepoint-:shortjobid-:savepointid/:xxx

```



### 通过命令行客户端管理应用

Flink命令行客户端提供了启动、停止、和管理Flink应用的功能。它会从./conf/flink-conf.yaml读取配置。你可以在Flink安装根目录下通过命令./bin/flink/来调用。

**启动应用**

```shell
# 不带参数
./bin/flink run ~/myApp.jar

# 带参数
./bin/flink run ~/myApp.jar my-arg1 my-arg2 my-arg3

# 分离模块提交应用
./bin/fink run -d ~/myApp.jar

# 指定并行度，代码中指定优先级更高
./bin/flink run -p 16 ~/myApp.jar

# 指定入口类
./bin/flink run -c my.app.MainClass ~/myApp.jar

# 提交到特定主进程，默认情况客户端会将应用提交到./conf/flink-conf.yaml文件中所指定的Flink主进程
./bin/flink run -m myMasterHost:9876 ~/myApp.jar
```

上述命令会从JAR包中META-INF/MANIFEST.MF文件内的program-class属性所指示的main()方法启动应用，JAR包提交到主进程，然后主进程分发到工作节点。

提交后，会返回并打印提交作业的JobID。该JobID可以生成保存点、取消应用等



**列出正在运行的应用**

```shell
./bin/flink list -r
```



**生成和清除保存点**

```shell
# 生成
./bin/flink savepoint <jobId> [savepointPath]

# 删除
./bin/flink savepoint -d <savepointPath>
```

如果你显示指定了保存点路径，它就会存到你所提供的目录中。否则，Flink会使用flink-conf.yaml文件中配置的默认保存点路径。



**取消应用**

```shell
# 不使用保存点
./bin/flink cancel <jobId>

# 使用保存点
./bin/flink cancel -s [savepointPath] <jobId>
```

如果保存点生产失败，作业将继续运行。你需要再次尝试取消。



**从保存点启动应用**

```shell
./bin/flink run -s <savepointPath> [options] <jobJar> [arguments]
```

作业启动后，Flink会将保存点内的各个状态快照和应用所有状态进行匹配。具体匹配过程分为两步。首先，Flink会对保存点中的唯一算子标识和应用算子内的进行笔记。其次，它会针对每个算子比较保存点和应用内的状态标识符。



**！！最好为每个算子定义唯一ID**

如果你没有使用uid()方法为算子分配唯一ID，则Flink会根据算子类型和它所有前置算子计算一个哈希值作为默认标识。保存点中的标识是无法修改的，因此你如果没有使用uid()手动分配算子标识，那么在更新或改进应用时就会受到一些限制。



**应用扩缩容**

减少或增加应用的并行度并不困难。你只需要生成一个保存点，取消应用，然后再将并行度调整后的应用从保存点启动起来即可。应用的状态会自动重新分配到更多或更少的并行算子任务上。



## Flink-参考

源码：https://github.com/apache/flink

编译：

```shell
git clone https://github.com/apache/flink.git
cd flink
mvn clean package -DskipTests # this will take up to 10 minutes
```



[Flink官方](https://ci.apache.org/projects/flink/flink-docs-release-1.12/zh/)

教程

https://github.com/zhisheng17/flink-learning

[Flink 从 0 到 1 学习](https://www.cnblogs.com/huanghanyu/category/1758541.html?page=1)



书籍代码：基于Apache Flink的流处理

https://github.com/streaming-with-flink



flink中国社区

公开课

https://github.com/flink-china/flink-training-course#s3-%E7%A4%BE%E5%8C%BA%E5%85%AC%E5%BC%80%E8%AF%BE%E8%AF%BE%E8%A1%A8%E8%BF%9B%E8%A1%8C%E4%B8%AD

https://github.com/flink-china



尚硅谷-flink课程

网友整理的课程笔记：https://www.yuque.com/yingwenerjie/yir85b/phgxuw



Demo:

https://github.com/Kimizhao/ss-java-start





### Flink windows下用maven创建flink项目

参考：[1](https://blog.csdn.net/walykyy/article/details/105938565)

1、java版本：**记住一定要用cmd执行**

```shell
mvn archetype:generate -DarchetypeGroupId=org.apache.flink -DarchetypeArtifactId=flink-quickstart-java -DarchetypeVersion=1.13.1 -DgroupId=org.apache.flink.quickstart -DartifactId=flink-java-project -Dversion=0.1 -Dpackage=org.apache.flink.quickstart -DinteractiveMode=false
```



2、scala版本：**记住一定要用cmd执行**

```shell
mvn archetype:generate -DarchetypeGroupId=org.apache.flink -DarchetypeArtifactId=flink-quickstart-scala -DarchetypeVersion=1.13.1 -DgroupId=org.apache.flink.quickstart -DartifactId=flink-scala-project -Dversion=0.1 -Dpackage=org.apache.flink.quickstart -DinteractiveMode=false
```



### 部署各种错误

##### Caused by: org.apache.flink.runtime.jobmanager.scheduler.NoResourceAvailableException: Could not acquire the minimum required resources

看集群中有几个job

设置

env.setParallelism(1);

或者调整

taskmanager.numberOfTaskSlots

##### Caused by: java.lang.ClassCastException: cannot assign instance of org.apache.commons.collections.map.LinkedMap to field org.apache.flink.streaming.connectors.kafka.FlinkKafkaConsumerBase.pendingOffsetsToCommit of type org.apache.commons.collections.map.LinkedMap in instance of org.apache.flink.streaming.connectors.kafka.FlinkKafkaConsumer

模块依赖复杂导致冲突。

查看flink lib下和打包依赖

