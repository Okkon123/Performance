#### 线程
##### [[创建线程时，JVM中发生了什么]]
1. 分配虚拟机栈、程序计数器
2. 分配本地方法栈
3. 初始化线程本地存储
4. 线程状态初始化
5. 调用start
6. JVM管理线程生命周期
##### [[介绍一下JMM]]
1. java内存模型中有两种内存
2. 一种是主内存，一种是各个线程的工作内存
3. 线程先从主内存读取数据到自己的工作内存
4. 写操作都在自己的工作内存中进行
5. 这也产生了内存的可见性问题
6. 因为是在自己的工作内存中进行操作，对其他线程不可见
java使用volitile保障了可见性
##### [[什么是线程，进程和线程之间的区别与联系]]
1. 进程是资源分配的最小单位
	1. 一个进程包含多个线程
2. 线程是操作系统调度的最小单位
	1. 多个线程共享一个进程的资源
##### [[线程之间如何进行通信]]
1. synchronized
2. wait/notify/notifyAll
3. ReentrantLock
4. 并发工具类
##### [[线程有哪些状态]]
1. Runnable
2. waiting
3. time_waiting
4. blocked
5. terminated
##### [[线程处于什么状态在runnable状态]]
1. 获取时间片运行中
2. 未获取时间片等待
##### [[volatile关键字的理解]]
1. 维护可见性
#### 悲观锁
##### [[介绍一下synchronized升级过程]]
 1. 无锁状态
	 1. 初始情况下，对象是处于无锁状态的，也就是说没有任何线程对该对象加锁。
2. 偏向锁（Biased Locking）
	1. 当一个线程第一次访问同步代码块时，JVM会为该线程设置偏向锁。再次进入同步代码块，则不需要再进行加锁
 3. 轻量级锁（Lightweight Locking）
	 1. 当第二个线程尝试获取已经被偏向锁占用的锁时，偏向锁会被撤销，然后升级为轻量级锁。当偏向锁升级为轻量级锁时，JVM会在当前线程的栈帧中创建一个锁记录（Lock Record），并使用对象头中的锁指针指向这个锁记录。
 4. 重量级锁（Heavyweight Locking）
	 1. 当锁的竞争进一步激烈，轻量级锁无法满足需求时，JVM会将锁升级为重量级锁。当锁状态升级到重量级锁状态时，JVM 会将该对象的锁变成一个重量级锁，并在对象头中记录指向等待队列的指针。  

##### [[synchronized实现原理]]
1. 同步代码块
	1. monitorenter
	2. monitorexit
2. 同步方法：
	1. ACC_SYNCHRONIZED 
##### [[介绍一下AQS]]
1. AQS即抽象同步队列
2. 他维护了一个Thread变量，表示持有锁的线程
3. 还维护了一个state变量，表示锁是否被持有，及重入次数，并且state的状态时通过CAS更新的
4. 他维持了一个队列，没有获得锁的线程会被封装成一个Node结点加入到队列中
5. 锁被释放时，AQS会唤醒队列中的节点，进行争抢锁
6. 线程唤醒和阻塞以来LockSupport的park和unpark
##### [[AQS核心的方法]]
1. getState()
2. setState(int newState)
3. compareAndSetState(int expect, int update)
4. tryAcquire(int arg)
5. tryRelease(int arg)
6. acquire(int arg)
7. release(int arg)
8. addWaiter(Node node)
9. enq(Node node)
10. acquireQueued(Node node, int arg)
11. unparkSuccessor(Node node)
12. parkAndCheckInterrupt()
##### [[Java提供了哪些锁]]
1. 乐观锁
	1. CAS
2. 悲观锁
	1. synchronized
	2. ReentrantLock
	3. ReentrantReadWriteLock
	4. LockSupport
	5. Condition
	6. Semaphore
	7. CountDownLatch
	8. CyclicBarrier
##### [[ReentrantLock公平_非公平实现原理]]
1. AQS维护了一个FIFO的等待队列，头节点为持有锁线程，根据队列顺序获取锁
	1. 非公平锁队列头Node线程与刚准备获取锁的新线程竞争
	2. 公平锁绝大部分情况按照队列顺序获取锁
