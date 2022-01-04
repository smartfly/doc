

# [缓存更新的套路](https://coolshell.cn/articles/17416.html)

## Cache aside Pattern
Read: 先读缓存，命中缓存返回；未命中缓存，读取数据库并更新缓存，返回结果;
Write: 先写数据库，写成功后, 删除缓存;

Facebook采用Cache aside为标准设计模式。为什么呢？
[Scaling Memcache at Facebook](https://www.usenix.org/system/files/conference/nsdi13/nsdi13-final170_update.pdf)

[Why does Facebook use delete to remove the key-value pair in Memcached instead of updating the Memcached during write request to the backend?](https://www.quora.com/Why-does-Facebook-use-delete-to-remove-the-key-value-pair-in-Memcached-instead-of-updating-the-Memcached-during-write-request-to-the-backend)

```
Just imagine what if two concurrent updates of the same data element occur? You might have different values of the same data item in DB and in memcached. Which is bad. There is a certain number of ways to avoid or to decrease probability of this. Here is the couple of them:

1. A single transaction coordinator
2. Many transaction coordinators, with an elected master via Paxos or Raft consensus algorithm
3. Deletion of elements from memcached on DB updates

I assume that they chose the way #3 because "a single" means a single point of failure, and Paxos/Raft is not easy to implement plus it sacrifices availability for the benefit of consistency.
```

要么通过2PC或是Paxos协议保证一致性，要么就是拼命的降低并发时脏数据的概率，
而Facebook使用了这个降低概率的玩法，因为2PC太慢，而Paxos太复杂。当然，最好还是为缓存设置上过期时间

## Read/Write Though
redis + repository 合体变成单一存储，存储自己维护自己cache

### Read Though

Read Through 套路就是在查询操作中更新缓存，也就是说，当缓存失效的时候（过期或LRU换出），
Cache Aside是由调用方负责把数据加载入缓存，而Read Through则用缓存服务自己来加载，从而对应用方是透明的。

### Write Though

Write Through 套路和Read Through相仿，不过是在更新数据时发生。当有数据更新的时候，
如果没有命中缓存，直接更新数据库，然后返回。如果命中了缓存，则更新缓存，然后再由Cache自己更新数据库

## Write Behind Caching Pattern
Write Behind 又叫 Write Back。Write Back套路，一句说就是，在更新数据的时候，只更新缓存，不更新数据库，而我们的缓存会异步地批量更新数据库。
这个设计的好处就是让数据的I/O操作飞快无比（因为直接操作内存嘛 ），
因为异步，write back还可以合并对同一个数据的多次操作，所以性能的提高是相当可观的


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

