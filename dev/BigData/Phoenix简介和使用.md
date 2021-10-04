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



修改自动类型

查看元数据信息

```SQL
SELECT TENANT_ID,TABLE_SCHEM,TABLE_NAME,COLUMN_NAME,COLUMN_FAMILY,DATA_TYPE,COLUMN_SIZE,DECIMAL_DIGITS FROM SYSTEM.CATALOG WHERE TABLE_NAME= 'GB32960_TRACK';
```

修改元数据

```sql
UPSERT INTO SYSTEM.CATALOG (TENANT_ID,TABLE_SCHEM,TABLE_NAME,COLUMN_NAME,COLUMN_FAMILY,DATA_TYPE) VALUES ('','','GB32960_TRACK','CAR_STATUS','0',5);
```





## 参考

1.[Phoenix的安装使用与SQL查询HBase](https://www.cnblogs.com/frankdeng/p/9536450.html)

2.[Apache Phoenix数据类型](https://www.cnblogs.com/sh425/p/7274249.html)

3.[IDEA 添加classPath](https://blog.csdn.net/t95921/article/details/53585225)

4.[Phoenix表数据类型修改](https://blog.csdn.net/yuanhaiwn/article/details/82141923)

5.[phoenix表结构修改（新增字段，字段类型修改）](https://blog.csdn.net/lz6363/article/details/105499504)

