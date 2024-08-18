1. 更新先写在Buffer Pool，断电数据消失，redolog会记录内存的改变，先也到磁盘当中，也即是WAL，MySQL 的写操作并不是立刻写到磁盘上，而是先写日志，然后在合适的时间再写到磁盘上。redo log 保证了事务四大特性中的持久性，让 MySQL 有 crash-safe 的能力。并且让MySQL 的写操作从磁盘的「随机写」变成了「顺序写」。
2.  redo log 也不是直接写入磁盘的，redolog也有buffer，不同参数写入时机不同。
3. redo log的存储是循环的
4. 由innodb实现