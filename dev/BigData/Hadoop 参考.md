# Hadoop 参考

[Hadoop-之数据均衡](https://blog.csdn.net/shufangreal/article/details/112391358)

节点之间的均衡
hadoop默认提供数据均衡的shell脚本，访问路径如下

`/opt/module/hadoop-2.7.7/sbin/start-balancer.sh`

切记在集群空闲的时候进行操作，不然的话rpc跨节点网络传输很考费资源，可能造成集群任务长时间获取不到资源而运行失败。

```bash
# 对于默认的参数10，代表的是集群中各个节点的磁盘空间利用率相差不超过10%，可根据实际情况操作
start-balancer.sh

start-balancer.sh -threshold 10
```

如需停止均衡操作执行以下命令

```bash
stop-balancer.sh
```

