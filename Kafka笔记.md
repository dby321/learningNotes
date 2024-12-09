# Kafka笔记

## 1. Kafka简介

### 1.1 Kafka定义

传统定义：Kafka是一个分布式的基于发布订阅模型的消息队列，主要应用于大数据实时处理领域。

最新定义：Kafka是一个开源的分布式事件流平台，被数千家公司用于高性能数据管道、流分析、数据集成和关键任务应用。

### 1.2 消息队列技术选型

| 维度           | [ActiveMQ](https://github.com/apache/activemq) | [RabbitMQ](https://github.com/rabbitmq/rabbitmq-server) | [RocketMQ](https://github.com/apache/rocketmq) | [Kafka](https://github.com/apache/kafka)                | [Pulsar](https://github.com/apache/pulsar)                   |
| :------------- | :--------------------------------------------- | :------------------------------------------------------ | :--------------------------------------------- | :------------------------------------------------------ | :----------------------------------------------------------- |
| 单机吞吐量     | 较低(万级)                                     | 一般（万级）                                            | 高（十万级）                                   | 高（十万级）                                            | 高（十万级）                                                 |
| 开发语言       | Java                                           | Erlang                                                  | Java                                           | Java/Scala                                              | Java                                                         |
| 维护者         | Apache                                         | Spring                                                  | Apache（Alibaba）                              | Apache（Confluent）                                     | Apache（StreamNative）                                       |
| Star 数量      | 2.3K                                           | 12K                                                     | 21K                                            | 28.2K                                                   | 14.1K                                                        |
| Contributor    | 139                                            | 264                                                     | 527                                            | 1189                                                    | 661                                                          |
| 社区活跃度     | 低                                             | 高                                                      | 较高                                           | 高                                                      | 高                                                           |
| 消费模式       | P2P、Pub-Sub                                   | direct、topic、Headers、fanout                          | 基于 Topic 和 MessageTag 的的 Pub-Sub          | 基于 Topic 的 Pub-Sub                                   | 基于 Topic 的 Pub-Sub，支持独占（exclusive）、共享（shared）、灾备（failover）、key 共享（key_shared）4 种模式 |
| 持久化         | 支持（小）                                     | 支持（小）                                              | 支持（大）                                     | 支持（大）                                              | 支持（大）                                                   |
| 顺序消息       | 不支持                                         | 不支持                                                  | 支持                                           | 支持                                                    | 支持                                                         |
| 性能稳定性     | 好                                             | 好                                                      | 一般                                           | 较差                                                    | 一般                                                         |
| 集群支持       | 主备模式                                       | 复制模式                                                | 主备模式                                       | Leader-Slave 每台既是 master 也是 slave，集群可扩展性强 | 集群模式，broker 无状态，易迁移，支持跨数据中心              |
| 管理界面       | 一般                                           | 较好                                                    | 一般                                           | 无                                                      | 无                                                           |
| 计算和存储分离 | 不支持                                         | 不支持                                                  | 不支持                                         | 不支持                                                  | 支持                                                         |
| AMQP 支持      | 支持                                           | 支持                                                    | 支持                                           | 不完全支持                                              | 不完全支持                                                   |

### 1.3 消息队列的作用

- 流量削峰
- 解耦

- 异步通信

### 1.4 Kafka的发布订阅模式

- 点对点模式
  - 消费者主动拉取数据，消息收到后删除信息
- 发布订阅模式（Kafka）
  - 可以有多个主题（topic）
  - 消息可回溯
  - 消费者相互独立，都可以消费到数据

### 1.5 Kafka的基础架构

> 前置知识好多没学过，学不下去了