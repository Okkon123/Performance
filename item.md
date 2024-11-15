分布式
1. 负载均衡
2. 请求响应模式(同步/异步)
3. 操作系统/计网
	1. IO
	2. 持久化
		1. 刷盘
		2. COW
	3. 读写

- 面试表现
	- 学习能力
	- 对原理理解
	- 做事
	- 代码运行在机器上

业务介绍：与算法与运维对接，对全国的单车、电动车进行智能调度并对电动车进行换电，提升车辆的的翻台率。
1. 核心链路稳定性-可观测性建设，通过Kafka+Flink+Prometheus+Grafana，监控核心链路的各项业务指标，配置相关告警，保障线上服务可靠性，预防P0事故一起，小事故若干起。
2. 核心链路稳定性-可用性建设，梳理核心链路涉及接口中间件，产出接口、MQ故障应急方案。
3. 排查线上问题，及时响应告警，解决了线程池阻塞队列堆积、Kafka发送超时、MySQL深分页等问题。
4. 优化Es车辆画像召回接口，对dsl语句进行调优，解决由Integer字段导致的主要耗时，将接口耗时从100ms降低到60ms，同比提升40%。
5. 优化蓝牙竞对嗅探链路，将大粒度全量任务拆分为细粒度任务，提升定时任务频率，解决了数据延时和蓝牙模块电量限制问题。
6. 使用线程池异步发送嗅探任务，减少蓝牙嗅探任务时延，避免了频繁创建和销毁线程的开销，通过动态配置灵活调整线程数。
7. 使用Redis防止消息的重复消费，同时利用动态配置Key的过期时间，根据业务情况灵活修改，减少开发工作量。
8. 利用RocketMQ的“异步”和“削峰”特性，实现对多个城市的蓝牙嗅探任务的下发，显著减少了瞬时压力，提高了系统的稳定性。
9. 完成产品侧、组内的业务技术需求，沉淀新人入职、业务梳理、业务开发、埋点开发、监控告警、压测流程等SOP文档。


微博短链接
核心技术：SpringBoot + Redis + MySQL + MyBatisPlus + RocketMQ + ShardingSphere + Sentinel
1. 使用布隆过滤器来判断短链接是否已存在，效率远胜使用分布式锁查询数据库，经Jmeter测试效率约为分布式锁的六倍。 
2. 封装缓存不存在读取功能，通过双重判定锁优化更新或失效场景下大量查询数据库问题。 
3. 采用了通过更新数据库在删除缓存的策略，保证了缓存一致性。 
4. 为实现短链接在海量访问场景下的数据修改功能，使用了 redisson 分布式锁，确保数据修改的安全和一致性。 
5. 借助 RocketMQ 消息队列的"削峰"特点，实现大量访问短链接场景下的监控信息存储功能。 
6. 在消息队列消费业务中，使用 Redis 来完成幂等场景的处理，确保消息在一定时间内仅被消费一次，避免重复处理。 
7. 考虑兼容短链接用户需求，短链接数据分片的基础上增加了路由表，使用户能够方便地分页查看短链接。 
8. 使用 Sentinel 进行接口访问的 QPS 限流，以保障短链接系统的稳定运行。当触发限流规则时，系统能够进行降级处理。


6.S081 Kernel Enhancement
 本项目基于类Unix系统xv6为基础，对系统调用、进程管理、文件系统、中断等模块进行扩展和优化
1. 理解xv6系统调用过程，增加新的系统调用
2. 利用缺页故障，在xv6上实现内存Lazy Allocation和COW
3. 理解xv6进程调度和上下文切换过程，以此参照在用户态实现协程
4. 理解并修改xv6的文件系统，使单个文件的大小限制由268KB改进为65803KB
5. 更改数据结构和锁策略，降低内存空闲页分配与释放、磁盘缓存块使用过程的锁争用

**专业技能**
- 熟练掌握Java基础知识，如集合框架(HashMap, ArrayList, LinkedList)、Stream流、反射机制，具备良好的编码能力 。
- 熟悉SSM、Spring Boot 、Maven等主流框架，理解 IoC 和 AOP 的原理。
- 熟悉RocketMQ、Kafka，理解消息队列模型，零拷贝原理，解决过消息堆积，发送超时问题。
- 熟悉MySQL，熟悉索引、事务、锁、日志等核心概念，解决过深分页导致的慢查询问题。
- 熟悉Redis，了解Redis数据结构、缓存策略及持久化方法，能有效解决缓存穿透、缓存击穿和缓存雪崩问题 。
- 熟悉JVM，内存模型、GC算法、常见垃圾回收器、类加载机制 
- 熟悉JUC，了解Java中的各种锁机制、CAS、AQS、线程池、ThreadLocal等。
- 熟悉Elasticsearch搜索中间件，了解其倒排索引原理，有对es接口优化的经验。
- 掌握计算机网络，熟悉常见网络协议如HTTP/HTTPS、TCP/UDP、IP、DNS 。
- 熟练使用Linux、git掌握各类基本命令。

- 名词解释
- 召回
- 逻辑处理
- 存储