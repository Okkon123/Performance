1. 不进行数据淘汰
2. 有过期时间
	1. 随机
	2. 根据过期时间淘汰
	3. 最近最少使用
3. 无过期时间
	1. 随机
	2. 最近最少使用
	3. 最近最久未使用

1. noeviction
2. volatile-random
3. volatile-ttl
4. volatile-lru
5. volatile-lfu
6. allkeys-random
7. allkeys-lru
8. allkeys-lfu

1. 在`Redis`中，数据有一部分访问频率较高，其余部分访问频率较低，或者无法预测数据的使用频率时，设置`allkeys-lru`是比较合适的。
2. 如果所有数据访问概率大致相等时，可以选择`allkeys-random`。
3. 如果研发者需要通过设置不同的ttl来判断数据过期的先后顺序，此时可以选择`volatile-ttl`策略。
4. 如果希望一些数据能长期被保存，而一些数据可以被淘汰掉时，选择`volatile-lru`或`volatile-random`都是比较不错的。
5. 由于设置`expire`会消耗额外的内存，如果计划避免`Redis`内存在此项上的浪费，可以选用`allkeys-lru`策略，这样就可以不再设置过期时间，高效利用内存了。
5. `maxmemory-policy`：参数配置淘汰策略。`maxmemory`：限制内存大小。

