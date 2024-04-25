# Zookeeper笔记

## 参考资料

《从Paxos到Zookeeper分布式一致性原理与实践》

[Apache Zookeeper官网3.5.7](https://zookeeper.apache.org/doc/r3.5.7/index.html)

[JavaGuide-Zookeeper实战](https://javaguide.cn/distributed-system/distributed-process-coordination/zookeeper/zookeeper-in-action.html)

[JavaGuide-Zookeeper入门进阶](https://javaguide.cn/distributed-system/distributed-process-coordination/zookeeper/zookeeper-intro.html)

## Zookeeper入门

### Zookeeper特点

1. 集群是主从架构，读写分离

2. 集群安排奇数台服务器较好，因为有半数zookeeper服务在工作就行

3. 同一个客户端的请求顺序执行

4. 数据更新原子性

5. 实时性

### Zookeeper数据结构

树形结构，类似Linux系统，一个Znode只能存储1M

### Zookeeper的作用

注册中心和配置中心

## Zookeeper涉及的设计模式

观察者模式，就是监听模式，消息队列中常用
