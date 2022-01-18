# Linux实用命令

一次性Kill所有的JPS进程

```bash
jps |grep MyJavaAPP|awk '{print $1}' |xargs kill -9
```

