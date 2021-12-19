## Java入门参考

[数组拷贝](http://blog.csdn.net/lazy_p/article/details/7365324/)

[Java中的instanceof关键字](https://www.cnblogs.com/rodney/archive/2005/08/18/instanceof.html)

Java HEX字符串和byte[]互相转换

https://blog.csdn.net/wxynj/article/details/81631135



[A Guide to Java Streams in Java 8: In-Depth Tutorial With Examples](https://stackify.com/streams-guide-java-8/)



## 基础

### Entity/Model/DTO 等用法区别

**Entity**

数据表对应到实体类的映射

**Model**

Model是MVC中一个概念，可能不和Entity一一对应，因为展示在View层中数据可能是一个Entity的精简，也可能是多个Entity的组合。

Model是一个高度优化组合或者精简后的一个用于在View层展示数据的对象。

**DTO**

数据传输对象（Data Transfer Object）。

用于系统间数据传输的对象，数据传输目标往往是数据访问对象从数据库中检索数据。

**Bean**

对于Bean而言，只要是Java的类的就可以称为一个Bean，

更用在Spring上，被Spring管理的对象就可以将其称作为Bean。

它不仅仅可以包括对象的属性以及get,set方法，还可以有具体的业务逻辑。

**POJO**

简单Java对象，貌似没有经常提到或作为类的后缀存在？

其特点是：除了属性和get、set方法外不包含具体的业务逻辑方法，这个和上文表述的Model很相像，和Entity区别在于没有和数据表中字段一一对应。

**EJB**

即EnterpriseBean，也就是Enterprise JavaBean（EJB）。

被称为Java企业Bean，是java的核心代码，

分别是会话Bean（Session Bean）、实体Bean（Entity Bean）、和消息驱动Bean（MessageDriven Bean）。



## 实用库

### fastjson

源码地址：https://github.com/alibaba/fastjsonFastjson 

中文 Wiki：https://github.com/alibaba/fastjson/wiki/Quick-Start-CN

最佳实践：[JSON Fastjson最佳实践](https://github.com/alibaba/fastjson/wiki/Best-Practice-for-JSON-Fastjson%EF%BC%88JSON-Fastjson%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5%EF%BC%89)



## 遇到的问题

### 日志打印太多。。SLF4J: Class path contains multiple SLF4J bindings.

SLF4J: Class path contains multiple SLF4J bindings.SLF4J: Found binding in [jar:file:/C:/Users/kimiz/.m2/repository/org/slf4j/slf4j-simple/1.7.25/slf4j-simple-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]SLF4J: Found binding in [jar:file:/C:/Users/kimiz/.m2/repository/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.jar!/org/slf4j/impl/StaticLoggerBinder.class]SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.



resource/logback.xml

<configuration>
<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">*<!-- encoders are assigned the type**ch.qos.logback.classic.encoder.PatternLayoutEncoder by default -->*<encoder><pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern></encoder></appender>
<root level="error"><appender-ref ref="STDOUT" /></root>

</configuration>