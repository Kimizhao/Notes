### MQ-kafka

[Kafaka](http://kafka.apache.org/) A distributed streaming platform.

- [QuickStart](http://kafka.apache.org/quickstart)
- [Getting Started](http://kafka.apache.org/documentation/#gettingStarted)



以下官方翻译而来。

## 快速启动

### 第一步：找到Kafka

下载 [Kafka](https://www.apache.org/dyn/closer.cgi?path=/kafka/2.7.0/kafka_2.13-2.7.0.tgz) 的最新版本并解压缩它:

```shell
$ tar -xzf kafka_2.13-2.7.0.tgz
$ cd kafka_2.13-2.7.0
```

### 第二步：开始Kafka环境

*注意: 您的本地环境必须安装了 java8 + 。*

运行以下命令以正确的顺序启动所有服务:

```
# Start the ZooKeeper service
# Note: Soon, ZooKeeper will no longer be required by Apache Kafka.
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```

打开另一个终端会话并运行:

```
# Start the Kafka broker service
$ bin/kafka-server-start.sh config/server.properties
```

一旦所有服务都成功启动，您将有一个基本的 Kafka 环境运行和准备使用。

### 第三步: 创建一个主题来存储你的事件

Kafka 是一个分布式事件流平台，允许您在多台机器上读取、写入、存储和处理[事件](http://kafka.apache.org/documentation/#messages)(在文档中也称为记录或消息)。

比如支付交易、手机地理定位更新、发货订单、物联网设备或医疗设备的传感器测量等等。这些活动被组织并存储在[主题](http://kafka.apache.org/documentation/#intro_concepts_and_terms)中。非常简单，主题类似于文件系统中的文件夹，事件是该文件夹中的文件。

所以在你写第一个事件之前，你必须创建一个主题，打开另一个终端会话并运行:

```
$ bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
```

所有 Kafka 的命令行工具都有额外的选项: 运行 `Kafka-topics.sh` 命令没有任何参数来显示使用信息。例如，它还可以显示[诸如新主题的分区计数等详细信息](http://kafka.apache.org/documentation/#intro_concepts_and_terms):

```
$ bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
Topic:quickstart-events  PartitionCount:1    ReplicationFactor:1 Configs:
    Topic: quickstart-events Partition: 0    Leader: 0   Replicas: 0 Isr: 0
```

### 第四步: 在主题中写入一些事件

卡夫卡的客户通过网络与卡夫卡的经纪人交流写作(或阅读)事件。一旦接收到，代理将以持久和容错的方式存储事件，直到您需要的时间——甚至是永久的时间。

运行控制台生产者客户机将一些事件写入您的主题。默认情况下，您输入的每一行都将导致向主题写入一个单独的事件。

```
$ bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
This is my first event
This is my second event
```

您可以随时使用 `Ctrl-C` 停止生产者客户机。

### 第五步: 阅读事件

打开另一个终端会话并运行控制台客户端来读取您刚刚创建的事件:

```
$ bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
This is my first event
This is my second event
```

您可以在任何时候使用 `Ctrl-C` 停止客户机。

您可以自由地进行实验: 例如，切换回生产者终端(前一步)以编写附加事件，并查看事件如何立即显示在消费者终端中。

因为事件可以永久地储存在卡夫卡中，所以它们可以被任意多次阅读，任意多的消费者都可以阅读。您可以通过打开另一个终端会话并重新运行上一个命令来轻松地验证这一点。

### 步骤6: 使用 KAFKA CONNECT 将数据导入/导出为事件流

您可能在现有系统(如关系数据库或传统消息传递系统)中有大量数据，还有许多已经使用这些系统的应用程序。[Kafka Connect](http://kafka.apache.org/documentation/#connect) 允许您不断地从外部系统摄取数据到 Kafka，反之亦然。因此，将现有系统与 Kafka 集成起来是非常容易的。为了使这个过程更加容易，有数百个这样的连接器可以随时使用。

查看 [Kafka Connect](http://kafka.apache.org/documentation/#connect) 部分，了解更多关于如何持续导入/导出 Kafka 中的数据。

### 第七步: 用 KAFKA STREAMS 处理你的事件

一旦你的数据以事件的形式存储在 Kafka 中，你可以使用 Java/Scala 的 Kafka Streams 客户端库来处理这些数据。它允许您实现任务关键的实时应用程序和微服务，其中输入和/或输出数据存储在 Kafka 主题中。Kafka Streams 将在客户端编写和部署标准 Java 和 Scala 应用程序的简单性与 Kafka 的服务器端集群技术的好处结合起来，使这些应用程序具有高度的可伸缩性、弹性、容错性和分布式。该库支持一次性处理、有状态操作和聚合、窗口化、连接、基于事件时间的处理等等。

为了给你一个初步的体验，我们来看看如何实现流行的 `WordCount` 算法:

```java
KStream<String, String> textLines = builder.stream("quickstart-events");

KTable<String, Long> wordCounts = textLines
            .flatMapValues(line -> Arrays.asList(line.toLowerCase().split(" ")))
            .groupBy((keyIgnored, word) -> word)
            .count();

wordCounts.toStream().to("output-topic", Produced.with(Serdes.String(), Serdes.Long()));
```

演示和应用程序开发教程展示了如何从开始到结束编码和运行这样一个流式应用程序。

### 第八步: 终止 KAFKA 环境

现在你已经到达了快速开始的终点，可以随心所欲地打破 Kafka 的环境，或者继续玩下去。

1. 停止生产者和消费者的客户端`Ctrl-C`, 如果你还没有这样做.
2. 停止Kafka的经纪人与`Ctrl-C`.
3. 最后，停止 ZooKeeper 服务器`Ctrl-C`.

如果你还想删除本地 Kafka 环境中的任何数据，包括你一路上创建的任何事件，请运行以下命令:

```
$ rm -rf /tmp/kafka-logs /tmp/zookeeper
```

### 祝贺你

你已经成功地完成了 Apache Kafka 的快速启动。

为了了解更多，我们建议以下步骤:

- 通读简介[引言](http://kafka.apache.org/intro)了解 Kafka 如何在高层次上工作，它的主要概念，以及它与其他技术的比较。要更详细地了解卡夫卡，请访问[文件](http://kafka.apache.org/documentation/).

- 浏览[用例](http://kafka.apache.org/powered-by) .了解我们全球社区的其他用户如何从 Kafka 中获得价值

-  加入一个[当地 Kafka 聚会小组](http://kafka.apache.org/events) 及[观看来自 Kafka 峰会的演讲](https://kafka-summit.org/past-events/), 卡夫卡社区的主要会议.