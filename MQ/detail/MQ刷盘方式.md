1. 同步刷盘
2. 异步刷盘

**Kafka 同步刷盘配置**
Kafka 提供了 `acks` 和 `flush.messages` 参数来控制同步刷盘的行为：
1. **acks 参数**：
    - `acks=0`：生产者不等待任何确认。
    - `acks=1`：生产者等待领导者（Leader）副本确认消息写入日志，但不等待同步到所有副本。
    - `acks=all` 或 `acks=-1`：生产者等待所有同步副本确认消息写入日志，确保消息的持久化。
2. **flush.messages 参数**：指定在刷盘前允许的未刷盘消息数量。
强制同步刷盘
你可以通过设置 `log.flush.interval.messages` 和 `log.flush.interval.ms` 参数来强制 Kafka 定期刷盘：
- `log.flush.interval.messages`：每隔指定数量的消息执行一次刷盘。
- `log.flush.interval.ms`：每隔指定时间间隔执行一次刷盘。
**RocketMQ**
异步刷盘 flushDiskType=ASYNC_FLUSH 
同步刷盘 flushDiskType=SYNC_FLUSH