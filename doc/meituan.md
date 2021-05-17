# 美团

## 1.mysql 日志文件有哪些，分别介绍下作用
* 错误日志
* 查询日志：查询日志里面记录了数据库执行的所有命令，不管语句是否正确，都会被记录
* 慢查询日志：慢查询日志记录的慢查询不仅仅是执行比较慢的SELECT语句，还有INSERT，DELETE，UPDATE，CALL等DML操作，只要超过了指定时间，都可以称为"慢查询"，被记录到慢查询日志中
* redo log：记录事务执行后的状态，用来恢复未写入磁盘的数据（data file）的已成功事务更新的数据。
* undo log：两个作用：提供回滚和多个行版本控制(MVCC)
* binlog：MySQL的二进制日志（binary log）是一个二进制文件，主要记录所有数据库表结构变更（例如CREATE、ALTER TABLE…）以及表数据修改（INSERT、UPDATE、DELETE…）的所有操作。binlog作用：恢复数据、复制数据、审计

## 2.你们项目为什么用 redis，快在哪，怎么保证高性能，高并发的
redis 实现高并发主要依靠主从架构，一主多从，一般来说，很多项目其实就足够了，单主用来写入数据，单机几万 QPS，多从用来查询数据，多个从实例可以提供每秒 10w 的 QPS。  
如果想要在实现高并发的同时，容纳大量的数据，那么就需要 redis 集群，使用 redis 集群之后，可以提供每秒几十万的读写并发。

## 3.redis 字典结构，hash 冲突怎么办，rehash，负载因子
* Redis 中的 Hash和 Java的 HashMap 更加相似,都是数组+链表的结构.当发生 hash 碰撞时将会把元素追加到链表上
* 我们先来了解下 hash 的内部结构.第一维是数组,第二维是链表.组成一个 hashtable.
* 在 Java 中 HashMap 扩容是个很耗时的操作,需要去申请新的数组，扩容的成本并不低，因为需要遍历一个时间复杂度为O(n)的数组，并且为其中的每个enrty进行hash计算。加入到新数组中
* 为了追求高性能,Redis 采用了渐进式 rehash 策略.这也是 hash 中最重要的部分.
* redis在扩容的时候执行 rehash 策略会保留新旧两个 hashtable 结构，查询时也会同时查询两个 hashtable.Redis会将旧 hashtable 中的内容一点一点的迁移到新的 hashtable 中,当迁移完成时,就会用新的 hashtable 取代之前的.当 hashtable 移除了最后一个元素之后,这个数据结构将会被删除.
* 数据搬迁的操作放在 hash 的后续指令中,也就是来自客户端对 hash 的指令操作.一旦客户端后续没有指令操作这个 hash.Redis就会使用定时任务对数据主动搬迁.
* 在 Redis 的实现里，扩容缩容有三条规则：1.当 Redis 没有进行 BGSAVE 相关操作，且 负载因子>1的时候进行扩容，扩容为原数组大小的2倍；2.当负载因子>5的时候，强行进行扩容；3.当负载因子<0.1的时候，进行缩容。
## 4.jvm 了解哪些参数，用过哪些指令
* -XX:+UseG1GC：表示启用了G1垃圾收集器；
* -Xms等价于-XX:InitialHeapSize：初始堆大小
* -Xmx等价于-XX:MaxHeapSize：最大堆大小
* -Xss等价于-XX:ThreadStackSize：线程堆栈大小

* jps：用来显示本地的java进程，可以查看本地运行着几个java程序，并显示他们的进程号。
* jmap：dump操作

## 5.zookeeper 的基本原理，数据模型，znode 类型，应用场景有哪些

## 6.一个热榜功能怎么设计，怎么设计缓存，如何保证缓存和数据库的一致性

## 7.容器化技术了解么，主要解决什么问题，原理是什么

## 8.算法：对于一个字符串，计算其中最长回文子串的长度

## 9.redis 集群，为什么是 16384，哨兵模式，选举过程，会有脑裂问题么，raft 算法，优缺点

## 10.jvm 类加载器，自定义类加载器，双亲委派机制，优缺点，tomcat 类加载机制
为什么要设计双亲委派机制：  
①：沙箱安全机制：自己写的java.lang.String.class类不会被加载，这样便可以防止核心 API库被随意篡改  
②：避免类的重复加载：当父亲已经加载了该类时，就没有必要子ClassLoader再加载一 次，保证被加载类的唯一性

## 11.tomcat 热部署，热加载了解么，怎么做到的

## 12.cms 收集器过程，g1 收集器原理，怎么实现可预测停顿的，region 的大小，结构

5. 内存溢出，内存泄漏遇到过么，什么场景产生的，怎么解决的

6. 锁升级过程，轻量锁可以变成偏向锁么，偏向锁可以变成无锁么，自旋锁，对象头结构，锁状态变化过程

7. kafka 重平衡，重启服务怎么保证 kafka 不发生重平衡，有什么方案

8. 怎么理解分布式和微服务，为什么要拆分服务，会产生什么问题，怎么解决这些问题

9. 你们用的什么消息中间件，kafka，为什么用 kafka，高吞吐量，怎么保证高吞吐量的，设计模型，零拷贝

10. 算法 1：给定一个长度为 N 的整形数组 arr，其中有 N 个互不相等的自然数 1-N，请实现 arr 的排序，但是不要把下标 0∼N−1 位置上的数通过直接赋值的方式替换成 1∼N

11. 算法 2：判断一个树是否是平衡二叉树



二面


1. Innodb 的结构了解么，磁盘页和缓存区是怎么配合，以及查找的，缓冲区和磁盘数据不一致怎么办，mysql 突然宕机了会出现数据丢失么

2. redis 字符串实现，sds 和 c 区别，空间预分配

3. redis 有序集合怎么实现的，跳表是什么，往跳表添加一个元素的过程，添加和获取元素，获取分数的时间复杂度，为什么不用红黑树，红黑树有什么特点，左旋右旋操作

4. io 模型了解么，多路复用，selete，poll，epoll，epoll 的结构，怎么注册事件，et 和 lt 模式

5. 怎么理解高可用，如何保证高可用，有什么弊端，熔断机制，怎么实现

6. 对于高并发怎么看，怎么算高并发，你们项目有么，如果有会产生什么问题，怎么解决

7. 项目介绍

8. 算法：给定一个二叉树，请计算节点值之和最大的路径的节点值之和是多少，这个路径的开始节点和结束节点可以是二叉树中的任意节点



三面


1. 项目介绍

2. 算法：求一个 float 数的立方根，牛顿迭代法

3. 什么时候能入职，你对岗位的期望是什么

4. 你还在面其他公司么，目前是一个什么流程