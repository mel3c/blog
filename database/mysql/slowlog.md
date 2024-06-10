# MySQL 慢日志分析

## 分析慢日志
* 通过工具分析慢日志
```
mysqldumpslow /tmp/mysql_slow.log

pt-query-digest /path/to/slow-query.log
```

## 慢日志实时采集

### 通过 fluentd 采集慢日志

fluent-gem install fluent-plugin-mysqlslowquery 0.0.9

```
<source>
    type mysql_slow_query
    path /var/log/mysql/slow.log
    tag mysqld.slow_query
</source>
````

slow_query_log = 1  
long_query_time = 2  
log_slow_admin_statements = 1  
"slow_query_log_file": "/var/log/mysql/slow.log"

