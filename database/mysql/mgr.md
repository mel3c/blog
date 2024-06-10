# MySQL MGR 常用操作

## MGR 集群管理

> CentOS 安装 GTID 修复工具  
> yum localinstall -y https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm  
> yum install mysql-utilities  
> ln -s /usr/lib/python2.7/site-packages/mysql/utilities /usr/lib64/python2.7/site-packages/mysql/utilities  

* 建立集群连接
```
mysqlsh admin@127.0.0.1 -pDream001
```

* 查看当前节点状态
```
mysql-js> \status
```

* 检查节点是否满足创建集群的条件
```
mysql-js> dba.checkInstanceConfiguration('admin@mysql-5-0.mysql-5:3306');
```

* 检查节点状态
```
dba.getCluster('MySQLCluster').checkInstanceState('root:11111111@mysql-test-0.mysql-test:3306')
```

* 创建集群
```
mysql-js> dba.createCluster('MySQLCluster');
```

* 查看集群状态
```
mysql-js> dba.getCluster('MySQLCluster').status()
```

* 添加实例
```
mysql-js> dba.getCluster('MySQLCluster').addInstance('admin@mysql-5-0.mysql-5:3306')
```

* 移除实例
```
mysql-js> dba.getCluster('MySQLCluster').removeInstance('mysql-5-0.mysql-5:3306')
```

* 指定一个新主节点
```
mysql-js> dba.getCluster('MySQLCluster').setPrimaryInstance('192.168.10.102:3306')
```

* 重新加入节点
```
dba.getCluster('MySQLCluster').rejoinInstance('admin@mysql-5-0.mysql-5:3306')
```

* 重启集群
```
dba.rebootClusterFromCompleteOutage('itopMgr')
```

* 切换为多主模式
```
mysql-js> dba.getCluster('MySQLCluster').switchToMultiPrimaryMode()
```

* 切花为单主模式
```
mysql-js> dba.getCluster('MySQLCluster').switchToSinglePrimaryMode('172.16.1.125:3306')
```

* 查看集群实例列表
```
mysql> select * from performance_schema.replication_group_members;

mysql> SELECT MEMBER_ID, MEMBER_HOST,MEMBER_PORT,MEMBER_STATE,IF(global_status.VARIABLE_NAME IS NOT NULL,'PRIMARY','SECONDARY') AS MEMBER_ROLE FROM performance_schema.replication_group_members LEFT JOIN performance_schema.global_status ON global_status.VARIABLE_NAME = 'group_replication_primary_member' AND global_status.VARIABLE_VALUE = replication_group_members.MEMBER_ID;
```


### 参考
* https://dev.mysql.com/doc/refman/8.0/en/group-replication-system-variables.html





