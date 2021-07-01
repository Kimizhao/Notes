# Phoenix简介和使用

### 安装注意

1.版本和hbase要匹配，比如Phonenix 5.1.1 搭配 HBase 2.2版本

服务端和客户端的jar也要一致。

2.客户端连服务端，如果是zookeeper的地址和端口，需要在客户机的host加192.168.10.100 hadoop001



### SQuirrel客户端

SQL命令

1.建表

```sql
create table person (id integer not null primary key,name varchar,age integer)
```



2.插入一条数据

```sql
upsert into person values (1,'zhangsan' ,18)
```



3.查询数据

```sql
select * from person
```



4.关联hbase已创建的视图表

创建视图表关联hbase;

```sql
create view "person" (
pk varchar primary key,
"info"."name" varchar,
"info"."age" varchar,
"info"."birthday" varchar,
"info"."company" varchar,
"address"."contry" varchar,
"address"."province" varchar,
"address"."city" varchar
) as select * from "person";
```



查询表

```sql
select * from "person";
```



删除视图表

```sql
drop view "person";
```



## 参考

1.[Phoenix的安装使用与SQL查询HBase](https://www.cnblogs.com/frankdeng/p/9536450.html)