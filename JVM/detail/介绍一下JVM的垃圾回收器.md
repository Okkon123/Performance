**按回收区域分**
1. 新生代垃圾回收器
	1. Serial New
	2. ParNew
	3. Parallel Scavenge
	4. 都基于标记-复制
2. 老年代垃圾回收器
	1. Serial Old
		1. 标记整理
	2. Parallel Old
		1. 标记整理
	3. CMS
		1. 标记清除，三色标记
3. 整堆垃圾回收器
	1. G1 
		1. 标记-复制新生代
		2. 标记-整理老年代
		3. 三色标记
	2. ZGC
按串并行分
1. 串行
	1. Serial New
	2. Serial Old
2. 并行
	1. ParNew
	2. Parallel Scavenge
	3. Parallel Old
	4. G1
	5. CMS
	6. ZGC