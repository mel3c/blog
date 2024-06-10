# kafka 配置

参考：  
https://cloud.tencent.com/developer/article/1806997  
https://www.cnblogs.com/jing99/p/13827789.html  
https://blog.csdn.net/yancychas/article/details/88561291

## Broker 配置

查看当前 Broker 的配置
```
./kafka-configs.sh --describe --bootstrap-server 10.99.4.59:29092 --entity-type brokers --entity-name 1 --all
```

```
controller.socket.timeout.ms = 30000  // partition leader与replicas之间通讯时,socket的超时时间
replica.socket.timeout.ms = 30000     // follower与leader之间的socket超时时
replica.lag.time.max.ms = 30000       // Follower 与 Leader 之间的最大复制滞后时间
replica.fetch.wait.max.ms =500        // replicas 同 leader 之间通信的最大等待时间，失败了会重试
```


## Producer 配置

```
acks = 1  // 只等待分区的 Leader 确认接收
request.timeout.ms = 10000    // 生产者等待分区的 Leader 确认消息的最长时间
message.send.max.retries = 3  // 写入失败后，重复尝试的次数
queue.buffering.max.ms = 5000  // 异步时设置的缓存时间，超过此值就发送
queue.buffering.max.message = 20000   // 异步时,缓存的最大消息量，条，超过则被阻塞/抛弃
batch.num.messages = 500
queue.enqueue.timeout.ms = -1  // -1 永远阻塞，0 清空队列

zookeeper.session.timeout.ms=6000  // ZooKeeper的最大超时时间，就是心跳的间隔，若是没有反映，那么认为已经死了，不易过大
zookeeper.connection.timeout.ms =6000  // ZooKeeper的连接超时时间
```
