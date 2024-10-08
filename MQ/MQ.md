#### MQ架构
##### [[Kafka和RocketMQ架构]]
- Producer
- Broker
	- topic
		- Kafka：partition
		- RocketMQ：queue
- 协调中心
	- Kafka: zookeeper
	- Rocket: namesever
- Consumer
#### 消息发送
##### [[消息推拉模式]]
1. 推
	1. 实效性高，有消息就推给消费者
	2. 对消费者压力大，容易造成消息堆积
2. 拉
	1. 实效性低
	2. 对消费者的压力小
RocketMQ可以使用推/拉模式
Kafka主要使用拉模式
##### [[消息确认方式]]
1. 同步
2. 异步回调
RocketMQ/Kafka都可采用同步/异步方式

#### 消息存储
##### [[MQ刷盘方式]]
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
##### [[消息分发模式]]
1. 发布-订阅
2. 点对点
3. 广播模式
##### [[消息存活时间]]
1. Kafka：日志文件的存储时间
2. RocketMQ：消息文件的保存时间
##### [[消息大量堆积会造成什么问题]]
1. 磁盘耗尽
2. 新消息无法被消费，造成延迟
3. 可能导致新消息未被消费就过期
##### [[消息队列堆积了如何解决]]
1. 生产者限流、减少线程
2. 消费者限流、增加线程
3. 根据业务
	1. 减少定时任务频率
	2. 临时开启另一个队列缓冲

##### [[消息存储]]
1. Kafka
	1. 逻辑：topic
	2. 物理：partition
2. RocketMQ
	1. 逻辑：topic
	2. 物理：MQ

RocketMQ 如何存储消息
RocketMQ 使用一种称为 CommitLog 的文件系统来存储消息。CommitLog 是一个顺序写入的日志文件，消息被按照接收到的顺序写入到这个文件中。此外，RocketMQ 还使用了 ConsumeQueue 和 IndexFile 来辅助消息的快速检索和消费。
存储结构
1. CommitLog
2. ConsumeQueue
3. IndexFile


Kafka 如何存储消息
Kafka 采用了分区（Partition）和段（Segment）机制来存储消息。每个主题（Topic）可以有多个分区，每个分区被存储为一个目录，目录下包含多个段文件。

存储结构
1. 分区（Partition）
2. 段（Segment）

#### 消息消费
##### [[怎么实现顺序消费]]
1. 在 Kafka 中实现顺序消费，需要确保相同的消息键被发送到同一个分区，并使用单线程消费者处理分区的数据。
2. 在 RocketMQ 中实现顺序消费，通过顺序消息（Orderly Message）和消息队列选择器来保证顺序消费。
	1. 总结下来就是三次加锁，先锁定Broker上的MessageQueue，确保消息只会投递到唯一的消费者，对本地的MessageQueue加锁，确保只有一个线程能处理这个消息队列。对存储消息的ProcessQueue加锁，确保在重平衡的过程中不会出现消息的重复消费。
#### MQ选型
##### [[Kafka和RocketMQ选型]]
RocketMQ
1. 订单、支付等业务交互信息
2. 延迟消息
3. tag、消费重试、广播模式、消费限流
4. 发送tps低于2k、无陡增

Kafka
1. 心跳、埋点、日志等数据
2. 发送tps高于2k，存在陡增
3. 消息体较大，5kb以上
4. 需要用到flink消费

##### [[延迟消息]]
1. Broker收到延时消息了，会先发送到主题（SCHEDULE_TOPIC_XXXX）的相应时间段的Message Queue中，然后通过一个定时任务轮询这些队列，到期后，把消息投递到目标Topic的队列中，然后消费者就可以正常消费这些消息。
##### [[什么时候使用RPC，什么时候使用MQ]]

| 特性   | RPC 调用    | 消息队列              |
| ---- | --------- | ----------------- |
| 调用方式 | 同步调用      | 异步调用              |
| 耦合度  | 紧耦合、强依赖   | 松耦合、弱依赖           |
| 实时性  | 高         | 低                 |
| 可靠性  | 依赖于服务的可靠性 | 高，支持消息持久化         |
| 并发性能 | 难以应对高并发   | 易于平滑处理高并发         |
| 适用场景 | 实时性要求高的操作 | 异步处理、解耦、削峰填谷、事件驱动 |

##### [[Kafka为什么快]]
1. 生产者
	1. 异步发送
	2. 批量发送
	3. 消息压缩
	4. 并行发送到多个partition
2. 存储
	1. 异步刷盘
	2. 磁盘顺序写
	3. 零拷贝
3. 消息消费
	1. 批量拉取
	2. 并行消费
##### [[Kafka的重平衡机制]]
1. 暂停消费
2. 计算分区分配方案
3. 通知消费者
4. 重新分配分区
5. 恢复消费
6. 重平衡期间STW
##### [[分布式事务]]
1. 两阶段提交（2PC）：适用于需要强一致性的场景，但性能开销较大。
2. 三阶段提交（3PC）：减少了阻塞问题，但实现复杂。
3. 基于消息队列的最终一致性：适用于对一致性要求不高的场景，易于扩展。
4. TCC（Try-Confirm-Cancel）：适用于需要灵活控制事务的场景，但实现复杂。

##### [[事务消息]]
1. RocketMQ支持事务消息
2. 把本地消息表移到了Broker

1. 发送半消息
2. 执行本地事务
3. 提交事物消息
4. 回滚事务消息

