1. AQS维护了一个FIFO的等待队列，头节点为持有锁线程，根据队列顺序获取锁
	1. 非公平锁队列头Node线程与刚准备获取锁的新线程竞争
	2. 公平锁绝大部分情况按照队列顺序获取锁