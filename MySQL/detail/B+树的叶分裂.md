1. 当向B+树中插入一个新元素时，如果目标叶子节点已经满了（即达到了树的最大存储容量），就需要进行叶分裂操作，以保持树的平衡性和性能。
2. 所以主键最好是要能递增的，添加数据时能够能够减少页分裂的次数。