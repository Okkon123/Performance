
| 特性   | RPC 调用    | 消息队列              |
| ---- | --------- | ----------------- |
| 调用方式 | 同步调用      | 异步调用              |
| 耦合度  | 紧耦合、强依赖   | 松耦合、弱依赖           |
| 实时性  | 高         | 低                 |
| 可靠性  | 依赖于服务的可靠性 | 高，支持消息持久化         |
| 并发性能 | 难以应对高并发   | 易于平滑处理高并发         |
| 适用场景 | 实时性要求高的操作 | 异步处理、解耦、削峰填谷、事件驱动 |
- RPC优点
	- 定时化
	- 压缩
	- 连接池

