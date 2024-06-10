# MySQL 常用操作

## 用户管理
* 查看用户列表
```
mysql> select host,user,authentication_string from mysql.user;

mysql> select user,host,password_expired,password_lifetime,password_last_changed,account_locked from mysql.user;
```

* 设置用户密码立即过期
```
mysql> ALTER USER 'admin'@'%' PASSWORD EXPIRE;
```

* 设置用户密码永不过期
```
mysql> ALTER USER 'admin'@'%' PASSWORD EXPIRE NEVER;
```

* 设置用户密码 90 天过期
```
mysql> ALTER USER 'admin'@'%' PASSWORD EXPIRE INTERVAL 90 DAY;
```

* 设置用户密码使用全局默认过期策略
```
mysql> ALTER USER 'admin'@'%' PASSWORD EXPIRE DEFAULT;
```


## 插件管理
* 查看插件列表
```
mysql> show plugins;
```


## 集群管理
* 查看 Master 状态
```
mysql> show master status\G
```

* 查看集群状态
```
mysqladmin -uroot -pmysql status
```


## 全局变黄
* 查看组复制当前设置
```
mysql> show global variables like '%group_replication%';
```

* 查看全局ID标记
```
mysql> show global variables like '%gtid%';
```

* 查看半同步设置
```
mysql> show global variables like '%semi%';
```

* 查看指定字段当前配置
```
mysql> SHOW VARIABLES LIKE "max_connections";
```


## 索引管理
* 查看索引使用情况
```
mysql> SELECT * FROM sys.schema_index_statistics;
```

* 索引命中率
```
计算公式：命中率 = 1 - (Innodb_buffer_pool_reads / Innodb_buffer_pool_read_requests)
```


## 函数管理
* 查看 MGR 对应的函数列表
```
mysql> SHOW FUNCTION STATUS LIKE '%gr%'\G
```

##  资源管理
* 查看 MySQL 资源占用情况
```
mysql> select now();select EVENT_NAME,COUNT_ALLOC,COUNT_FREE,FORMAT_BYTES(CURRENT_NUMBER_OF_BYTES_USED) AS CURRENT_NUMBER_OF_BYTES_USED_MB from performance_schema.memory_summary_global_by_event_name order by CURRENT_NUMBER_OF_BYTES_USED desc limit 30;
```

* MySQL 资源占用排序(8.0 版本)
```
mysql> select now();SELECT event_name, FORMAT_BYTES(current_alloc) AS current_alloc_mb FROM sys.x$memory_global_by_current_bytes order by current_alloc DESC limit 20;
```

* MySQL 资源占用排序(5.7 版本)
```
mysql> select now();SELECT event_name, sys.format_bytes(current_alloc) AS current_alloc_mb FROM sys.x$memory_global_by_current_bytes order by current_alloc DESC limit 20;
```

* 查看内存分配情况
```
mysql> select * from sys.memory_by_user_by_current_bytes;
```

* 查看用户线程内存分配情况
```
select * from sys.memory_by_thread_by_current_bytes;
```

* 查看Mysql总内存
```
select * from sys.memory_global_total;
```


* 查看进程列表
```
mysql> SHOW PROCESSLIST;
```

* 查看线程分配状态
```
mysql> SHOW STATUS LIKE 'Threads%';
```

* 查看 innodb_buffer 分配状态
```
mysql> SHOW STATUS LIKE 'Innodb_buffer_pool%';
```

* 查看存储引擎状态
```
mysql> SHOW ENGINE INNODB STATUS\G;
```

* 查看存储引擎
```
mysql> show engines;
```





