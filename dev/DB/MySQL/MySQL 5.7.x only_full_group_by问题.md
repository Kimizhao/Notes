# MySQL 5.7.x only_full_group_by问题

错误日志：

this is incompatible with sql_mode=only_full_group_by



描述：select的列都要在group中，或许本身是聚合列（SUM,AVG,MAX,MIN）才行

查看mysql版本命令：select version();

查看sql_model参数命令：

SELECT @@GLOBAL.sql_mode;

SELECT @@SESSION.sql_mode;

发现：
ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
**第一项默认开启ONLY_FULL_GROUP_BY,**



解决方案1

1.只选择出现在group by后面的列，或者给列增加聚合函数；

2.命令行输入：

```
set sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
 
如果上面设置完成后依然报错，这里执行下面的SQL，将全局设置更改，然后必须关闭现有的连接，重新打开连接后查询即可
 
set global sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
```



3.永久关闭

打开数据配置文件，在[mysqld]节下新增配置:

sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION

保存后重启mysql服务。
