1. 概念
	1. 它允许每个线程都有拥有自己的独立副本，从而实现线程隔离，用于解决多线程中共享对象的线程安全问题。
2. 实现
	1. 线程中有一个ThreadLocalMap
	2. ThreadLocal是ThreadLocalMap的键
	3. ThreadLocalMap对ThreadLocal是弱引用
	4. ThreadLocal有内存泄漏的风险
	5. ThreadLocalMap对value是强引用