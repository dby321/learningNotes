# Spring Data Redis 中文文档

version 3.1.0-SNAPSHOT,2023-09-10

- [前言](https://springdoc.cn/spring-data-redis/#preface)

- 参考文档

  - [7. 简介](https://springdoc.cn/spring-data-redis/#introduction)

  - [8. Spring Data 升级](https://springdoc.cn/spring-data-redis/#upgrading)

  - [9. 为什么选择 Spring Data Redis？](https://springdoc.cn/spring-data-redis/#why-spring-redis)

  - 10. Redis 的支持

    - [10.1. 入门](https://springdoc.cn/spring-data-redis/#redis:setup)
    - [10.2. Redis的要求](https://springdoc.cn/spring-data-redis/#redis:requirements)
    - [10.3. Redis支持高层次视图](https://springdoc.cn/spring-data-redis/#redis:architecture)
    - [10.4. 连接到 Redis](https://springdoc.cn/spring-data-redis/#redis:connectors)
    - [10.5. Redis Sentinel （哨兵）的支持](https://springdoc.cn/spring-data-redis/#redis:sentinel)
    - [10.6. 通过 RedisTemplate 处理对象](https://springdoc.cn/spring-data-redis/#redis:template)
    - [10.7. 以字符串为中心的便利类](https://springdoc.cn/spring-data-redis/#redis:string)
    - [10.8. Serializer](https://springdoc.cn/spring-data-redis/#redis:serializer)
    - [10.9. Hash 映射](https://springdoc.cn/spring-data-redis/#redis.hashmappers.root)
    - [10.10. Redis 消息 (Pub/Sub)](https://springdoc.cn/spring-data-redis/#pubsub)
    - [10.11. Redis Stream](https://springdoc.cn/spring-data-redis/#redis.streams)
    - [10.12. Redis 事务](https://springdoc.cn/spring-data-redis/#tx)
    - [10.13. Pipelining](https://springdoc.cn/spring-data-redis/#pipeline)
    - [10.14. Redis 脚本](https://springdoc.cn/spring-data-redis/#scripting)
    - [10.15. Redis 缓存](https://springdoc.cn/spring-data-redis/#redis:support:cache-abstraction)
    - [10.16. Support Class](https://springdoc.cn/spring-data-redis/#redis:support)
    - [10.17. Observability（可观察性）](https://springdoc.cn/spring-data-redis/#redis.observability)

  - [11. 响应式的 Redis 支持](https://springdoc.cn/spring-data-redis/#redis:reactive)

  - [12. Redis 集群](https://springdoc.cn/spring-data-redis/#cluster)

  - [13. Redis Repository](https://springdoc.cn/spring-data-redis/#redis.repositories)

- [Appendixes](https://springdoc.cn/spring-data-redis/#appendixes)

© 2011-2023 The original authors.

|      | 本站([springdoc.cn](https://springdoc.cn/))中的内容来源于 [spring.io](https://spring.io/) ，原始版权归属于 [spring.io](https://spring.io/)。由 [springdoc.cn](https://springdoc.cn/) 进行翻译，整理。可供个人学习、研究，未经许可，不得进行任何转载、商用或与之相关的行为。 商标声明：Spring 是 Pivotal Software, Inc. 在美国以及其他国家的商标。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

# 前言

Spring Data Redis项目通过使用key/value风格的数据存储（data store），将Spring核心概念应用于解决方案的开发。我们提供一个 “template” 作为发送和接收消息的高级抽象。你可能会注意到与Spring框架中的JDBC支持有相似之处。

本节为开始使用Spring Data Redis模块提供了一个简单易行的指南。

## 1. 学习 Spring

Spring Data使用Spring框架的 [核心](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html) 功能，包括：

- [IoC](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#beans) 容器
- [类型转换系统](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#validation)
- [表达式语言](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#expressions)
- [JMX集成](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/integration.html#jmx)
- [DAO异常的层次结构](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#dao-exceptions).

虽然你不需要知道Spring的API，但理解它们背后的概念是很重要的。至少，你应该熟悉控制反转（IoC）背后的理念，而且你应该熟悉你选择使用的任何IoC容器。

Redis支持的核心功能可以直接使用，而不需要调用Spring容器的IoC服务。这很像 `JdbcTemplate`，它可以 "独立" 使用，不需要Spring容器的任何其他服务。为了利用Spring Data Redis的所有功能，如 repository 支持，你需要配置库的某些部分来使用Spring。

要了解更多关于Spring的信息，你可以参考详细解释 Spring 框架的综合文档。有很多关于这个主题的文章、博客条目和书籍。更多信息请参见 [Spring framework 主页](https://spring.io/docs)。

一般来说，这应该是想要尝试Spring Data Redis的开发者的起点。

## 2. 学习 NoSQL 和 Key/Value 存储

NoSQL存储已经在存储世界中掀起了风暴。这是一个庞大的领域，有大量的解决方案、术语和模式（更糟糕的是，甚至这个术语本身也有多种 [含义](https://www.google.com/search?q=nosoql+acronym)）。虽然有些原则是通用的，但关键是你要在一定程度上熟悉SDR支持的存储。熟悉这些解决方案的最好方法是阅读它们的文档并跟随它们的例子。通常不需要花5到10分钟就能看完，如果你只来自于RDMBS的背景，很多时候这些练习会让你大开眼界。

### 2.1. 尝试示例

你可以在专门的 Spring Data 示例库（ https://github.com/spring-projects/spring-data-keyvalue-examples）中找到各种key/value存储的示例。对于Spring Data Redis，你应该特别注意 `retwisj` 示例，这是一个建立在Redis之上的 Twitter 克隆，可以在本地运行，也可以部署到云端。

## 3. 要求

Spring Data Redis 的二进制文件需要 JDK 17 及以上， [Spring Framework](https://spring.io/projects/spring-framework/) 6.0.4 及以上。

在key/value存储方面，需要 [Redis](https://redis.io/) 2.6.x 或更高版本。Spring Data Redis目前是针对最新的6.0版本进行测试。

## 4. 额外的帮助资源

学习一个新的框架并不总是简单明了的。在本节中，我们试图提供一个我们认为是简单易行的指南，让大家从Spring Data Redis模块开始学习。然而，如果你遇到问题或需要建议，请随时使用以下链接。

- Spring Boot 中文社区

  欢迎到 [Spring Boot 中文社区](https://springboot.io/) 进行讨论，分享。

- 社区论坛

  [Stack Overflow](https://stackoverflow.com/questions/tagged/spring-data) 上的 Spring Data 是所有Spring Data（不仅仅是Document）用户分享信息和相互帮助的一个标签。请注意，只有在发帖时才需要注册。

- 专业的支持

  [Pivotal Sofware, Inc.](https://pivotal.io/) 是Spring Data和Spring背后的公司，可以提供专业的、来自源头的支持，并保证响应时间。

## 5. 追踪发展

有关 Spring Data 源代码库、夜间构建和快照工件的信息，请参见Spring Data [主页](https://spring.io/spring-data)。

你可以通过在 Stack Overflow 的 [spring-data](https://stackoverflow.com/questions/tagged/spring-data) 或 [spring-data-redis](https://stackoverflow.com/questions/tagged/spring-data-redis) 上与开发者互动，帮助Spring Data更好地满足Spring社区的需求。

如果你遇到一个bug或想提出改进建议（包括对本文档），请在 [Github](https://github.com/spring-projects/spring-data-redis/issues/new) 上创建一个 ticket。

要了解Spring生态体系的最新消息和公告，请订阅Spring社区 [门户网站](https://spring.io/)。

最后，你可以在Twitter上关注Spring [博客](https://spring.io/blog/) 或项目团队（ [@SpringData](https://twitter.com/SpringData)）。

## 6. 依赖

由于各个Spring Data模块的起始日期不同，它们中的大多数都有不同的主要和次要版本号。找到兼容的模块最简单的方法是依靠Spring Data Release Train BOM，我们在发行时定义了兼容版本。在Maven项目中，你可以在POM的 `<dependencyManagement />` 部分声明这一依赖，如下所示。

Example 1. Using the Spring Data release train BOM

```xml
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.springframework.data</groupId>
      <artifactId>spring-data-bom</artifactId>
      <version>2023.0.0-SNAPSHOT</version>
      <scope>import</scope>
      <type>pom</type>
    </dependency>
  </dependencies>
</dependencyManagement>
```

当前发布的 train version 是 `2023.0.0-SNAPSHOT`。train version 使用模式为 `YYY.MINOR.MICRO` 的 [calver](https://calver.org/)。对于GA版本和服务版本，版本名称遵循 `${calver}`，对于所有其他版本，版本名称遵循以下模式：`${calver}-${modifier}`，其中 `modifier` 可以是以下之一。

- `SNAPSHOT`: 当前快照
- `M1`, `M2`, 以此类推: 里程碑
- `RC1`, `RC2`, 以此类推: 候选发布

你可以在我们的 [Spring Data示例库](https://github.com/spring-projects/spring-data-examples/tree/master/bom) 中找到一个使用BOM的工作实例。有了这些，你可以在 `<dependencies />` 块中声明你想使用的没有版本的Spring Data模块，如下所示。

Example 2. Declaring a dependency to a Spring Data module

```xml
<dependencies>
  <dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-jpa</artifactId>
  </dependency>
<dependencies>
```

### 6.1. 使用Spring Boot的依赖管理

Spring Boot会为你选择一个最新版本的Spring Data模块。如果你仍然想升级到较新的版本，将 `spring-data-releasetrain.version` 属性设置为你想使用的[train version 和 iteration](https://springdoc.cn/spring-data-redis/#dependencies.train-version)。

### 6.2. Spring Framework

当前版本的Spring Data模块需要Spring框架 6.0.4 或更高的版本。这些模块也可能在该次要版本的较早的错误修复版本中工作。但是，强烈建议使用那一代中的最新版本。

# 参考文档

## 7. 简介

### 7.1. 文档结构

参考文档的这一部分解释了Spring Data Redis所提供的核心功能。它解释了key/value模块的概念和语义以及各种存储命名空间（store namespace）的语法。有关key/value存储、Spring或Spring Data示例的介绍，请参见 [学习 NoSQL 和 Key/Value 存储](https://springdoc.cn/spring-data-redis/#get-started:first-steps:nosql)。本文档仅提及Spring Data Redis支持，并假设用户熟悉 key/value 存储和Spring概念。

“[Redis 的支持](https://springdoc.cn/spring-data-redis/#redis)” 介绍了Redis模块的特点。

“[Redis Repository](https://springdoc.cn/spring-data-redis/#redis.repositories)” 介绍了对Redis的 repository 支持。

本文档是Spring Data Redis（SDR）支持的参考指南。

## 8. Spring Data 升级

关于如何从Spring Data的早期版本升级的说明在项目 [wiki](https://github.com/spring-projects/spring-data-commons/wiki)上提供。按照 [发布说明部分](https://github.com/spring-projects/spring-data-commons/wiki#release-notes) 的链接，找到你要升级的版本。

升级说明总是在发行说明中的第一项。如果你落后一个以上的版本，请确保你也查看你跳过的版本的发行说明。

### 8.1. 接下来要读什么？

一旦你决定升级你的应用程序，你可以在文档的其余部分找到有关具体功能的详细信息。你可以在本文档的末尾找到针对主要版本迁移的 [迁移指南](https://springdoc.cn/spring-data-redis/#redis.upgrading)。

Spring Data的文档是针对该版本的，所以你在这里找到的任何信息都将包含该版本的最新变化。

## 9. 为什么选择 Spring Data Redis？

Spring框架是领先的全堆栈Java/JEE应用框架。它提供了一个轻量级的容器和一个非侵入式的编程模型，通过使用依赖注入、AOP和可移植的服务抽象来实现。

[NoSQL](https://en.wikipedia.org/wiki/NoSQL) 存储系统为经典的RDBMS提供了横向可扩展性和速度的替代方案。在实施方面，key/value存储代表了NoSQL空间中最大（和最古老）的成员之一。

Spring Data Redis（SDR）框架使编写使用Redis key/value 存储的Spring应用程序变得很容易，它通过Spring出色的基础设施支持，消除了与存储（store）互动所需的冗余任务和模板代码。

## 10. Redis 的支持

Spring Data 支持的 key/value 存储之一是Redis。引用 [Redis](https://redis.io/) 项目主页的内容：

> Redis是一个高级key/value存储。它类似于memcached，但数据集是不稳定（volatile）的，值可以是字符串，与memcached完全一样，也可以是列表、集合和有序集合。所有这些数据类型都可以通过原子操作来操作，如push/pop元素、添加/删除元素、执行服务器端的并集、交集、差集等等。Redis支持不同类型的排序能力。

Spring Data Redis为Spring应用程序提供了简单的配置和对Redis的访问。它为与存储（store）的互动提供了低级和高级的抽象，将用户从基础设施的关注中解放出来。

### 10.1. 入门

建立工作环境的一个简单方法是在 [STS](https://spring.io/tools/) 中创建一个基于Spring的项目。

首先，你需要建立一个正在运行的Redis服务器。

在STS中创建一个Spring项目。

1. Go to File → New → Spring Template Project → Simple Spring Utility Project, 并在出现提示时按Yes。然后输入一个项目和一个包的名称，如 `org.spring.redis.example`。

在 pom.xml 文件的 `dependencies` 元素中添加以下内容。

+

```xml
<dependencies>

  <!-- other dependency elements omitted -->

  <dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-redis</artifactId>
    <version>3.1.0-SNAPSHOT</version>
  </dependency>

</dependencies>
```

1. 将pom.xml中Spring的版本改为：

   ```xml
   <spring.framework.version>6.0.4</spring.framework.version>
   ```

2. 在你的 `pom.xml` 中添加以下Maven的Spring Milestone 资源库的位置，使其位于 `<dependencies/>` 元素的同一级别。

   ```xml
   <repositories>
     <repository>
       <id>spring-milestone</id>
       <name>Spring Maven MILESTONE Repository</name>
       <url>https://repo.spring.io/libs-milestone</url>
     </repository>
   </repositories>
   ```

repository 也可 [在此浏览](https://repo.spring.io/milestone/org/springframework/data/)。

### 10.2. Redis的要求

Spring Redis需要Redis 2.6或以上版本，Spring Data Redis与 [Lettuce](https://github.com/lettuce-io/lettuce-core) 和 [Jedis](https://github.com/xetorthio/jedis)（两个流行的Redis开源Java库）整合。

### 10.3. Redis支持高层次视图

Redis支持提供了几个组件。对于大多数任务，高层抽象和支持服务是最好的选择。注意，在任何时候，你都可以在各层之间移动。例如，你可以得到一个低级别的连接（甚至是 native 库）来直接与Redis通信。

### 10.4. 连接到 Redis

使用Redis和Spring的首要任务之一是通过IoC容器连接到store。要做到这一点，需要一个Java connector（或 binding）。无论你选择哪种库，你都只需要使用一套Spring Data Redis API（在所有 connector 中表现一致）：`org.springframework.data.redis.connection` 包及其 `RedisConnection` 和 `RedisConnectionFactory` 接口，用于处理和检索到Redis的活动连接。

#### 10.4.1. RedisConnection 和 RedisConnectionFactory

`RedisConnection` 提供了Redis通信的核心构建模块，因为它处理与Redis后端的通信。它还自动将底层连接库的异常转化为Spring一致的DAO异常 [hierarchy](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#dao-exceptions)，因此你可以在不改变代码的情况下切换连接器（connector），因为操作语义保持不变。

|      | 对于需要本地库API的情况，`RedisConnection` 提供了一个专门的方法（`getNativeConnection`），返回用于通信的原始底层对象。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

活动的 `RedisConnection` 对象是通过 `RedisConnectionFactory` 创建的。此外，该工厂作为 `PersistenceExceptionTranslator` 对象，意味着一旦声明，它们可以让你做透明的异常转换。例如，你可以通过使用 `@Repository` 注解和AOP来做异常转换。欲了解更多信息，请参见Spring框架文档中的专门 [章节](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#orm-exception-translation)。

|      | 根据底层配置，工厂可以返回一个新的连接或现有的连接（当使用池或共享本地连接时）。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

使用 `RedisConnectionFactory` 的最简单方法是通过IoC容器配置适当的connector，并将其注入要使用的类中。

不幸的是，目前并非所有的连接器都支持所有的Redis功能。当在连接API上调用底层库不支持的方法时，会抛出一个 `UnsupportedOperationException`。下面的概述解释了各个Redis连接器所支持的功能。

| 支持的功能                                                   | Lettuce                                                      | Jedis                                                        |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 单机连接                                                     | X                                                            | X                                                            |
| [主/从 连接](https://springdoc.cn/spring-data-redis/#redis:write-to-master-read-from-replica) | X                                                            |                                                              |
| [Redis 哨兵（Sentinel）](https://springdoc.cn/spring-data-redis/#redis:sentinel) | Master Lookup, Sentinel Authentication, Replica Reads        | Master Lookup                                                |
| [Redis 集群](https://springdoc.cn/spring-data-redis/#cluster) | Cluster Connections, Cluster Node Connections, Replica Reads | Cluster Connections, Cluster Node Connections                |
| Transport Channels                                           | TCP, OS-native TCP (epoll, kqueue), Unix Domain Sockets      | TCP                                                          |
| 连接池                                                       | X (using `commons-pool2`)                                    | X (using `commons-pool2`)                                    |
| 其他连接特性                                                 | Singleton-connection sharing for non-blocking commands       | Pipelining and Transactions mutually exclusive. Cannot use server/connection commands in pipeline/transactions. |
| SSL 支持                                                     | X                                                            | X                                                            |
| [Pub/Sub](https://springdoc.cn/spring-data-redis/#pubsub)    | X                                                            | X                                                            |
| [Pipelining（管道）](https://springdoc.cn/spring-data-redis/#pipeline) | X                                                            | X (Pipelining and Transactions mutually exclusive)           |
| [Transactions（事务）](https://springdoc.cn/spring-data-redis/#tx) | X                                                            | X (Pipelining and Transactions mutually exclusive)           |
| Datatype support（数据类型支持）                             | Key, String, List, Set, Sorted Set, Hash, Server, Stream, Scripting, Geo, HyperLogLog | Key, String, List, Set, Sorted Set, Hash, Server, Scripting, Geo, HyperLogLog |
| [响应式（非阻塞） API](https://springdoc.cn/spring-data-redis/#redis:reactive) | X                                                            |                                                              |

#### 10.4.2. 配置 Lettuce Connector

[Lettuce](https://github.com/lettuce-io/lettuce-core) 是一个基于 [Netty](https://netty.io/) 的开源 connector，由Spring Data Redis通过 `org.springframework.data.redis.connection.lettuce` 包支持。

在pom.xml文件的 `dependencies` 元素中加入以下内容。

```xml
<dependencies>

  <!-- other dependency elements omitted -->

  <dependency>
    <groupId>io.lettuce</groupId>
    <artifactId>lettuce-core</artifactId>
    <version>6.2.2.RELEASE</version>
  </dependency>

</dependencies>
```

下面的例子显示了如何创建一个新的 Lettuce connection factory。

```java
@Configuration
class AppConfig {

  @Bean
  public LettuceConnectionFactory redisConnectionFactory() {

    return new LettuceConnectionFactory(new RedisStandaloneConfiguration("server", 6379));
  }
}
```

还有一些Lettuce特有的连接参数可以调整。默认情况下，由 `LettuceConnectionFactory` 创建的所有 `LettuceConnection` 实例在所有非阻塞和非事务操作中共享同一个线程安全的本地连接。要想每次都使用一个专用的连接，请将 `shareNativeConnection` 设置为 `false`。如果 `shareNativeConnection` 被设置为 `false`，`LettuceConnectionFactory` 也可以被配置为使用 `LettucePool` 来池化阻塞和事务性连接或所有连接。

Lettuce集成了Netty的 [native transports](https://netty.io/wiki/native-transports.html)，让你使用 Unix domain sockets 来与Redis通信。请确保包括与你的运行环境相匹配的适当的 native transport 依赖。下面的例子显示了如何为 `/var/run/redis.sock` 的Unix域套接字创建一个 Lettuce Connection factory。

```java
@Configuration
class AppConfig {

  @Bean
  public LettuceConnectionFactory redisConnectionFactory() {

    return new LettuceConnectionFactory(new RedisSocketConfiguration("/var/run/redis.sock"));
  }
}
```

|      | Netty目前支持epoll（Linux）和kqueue（BSD/MacOS）接口的操作系统 native transport。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 10.4.3. 配置 Jedis Connector

[Jedis](https://github.com/redis/jedis) 是一个社区驱动的Connector，由Spring Data Redis模块通过 `org.springframework.data.redis.connection.jedis` 包支持。

在pom.xml文件的 `dependencies` 元素中加入以下内容。

```xml
<dependencies>

  <!-- other dependency elements omitted -->

  <dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>4.3.1</version>
  </dependency>

</dependencies>
```

在其最简单的形式中，Jedis的配置看起来如下。

```java
@Configuration
class AppConfig {

  @Bean
  public JedisConnectionFactory redisConnectionFactory() {
    return new JedisConnectionFactory();
  }
}
```

然而，对于生产使用，你可能想调整一些设置，如host或password，如下面的例子所示。

```java
@Configuration
class RedisConfiguration {

  @Bean
  public JedisConnectionFactory redisConnectionFactory() {

    RedisStandaloneConfiguration config = new RedisStandaloneConfiguration("server", 6379);
    return new JedisConnectionFactory(config);
  }
}
```

#### 10.4.4. 读写分离（写主节点，读从节点）

Redis Master/Replica 设置 — 没有自动故障切换（自动故障切换见：[Sentinel](https://springdoc.cn/spring-data-redis/#redis:sentinel)） — 不仅允许数据安全地存储在更多节点上。它还允许，通过使用 [Lettuce](https://springdoc.cn/spring-data-redis/#redis:connectors:lettuce)，从 replica 中读取数据，同时将写入推送到 master。你可以通过使用 `LettuceClientConfiguration` 来设置要使用的读/写策略，如下面的例子所示。

```java
@Configuration
class WriteToMasterReadFromReplicaConfiguration {

  @Bean
  public LettuceConnectionFactory redisConnectionFactory() {

    LettuceClientConfiguration clientConfig = LettuceClientConfiguration.builder()
      .readFrom(REPLICA_PREFERRED)
      .build();

    RedisStandaloneConfiguration serverConfig = new RedisStandaloneConfiguration("server", 6379);

    return new LettuceConnectionFactory(serverConfig, clientConfig);
  }
}
```

|      | 对于通过 `INFO` 命令报告非公共地址的环境（例如使用AWS时），使用 `RedisStaticMasterReplicaConfiguration` 而不是 `RedisStandaloneConfiguration`。请注意，`RedisStaticMasterReplicaConfiguration` 不支持 Pub/Sub，因为缺少跨单个服务器的 Pub/Sub 消息传播。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 10.5. Redis Sentinel （哨兵）的支持

为了处理高可用性的 Redis，Spring Data Redis 支持 [Redis Sentinel](https://redis.io/topics/sentinel)，使用 `RedisSentinelConfiguration`，如下例所示。

```java
/**
 * Lettuce
 */
@Bean
public RedisConnectionFactory lettuceConnectionFactory() {
  RedisSentinelConfiguration sentinelConfig = new RedisSentinelConfiguration()
  .master("mymaster")
  .sentinel("127.0.0.1", 26379)
  .sentinel("127.0.0.1", 26380);
  return new LettuceConnectionFactory(sentinelConfig);
}

/**
 * Jedis
 */
@Bean
public RedisConnectionFactory jedisConnectionFactory() {
  RedisSentinelConfiguration sentinelConfig = new RedisSentinelConfiguration()
  .master("mymaster")
  .sentinel("127.0.0.1", 26379)
  .sentinel("127.0.0.1", 26380);
  return new JedisConnectionFactory(sentinelConfig);
}
```

|      | `RedisSentinelConfiguration` 也可以用 `PropertySource` 定义，它可以让你设置以下属性。配置属性`spring.redis.sentinel.master`: 主节点的名称。`spring.redis.sentinel.nodes`: 以逗号分隔的主机：host:port 对`spring.redis.sentinel.username`: 与Redis Sentinel进行身份验证时应用的用户名（需要Redis 6）。`spring.redis.sentinel.password`: 在与Redis Sentinel进行身份验证时要应用的密码 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

有时，需要与其中一个哨兵直接互动。使用 `RedisConnectionFactory.getSentinelConnection()` 或 `RedisConnection.getSentinelCommands()` 可以让你访问配置的第一个活动哨兵。

### 10.6. 通过 RedisTemplate 处理对象

大多数用户可能会使用 `RedisTemplate` 及其相应的包，`org.springframework.data.redis.core`。事实上，template 是Redis模块的中心类，由于其丰富的功能集。该template为Redis的交互提供了一个高层次的抽象。`RedisConnection` 提供了接受和返回二进制值（`byte` 数组）的低级方法，模板负责序列化和连接管理，将用户从处理这些细节中解放出来。

此外，模板还提供了操作视图（operations view）（遵循Redis命令 [参考](https://redis.io/commands) 中的分组），这些视图提供了丰富的、生成的接口，用于针对某种类型或某种key（通过 `KeyBound` 接口）进行操作，如下表所述。

| 接口                    | 说明                                                    |
| :---------------------- | :------------------------------------------------------ |
| *key 类型的操作*        |                                                         |
| `GeoOperations`         | Redi s的 geospatial 操作，如 `GEOADD`、`GEORADIUS` 等。 |
| `HashOperations`        | Redis hash 操作                                         |
| `HyperLogLogOperations` | Redis HyperLogLog 操作，如 `PFADD`，`PFCOUNT`，…        |
| `ListOperations`        | Redis list 操作                                         |
| `SetOperations`         | Redis set 操作                                          |
| `ValueOperations`       | Redis string (或 value) 操作                            |
| `ZSetOperations`        | Redis zset (或 sorted set) 操作                         |
| *Key 绑定 操作*         |                                                         |
| `BoundGeoOperations`    | Redis geospatial key 绑定操作                           |
| `BoundHashOperations`   | Redis hash key 绑定操作。                               |
| `BoundKeyOperations`    | Redis key 绑定操作                                      |
| `BoundListOperations`   | Redis list key 绑定操作。                               |
| `BoundSetOperations`    | Redis set key 绑定操作。                                |
| `BoundValueOperations`  | Redis string (or value) key 绑定操作。                  |
| `BoundZSetOperations`   | Redis zset (or sorted set) key 绑定操作。               |

一旦配置好，该 template 是线程安全的，可以在多个实例中重复使用。

`RedisTemplate` 使用基于Java的序 serializer 进行大部分操作。这意味着template写入或读取的任何对象都是通过Java进行序列化和反序列化的。你可以改变template上的序列化机制，Redis模块提供了几种实现，可在 `org.springframework.data.redis.serializer` 包中找到。更多信息请参见 [Serializer](https://springdoc.cn/spring-data-redis/#redis:serializer)。你也可以将任何一个serializer设置为空，并通过将 `enableDefaultSerializer` 属性设置为 `false` 来使用 `RedisTemplate` 的原始字节数组。注意，template 要求所有的key都是非 null 的。然而，只要底层 serializer 接受它们，值就可以是 null 的。阅读每个 serializer 的 Javadoc 以了解更多信息。

对于需要某种 template view 的情况，将 view 声明为依赖关系并注入模板。容器会自动进行转换，消除对 `opsFor[X]` 的调用，如下面的例子所示。

Java

XML

```java
@Configuration
class MyConfig {

  @Bean
  LettuceConnectionFactory redisConnectionFactory() {
    return new LettuceConnectionFactory();
  }

  @Bean
  RedisTemplate<String, String> redisTemplate(RedisConnectionFactory redisConnectionFactory) {

    RedisTemplate<String, String> template = new RedisTemplate<>();
    template.setConnectionFactory(redisConnectionFactory);
    return template;
  }
}
public class Example {

  // inject the actual template
  @Autowired
  private RedisTemplate<String, String> template;

  // inject the template as ListOperations
  @Resource(name="redisTemplate")
  private ListOperations<String, String> listOps;

  public void addLink(String userId, URL url) {
    listOps.leftPush(userId, url.toExternalForm());
  }
}
```

### 10.7. 以字符串为中心的便利类

由于存储在Redis中的key和value是很常见的 `java.lang.String`，Redis模块提供了两个 `RedisConnection` 和 `RedisTemplate` 的扩展，分别是 `StringRedisConnection`（及其 `DefaultStringRedisConnection` 实现）和 `StringRedisTemplate`，作为密集字符串操作的方便的一站式解决方案。除了与 `String` key 绑定外，template和connection还使用下面的 `StringRedisSerializer`，这意味着存储的key和value是人类可读的（假设Redis和你的代码都使用相同的编码）。下面的列表显示了一个例子。

Java

XML

```java
@Configuration
class MyConfig {

  @Bean
  LettuceConnectionFactory redisConnectionFactory() {
    return new LettuceConnectionFactory();
  }

  @Bean
  StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory) {

    StringRedisTemplate template = new StringRedisTemplate();
    template.setConnectionFactory(redisConnectionFactory);
    return template;
  }
}
public class Example {

  @Autowired
  private StringRedisTemplate redisTemplate;

  public void addLink(String userId, URL url) {
    redisTemplate.opsForList().leftPush(userId, url.toExternalForm());
  }
}
```

与其他Spring template一样，`RedisTemplate` 和 `StringRedisTemplate` 让你通过 `RedisCallback` 接口直接与 Redis 对话。这个功能给了你完全的控制权，因为它直接与 `RedisConnection` 对话。注意，当使用 `StringRedisTemplate` 时，回调会收到一个 `StringRedisConnection` 的实例。下面的例子展示了如何使用 `RedisCallback` 接口。

```java
public void useCallback() {

  redisTemplate.execute(new RedisCallback<Object>() {
    public Object doInRedis(RedisConnection connection) throws DataAccessException {
      Long size = connection.dbSize();
      // Can cast to StringRedisConnection if using a StringRedisTemplate
      ((StringRedisConnection)connection).set("key", "value");
    }
   });
}
```

### 10.8. Serializer

从框架的角度来看，存储在Redis中的数据只是字节。虽然Redis本身支持各种类型，但在大多数情况下，这些类型指的是数据的存储方式，而不是它代表的内容。由用户来决定信息是否被转换成字符串或任何其他对象。

在 Spring Data 中，用户（自定义）类型和原始数据之间的转换（反之亦然）由 `org.springframework.data.redis.serializer` 包中的 Redis 处理。

这个包包含两种类型的 serializer，顾名思义，它们负责序列化过程。

- 基于 `RedisSerializer` 的双向序列化器（serializer）。
- 使用 `RedisElementReader` 和 `RedisElementWriter` 的 Element reader 和 writer。

这些变体之间的主要区别是，`RedisSerializer` 主要序列化到 `byte[]`，而 reader 和 writer 使用 `ByteBuffer`。

有多种实现方式（包括本文档中已经提到的两种）。

- `JdkSerializationRedisSerializer`，它被默认用于 `RedisCache` 和 `RedisTemplate`。
- `StringRedisSerializer`.

然而，可以通过Spring [OXM](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#oxm) 支持使用 `OxmSerializer` 进行 Object/XML 映射，或者使用 `Jackson2JsonRedisSerializer` 或 `GenericJackson2JsonRedisSerializer` 以 [JSON](https://en.wikipedia.org/wiki/JSON) 格式存储数据。

请注意，存储格式不只限于value。它可以用于key、value或hash value，没有任何限制。

|      | 默认情况下，`RedisCache` 和 `RedisTemplate` 被配置为使用Java序列化。众所周知，Java原生序列化允许运行由利用脆弱的库和类注入未经验证的字节码的 payload 引起的远程代码。在反序列化步骤中，被操纵的输入可能导致不需要的代码在应用程序中运行。因此，不要在不受信任的环境中使用序列化。一般来说，我们强烈建议使用任何其他的消息格式（如JSON）来代替。如果你担心Java序列化带来的安全漏洞，可以考虑核心JVM层面的通用序列化过滤机制，该机制最初是为JDK 9开发的，但已回传到JDK 8、7和6。[过滤传入的序列化数据](https://blogs.oracle.com/java-platform-group/entry/incoming_filter_serialization_data_a)[JEP 290](https://openjdk.java.net/jeps/290)[OWASP：不受信任的数据的反序列化](https://www.owasp.org/index.php/Deserialization_of_untrusted_data) |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 10.9. Hash 映射

数据可以通过使用Redis中的各种数据结构来存储。`Jackson2JsonRedisSerializer` 可以转换 [JSON](https://en.wikipedia.org/wiki/JSON) 格式的对象。理想情况下，JSON可以通过使用普通key来存储为一个value。你可以通过使用Redis的 hash 来实现结构化对象的更复杂的映射。Spring Data Redis提供了各种将数据映射为 hash 的策略（取决于用例）。

- 通过使用 `HashOperations` 和一个 [serializer](https://springdoc.cn/spring-data-redis/#redis:serializer) 直接映射
- 使用 [Redis Repository](https://springdoc.cn/spring-data-redis/#redis.repositories)
- 使用 `HashMapper` 和 `HashOperations`

#### 10.9.1. Hash Mapper

哈希映射器（Hash Mapper）是将 Map 对象转换为 `Map<K, V>` 并返回的 converter。 `HashMapper` 旨在与 Redis Hash 一起使用。

有多种实现方式可供选择。

- `BeanUtilsHashMapper` 使用 Spring 的 [BeanUtils](https://docs.spring.io/spring-framework/docs/6.0.4/javadoc-api/org/springframework/beans/BeanUtils.html)。
- `ObjectHashMapper` 使用 [对象（Object）到哈希的映射](https://springdoc.cn/spring-data-redis/#redis.repositories.mapping).
- [`Jackson2HashMapper`](https://springdoc.cn/spring-data-redis/#redis.hashmappers.jackson2) 使用 [FasterXML Jackson](https://github.com/FasterXML/jackson).

下面的例子显示了实现哈希映射（hash mapping）的一种方法。

```java
public class Person {
  String firstname;
  String lastname;

  // …
}

public class HashMapping {

  @Autowired
  HashOperations<String, byte[], byte[]> hashOperations;

  HashMapper<Object, byte[], byte[]> mapper = new ObjectHashMapper();

  public void writeHash(String key, Person person) {

    Map<byte[], byte[]> mappedHash = mapper.toHash(person);
    hashOperations.putAll(key, mappedHash);
  }

  public Person loadHash(String key) {

    Map<byte[], byte[]> loadedHash = hashOperations.entries("key");
    return (Person) mapper.fromHash(loadedHash);
  }
}
```

#### 10.9.2. Jackson2HashMapper

`Jackson2HashMapper` 通过使用 [FasterXML Jackson](https://github.com/FasterXML/jackson) 为 domain 对象提供 Redis Hash 映射。`Jackson2HashMapper` 可以将顶层属性映射为 Hash 字段名，并且可以选择将结构扁平化。简单类型映射为简单的值。复杂的类型（嵌套的对象、Collection、Map等等）被表示为嵌套的JSON。

扁平化为所有嵌套的属性创建单独的哈希条目（entry），并尽可能地将复杂类型解析为简单类型。

请考虑以下类和它所包含的数据结构。

```java
public class Person {
  String firstname;
  String lastname;
  Address address;
  Date date;
  LocalDateTime localDateTime;
}

public class Address {
  String city;
  String country;
}
```

下表显示了前述类中的数据在正常映射中是如何出现的。

| Hash 字段     | 值                                                     |
| :------------ | :----------------------------------------------------- |
| firstname     | `Jon`                                                  |
| lastname      | `Snow`                                                 |
| address       | `{ "city" : "Castle Black", "country" : "The North" }` |
| date          | `1561543964015`                                        |
| localDateTime | `2018-01-02T12:13:14`                                  |

下表显示了前述类中的数据将如何出现在扁平化映射中。

| Hash 字段       | 值                    |
| :-------------- | :-------------------- |
| firstname       | `Jon`                 |
| lastname        | `Snow`                |
| address.city    | `Castle Black`        |
| address.country | `The North`           |
| date            | `1561543964015`       |
| localDateTime   | `2018-01-02T12:13:14` |

|      | 扁平化要求所有的属性名称不与 JSON 路径相干扰。当你使用扁平化时，不支持在映射key中使用点或括号或作为属性名。产生的哈希值不能被映射回一个对象。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | `java.util.Date` 和 `java.util.Calendar` 用毫秒表示。如果 `jackson-datatype-jsr310` 在类的路径上，JJSR-310 Date/Time 类型被序列化为其 `toString` 形式。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 10.10. Redis 消息 (Pub/Sub)

Spring Data为Redis提供了专门的消息传递集成，在功能和命名上与Spring Framework中的JMS集成类似。

Redis的消息传递可以大致分为两个方面的功能。

- 发布或生产消息
- 订阅或消费消息

这是一个通常被称为发布/订阅（简称Pub/Sub）模式的例子。`RedisTemplate` 类用于消息生产。对于类似于Java EE的消息驱动Bean风格的异步接收，Spring Data提供了一个专门的消息监听器容器，用于创建消息驱动POJO（MDP），对于同步接收，则是 `RedisConnection` contract。

`org.springframework.data.redis.connection` 和 `org.springframework.data.redis.listener` 包提供了Redis消息传递的核心功能。

#### 10.10.1. 发布（生产消息）

要发布一个消息，你可以像其他操作一样，使用低级别的 `RedisConnection` 或高级别的 `RedisTemplate`。这两个实体都提供 `publish` 方法，它接受消息和目标通道（channel）作为参数。`RedisConnection` 需要原始数据（字节数组），而 `RedisTemplate` 允许将任意对象作为消息传入，如下面的例子所示。

```java
// send message through connection RedisConnection con = ...
byte[] msg = ...
byte[] channel = ...
con.publish(msg, channel); // send message through RedisTemplate
RedisTemplate template = ...
Long numberOfClients = template.convertAndSend("hello!", "world");
```

#### 10.10.2. 订阅（消费消息）

在接收方面，人们可以通过直接命名或使用 pattern 匹配来订阅一个或多个频道。后一种方法非常有用，因为它不仅可以用一个命令创建多个订阅，而且还可以监听订阅时尚未创建的频道（只要它们符合 pattern）。

在底层，`RedisConnection` 提供了 `subscribe` 和 `pSubscribe` 方法，分别映射Redis命令，用于按通道或按 pattern 进行订阅。注意，多个通道或 pattern 可以作为参数使用。要改变一个连接的订阅或查询它是否在监听，`RedisConnection` 提供了 `getSubscription` 和 `isSubscribed` 方法。

|      | Spring Data Redis的订阅命令是阻塞的。也就是说，在一个连接上调用订阅会导致当前线程阻塞，因为它开始等待消息。只有当订阅被取消时，线程才会被释放，这发生在另一个线程在同一连接上调用 `unsubscribe` 或 `pUnsubscribe` 时。参见 “[消息监听器容器](https://springdoc.cn/spring-data-redis/#redis:pubsub:subscribe:containers)”（本文稍后），以了解这个问题的解决方案。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

如前所述，一旦订阅，一个连接就开始等待消息。只有添加新的订阅、修改现有的订阅和取消现有的订阅的命令才被允许。调用除 `subscribe`、`pSubscribe`、`unsubscribe` 或 `pUnsubscribe` 以外的任何命令都会抛出一个异常。

为了订阅消息，人们需要实现 `MessageListener` 回调。每次有新的消息到来，回调就会被调用，用户代码就会通过 `onMessage` 方法运行。该接口不仅可以访问实际的消息，还可以访问它所接收的通道，以及订阅者用来匹配该通道的pattern（如果有的话）。这些信息让被调用者不仅可以通过内容来区分不同的消息，还可以检查其他细节。

##### 消息监听器容器

由于其阻塞性，低级别的订阅没有吸引力，因为它需要对每一个监听器进行连接和线程管理。为了缓解这个问题，Spring Data提供了 `RedisMessageListenerContainer`，它完成了所有繁重的工作。如果你熟悉EJB和JMS，你应该发现这些概念很熟悉，因为它被设计为尽可能接近Spring框架及其消息驱动的POJO（MDP）中的支持。

`RedisMessageListenerContainer` 作为一个消息监听器容器。它被用来接收来自Redis通道的消息，并驱动被注入其中的 `MessageListener` 实例。监听器容器负责所有消息接收的线程，并将其分配到监听器中进行处理。消息监听器容器是MDP和消息提供者之间的中介，负责注册接收消息、资源获取和释放、异常转换等工作。这让你作为一个应用开发者可以编写与接收消息相关的（可能是复杂的）业务逻辑（并对其做出反应），并将模板式的Redis基础设施问题委托给框架。

一个 `MessageListener` 可以额外地实现 `SubscriptionListener`，以便在订阅/取消订阅确认时接收通知。在同步调用时，监听订阅通知是很有用的。

此外，为了最大限度地减少应用程序的足迹，`RedisMessageListenerContainer` 允许一个连接和一个线程被多个监听器共享，即使它们不共享一个订阅。因此，无论一个应用程序跟踪多少个监听器或通道，在其整个生命周期中，运行时成本都是一样的。此外，该容器允许在运行时改变配置，因此你可以在应用程序运行时添加或删除监听器，而不需要重新启动。此外，该容器使用懒加载的订阅方法，只在需要时使用 `RedisConnection`。如果所有的监听器都被取消订阅，就会自动进行清理，并释放线程。

为了帮助解决消息的异步性，容器需要一个 `java.util.concurrent.Executor`（或Spring的 `TaskExecutor` ）来调度消息。根据负载、监听器的数量或运行时环境的不同，你应该改变或调整执行器以更好地满足你的需求。特别是在管理环境中（如应用服务器），强烈建议挑选一个合适的 `TaskExecutor` 来利用其运行时的优势。

##### MessageListenerAdapter

`MessageListenerAdapter` 类是Spring异步消息传递支持的最后一个组件。简而言之，它可以让你将几乎所有的类作为MDP来公开（尽管有一些限制）。

考虑下面的接口定义。

```java
public interface MessageDelegate {
  void handleMessage(String message);
  void handleMessage(Map message);
  void handleMessage(byte[] message);
  void handleMessage(Serializable message);
  // pass the channel/pattern as well
  void handleMessage(Serializable message, String channel);
 }
```

请注意，尽管该接口没有扩展 `MessageListener` 接口，但它仍然可以通过使用 `MessageListenerAdapter` 类作为MDP使用。还请注意各种消息处理方法是如何根据它们可以接收和处理的各种 `Message` 类型的内容进行强类型化的。此外，发送消息的通道或pattern可以作为 `String` 类型的第二个参数传递到方法中。

```java
public class DefaultMessageDelegate implements MessageDelegate {
  // implementation elided for clarity...
}
```

请注意上述 `MessageDelegate` 接口的实现（即上述 `DefaultMessageDelegate` 类）完全没有依赖Redis。它确实是一个POJO，我们用下面的配置把它变成一个MDP。

Java

XML

```java
@Configuration
class MyConfig {

  // …

  @Bean
  DefaultMessageDelegate listener() {
    return new DefaultMessageDelegate();
  }

  @Bean
  RedisMessageListenerContainer redisMessageListenerContainer(RedisConnectionFactory connectionFactory, DefaultMessageDelegate listener) {

    RedisMessageListenerContainer container = new RedisMessageListenerContainer();
    container.setConnectionFactory(connectionFactory);
    container.addMessageListener(new MessageListenerAdapter(listener, "handleMessage"), ChannelTopic.of("chatroom"));
    return container;
  }
}
```

|      | 监听主题可以是一个channel（例如，`topic="chatroom"`）或一个pattern（例如，`topic="*room"`）。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

前面的例子使用 Redis 命名空间来声明消息监听器容器，并自动注册 POJOs 作为监听器。完整的 Bean 类定义如下。

```xml
<bean id="messageListener" class="org.springframework.data.redis.listener.adapter.MessageListenerAdapter">
  <constructor-arg>
    <bean class="redisexample.DefaultMessageDelegate"/>
  </constructor-arg>
</bean>

<bean id="redisContainer" class="org.springframework.data.redis.listener.RedisMessageListenerContainer">
  <property name="connectionFactory" ref="connectionFactory"/>
  <property name="messageListeners">
    <map>
      <entry key-ref="messageListener">
        <bean class="org.springframework.data.redis.listener.ChannelTopic">
          <constructor-arg value="chatroom"/>
        </bean>
      </entry>
    </map>
  </property>
</bean>
```

每次收到消息，适配器都会自动、透明地在低级格式和所需的对象类型之间进行转换（使用配置的 `RedisSerializer`）。由方法调用引起的任何异常都会被容器捕捉和处理（默认情况下，异常会被通过logger输出）。

### 10.11. Redis Stream

Redis Streams以一种抽象的方式模拟了一个日志数据结构。通常情况下，日志是 append-only （仅追加）数据结构，从 beginning 开始消费，在一个随机的位置，或通过流式的新消息。

|      | 在 [Redis参考文档](https://redis.io/topics/streams-intro) 中了解更多关于Redis Streams的信息。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

Redis Stream 可以大致分为两个方面的功能。

- 追加记录
- 消费记录

虽然这种模式与 [Pub/Sub](https://springdoc.cn/spring-data-redis/#pubsub) 有相似之处，但主要区别在于消息的持久性和如何消费。

Pub/Sub依赖于瞬时消息的广播（也就是说，如果你不监听，就会错过消息），而Redis Stream 使用一个持久的、只追加的数据类型，保留消息直到流被修剪。消费方面的另一个区别是，Pub/Sub注册了一个服务器端的订阅。Redis将到达的消息推送给客户端，而 Redis Stream 需要主动轮询。

`org.springframework.data.redis.connection` 和 `org.springframework.data.redis.stream` 包提供了 Redis Stream 的核心功能。

#### 10.11.1. 追加

要发送一条record（记录），你可以像其他操作一样，使用低级别的 `RedisConnection` 或高级别的 `StreamOperations`。这两个实体都提供了 `add` （`xAdd`）方法，它接受 record 和目标流作为参数。`RedisConnection` 需要原始数据（字节数组），而 `StreamOperations` 允许任意对象作为 record 传入，如下面的例子所示。

```java
// append message through connection
RedisConnection con = …
byte[] stream = …
ByteRecord record = StreamRecords.rawBytes(…).withStreamKey(stream);
con.xAdd(record);

// append message through RedisTemplate
RedisTemplate template = …
StringRecord record = StreamRecords.string(…).withStreamKey("my-stream");
template.streamOps().add(record);
```

Stream record 携带一个 `Map`，key/value tuple，作为其有效载荷（payload）。将一个 record 添加到一个流中会返回 `RecordId`，可以作为进一步的参考。

#### 10.11.2. 消费

在消费方面，可以消费一个或多个流。Redis流提供读取命令，允许从已知流内容中的任意位置（随机访问）消费流，并在流结束后消费新的流记录。

在底层，`RedisConnection` 提供了 `xRead` 和 `xReadGroup` 方法，分别映射Redis命令，用于读取和在消费者组内读取。请注意，可以使用多个流作为参数。

|      | Redis中的订阅命令可以是阻塞的。也就是说，在一个连接上调用 `xRead` 会导致当前线程阻塞，因为它开始等待消息。只有当读取命令超时或收到消息时，该线程才会被释放。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

要消费流消息，可以在应用程序代码中轮询消息，或者使用两种异步接收方式之一，即通过[通过消息监听器容器进行异步接收](https://springdoc.cn/spring-data-redis/#redis.streams.receive.containers)（`imperative` 或 `reactive`）。每次有新的 record 到达时，容器会通知应用程序代码。

##### 同步接收

虽然流消费通常与异步处理相关，但也有可能同步消费消息。重载的 `StreamOperations.read(…)` 法提供了这种功能。在同步接收期间，调用线程可能会阻塞，直到有消息可用。属性 `StreamReadOptions.block` 指定了接收器（receiver）在放弃等待消息之前应该等待多长时间。

```java
// Read message through RedisTemplate
RedisTemplate template = …

List<MapRecord<K, HK, HV>> messages = template.streamOps().read(StreamReadOptions.empty().count(2),
				StreamOffset.latest("my-stream"));

List<MapRecord<K, HK, HV>> messages = template.streamOps().read(Consumer.from("my-group", "my-consumer"),
				StreamReadOptions.empty().count(2),
				StreamOffset.create("my-stream", ReadOffset.lastConsumed()))
```

##### 通过消息监听器容器进行异步接收

由于其阻塞性，低级别的轮询没有吸引力，因为它需要对每个消费者进行连接和线程管理。为了缓解这个问题，Spring Data提供了消息监听器，它可以完成所有繁重的工作。如果你熟悉EJB和JMS，你应该发现这些概念很熟悉，因为它被设计为尽可能接近Spring框架及其消息驱动的POJO（MDP）中的支持。

Spring Data提供了两种针对所使用的编程模型的实现。

- `StreamMessageListenerContainer` 作为强制性编程模型的消息监听器容器。它被用来从Redis流中消费record，并驱动注入其中的 `StreamListener` 实例。
- `StreamReceiver` 提供了一个消息监听器的响应式变体。它被用来从 Redis 流中消费消息，作为潜在的无限流，并通过 `Flux` 发出流消息。

`StreamMessageListenerContainer` 和 `StreamReceiver` 负责消息接收的所有线程，并将其分派到监听器中进行处理。消息监听器容器/接收器是MDP和消息提供者之间的中介，负责注册接收消息、资源获取和释放、异常转换等工作。这让你作为一个应用开发者可以编写与接收消息相关的（可能是复杂的）业务逻辑（并对其作出反应），并将模板式的Redis基础设施问题委托给该框架。

这两个容器都允许在运行时改变配置，因此你可以在应用程序运行时添加或删除订阅，而不需要重新启动。此外，该容器使用懒加载的订阅方法，只在需要时使用 `RedisConnection`。如果所有的监听器都被取消订阅，它会自动进行清理，并释放线程。

###### Imperative `StreamMessageListenerContainer`

与EJB世界中的Message-Driven Bean（MDB）类似，Stream-Driven POJO（SDP）作为Stream消息的接收器。对SDP的一个限制是，它必须实现 `org.springframework.data.redis.stream.StreamListener` 接口。还请注意，如果你的POJO在多个线程上接收消息，必须确保你的实现是线程安全的。

```java
class ExampleStreamListener implements StreamListener<String, MapRecord<String, String, String>> {

	@Override
	public void onMessage(MapRecord<String, String, String> message) {

		System.out.println("MessageId: " + message.getId());
		System.out.println("Stream: " + message.getStream());
		System.out.println("Body: " + message.getValue());
	}
}
```

`StreamListener` 代表了一个功能接口，因此可以使用其Lambda形式重写实现。

```java
message -> {

    System.out.println("MessageId: " + message.getId());
    System.out.println("Stream: " + message.getStream());
    System.out.println("Body: " + message.getValue());
};
```

一旦你实现了你的 `StreamListener`，就可以创建一个消息监听器容器并注册一个订阅。

```java
RedisConnectionFactory connectionFactory = …
StreamListener<String, MapRecord<String, String, String>> streamListener = …

StreamMessageListenerContainerOptions<String, MapRecord<String, String, String>> containerOptions = StreamMessageListenerContainerOptions
			.builder().pollTimeout(Duration.ofMillis(100)).build();

StreamMessageListenerContainer<String, MapRecord<String, String, String>> container = StreamMessageListenerContainer.create(connectionFactory,
				containerOptions);

Subscription subscription = container.receive(StreamOffset.fromStart("my-stream"), streamListener);
```

请参考各种消息监听器容器的Javadoc，以获得每个实现所支持的功能的完整描述。

###### 响应式 `StreamReceiver`

streaming data 的响应式消费通常通过事件或消息的 `Flux` 发生。响应式接收器（receiver）的实现由 `StreamReceiver` 及其重载的 `receive(…)` 消息提供。与 `StreamMessageListenerContainer` 相比，该响应式方法需要更少的基础设施资源，如线程，因为它是利用驱动提供的线程资源。接收 stream 是一个 Deman 驱动的 `StreamMessage` 的发布者（publisher）。

```java
Flux<MapRecord<String, String, String>> messages = …

return messages.doOnNext(it -> {
    System.out.println("MessageId: " + message.getId());
    System.out.println("Stream: " + message.getStream());
    System.out.println("Body: " + message.getValue());
});
```

现在我们需要创建 `StreamReceiver`，并注册一个订阅来消费流信息。

```java
ReactiveRedisConnectionFactory connectionFactory = …

StreamReceiverOptions<String, MapRecord<String, String, String>> options = StreamReceiverOptions.builder().pollTimeout(Duration.ofMillis(100))
				.build();
StreamReceiver<String, MapRecord<String, String, String>> receiver = StreamReceiver.create(connectionFactory, options);

Flux<MapRecord<String, String, String>> messages = receiver.receive(StreamOffset.fromStart("my-stream"));
```

请参考各种消息监听器容器的Javadoc，以获得每个实现所支持的功能的完整描述。

|      | Deman 驱动的消费使用背压信号来激活和停用轮询。如果 Deman 被满足， `StreamReceiver` 订阅就会暂停轮询，直到订阅者发出进一步的需求信号。根据 `ReadOffset` 策略的不同，这可能会导致消息被跳过。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### `Acknowledge` 策略

当你通过 `Consumer Group` （消费组）读取信息时，服务器会记住某个给定的信息已被送达，并将其添加到 Pending Entries List （待定条目列表 - PEL）中。一个已交付但尚未确认的消息的列表。

消息必须通过 `StreamOperations.acknowledge` 被确认，才能从 Pending Entries List 中删除，如下所示。

```java
StreamMessageListenerContainer<String, MapRecord<String, String, String>> container = ...

container.receive(Consumer.from("my-group", "my-consumer"), 
	StreamOffset.create("my-stream", ReadOffset.lastConsumed()),
    msg -> {

	    // ...
	    redisTemplate.opsForStream().acknowledge("my-group", msg); 
    });
```

|      | 消费者为 *my-consumer*，来自 *my-group* 组。收到的信息没有确认。 |
| ---- | ------------------------------------------------------------ |
|      | 处理后确认了该信息。                                         |

|      | 要在接收时自动确认信息，请使用 `receiveAutoAck` 而不是 `receive`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### `ReadOffset` 策略

Stream read 操作接受一个读偏移量规范，从给定的偏移量开始消费消息。`ReadOffset` 代表读取偏移量规范。Redis 支持3种不同的偏移量，这取决于你是独立消费流还是在一个消费者组内消费流。

- `ReadOffset.latest()` - 读取最新的消息。
- `ReadOffset.from(…)` - 在一个特定的消息Id后读取。
- `ReadOffset.lastConsumed()` - 在最后消费的消息Id之后读取（仅消费者组）。

In the context of a message container-based consumption, we need to advance (or increment) the read offset when consuming a message. Advancing depends on the requested `ReadOffset` and consumption mode (with/without consumer groups). The following matrix explains how containers advance `ReadOffset`:

在基于消息容器的消费中，我们需要在消费消息时提前（或增加）读取偏移量。推进取决于请求的 `ReadOffset` 和消费模式（有/无消费组）。下面的表格解释了容器如何推进 `ReadOffset`。

| Read offset         | 单独                                  | 消费组                                |
| :------------------ | :------------------------------------ | :------------------------------------ |
| Latest              | 读取最新信息                          | 读取最新信息                          |
| Specific Message Id | 使用最后看到的信息作为下一个MessageId | 使用最后看到的信息作为下一个MessageId |
| Last Consumed       | 使用最后看到的信息作为下一个MessageId | 根据消费者组，最后消费的信息          |

从特定的消息ID和最后消费的消息读取可以被认为是安全的操作，确保消费所有被附加到流中的消息。使用最新的消息进行读取可以跳过那些在轮询操作处于死信状态时被添加到流中的消息。轮询引入了一个dead time，在各个轮询命令之间，消息可以到达。Stream 的消费不是一个线性的连续读取，而是分成重复的 `XREAD` 调用。

##### 序列化（Serialization）

任何发送到流的 Record 都需要被序列化为二进制格式。由于stream与hash数据结构接近，流的key、字段名和值使用 `RedisTemplate` 上配置的相应序列化器。

| Stream 属性 | 序列化器            | 说明                           |
| :---------- | :------------------ | :----------------------------- |
| key         | keySerializer       | 用于 `Record#getStream()`      |
| 字段名      | hashKeySerializer   | 用于 payload 中的每个map key   |
| 值          | hashValueSerializer | 用于 payload 中的每个map value |

请确保审查使用中的 `RedisSerializer`，并注意如果你决定不使用任何序列化器，你需要确保这些值已经是二进制的。

##### 对象映射

###### 基本数据

`StreamOperations` 允许通过 `ObjectRecord` 将简单的值直接追加到流中，而不需要将这些值放到 `Map` 结构中。然后，该值将被分配到一个 *payload* 字段，在读回该值时可以提取。

```java
ObjectRecord<String, String> record = StreamRecords.newRecord()
    .in("my-stream")
    .ofObject("my-value");

redisTemplate()
    .opsForStream()
    .add(record); 

List<ObjectRecord<String, String>> records = redisTemplate()
    .opsForStream()
    .read(String.class, StreamOffset.fromStart("my-stream"));
```

|      | XADD my-stream * "_class" "java.lang.String" "_raw" "my-value" |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

`ObjectRecord` 和其他所有的record一样经过了序列化过程，因此Record也可以使用返回 `MapRecord` 的非类型化读取操作获得。

###### 复杂的值

将复杂的值添加到流中，可以通过3种方式进行。

- 转换为简单的值，例如使用字符串JSON表示。
- 用合适的 `RedisSerializer` 来序列化该值。
- 使用 `HashMapper` 将该值转换为适合序列化的 `Map`。

第一个方案是最直接的方案，但忽略了流结构所提供的字段值功能，流中的值对其他消费者来说仍然是可读的。第二种方法与第一种方法有相同的好处，但可能会导致非常特殊的消费者限制，因为所有消费者都必须实现相同的序列化机制。`HashMapper` 方法是一个更复杂的方法，它使用了steam的hash结构，但将源扁平化。只要选择合适的序列化器组合，其他消费者仍然能够读取record。

|      | `HashMapper` 将有payload换为具有特定类型的 `Map`。请确保使用能够序列/反序列化哈希的Hash-Key和Hash-Value序列化器。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

```java
ObjectRecord<String, User> record = StreamRecords.newRecord()
    .in("user-logon")
    .ofObject(new User("night", "angel"));

redisTemplate()
    .opsForStream()
    .add(record); 

List<ObjectRecord<String, User>> records = redisTemplate()
    .opsForStream()
    .read(User.class, StreamOffset.fromStart("user-logon"));
```

|      | XADD user-logon * "_class" "com.example.User" "firstname" "night" "lastname" "angel" |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

`StreamOperations` 默认使用 `ObjectHashMapper`。在获得 [ObjectHashMapper](https://springdoc.cn/spring-data-redis/#redis.repositories.mapping) 时，你可以提供一个适合你要求的 `HashMapper`。

```java
redisTemplate()
    .opsForStream(new Jackson2HashMapper(true))
    .add(record); 
```

|      | XADD user-logon * "firstname" "night" "@class" "com.example.User" "lastname" "angel" |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | `StreamMessageListenerContainer` 可能不知道domain类上使用的任何 `@TypeAlias`，因为这些需要通过 `MappingContext` 来解决。请确保用 `initialEntitySet` 初始化 `RedisMappingContext`。`@Bean RedisMappingContext redisMappingContext() {    RedisMappingContext ctx = new RedisMappingContext();    ctx.setInitialEntitySet(Collections.singleton(Person.class));    return ctx; } @Bean RedisConverter redisConverter(RedisMappingContext mappingContext) {    return new MappingRedisConverter(mappingContext); } @Bean ObjectHashMapper hashMapper(RedisConverter converter) {    return new ObjectHashMapper(converter); } @Bean StreamMessageListenerContainer streamMessageListenerContainer(RedisConnectionFactory connectionFactory, ObjectHashMapper hashMapper) {    StreamMessageListenerContainerOptions<String, ObjectRecord<String, Object>> options = StreamMessageListenerContainerOptions.builder()            .objectMapper(hashMapper)            .build();     return StreamMessageListenerContainer.create(connectionFactory, options); } ` |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 10.12. Redis 事务

Redis通过 `multi`、`exec` 和 `discard` 命令提供对 [事务](https://redis.io/topics/transactions) 的支持。这些操作在 `RedisTemplate` 上可用。然而，`RedisTemplate` 不能保证用同一个连接运行事务中的所有操作。

Spring Data Redis提供了 `SessionCallback` 接口，以便在需要用同一个连接执行多个操作时使用，例如在使用Redis事务时。以下例子使用了 `multi` 方法。

```java
//execute a transaction
List<Object> txResults = redisTemplate.execute(new SessionCallback<List<Object>>() {
  public List<Object> execute(RedisOperations operations) throws DataAccessException {
    operations.multi();
    operations.opsForSet().add("key", "value1");

    // This will contain the results of all operations in the transaction
    return operations.exec();
  }
});
System.out.println("Number of items added to set: " + txResults.get(0));
```

`RedisTemplate` 使用它的 value、hash key 和 hash value 序列化器，在返回之前对 `exec` 的所有结果进行反序列化。还有一个额外的 `exec` 方法，让你为事务结果传递一个自定义的序列化器。

#### 10.12.1. @Transactional 的支持

默认情况下，`RedisTemplate` 不参与管理Spring事务。如果你想让 `RedisTemplate` 在使用 `@Transactional` 或 `TransactionTemplate` 时利用Redis事务，你需要通过设置 `setEnableTransactionSupport(true)` 为每个 `RedisTemplate` 明确启用事务支持。启用事务支持将 `RedisConnection` 绑定到由 `ThreadLocal` 支持的当前事务。如果事务完成时没有错误，Redis事务会以 `EXEC` 的方式提交，否则以 `DISCARD` 的方式回滚。Redis 事务是面向批处理的。在一个正在进行的事务中发出的命令是排队的，只有在提交事务时才应用。

Spring Data Redis 区分了正在进行的事务中的只读和写命令。只读命令，如 `KEYS`，被输送到一个新的（非线程绑定的）`RedisConnection` 以允许读取。写命令由 `RedisTemplate` 排队，在提交时应用。

下面的例子显示了如何配置事务管理。

Example 3. 配置启用事务管理

```java
@Configuration
@EnableTransactionManagement                                 
public class RedisTxContextConfiguration {

  @Bean
  public StringRedisTemplate redisTemplate() {
    StringRedisTemplate template = new StringRedisTemplate(redisConnectionFactory());
    // explicitly enable transaction support
    template.setEnableTransactionSupport(true);              
    return template;
  }

  @Bean
  public RedisConnectionFactory redisConnectionFactory() {
    // jedis || Lettuce
  }

  @Bean
  public PlatformTransactionManager transactionManager() throws SQLException {
    return new DataSourceTransactionManager(dataSource());   
  }

  @Bean
  public DataSource dataSource() throws SQLException {
    // ...
  }
}
```

|      | 配置Spring Context以实现 [声明式事务管理](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#transaction-declarative)。 |
| ---- | ------------------------------------------------------------ |
|      | 配置 `RedisTemplate`，使其通过绑定连接到当前线程来参与事务。 |
|      | 事务管理需要一个 `PlatformTransactionManager`。Spring Data Redis并没有附带 `PlatformTransactionManager` 的实现。假设你的应用程序使用JDBC，Spring Data Redis可以通过使用现有的事务管理器参与事务。 |

下面的例子分别展示了一个使用约束。

Example 4. Usage Constraints

```java
// must be performed on thread-bound connection
template.opsForValue().set("thing1", "thing2");

// read operation must be run on a free (not transaction-aware) connection
template.keys("*");

// returns null as values set within a transaction are not visible
template.opsForValue().get("thing1");
```

### 10.13. Pipelining

Redis提供了对 [pipelining](https://redis.io/topics/pipelining) 的支持，这涉及到向服务器发送多个命令而不需要等待回复，然后在一个步骤中读取回复。当你需要连续发送几条命令时，例如向同一个List添加许多元素，管道可以提高性能。

Spring Data Redis提供了几种 `RedisTemplate` 方法，用于在 pipeline 中运行命令。如果你不关心 pipeline 操作的结果，你可以使用标准的 `execute` 方法，为 pipeline 参数传递 `true`。`executePipelined` 方法在 pipeline 中运行提供的 `RedisCallback` 或 `SessionCallback`，并返回结果，如下例所示。

```java
//pop a specified number of items from a queue
List<Object> results = stringRedisTemplate.executePipelined(
  new RedisCallback<Object>() {
    public Object doInRedis(RedisConnection connection) throws DataAccessException {
      StringRedisConnection stringRedisConn = (StringRedisConnection)connection;
      for(int i=0; i< batchSize; i++) {
        stringRedisConn.rPop("myqueue");
      }
    return null;
  }
});
```

前面的例子在一个 pipeline 上从一个队列中批量从右边弹出项目。`results` `List` 包含所有被弹出的项目。`RedisTemplate` 在返回前使用其value、hash key和hash value序列化器来反序列化所有的结果，所以前面的例子中返回的项目是字符串。还有一些额外的 `executePipelined` 方法，让你为管道化的结果传递一个自定义的序列化器。

请注意，从 `RedisCallback` 返回的值必须是 `null` 的，因为这个值会被丢弃，以便于返回 pipelined 命令的结果。

|      | Lettuce驱动支持细粒度的刷新控制，允许在命令出现时刷新，在连接关闭时缓冲或发送。`LettuceConnectionFactory factory = // ... factory.setPipeliningFlushPolicy(PipeliningFlushPolicy.buffered(3));  `在本地进行缓存，每隔3条命令后进行刷出。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 10.14. Redis 脚本

Redis 2.6及以上版本通过 [eval](https://redis.io/commands/eval) 和 [evalsha](https://redis.io/commands/evalsha) 命令提供了对运行Lua脚本的支持。Spring Data Redis 为运行脚本提供了一个高级抽象，它可以处理序列化并自动使用Redis的脚本缓存。

脚本可以通过调用 `RedisTemplate` 和 `ReactiveRedisTemplate` 的 `execute` 方法来运行。两者都使用一个可配置的 `ScriptExecutor`（或 `ReactiveScriptExecutor`）来运行所提供的脚本。默认情况下，`ScriptExecutor`（或 `ReactiveScriptExecutor`）负责对提供的key和参数进行序列化，并对脚本结果进行反序列化。这是通过 template 的key和value序列化器完成的。还有一个额外的重载，可以让你为脚本参数和结果传递自定义序列化器。

默认的 `ScriptExecutor` 通过检索脚本的 SHA1 来优化性能，并首先尝试运行 `evalsha`，如果脚本还没有出现在Redis脚本缓存中，则退回到 `eval`。

下面的例子通过使用Lua脚本运行一个常见的 "检查和设置" 场景。这是Redis脚本的一个理想用例，因为它需要原子化地运行一组命令，而且一个命令的行为会受到另一个命令结果的影响。

```java
@Bean
public RedisScript<Boolean> script() {

  ScriptSource scriptSource = new ResourceScriptSource(new ClassPathResource("META-INF/scripts/checkandset.lua"));
  return RedisScript.of(scriptSource, Boolean.class);
}
public class Example {

  @Autowired
  RedisScript<Boolean> script;

  public boolean checkAndSet(String expectedValue, String newValue) {
    return redisTemplate.execute(script, singletonList("key"), asList(expectedValue, newValue));
  }
}
-- checkandset.lua
local current = redis.call('GET', KEYS[1])
if current == ARGV[1]
  then redis.call('SET', KEYS[1], ARGV[2])
  return true
end
return false
```

前面的代码配置了一个 `RedisScript`，它指向一个名为 `checkandset.lua` 的文件，预计将返回一个布尔值。脚本的 `resultType` 应该是 `Long`、`Boolean`、`List` 或反序列化的value类型之一。如果脚本返回的是弃权状态（特别是OK），它也可以为 `null`。

|      | 最理想的做法是在你的应用程序上下文中配置一个 `DefaultRedisScript` 的实例，以避免在每次脚本运行时重新计算脚本的 SHA1。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

上面的 `checkAndSet` 方法会运行这些脚本。脚本可以作为事务或管道（pipeline）的一部分在 `SessionCallback` 中运行。参见 “[Redis 事务](https://springdoc.cn/spring-data-redis/#tx)” 和 “[Pipelining](https://springdoc.cn/spring-data-redis/#pipeline)” 以获得更多信息。

Spring Data Redis提供的脚本支持还允许你通过使用Spring Task和Scheduler抽象来安排Redis脚本的定期运行。更多细节请参见 [Spring Framework文档](https://spring.io/projects/spring-framework/) 。

### 10.15. Redis 缓存

|      | 在2.0中有所改变 |
| ---- | --------------- |
|      |                 |

Spring Redis通过 `org.springframework.data.redis.cache` 包为 Spring [缓存抽象](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/integration.html#cache) 提供了一个实现。要使用Redis作为支撑实现，请在配置中添加 `RedisCacheManager`，如下所示。

```java
@Bean
public RedisCacheManager cacheManager(RedisConnectionFactory connectionFactory) {
	return RedisCacheManager.create(connectionFactory);
}
```

`RedisCacheManager` 行为可以通过 `RedisCacheManagerBuilder` 进行配置，让你设置默认的 `RedisCacheConfiguration` 、事务行为和预定义的缓存。

```java
RedisCacheManager cm = RedisCacheManager.builder(connectionFactory)
	.cacheDefaults(defaultCacheConfig())
	.withInitialCacheConfigurations(singletonMap("predefined", defaultCacheConfig().disableCachingNullValues()))
	.transactionAware()
	.build();
```

如前面的例子所示，`RedisCacheManager` 允许在每个缓存的基础上定义配置。

用 `RedisCacheManager` 创建的 `RedisCache` 的行为是用 `RedisCacheConfiguration` 定义的。该配置可以让你设置 key 过期时间、前缀和用于转换为二进制存储格式的 `RedisSerializer` 实现，如以下例子所示。

```java
RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
    .entryTtl(Duration.ofSeconds(1))
	.disableCachingNullValues();
```

`RedisCacheManager` 默认使用无锁的 `RedisCacheWriter` 来读写二进制值。无锁缓存提高了吞吐量。缺乏入口锁定会导致 `putIfAbsent` 和 `clean` 方法的非原子命令重叠，因为这些需要向 Redis 发送多个命令。锁定的对应方法通过设置一个显式的锁key和检查该key的存在来防止命令重叠，这导致了额外的请求和潜在的命令等待时间。

锁定适用于缓存级别（cache level），而不是每个缓存条目（cache entry）。

可以选择加入锁行为，具体如下。

```java
RedisCacheManager cm = RedisCacheManager.build(RedisCacheWriter.lockingRedisCacheWriter(connectionFactory))
	.cacheDefaults(defaultCacheConfig())
	...
```

默认情况下，任何缓存条目的 `key` 都会以实际的缓存名称为前缀，后面是两个冒号。这种行为可以改变为静态和计算的前缀。

下面的例子显示了如何设置一个静态前缀。

```java
// static key prefix
RedisCacheConfiguration.defaultCacheConfig().prefixKeysWith("( ͡° ᴥ ͡°)");

The following example shows how to set a computed prefix:

// computed key prefix
RedisCacheConfiguration.defaultCacheConfig().computePrefixWith(cacheName -> "¯\_(ツ)_/¯" + cacheName);
```

缓存的实现默认为使用 `KEYS` 和 `DEL` 来清除缓存。`KEYS` 可能会在海量key时导致性能问题。因此，默认的 `RedisCacheWriter` 可以创建一个 `BatchStrategy` 来切换到一个基于 `SCAN` 的批处理策略。`SCAN` 策略需要一个 batch size，以避免过多的 `Redis` 命令往返。

```java
RedisCacheManager cm = RedisCacheManager.build(RedisCacheWriter.nonLockingRedisCacheWriter(connectionFactory, BatchStrategies.scan(1000)))
	.cacheDefaults(defaultCacheConfig())
	...
```

|      | 使用任何驱动和Redis运维模式（独立、集群）都完全支持 `KEYS` 批处理策略。当使用 Lettuce 驱动时，完全支持 `SCAN`。Jedis只在非集群模式下支持 `SCAN`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

下表列出了 `RedisCacheManager` 的默认设置。

| 设置                | 值                                             |
| :------------------ | :--------------------------------------------- |
| Cache Writer        | 无锁，`KEYS` 批处理策略                        |
| Cache Configuration | `RedisCacheConfiguration#defaultConfiguration` |
| Initial Caches      | None                                           |
| Transaction Aware   | No                                             |

下表列出了 `RedisCacheConfiguration` 的默认设置。

| Key Expiration     | None                                                         |
| :----------------- | :----------------------------------------------------------- |
| Cache `null`       | Yes                                                          |
| Prefix Keys        | Yes                                                          |
| Default Prefix     | The actual cache name                                        |
| Key Serializer     | `StringRedisSerializer`                                      |
| Value Serializer   | `JdkSerializationRedisSerializer`                            |
| Conversion Service | 带有默认 cache key converter 的 `DefaultFormattingConversionService`。 |

|      | 默认情况下，`RedisCache`，统计是禁用的。使用 `RedisCacheManagerBuilder.enableStatistics()`，通过 `RedisCache#getStatistics()` 收集本地的命中和失误率，返回收集到的数据的快照。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 10.16. Support Class

包 `org.springframework.data.redis.support` 提供了各种依靠Redis作为支撑存储的可重用组件。目前，该包包含在Redis之上的各种基于JDK的接口实现，如 [atomic](https://download.oracle.com/javase/8/docs/api/java/util/concurrent/atomic/package-summary.html) 计数器和JDK [Collections](https://download.oracle.com/javase/8/docs/api/java/util/Collection.html)。

原子计数器使Redis键的 incrementation 很容易被封装，而集合允许以最小的存储暴露或API泄漏轻松管理 Redis Key。特别是，`RedisSet` 和 `RedisZSet` 接口提供了对Redis支持的集合操作的简单访问，如 `intersection` 和 `union`。`RedisList` 在 Redis 之上实现了 `List`、`Queue` 和 `Deque` 合约（以及它们的等效阻塞同类），以最小的配置将存储暴露为 FIFO（先入先出）、LIFO（后入先出）或上限集合。下面的例子显示了一个使用 `RedisList` 的 bean 的配置。

Java

XML

```java
@Configuration
class MyConfig {

  // …

  @Bean
  RedisList<String> stringRedisTemplate(RedisTemplate<String, String> redisTemplate) {
    return new DefaultRedisList<>(template, "queue-key");
  }
}
```

下面的例子显示了一个 `Deque` 的 Java 配置实例。

```java
public class AnotherExample {

  // injected
  private Deque<String> queue;

  public void addTag(String tag) {
    queue.push(tag);
  }
}
```

如前面的例子所示，消费代码与实际的存储实现是解耦的。事实上，没有任何迹象表明Redis在下面被使用。这使得从开发环境转移到生产环境是透明的，并高度提高了可测试性（Redis的实现可以用内存中的实现代替）。

### 10.17. Observability（可观察性）

从应用组件中获得关于其操作、时间和与应用代码关系的洞察力对于理解延迟至关重要。Spring Data Redis通过Lettuce驱动与Micrometer集成，以收集Redis交互过程中的观察结果。一旦集成设置完成，Micrometer将为每个Redis命令创建meter和span（用于分布式跟踪）。

要启用集成，请在 `LettuceClientConfiguration` 中应用以下配置。

```java
@Configuration
class ObservabilityConfiguration {

  @Bean
  public ClientResources clientResources(ObservationRegistry observationRegistry) {

    return ClientResources.builder()
              .tracing(new MicrometerTracingAdapter(observationRegistry, "my-redis-cache"))
              .build();
  }

  @Bean
  public LettuceConnectionFactory lettuceConnectionFactory(ClientResources clientResources) {

    LettuceClientConfiguration clientConfig = LettuceClientConfiguration.builder()
                                                .clientResources(clientResources).build();
    RedisConfiguration redisConfiguration = …;
    return new LettuceConnectionFactory(redisConfiguration, clientConfig);
  }
}
```

#### 10.17.1. Observability - 约定

下面你可以找到这个项目所声明的所有 `GlobalObservabilityConventions` 和 `ObservabilityConventions` 的列表。

#### 10.17.2. Observability - Metric

下面你可以找到这个项目所声明的所有Metric（指标）的清单。

##### Redis命令的观察

> 围绕Redis命令的执行而创建的定时器（Timer ）。

**Metric 名称** `spring.data.redis`。 **类型** `timer` 和 **基本单位** `seconds`.

包裹类的全路径名称： `org.springframework.data.redis.connection.lettuce.observability.RedisObservation`.

| Name                      | Description          |
| ------------------------- | -------------------- |
| `db.operation`            | Redis命令值。        |
| `db.redis.database_index` | Redis数据库索引。    |
| `db.system`               | 数据库系统。         |
| `db.user`                 | Redis 用户。         |
| `net.peer.name`           | 数据库主机的名称。   |
| `net.peer.port`           | 逻辑上的远程端口号。 |
| `net.sock.peer.addr`      | Mongo peer 地址。    |
| `net.sock.peer.port`      | Mongo peer 端口。    |
| `net.transport`           | 网络运输。           |

| Name                              | Description       |
| --------------------------------- | ----------------- |
| `db.statement`                    | Redis statement。 |
| `spring.data.redis.command.error` | Redis错误响应。   |

#### 10.17.3. Observability - Span

下面你可以找到这个项目所声明的所有 span 的列表。

##### Redis 命令 Observation Span

> 围绕Redis命令的执行而创建的定时器（Timer）。

**Span 名称** `spring.data.redis`.

完整的保包裹类路径： `org.springframework.data.redis.connection.lettuce.observability.RedisObservation`.

| Name                              | Description          |
| --------------------------------- | -------------------- |
| `db.operation`                    | Redis命令值。        |
| `db.redis.database_index`         | Redis数据库索引。    |
| `db.statement`                    | Redis statement.     |
| `db.system`                       | 数据库系统。         |
| `db.user`                         | Redis 用户。         |
| `net.peer.name`                   | 数据库主机的名称。   |
| `net.peer.port`                   | 逻辑上的远程端口号。 |
| `net.sock.peer.addr`              | Mongo peer 地址.     |
| `net.sock.peer.port`              | Mongo peer 端口.     |
| `net.transport`                   | 网络运输。           |
| `spring.data.redis.command.error` | Redis错误响应。      |

请参阅 [OpenTelemetry Semantic Conventions](https://opentelemetry.io/docs/reference/specification/trace/semantic_conventions/database/#redis) 以进一步参考。

## 11. 响应式的 Redis 支持

本节涵盖了响应式Redis支持以及如何开始。响应式Redis支持自然与 [命令式Redis支持](https://springdoc.cn/spring-data-redis/#redis) 有某些重叠。

### 11.1. Redis的要求

Spring Data Redis目前与 [Lettuce](https://github.com/lettuce-io/lettuce-core) 集成，是唯一的响应式Java连接器。 [Project Reactor](https://projectreactor.io/) 被用作响应式组合库（composition library）。

### 11.2. 使用响应式驱动连接到Redis

使用Redis和Spring的首要任务之一是通过IoC容器连接到store。要做到这一点，需要一个Java连接器（或绑定）。无论你选择哪种库，你都必须使用 `org.springframework.data.redis.connection` 包及其 `ReactiveRedisConnection` 和 `ReactiveRedisConnectionFactory` 接口来处理和检索到Redis的活动 `connection`。

#### 11.2.1. Redis的运行模式

Redis可以作为一个独立的服务器，与 [Redis Sentinel](https://springdoc.cn/spring-data-redis/#redis:sentinel) 一起运行，或者以 [Redis集群](https://springdoc.cn/spring-data-redis/#cluster) 模式运行。 [Lettuce](https://github.com/lettuce-io/lettuce-core) 支持前面提到的所有连接类型。

#### 11.2.2. `ReactiveRedisConnection` 和 `ReactiveRedisConnectionFactory`

`ReactiveRedisConnection` 是Redis通信的核心，因为它处理与Redis后端的通信。它还自动将底层驱动的异常转化为Spring一致的DAO异常 [层次](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#dao-exceptions)，因此你可以在不改变任何代码的情况下切换连接器，因为操作语义保持不变。

`ReactiveRedisConnectionFactory` 创建活跃的 `ReactiveRedisConnection` 实例。此外，工厂作为 `PersistenceExceptionTranslator` 实例，意味着一旦声明，它们可以让你做透明的异常转换—例如，通过使用 `@Repository` 注解和AOP进行异常转换。欲了解更多信息，请参见Spring框架文档中专门的 [部分](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#orm-exception-translation)。

|      | 根据底层配置，factory可以返回一个新的连接或现有的连接（如果使用池或共享的本地连接）。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 使用 `ReactiveRedisConnectionFactory` 的最简单方法是通过IoC容器配置适当的连接器，并将其注入要使用的类中。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 11.2.3. 配置 Lettuce 连接器（Connector）

Spring Data Redis 通过 `org.springframework.data.redis.connection.lettuce` 包支持 [Lettuce](https://github.com/lettuce-io/lettuce-core)。

你可以为 Lettuce 设置 `ReactiveRedisConnectionFactory`，具体如下。

```java
@Bean
public ReactiveRedisConnectionFactory connectionFactory() {
  return new LettuceConnectionFactory("localhost", 6379);
}
```

下面的例子显示了一个更复杂的配置，包括SSL和超时，它使用 `LettuceClientConfigurationBuilder`。

```java
@Bean
public ReactiveRedisConnectionFactory lettuceConnectionFactory() {

  LettuceClientConfiguration clientConfig = LettuceClientConfiguration.builder()
    .useSsl().and()
    .commandTimeout(Duration.ofSeconds(2))
    .shutdownTimeout(Duration.ZERO)
    .build();

  return new LettuceConnectionFactory(new RedisStandaloneConfiguration("localhost", 6379), clientConfig);
}
```

更详细的客户端配置调整，见 [`LettuceClientConfiguration`](https://docs.spring.io/spring-data/redis/docs/3.1.0-SNAPSHOT/api/org/springframework/data/redis/connection/lettuce/LettuceClientConfiguration.html)。

### 11.3. 通过 ReactiveRedisTemplate 处理对象

大多数用户可能会使用 `ReactiveRedisTemplate` 和其对应的包 `org.springframework.data.redis.core`。由于其丰富的功能集，该template实际上是Redis模块的中心类。该template为Redis的交互提供了一个高层次的抽象。虽然 `ReactiveRedisConnection` 提供了接受和返回二进制值（`ByteBuffer`）的低级方法，但template负责序列化和连接管理，让你免于处理这些细节。

此外，template 提供了操作视图（遵循Redis命令 [参考](https://redis.io/commands) 中的分组），为针对某个类型的工作提供丰富的、生成的接口，如下表所述。

| 接口                          | 说明                                                     |
| :---------------------------- | :------------------------------------------------------- |
| *Key 类型的操作*              |                                                          |
| ReactiveGeoOperations         | Redis geospatial 操作，如 `GEOADD`、`GEORADIUS` 和其他。 |
| ReactiveHashOperations        | Redis hash 操作                                          |
| ReactiveHyperLogLogOperations | Redis HyperLogLog 操作 如 `PFADD`, `PFCOUNT` 等。        |
| ReactiveListOperations        | Redis list 操作                                          |
| ReactiveSetOperations         | Redis set 操作                                           |
| ReactiveValueOperations       | Redis string (或 value) 操作                             |
| ReactiveZSetOperations        | Redis zset (或 sorted set) 操作                          |

一旦配置好，该 template 是线程安全的，可以在多个实例中重复使用。

`ReactiveRedisTemplate` 使用基于Java的serializer进行大部分操作。这意味着template写入或读取的任何对象都是通过 `RedisElementWriter` 或 `RedisElementReader` 进行序列化或反序列化的。序列化上下文在构造时被传递给模板，Redis 模块在 `org.springframework.data.redis.serializer` 包中提供了几种实现。更多信息见 [Serializer](https://springdoc.cn/spring-data-redis/#redis:serializer) 。

下面的例子显示了一 `个ReactiveRedisTemplate` 被用来返回一个 `Mono`:

```java
@Configuration
class RedisConfiguration {

  @Bean
  ReactiveRedisTemplate<String, String> reactiveRedisTemplate(ReactiveRedisConnectionFactory factory) {
    return new ReactiveRedisTemplate<>(factory, RedisSerializationContext.string());
  }
}
public class Example {

  @Autowired
  private ReactiveRedisTemplate<String, String> template;

  public Mono<Long> addLink(String userId, URL url) {
    return template.opsForList().leftPush(userId, url.toExternalForm());
  }
}
```

### 11.4. 以字符串为重点的便利类

由于存储在Redis中的key和value是很常见的 `java.lang.String`，Redis模块为 `ReactiveRedisTemplate` 提供了一个基于String的扩展。 `ReactiveStringRedisTemplate`。它是一个方便的一站式解决方案，用于密集的String操作。除了与 `String` key 绑定外，该模板还使用了基于字符串的 `RedisSerializationContext`，这意味着存储的key和value是人类可读的（假设Redis和你的代码都使用相同的编码）。下面的例子显示了 `ReactiveStringRedisTemplate` 的使用情况。

```java
@Configuration
class RedisConfiguration {

  @Bean
  ReactiveStringRedisTemplate reactiveRedisTemplate(ReactiveRedisConnectionFactory factory) {
    return new ReactiveStringRedisTemplate<>(factory);
  }
}
public class Example {

  @Autowired
  private ReactiveStringRedisTemplate redisTemplate;

  public Mono<Long> addLink(String userId, URL url) {
    return redisTemplate.opsForList().leftPush(userId, url.toExternalForm());
  }
}
```

### 11.5. Redis Messaging/PubSub

Spring Data为Redis提供了专门的消息传递集成，在功能和命名上与Spring Framework中的JMS集成非常相似；事实上，熟悉Spring中JMS支持的用户应该感到很自在。

Redis消息传递可以大致分为两个方面的功能，即消息的生产或发布和消费或订阅，因此有了pubsub（Publish/Subscribe）这个短语。`ReactiveRedisTemplate` 类用于消息生产。对于异步接收，Spring Data提供了一个专门的消息监听器容器，用于消费消息流。对于仅仅是订阅的目的， `ReactiveRedisTemplate` 提供了利用监听器容器的精简替代方案。

`org.springframework.data.redis.connection` 和 `org.springframework.data.redis.listener` 这两个包提供了使用Redis消息传递的核心功能。

#### 11.5.1. 发布消息

为了发布消息，我们可以像其他操作一样，使用低级别的 `ReactiveRedisConnection` 或高级别的 `ReactiveRedisTemplate`。这两个实体都提供了一个 publish 方法，接受需要发送的消息和目标通道作为参数。`ReactiveRedisConnection` 需要原始数据，而 `ReactiveRedisTemplate` 则允许任意对象作为消息被传入。

```java
// send message through ReactiveRedisConnection
ByteBuffer msg = …
ByteBuffer channel = …
Mono<Long> publish = con.publish(msg, channel);

// send message through ReactiveRedisTemplate
ReactiveRedisTemplate template = …
Mono<Long> publish = template.convertAndSend("channel", "message");
```

#### 11.5.2. 订阅消息

在接收方面，人们可以通过直接命名或使用 pattern 匹配来订阅一个或多个 channel。后一种方法非常有用，因为它不仅允许用一个命令创建多个订阅，而且还可以监听在订阅时尚未创建的 channel（只要它们符合 pattern）。

在底层，`ReactiveRedisConnection` 提供了 `subscribe` 和 `pSubscribe` 方法，分别按通道和 pattern 映射 Redis 命令进行订阅。注意，多个 channel 或 pattern 可以作为参数使用。要改变一个订阅，只需查询 `ReactiveSubscription` 的 channel 和 pattern。

|      | Spring Data Redis中的响应式订阅命令是非阻塞的，可以在不发出元素的情况下结束。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

如上所述，一旦订阅了一个连接就开始等待消息。除了添加新的订阅或修改/取消现有的订阅外，不能对它调用其他命令。除了 `subscribe`、`pSubscribe`、`unsubscribe` 或 `pUnsubscribe` 以外的命令都是非法的，并会引起异常。

为了接收消息，需要获得消息流。注意，一个订阅只为在该特定订阅中注册的channel和pattern发布消息。消息流本身是一个 hot sequence，产生的元素不考虑 demand。请确保注册足够的 demand，以免耗尽消息缓冲区。

##### 消息监听器容器

Spring Data提供了 `ReactiveRedisMessageListenerContainer`，它代表用户完成了转换和订阅状态管理的所有繁重工作。

`ReactiveRedisMessageListenerContainer` 作为一个消息监听器容器。它被用来接收来自Redis Channel 的消息，并暴露出一个stream，该stream发射了应用了反序列化的通道消息。它负责注册接收消息、资源获取和释放、异常转换等工作。这允许你作为一个应用程序开发人员编写与接收消息（和对其作出反应）相关的（可能是复杂的）业务逻辑，并将模板式的Redis基础设施问题委托给该框架。stream 在发布者订阅时在Redis中注册一个subscription，如果subscription被取消，则取消注册。

此外，为了最大限度地减少应用程序的足迹，`ReactiveRedisMessageListenerContainer` 允许一个连接和一个线程被多个监听器共享，即使他们不共享一个订阅。因此，无论一个应用程序跟踪多少个监听器或通道（Channel），运行时的成本在其整个生命周期内都是相同的。此外，该容器允许在运行时改变配置，因此人们可以在应用程序运行时添加或删除监听器，而不需要重新启动。此外，该容器使用懒加载地订阅方法，只在需要时使用 `ReactiveRedisConnection` --如果所有的监听器都被取消订阅，就会自动进行清理。

消息监听器容器本身不需要外部线程资源。它使用驱动线程来发布消息。

```java
ReactiveRedisConnectionFactory factory = …
ReactiveRedisMessageListenerContainer container = new ReactiveRedisMessageListenerContainer(factory);

Flux<ChannelMessage<String, String>> stream = container.receive(ChannelTopic.of("my-channel"));
```

为了等待并确保正确的订阅，你可以使用返回 `Mono<Flux<ChannelMessage>>` 的 `receiveLater` 方法。由此产生的 `Mono` 完成了一个内部发布者，作为完成对给定topic的订阅的结果。通过拦截 `onNext` 信号，你可以同步服务器端的订阅。

```java
ReactiveRedisConnectionFactory factory = …
ReactiveRedisMessageListenerContainer container = new ReactiveRedisMessageListenerContainer(factory);

Mono<Flux<ChannelMessage<String, String>>> stream = container.receiveLater(ChannelTopic.of("my-channel"));

stream.doOnNext(inner -> // notification hook when Redis subscriptions are synchronized with the server)
    .flatMapMany(Function.identity())
    .…;
```

##### 通过 template API 订阅

如上所述，你可以直接使用 `ReactiveRedisTemplate` 来订阅 channel / pattern。这种方法提供了一个直接的解决方案，尽管是有限的，因为你失去了在最初的订阅之后增加订阅的选项。尽管如此，你仍然可以通过返回的 `Flux` 来控制消息流，例如使用 `take(Duration)`。读取完毕后，如果出现错误或取消，所有绑定的资源将被再次释放。

```java
redisTemplate.listenToChannel("channel1", "channel2").doOnNext(msg -> {
    // message processing ...
}).subscribe();
```

### 11.6. 响应式脚本（Reactive Scripting）

你可以通过使用 `ReactiveScriptExecutor` 来运行Redis脚本，最好通过 `ReactiveRedisTemplate` 来访问。

```java
public class Example {

  @Autowired
  private ReactiveRedisTemplate<String, String> template;

  public Flux<Long> theAnswerToLife() {

    DefaultRedisScript<Long> script = new DefaultRedisScript<>();
    script.setLocation(new ClassPathResource("META-INF/scripts/42.lua"));
    script.setResultType(Long.class);

    return reactiveTemplate.execute(script);
  }
}
```

更多关于脚本命令的细节，请参见 [脚本](https://springdoc.cn/spring-data-redis/#scripting) 部分。

## 12. Redis 集群

使用 [Redis Cluster](https://redis.io/topics/cluster-spec) 需要 Redis Server 3.0 以上版本。更多信息请参见 [集群手册](https://redis.io/topics/cluster-tutorial)。

### 12.1. 启用 Redis 集群

集群支持是基于与非集群通信相同的构建模块。`RedisClusterConnection` 是 `RedisConnection` 的扩展，它处理与Redis集群的通信，并将错误转化为Spring DAO的异常层次。`RedisClusterConnection` 实例是通过 `RedisConnectionFactory` 创建的，它必须与相关的 `RedisClusterConfiguration` 一起设置，如下例所示。

Example 5. 用于 Redis 集群的 `RedisConnectionFactory` 配置示例

```java
@Component
@ConfigurationProperties(prefix = "spring.redis.cluster")
public class ClusterConfigurationProperties {

    /*
     * spring.redis.cluster.nodes[0] = 127.0.0.1:7379
     * spring.redis.cluster.nodes[1] = 127.0.0.1:7380
     * ...
     */
    List<String> nodes;

    /**
     * Get initial collection of known cluster nodes in format {@code host:port}.
     *
     * @return
     */
    public List<String> getNodes() {
        return nodes;
    }

    public void setNodes(List<String> nodes) {
        this.nodes = nodes;
    }
}

@Configuration
public class AppConfig {

    /**
     * Type safe representation of application.properties
     */
    @Autowired ClusterConfigurationProperties clusterProperties;

    public @Bean RedisConnectionFactory connectionFactory() {

        return new LettuceConnectionFactory(
            new RedisClusterConfiguration(clusterProperties.getNodes()));
    }
}
```

|      | `RedisClusterConfiguration` 也可以通过 `PropertySource` 定义，并具有以下属性。配置属性`spring.redis.cluster.nodes`: 以逗号分割的 host:port 对。`spring.redis.cluster.max-redirects`: 允许集群重定向的数量。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 初始配置将驱动库指向一组初始的集群节点。现场集群重新配置所产生的变化只保留在本地驱动程序中，而不写回配置中。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 12.2. 使用 Redis Cluster Connection

如前所述，Redis Cluster 的行为与单节点Redis或甚至Sentinel监控的主从环境不同。这是因为自动分片将一个键映射到 16384 个槽（slot）中的一个，这些槽分布在各节点上。因此，涉及一个以上的key的命令必须断言所有的key映射到完全相同的槽，以避免跨槽错误。一个集群节点只为一组专用的key服务。对一个特定的服务器发出的命令只返回该服务器所服务的key的结果。作为一个简单的例子，考虑 `KEYS` 命令。当向集群环境中的一个服务器发出命令时，它只返回请求被发送到的节点所提供的key，而不一定是集群中的所有key。因此，为了获得集群环境中的所有key，你必须从所有已知的主节点上读取key。

虽然特定key的重定向到相应的槽服务节点由驱动库处理，但更高层次的功能，如收集跨节点的信息或向集群中的所有节点发送命令，由 `RedisClusterConnection` 覆盖。拿前面的 `keys` 例子来说，这意味着 `keys(pattern)` 方法会拾取集群中的每个主节点，并同时在每个主节点上运行 `KEYS` 命令，同时拾取结果并返回累积的keys集合。为了只请求单个节点的key，`RedisClusterConnection` 为这些方法提供了重载（例如，`keys(node, pattern)`）。

`RedisClusterNode` 可以从 `RedisClusterConnection.clusterGetNodes` 获得，也可以通过使用主机和端口或节点Id构建。

下面的例子显示了一组正在跨集群运行的命令。

Example 6. 在整个集群中运行的命令示例

```text
redis-cli@127.0.0.1:7379 > cluster nodes

6b38bb... 127.0.0.1:7379 master - 0 0 25 connected 0-5460                      
7bb78c... 127.0.0.1:7380 master - 0 1449730618304 2 connected 5461-10922       
164888... 127.0.0.1:7381 master - 0 1449730618304 3 connected 10923-16383      
b8b5ee... 127.0.0.1:7382 slave 6b38bb... 0 1449730618304 25 connected          
RedisClusterConnection connection = connectionFactory.getClusterConnnection();

connection.set("thing1", value);                                               
connection.set("thing2", value);                                               

connection.keys("*");                                                          

connection.keys(NODE_7379, "*");                                               
connection.keys(NODE_7380, "*");                                               
connection.keys(NODE_7381, "*");                                               
connection.keys(NODE_7382, "*");                                               
```

|      | 主节点为0至5460号槽位提供服务，并复制到7382号的副本中。 |
| ---- | ------------------------------------------------------- |
|      | 为5461至10922槽位服务的主节点                           |
|      | 为10923至16383槽位服务的主节点                          |
|      | 持有7379号主站复制体的复制节点                          |
|      | 请求被路由到为12182插槽服务的7381节点                   |
|      | 请求被路由到为5061插槽服务的7379节点                    |
|      | 请求被路由到7379、7380、7381的节点。 → [thing1, thing2] |
|      | 请求被路由到7379的节点 → [thing2]                       |
|      | 请求被路由到7380的节点 → []                             |
|      | 请求被路由到7381的节点 → [thing1]                       |
|      | 请求被路由到7382的节点 → [thing2]                       |

当所有的key都映射到同一个槽时，本地驱动库会自动提供跨槽请求，如 `MGET`。然而，一旦不是这种情况，`RedisClusterConnection` 就会针对提供槽位的节点运行多个并行的 `GET` 命令，并再次返回一个累积的结果。这比单槽方法的性能要差，因此，应该谨慎使用。如果有疑问，可以考虑通过提供大括号中的前缀将 key 绑定在同一个槽上，比如 `{my-prefix}.thing1` 和 `{my-prefix}.thing2`，它们都会映射到同一个槽号。下面的例子显示了跨槽位的请求处理。

Example 7. 跨 Slot 请求处理示例

```text
redis-cli@127.0.0.1:7379 > cluster nodes

6b38bb... 127.0.0.1:7379 master - 0 0 25 connected 0-5460                      
7bb...
RedisClusterConnection connection = connectionFactory.getClusterConnnection();

connection.set("thing1", value);           // slot: 12182
connection.set("{thing1}.thing2", value);  // slot: 12182
connection.set("thing2", value);           // slot:  5461

connection.mGet("thing1", "{thing1}.thing2");                                  

connection.mGet("thing1", "thing2");                                           
```

|      | 与之前样本中的配置（Configuration ）相同。                   |
| ---- | ------------------------------------------------------------ |
|      | key 映射到同一槽位 → 127.0.0.1:7381 MGET thing1 {thing1}.thing2 |
|      | key映射到不同的槽，并被分割成单槽的，路由到相应的节点。 → 127.0.0.1:7379 GET thing2 → 127.0.0.1:7381 GET thing1 |

|      | 前面的例子展示了Spring Data Redis遵循的一般策略。请注意，有些操作可能需要将大量的数据加载到内存中来计算所需的命令。此外，并不是所有的跨槽请求都可以安全地移植到多个单槽请求中，如果误用就会出错（例如 `PFCOUNT`）。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 12.3. 使用 `RedisTemplate` 和 `ClusterOperations`

有关 `RedisTemplate` 的一般目的、配置和使用的信息，请参阅 [通过 RedisTemplate 处理对象](https://springdoc.cn/spring-data-redis/#redis:template) 部分。

|      | 在使用任何 JSON `RedisSerializer` 设置 `RedisTemplate#keySerializer` 时要小心，因为改变JSON结构对哈希槽的计算有直接影响。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

`RedisTemplate` 通过 `ClusterOperations` 接口提供对集群特定操作的访问，可以从 `RedisTemplate.opsForCluster()` 获得。这让你可以明确地在集群内的单个节点上运行命令，同时保留为 template 配置的序列化和反序列化功能。它还提供了管理命令（如 `CLUSTER MEET`）或更高级的操作（例如，重新分片 - resharding）。

下面的例子显示了如何用 `RedisTemplate` 访问 `RedisClusterConnection`。

Example 8. 用 `RedisTemplate` 访问 `RedisClusterConnection`

```text
ClusterOperations clusterOps = redisTemplate.opsForCluster();
clusterOps.shutdown(NODE_7379);                                              
```

|      | 关闭7379的节点，并祈祷有一个可以接管的副本在那里（祈祷？，官方还有点幽默）。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

## 13. Redis Repository

通过与 Redis Repository 合作，你可以在 Redis Hashe 中无缝转换和存储domain对象，应用自定义映射策略，并使用二级索引。

|      | Redis Repository 至少需要 Redis Server 2.8.0 版本，并且不能使用事务。请确保使用 [禁用事务支持](https://springdoc.cn/spring-data-redis/#tx.spring) 的 `RedisTemplate`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 13.1. 用法

Spring Data Redis 可以让你轻松实现 domain 实体，如下面的例子所示。

Example 9. Person 实体示例

```java
@RedisHash("people")
public class Person {

  @Id String id;
  String firstname;
  String lastname;
  Address address;
}
```

我们这里有一个非常简单的 domain 对象。请注意，它的类型上有一个 `@RedisHash` 注解，还有一个用 `org.springframework.data.annotation.Id` 注解的名为 `id` 的属性。这两个项目负责创建用于持久化 hash 的实际key。

|      | 带有 `@Id` 注解的属性以及那些名为 `id` 的属性被认为是标识符属性。那些带有注解的属性比其他属性更受青睐。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

现在要真正有一个负责存储和检索的组件，我们需要定义一个 repository 接口，如下例所示。

Example 10. 保存 Person 实体的 Basic Repository 接口

```java
public interface PersonRepository extends CrudRepository<Person, String> {

}
```

由于我们的 repository 继承了 `CrudRepository`，它提供了基本的 CRUD 和 finder 操作。在这中间我们需要把事情粘合在一起的是相应的Spring配置，如下例所示。

Example 11. 用于Redis Repository 的JavaConfig

```java
@Configuration
@EnableRedisRepositories
public class ApplicationConfig {

  @Bean
  public RedisConnectionFactory connectionFactory() {
    return new LettuceConnectionFactory();
  }

  @Bean
  public RedisTemplate<?, ?> redisTemplate(RedisConnectionFactory redisConnectionFactory) {

    RedisTemplate<byte[], byte[]> template = new RedisTemplate<byte[], byte[]>();
    template.setConnectionFactory(redisConnectionFactory);
    return template;
  }
}
```

鉴于前面的设置，我们可以将 `PersonRepository` 注入到我们的组件中，如下面的例子所示。

Example 12. 访问 Person 实体

```java
@Autowired PersonRepository repo;

public void basicCrudOperations() {

  Person rand = new Person("rand", "al'thor");
  rand.setAddress(new Address("emond's field", "andor"));

  repo.save(rand);                                         

  repo.findOne(rand.getId());                              

  repo.count();                                            

  repo.delete(rand);                                       
}
```

|      | 如果当前值（value）为 `null`，则生成一个新的 `id`，或者重用一个已经设置的 `id` 值，并在Redis Hash中存储 `Person` 类型的属性，其key值为 `keyspace:id` 格式 — 在本例中，可能是 `people:5d67b7e1-8640-4475-beeb-c666fab4c0e5`。 |
| ---- | ------------------------------------------------------------ |
|      | 使用提供的 `id` 来检索存储在 `keyspace:id` 的对象。          |
|      | `@RedisHash` 在 `Person` 上定义的 keyspace `people` 内可用的实体总数。 |
|      | 从Redis中删除给定对象的key。                                 |

### 13.2. 对象映射的基础知识

本节涵盖了Spring Data对象映射、对象创建、字段和属性访问、可变性和不可变性的基本原理。注意，本节只适用于不使用底层数据存储的对象映射的Spring Data模块（如JPA）。此外，请务必查阅特定于存储的对象映射部分，如索引、自定义列或字段名或类似内容。

Spring Data对象映射的核心职责是创建domain对象的实例，并将存储的本地数据结构映射到这些对象上。这意味着我们需要两个基本步骤。

1. 通过使用暴露的构造函数之一来创建实例。
2. Instance population to materialize all exposed properties.

#### 13.2.1. Object 创建

Spring Data会自动尝试检测一个持久化实体的构造函数，以用于将该类型的对象具体化。该解析算法的工作原理如下。

1. 如果有一个用 `@PersistenceCreator` 注解的静态工厂方法，那么就使用它。
2. 如果有一个单一的构造函数，它就被使用。
3. 如果有多个构造函数，并且正好有一个被注解为 `@PersistenceCreator`，那么就使用它。
4. 如果类型是Java `Record`，则使用规范的构造函数。
5. 如果有一个无参数的构造函数，它将被使用。其他构造函数将被忽略。

值解析假定构造器/工厂方法参数名与实体的属性名相匹配，即解析将在属性被填充的情况下进行，包括映射中的所有定制（不同的数据存储列或字段名等）。这也需要在class文件中提供参数名称信息，或者在构造函数上提供 `@ConstructorProperties` 注解。

通过使用Spring Framework的 `@Value` 值注解，可以使用store特定的SpEL表达式来定制值解析。请查阅关于 store 特定映射的章节以了解更多细节。

对象创建的内部细节

为了避免反射的开销，Spring Data对象的创建默认使用一个在运行时生成的工厂类，它将直接调用 domain 类的构造函数。例如，对于这个例子的中的类：

```java
class Person {
  Person(String firstname, String lastname) { … }
}
```

我们将在运行时创建一个语义上等同于此的工厂类。

```java
class PersonObjectInstantiator implements ObjectInstantiator {

  Object newInstance(Object... args) {
    return new Person((String) args[0], (String) args[1]);
  }
}
```

这使我们在性能上比反射有大约10%的提升。为了使 domain 类能进行这种优化，它需要遵守一系列的限制条件。

- 它不能是一个私有类。
- 它不能是一个非静态的内部类。
- 它不能是CGLib代理类。
- 被Spring Data使用的构造函数不能是私有的。

如果这些标准中的任何一项不符合，Spring Data将回退到使用反射来进行实例化。

#### 13.2.2. 属性填充

一旦实体的实例被创建，Spring Data就会填充该类的所有剩余持久化属性。除非已经由实体的构造函数填充（即通过其构造函数参数列表设置），否则 identifier 属性将首先被填充，以允许解决循环对象引用。之后，所有尚未被构造函数填充的非瞬时（non-transient）属性都被设置在实体实例上。为此，我们使用以下算法。

1. 如果该属性是不可变的，但暴露了一个 `with…` 方法（见下文），我们使用 `with…` 方法来创建一个具有新属性值的新实体实例。
2. 如果定义了属性 access（即通过getter和setter访问），我们就调用setter方法。
3. 如果该属性是可变的，我们直接设置该字段。
4. 如果属性是不可变的，我们就使用持久化操作（见 [Object 创建](https://springdoc.cn/spring-data-redis/#mapping.object-creation)）所使用的构造函数来创建一个实例的副本。
5. 默认情况下，我们直接设置字段的值。

属性填充的内部细节

与我们在[对象构造中的优化](https://springdoc.cn/spring-data-redis/#mapping.object-creation.details)类似，我们也使用Spring Data 运行时生成的 accessor 类来与实体实例进行交互。

```java
class Person {

  private final Long id;
  private String firstname;
  private @AccessType(Type.PROPERTY) String lastname;

  Person() {
    this.id = null;
  }

  Person(Long id, String firstname, String lastname) {
    // Field assignments
  }

  Person withId(Long id) {
    return new Person(id, this.firstname, this.lastame);
  }

  void setLastname(String lastname) {
    this.lastname = lastname;
  }
}
```

Example 13. A generated Property Accessor

```java
class PersonPropertyAccessor implements PersistentPropertyAccessor {

  private static final MethodHandle firstname;              

  private Person person;                                    

  public void setProperty(PersistentProperty property, Object value) {

    String name = property.getName();

    if ("firstname".equals(name)) {
      firstname.invoke(person, (String) value);             
    } else if ("id".equals(name)) {
      this.person = person.withId((Long) value);            
    } else if ("lastname".equals(name)) {
      this.person.setLastname((String) value);              
    }
  }
}
```

|      | PropertyAccessor 持有底层对象的一个可变实例。这是为了使其他不可变的属性能够进行修改。 |
| ---- | ------------------------------------------------------------ |
|      | 默认情况下，Spring Data使用字段访问（field-access）来读取和写入属性值。根据 `private` 字段的可见性规则，`MethodHandles` 被用来与字段交互。 |
|      | 该类暴露了一个 `withId(…)` 方法，用来设置标识符（identifier），例如，当一个实例被插入到数据存储中并生成了一个标识符。调用 `withId(…)` 会创建一个新的 `Person` 对象。所有后续的修改都将在新的实例中进行，而之前的实例不受影响。 |
|      | 使用属性访问（property-access）允许直接调用方法而不使用 `MethodHandles`。 |

这使我们在性能上比反射有了大约25%的提升。为了使domain类有能进行这种优化，它需要遵守一系列的限制条件。

- 类不能在默认（default ）或 `java` 包下。
- 类和它们的构造函数必须是 `public`。
- 作为内部类的类必须是 `static` 的。
- 所使用的Java Runtime 必须允许在原 ClassLoader 中声明类。Java 9 和更新的版本有某些限制。

默认情况下，Spring Data会尝试使用生成的属性访问器（property accessor），如果检测到限制，则会退回到基于反射的访问器。

让我们来看看以下实体。

Example 14. A sample entity

```java
class Person {

  private final @Id Long id;                                                
  private final String firstname, lastname;                                 
  private final LocalDate birthday;
  private final int age;                                                    

  private String comment;                                                   
  private @AccessType(Type.PROPERTY) String remarks;                        

  static Person of(String firstname, String lastname, LocalDate birthday) { 

    return new Person(null, firstname, lastname, birthday,
      Period.between(birthday, LocalDate.now()).getYears());
  }

  Person(Long id, String firstname, String lastname, LocalDate birthday, int age) { 

    this.id = id;
    this.firstname = firstname;
    this.lastname = lastname;
    this.birthday = birthday;
    this.age = age;
  }

  Person withId(Long id) {                                                  
    return new Person(id, this.firstname, this.lastname, this.birthday, this.age);
  }

  void setRemarks(String remarks) {                                         
    this.remarks = remarks;
  }
}
```

|      | id（identifier）属性是 `final` 的，但在构造函数中设置为 `null`。该类暴露了一个 `withId(…)` 方法，用于设置id，例如，当一个实例被插入到数据存储中并且已经生成了一个id。当一个新的 `Person` 实例被创建时，原来的 `Person` 实例保持不变。同样的模式通常适用于其他的属性，这些属性是存储管理的，但可能要为持久化操作而改变。`with` 方法是可选的，因为持久化构造函数（见6）实际上是一个复制构造函数，设置该属性将被转化为创建一个新的实例，并应用新的id值。 |
| ---- | ------------------------------------------------------------ |
|      | `firstname` 和 `lastname` 属性是普通的不可变的属性，可能通过 `getter` 暴露。 |
|      | `age` 属性是一个不可变的，但是从 `birthday` 属性派生出来的。在所示的设计中，数据库的值将胜过默认值，因为Spring Data使用唯一声明的构造函数。即使意图是优先考虑计算，重要的是这个构造函数也将 `age` 作为参数（有可能忽略它），否则属性填充步骤将试图设置 `age` 字段，但由于它是不可变的，而且没有 `with…` 方法存在，因此失败了。 |
|      | `comment` 属性是可变的，通过直接设置其字段来填充。           |
|      | `remarks` 属性是可变的，通过调用setter方法来填充。           |
|      | 该类暴露了一个工厂方法和一个用于创建对象的构造器。这里的核心思想是使用工厂方法而不是额外的构造函数，以避免通过 `@PersistenceCreator` 进行构造函数消歧义的需要。相反，属性的缺省是在工厂方法中处理的。如果你想让Spring Data使用工厂方法进行对象实例化，请用 `@PersistenceCreator` 来注解它。 |

#### 13.2.3. 一般建议

- *尽量坚持使用不可变的对象* --不可变的对象创建起来很简单，因为具体化一个对象只需要调用其构造函数即可。同时，这也避免了你的domain对象充满了允许客户端代码操纵对象状态的 setter 方法。如果你需要这些，最好使它们受到 `package` 的保护，这样它们就只能被有限的共存类型所调用。纯构造函数实例化属性比填充快30%。
- *提供一个全参数构造函数* — 即使你不能或不想将你的实体建模为不可变的值，提供一个将实体的所有属性作为参数的构造函数仍有价值，包括可变的属性，因为这允许对象映射跳过属性填充以获得最佳性能。
- *使用工厂方法而不是重载构造函数，以避免 `@PersistenceCreator`* — 由于需要全参数构造函数以获得最佳性能，我们通常希望暴露更多的应用用例特定的构造函数，省略自动生成的ID等东西。宁愿使用静态工厂方法来暴露这些全参数构造函数的变体，这是一种既定的模式。
- *确保你遵守允许生成的实例化器（instantiator）和属性访问器（accessor）类被使用的约束。* — 
- *对于要生成的ID，仍然使用 final 字段与全参数持久化构造函数（首选）或 `with…` 方法相结合* — 
- *使用Lombok来避免模板代码* --由于持久化操作通常需要一个接受所有参数的构造器，它们的声明变成了繁琐的重复的模板参数到字段的分配，通过使用 Lombok 的 `@AllArgsConstructor` 可以最好地避免。

##### 属性覆盖

Java允许对 domain 类进行灵活的设计，子类可以定义一个在其父类中已经用相同名称声明的属性。考虑一下下面的例子。

```java
public class SuperType {

   private CharSequence field;

   public SuperType(CharSequence field) {
      this.field = field;
   }

   public CharSequence getField() {
      return this.field;
   }

   public void setField(CharSequence field) {
      this.field = field;
   }
}

public class SubType extends SuperType {

   private String field;

   public SubType(String field) {
      super(field);
      this.field = field;
   }

   @Override
   public String getField() {
      return this.field;
   }

   public void setField(String field) {
      this.field = field;

      // optional
      super.setField(field);
   }
}
```

这两个类都使用可分配类型来定义一个 `field`。然而，`SubType` 会影射 `SuperType.field`。根据类的设计，使用构造函数可能是设置 `SuperType.field` 的唯一默认方法。另外，在 setter 中调用 `super.setField(…)` 可以在 `SuperType` 中设置 `field`。所有这些机制在某种程度上都会产生冲突，因为这些属性共享相同的名称，但可能代表两个不同的值。如果类型不可分配，Spring Data会跳过父类属性。也就是说，被覆盖的属性的类型必须可分配给它的父类的属性类型才能被注册为覆盖（override），否则父类的属性就会被认为是 transient 的。我们一般建议使用不同的属性名称。

Spring Data模块通常支持持有不同 value 的重写属性。从编程模型的角度来看，有几件事需要考虑。

1. 哪些属性应该被持久化（默认为所有声明的属性）？你可以通过用 `@Transient` 来注解这些属性来排除它们。
2. 如何在你的数据存储中表示属性？对不同的值使用相同的字段/列名通常会导致数据损坏，所以你应该至少使用一个明确的字段/列名来注解其中的一个属性。
3. 不能使用 `@AccessType(PROPERTY)`，因为在不对 setter 实现做任何进一步假设的情况下，一般不能设置父类属性。

#### 13.2.4. 对 Kotlin 的支持

Spring Data 适应了 Kotlin 的具体特性，允许对象的创建和变异（mutation）。

##### Kotlin object 创建

Kotlin类支持实例化，所有的类默认是不可变的，需要明确的属性声明来定义可变的属性。

Spring Data会自动尝试检测一个持久化实体的构造函数，以用于将该类型的对象具体化。 该解析算法的工作原理如下。

1. 如果有一个构造函数被注解为 `@PersistenceCreator`，它将被使用。
2. 如果类型是 [Kotlin data cass](https://springdoc.cn/spring-data-redis/#mapping.kotlin)，则使用 primary 构造函数。
3. 如果有一个用 `@PersistenceCreator` 注解的静态工厂方法，那么就使用它。
4. 如果有一个单一的构造函数，它就被使用。
5. 如果有多个构造函数，并且正好有一个被 `@PersistenceCreator` 注解，那么它就被使用。
6. 如果该类型是一个Java Record ，则使用规范的构造函数。
7. 如果有一个无参数的构造函数，它将被使用。其他构造函数将被忽略。

考虑以下 `data` 类 `Person`。

```kotlin
data class Person(val id: String, val name: String)
```

上面的类编译成一个典型的具有显式构造函数的类。我们可以通过添加另一个构造函数来定制这个类，并用 `@PersistenceCreator` 来注释它，以表明构造函数的偏好。

```kotlin
data class Person(var id: String, val name: String) {

    @PersistenceCreator
    constructor(id: String) : this(id, "unknown")
}
```

Kotlin通过允许在未提供参数时使用默认值来支持参数的可选性。当Spring Data检测到具有参数默认值的构造函数时，如果数据存储没有提供值（或简单地返回 `null`），那么它就会不提供这些参数，这样Kotlin就可以应用参数默认值。考虑以下类，它对 `name` 应用参数默认值。

```kotlin
data class Person(var id: String, val name: String = "unknown")
```

每当 `name` 参数不是结果的一部分或者其值为 `null` 时，那么 `name` 就默认为 `unknown`。

##### Kotlin data class 的属性填充

在Kotlin中，所有的类默认都是不可变的，需要明确的属性声明来定义可变属性。考虑下面这个 `data` class `Person`。

```kotlin
data class Person(val id: String, val name: String)
```

这个类实际上是不可变的。它允许创建新的实例，因为 Kotlin 生成了一个 `copy(…)` 方法，该方法创建了新的对象实例，复制了现有对象的所有属性值并应用了作为参数提供给该方法的属性值。

##### Kotlin 属性覆盖

Kotlin允许声明 [属性覆盖](https://kotlinlang.org/docs/inheritance.html#overriding-properties) 来改变子类中的属性。

```kotlin
open class SuperType(open var field: Int)

class SubType(override var field: Int = 1) :
	SuperType(field) {
}
```

这样的安排渲染了两个名为 `field` 的属性。Kotlin为每个类中的每个属性生成了属性访问器（getter 和 setter），代码看起来像如下。

```java
public class SuperType {

   private int field;

   public SuperType(int field) {
      this.field = field;
   }

   public int getField() {
      return this.field;
   }

   public void setField(int field) {
      this.field = field;
   }
}

public final class SubType extends SuperType {

   private int field;

   public SubType(int field) {
      super(field);
      this.field = field;
   }

   public int getField() {
      return this.field;
   }

   public void setField(int field) {
      this.field = field;
   }
}
```

`SubType` 上的Getter和Setter只设置 `SubType.field` 而不是 `SuperType.field`。在这样的安排中，使用构造函数是设置 `SuperType.field` 的唯一默认方法。通过 `this.SuperType.field = …` 给 `SubType` 添加一个方法来设置 `SuperType.field` 是可能的，但是超出了支持的惯例。属性覆盖在某种程度上会产生冲突，因为这些属性共享相同的名称，但可能代表两个不同的值。我们通常建议使用不同的属性名称。

Spring Data模块通常支持持有不同value的重写属性。从编程模型的角度来看，有几件事需要考虑。

1. 哪些属性应该被持久化（默认为所有声明的属性）？你可以通过用 `@Transient` 来注解这些属性来排除它们。
2. 如何在你的数据存储中表示属性？对不同的值使用相同的字段/列名通常会导致数据损坏，所以你应该至少使用一个明确的字段/列名来注解其中的一个属性。
3. `@AccessType(PROPERTY)` 不能使用，因为不能设置父类属性。

### 13.3. 对象（Object）到哈希的映射

Redis Repository 支持将 Object 持久化为Hashe。这需要一个 Object 到Hashe的转换，由 `RedisConverter` 完成。默认实现使用 `Converter` 将属性值映射到 `Redis` 原生 `byte[]`，或从 Redis 原生 `byte[]` 中提取。

考虑到前几节中的 `Person` 类型，默认的映射看起来像下面这样。

```text
_class = org.example.Person                 
id = e2c7dcee-b8cd-4424-883e-736ce564363e
firstname = rand                            
lastname = al’thor
address.city = emond's field                
address.country = andor
```

|      | `_class` 属性包括在root层以及任何嵌套的接口或抽象类型上。 |
| ---- | --------------------------------------------------------- |
|      | 简单的属性值是通过路径(Path)映射的。                      |
|      | 复杂类型的属性是通过"."来映射的。                         |

下表描述了默认的映射规则。

| 类型                      | 示例                                                         | 映射值                                                       |
| :------------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 基本类型 (例如 String)    | String firstname = "rand";                                   | firstname = "rand"                                           |
| 字节数组 (`byte[]`)       | byte[] image = "rand".getBytes();                            | image = "rand"                                               |
| 复合类型 (例如， Address) | Address address = new Address("emond’s field");              | address.city = "emond’s field"                               |
| 基本类型 List             | List<String> nicknames = asList("dragon reborn", "lews therin"); | nicknames.[0] = "dragon reborn", nicknames.[1] = "lews therin" |
| 基本类型 Map              | Map<String, String> atts = asMap({"eye-color", "grey"}, {"…  | atts.[eye-color] = "grey", atts.[hair-color] = "…            |
| 复合类型 List             | List<Address> addresses = asList(new Address("em…            | addresses.[0].city = "emond’s field", addresses.[1].city = "… |
| 复合类型 Map              | Map<String, Address> addresses = asMap({"home", new Address("em… | addresses.[home].city = "emond’s field", addresses.[work].city = "… |

|      | 由于扁平化的表示结构，`Map` 的 key 需要是简单的类型，如 `String` 或 `Number`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

通过在 `RedisCustomConversions` 中注册相应的 `Converter`，可以定制映射行为。这些转换器可以负责从一个单一的 `byte[]` 以及 `Map<String,byte[]>` 转换。第一个适合于（例如）将复杂类型转换为（例如）二进制JSON表示，仍然使用默认的映射哈希结构。第二个选项提供了对产生的哈希值的完全控制。

|      | 将对象写入Redis哈希时，会删除哈希中的内容并重新创建整个哈希，因此未被映射的数据会丢失。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

下面的例子显示了两个字节数组转换器（ converter）的示例。

Example 15. byte[] Converter 示例

```java
@WritingConverter
public class AddressToBytesConverter implements Converter<Address, byte[]> {

  private final Jackson2JsonRedisSerializer<Address> serializer;

  public AddressToBytesConverter() {

    serializer = new Jackson2JsonRedisSerializer<Address>(Address.class);
    serializer.setObjectMapper(new ObjectMapper());
  }

  @Override
  public byte[] convert(Address value) {
    return serializer.serialize(value);
  }
}

@ReadingConverter
public class BytesToAddressConverter implements Converter<byte[], Address> {

  private final Jackson2JsonRedisSerializer<Address> serializer;

  public BytesToAddressConverter() {

    serializer = new Jackson2JsonRedisSerializer<Address>(Address.class);
    serializer.setObjectMapper(new ObjectMapper());
  }

  @Override
  public Address convert(byte[] value) {
    return serializer.deserialize(value);
  }
}
```

使用前面的字节数组 `Converter` 会产生类似于以下的输出。

```text
_class = org.example.Person
id = e2c7dcee-b8cd-4424-883e-736ce564363e
firstname = rand
lastname = al’thor
address = { city : "emond's field", country : "andor" }
```

下面的例子显示了两个 `Map` converter 的例子。

Example 16. Map<String,byte[]> Converter 示例

```java
@WritingConverter
public class AddressToMapConverter implements Converter<Address, Map<String,byte[]>> {

  @Override
  public Map<String,byte[]> convert(Address source) {
    return singletonMap("ciudad", source.getCity().getBytes());
  }
}

@ReadingConverter
public class MapToAddressConverter implements Converter<Map<String, byte[]>, Address> {

  @Override
  public Address convert(Map<String,byte[]> source) {
    return new Address(new String(source.get("ciudad")));
  }
}
```

使用前面的 Map `Converter` 会产生类似以下的输出。

```text
_class = org.example.Person
id = e2c7dcee-b8cd-4424-883e-736ce564363e
firstname = rand
lastname = al’thor
ciudad = "emond's field"
```

|      | 自定义转换对索引解析没有影响。[二级索引](https://springdoc.cn/spring-data-redis/#redis.repositories.indexes) 仍然被创建，即使对于自定义转换的类型。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 13.3.1. 自定义类型映射

如果你想避免把整个Java类的名字写成类型信息，而宁愿使用一个key，你可以在被持久化的实体类上使用 `@TypeAlias` 注解。如果你需要更多的自定义映射，可以看看 [`TypeInformationMapper`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/convert/TypeInformationMapper.html) 接口。该接口的一个实例可以在 `DefaultRedisTypeMapper` 处配置，它可以在 `MappingRedisConverter` 上配置。

下面的例子显示了如何为一个实体定义一个类型别名。

Example 17. 为一个实体定义 `@TypeAlias`

```java
@TypeAlias("pers")
class Person {

}
```

由此产生的文档包含 `pers` 作为 `_class` 字段中的值。

##### 配置自定义类型映射

下面的例子演示了如何在 `MappingRedisConverter` 中配置一个自定义 `RedisTypeMapper`。

Example 18. 通过Spring Java配置自定义的 `RedisTypeMapper`

```java
class CustomRedisTypeMapper extends DefaultRedisTypeMapper {
  //implement custom type mapping here
}
@Configuration
class SampleRedisConfiguration {

  @Bean
  public MappingRedisConverter redisConverter(RedisMappingContext mappingContext,
        RedisCustomConversions customConversions, ReferenceResolver referenceResolver) {

    MappingRedisConverter mappingRedisConverter = new MappingRedisConverter(mappingContext, null, referenceResolver,
            customTypeMapper());

    mappingRedisConverter.setCustomConversions(customConversions);

    return mappingRedisConverter;
  }

  @Bean
  public RedisTypeMapper customTypeMapper() {
    return new CustomRedisTypeMapper();
  }
}
```

### 13.4. Keyspace（Key空间）

keyspace 定义了用于创建 Redis Hash 的实际key的前缀。默认情况下，前缀被设置为 `getClass().getName()`。你可以通过在 aggregate root 级别上设置 `@RedisHash` 或通过设置程序化配置来改变这个默认值。然而，注解的 keyspace 取代了任何其他配置。

下面的例子显示了如何用 `@EnableRedisRepositories` 注解来设置 keyspace 配置。

Example 19. 通过 `@EnableRedisRepositories` 配置 Keyspace

```java
@Configuration
@EnableRedisRepositories(keyspaceConfiguration = MyKeyspaceConfiguration.class)
public class ApplicationConfig {

  //... RedisConnectionFactory and RedisTemplate Bean definitions omitted

  public static class MyKeyspaceConfiguration extends KeyspaceConfiguration {

    @Override
    protected Iterable<KeyspaceSettings> initialConfiguration() {
      return Collections.singleton(new KeyspaceSettings(Person.class, "people"));
    }
  }
}
```

下面的例子显示了如何以编程方式设置 keyspace。

Example 20. 以编程方式设置 keyspace

```java
@Configuration
@EnableRedisRepositories
public class ApplicationConfig {

  //... RedisConnectionFactory and RedisTemplate Bean definitions omitted

  @Bean
  public RedisMappingContext keyValueMappingContext() {
    return new RedisMappingContext(
      new MappingConfiguration(new IndexConfiguration(), new MyKeyspaceConfiguration()));
  }

  public static class MyKeyspaceConfiguration extends KeyspaceConfiguration {

    @Override
    protected Iterable<KeyspaceSettings> initialConfiguration() {
      return Collections.singleton(new KeyspaceSettings(Person.class, "people"));
    }
  }
}
```

### 13.5. 二级索引

[Secondary indexes](https://redis.io/topics/indexes) （二级索引）是用来实现基于本地Redis结构的查询操作。每次保存时，数值会被写入相应的索引，当对象被删除或过期时，会被删除。

#### 13.5.1. 简单地属性索引

考虑到前面显示的 `Person` 实体的示例，我们可以通过用 `@Indexed` 注解该属性来为 `firstname` 创建一个索引，如下面的例子中所示。

Example 21. 注释驱动索引

```java
@RedisHash("people")
public class Person {

  @Id String id;
  @Indexed String firstname;
  String lastname;
  Address address;
}
```

索引是为实际的属性值建立的。保存两个 Person（例如，"rand" 和 "aviendha"）的结果是建立类似以下的索引。

```text
SADD people:firstname:rand e2c7dcee-b8cd-4424-883e-736ce564363e
SADD people:firstname:aviendha a9d4b3a0-50d3-4538-a2fc-f7fc2581ee56
```

在嵌套元素上也可以有索引。假设 `Address` 有一个 `city` 属性，该属性被 `@Indexed` 注解了。在这种情况下，一旦 `person.address.city` 不为 `null`，我们就有每个城市的 Set，如下面的例子所示。

```text
SADD people:address.city:tear e2c7dcee-b8cd-4424-883e-736ce564363e
```

此外，程序化设置可以让你在map key和 list 属性上定义索引，如下面的例子所示。

```java
@RedisHash("people")
public class Person {

  // ... other properties omitted

  Map<String,String> attributes;      
  Map<String Person> relatives;       
  List<Address> addresses;            
}
```

|      | `SADD people:attributes.map-key:map-value e2c7dcee-b8cd-4424-883e-736ce564363e` |
| ---- | ------------------------------------------------------------ |
|      | `SADD people:relatives.map-key.firstname:tam e2c7dcee-b8cd-4424-883e-736ce564363e` |
|      | `SADD people:addresses.city:tear e2c7dcee-b8cd-4424-883e-736ce564363e` |

|      | 索引不能在 [参考文献](https://springdoc.cn/spring-data-redis/#redis.repositories.references) 上解析。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

与 keyspace 一样，你可以配置索引而不需要注解实际的 domain 类型，如下面的例子所示。

Example 22. 用 `@EnableRedisRepositories` 设置索引

```java
@Configuration
@EnableRedisRepositories(indexConfiguration = MyIndexConfiguration.class)
public class ApplicationConfig {

  //... RedisConnectionFactory and RedisTemplate Bean definitions omitted

  public static class MyIndexConfiguration extends IndexConfiguration {

    @Override
    protected Iterable<IndexDefinition> initialConfiguration() {
      return Collections.singleton(new SimpleIndexDefinition("people", "firstname"));
    }
  }
}
```

同样，与 keyspace 一样，你可以以编程方式配置索引，如以下例子所示。

Example 23. 编程方式配置索引

```java
@Configuration
@EnableRedisRepositories
public class ApplicationConfig {

  //... RedisConnectionFactory and RedisTemplate Bean definitions omitted

  @Bean
  public RedisMappingContext keyValueMappingContext() {
    return new RedisMappingContext(
      new MappingConfiguration(
        new KeyspaceConfiguration(), new MyIndexConfiguration()));
  }

  public static class MyIndexConfiguration extends IndexConfiguration {

    @Override
    protected Iterable<IndexDefinition> initialConfiguration() {
      return Collections.singleton(new SimpleIndexDefinition("people", "firstname"));
    }
  }
}
```

#### 13.5.2. Geospatial Index （地理空间索引）

假设 `Address` 类型包含一个 `Point` 类型的 `location` 属性，它持有特定地址的地理坐标。通过用 `@GeoIndexed` 注解该属性，Spring Data Redis通过使用Redis `GEO` 命令添加这些值，如下例所示。

```java
@RedisHash("people")
public class Person {

  Address address;

  // ... other properties omitted
}

public class Address {

  @GeoIndexed Point location;

  // ... other properties omitted
}

public interface PersonRepository extends CrudRepository<Person, String> {

  List<Person> findByAddressLocationNear(Point point, Distance distance);     
  List<Person> findByAddressLocationWithin(Circle circle);                    
}

Person rand = new Person("rand", "al'thor");
rand.setAddress(new Address(new Point(13.361389D, 38.115556D)));

repository.save(rand);                                                        

repository.findByAddressLocationNear(new Point(15D, 37D), new Distance(200)); 
```

|      | 在查询方法上声明嵌套属性，使用 `Point` 和 `Distance`。       |
| ---- | ------------------------------------------------------------ |
|      | 在查询方法上声明嵌套属性，使用 `Circle` 来搜索范围。         |
|      | `GEOADD people:address:location 13.361389 38.115556 e2c7dcee-b8cd-4424-883e-736ce564363e` |
|      | `GEORADIUS people:address:location 15.0 37.0 200.0 km`       |

在前面的例子中，经度和纬度值是通过使用 `GEOADD` 来存储的，它使用对象的 `id` 作为成员的名字。查找方法允许使用 `Circle` 或 `Point`、`Distance` 组合来查询这些值。

|      | 不可能将 `near` 和 `within` 与其他 criteria 结合起来。 |
| ---- | ------------------------------------------------------ |
|      |                                                        |

### 13.6. Example 查询

#### 13.6.1. 介绍

本章介绍了 "Example 查询" 并解释了如何使用它。

Example 查询（QBE）是一种用户友好的查询技术，接口简单。它允许动态查询创建，不要求你写包含字段名的查询。事实上，"Example 查询" 根本不要求你通过使用store特定的查询语言来编写查询。

#### 13.6.2. 使用方式

Example 查询API由四部分组成。

- `Probe`: 带有填充字段的domain对象的实际例子。
- `ExampleMatcher` 承载了如何匹配特定字段的细节。它可以在多个实例中重复使用。
- `Example`: 一个 `Example` 由 probe 和 `ExampleMatcher` 组成。它被用来创建查询。
- `FetchableFluentQuery`: `FetchableFluentQuery` 提供了一个 fluent API，它允许进一步定制从 `Example` 衍生的查询。使用fluent API，你可以为你的查询指定排序投影和结果处理。

Example 查询很适合几种使用情况。

- 用一组静态或动态约束来查询你的data store。
- 频繁地重构domain对象而不用担心破坏现有的查询。
- 独立于底层 data store API 工作。

Example 查询也有一些限制。

- 不支持嵌套或分组的属性约束，如 `firstname = ?0 or (firstname = ?1 and lastname = ?2)`.。
- 对于字符串只支持 开始/包含/结束/regex 匹配，对于其他属性类型支持精确匹配。

在开始使用Example 查询之前，你需要有一个domain对象。为了开始，为你的 repository 创建一个接口，如下面的例子所示。

Example 24. Sample Person object

```java
public class Person {

  @Id
  private String id;
  private String firstname;
  private String lastname;
  private Address address;

  // … getters and setters omitted
}
```

前面的例子显示了一个简单的domain对象。你可以用它来创建一个 `Example`。默认情况下，具有 `null` 值的字段会被忽略，而字符串则通过使用store特定的默认值进行匹配。

|      | 将属性纳入 Example 查询的标准是基于非 null 的。使用原始类型（`int`, `double`, …）的属性总是被包括在内，除非 [`ExampleMatcher` 忽略了属性路径](https://springdoc.cn/spring-data-redis/#query-by-example.matchers)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

例子可以通过使用工厂方法或通过使用 [`ExampleMatcher`](https://springdoc.cn/spring-data-redis/#query-by-example.matchers) 来构建。`Example` 是不可改变的。下面的列表显示了一个简单的例子。

Example 25. Simple Example

```java
Person person = new Person();                         
person.setFirstname("Dave");                          

Example<Person> example = Example.of(person);         
```

|      | 创建一个新的 domain 对象的实例。 |
| ---- | -------------------------------- |
|      | 设置要查询的属性。               |
|      | 创建 `Example`。                 |

你可以通过使用 repository 来运行示例查询。要做到这一点，让你的 repository 接口扩展 `QueryByExampleExecutor<T>`。下面的列表显示了 `QueryByExampleExecutor` 接口的一个节选。

Example 26. `QueryByExampleExecutor`

```java
public interface QueryByExampleExecutor<T> {

  <S extends T> S findOne(Example<S> example);

  <S extends T> Iterable<S> findAll(Example<S> example);

  // … more functionality omitted.
}
```

#### 13.6.3. Example Matcher

示例不限于默认设置。你可以通过使用 `ExampleMatcher` 为字符串匹配、null处理和特定属性设置指定你自己的默认值，如下面的例子所示。

Example 27. Example matcher with customized matching

```java
Person person = new Person();                          
person.setFirstname("Dave");                           

ExampleMatcher matcher = ExampleMatcher.matching()     
  .withIgnorePaths("lastname")                         
  .withIncludeNullValues()                             
  .withStringMatcher(StringMatcher.ENDING);            

Example<Person> example = Example.of(person, matcher); 
```

|      | 创建一个新的domain 象的实例。                                |
| ---- | ------------------------------------------------------------ |
|      | 设置属性。                                                   |
|      | 创建一个 `ExampleMatcher`，期望所有的值都能匹配。在这个阶段，即使没有进一步的配置，它也是可用的。 |
|      | 构建一个新的 `ExampleMatcher` 来忽略 `lastname` 属性路径。   |
|      | 构建一个新的 `ExampleMatcher`，忽略 `lastname` 属性路径，并包含 null 值。 |
|      | 构建一个新的 `ExampleMatcher`，忽略 `lastname` 属性路径，包含 null 值，并进行后缀字符串匹配。 |
|      | 基于domain对象和配置的 `ExampleMatcher` 创建一个新的 `Example`。 |

默认情况下，`ExampleMatcher` 希望 probe 上设置的所有值都能匹配。如果你想得到与任何隐式定义的谓词（predicate）相匹配的结果，请使用 `ExampleMatcher.matchingAny()`。

你可以为单个属性（如 "firstname" 和 "lastname"，或者对于嵌套属性，"address.city"）指定行为。你可以用匹配选项和大小写敏感性来调整它，如下面的例子所示。

Example 28. 配置 matcher 属性

```java
ExampleMatcher matcher = ExampleMatcher.matching()
  .withMatcher("firstname", endsWith())
  .withMatcher("lastname", startsWith().ignoreCase());
}
```

配置 matcher 选项的另一种方法是使用 `lambda` （在Java 8中引入）。这种方法创建一个回调，要求实现者修改matcher。你不需要返回matcher，因为配置选项被保存在matcher实例中。下面的例子显示了一个使用lambda的matcher。

Example 29. 使用 lambda 配置 matcher option

```java
ExampleMatcher matcher = ExampleMatcher.matching()
  .withMatcher("firstname", match -> match.endsWith())
  .withMatcher("firstname", match -> match.startsWith());
}
```

`Example` 创建的查询使用的是配置的合并视图。默认的匹配设置可以在 `ExampleMatcher` 层面上设置，而个别设置可以应用于特定的属性路径。在 `ExampleMatcher` 上设置的设置会被属性路径设置所继承，除非它们被明确定义。属性补丁（patch）上的设置比默认设置有更高的优先权。下表描述了各种 `ExampleMatcher` 设置的范围。

| Setting              | Scope                              |
| :------------------- | :--------------------------------- |
| Null-handling        | `ExampleMatcher`                   |
| String matching      | `ExampleMatcher` and property path |
| Ignoring properties  | Property path                      |
| Case sensitivity     | `ExampleMatcher` and property path |
| Value transformation | Property path                      |

#### 13.6.4. Fluent API

`QueryByExampleExecutor` 还提供了一个方法，我们到目前为止还没有提到。`<S extends T, R> R findBy(Example<S> example, Function<FluentQuery.FetchableFluentQuery<S>, R> queryFunction)` 。和其他方法一样，它执行一个从 `Example` 派生的查询。然而，通过第二个参数，你可以控制该执行的各个方面，否则你无法动态地控制。你可以通过调用第二个参数中的 `FetchableFluentQuery` 的各种方法来做到这一点。 `sortBy` 让你为你的结果指定一个排序。`as` 让你指定你希望结果被转换的类型。`project` 限制了被查询的属性。`first`, `firstValue`, `one`, `oneValue`, `all`, `page`, `stream`, `count`, 和 `exists` 定义了你得到什么样的结果以及当超过预期结果数量时查询的行为方式。

Example 30. 使fluent API来获得可能是许多结果中的最后一个，按lastname排序。

```java
Optional<Person> match = repository.findBy(example,
    q -> q
        .sortBy(Sort.by("lastname").descending())
        .first()
);
```

#### 13.6.5. 使用 Example

下面的例子针对一个 repository 使用了 Example 查询。

Example 31. 使用 Repository 的 Example 查询

```java
interface PersonRepository extends QueryByExampleExecutor<Person> {
}

class PersonService {

  @Autowired PersonRepository personRepository;

  List<Person> findPeople(Person probe) {
    return personRepository.findAll(Example.of(probe));
  }
}
```

Redis Repository 通过其二级索引支持 Spring Data 的 Example 功能的一个子集。特别是，只有精确的、区分大小写的和非null的值才能被用来构建查询。

二级索引使用基于集合的操作（Set 交集，Set 并集）来确定匹配的key。在查询中添加一个没有索引的属性不会返回任何结果，因为没有索引存在。例子查询支持检查索引配置，以便在查询中只包括被索引覆盖的属性。这是为了防止意外地包含非索引的属性。

不区分大小写的查询和不支持的 `StringMatcher` 实例在运行时被拒绝。

下面的列表显示了支持的 "Example 查询" 选项。

- 对简单和嵌套属性的大小写敏感、精确匹配。
- Any/All 匹配模式。
- 对 `criteria` 值进行 Value 转换。
- 从 criteria 中排除 `null` 值。

The following list shows properties not supported by Query by Example:

下面的列表显示了不被 "Example 查询" 支持的属性。

- 不区分大小写的匹配
- Regex, prefix/contains/suffix 字符串匹配
- 查询关联、Collection和类似Map的属性
- 将 `null` 值纳入 criteria 中
- 带排序的 `findAll`

### 13.7. 生命周期

存储在Redis中的对象可能只在一定时间内有效。这对于在Redis中持久化短命的对象特别有用，而不必在它们到达生命终点时手动删除它们。过期时间以秒为单位，可以通过 `@RedisHash(timeToLive=…)` 以及使用 `KeyspaceSettings`（见 [Keyspace（Key空间）](https://springdoc.cn/spring-data-redis/#redis.repositories.keyspaces)）来设置。

通过在数字属性或方法上使用 `@TimeToLive` 注解，可以设置更灵活的过期时间。然而，不要在同一个类中的一个方法和一个属性上同时应用 `@TimeToLive`。下面的例子显示了在一个属性和一个方法上的 `@TimeToLive` 注解。

Example 32. 过期时间设置

```java
public class TimeToLiveOnProperty {

  @Id
  private String id;

  @TimeToLive
  private Long expiration;
}

public class TimeToLiveOnMethod {

  @Id
  private String id;

  @TimeToLive
  public long getTimeToLive() {
  	return new Random().nextLong();
  }
}
```

|      | 用 `@TimeToLive` 明确地注解一个属性，从Redis读回实际的 `TTL` 或 `PTTL` 值。`-1` 表示该对象没有相关的过期时间。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

repository 实现确保通过 `RedisMessageListenerContainer` 订阅 [Redis keyspace 通知](https://redis.io/topics/notifications) 。

当过期时间被设置为正值时，相应的 `EXPIRE` 命令就会运行。除了持久化原始的，还有一个幻象副本被持久化在 Redis 中，并被设置为在原始副本的五分钟后过期。这样做是为了让 `Repository` 支持发布 `RedisKeyExpiredEvent`，每当key过期时，在Spring的 `ApplicationEventPublisher` 中持有过期值，尽管原始值已经被删除。所有使用Spring Data Redis Repository 的连接应用程序都会收到过期事件。

默认情况下，key 过期监听器在初始化应用程序时被禁用。启动模式可以在 `@EnableRedisRepositories` 或 `RedisKeyValueAdapter` 中调整，以便与应用程序一起启动监听器，或在首次插入具有 `TTL` 的实体时启动监听器。请参阅 [`EnableKeyspaceEvents`](https://docs.spring.io/spring-data/redis/docs/3.1.0-SNAPSHOT/api/org/springframework/data/redis/core/RedisKeyValueAdapter.EnableKeyspaceEvents.html) 了解可能的值。

`RedisKeyExpiredEvent` 持有一份过期 domain 对象的副本以及 key。

|      | 延迟或禁用过期事件监听器的启动会影响 `RedisKeyExpiredEvent` 的发布。一个被禁用的事件监听器不会发布过期事件。延迟启动可能会因为延迟的监听器初始化而导致事件的丢失。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | keyspace 通知消息监听器改变了Redis中的 `notify-keyspace-events` 设置，如果这些设置还没有被设置。现有的设置不会被覆盖，所以你必须正确地设置这些设置（或者让它们为空）。注意，在AWS ElastiCache 上 `CONFIG` 是禁用的，启用监听器会导致一个错误。为了解决这种行为，将 `keyspaceNotificationsConfigParameter` 参数设置为空字符串。这可以防止 `CONFIG` 命令的使用。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | Redis的Pub/Sub消息是不持久的。如果一个key在应用程序关闭时过期，过期事件不会被处理，这可能导致二级索引包含对过期对象的引用。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | `@EnableKeyspaceEvents(shadowCopy = OFF)` 禁止存储幻影副本，并减少Redis内的数据大小。`RedisKeyExpiredEvent` 将只包含过期 key 的 `id`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 13.8. 持久化引用

用 `@Reference` 来标记属性，可以存储一个简单的 key 引用，而不是将值复制到哈希本身。在从Redis加载时，引用被自动解析并映射回对象中，如下例所示。

Example 33. 属性引用示例

```text
_class = org.example.Person
id = e2c7dcee-b8cd-4424-883e-736ce564363e
firstname = rand
lastname = al’thor
mother = people:a9d4b3a0-50d3-4538-a2fc-f7fc2581ee56      
```

|      | 引用存储被引用对象的整个key （`keyspace:id`）。 |
| ---- | ----------------------------------------------- |
|      |                                                 |

|      | 当引用对象被保存时，被引用对象不会被持久化。你必须单独坚持对被引用对象的改变，因为只有引用被存储。在被引用类型的属性上设置的索引不被解析。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 13.9. 持久化部分更新

在某些情况下，你不需要加载和重写整个实体，只是在其中设置一个新的值。最后活动时间的会话时间戳可能就是这样一种情况，你想改变一个属性。`PartialUpdate` 让你在现有的对象上定义 `set` 和 `delete` 操作，同时照顾到更新实体本身和索引结构的潜在过期时间。下面的例子显示了一个部分更新。

Example 34. 部分更新示例

```java
PartialUpdate<Person> update = new PartialUpdate<Person>("e2c7dcee", Person.class)
  .set("firstname", "mat")                                                           
  .set("address.city", "emond's field")                                              
  .del("age");                                                                       

template.update(update);

update = new PartialUpdate<Person>("e2c7dcee", Person.class)
  .set("address", new Address("caemlyn", "andor"))                                   
  .set("attributes", singletonMap("eye-color", "grey"));                             

template.update(update);

update = new PartialUpdate<Person>("e2c7dcee", Person.class)
  .refreshTtl(true);                                                                 
  .set("expiration", 1000);

template.update(update);
```

|      | 将简单的 `firstname` 属性设置为mat。                         |
| ---- | ------------------------------------------------------------ |
|      | 将简单的 'address.city' 属性设置为 'emond' 字段，而不需要传入整个对象。当注册了一个自定义转换时，这不起作用。 |
|      | 删除 `age` 属性。                                            |
|      | 设置复合的 `address` 属性。                                  |
|      | 设置一个 value 的 map，删除之前存在的 map，用给定的值替换。  |
|      | 在改变 [生命周期](https://springdoc.cn/spring-data-redis/#redis.repositories.expirations) 时，自动更新服务器的过期时间。 |

|      | 更新复合的对象以及map（或其他collection）结构需要与Redis进一步互动以确定现有值，这意味着重写整个实体可能会更快。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 13.10. 查询和查询方法

查询方法允许从方法名称中自动推导出简单的搜索器查询，如以下例子所示。

Example 35. Repository 查找方法示例

```java
public interface PersonRepository extends CrudRepository<Person, String> {

  List<Person> findByFirstname(String firstname);
}
```

|      | 请确保finder方法中使用的属性是为索引而设置的。 |
| ---- | ---------------------------------------------- |
|      |                                                |

|      | Redis Repository 的查询方法只支持对实体和实体集合的分页查询。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

使用派生查询方法可能并不总是足以建立要运行的查询模型。`RedisCallback` 提供了对索引结构甚至是自定义索引的实际匹配的更多控制。要做到这一点，提供一个 `RedisCallback`，返回一个单一的或 `Iterable` 的 `id` 值集合，如下面的例子所示。

Example 36. 使用 RedisCallback 的查找器示例

```java
String user = //...

List<RedisSession> sessionsByUser = template.find(new RedisCallback<Set<byte[]>>() {

  public Set<byte[]> doInRedis(RedisConnection connection) throws DataAccessException {
    return connection
      .sMembers("sessions:securityContext.authentication.principal.username:" + user);
  }}, RedisSession.class);
```

下表提供了Redis支持的关键字的概述，以及包含该关键字的方法基本上转化为什么。

| 关键字       | 示例                                                         | Redis 片段                                   |
| :----------- | :----------------------------------------------------------- | :------------------------------------------- |
| `And`        | `findByLastnameAndFirstname`                                 | `SINTER …:firstname:rand …:lastname:al’thor` |
| `Or`         | `findByLastnameOrFirstname`                                  | `SUNION …:firstname:rand …:lastname:al’thor` |
| `Is, Equals` | `findByFirstname`, `findByFirstnameIs`, `findByFirstnameEquals` | `SINTER …:firstname:rand`                    |
| `IsTrue`     | `FindByAliveIsTrue`                                          | `SINTER …:alive:1`                           |
| `IsFalse`    | `findByAliveIsFalse`                                         | `SINTER …:alive:0`                           |
| `Top,First`  | `findFirst10ByFirstname`,`findTop5ByFirstname`               |                                              |

#### 13.10.1. 对查询方法的结果进行排序

Redis Repository 允许采用各种方法来定义排序顺序。Redis 本身不支持在检索哈希值或集合时进行排序。因此，Redis repository 的查询方法构建了一个 `Comparator`，在将结果作为 `List` 返回之前，该 `Comparator` 被应用于结果。让我们看一下下面的例子。

Example 37. 对查询方法的结果进行排序

```java
interface PersonRepository extends RedisRepository<Person, String> {

  List<Person> findByFirstnameOrderByAgeDesc(String firstname); 

  List<Person> findByFirstname(String firstname, Sort sort);   
}
```

|      | 静态排序来自于方法名称。   |
| ---- | -------------------------- |
|      | 使用方法参数进行动态排序。 |

### 13.11. 在 Redis 集群上使用 Repository

你可以在集群的Redis环境中使用 Redis repository 支持。参见 “[Redis 集群](https://springdoc.cn/spring-data-redis/#cluster)” 部分，了解 `ConnectionFactory` 的配置细节。尽管如此，仍然必须做一些额外的配置，因为默认的 key 分布将实体和二级索引分散到整个集群及其槽中。

下表显示了一个集群上的数据细节（基于以前的例子）。

| Key                                         | 类型        | Slot（插槽） | 节点           |
| :------------------------------------------ | :---------- | :----------- | :------------- |
| people:e2c7dcee-b8cd-4424-883e-736ce564363e | id for hash | 15171        | 127.0.0.1:7381 |
| people:a9d4b3a0-50d3-4538-a2fc-f7fc2581ee56 | id for hash | 7373         | 127.0.0.1:7380 |
| people:firstname:rand                       | index       | 1700         | 127.0.0.1:7379 |

有些命令（如 `SINTER` 和 `SUNION`）只有在所有涉及的 key 都映射到同一槽时才能在服务器端处理。否则，计算就必须在客户端完成。因此，把 keyspaces 绑定在一个槽上是很有用的，这样就可以立即利用Redis服务器端的计算。下表显示了你这样做时发生的情况（注意插槽列的变化和节点列的端口值）。

| Key                                           | 类型        | Slot（插槽） | 节点           |
| :-------------------------------------------- | :---------- | :----------- | :------------- |
| {people}:e2c7dcee-b8cd-4424-883e-736ce564363e | id for hash | 2399         | 127.0.0.1:7379 |
| {people}:a9d4b3a0-50d3-4538-a2fc-f7fc2581ee56 | id for hash | 2399         | 127.0.0.1:7379 |
| {people}:firstname:rand                       | index       | 2399         | 127.0.0.1:7379 |

|      | 当你使用Redis集群时，通过使用 `@RedisHash("{yourkeyspace}")` 来定义和绑定 keyspace 到特定的槽。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 13.12. CDI 整合

repository 接口的实例通常由容器创建，在使用Spring Data时，Spring是最自然的选择。Spring为创建Bean实例提供了复杂的方法。Spring Data Redis有一个自定义的CDI扩展，可以让你在CDI环境中使用 repository 抽象。该扩展是JAR的一部分，因此，要激活它，请将Spring Data Redis JAR 放入你的classpath。

然后你可以通过为 `RedisConnectionFactory` 和 `RedisOperations` 实现CDI Producer 来设置基础设施，如下例所示。

```java
class RedisOperationsProducer {


  @Produces
  RedisConnectionFactory redisConnectionFactory() {

    LettuceConnectionFactory connectionFactory = new LettuceConnectionFactory(new RedisStandaloneConfiguration());
    connectionFactory.afterPropertiesSet();

    return connectionFactory;
  }

  void disposeRedisConnectionFactory(@Disposes RedisConnectionFactory redisConnectionFactory) throws Exception {

    if (redisConnectionFactory instanceof DisposableBean) {
      ((DisposableBean) redisConnectionFactory).destroy();
    }
  }

  @Produces
  @ApplicationScoped
  RedisOperations<byte[], byte[]> redisOperationsProducer(RedisConnectionFactory redisConnectionFactory) {

    RedisTemplate<byte[], byte[]> template = new RedisTemplate<byte[], byte[]>();
    template.setConnectionFactory(redisConnectionFactory);
    template.afterPropertiesSet();

    return template;
  }

}
```

必要的设置会有所不同，这取决于你的JavaEE环境。

Spring Data Redis CDI扩展将所有可用的 repository 作为CDI Bean，并在容器请求 repository 类型的 Bean 时为 Spring Data Repository 创建一个代理。因此，获得一个Spring Data Repository 的实例只需声明一个 `@Injected` 属性，如下例所示。

```java
class RepositoryClient {

  @Inject
  PersonRepository repository;

  public void businessMethod() {
    List<Person> people = repository.findAll();
  }
}
```

Redis Repository 需要 `RedisKeyValueAdapter` 和 `RedisKeyValueTemplate` 实例。如果没有找到提供的 Bean，这些 Bean 将由Spring Data CDI 扩展创建和管理。然而，你可以提供你自己的 Bean 来配置 `RedisKeyValueAdapter` 和 `RedisKeyValueTemplate` 的特定属性。

### 13.13. Redis Repository 剖析

Redis作为一个存储本身提供了一个非常有限的低级别的API，将更高级别的功能，如二级索引和查询操作，留给了用户。

本节提供了对 repository 抽象所发出的命令的更详细的看法，以便更好地理解潜在的性能影响。

考虑将以下实体类作为所有操作的起点。

Example 38. Example entity

```java
@RedisHash("people")
public class Person {

  @Id String id;
  @Indexed String firstname;
  String lastname;
  Address hometown;
}

public class Address {

  @GeoIndexed Point location;
}
```

#### 13.13.1. Insert new

```java
repository.save(new Person("rand", "al'thor"));
HMSET "people:19315449-cda2-4f5c-b696-9cb8018fa1f9" "_class" "Person" "id" "19315449-cda2-4f5c-b696-9cb8018fa1f9" "firstname" "rand" "lastname" "al'thor" 
SADD  "people" "19315449-cda2-4f5c-b696-9cb8018fa1f9"                           
SADD  "people:firstname:rand" "19315449-cda2-4f5c-b696-9cb8018fa1f9"            
SADD  "people:19315449-cda2-4f5c-b696-9cb8018fa1f9:idx" "people:firstname:rand" 
```

|      | 将扁平化的条目保存为hash。                                   |
| ---- | ------------------------------------------------------------ |
|      | 将写在 <1> 中的 hash 的 key 添加到同一 keyspace 的实体的辅助索引中。 |
|      | 将写在 <2> 中的 hash key 添加到带有属性值的 firstnames 的二级索引中。 |
|      | 将 <3> 的索引添加到 entry 的辅助结构集合中，以记录删除/更新时要清理的索引。 |

#### 13.13.2. 替换现有的

```java
repository.save(new Person("e82908cf-e7d3-47c2-9eec-b4e0967ad0c9", "Dragon Reborn", "al'thor"));
DEL       "people:e82908cf-e7d3-47c2-9eec-b4e0967ad0c9"                           
HMSET     "people:e82908cf-e7d3-47c2-9eec-b4e0967ad0c9" "_class" "Person" "id" "e82908cf-e7d3-47c2-9eec-b4e0967ad0c9" "firstname" "Dragon Reborn" "lastname" "al'thor" 
SADD      "people" "e82908cf-e7d3-47c2-9eec-b4e0967ad0c9"                         
SMEMBERS  "people:e82908cf-e7d3-47c2-9eec-b4e0967ad0c9:idx"                       
TYPE      "people:firstname:rand"                                                 
SREM      "people:firstname:rand" "e82908cf-e7d3-47c2-9eec-b4e0967ad0c9"          
DEL       "people:e82908cf-e7d3-47c2-9eec-b4e0967ad0c9:idx"                       
SADD      "people:firstname:Dragon Reborn" "e82908cf-e7d3-47c2-9eec-b4e0967ad0c9" 
SADD      "people:e82908cf-e7d3-47c2-9eec-b4e0967ad0c9:idx" "people:firstname:Dragon Reborn" 
```

|      | 删除现有的hash，以避免可能不再存在的 hash key 的遗留。       |
| ---- | ------------------------------------------------------------ |
|      | 将扁平化的条目保存为hash。                                   |
|      | 将写在 <1> 中的 hash 的 key 添加到同一 keyspace 的实体的辅助索引中。 |
|      | 获取可能需要更新的现有索引结构。                             |
|      | 检查索引是否存在以及它的类型（text, geo）。                  |
|      | 从索引中删除一个可能存在的key。                              |
|      | 删除保存索引信息的助手。                                     |
|      | 将 <2> 中添加的 hash 的 key 添加到带有属性值的 firstnames 的二级索引。 |
|      | 将 <6> 的索引添加到入口的辅助结构集合中，以记录删除/更新时要清理的索引。 |

#### 13.13.3. 保存 Geo 数据

Geo 索引遵循与普通文本索引相同的规则，但使用 Geo 结构来存储数值。保存一个使用 Geo 索引属性的实体，会出现以下命令。

```text
GEOADD "people:hometown:location" "13.361389" "38.115556" "76900e94-b057-44bc-abcf-8126d51a621b"  
SADD   "people:76900e94-b057-44bc-abcf-8126d51a621b:idx" "people:hometown:location"               
```

|      | 将保存的条目的 key 添加到 geo 索引中。 |
| ---- | -------------------------------------- |
|      | 追踪索引结构。                         |

#### 13.13.4. 使用简单的索引查找

```java
repository.findByFirstname("egwene");
SINTER  "people:firstname:egwene"                     
HGETALL "people:d70091b5-0b9a-4c0a-9551-519e61bc9ef3" 
HGETALL ...
```

|      | 获取二级索引中包含的 key。    |
| ---- | ----------------------------- |
|      | 单独获取由 <1>返回的每个key。 |

#### 13.13.5. 使用 Geo 索引查找

```java
repository.findByHometownLocationNear(new Point(15, 37), new Distance(200, KILOMETERS));
GEORADIUS "people:hometown:location" "15.0" "37.0" "200.0" "km" 
HGETALL   "people:76900e94-b057-44bc-abcf-8126d51a621b"         
HGETALL   ...
```

|      | 获取二级索引中包含的 key。    |
| ---- | ----------------------------- |
|      | 单独获取由 <1> 返回的每个键。 |

# Appendixes

## 附录文档结构

该附录包含各种额外的细节，补充了参考文件其他部分的信息。

- “[Schema](https://springdoc.cn/spring-data-redis/#appendix:schema)” 定义了由Spring Data Redis提供的schema。
- “[命令参考](https://springdoc.cn/spring-data-redis/#appendix:command-reference)” 详细说明 `RedisTemplate` 支持哪些命令。

## Appendix A: Schema

[Spring Data Redis Schema (redis-namespace)](https://www.springframework.org/schema/redis/spring-redis-1.0.xsd)

## Appendix B: 命令参考

### 支持的命令

| 命令                            | Template 支持 |
| :------------------------------ | :-----------: |
| APPEND                          |       X       |
| AUTH                            |       X       |
| BGREWRITEAOF                    |       X       |
| BGSAVE                          |       X       |
| BITCOUNT                        |       X       |
| BITFIELD                        |       X       |
| BITOP                           |       X       |
| BLPOP                           |       X       |
| BRPOP                           |       X       |
| BRPOPLPUSH                      |       X       |
| CLIENT KILL                     |       X       |
| CLIENT GETNAME                  |       X       |
| CLIENT LIST                     |       X       |
| CLIENT SETNAME                  |       X       |
| CLUSTER SLOTS                   |       -       |
| COMMAND                         |       -       |
| COMMAND COUNT                   |       -       |
| COMMAND GETKEYS                 |       -       |
| COMMAND INFO                    |       -       |
| CONFIG GET                      |       X       |
| CONFIG RESETSTAT                |       X       |
| CONFIG REWRITE                  |       -       |
| CONFIG SET                      |       X       |
| DBSIZE                          |       X       |
| DEBUG OBJECT                    |       -       |
| DEBUG SEGFAULT                  |       -       |
| DECR                            |       X       |
| DECRBY                          |       X       |
| DEL                             |       X       |
| DISCARD                         |       X       |
| DUMP                            |       X       |
| ECHO                            |       X       |
| EVAL                            |       X       |
| EVALSHA                         |       X       |
| EXEC                            |       X       |
| EXISTS                          |       X       |
| EXPIRE                          |       X       |
| EXPIREAT                        |       X       |
| FLUSHALL                        |       X       |
| FLUSHDB                         |       X       |
| GEOADD                          |       X       |
| GEODIST                         |       X       |
| GEOHASH                         |       X       |
| GEOPOS                          |       X       |
| GEORADIUS                       |       X       |
| GEORADIUSBYMEMBER               |       X       |
| GEOSEARCH                       |       X       |
| GEOSEARCHSTORE                  |       X       |
| GET                             |       X       |
| GETBIT                          |       X       |
| GETRANGE                        |       X       |
| GETSET                          |       X       |
| HDEL                            |       X       |
| HEXISTS                         |       X       |
| HGET                            |       X       |
| HGETALL                         |       X       |
| HINCRBY                         |       X       |
| HINCRBYFLOAT                    |       X       |
| HKEYS                           |       X       |
| HLEN                            |       X       |
| HMGET                           |       X       |
| HMSET                           |       X       |
| HSCAN                           |       X       |
| HSET                            |       X       |
| HSETNX                          |       X       |
| HVALS                           |       X       |
| INCR                            |       X       |
| INCRBY                          |       X       |
| INCRBYFLOAT                     |       X       |
| INFO                            |       X       |
| KEYS                            |       X       |
| LASTSAVE                        |       X       |
| LINDEX                          |       X       |
| LINSERT                         |       X       |
| LLEN                            |       X       |
| LPOP                            |       X       |
| LPUSH                           |       X       |
| LPUSHX                          |       X       |
| LRANGE                          |       X       |
| LREM                            |       X       |
| LSET                            |       X       |
| LTRIM                           |       X       |
| MGET                            |       X       |
| MIGRATE                         |       -       |
| MONITOR                         |       -       |
| MOVE                            |       X       |
| MSET                            |       X       |
| MSETNX                          |       X       |
| MULTI                           |       X       |
| OBJECT                          |       -       |
| PERSIST                         |       X       |
| PEXIPRE                         |       X       |
| PEXPIREAT                       |       X       |
| PFADD                           |       X       |
| PFCOUNT                         |       X       |
| PFMERGE                         |       X       |
| PING                            |       X       |
| PSETEX                          |       X       |
| PSUBSCRIBE                      |       X       |
| PTTL                            |       X       |
| PUBLISH                         |       X       |
| PUBSUB                          |       -       |
| PUBSUBSCRIBE                    |       -       |
| QUIT                            |       X       |
| RANDOMKEY                       |       X       |
| RENAME                          |       X       |
| RENAMENX                        |       X       |
| REPLICAOF                       |       X       |
| RESTORE                         |       X       |
| ROLE                            |       -       |
| RPOP                            |       X       |
| RPOPLPUSH                       |       X       |
| RPUSH                           |       X       |
| RPUSHX                          |       X       |
| SADD                            |       X       |
| SAVE                            |       X       |
| SCAN                            |       X       |
| SCARD                           |       X       |
| SCRIPT EXITS                    |       X       |
| SCRIPT FLUSH                    |       X       |
| SCRIPT KILL                     |       X       |
| SCRIPT LOAD                     |       X       |
| SDIFF                           |       X       |
| SDIFFSTORE                      |       X       |
| SELECT                          |       X       |
| SENTINEL FAILOVER               |       X       |
| SENTINEL GET-MASTER-ADD-BY-NAME |       -       |
| SENTINEL MASTER                 |       -       |
| SENTINEL MASTERS                |       X       |
| SENTINEL MONITOR                |       X       |
| SENTINEL REMOVE                 |       X       |
| SENTINEL RESET                  |       -       |
| SENTINEL SET                    |       -       |
| SENTINEL SLAVES                 |       X       |
| SET                             |       X       |
| SETBIT                          |       X       |
| SETEX                           |       X       |
| SETNX                           |       X       |
| SETRANGE                        |       X       |
| SHUTDOWN                        |       X       |
| SINTER                          |       X       |
| SINTERSTORE                     |       X       |
| SISMEMBER                       |       X       |
| SLAVEOF                         |       X       |
| SLOWLOG                         |       -       |
| SMEMBERS                        |       X       |
| SMOVE                           |       X       |
| SORT                            |       X       |
| SPOP                            |       X       |
| SRANDMEMBER                     |       X       |
| SREM                            |       X       |
| SSCAN                           |       X       |
| STRLEN                          |       X       |
| SUBSCRIBE                       |       X       |
| SUNION                          |       X       |
| SUNIONSTORE                     |       X       |
| SYNC                            |       -       |
| TIME                            |       X       |
| TTL                             |       X       |
| TYPE                            |       X       |
| UNSUBSCRIBE                     |       X       |
| UNWATCH                         |       X       |
| WATCH                           |       X       |
| ZADD                            |       X       |
| ZCARD                           |       X       |
| ZCOUNT                          |       X       |
| ZINCRBY                         |       X       |
| ZINTERSTORE                     |       X       |
| ZLEXCOUNT                       |       -       |
| ZRANGE                          |       X       |
| ZRANGEBYLEX                     |       -       |
| ZREVRANGEBYLEX                  |       -       |
| ZRANGEBYSCORE                   |       X       |
| ZRANGESTORE                     |       X       |
| ZRANK                           |       X       |
| ZREM                            |       X       |
| ZREMRANGEBYLEX                  |       -       |
| ZREMRANGEBYRANK                 |       X       |
| ZREVRANGE                       |       X       |
| ZREVRANGEBYSCORE                |       X       |
| ZREVRANK                        |       X       |
| ZSCAN                           |       X       |
| ZSCORE                          |       X       |
| ZUNINONSTORE                    |       X       |

## Appendix C: 迁移指南

本节包含了关于迁移步骤、废弃和移除的细节。

### 从2.x升级到3.x

#### 重新/移动的类型

| 类型                                               | 替换                                  |
| :------------------------------------------------- | :------------------------------------ |
| o.s.d.redis.Version                                | o.s.d.util.Version                    |
| o.s.d.redis.VersionParser                          | -                                     |
| o.s.d.redis.connection.RedisZSetCommands.Aggregate | o.s.d.redis.connection.zset.Aggregate |
| o.s.d.redis.connection.RedisZSetCommands.Tuple     | o.s.d.redis.connection.zset.Tuple     |
| o.s.d.redis.connection.RedisZSetCommands.Weights   | o.s.d.redis.connection.zset.Weights   |
| o.s.d.redis.connection.RedisZSetCommands.Range     | o.s.d.domain.Range                    |
| o.s.d.redis.connection.RedisZSetCommands.Limit     | o.s.d.redis.connection.Limit.java     |
| o.s.d.redis.connection.jedis.JedisUtils            | -                                     |
| o.s.d.redis.connection.jedis.JedisVersionUtil      | -                                     |
| o.s.d.redis.core.convert.CustomConversions         | o.s.d.convert.CustomConversions       |

#### 更改的方法和类型

| 类型                                              | 方法      | 替代              |
| :------------------------------------------------ | :-------- | :---------------- |
| o.s.d.redis.core.Cursor                           | open      | -                 |
| o.s.d.redis.core.RedisTemplate                    | execute   | doWithKeys        |
| o.s.d.redis.stream.StreamMessageListenerContainer | isAutoAck | isAutoAcknowledge |
| o.s.d.redis.stream.StreamMessageListenerContainer | autoAck   | autoAcknowledge   |

| Type                                                         | Method                   | Replacement                |
| :----------------------------------------------------------- | :----------------------- | :------------------------- |
| o.s.d.redis.connection.ClusterCommandExecutionFailureException | getCauses                | getSuppressed              |
| o.s.d.redis.connection.RedisConnection                       | bgWriteAof               | bgReWriteAof               |
| o.s.d.redis.connection.RedisConnection                       | slaveOf                  | replicaOf                  |
| o.s.d.redis.connection.RedisConnection                       | slaveOfNoOne             | replicaOfNoOne             |
| o.s.d.redis.connection.ReactiveClusterCommands               | clusterGetSlaves         | clusterGetReplicas         |
| o.s.d.redis.connection.ReactiveClusterCommands               | clusterGetMasterSlaveMap | clusterGetMasterReplicaMap |
| o.s.d.redis.connection.ReactiveKeyCommands                   | getNewName               | getNewKey                  |
| o.s.d.redis.connection.RedisClusterNode.Flag                 | SLAVE                    | REPLICA                    |
| o.s.d.redis.connection.RedisClusterNode.Builder              | slaveOf                  | replicaOf                  |
| o.s.d.redis.connection.RedisNode                             | isSlave                  | isReplica                  |
| o.s.d.redis.connection.RedisSentinelCommands                 | slaves                   | replicas                   |
| o.s.d.redis.connection.RedisServer                           | getNumberSlaves          | getNumberReplicas          |
| o.s.d.redis.connection.RedisServerCommands                   | slaveOf                  | replicaOf                  |
| o.s.d.redis.core.ClusterOperations                           | getSlaves                | getReplicas                |
| o.s.d.redis.core.RedisOperations                             | slaveOf                  | replicaOf                  |

| Type                                                | Method            | Replacement |
| :-------------------------------------------------- | :---------------- | :---------- |
| o.s.d.redis.core.GeoOperations & BoundGeoOperations | geoAdd            | add         |
| o.s.d.redis.core.GeoOperations & BoundGeoOperations | geoDist           | distance    |
| o.s.d.redis.core.GeoOperations & BoundGeoOperations | geoHash           | hash        |
| o.s.d.redis.core.GeoOperations & BoundGeoOperations | geoPos            | position    |
| o.s.d.redis.core.GeoOperations & BoundGeoOperations | geoRadius         | radius      |
| o.s.d.redis.core.GeoOperations & BoundGeoOperations | geoRadiusByMember | radius      |
| o.s.d.redis.core.GeoOperations & BoundGeoOperations | geoRemove         | remove      |

| Type                                      | Method         | Replacement         |
| :---------------------------------------- | :------------- | :------------------ |
| o.s.d.redis.cache.RedisCacheConfiguration | prefixKeysWith | prefixCacheNameWith |
| o.s.d.redis.cache.RedisCacheConfiguration | getKeyPrefix   | getKeyPrefixFor     |

#### Jedis

请阅读Jedis的 [升级指南](https://github.com/redis/jedis/blob/v4.0.0/docs/3to4.md)，其中包括重要的驱动程序变化。

| Type                                                | Method                | Replacement                                                  |
| :-------------------------------------------------- | :-------------------- | :----------------------------------------------------------- |
| o.s.d.redis.connection.jedis.JedisConnectionFactory | getShardInfo          | *can be obtained via JedisClientConfiguration*               |
| o.s.d.redis.connection.jedis.JedisConnectionFactory | setShardInfo          | *can be set via JedisClientConfiguration*                    |
| o.s.d.redis.connection.jedis.JedisConnectionFactory | createCluster         | *now requires a `Connection` instead of `Jedis` instance*    |
| o.s.d.redis.connection.jedis.JedisConverters        |                       | has package visibility now                                   |
| o.s.d.redis.connection.jedis.JedisConverters        | tuplesToTuples        | -                                                            |
| o.s.d.redis.connection.jedis.JedisConverters        | tuplesToTuples        | -                                                            |
| o.s.d.redis.connection.jedis.JedisConverters        | stringListToByteList  | -                                                            |
| o.s.d.redis.connection.jedis.JedisConverters        | stringSetToByteSet    | -                                                            |
| o.s.d.redis.connection.jedis.JedisConverters        | stringMapToByteMap    | -                                                            |
| o.s.d.redis.connection.jedis.JedisConverters        | tupleSetToTupleSet    | -                                                            |
| o.s.d.redis.connection.jedis.JedisConverters        | toTupleSet            | -                                                            |
| o.s.d.redis.connection.jedis.JedisConverters        | toDataAccessException | o.s.d.redis.connection.jedis.JedisExceptionConverter#convert |

#### 事务 / 管道

管道和事务现在是相互排斥的。在管道/事务模式下使用服务器或连接命令已不再可能。

#### Lettuce

##### Lettuce 连接池

`LettucePool` 和它的实现 `DefaultLettucePool` 已经被删除，没有替换。请参考 [驱动文档](https://lettuce.io/core/release/reference/index.html#_connection_pooling)，了解驱动的本地池功能。接受池化参数的方法已经更新。这影响了 `LettuceConnectionFactory` 和 `LettuceConnection` 的方法。

##### Lettuce 认证

`AuthenticatingRedisClient` 已经被删除，没有替换。请参考 `RedisURI` 的 [驱动文档](https://lettuce.io/core/release/reference/index.html#basic.redisuri) 来设置认证数据。

Version 3.1.0-SNAPSHOT
Last updated 2023-02-02 18:54:33 +0800

[主页](https://springdoc.cn/docs/)