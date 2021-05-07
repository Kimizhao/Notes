### HBase-实践参考

**模式设计相关**

[HBase and Schema Design](http://hbase.apache.org/book.html#schema)

Schema设计

45. [Schema Design Case Studies](http://hbase.apache.org/book.html#schema.casestudies)好文实践

37. [Rowkey Design](http://hbase.apache.org/book.html#rowkey.design)

1. [Storing Medium-sized Objects (MOB)](http://hbase.apache.org/book.html#hbase_mob)

存储二进制

http://hbase.apache.org/book.html#_architecture

71.7.6. [KeyValue](http://hbase.apache.org/book.html#keyvalue)



http://opentsdb.net/schema.html

**理解 HBase 模式设计的优秀视频: http://www.youtube.com/watch?v=_hloh_pgrlk**

A manually paginated version has lots more complexities, as you note, like having to keep track of how many things are in each page, re-shuffling if new values are inserted, etc. That seems significantly more complex. It might have some slight speed advantages (or disadvantages!) at extremely high throughput, and the only way to really know that would be to try it out. If you don’t have time to build it both ways and compare, my advice would be to start with the simplest option (one row per user+value). Start simple and iterate! :)

正如您所注意到的，手动分页的版本具有更多的复杂性，比如必须跟踪每个页面中有多少内容，如果插入了新值则需要重新排列等等。这看起来要复杂得多。它可能有一些轻微的速度优势(或缺点!)在极高的吞吐量，唯一的方法，真正知道这将是尝试。如果您没有时间以两种方式构建并进行比较，我的建议是从最简单的选项(每个用户一行 + 值)开始。开始简单的迭代！:)

阿里云：云数据库HBase

开发指南：https://help.aliyun.com/document_detail/164394.html?spm=a2c4g.11186623.6.920.3a045e1fyj0Ntf