# PostgreSQL 常用操作命令

## 模式操作

## 库操作

* 启动数据库
```
pg_ctl -D /pgdata -l /tmp/pg.log  start
```

* 登录数据库
```
// 方法一
psql -U username -d dbname -h hostip -p port

// 方法二
psql -h 172.17.0.10 -p 5432 -U postgres password=postgres -c "select inet_server_addr(),pg_is_in_recovery()"

// 方法三
psql postgres://postgres:pgsql%401234@172.17.0.48:5432/postgres
```

* 数据库列表
```
postgres=# \l
postgres=# \l+
postgres=# select * from pg_database;
```

* 切换数据库
```
postgres=# \c testdb
```

* 创建数据库
```
postgres=# create database p000db owner logical_rep;
[root@localhost ~]# createdb mydb   // 终端命令创建
```

* 删除数据库
```
postgres=# drop database test2;
[root@localhost ~]# dropdb mydb
```

* 批量删除库
```
postgres=# SELECT 'DROP DATABASE ' || quote_ident(datname) || ';'
postgres=# FROM pg_database
postgres=# WHERE datname LIKE 'test%' AND datistemplate=false
postgres=#
postgres=# \gexec
```

* 查看支持的编码
```
template1=# select * from pg_language;
```

* 查看当前的会话连接
```
template1=# select * from pg_stat_activity;
```

* 查看数据库当前的配置
```
postgres=# select * from pg_settings;
postgres=# show primary_conninfo;
```

* 查看库大小
```
postgres=# select pg_database.datname, pg_database_size(pg_database.datname) AS size from pg_database;
```

* 查看数据库版本
```
postgres=# SELECT version();
```

## 表操作

* 列出所有表
```
postgres=# \d  // 默认只列出 public 模式的表
postgres=# \d+
postgres=# SELECT * FROM pg_tables; // 列出所有模式下的所有表
postgres=# SELECT * FROM pg_tables where schemaname = 'public'; // 列出指定模式下的表
```

* 列表表字段
```
template1=# \d tablename
template1=# \d+ tablename
```

* 新建表
```
template1=# create table test(id INTEGER, task_class INTEGER,age TEXT,PRIMARY KEY(id, task_class));
template1=# create table test1 ( c1 int primary key, c2 varchar(10),c3 varchar(20) );
template1=# CREATE TABLE ldcode (id bigint primary key,name text);
```

* 查看表空间
```
template1=# select * from pg_tablespace;
```

* 插入数据
```
postgres=# INSERT INTO test1 VALUES (1, '22', '222');

// 批量插入数据
postgres=# insert into test1 select generate_series(1,100), 'picc', '201807191010';

// 从文件导入数据
postgres=# COPY weather FROM '/home/user/weather.txt';

// 自动插入指定数据库的数据
postgres=# INSERT INTO t1 (id, name) select generate_series(1,100000), 'aaa';
```

* 更新数据
```
postgres=# update test1 set c3='aaaa' where c1=3;
```

* 删除数据
```
postgres=# delete from test1 where c1=3;
```

## 用户管理

* 创建用户
```
postgres=# create user logical_rep with login password 'logical_rep';

// 创建超级用户
postgres=# CREATE ROLE zhangsan superuser PASSWORD 'Passw0rd12' login;
```

* 修改用户密码
```
postgres=# ALTER USER replicator WITH PASSWORD '234efedk1u';
```

* 删除用户
```
// 解除用户关联
postgres=#  revoke ALL on schema public from piccaddm;
postgres=#  revoke ALL on all tables in schema public from piccaddm;

// 删除用户
postgres=# drop role piccaddm;
```

* 查看角色用户
```
postgres=# \du
postgres=# select * from pg_user;
postgres=# select * from pg_shadow;
```

* 查看角色列表
```
postgres=#  select * from pg_roles;
```

## 复制槽管理

## `wal` 日志

## 集群管理

## 流复制

## 插件管理

## 发布/订阅


