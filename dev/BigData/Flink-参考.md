### Flink-参考

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

