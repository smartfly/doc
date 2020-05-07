



# [分布式系统的事务处理](https://coolshell.cn/articles/10910.html)

- 一致性模型
- Master-Slave
- Master-Master
- Two/Three Phase Commit
- Two Generals Problem
- Paxos Algorithm

# [Gossip 协议简介](http://kaiyuan.me/2015/07/08/Gossip/)

Gossip是分布式系统中被广泛使用的协议，其主要用于实现分布式节点或者进程之间的信息交换。Gossip协议同时满足应用层多播协议所要求的低负载，高可用和可扩展性的要求。由于其简单而易于实现，当前很多系统(例如Amazon S3, Usenet NNTP等)选择基于Gossip协议以实现应用层多播的功能。

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