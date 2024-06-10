# MySQL 证书管理

## MySQL 证书配置
* 更新配置设置证书路径
```
[mysqld]
ssl-ca=/etc/mysql/ssl-certs/ca.crt
ssl-cert=/etc/mysql/ssl-certs/tls.crt
ssl-key=/etc/mysql/ssl-certs/tls.key
```

* 查看证书当前设置
```
mysql> show variables like '%ssl%';
```

* 创建用户开启证书认证
```
mysql> CREATE USER 'test2'@'%' IDENTIFIED BY 'admin123456';
mysql> GRANT ALL ON *.* TO 'test2'@'%' WITH GRANT OPTION ;
mysql> ALTER USER 'test2'@'%' REQUIRE SSL;  // 开启证书认证
mysql> ALTER USER 'test2'@'%' REQUIRE NONE; // 关闭证书认证
mysql> FLUSH PRIVILEGES;
```

* 查看用户是否开启SSL
```
mysql> select user,host,ssl_type from mysql.user;
```

## ProxySQL 证书配置
> 参考：https://proxysql.com/documentation/ssl-support/

* 连接 ProxySQL 服务
```
mysql -h127.0.0.1 -uadmin -padmin123456 -P6032 --skip-ssl
```

* 查看当前服务是否开启SSL
```
SELECT * FROM global_variables WHERE variable_name LIKE 'mysql-have_ssl';
```

* 开启SSL
```
SET mysql-have_ssl=true;
LOAD MYSQL VARIABLES TO RUNTIME;
SAVE MYSQL VARIABLES TO DISK;
```

* 设置指定路径证书
```
SELECT * FROM global_variables WHERE variable_name LIKE '%ssl%';
SET mysql-ssl_p2s_ca="/etc/proxysql/ssl-certs/ca.crt";
SET mysql-ssl_p2s_cert="/etc/proxysql/ssl-certs/tls.crt";
SET mysql-ssl_p2s_key="/etc/proxysql/ssl-certs/tls.key";
LOAD MYSQL VARIABLES TO RUNTIME;
SAVE MYSQL VARIABLES TO DISK;
```

* 设置 mysql_server 开启SSL
```
SELECT * FROM runtime_mysql_servers;
UPDATE mysql_servers SET use_ssl=1 WHERE port=3306;
LOAD MYSQL SERVERS TO RUNTIME;
SAVE MYSQL SERVERS TO DISK;
```



