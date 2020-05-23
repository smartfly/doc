

# [system-design-primer](https://github.com/donnemartin/system-design-primer)



# [分布式系统的事务处理](https://coolshell.cn/articles/10910.html)

- 一致性模型
- Master-Slave
- Master-Master
- Two/Three Phase Commit
- Two Generals Problem
- Paxos Algorithm

# [Gossip 协议简介](http://kaiyuan.me/2015/07/08/Gossip/)

Gossip是分布式系统中被广泛使用的协议，其主要用于实现分布式节点或者进程之间的信息交换。Gossip协议同时满足应用层多播协议所要求的低负载，高可用和可扩展性的要求。由于其简单而易于实现，当前很多系统(例如Amazon S3, Usenet NNTP等)选择基于Gossip协议以实现应用层多播的功能。

# [分布式原理：一文了解Gossip协议](https://blog.csdn.net/b6ecl1k7BS8O/article/details/86653449)

## Gossip是什么

Gossip协议(gossip protocol) 又称epidemic 协议(epidemic protocol), 基于病传播方式的节点或者进程之间信息交换的协议，在分布式系统中被广泛使用，比如我们可以使用 gossip 协议来确保网络中所有节点的数据一样。

从 gossip 单词就可以看到，其中文意思是八卦、流言等意思，我们可以想象下绯闻的传播（或者流行病的传播）；gossip 协议的工作原理就类似于这个。gossip 协议利用一种随机的方式将信息传播到整个网络中，并在一定时间内使得系统内的所有节点数据一致。Gossip 其实是一种去中心化思路的分布式协议，解决状态在集群中的传播和状态一致性的保证两个问题。

## Gossip优势

### 可扩展性(Scalable)

### 容错性(Fault-tolerance)

### 健壮性(Robust)

### 最终一致性(Convergent consistency)

 ## Gossip协议类型

### Anti-Entropy(反熵)

- susceptible: 处于该状态代表其并没有收到来自其他节点的更新
- infective: 处于该状态的节点代表其数据更新

### Rumor-Mongering(谣言传播)

- susceptible
- infective
- removed: 该状态说明其已经收到来自其他节点的更新，但是其并不会将这个更新分享给其他节点

## Gossip协议的通讯方式

不管是 Anti-Entropy 还是 Rumor-Mongering 都涉及到节点间的数据交互方式，节点间的交互方式主要有三种：Push、Pull 以及 Push&Pull。

- Push
- Pull
- Push&Pull

## Gossip在工程上的使用

gossip 协议可以支持以下需求：

- Database replication
- 消息传播
- Cluster membership
- Failure detection
- Overlay Newworks
- Aggregations (比如计算平均值、最大值以及总和)

## Gossip在开源项目使用

- Riak: （https://github.com/basho/riak） 使用 gossip 协议来共享和传递集群的环状态（ring state）和存储桶属性（bucket properties）。
- Cassandra: 节点间的信息交换使用了 gossip 协议，因此所有节点都可以快速了解集群中的所有其他节点。
- Dynamo: 采用基于 gossip 协议的分布式故障检测和成员协议，这样集群中添加或移除节点，其他节点可以快速检测到。
- Consul: 使用了称为 SERF 的gossip 协议，主要有两个目的：1、发现新的节点或者发现故障节点；2、为一些重要的事件（比如 Leader 选举）传播提供可靠、快速的传播
- Amazon s3: 使用 gossip 协议将服务的状态传递给系统。





# [alibaba/Sentinel](https://github.com/alibaba/Sentinel)

A powerful flow control component enabling reliability, resilience and monitoring for micro-services.

# [What's a Service Mesh](https://jimmysong.io/blog/what-is-a-service-mesh/)



# [服务发现框架选型：Consul、Zookeeper还是etcd?](https://www.cnblogs.com/sunsky303/p/11127324.html)

- Zookeeper
- etcd
- Consul

# [服务发现：Zookeeper vs etcd vs Consul](https://technologyconversations.com/2015/09/08/service-discovery-zookeeper-vs-etcd-vs-consul/)

## 服务发现工具

## 手动配置

## Zookeeper



## etcd



## Registrator



## Confd



## Consul





# [bilibili技术总监毛剑：B站高可用架构实践](https://mp.weixin.qq.com/s?__biz=MzI2NDU4OTExOQ==&mid=2247491637&idx=1&sn=33997fbcba394352567c1950ff7dae69&chksm=eaa8fa65dddf7373bdcada20cebbb08e6b5f35c5c99775311adcaa5065f449be71213cda9857&token=6036749&lang=zh_CN#rd)

- 负载均衡
- 限流
- 重试
- 超时
- 应对连锁故障

# [分布式从 ACID、CAP、BASE 的理论推进](https://gocn.vip/topics/10121)

## ACID

数据库事务的```ACID```特性

- atomicity(原子性)
- consistency(一致性)
- isolation(隔离性)
- durability(持久性)

## CAP

分布式计算机系统CAP定律：

- consistency(一致性)
- availability(可用性)
- partition-torlerance(分区容错性)

## BASE

BASE是对CAP中一致性和可用性权衡的结果。

- Basically Available(基本可用)
- Soft State(软状态)
- Eventually Consistent(最终一致性)