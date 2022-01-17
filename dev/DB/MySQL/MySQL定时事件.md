# MySQL 定时事件

## 配置

**1.查看事件是否开启**

```sql
SHOW VARIABLES LIKE  'event_scheduler';
```



**2.设置当前事件开启**

```sql
#重启服务后失效
SET GLOBAL event_scheduler = 1;
```



**3.设置事件在mysql启动时自动开启方法**

在`/etc/mysql/my.cnf`中添加`event_scheduler=ON`

或者

在`/etc/mysql/mysql.conf.d/mysqld.cnf`中添加`event_scheduler=ON`

```sh
# echo "event_scheduler=ON" >> my.cnf
```



## 创建事件

**每天凌晨1点执行**

```sql
CREATE EVENT IF NOT EXISTS temp_event   
    ON SCHEDULE EVERY 1 DAY STARTS DATE_ADD(DATE_ADD(CURDATE(), INTERVAL 1 DAY), INTERVAL 1 HOUR)   
    ON COMPLETION PRESERVE ENABLE
    DO update users set support=0 where support=1;
```

**每隔一分钟执行**

```sql
CREATE event IF NOT EXISTS temp_event ON SCHEDULE EVERY 1 MINUTE   
ON COMPLETION PRESERVE   
DO update users set support=0 where support=1;
```

**查看定时任务**

```sql
 SELECT * FROM information_schema.events;
```

**关闭定时任务**

```sql
DROP event temp_event;
```

