# CentOS 内核调优

参考服务器配置

[JT808Gateway-压力测试](https://github.com/SmallChi/JT808Gateway/blob/master/doc/README.md)

参考Centos7内核参数调优

[改内核参数支持多的连接](https://github.com/SmallChi/JT808Gateway/blob/master/doc/README.md#%E5%8F%82%E8%80%83centos7%E5%86%85%E6%A0%B8%E5%8F%82%E6%95%B0%E8%B0%83%E4%BC%98)

![](..\..\images\linux\centos-1.png)

查看连接数

```bash
lsof -i:8090 | wc -l
```