#### 乐观锁
##### [[介绍一下CAS]]
1. CAS 操作包含三个操作数 —— 内存位置（V）、预期原值（A）和新值(B)。在进行并发修改的时候，会先比较A和V中取出的值是否相等，如果相等，则会把值替换成B，否则就不做任何操作。
2. CAS底层依赖Unsafe类，直接对底层数据进行修改，在CPU层面以来cmpxchg指令，实现操作的原子性
3. CAS摒弃了传统的锁机制，避免了因获取和释放锁产生的上下文切换和线程阻塞，但是，在高并发条件下，频繁的CAS操作可能导致大量的自旋重试，消耗大量的CPU资源，所以CAS不适合特别高并发的场景。
##### [[CAS本身有什么问题]]
1. ABA问题
2. 常使用递增版本号解决
##### [[乐观锁和悲观锁之间的区别，有哪些实现方式]]
1. 乐观锁
- **态度**：乐观锁假设并发冲突不会经常发生，因此在访问资源时不会立即加锁，而是在提交操作时检查冲突。
- **实现方式**：常用的实现方式是版本号机制和CAS（Compare-And-Swap）操作。
- **适用场景**：适用于读多写少的场景，因为在高并发读操作中，冲突较少。
2. 悲观锁
- **态度**：悲观锁假设并发冲突会经常发生，因此在访问资源时立即加锁，以确保互斥访问。
- **实现方式**：常用的实现方式是使用数据库的行锁、表锁，或者Java中的`ReentrantLock`、`synchronized`关键字。
- **适用场景**：适用于写多读少的场景，因为在高并发写操作中，可以有效防止数据不一致。
#### ThreadLocal
##### [[介绍一下ThreadLocal]]
1. 概念
	1. 它允许每个线程都有拥有自己的独立副本，从而实现线程隔离，用于解决多线程中共享对象的线程安全问题。
2. 实现
	1. 线程中有一个ThreadLocalMap
	2. ThreadLocal是ThreadLocalMap的键
	3. ThreadLocalMap对ThreadLocal是弱引用
	4. ThreadLocal有内存泄漏的风险
	5. ThreadLocalMap对value是强引用
##### [[ThreadLocal有什么问题]]
1. ThreadLocal有内存泄漏的风险
2. ThreadLocalMap对value是强引用
##### [[怎么进行父子进程之间的信息传递]]
1. inheritableThreadLocals
#### 线程池
##### [[线程池的原理，解决了什么问题]]
1. 通过池化思想，减少了线程创建的次数
2. **降低资源消耗**：通过池化技术重复利用已创建的线程，降低线程创建和销毁造成的损耗。
3. **提高响应速度**：任务到达时，无需等待线程创建即可立即执行。
4. **提高线程的可管理性**：线程是稀缺资源，如果无限制创建，不仅会消耗系统资源，还会因为线程的不合理分布导致资源调度失衡，降低系统的稳定性。使用线程池可以进行统一的分配、调优和监控。
5. **提供更多更强大的功能**：线程池具备可拓展性，允许开发人员向其中增加更多的功能。比如延时定时线程池ScheduledThreadPoolExecutor，就允许任务延期执行或定期执行。

##### [[线程池参数有哪些]]
1. corePoolSize 核心线程数
2. maximumPoolSize 最大线程数
3. keepAliveTime 非核心线程存活时间
4. unit 非核心线程存活时间单位
5. workQueue 等待队列
6. threadFactory 创建线程适用工厂
7. handler 拒绝策略
##### [[拒绝策略有哪些]]
1. AbortPolicy
2. CallerRunsPolicy
3. DiscardOldestPolicy
4. DiscardPolicy
5. 自定义拒绝策略，实现RejectedExecutionHandler接口
##### [[线程池等待队列有哪些]]
1. ArrayBlockingQueue 数组有界队列
2. LinkedBlockingQueue 链表结构队列
3. PriorityBlockingQueue 有优先级无界阻塞队列
4. DealyQueue 延迟执行队列
5. SynchronousQueue 无元素阻塞队列
6. LinkedTransferQueue 
7. LinkedBlockingDeque
##### [[线程池工作流程]]
1. 首先检查线程池是否还在工作
2. 检查线程数是否小于最小核心数，
3. 小于最小核心数则增加线程
4. 否则，检查阻塞队列是否满了
5. 没满加入阻塞队列
6. 满了看线程数是否小于最大线程数
7. 小于则增加线程
8. 否则拒绝
##### [[线程池提交execute和submit有什么区别]]
1. execute用于提交不需要返回值的任务
2. submit用于提交需要返回值的任务，线程池会返回一个future类型的对象。

#### 并发工具类
##### [[countdownlatch和cyclebarriar之间的区别]]
1. countDownLatch不可重用，cyclebarriar可重用
2. CyclicBarrier：还可以指定一个`Runnable`任务，当所有线程都到达屏障时执行该任务。适用于在所有线程到达屏障点后需要做一些额外操作的场景。