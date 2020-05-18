

# [为什么 Redis 选择单线程模型](https://draveness.me/whys-the-design-redis-single-thread/)

CPU资源往往都不是Redis服务器的性能瓶颈。哪怕我们在一个普通的 Linux 服务器上启动 Redis 服务，它也能在 1s 的时间内处理 1,000,000 个用户请求。

# [为什么 Redis 快照使用子进程](https://draveness.me/whys-the-design-redis-bgsave-fork/)

# [美团针对Redis Rehash机制的探索和实践](https://mp.weixin.qq.com/s/ufoLJiXE0wU4Bc7ZbE9cDQ) 

# Redis 多地集群数据如何做到双向同步

[携程Redis跨IDC多向同步实践](https://zhuanlan.zhihu.com/p/83298388)

CRDT（Conflict-Free Replicated Data Type）是各种基础数据结构最终一致算法的理论总结，能根据一定的规则自动合并，解决冲突，达到强最终一致的效果。

[CRDT——解决最终一致问题的利器](https://yq.aliyun.com/articles/635632?utm_content=m_1000015503)

# Redis  存储结构体信息到底使用hash还是string

[strings-vs-hash-to-represent-json](https://stackoverflow.com/questions/16375188/redis-strings-vs-redis-hashes-to-represent-json-efficiency)

[Redis JSON store: rejson](https://redislabs.com/blog/redis-as-a-json-store/)

# Redis 各个数据类型最大存储量

String类型：一个String类型的value最大可以存储512M

List类型: list的元素个数最多为2^32-1个，也就是4294967295个。

Set类型：元素个数最多为2^32-1个，也就是4294967295个。

Hash类型：键值对个数最多为2^32-1个，也就是4294967295个。

Sorted-Set类型：元素个数最多为2^32-1个，也就是4294967295个。

参考资料：[Data types](https://redis.io/topics/data-types)

