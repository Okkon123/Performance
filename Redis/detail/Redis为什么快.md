1. Redis基于内存，操作内存的速度快很多
2. Redis单线程，减少了上下文切换的损耗，不需要所保证原子性
3. 多路复用的 IO 方式
4. `Redis`整个结构类似于`HashMap`，查找和操作复杂度为`O(1)`，不需要和`MySQL`查找数据一样需要产生随机磁盘IO或者全表
5. 后台处理重写
6. 客户端通信协议采用`RESP`，简单易读，避免了复杂请求的解析开销
7. 渐进式rehash，减少阻塞
8. 惰性删除