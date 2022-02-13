# InnoDB七种类型的锁
- Auto-inc Locks(自增锁): 表级锁，专门针对事务插入AUTO_INC的列，如果插入位置冲突，多个事务会阻塞，以保证数据一致性
- Shared and Exclusive Locks(共享/排它锁): 行级锁，S锁与X锁，强锁
- Intention Locks(意向锁): 表级锁，IS锁与IX锁，弱锁，仅仅表名意向；
- Insert Intention Locks(插入意向锁): 针对insert的，如果插入位置不冲突，多个事务不会阻塞，以提高插入并发
- Record Locks(记录锁): 索引记录上加锁，对索引记录实施互斥，以保证数据一致性；
- Gap Locks(间隙锁): 封锁索引记录中间的间隔，在RR下有效，防止间隔中被其他事务插入；
- Next-key Locks(临键锁): 封锁索引记录以及索引记录中间的间隔，在RR下有效，防止幻读；

[这次终于懂了，InnoDB的七种锁](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651967369&idx=1&sn=d639abf6772a72c25cc2537749258163&chksm=bd2d7a558a5af3436e0a435a3e7cd9f72f67c1269370be936a82dc0af483f47aefa48bd58d69&scene=178&cur_album_id=1776446719614910465#rd)



# [InnoDB](https://novoland.github.io/%E6%95%B0%E6%8D%AE%E5%BA%93/2015/08/17/InnoDB%20%E9%94%81.html)
## 事务并发问题
- Dirty Read(脏读): A看到B进行更新的数据，并以此为根据继续执行相关的操作；B回滚导致A操作的是脏数据。
- Non-repeatable Read(不可重复读): A先查询一次数据，然后B更新并提交，A再次查询，得到和上一次不同的查询结果
- Phantom Read(幻读): A查询一批数据，B插入或者删除了某些记录并提交，A再次查询发现结果集中出现了上次没有的记录，或者上次有的记录消失了

## 事务隔离界别
- Read Uncommited: 最弱，事务的任何动作对其他事务都是立即可见的。存在脏读、不可重复读、幻读问题
- Read Commited: 只能读到其他事务已提交的数据，中间状态的数据则看不到，解决了脏读问题。
- Repeatable Read(innoDB的默认隔离界别): 根据标准的SQL规范，该级别解决了不可重复读的问题，保证在一个事务内，对同一条记录的重复读都是一致的。 
- Serial: 最高，所有读写都是串行的

**注意**: InnoDB的Repeatable Read通过MVCC和间隙锁机制解决幻读问题。InnoDB 对事务隔离级别的实现依赖两个手段：锁、MVCC(多版本控制协议)。
MVCC可以认为是对锁机制的优化，让普通select避免加锁，同时还能有事务隔离级别的语义保证


# [详解mysql如何做事务](https://mp.weixin.qq.com/s/Ng9akFrQNtaHhzi_Obl_og)



# [MySQL和InnoDB](https://draveness.me/mysql-innodb/)


# [为什么MySQL使用B+树](https://draveness.me/whys-the-design-mysql-b-plus-tree/)



[how the append-only btree works](http://www.bzero.se/ldapd/btree.html)

# MySQL基本概念

- OLTP(On-line Transaction Processing)

- OLAP(On-line Analytical Processing)

- ACID(atomicity, consistency, isolation, durability)

# 参考资料

[OLTP vs. OLAP](https://www.datawarehouse4u.info/OLTP-vs-OLAP.html)