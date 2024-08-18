1. MVCC的核心是Read View和undo log版本链
2. Read View记录了事务启动时的状态，包括自己的事务id，活跃未提交的的事务id列表，下一个将要被分配的事务的id
3. 当访问时，通过自己事务id和活跃未提交的事务id列表进行确定访问到的值
	1. id < min_trx_id 可见
	2. id > max_trx_id 不可见
	3. id > min_trx_id && id < max_trx_id && id **not in** m_ids 可见
	4. id > min_trx_id && id < max_trx_id && id **in** m_ids 不可见
4. 而事务的管理级别则通过控制生成Read View的时机来进行实现