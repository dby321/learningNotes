# MongoDB笔记

## 1. MongoDB相关概念

### 业务使用场景

- 高并发、高存储、高可用、高扩展性场景下MySql有点力不从心
- 应用场景
  - 社交场景
  - 游戏场景
  - 物流场景
  - 物联网场景
  - 视频直播
- 数据操作的共同点
  - 数据量大
  - 写入操作频繁
  - 价值比较低，对事务要求不高
  - 需求会经常变动
  - 应用需要高存储

### 主要数据库的对比

| 特性           | MySQL                        | Redis                              | MongoDB                            | Elasticsearch                        |
| -------------- | ---------------------------- | ---------------------------------- | ---------------------------------- | ------------------------------------ |
| 类型           | 关系型数据库 (RDBMS)         | 键值存储 (In-Memory Data Store)    | 文档型数据库 (NoSQL)               | 搜索引擎 (Distributed Search Engine) |
| 数据模型       | 表结构，行与列               | 键值对                             | JSON-like 文档                     | JSON-like 文档，优化搜索             |
| 数据一致性     | 强一致性                     | 最终一致性（取决于配置）           | 最终一致性                         | 最终一致性                           |
| 主要使用场景   | 交易系统，需要ACID属性的场合 | 缓存，实时分析，计数器等           | 内容管理，大数据，灵活模式         | 全文搜索，日志分析，推荐系统         |
| 支持的数据类型 | 固定数据类型                 | 字符串, 哈希, 列表, 集合, 有序集合 | BSON（二进制JSON）                 | JSON                                 |
| 查询语言       | SQL                          | 自定义命令集                       | MongoDB 查询语言                   | Query DSL (Domain Specific Language) |
| 扩展性         | 垂直扩展为主，水平扩展较难   | 易于水平扩展                       | 易于水平扩展                       | 易于水平扩展                         |
| 内存中处理     | 磁盘为基础，内存缓存         | 完全在内存中处理                   | 可选内存映射文件                   | 基于磁盘但有内存缓存机制             |
| 分片支持       | 不直接支持                   | 支持                               | 内置分片                           | 内置分片                             |
| 复制支持       | 支持主从复制                 | 支持主从复制                       | 支持副本集                         | 支持节点间复制                       |
| 事务支持       | 支持ACID事务                 | 不支持完整事务                     | 单文档操作是原子的，多文档事务支持 | 不支持事务                           |
| 性能           | 对复杂查询优化较好           | 极速读写，特别适合简单查询         | 对非结构化数据查询性能好           | 搜索和分析性能非常好                 |



## 2. MongoDB基础操作

[菜鸟教程-MongoDB](https://www.runoob.com/mongodb/mongodb-tutorial.html)

## 3. SpringData MongoDB

[Spring-Data-MongoDB中文文档](https://springdoc.cn/spring-data-mongodb/)

# Spring Data MongoDB 中文文档

version 4.1.0-SNAPSHOT,2023-09-10

- [前言](https://springdoc.cn/spring-data-mongodb/#preface)

- 参考文档

  - [9. 简介](https://springdoc.cn/spring-data-mongodb/#introduction)

  - 10. MongoDB 的支持

    - [10.1. 入门](https://springdoc.cn/spring-data-mongodb/#mongodb-getting-started)
    - [10.2. 示例仓库](https://springdoc.cn/spring-data-mongodb/#mongo.examples-repo)
    - [10.3. 使用 Spring 连接到 MongoDB](https://springdoc.cn/spring-data-mongodb/#mongodb-connectors)
    - [10.4. `MongoTemplate` 简介](https://springdoc.cn/spring-data-mongodb/#mongo-template)
    - [10.5. 保存、更新和删除文档](https://springdoc.cn/spring-data-mongodb/#mongo-template.save-update-remove)
    - [10.6. 查询文档](https://springdoc.cn/spring-data-mongodb/#mongo.query)
    - [10.7. Example 查询](https://springdoc.cn/spring-data-mongodb/#query-by-example)
    - [10.8. 统计文档](https://springdoc.cn/spring-data-mongodb/#mongo.query.count)
    - [10.9. Map-Reduce 操作](https://springdoc.cn/spring-data-mongodb/#mongo.mapreduce)
    - [10.10. 脚本操作](https://springdoc.cn/spring-data-mongodb/#mongo.server-side-scripts)
    - [10.11. Group 操作](https://springdoc.cn/spring-data-mongodb/#mongo.group)
    - [10.12. 聚合（Aggregation）框架的支持](https://springdoc.cn/spring-data-mongodb/#mongo.aggregation)
    - [10.13. 索引和 Collection 管理](https://springdoc.cn/spring-data-mongodb/#mongo-template.index-and-collections)
    - [10.14. 运行命令](https://springdoc.cn/spring-data-mongodb/#mongo-template.commands)
    - [10.15. 生命周期事件](https://springdoc.cn/spring-data-mongodb/#mongodb.mapping-usage.events)
    - 10.16. 实体（Entity）回调
      - [10.16.1. 实现实体回调](https://springdoc.cn/spring-data-mongodb/#entity-callbacks.implement)
      - [10.16.2. 注册实体回调](https://springdoc.cn/spring-data-mongodb/#entity-callbacks.register)
      - [10.16.3. Store 特定的实体回调（EntityCallback）](https://springdoc.cn/spring-data-mongodb/#mongo.entity-callbacks)
    - [10.17. 异常（Exception）转换](https://springdoc.cn/spring-data-mongodb/#mongo.exception)
    - [10.18. execute 回调](https://springdoc.cn/spring-data-mongodb/#mongo.executioncallback)
    - [10.19. 支持 GridFS](https://springdoc.cn/spring-data-mongodb/#gridfs)
    - [10.20. 具有可追踪游标的无限流](https://springdoc.cn/spring-data-mongodb/#tailable-cursors)
    - [10.21. 变更流（Change Streams）](https://springdoc.cn/spring-data-mongodb/#change-streams)
    - [10.22. 时间序列](https://springdoc.cn/spring-data-mongodb/#time-series)
    - [10.23. 可观察性（Observability）](https://springdoc.cn/spring-data-mongodb/#mongodb.observability)

  - [11. MongoDB 会话（Session）](https://springdoc.cn/spring-data-mongodb/#mongo.sessions)

  - [12. MongoDB 事务](https://springdoc.cn/spring-data-mongodb/#mongo.transactions)

  - [13. 响应式 MongoDB 支持](https://springdoc.cn/spring-data-mongodb/#mongo.reactive)

  - [14. MongoDB Repository](https://springdoc.cn/spring-data-mongodb/#mongo.repositories)

  - [15. 响应式 MongoDB Repository](https://springdoc.cn/spring-data-mongodb/#mongo.reactive.repositories)

  - [16. 审计](https://springdoc.cn/spring-data-mongodb/#auditing)

  - [17. 映射（Mapping）](https://springdoc.cn/spring-data-mongodb/#mapping-chapter)

  - [18. 分片](https://springdoc.cn/spring-data-mongodb/#sharding)

  - [19. Kotlin 的支持](https://springdoc.cn/spring-data-mongodb/#kotlin)

  - [20. 支持 JMX](https://springdoc.cn/spring-data-mongodb/#mongo.jmx)

- [附录](https://springdoc.cn/spring-data-mongodb/#appendix)

© 2008-2022 The original authors.

|      | 本站([springdoc.cn](https://springdoc.cn/))中的内容来源于 [spring.io](https://spring.io/) ，原始版权归属于 [spring.io](https://spring.io/)。由 [springdoc.cn](https://springdoc.cn/) 进行翻译，整理。可供个人学习、研究，未经许可，不得进行任何转载、商用或与之相关的行为。 商标声明：Spring 是 Pivotal Software, Inc. 在美国以及其他国家的商标。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

# 前言

Spring Data MongoDB项目将Spring的核心概念应用于开发使用MongoDB文档风格数据存储的解决方案。我们提供一个 “template” 作为存储和查询文档的高级抽象。你可能会注意到与Spring框架所提供的JDBC支持有相似之处。

本文档是 Spring Data - MongoDB 支持的参考指南。它解释了MongoDB模块的概念以及各种存储命名空间（store namespace）的语义和语法。

本节提供了一些关于Spring和文档数据库的基本介绍。本文的其余部分仅提及Spring Data MongoDB的功能，并假设用户熟悉MongoDB和Spring的概念。

## 1. 学习 Spring

Spring Data使用Spring框架的 [核心](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html) 功能，包括。

- [IoC](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#beans) 容器
- [类型转换系统](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#validation)
- [表达式语言](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#expressions)
- [JMX 集成](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/integration.html#jmx)
- [DAO异常层次结构](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#dao-exceptions)

虽然你不需要知道Spring的API，但理解它们背后的概念是很重要的。至少，你应该熟悉控制反转（IoC）背后的理念，而且你应该熟悉你选择使用的任何IoC容器。

可以直接使用MongoDB支持的核心功能，而不需要调用Spring容器的IoC服务。这很像 `JdbcTemplate`，它可以 "独立" 使用，不需要Spring容器的任何其他服务。为了利用Spring Data MongoDB的所有功能，如 repository 支持，你需要配置库的某些部分来使用Spring。

要了解更多关于Spring的信息，你可以参考详细解释Spring框架的综合文档。有很多关于这个主题的文章、博客条目和书籍。更多信息请参见 Spring framework [主页](https://spring.io/docs)。

## 2. 学习NoSQL和文档数据库

NoSQL存储已经在存储世界中掀起了风暴。这是一个庞大的领域，有大量的解决方案、术语和模式（更糟糕的是，甚至这个术语本身也有多种 [含义](https://www.google.com/search?q=nosoql+acronym)）。虽然有些原则是通用的，但你必须在某种程度上熟悉MongoDB。熟悉的最好方法是阅读文档和跟随例子。通常不需要花5-10分钟就能看完，特别是如果你只来自于 RDMBS （关系数据库管理系统）的背景，这些练习可以让你大开眼界。

学习MongoDB的起点是 [www.mongodb.org](https://www.mongodb.org/)。这里有一个其他有用资源的列表。

- 该 [手册](https://docs.mongodb.org/manual/) 介绍了MongoDB，并包含指向入门指南、参考文档和教程的链接。
- 访问 [MongoDB大学](https://learn.mongodb.com/)，获取免费培训材料和在线课程。
- MongoDB [Java 语言中心](https://docs.mongodb.org/ecosystem/drivers/java/).
- 你可以购买的几本 [书](https://www.mongodb.org/books)。
- Karl Seguin的在线书籍： [The Little MongoDB Book](https://openmymind.net/mongodb.pdf)。

## 3. 要求

Spring Data MongoDB 4.x二进制文件需要JDK 17级及以上，以及 [Spring Framework](https://spring.io/docs) 6.0.4 及以上。

在文档存储方面，你至少需要3.6版的 [MongoDB](https://www.mongodb.org/)，尽管我们建议使用较新的版本。

### 3.1. 兼容性表

下面的兼容性表总结了Spring Data版本与MongoDB驱动/数据库版本的关系。数据库版本显示的是通过Spring Data测试套件的最高支持服务器版本。你可以使用较新的服务器版本，除非你的应用程序使用的功能受到 [MongoDB 服务器变化](https://springdoc.cn/spring-data-mongodb/#compatibility.changes) 的影响。关于驱动程序和服务器版本的兼容性，还请参见 [官方的MongoDB驱动程序兼容性表](https://www.mongodb.com/docs/drivers/java/sync/current/compatibility/)。

| Spring Data Release Train | Spring Data MongoDB | Driver Version                   | Server Version |
| :------------------------ | :------------------ | :------------------------------- | :------------- |
| 2022.0                    | `4.0.x`             | `4.7.x`                          | `6.0.x`        |
| 2021.2                    | `3.4.x`             | `4.6.x`                          | `5.0.x`        |
| 2021.1                    | `3.3.x`             | `4.4.x`                          | `5.0.x`        |
| 2021.0                    | `3.2.x`             | `4.1.x`                          | `4.4.x`        |
| 2020.0                    | `3.1.x`             | `4.1.x`                          | `4.4.x`        |
| Neumann                   | `3.0.x`             | `4.0.x`                          | `4.4.x`        |
| Moore                     | `2.2.x`             | `3.11.x/Reactive Streams 1.12.x` | `4.2.x`        |
| Lovelace                  | `2.1.x`             | `3.8.x/Reactive Streams 1.9.x`   | `4.0.x`        |

#### 3.1.1. MongoDB 4.4 的相关变化

- 当没有 `$text` 条件存在时，字段列表不得包含文本搜索得分属性。另见 [`$text` 操作符](https://docs.mongodb.com/manual/reference/operator/query/text/)
- 在运行 map reduce 时，排序不能是一个空文档。

#### 3.1.2. MongoDB 4.2 的相关变化

- 移除 `geoNear` 命令。另见 ["移除地理坐标"（Removal of `geoNear`）](https://docs.mongodb.com/manual/release-notes/4.2-compatibility/#remove-support-for-the-geonear-command)。
- 移除 `eval` 命令。参见 ["移除 `eval`"](https://docs.mongodb.com/manual/release-notes/4.2-compatibility/#remove-support-for-the-eval-command)。

## 4. 其他帮助资源

学习一个新的框架并不总是简单明了的。在本节中，我们试图提供一个我们认为是简单易行的指南，让大家从Spring Data MongoDB模块开始学习。然而，如果你遇到问题或需要建议，请随时使用以下链接。

- Spring Boot 中文社区

  欢迎在 [Spring Boot中文社区](https://springboot.io/) 发帖分享或参与讨论。

- 社区论坛

  [Stack Overflow](https://stackoverflow.com/questions/tagged/spring-data) 上的 Spring Data 是所有Spring Data（不仅仅是Document）用户分享信息和相互帮助的一个标签。请注意，只有在发帖时才需要注册。

Spring Data和Spring背后的公司 [Pivotal Software, Inc.](https://pivotal.io/) 提供专业的、来自源头的支持，并保证响应时间。

## 5. 关注

有关Spring Data Mongo源代码库、夜间构建和快照工件的信息，请参阅Spring Data Mongo [主页](https://spring.io/projects/spring-data-mongodb/)。你可以通过 [Stack Overflow](https://stackoverflow.com/questions/tagged/spring-data) 上的社区与开发人员互动，帮助Spring Data最好地满足Spring社区的需求。要关注开发人员的活动，请在Spring Data Mongo [主页](https://spring.io/projects/spring-data-mongodb/) 上查找邮件列表信息。如果你遇到了一个错误或想提出改进建议，请在Spring Data [issue tracker](https://github.com/spring-projects/spring-data-mongodb/issues) 上创建一个 ticket。要想了解Spring生态中的最新新闻和公告，请订阅Spring社区 [门户](https://spring.io/)。你也可以在Twitter（ [SpringData](https://twitter.com/SpringData)）上关注Spring [博客](https://spring.io/blog) 或项目团队。

## 6. 升级

### 6.1. Spring Data 升级

关于如何从Spring Data的早期版本升级的说明在项目 [wiki](https://github.com/spring-projects/spring-data-commons/wiki)上提供。按照 [发布说明部分](https://github.com/spring-projects/spring-data-commons/wiki#release-notes) 的链接，找到你要升级的版本。

升级说明总是在发行说明中的第一项。如果你落后一个以上的版本，请确保你也查看你跳过的版本的发行说明。

### 6.2. 升级 MongoDB 驱动

Spring Data MongoDB 4.x 需要 MongoDB Java 驱动 4.8.x。 要了解更多关于驱动版本的信息，请访问 [MongoDB文档](https://www.mongodb.com/docs/drivers/java/sync/current/upgrade/)。

## 7. 依赖

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

### 7.1. 使用Spring Boot的依赖管理

Spring Boot会为你选择一个最新版本的Spring Data模块。如果你仍然想升级到较新的版本，将 `spring-data-releasetrain.version` 属性设置为你想使用的[train version 和 iteration](https://springdoc.cn/spring-data-mongodb/#dependencies.train-version)。

### 7.2. Spring Framework

当前版本的Spring Data模块需要Spring框架 6.0.4 或更高的版本。这些模块也可能在该次要版本的较早的错误修复版本中工作。但是，强烈建议使用那一代中的最新版本。

## 8. 与 Spring Data Repository 一起工作

Spring Data Repository 抽象的目标是大大减少为各种持久性store实现数据访问层所需的模板代码量。

|      | *Spring Data Repository 文档和你的模块*本章解释了Spring Data Repository 的核心概念和接口。本章的信息是从Spring Data Commons模块中提取的。它使用了Jakarta Persistence API（JPA）模块的配置和代码样本。 如果你想使用XML配置，你应该将XML命名空间声明和要扩展的类型调整为你使用的特定模块的等价物。“[命名空间参考](https://springdoc.cn/spring-data-mongodb/#repositories.namespace-reference)” 涵盖了XML配置，所有支持 Repository API的Spring Data模块都支持这种配置。 “[Repository query 关键字](https://springdoc.cn/spring-data-mongodb/#repository-query-keywords)” 涵盖了 Repository 抽象所支持的一般的查询方法关键字。关于你的模块的具体功能的详细信息，请参阅本文档中关于该模块的章节。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 8.1. 核心概念

Spring Data repository 抽象的中心接口是 `Repository`。它把要管理的 domain 类以及 domain 类的ID类型作为泛型参数。这个接口主要是作为一个标记接口，用来捕捉工作中的类型，并帮助你发现扩展这个接口的接口。 [`CrudRepository`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html) 和 [`ListCrudRepository`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/ListCrudRepository.html) 接口为被管理的实体类提供复杂的CRUD功能。

Example 3. `CrudRepository` 接口

```java
public interface CrudRepository<T, ID> extends Repository<T, ID> {

  <S extends T> S save(S entity);      

  Optional<T> findById(ID primaryKey); 

  Iterable<T> findAll();               

  long count();                        

  void delete(T entity);               

  boolean existsById(ID primaryKey);   

  // … more functionality omitted.
}
```

|      | 保存给定的实体。         |
| ---- | ------------------------ |
|      | 根据ID返回实体。         |
|      | 返回所有实体。           |
|      | 返回实体数量。           |
|      | 删除给定的实体。         |
|      | 根据ID判断实体是否存在。 |

`ListCrudRepository` 提供了同等的方法，但它们返回 `List`，而 `CrudRepository` 的方法返回 `Iterable`。

|      | 我们还提供了持久化技术的特定抽象，如 `JpaRepository` 或 `MongoRepository`。这些接口扩展了 `CrudRepository`，除了像 `CrudRepository` 这样相当通用的持久化技术的接口之外，还暴露了底层持久化技术的能力。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

除了 `CrudRepository` 之外，还有一个 [`PagingAndSortingRepository`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/PagingAndSortingRepository.html) 的抽象，它增加了额外的分页，排序方法。

Example 4. `PagingAndSortingRepository` 接口

```java
public interface PagingAndSortingRepository<T, ID>  {

  Iterable<T> findAll(Sort sort);

  Page<T> findAll(Pageable pageable);
}
```

例如，访问第2页的 `User` ，每页20条数据，你可以这样：

```java
PagingAndSortingRepository<User, Long> repository = // … get access to a bean
Page<User> users = repository.findAll(PageRequest.of(1, 20));
```

除了 query 方法外，count 和 delete 查询的查询派生也是可用的。下面的列表显示了派生的 count 查询的接口定义。

Example 5. Derived Count Query

```java
interface UserRepository extends CrudRepository<User, Long> {

  long countByLastname(String lastname);
}
```

下面的列表显示了一个派生的 delete 查询的接口定义。

Example 6. Derived Delete Query

```java
interface UserRepository extends CrudRepository<User, Long> {

  long deleteByLastname(String lastname);

  List<User> removeByLastname(String lastname);
}
```

### 8.2. Query 方法

标准的CRUD Repository 通常有对底层数据store的查询。使用Spring Data，声明这些查询成为一个四步过程。

1. 声明一个扩展 `Repository` 或其子接口之一的接口，并将其泛型指定为它应该处理的domain类和ID类型，如以下例子所示。

   ```java
   interface PersonRepository extends Repository<Person, Long> { … }
   ```

2. 在接口中声明query方法。

   ```java
   interface PersonRepository extends Repository<Person, Long> {
     List<Person> findByLastname(String lastname);
   }
   ```

3. 设置Spring为这些接口创建代理实例，可以用[JavaConfig](https://springdoc.cn/spring-data-mongodb/#repositories.create-instances.java-config)或[XML 配置](https://springdoc.cn/spring-data-mongodb/#repositories.create-instances)。

   Java

   XML

   ```java
   @EnableMongoRepositories
   class Config { … }
   ```

   本例中使用的是JPA namespace。如果你对任何其他store使用 Repository 抽象，你需要把它改为你的store模块的适当 namespace 声明。换句话说，你应该把 `jpa` 换成，例如 `mongodb`。

   请注意，JavaConfig 并没有明确地配置 `package`，因为被注解的类的 `package` 是默认使用的。要自定义要扫描的包，请使用数据store特定库的 `@EnableMongoRepositories` 注解的 `basePackage…` 属性之一。

   ```java
   class SomeClient {
   
     private final PersonRepository repository;
   
     SomeClient(PersonRepository repository) {
       this.repository = repository;
     }
   
     void doSomething() {
       List<Person> persons = repository.findByLastname("Matthews");
     }
   }
   ```

下面的章节将详细解释每一个步骤。

- [定义 Repository 接口](https://springdoc.cn/spring-data-mongodb/#repositories.definition)
- [定义 Query 方法](https://springdoc.cn/spring-data-mongodb/#repositories.query-methods.details)
- [创建 Repository 实例](https://springdoc.cn/spring-data-mongodb/#repositories.create-instances)
- [自定义 Spring Data Repository 的实现](https://springdoc.cn/spring-data-mongodb/#repositories.custom-implementations)

### 8.3. 定义 Repository 接口

要定义一个 repository 接口，你首先需要定义一个domain类专用的 repository 接口。该接口必须继承 `Repository`，并将其泛型设置为domain类和ID类。如果你想为该domain类公开CRUD方法，你可以继承 `CrudRepository`，或其变体，而不是 `Repository`。

#### 8.3.1. 稍微修改 Repository 的定义

有几种变体可以让你开始使用你的 repository 接口。

典型的方法是继承 `CrudRepository`，它为你提供了 CRUD 功能的方法。CRUD是指创建、读取、更新、删除。在3.0版本中，我们还引入了 `ListCrudRepository`，它与 `CrudRepository` 非常相似，但对于那些返回多个实体的方法，它返回一个 `List` 而不是一个 `Iterable`，你可能会发现它更容易使用。

如果你使用的是响应式store，你可以选择 `ReactiveCrudRepository`，或者 `RxJava3CrudRepository`，这取决于你使用的是哪种响应式框架。

如果你使用的是Kotlin，你可以选择 `CoroutineCrudRepository`，它利用了Kotlin的 `coroutine`（协程）。

额外的你可以扩展 `PagingAndSortingRepository`、`ReactiveSortingRepository`、`RxJava3SortingRepository` 或 `CoroutineSortingRepository`，如果你需要允许指定一个 `Sort` 抽象的方法，或者在第一种情况下是 `Pageable` 抽象。请注意，各种排序 repository 不再像Spring Data 3.0之前的版本那样扩展各自的CRUD库。因此，如果你想获得这两个接口的功能，你需要扩展这两个接口。

如果你不想扩展Spring Data接口，你也可以用 `@RepositoryDefinition` 来注解你的 repository 接口。扩展CRUD repository 接口之一会暴露出一套完整的方法来操作你的实体。如果你想对暴露的方法有所选择，可以从CRUD repository 复制你想暴露的方法到你的 domain repository。这样做时，你可以改变方法的返回类型。如果可能的话，Spring Data会尊重返回类型。例如，对于返回多个实体的方法，你可以选择 `Iterable<T>`、`List<T>`、`Collection<T>` 或 VAVR list

如果你的应用程序中的许多 repository 应该有相同的方法集，你可以定义你自己的基础接口来继承。这样的接口必须用 `@NoRepositoryBean` 来注释。这可以防止Spring Data试图直接创建它的实例而导致异常，因为它仍然包含一个泛型变量，Spring data 无法确定该 repository 的实体。

下面的例子展示了如何有选择地公开CRUD方法（本例中为 `findById` 和 `save`）。

Example 7. Selectively exposing CRUD methods

```java
@NoRepositoryBean
interface MyBaseRepository<T, ID> extends Repository<T, ID> {

  Optional<T> findById(ID id);

  <S extends T> S save(S entity);
}

interface UserRepository extends MyBaseRepository<User, Long> {
  User findByEmailAddress(EmailAddress emailAddress);
}
```

在前面的例子中，你为所有的 domain repository 定义了一个通用的基础接口，并暴露了 `findById(…)` 以及 `save(…)`。这些方法被路由到Spring Data提供的你所选择的store的基础 repository 实现（例如，如果你使用JPA，实现是 `SimpleJpaRepository`），因为它们与 `CrudRepository` 中的方法签名一致。 所以 `UserRepository` 现在可以保存用户，通过ID查找单个用户，通过电子邮件地址查找 `User`。

|      | 中间的 repository 接口被注解为 `@NoRepositoryBean`。确保你在所有Spring Data不应该在运行时创建实例的 repository 接口上添加该注解。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 8.3.2. 在多个Spring数据模块中使用 Repository

在你的应用程序中使用一个独特的Spring Data模块使事情变得简单，因为定义范围内的所有 repository 接口都绑定到Spring Data模块。有时，应用程序需要使用一个以上的Spring Data模块。在这种情况下，repository 定义必须区分持久化技术。当它检测到类路径上有多个 repository 工厂时，Spring Data会进入严格的 repository 配置模式。严格的配置使用 repository 或domain类的细节来决定 repository 定义的Spring Data模块绑定。

1. 如果 repository 定义 [继承了特定模块的 repository](https://springdoc.cn/spring-data-mongodb/#repositories.multiple-modules.types)，那么它就是特定Spring Data模块的有效候选 repository。
2. 如果 domain 类被注解了 [模块特定的类型注解](https://springdoc.cn/spring-data-mongodb/#repositories.multiple-modules.annotations)，它就是特定Spring Data模块的有效候选者。Spring Data模块接受第三方注解（如JPA的 `@Entity`）或提供自己的注解（如Spring Data MongoDB和Spring Data Elasticsearch的 `@Document`）。

下面的例子显示了一个使用特定模块接口的 repository（本例中为JPA）。

Example 8. 使用模块特定接口的 Repository 定义

```java
interface MyRepository extends JpaRepository<User, Long> { }

@NoRepositoryBean
interface MyBaseRepository<T, ID> extends JpaRepository<T, ID> { … }

interface UserRepository extends MyBaseRepository<User, Long> { … }
```

`MyRepository` 和 `UserRepository` 在其类型层次上扩展了 `JpaRepository`。它们是Spring Data JPA 模块的有效候选者。

下面的例子显示了一个使用通用（泛型）接口的 repository。

Example 9. 使用泛型接口的 repository 定义

```java
interface AmbiguousRepository extends Repository<User, Long> { … }

@NoRepositoryBean
interface MyBaseRepository<T, ID> extends CrudRepository<T, ID> { … }

interface AmbiguousUserRepository extends MyBaseRepository<User, Long> { … }
```

`AmbiguousRepository` 和 `AmbiguousUserRepository` 在其类型层次结构中只继承了 `Repository` 和 `CrudRepository` 。虽然在使用唯一的Spring Data模块时这很好，但多个模块无法区分这些 repository 应该被绑定到哪个特定的Spring Data。

下面的例子显示了一个使用带注解的domain类的repository。

Example 10. 使用带注解的 domain 类的Repository 定义

```java
interface PersonRepository extends Repository<Person, Long> { … }

@Entity
class Person { … }

interface UserRepository extends Repository<User, Long> { … }

@Document
class User { … }
```

`PersonRepository` 引用了 `Person`，它被 JPA 的 `@Entity` 注解所注解，所以这个 repository 显然属于Spring Data JPA。`UserRepository` 引用了 `User`，它被Spring Data MongoDB 的 `@Document` 注解所注解。

下面的坏例子显示了一个使用混合注解的 domain 类的 Repository。

Example 11. 使用具有混合注解的 domain 类的 repository 定义

```java
interface JpaPersonRepository extends Repository<Person, Long> { … }

interface MongoDBPersonRepository extends Repository<Person, Long> { … }

@Entity
@Document
class Person { … }
```

这个例子展示了一个同时使用JPA和Spring Data MongoDB注解的 domain 类。它定义了两个repository：`JpaPersonRepository` 和 `MongoDBPersonRepository`。一个用于JPA，另一个用于MongoDB的使用。Spring Data不再能够区分这些repository，这导致了未定义的行为。

[Repository 类型细节](https://springdoc.cn/spring-data-mongodb/#repositories.multiple-modules.types)和[区分domain类注解](https://springdoc.cn/spring-data-mongodb/#repositories.multiple-modules.annotations)用于严格的repository库配置，以确定特定Spring Data模块的repository候选者。在同一domain类型上使用多个持久化技术的特定注解是可能的，并且能够在多个持久化技术中重复使用domain类型。然而，Spring Data就不能再确定一个唯一的模块来绑定repository了。

区分 repository 的最后一个方法是通过对 repository base package的扫描。base package 定义了扫描 repository 接口定义的起点，这意味着将 repository 的定义放在适当的包中。默认情况下，注解驱动的配置使用配置类所在的base package。基于[XML的配置中的base package](https://springdoc.cn/spring-data-mongodb/#repositories.create-instances.xml)，需要手动强制配置。

下面的例子显示了注解驱动的 base package 的配置。

Example 12. 注解驱动的 base package 的配置

```java
@EnableJpaRepositories(basePackages = "com.acme.repositories.jpa")
@EnableMongoRepositories(basePackages = "com.acme.repositories.mongo")
class Configuration { … }
```

### 8.4. 定义 Query 方法

repository 代理有两种方法可以从方法名中推导出 repository 特定的查询。

- 通过直接从方法名派生出查询。
- 通过使用手动定义的查询。

可用的选项取决于实际的store。然而，必须有一个策略来决定创建什么样的实际查询。下一节将介绍可用的选项。

#### 8.4.1. Query 的查询策略

下列策略可用于 repository 基础设施解析查询。 对于 XML 配置，你可以通过 `query-lookup-strategy` 属性在命名空间配置策略。 对于 Java 配置，你可以使用 `EnableMongoRepositories` 注解的 `queryLookupStrategy` 属性。有些策略可能不支持特定的datastore。

- `CREATE` 试图从查询方法名称中构建一个特定的存储查询。一般的做法是从方法名中删除一组已知的前缀，然后解析方法的其余部分。你可以在 “[Query 创建](https://springdoc.cn/spring-data-mongodb/#repositories.query-methods.query-creation)” 中阅读更多关于查询构建的信息。 `USE_DECLARED_QUERY` 试图找到一个已声明的查询，如果找不到就会抛出一个异常。查询可以由某处的注解来定义，也可以通过其他方式来声明。请参阅特定store的文档以找到该store的可用选项。如果 repository 基础设施在启动时没有为该方法找到一个已声明的查询，则会失败。
- `CREATE_IF_NOT_FOUND` (默认) 结合了 `CREATE` 和 `USE_DECLARED_QUERY`。它首先查找一个已声明的查询， 如果没有找到已声明的查询， 它将创建一个基于方法名的自定义查询。这是默认的查询策略，因此，如果你没有明确地配置任何东西，就会使用这种策略。它允许通过方法名快速定义查询，但也可以根据需要通过引入已声明的查询对这些查询进行自定义调整。

#### 8.4.2. Query 创建

内置在Spring Data repository 基础架构中的查询 builder 机制对于在资源库的实体上建立约束性查询非常有用。

下面的例子展示了如何创建一些查询。

Example 13. Query creation from method names

```java
interface PersonRepository extends Repository<Person, Long> {

  List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastname);

  // Enables the distinct flag for the query
  List<Person> findDistinctPeopleByLastnameOrFirstname(String lastname, String firstname);
  List<Person> findPeopleDistinctByLastnameOrFirstname(String lastname, String firstname);

  // Enabling ignoring case for an individual property
  List<Person> findByLastnameIgnoreCase(String lastname);
  // Enabling ignoring case for all suitable properties
  List<Person> findByLastnameAndFirstnameAllIgnoreCase(String lastname, String firstname);

  // Enabling static ORDER BY for a query
  List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
  List<Person> findByLastnameOrderByFirstnameDesc(String lastname);
}
```

解析查询方法名称分为主语和谓语。第一部分（`find…By`, `exists…By`）定义了查询的主语，第二部分形成谓语。引入句（主语）可以包含进一步的表达。在 `find`（或其他引入关键词）和 `By` 之间的任何文本都被认为是描述性的，除非使用一个限制结果的关键词，如 `Distinct` 在要创建的查询上设置一个不同的标志，或 [`Top` / `First` 来限制查询结果](https://springdoc.cn/spring-data-mongodb/#repositories.limit-query-result)。

附录中包含了 [查询方法主语关键词](https://springdoc.cn/spring-data-mongodb/#appendix.query.method.subject) 和 [查询方法谓语关键词的完整列表](https://springdoc.cn/spring-data-mongodb/#appendix.query.method.predicate)，包括排序和字母修饰语。然而，第一个 `By` 作为分界符，表示实际条件谓词的开始。在一个非常基本的层面上，你可以在实体属性上定义条件，并用 `And` 和 `Or` 来连接它们。

解析方法的实际结果取决于你为之创建查询的持久性store。然而，有一些东西需要注意。

- 表达式通常是属性遍历与可以串联的运算符的组合。你可以用 `AND` 和 `OR` 来组合属性表达式。你还可以得到对属性表达式的运算符的支持，如 `Between`, `LessThan`, `GreaterThan`, 和 `Like`。支持的运算符可能因 datastore 的不同而不同，所以请查阅参考文档的适当部分。
- 方法解析器支持为单个属性（例如，`findByLastnameIgnoreCase(…)`）或支持忽略大小写的类型的所有属性（通常是字符串实例—例如，`findByLastnameAndFirstnameAllIgnoreCase`(…)）设置忽略大小写标志。是否支持忽略大小写可能因store而异，所以请查阅参考文档中的相关章节，了解特定store的查询方法。
- 你可以通过在引用属性的查询方法中附加一个 `OrderBy` 子句，并提供一个排序方向（`Asc` 或 `Desc`）来应用静态排序。要创建一个支持动态排序的查询方法，请参阅 “[特殊参数处理](https://springdoc.cn/spring-data-mongodb/#repositories.special-parameters)”。

#### 8.4.3. 属性表达式

属性表达式只能引用被管理实体的一个直接属性，如前面的例子所示。在查询创建时，你已经确保解析的属性是被管理的domian类的一个属性。然而，你也可以通过遍历嵌套属性来定义约束。考虑一下下面的方法签名。

```java
List<Person> findByAddressZipCode(ZipCode zipCode);
```

假设 `Person` 有一个带有 `ZipCode` 的 `Address`。在这种情况下，该方法创建 `x.address.zipCode` 属性遍历。解析算法首先将整个部分（`AddressZipCode`）解释为属性，并检查domain类中是否有该名称的属性（未加首字母）。如果算法成功，它就使用该属性。如果没有，该算法将源头的驼峰字母部分从右侧分割成一个头和一个尾，并试图找到相应的属性—在我们的例子中，是 `AddressZip` 和 `Code`。如果该算法找到了具有该头部的属性，它就取其尾部，并从那里继续向下构建树，以刚才描述的方式将尾部分割开来。如果第一次分割不匹配，该算法将分割点移到左边（`Address`, `ZipCode`）并继续。

虽然这在大多数情况下应该是有效的，但该算法有可能选择错误的属性。假设 `Person` 类也有一个 `addressZip` 属性。该算法将在第一轮分割中已经匹配，选择错误的属性，并且失败（因为 `addressZip` 的类型可能没有 `code` 属性）。

为了解决这个模糊的问题，你可以在你的方法名里面使用 `_` 来手动定义遍历点。因此，我们的方法名称将如下。

```java
List<Person> findByAddress_ZipCode(ZipCode zipCode);
```

因为我们把下划线字符当作一个保留字符，所以我们强烈建议遵循标准的Java命名惯例（也就是说，不要在属性名中使用下划线，而要使用驼峰大写）。

#### 8.4.4. 特殊参数处理

为了处理你的查询中的参数，定义方法参数，正如在前面的例子中已经看到的。除此之外，基础设施还能识别某些特定的类型，如 `Pageable` 和 `Sort`，以动态地将分页和排序应用于你的查询。下面的例子演示了这些功能。

Example 14. 在查询方法中使用 `Pageable`、`Slice` 和 `Sort`。

```java
Page<User> findByLastname(String lastname, Pageable pageable);

Slice<User> findByLastname(String lastname, Pageable pageable);

List<User> findByLastname(String lastname, Sort sort);

List<User> findByLastname(String lastname, Pageable pageable);
```

|      | API中定义的 `Sort` 和 `Pageable` 实际调用时不能为 `null`。如果你不想应用任何排序或分页，请使用 `Sort.unsorted()` 和 `Pageable.unpaged()`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

第一个方法让你把 `org.springframework.data.domain.Pageable` 实例传递给 query 方法，以动态地将分页添加到你静态定义的查询中。一个 `Page` 知道可用的元素和页面的总数。它是通过基础设施触发一个 count 查询来计算总数量。由于这可能是昂贵的（取决于使用的store），你可以返回一个 `Slice`。一个 `Slice` 只知道下一个 `Slice` 是否可用，当遍历一个较大的结果集时，这可能就足够了。

排序选项也是通过 `Pageable` 实例处理的。如果你只需要排序，在你的方法中加入 `org.springframework.data.domain.Sort` 参数。正如你所看到的，返回一个 `List` 也是可能的。在这种情况下，构建实际的 `Page` 实例所需的额外元数据并没有被创建（这反过来意味着不需要发出额外的 count 查询）。相反，它限制了查询，只查询给定范围的实体。

|      | 要想知道你 query 的总页数，你必须触发一个额外的count查询。默认情况下，这个查询是由你实际触发的查询派生出来的。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 分页和排序

你可以通过使用属性名称来定义简单的排序表达式。你可以将表达式连接起来，将多个 criteria 收集到一个表达式中。

Example 15. Defining sort expressions

```java
Sort sort = Sort.by("firstname").ascending()
  .and(Sort.by("lastname").descending());
```

对于定义排序表达式的更加类型安全的方式，从定义排序表达式的类型开始，使用方法引用来定义排序的属性。

Example 16. 通过使用类型安全的API来定义排序表达式

```java
TypedSort<Person> person = Sort.sort(Person.class);

Sort sort = person.by(Person::getFirstname).ascending()
  .and(person.by(Person::getLastname).descending());
```

|      | `TypedSort.by(…)` 通过（通常）使用 CGlib 来使用运行时代理，这在使用 Graal VM Native 等工具时可能会干扰原生镜像的编译。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

如果你的 store 实现支持 Querydsl，你也可以使用生成的 metamodel 类型来定义排序表达式。

Example 17. 通过使用Querydsl API定义排序表达式

```java
QSort sort = QSort.by(QPerson.firstname.asc())
  .and(QSort.by(QPerson.lastname.desc()));
```

#### 8.4.5. 限制查询结果

你可以通过使用 `first` 或 `top` 关键字来限制查询方法的结果，这两个关键字可以互换使用。你可以在 `top` 或 `first` 后面附加一个可选的数值，以指定要返回的最大结果大小。如果不加数字，就会假定结果大小为 `1`。下面的例子显示了如何限制查询的大小。

Example 18. 使用 `Top` 和 `First` 限制查询结果集

```java
User findFirstByOrderByLastnameAsc();

User findTopByOrderByAgeDesc();

Page<User> queryFirst10ByLastname(String lastname, Pageable pageable);

Slice<User> findTop3ByLastname(String lastname, Pageable pageable);

List<User> findFirst10ByLastname(String lastname, Sort sort);

List<User> findTop10ByLastname(String lastname, Pageable pageable);
```

对于支持不同查询的数据集，限制表达式也支持 `Distinct` 关键字。另外，对于将结果集限制在一个实例的查询，支持用 `Optional` 关键字将结果包入。

如果分页或 slice 应用于 limit 查询的分页（以及可用页数的计算），则会在 limit 结果中应用。

|      | 通过使用 `Sort` 参数将结果与动态排序相结合，可以让你表达对 "K" 最小元素和 "K" 最大元素的查询方法。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 8.4.6. Repository 方法返回 Collection 或 Iterable

返回多个结果的查询方法可以使用标准的Java `Iterable`、`List` 和 `Set`。除此之外，我们还支持返回Spring Data的 `Streamable`，这是 `Iterable` 的一个自定义扩展，以及 [Vavr](https://www.vavr.io/) 提供的 collection 类型。请参考附录中对所有可能的 [查询方法返回类型](https://springdoc.cn/spring-data-mongodb/#appendix.query.return.types)的解释。

##### 使用 Streamable 作为 Query 方法的返回类型

你可以用 `Streamable` 来替代 `Iterable` 或任何 collection 类型。它提供了方便的方法来访问一个非并行的 `Stream`（`Iterable` 所没有的），并且能够在元素上直接 `…filter(…)` 和 `…map(…)`，并将 `Streamable` 与其他元素连接起来。

Example 19. 使用 Streamable 来组合 query 方法的结果

```java
interface PersonRepository extends Repository<Person, Long> {
  Streamable<Person> findByFirstnameContaining(String firstname);
  Streamable<Person> findByLastnameContaining(String lastname);
}

Streamable<Person> result = repository.findByFirstnameContaining("av")
  .and(repository.findByLastnameContaining("ea"));
```

##### 返回自定义的 Streamable Wrapper 类型

为集合提供专用的 wrapper 类型是一种常用的模式，为返回多个元素的查询结果提供API。通常，这些类型的使用是通过调用返回类似集合类型的 repository 方法，并手动创建一个 wrapper 类型的实例。你可以避免这个额外的步骤，因为Spring Data允许你使用这些 wrapper 类型作为查询方法的返回类型，如果它们满足以下条件。

1. 该类型实现了 `Streamable`.
2. 该类型暴露了一个构造函数或一个名为 `of(…)` 或 `valueOf(…)` 的静态工厂方法，该方法将 `Streamable` 作为一个参数。

下面列出了一个例子。

```java
class Product {                                         
  MonetaryAmount getPrice() { … }
}

@RequiredArgsConstructor(staticName = "of")
class Products implements Streamable<Product> {         

  private final Streamable<Product> streamable;

  public MonetaryAmount getTotal() {                    
    return streamable.stream()
      .map(Priced::getPrice)
      .reduce(Money.of(0), MonetaryAmount::add);
  }


  @Override
  public Iterator<Product> iterator() {                 
    return streamable.iterator();
  }
}

interface ProductRepository implements Repository<Product, Long> {
  Products findAllByDescriptionContaining(String text); 
}
```

|      | 一个 `Product` 实体，公开API以访问 product 的price。         |
| ---- | ------------------------------------------------------------ |
|      | 一个 `Streamable<Product>` 的封装类型，可以通过使用 `Products.of(…)`（用Lombok注解创建的工厂方法）来构建。使用 `Streamable<Product>` 的标准构造函数也可以。 |
|      | wrapper 类型暴露了一个额外的API，在 `Streamable<Product>` 上计算新值。 |
|      | 实现 `Streamable` 接口并委托给实际结果。                     |
|      | wrapper 类型 `Products` 可以直接作为查询方法的返回类型。你不需要返回 `Streamable<Product>` 并在调用 repository 后手动封装它。 |

##### 支持 Vavr 集合

[Vavr](https://www.vavr.io/) 是一个拥抱Java中函数式编程概念的库。它带有一组自定义的集合类型，你可以将其作为查询方法的返回类型，如下表所示。

| Vavr collection 类型     | 使用的Vavr实现类型                 | 有效的Java原类型     |
| :----------------------- | :--------------------------------- | :------------------- |
| `io.vavr.collection.Seq` | `io.vavr.collection.List`          | `java.util.Iterable` |
| `io.vavr.collection.Set` | `io.vavr.collection.LinkedHashSet` | `java.util.Iterable` |
| `io.vavr.collection.Map` | `io.vavr.collection.LinkedHashMap` | `java.util.Map`      |

你可以使用第一列中的类型（或其子类型）作为查询方法的返回类型，并根据实际查询结果的Java类型（第三列），获得第二列中的类型作为实现类型使用。或者，你可以声明 `Traversable`（相当于Vavr `Iterable`），然后我们从实际返回值中派生出实现类。也就是说，`java.util.List` 会变成 Vavr `List` 或 `Seq`，`java.util.Set` 会变成 Vavr `LinkedHashSet` `Set`，以此类推。

#### 8.4.7. Repository 方法的 Null 处理

从Spring Data 2.0开始，返回单个聚合实例的 repository CRUD方法使用Java 8的 `Optional` 来表示可能没有的值。除此之外，Spring Data还支持在查询方法上返回以下 wrapper 类型。

- `com.google.common.base.Optional`
- `scala.Option`
- `io.vavr.control.Option`

另外，查询方法可以选择完全不使用 wrapper 类型。没有查询结果的话会通过返回 `null` 来表示。Repository 方法返回集合、集合替代物、wrapper和流时，保证不会返回 `null`，而是返回相应的空（Empty）表示。详见 “[Repository 查询返回类型](https://springdoc.cn/spring-data-mongodb/#repository-query-return-types)”。

##### null约束注解

你可以通过使用 [Spring Framework的nullability注解](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#null-safety) 来表达 repository 方法的 nullability 约束。它们提供了一种友好的方法，并在运行时选择加入 `null` 值检查，如下所示。

- [`@NonNullApi`](https://docs.spring.io/spring/docs/6.0.4/javadoc-api/org/springframework/lang/NonNullApi.html): 在 package 的层面上用于声明参数和返回值的默认行为，分别是既不接受也不产生 `null` 值。
- [`@NonNull`](https://docs.spring.io/spring/docs/6.0.4/javadoc-api/org/springframework/lang/NonNull.html): 用于不得为 `null` 的参数或返回值（不需要在参数和返回值中使用 `@NonNullApi`）。
- [`@Nullable`](https://docs.spring.io/spring/docs/6.0.4/javadoc-api/org/springframework/lang/Nullable.html): 用于可以是 `null` 的参数或返回值。

Spring注解是用 [JSR 305](https://jcp.org/en/jsr/detail?id=305) 注解（一个休眠状态但广泛使用的JSR）进行元注解的。JSR 305元注解让工具供应商（如 [IDEA](https://www.jetbrains.com/help/idea/nullable-and-notnull-annotations.html)、 [Eclipse](https://help.eclipse.org/oxygen/index.jsp?topic=/org.eclipse.jdt.doc.user/tasks/task-using_external_null_annotations.htm) 和 [Kotlin](https://kotlinlang.org/docs/reference/java-interop.html#null-safety-and-platform-types)）以通用的方式提供 null-safety 支持，而不需要对Spring注解进行硬编码支持。为了在运行时检查查询方法的无效性约束，你需要通过在 `package-info.java` 中使用 Spring 的 `@NonNullApi`，在包级别上激活null约束，如下例所示。

Example 20. Declaring Non-nullability in `package-info.java`

```java
@org.springframework.lang.NonNullApi
package com.acme;
```

一旦定义了非null约束，repository 的查询方法调用就会在运行时被验证是否有nul约束。如果查询结果违反了定义的约束条件，就会抛出一个异常。这种情况发生在方法会返回 `null`，但被声明为non-nullable（在 repository 所在的包上定义注解的默认值）。如果你想再次选择加入允许结果为null，可以有选择地在个别方法上使用 `@Nullable`。使用本节开头提到的结果wrapper类型继续按预期工作：一个空的结果被翻译成代表不存在的值。

下面的例子显示了刚才描述的一些技术。

Example 21. Using different nullability constraints

```java
package com.acme;                                                       

interface UserRepository extends Repository<User, Long> {

  User getByEmailAddress(EmailAddress emailAddress);                    

  @Nullable
  User findByEmailAddress(@Nullable EmailAddress emailAdress);          

  Optional<User> findOptionalByEmailAddress(EmailAddress emailAddress); 
}
```

|      | repository 在一个包（或子包）中，我们已经为其定义了非空的行为。 |
| ---- | ------------------------------------------------------------ |
|      | 当查询没有产生结果时，抛出一个 `EmptyResultDataAccessException`。当交给该方法的 `emailAddress` 为 `null` 时，抛出一个 `IllegalArgumentException`。 |
|      | 当查询没有产生结果时返回 `null`。也接受 `null` 作为 `emailAddress` 的值。 |
|      | 当查询没有产生结果时，返回 `Optional.empty()`。当交给该方法的 `emailAddress` 为 `null` 时，抛出一个 `IllegalArgumentException`。 |

##### 基于Kotlin的 Repository 中的 Nullability

Kotlin在语言中加入了 [对无效性约束的定义](https://kotlinlang.org/docs/reference/null-safety.html)。Kotlin代码编译为字节码，它不通过方法签名来表达无效性约束，而是通过编译后的元数据。请确保在你的项目中包含 `kotlin-reflect` JAR，以实现对Kotlin的nullability约束的内省。Spring Data Repository 使用语言机制来定义这些约束，以应用相同的运行时检查，如下所示。

Example 22. Using nullability constraints on Kotlin repositories

```kotlin
interface UserRepository : Repository<User, String> {

  fun findByUsername(username: String): User     

  fun findByFirstname(firstname: String?): User? 
}
```

|      | 该方法将参数和结果都定义为不可为空（Kotlin默认）。Kotlin编译器会拒绝那些向方法传递 `null` 的方法调用。如果查询产生了一个空的结果，就会抛出一个 `EmptyResultDataAccessException`。 |
| ---- | ------------------------------------------------------------ |
|      | 这个方法接受 `null` 作为 `firstname` 参数，如果查询没有产生结果，则返回 `null`。 |

#### 8.4.8. 流式（Stream）查询结果

你可以通过使用Java 8 `Stream<T>` 作为返回类型来增量地处理查询方法的结果。如下面的例子所示，不把查询结果包裹在 `Stream` 中，而是使用 data store 的特定方法来执行流式处理。

Example 23. Stream the result of a query with Java 8 `Stream<T>`

```java
@Query("select u from User u")
Stream<User> findAllByCustomQueryAndStream();

Stream<User> readAllByFirstnameNotNull();

@Query("select u from User u")
Stream<User> streamAllPaged(Pageable pageable);
```

|      | 一个 `Stream` 可能包裹了底层 data store 的特定资源，因此，在使用后必须关闭。你可以通过使用 `close()` 方法来手动关闭 `Stream`，或者使用Java 7 `try-with-resources` 块来关闭，如下面的例子中所示。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

Example 24. Working with a `Stream<T>` result in a `try-with-resources` block

```java
try (Stream<User> stream = repository.findAllByCustomQueryAndStream()) {
  stream.forEach(…);
}
```

|      | 目前并非所有的Spring Data模块都支持 `Stream<T>` 作为返回类型。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 8.4.9. 异步（Asynchronous）查询结果

你可以通过使用 [Spring的异步方法运行能力](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/integration.html#scheduling) 来异步运行 repository 查询。这意味着该方法在调用后立即返回，而实际的查询发生在一个已经提交给Spring `TaskExecutor` 的任务中。异步查询与响应式查询不同，不应混合使用。关于响应式支持的更多细节，请参见store的特定文档。下面的例子显示了一些异步查询的案例。

```java
@Async
Future<User> findByFirstname(String firstname);               

@Async
CompletableFuture<User> findOneByFirstname(String firstname); 
```

|      | 使用 `java.util.concurrent.Future` 作为返回类型。            |
| ---- | ------------------------------------------------------------ |
|      | 使用 Java 8 `java.util.concurrent.CompletableFuture` 作为返回类型。 |

### 8.5. 创建 Repository 实例

本节介绍了如何为定义的 repository 接口创建实例和Bean定义。

#### 8.5.1. Java 配置

在Java配置类上使用store特有的 `@EnableMongoRepositories` 注解来定义 repository 激活的配置。关于基于Java的Spring容器配置的介绍，请参见 [Spring参考文档中的JavaConfig](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#beans-java)。

启用 Spring Data Repository 的示例配置类似于以下内容。

Example 25. 基于注解的 repository 配置示例

```java
@Configuration
@EnableJpaRepositories("com.acme.repositories")
class ApplicationConfiguration {

  @Bean
  EntityManagerFactory entityManagerFactory() {
    // …
  }
}
```

|      | 前面的例子使用了JPA特定的注解，你可以根据你实际使用的store模块来改变它。这同样适用于 `EntityManagerFactory` Bean的定义。请看涉及store特定配置的章节。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 8.5.2. XML 配置

每个Spring Data模块都包括一个 `repositories` 元素，让你定义一个Spring为你扫描的 base package，如下例所示。

Example 26. 通过XML启用Spring Data Repository

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns:beans="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns="http://www.springframework.org/schema/data/jpa"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/data/jpa
    https://www.springframework.org/schema/data/jpa/spring-jpa.xsd">

  <jpa:repositories base-package="com.acme.repositories" />

</beans:beans>
```

在前面的例子中，Spring被指示扫描 `com.acme.repositories` 及其所有子包，以寻找扩展 `Repository` 或其子接口之一的接口。对于找到的每个接口，基础设施都会注册持久化技术专用的 `FactoryBean`，以创建适当的代理，处理查询方法的调用。每个Bean都被注册在一个从接口名称衍生出来的Bean名称下，所以 `UserRepository` 的接口将被注册在 `userRepository` 下。嵌套的存储库接口的Bean名是以其包裹的类型名称为前缀。base package 属性允许通配符，这样你就可以定义一个扫描包的模式。

#### 8.5.3. 使用 Filter

默认情况下，基础架构会抓取每个扩展了位于配置的 base package 下的持久化技术特定的 `Repository` 子接口的接口，并为其创建一个Bean实例。然而，你可能想要更精细地控制哪些接口为其创建Bean实例。要做到这一点，可以在 Repository 声明中使用 filter 元素。其语义与Spring的组件过滤器中的元素完全等同。详情请见 [Spring参考文档](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#beans-scanning-filters) 中的这些元素。

例如，为了排除某些接口作为 Repository Bean 的实例化，你可以使用以下配置。

Example 27. 使用 Filter

Java

XML

```java
@Configuration
@EnableMongoRepositories(basePackages = "com.acme.repositories",
    includeFilters = { @Filter(type = FilterType.REGEX, pattern = ".*SomeRepository") },
    excludeFilters = { @Filter(type = FilterType.REGEX, pattern = ".*SomeOtherRepository") })
class ApplicationConfiguration {

  @Bean
  EntityManagerFactory entityManagerFactory() {
    // …
  }
}
```

前面的例子排除了所有以 `SomeRepository` 结尾的接口被实例化，包括以 `SomeOtherRepository` 结尾的接口。

#### 8.5.4. 单独使用

你也可以在Spring容器之外使用资源库基础设施—例如，在CDI环境中。你仍然需要在你的classpath中使用一些Spring库，但是，一般来说，你也可以通过编程来设置 Repository。Repository 支持的Spring Data模块都有一个特定于持久化技术的 `RepositoryFactory`，你可以使用，如下所示。

Example 28. repository factory 的独立使用

```java
RepositoryFactorySupport factory = … // Instantiate factory here
UserRepository repository = factory.getRepository(UserRepository.class);
```

### 8.6. 自定义 Spring Data Repository 的实现

Spring Data提供了各种选项来创建查询方法，只需少量编码。但当这些选项不符合你的需求时，你也可以为 repository 方法提供你自己的自定义实现。本节介绍了如何做到这一点。

#### 8.6.1. 自定义个别 Repository

要用自定义的功能来丰富 repository，你必须首先定义一个片段（fragment）接口和自定义功能的实现，如下所示。

Example 29. 定制 repository 功能的接口

```java
interface CustomizedUserRepository {
  void someCustomMethod(User user);
}
```

Example 30. 实现自定义 repository 的功能

```java
class CustomizedUserRepositoryImpl implements CustomizedUserRepository {

  public void someCustomMethod(User user) {
    // Your custom implementation
  }
}
```

|      | 与片段接口对应的类名中最重要的部分是 `Impl` 后缀。 |
| ---- | -------------------------------------------------- |
|      |                                                    |

实现本身并不依赖于Spring Data，它可以是一个普通的Spring Bean。因此，你可以使用标准的依赖注入行为来注入对其他Bean（如 `JdbcTemplate`）的引用，参与到各个切面，等等。

然后你可以让你的 repository 接口继承片段接口，如下所示。

Example 31. 改变你的 repository 接口

```java
interface UserRepository extends CrudRepository<User, Long>, CustomizedUserRepository {

  // Declare query methods here
}
```

用你的存储库接口继承片段接口，结合了CRUD和自定义功能，并使其对客户端可用。

Spring Data Repository 是通过使用形成 repository 组合的片段来实现的。片段是基础repository、功能方面（如 [QueryDsl](https://springdoc.cn/spring-data-mongodb/#core.extensions.querydsl)），以及自定义接口和它们的实现。每当你为你的repository接口添加一个接口，你就通过添加一个片段来增强组合。基础repository和repository方面的实现是由每个Spring Data模块提供的。

下面的例子显示了自定义接口和它们的实现。

Example 32. 片段及其实现

```java
interface HumanRepository {
  void someHumanMethod(User user);
}

class HumanRepositoryImpl implements HumanRepository {

  public void someHumanMethod(User user) {
    // Your custom implementation
  }
}

interface ContactRepository {

  void someContactMethod(User user);

  User anotherContactMethod(User user);
}

class ContactRepositoryImpl implements ContactRepository {

  public void someContactMethod(User user) {
    // Your custom implementation
  }

  public User anotherContactMethod(User user) {
    // Your custom implementation
  }
}
```

下面的例子显示了一个扩展了 `CrudRepository` 的自定义 `repository` 的接口。

Example 33. 修改你的 repository 接口

```java
interface UserRepository extends CrudRepository<User, Long>, HumanRepository, ContactRepository {

  // Declare query methods here
}
```

Repository可以由多个自定义实现组成，这些自定义实现按其声明的顺序被导入。自定义实现的优先级高于基础实现和Repository 切面。这种排序可以让你覆盖基础Repository和切面的方法，并在两个片段贡献了相同的方法签名时解决歧义。Repository片段不限于在单一存储库接口中使用。多个Repository可以使用一个片段接口，让你在不同的 Repository 中重复使用自定义的内容。

下面的例子显示了一个 Repository 片段及其实现。

Example 34. Fragments overriding `save(…)`

```java
interface CustomizedSave<T> {
  <S extends T> S save(S entity);
}

class CustomizedSaveImpl<T> implements CustomizedSave<T> {

  public <S extends T> S save(S entity) {
    // Your custom implementation
  }
}
```

下面的例子显示了一个使用前述 repository 片段的 repository。

Example 35. 自定义 repository 接口

```java
interface UserRepository extends CrudRepository<User, Long>, CustomizedSave<User> {
}

interface PersonRepository extends CrudRepository<Person, Long>, CustomizedSave<Person> {
}
```

##### 配置

repository基础设施试图通过扫描发现repository的包下面的类来自动检测自定义实现片段。这些类需要遵循后缀默认为 `Impl` 的命名惯例。

下面的例子显示了一个使用默认后缀的 repository 和一个为后缀设置了自定义值的 repository。

Example 36. 配置示例

Java

XML

```java
@EnableMongoRepositories(repositoryImplementationPostfix = "MyPostfix")
class Configuration { … }
```

前面例子中的第一个配置试图查找一个叫做 `com.acme.repository.CustomizedUserRepositoryImpl` 的类，作为一个自定义的 repository 实现。第二个例子试图查找 `com.acme.repository.CustomizedUserRepositoryMyPostfix`。

###### 消除歧义

如果在不同的包中发现了具有匹配类名的多个实现，Spring Data会使用Bean名称来确定使用哪一个。

考虑到前面显示的 `CustomizedUserRepository` 的以下两个自定义实现，第一个实现被使用。它的Bean名是 `customedUserRepositoryImpl`，与片段接口（`CustomizedUserRepository`）加后缀 `Impl` 的名字一致。

Example 37. 解决模棱两可的实现

```java
package com.acme.impl.one;

class CustomizedUserRepositoryImpl implements CustomizedUserRepository {

  // Your custom implementation
}
package com.acme.impl.two;

@Component("specialCustomImpl")
class CustomizedUserRepositoryImpl implements CustomizedUserRepository {

  // Your custom implementation
}
```

如果你用 `@Component("specialCustom")` 来注解 `UserRepository` 接口，那么 Bean 的名字加上 `Impl` 就与 `com.acme.impl.two` 中为 repository 实现定义的名字相匹配，并被用来代替第一个接口。

###### 手动注入

如果你的自定义实现只使用基于注解的配置和自动注入，前面所示的方法很好用，因为它被当作任何其他Spring Bean。如果你的实现片段Bean需要特殊的注入，你可以根据[前文](https://springdoc.cn/spring-data-mongodb/#repositories.single-repository-behaviour.ambiguity)所述的约定来声明Bean并为其命名。然后，基础设施通过名称来引用手动定义的Bean定义，而不是自己创建一个。下面的例子展示了如何手动注入一个自定义的实现。

Example 38. 手动注入一个自定义实现

Java

XML

```java
class MyClass {
  MyClass(@Qualifier("userRepositoryImpl") UserRepository userRepository) {
    …
  }
}
```

#### 8.6.2. 自定义 Base Repository

当你想定制基础 repository 的行为时，[上一节](https://springdoc.cn/spring-data-mongodb/#repositories.manual-wiring)描述的方法需要定制每个 repository 的接口，以便所有的 repository 都受到影响。为了改变所有 repository 的行为，你可以创建一个扩展持久化技术特定 repository 基类的实现。然后这个类作为 repository 代理的自定义基类，如下面的例子所示。

Example 39. 自定义 repository base 类

```java
class MyRepositoryImpl<T, ID>
  extends SimpleJpaRepository<T, ID> {

  private final EntityManager entityManager;

  MyRepositoryImpl(JpaEntityInformation entityInformation,
                          EntityManager entityManager) {
    super(entityInformation, entityManager);

    // Keep the EntityManager around to used from the newly introduced methods.
    this.entityManager = entityManager;
  }

  @Transactional
  public <S extends T> S save(S entity) {
    // implementation goes here
  }
}
```

|      | 该类需要有一个super类的构造器，store特定的 repository factory 实现使用该构造器。如果repository 基类有多个构造函数，请复写其中一个构造函数，该构造函数需要一个 `EntityInformation` 和一个store特定的基础设施对象（如 `EntityManager` 或模板类）。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

最后一步是让Spring Data基础设施意识到定制的 repository base 类。在配置中，你可以通过使用 `repositoryBaseClass` 来做到这一点，如下面的例子所示。

Example 40. 配置一个自定义 repository base 类

Java

XML

```java
@Configuration
@EnableMongoRepositories(repositoryBaseClass = MyRepositoryImpl.class)
class ApplicationConfiguration { … }
```

### 8.7. 从 Aggregate Root （聚合ROOT）中发布事件

由 Repository 管理的实体是 aggregate root。在领域驱动设计应用程序中，这些aggregate root通常会发布 domain 事件。Spring Data提供了一个名为 `@DomainEvents` 的注解，你可以在 aggregate root 的一个方法上使用该注解，以使这种发布尽可能地简单，如下例所示。

Example 41. 从aggregate root中暴露domain 事件

```java
class AnAggregateRoot {

    @DomainEvents 
    Collection<Object> domainEvents() {
        // … return events you want to get published here
    }

    @AfterDomainEventPublication 
    void callbackMethod() {
       // … potentially clean up domain events list
    }
}
```

|      | 使用 `@DomainEvents` 的方法可以返回一个单一的事件实例或一个事件的集合。它必须不接受任何参数。 |
| ---- | ------------------------------------------------------------ |
|      | 在所有的事件都被发布后，我们有一个用 `@AfterDomainEventPublication` 注解的方法。你可以用它来潜在地清理要发布的事件列表（除其他用途外）。 |

每次调用Spring Data Repository的 `save(…)`、`saveAll(…)`、`delete(…)` 或 `deleteAll(…)` 方法时都会调用这些方法。

### 8.8. Spring Data 扩展

本节记录了一组Spring Data扩展，这些扩展使Spring Data能够在各种情况下使用。目前，大部分的集成都是针对Spring MVC的。

#### 8.8.1. Querydsl 扩展

[Querydsl](http://www.querydsl.com/) 是一个框架，可以通过其 fluent API构建静态类型的类似SQL的查询。

一些Spring Data模块通过 `QuerydslPredicateExecutor` 提供与 Querydsl 的集成，正如下面的例子所示。

Example 42. QuerydslPredicateExecutor interface

```java
public interface QuerydslPredicateExecutor<T> {

  Optional<T> findById(Predicate predicate);  

  Iterable<T> findAll(Predicate predicate);   

  long count(Predicate predicate);            

  boolean exists(Predicate predicate);        

  // … more functionality omitted.
}
```

|      | 返回符合 `Predicate` 的实体。       |
| ---- | ----------------------------------- |
|      | 返回所有符合 `Predicate` 的实体。   |
|      | 返回符合 `Predicate` 实体的数量。   |
|      | 返回是否有符合 `Predicate` 的实体。 |

为了使用 Querydsl 支持，在你的版本库接口上扩展 `QuerydslPredicateExecutor`，如下面的例子所示。

Example 43. Repository 上的 Querydsl 整合

```java
interface UserRepository extends CrudRepository<User, Long>, QuerydslPredicateExecutor<User> {
}
```

前面的例子让你通过使用 Querydsl `Predicate` 实例来编写类型安全的查询，如下图所示。

```java
Predicate predicate = user.firstname.equalsIgnoreCase("dave")
	.and(user.lastname.startsWithIgnoreCase("mathews"));

userRepository.findAll(predicate);
```

#### 8.8.2. Web 的支持

支持 repository 编程模型的Spring Data模块带有各种Web支持。web相关的组件需要添加 Spring MVC 到项目。其中一些甚至提供了与 [Spring HATEOAS](https://github.com/spring-projects/spring-hateoas)的整合。一般来说，集成支持是通过在你的 `JavaConfig` 配置类中使用 `@EnableSpringDataWebSupport` 注解来启用的，如下面例子所示。

Example 44. 启用 Spring Data web 支持

Java

XML

```java
@Configuration
@EnableWebMvc
@EnableSpringDataWebSupport
class WebConfiguration {}
```

`@EnableSpringDataWebSupport` 注解注册了一些组件。我们将在本节后面讨论这些组件。它还会检测classpath上的Spring HATEOAS，并为其注册整合组件（如果存在）。

##### 基本的 Web 支持

在XML中启用Spring Data Web支持

[上一节](https://springdoc.cn/spring-data-mongodb/#core.web)所示的配置注册了一些基本组件。

- [使用 `DomainClassConverter` 类](https://springdoc.cn/spring-data-mongodb/#core.web.basic.domain-class-converter) 让Spring MVC从请求参数或路径变量中解析 Repository 管理的 domain 类实例。
- [`HandlerMethodArgumentResolver`](https://springdoc.cn/spring-data-mongodb/#core.web.basic.paging-and-sorting) 的实现，让Spring MVC从请求参数中解析 `Pageable` 和 `Sort` 实例。
- [Jackson模块](https://springdoc.cn/spring-data-mongodb/#core.web.basic.jackson-mappers) 对 `Point` 和 `Distance` 等类型进行序列化/反序列化，或存储特定的类型，这取决于使用的Spring数据模块。

###### 使用 `DomainClassConverter` 类

`DomainClassConverter` 类让你在Spring MVC Controller 方法签名中直接使用 domain 类型，这样你就不需要通过 repository 手动查找实例了，如下例所示。

Example 45. 一个在方法签名中使用 domain 类型的Spring MVC controller

```java
@Controller
@RequestMapping("/users")
class UserController {

  @RequestMapping("/{id}")
  String showUserForm(@PathVariable("id") User user, Model model) {

    model.addAttribute("user", user);
    return "userForm";
  }
}
```

该方法直接接收一个 `User` 实例，而不需要进一步的查找。该实例可以通过让Spring MVC先将路径变量转换为domain类的 `id` 类型来解决，最终通过调用为domain类注册的资源库实例 `findById(…)` 来访问该实例。

|      | 目前，repository 必须实现 `CrudRepository` 才有资格被发现进行转换。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

###### 使用 HandlerMethodArgumentResolvers 解析 Pageable 和 Sort

[上一节](https://springdoc.cn/spring-data-mongodb/#core.web.basic.domain-class-converter) 中的配置片段还注册了一个 `PageableHandlerMethodArgumentResolver` 以及一个 `SortHandlerMethodArgumentResolver` 的实例。注册后，`Pageable` 和 `Sort` 可以作为有效的controller方法参数，如下图所示。

Example 46. 使用 Pageable 作为 controller 方法参数

```java
@Controller
@RequestMapping("/users")
class UserController {

  private final UserRepository repository;

  UserController(UserRepository repository) {
    this.repository = repository;
  }

  @RequestMapping
  String showUsers(Model model, Pageable pageable) {

    model.addAttribute("users", repository.findAll(pageable));
    return "users";
  }
}
```

前面的方法签名使Spring MVC尝试通过使用以下默认配置从请求参数中派生出一个 `Pageable` 实例。

| `page` | 你想检索的页。索引从0开始，默认为0。                         |
| ------ | ------------------------------------------------------------ |
| `size` | 你想检索的每页数据大小。默认为20。                           |
| `sort` | 应该按格式 `property,property(,ASC|DESC)(,IgnoreCase)` 进行排序的属性。默认的排序方向是对大小写敏感的升序。如果你想切换方向或大小写敏感性，请使用多个排序参数—例如，`?sort=firstname&sort=lastname,asc&sort=city,ignorecase`。 |

要自定义这种行为，请注册一个分别实现 `PageableHandlerMethodArgumentResolverCustomizer` 接口或 `SortHandlerMethodArgumentResolverCustomizer` 接口的bean。它的 `customize()` 方法会被调用，让你改变设置，正如下面的例子所示。

```java
@Bean SortHandlerMethodArgumentResolverCustomizer sortCustomizer() {
    return s -> s.setPropertyDelimiter("<-->");
}
```

如果设置现有 `MethodArgumentResolver` 的属性不足以满足你的目的，可以扩展 `SpringDataWebConfiguration` 或启用HATEOAS的等价物，覆盖 `pageableResolver()` 或 `sortResolver()` 方法，并导入你的自定义的配置文件，而不是使用 `@Enable` 注解。

如果你需要从请求中解析多个 `Pageable` 或 `Sort` 实例（例如多个表），你可以使用 Spring 的 `@Qualifier` 注解来区分一个和另一个。然后请求参数必须以 `${qualifier}_` 为前缀。下面的例子显示了由此产生的方法签名。

```java
String showUsers(Model model,
      @Qualifier("thing1") Pageable first,
      @Qualifier("thing2") Pageable second) { … }
```

你必须填充 `thing1_page`、`thing2_page`，以此类推。

传入该方法的默认 `Pageable` 相当于一个 `PageRequest.of(0, 20)`，但你可以通过在 `Pageable` 参数上使用 `@PageableDefault` 注解来定制它。

##### 对 Pageable 的 Hypermedia 支持

Spring HATEOAS提供了一个表示 model 类（`PagedResources`），它允许用必要的页面元数据以及链接来丰富 `Page` 实例的内容，让客户轻松地浏览页面。`Page` 到 `PagedResources` 的转换是由Spring HATEOAS `ResourceAssembler` 接口的实现完成的，这个接口被称为 `PagedResourcesAssembler`。下面的例子展示了如何使用 `PagedResourcesAssembler` 作为 controller 方法的参数。

Example 47. 使用 PagedResourcesAssembler 作为 controller 方法参数

```java
@Controller
class PersonController {

  @Autowired PersonRepository repository;

  @RequestMapping(value = "/persons", method = RequestMethod.GET)
  HttpEntity<PagedResources<Person>> persons(Pageable pageable,
    PagedResourcesAssembler assembler) {

    Page<Person> persons = repository.findAll(pageable);
    return new ResponseEntity<>(assembler.toResources(persons), HttpStatus.OK);
  }
}
```

启用配置，如前面的例子所示，让 `PagedResourcesAssembler` 被用作控制器方法的参数。对它调用 `toResources(…)` 有以下效果。

- `Page` 的内容成为 `PagedResources` 实例的内容。
- `PagedResources` 对象被附加了一个 `PageMetadata` 实例，它被填充了来自 `Page` 和基础 `PageRequest` 的信息。
- `PagedResources` 可能会有一个 `prev` 和一个 `next` 链接，这取决于页面的状态。这些链接指向该方法所映射的URI。添加到方法中的分页参数与 `PageableHandlerMethodArgumentResolver` 的设置相匹配，以确保链接可以在稍后被解析。

假设我们在数据库中有30个的 `Person` 实例。现在你可以触发一个请求（`GET http://localhost:8080/persons`），看到类似以下的输出。

```javascript
{ "links" : [ { "rel" : "next",
                "href" : "http://localhost:8080/persons?page=1&size=20" }
  ],
  "content" : [
     … // 20 Person instances rendered here
  ],
  "pageMetadata" : {
    "size" : 20,
    "totalElements" : 30,
    "totalPages" : 2,
    "number" : 0
  }
}
```

assembler 产生了正确的URI，并且还拾取了默认的配置，以便为即将到来的请求将参数解析为一个 `Pageable`。这意味着，如果你改变了配置，链接会自动遵守这一变化。默认情况下，assembler 会指向它被调用的controller方法，但你可以通过传递一个自定义的 `Link` 来定制，作为建立分页链接的基础，它重载了 `PagedResourcesAssembler.toResource(..)` 方法。

##### Spring Data Jackson 模块

核心模块和一些特定的存储模块与一组Jackson模块一起发布，用于Spring Data domain 域使用的类型，如 `org.springframework.data.geo.Distance` 和 `org.springframework.data.geo.Point`。 一旦启用 [web支持](https://springdoc.cn/spring-data-mongodb/#core.web) 和 `com.fasterxml.jackson.databind.ObjectMapper` 可用，就会导入这些模块。

在初始化过程中，`SpringDataJacksonModules` 和 `SpringDataJacksonConfiguration` 一样，被基础设施所接收，这样，声明的 `com.fasterxml.jackson.databind.Module` 就被提供给Jackson `ObjectMapper`。

以下domain类型的 Data binding mixins 由公共基础设施注册。

```
org.springframework.data.geo.Distance
org.springframework.data.geo.Point
org.springframework.data.geo.Box
org.springframework.data.geo.Circle
org.springframework.data.geo.Polygon
```

|      | 单个模块可以提供额外的 `SpringDataJacksonModules`。更多细节请参考商店的具体章节。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### Web Databinding 的支持

你可以通过使用 [JSONPath](https://goessner.net/articles/JsonPath/) 表达式（需要 [Jayway JsonPath](https://github.com/json-path/JsonPath)）或 [XPath](https://www.w3.org/TR/xpath-31/) 表达式（需要 [XmlBeam](https://xmlbeam.org/)）来使用 Spring Data 投影（在 [投影](https://springdoc.cn/spring-data-mongodb/#projections) 中描述）来绑定传入的请求的payload，如下例所示。

Example 48. 使用JSONPath 或 XPath 表达式来绑定HTTP payload

```java
@ProjectedPayload
public interface UserPayload {

  @XBRead("//firstname")
  @JsonPath("$..firstname")
  String getFirstname();

  @XBRead("/lastname")
  @JsonPath({ "$.lastname", "$.user.lastname" })
  String getLastname();
}
```

你可以将前面的例子中显示的类型作为Spring MVC controller 的方法参数，或者通过在 `RestTemplate` 的某个方法中使用 `ParameterizedTypeReference`。前面的方法声明将尝试在给定 document 中的任何地方找到 `firstname`。`lastname` 的XML查找是在传入 document 的顶层进行的。JSON的变体首先尝试顶层的 `lastname`，但是如果前者没有返回一个值，也会尝试嵌套在 `user` 子 document 中的 `lastname`。这样，源 document 结构的变化可以很容易地被减轻，而不需要客户端调用暴露的方法（通常是基于类的 payload 绑定的一个缺点）。

如 [投影](https://springdoc.cn/spring-data-mongodb/#projections) 中所述，支持嵌套投影。如果该方法返回一个复杂的、非接口类型，则使用Jackson `ObjectMapper` 来映射最终值。

对于Spring MVC，一旦 `@EnableSpringDataWebSupport` 被激活，并且classpath上有必要的依赖，必要的 converter 就会被自动注册。对于 `RestTemplate` 的使用，需要手动注册一个 `ProjectingJackson2HttpMessageConverter`（JSON）或 `XmlBeamHttpMessageConverter`。

欲了解更多信息，请参见 [Spring Data 示例库](https://github.com/spring-projects/spring-data-examples) 中的 [web投影示例](https://github.com/spring-projects/spring-data-examples/tree/master/web/projection)。

##### Querydsl 的 Web 支持

对于那些有 [QueryDSL](http://www.querydsl.com/) 集成的 store，你可以从 `Request` 查询字符串中包含的属性导出查询。

考虑下面这个查询字符串：

```text
?firstname=Dave&lastname=Matthews
```

给出前面例子中的 `User` 对象，你可以通过使用 `QuerydslPredicateArgumentResolver` 将一个查询字符串解析为以下值，如下所示。

```text
QUser.user.firstname.eq("Dave").and(QUser.user.lastname.eq("Matthews"))
```

|      | 当在 classpath 上发现 Querydsl 时，该功能与 `@EnableSpringDataWebSupport` 一起被自动启用。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

在方法签名中添加 `@QuerydslPredicate` 提供了一个随时可用的 `Predicate`，你可以通过使用 `QuerydslPredicateExecutor` 来运行它。

|      | 类型信息通常是由方法的返回类型来解决的。由于该信息不一定与 domain 类型相匹配，使用 `QuerydslPredicate` 的 `root` 属性可能是个好主意。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

下面的例子显示了如何在方法签名中使用 `@QuerydslPredicate`。

```java
@Controller
class UserController {

  @Autowired UserRepository repository;

  @RequestMapping(value = "/", method = RequestMethod.GET)
  String index(Model model, @QuerydslPredicate(root = User.class) Predicate predicate,    
          Pageable pageable, @RequestParam MultiValueMap<String, String> parameters) {

    model.addAttribute("users", repository.findAll(predicate, pageable));

    return "index";
  }
}
```

|      | 将查询字符串参数解析为 `User` 的匹配 `Predicate`。 |
| ---- | -------------------------------------------------- |
|      |                                                    |

默认的绑定方式如下。

- `Object` 在简单的属性上作为 `eq`。
- `Object` 在集合上的属性与 `contains` 一样。
- `Collection` 上的简单属性为 `in`。

你可以通过 `@QuerydslPredicate` 的 `bindings` 属性或者利用Java 8的 `default methods` 来定制这些绑定，并将 `QuerydslBinderCustomizer` 方法添加到 repository 接口，如下所示。

```java
interface UserRepository extends CrudRepository<User, String>,
                                 QuerydslPredicateExecutor<User>,                
                                 QuerydslBinderCustomizer<QUser> {               

  @Override
  default void customize(QuerydslBindings bindings, QUser user) {

    bindings.bind(user.username).first((path, value) -> path.contains(value))    
    bindings.bind(String.class)
      .first((StringPath path, String value) -> path.containsIgnoreCase(value)); 
    bindings.excluding(user.password);                                           
  }
}
```

|      | `QuerydslPredicateExecutor` 提供了对 `Predicate` 的特定查找方法的访问。 |
| ---- | ------------------------------------------------------------ |
|      | repository 接口上定义的 `QuerydslBinderCustomizer` 被自动拾取，并成为 `@QuerydslPredicate(bindings=…)` 的快捷方式。 |
|      | 定义 `username` 属性的绑定是一个简单的 `contains` 绑定。     |
|      | 定义 `String` 属性的默认绑定为不区分大小写的 `contains` 匹配。 |
|      | 将 `password` 属性排除在 `Predicate` 解析之外。              |

|      | 你可以在应用来自 repository 或 `@QuerydslPredicate` 的特定绑定之前，注册一个持有默认Querydsl绑定的 `QuerydslBinderCustomizerDefaults` bean。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 8.8.3. Repository 填充

如果你使用Spring JDBC模块，你可能很熟悉对用SQL脚本填充 `DataSource` 的支持。在 repository 层面也有类似的抽象，尽管它不使用SQL作为数据定义语言，因为它必须是独立于store的。因此，填充器支持XML（通过Spring的OXM抽象）和JSON（通过Jackson）来定义数据，用它来填充repository。

假设你有一个名为 `data.json` 的文件，内容如下。

Example 49. 在JSON中定义的数据

```javascript
[ { "_class" : "com.acme.Person",
 "firstname" : "Dave",
  "lastname" : "Matthews" },
  { "_class" : "com.acme.Person",
 "firstname" : "Carter",
  "lastname" : "Beauford" } ]
```

你可以通过使用Spring Data Commons中提供的 Repository 命名空间的populator元素来填充你的Repository。为了将前面的数据填充到你的 `PersonRepository` 中，声明一个类似于下面的 populator。

Example 50. 声明一个 Jackson repository populator

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:repository="http://www.springframework.org/schema/data/repository"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/data/repository
    https://www.springframework.org/schema/data/repository/spring-repository.xsd">

  <repository:jackson2-populator locations="classpath:data.json" />

</beans>
```

前面的声明导致 `data.json` 文件被 Jackson `ObjectMapper` 读取和反序列化。

JSON对象被反序列化的类型是通过检查JSON文档的 `_class` 属性决定的。基础设施最终会选择适当的 repository 来处理被反序列化的对象。

为了使用XML来定义 repository 应该填充的数据，你可以使用 `unmarshaller-populator` 元素。你把它配置为使用Spring OXM中的一个 XML marshaller 选项。详情请参见 [Spring参考文档](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#oxm)。下面的例子展示了如何用JAXB来 unmarshall 对 repository 填充器的 marshall。

下面的例子显示了如何用 JAXB 来 `unmarshall` 一个 repository 填充器（populator）。

Example 51. 声明一个 `unmarshalling` repository populator（使用JAXB）。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:repository="http://www.springframework.org/schema/data/repository"
  xmlns:oxm="http://www.springframework.org/schema/oxm"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/data/repository
    https://www.springframework.org/schema/data/repository/spring-repository.xsd
    http://www.springframework.org/schema/oxm
    https://www.springframework.org/schema/oxm/spring-oxm.xsd">

  <repository:unmarshaller-populator locations="classpath:data.json"
    unmarshaller-ref="unmarshaller" />

  <oxm:jaxb2-marshaller contextPath="com.acme" />

</beans>
```

# 参考文档

## 9. 简介

### 9.1. 文档结构

参考文档的这一部分解释了 Spring Data MongoDB 所提供的核心功能。

“[MongoDB 的支持](https://springdoc.cn/spring-data-mongodb/#mongo.core)” 介绍了MongoDB模块的功能集。

“[MongoDB Repository](https://springdoc.cn/spring-data-mongodb/#mongo.repositories)” 介绍了对 MongoDB 的 repository 支持。

## 10. MongoDB 的支持

MongoDB的支持包含广泛的功能。

- Spring配置支持基于Java的 `@Configuration` 类或XML命名空间的Mongo驱动实例和复制集。
- `MongoTemplate` 辅助类，在执行常见的Mongo操作时提高生产力。包括文档和POJO之间的集成对象映射。
- 异常翻译成Spring的可移植 Data Access Exception 层次结构。
- 功能丰富的对象映射与Spring的转换服务集成。
- 基于注解的映射元数据，可扩展到支持其他元数据格式。
- 持久化和映射生命周期事件。
- 基于Java的查询、Criteria和更新DSL。
- 自动实现 Repository 接口，包括支持自定义查询方法。
- QueryDSL集成，支持类型安全的查询。
- 对JPA实体的跨存储持久性支持，其字段可通过MongoDB进行透明的持久化和检索（已废弃—将被移除，没有替换）。
- 地理空间（GeoSpatial ）整合。

对于大多数任务，你应该使用 `MongoTemplate` 或 `Repository` 支持，它们都利用了丰富的映射功能。`MongoTemplate` 是访问功能的地方，如递增计数器或临时CRUD操作。`MongoTemplate` 还提供了回调方法，这样你就很容易获得低级别的API工件，如 `com.mongodb.client.MongoDatabase`，从而直接与MongoDB进行通信。对各种API工件进行命名的目的是为了复制基础MongoDB Java驱动中的那些API工件，这样你就可以轻松地将现有知识映射到Spring API上。

### 10.1. 入门

建立工作环境的一个简单方法是在 [STS](https://spring.io/tools/) 中创建一个基于Spring的项目。

首先，你需要设置一个正在运行的MongoDB服务器。请参考 [MongoDB快速入门指南](https://docs.mongodb.org/manual/core/introduction/)，了解如何启动MongoDB实例。安装完毕后，启动MongoDB通常只需运行以下命令即可。`${MONGO_HOME}/bin/mongod`

在STS中创建一个Spring项目：

1. Go to File → New → Spring Template Project → Simple Spring Utility Project，并在出现提示时按Yes。然后输入一个项目和一个包的名称，比如 `org.spring.mongodb.example`。

2. 在pom.xml文件的 `dependencies` 元素中加入以下内容。

   ```xml
   <dependencies>
   
     <!-- other dependency elements omitted -->
   
     <dependency>
       <groupId>org.springframework.data</groupId>
       <artifactId>spring-data-mongodb</artifactId>
       <version>4.1.0-SNAPSHOT</version>
     </dependency>
   
   </dependencies>
   ```

3. 将pom.xml中Spring的版本改为：

   ```xml
   <spring.framework.version>6.0.4</spring.framework.version>
   ```

4. 在你的 `pom.xml` 中添加以下 Maven 的 Spring Milestone repository 的位置，使其位于 `<dependencies/>` 元素的同一级别。

   ```xml
   <repositories>
     <repository>
       <id>spring-milestone</id>
       <name>Spring Maven MILESTONE Repository</name>
       <url>https://repo.spring.io/libs-milestone</url>
     </repository>
   </repositories>
   ```

repository 也可在此 [浏览](https://repo.spring.io/milestone/org/springframework/data/)。

你可能还想把日志级别设置为 `DEBUG`，以查看一些额外的信息。要做到这一点，编辑 `log4j.properties` 文件，使其具有以下内容。

```
log4j.category.org.springframework.data.mongodb=DEBUG
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %40.40c:%4L - %m%n
```

然后你可以创建一个 `Person` 类来持久化。

```java
package org.spring.mongodb.example;

public class Person {

  private String id;
  private String name;
  private int age;

  public Person(String name, int age) {
    this.name = name;
    this.age = age;
  }

  public String getId() {
    return id;
  }
  public String getName() {
    return name;
  }
  public int getAge() {
    return age;
  }

  @Override
  public String toString() {
    return "Person [id=" + id + ", name=" + name + ", age=" + age + "]";
  }
}
```

你还需要一个 main application 来运行。

```java
package org.spring.mongodb.example;

public class MongoApp {

  private static final Log log = LogFactory.getLog(MongoApp.class);

  public static void main(String[] args) throws Exception {

    MongoOperations mongoOps = new MongoTemplate(MongoClients.create(), "database");
    mongoOps.insert(new Person("Joe", 34));

    log.info(mongoOps.findOne(new Query(where("name").is("Joe")), Person.class));

    mongoOps.dropCollection("person");
  }
}
```

当你运行 main 程序时，前面的例子会产生以下输出。

```
10:01:32,062 DEBUG apping.MongoPersistentEntityIndexCreator:  80 - Analyzing class class org.spring.example.Person for index information.
10:01:32,265 DEBUG ramework.data.mongodb.core.MongoTemplate: 631 - insert Document containing fields: [_class, age, name] in collection: Person
10:01:32,765 DEBUG ramework.data.mongodb.core.MongoTemplate:1243 - findOne using query: { "name" : "Joe"} in db.collection: database.Person
10:01:32,953  INFO      org.spring.mongodb.example.MongoApp:  25 - Person [id=4ddbba3c0be56b7e1b210166, name=Joe, age=34]
10:01:32,984 DEBUG ramework.data.mongodb.core.MongoTemplate: 375 - Dropped collection [database.person]
```

即使在这个简单的例子中，也有一些事情需要注意。

- 你可以通过使用标准的 `com.mongodb.client.MongoClient` 对象和要使用的数据库名称，来实例化Spring Mongo的中心辅助类 [`MongoTemplate`](https://springdoc.cn/spring-data-mongodb/#mongo-template)。
- 该 mapper 针对标准的POJO对象工作，不需要任何额外的元数据（尽管你可以选择性地提供这些信息。 见 [这里](https://springdoc.cn/spring-data-mongodb/#mapping-chapter)）。
- 惯例用于处理 `id` 字段，在数据库中存储时将其转换为 `ObjectId`。
- 映射约定可以使用字段访问。请注意，`Person` 类只有 getter。
- 如果构造函数的参数名与存储文档的字段名相匹配，它们将被用来实例化对象。

### 10.2. 示例仓库

有一个GitHub仓库，里面有 [几个例子](https://github.com/spring-projects/spring-data-examples)，你可以下载并玩一玩，感受一下这个库是如何工作的。

### 10.3. 使用 Spring 连接到 MongoDB

在使用MongoDB和Spring时，首要任务之一是使用IoC容器创建一 个 `com.mongodb.client.MongoClient` 对象。有两种主要的方法可以做到这一点，一种是使用基于Java的bean元数据，另一种是使用基于 XML 的 bean 元数据。这两种方式将在下面的章节中讨论。

|      | 对于那些不熟悉如何使用基于 Java 的 Bean 元数据而不是基于XML的元数据来配置Spring容器的人来说，请看 [这里](https://docs.spring.io/spring/docs/3.2.x/spring-framework-reference/html/new-in-3.0.html#new-java-configuration)的参考文档中的高层介绍，以及 [这里](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#beans-java-instantiating-container)的详细文档。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 10.3.1. 通过使用基于 Java 的元数据注册 Mongo 实例

下面的例子显示了一个使用基于Java的bean元数据来注册 `com.mongodb.client.MongoClient` 实例的例子。

Example 52. 使用基于Java的bean元数据注册一个 `com.mongodb.client.MongoClient` 对象

```java
@Configuration
public class AppConfig {

  /*
   * Use the standard Mongo driver API to create a com.mongodb.client.MongoClient instance.
   */
   public @Bean MongoClient mongoClient() {
       return MongoClients.create("mongodb://localhost:27017");
   }
}
```

这种方法可以让你使用标准的 `com.mongodb.client.MongoClient` 实例，而容器则使用 Spring 的 `MongoClientFactoryBean`。与直接实例化 `com.mongodb.client.MongoClient` 实例相比，`FactoryBean` 的额外优势在于它还为容器提供了 `ExceptionTranslator` 实现，该实现可将 MongoDB 异常转换为 Spring 的可移植 `DataAccessException` 层次结构中的异常，用于注释有 `@Repository` 的数据访问类。这个层次结构和 `@Repository` 的使用在 [Spring的DAO支持功能](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html) 中有所描述。

下面的例子显示了一个基于Java的Bean元数据的例子，它支持对 `@Repository` 注解的类进行异常转换。

Example 53. 通过使用Spring的 `MongoClientFactoryBean` 注册 `com.mongodb.client.MongoClient` 对象，并启用Spring的异常翻译支持。

```java
@Configuration
public class AppConfig {

    /*
     * Factory bean that creates the com.mongodb.client.MongoClient instance
     */
     public @Bean MongoClientFactoryBean mongo() {
          MongoClientFactoryBean mongo = new MongoClientFactoryBean();
          mongo.setHost("localhost");
          return mongo;
     }
}
```

要访问由 `MongoClientFactoryBean` 在其他 `@Configuration` 类或你自己的类中创建的 `com.mongodb.client.MongoClient` 对象，请使用一个私有的 `@Autowired MongoClient mongoClient;` 字段。

#### 10.3.2. 通过使用基于XML的元数据注册Mongo实例

虽然你可以使用Spring传统的 `<beans/>` XML命名空间来向容器注册 `com.mongodb.client.MongoClient` 的实例，但由于它是通用的，所以XML可能相当冗长。XML 命名空间是配置常用对象（如 Mongo 实例）的一个更好的选择。mongo命名空间可以让你创建一个Mongo实例的服务器位置、副本集和选项。

要使用Mongo命名空间元素，你需要引用Mongo schema，如下所示。

Example 54. 配置MongoDB的XML schema

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:mongo="http://www.springframework.org/schema/data/mongo"
          xsi:schemaLocation=
          "
          http://www.springframework.org/schema/data/mongo https://www.springframework.org/schema/data/mongo/spring-mongo.xsd
          http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- Default bean name is 'mongo' -->
    <mongo:mongo-client host="localhost" port="27017"/>

</beans>
```

Example 55. 配置具有 `MongoClientSettings` 的 `com.mongodb.client.MongoClient` 对象的XML schema

```xml
<beans>

  <mongo:mongo-client host="localhost" port="27017">
    <mongo:client-settings connection-pool-max-connection-life-time="10"
        connection-pool-min-size="10"
		connection-pool-max-size="20"
		connection-pool-maintenance-frequency="10"
		connection-pool-maintenance-initial-delay="11"
		connection-pool-max-connection-idle-time="30"
		connection-pool-max-wait-time="15" />
  </mongo:mongo-client>

</beans>
```

下面的例子显示了一个使用副本集的配置。

Example 56. 用 XML schema 来配置带有Replica Sets的 `com.mongodb.client.MongoClient` 对象

```xml
<mongo:mongo-client id="replicaSetMongo" replica-set="rs0">
    <mongo:client-settings cluster-hosts="127.0.0.1:27017,localhost:27018" />
</mongo:mongo-client>
```

#### 10.3.3. MongoDatabaseFactory 接口

虽然 `com.mongodb.client.MongoClient` 是 MongoDB 驱动程序 API 的入口，但连接到特定的 MongoDB 数据库实例需要额外的信息，例如数据库名称以及可选的用户名和密码。有了这些信息，你就可以获得 `com.mongodb.client.MongoDatabase` 对象并访问特定 MongoDB 数据库实例的所有功能。Spring 提供了 `org.springframework.data.mongodb.core.MongoDatabaseFactory` 接口，如下表所示，用于引导与数据库的连接。

```java
public interface MongoDatabaseFactory {

  MongoDatabase getDatabase() throws DataAccessException;

  MongoDatabase getDatabase(String dbName) throws DataAccessException;
}
```

下面的章节展示了你如何使用基于 Java 或基于 XML 的元数据的容器来配置 `MongoDatabaseFactory` 接口的实例。反过来，你可以使用 `MongoDatabaseFactory` 实例来配置 `MongoTemplate`。

你可以在标准的Java代码中使用它们，而不是使用IoC容器来创建 `MongoTemplate` 的实例，如下所示。

```java
public class MongoApp {

  private static final Log log = LogFactory.getLog(MongoApp.class);

  public static void main(String[] args) throws Exception {

    MongoOperations mongoOps = new MongoTemplate(new SimpleMongoClientDatabaseFactory(MongoClients.create(), "database"));

    mongoOps.insert(new Person("Joe", 34));

    log.info(mongoOps.findOne(new Query(where("name").is("Joe")), Person.class));

    mongoOps.dropCollection("person");
  }
}
```

黑体字的代码强调了 `SimpleMongoClientDbFactory` 的使用，这也是与 [入门部分](https://springdoc.cn/spring-data-mongodb/#mongodb-getting-started) 所示清单的唯一区别。

|      | 在选择 `com.mongodb.client.MongoClient` 作为首选入口时，使用 `SimpleMongoClientDbFactory`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 10.3.4. 注册 `MongoDatabaseFactory`

为了在容器中注册一个 `MongoDatabaseFactory` 实例，你写的代码很像前面代码清单中强调的那样。下面的列表显示了一个简单的例子。

```java
@Configuration
public class MongoConfiguration {

  @Bean
  public MongoDatabaseFactory mongoDatabaseFactory() {
    return new SimpleMongoClientDatabaseFactory(MongoClients.create(), "database");
  }
}
```

MongoDB服务器第3代改变了连接到DB时的认证模式。因此，一些可用于认证的配置选项不再有效。你应该使用 `MongoClient` 特有的选项，通过 `MongoCredential` 设置证书，以提供认证数据，如下例所示。

Java

XML

```java
@Configuration
public class ApplicationContextEventTestsAppConfig extends AbstractMongoClientConfiguration {

  @Override
  public String getDatabaseName() {
    return "database";
  }

  @Override
  protected void configureClientSettings(Builder builder) {

    builder
        .credential(MongoCredential.createCredential("name", "db", "pwd".toCharArray()))
        .applyToClusterSettings(settings  -> {
          settings.hosts(singletonList(new ServerAddress("127.0.0.1", 27017)));
        });
  }
}
```

|      | 在基于XML的配置中使用的用户名和密码凭证，如果包含保留字符，如 `:`, `%`, `@` 或 `,` 必须进行URL编码。下面的例子显示了编码的凭证： `m0ng0@dmin:mo_res:bw6},Qsdxx@admin@database` → `m0ng0%40dmin:mo_res%3Abw6%7D%2CQsdxx%40admin@database` 更多细节请参见 [RFC 3986的2.2节](https://tools.ietf.org/html/rfc3986#section-2.2)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

如果你需要在用于创建 `SimpleMongoClientDbFactory` 的 `com.mongodb.client.MongoClient` 实例上配置额外的选项，你可以参考现有的 bean，如以下示例所示。为了显示另一种常见的使用模式，下面的列表显示了属性占位符的使用，它可以让你对配置和 `MongoTemplate` 的创建进行参数化。

Java

XML

```java
@Configuration
@PropertySource("classpath:/com/myapp/mongodb/config/mongo.properties")
public class ApplicationContextEventTestsAppConfig extends AbstractMongoClientConfiguration {

  @Autowired
  Environment env;

  @Override
  public String getDatabaseName() {
    return "database";
  }

  @Override
  protected void configureClientSettings(Builder builder) {

    builder.applyToClusterSettings(settings -> {
    settings.hosts(singletonList(
          new ServerAddress(env.getProperty("mongo.host"), env.getProperty("mongo.port", Integer.class))));
    });

    builder.applyToConnectionPoolSettings(settings -> {

      settings.maxConnectionLifeTime(env.getProperty("mongo.pool-max-life-time", Integer.class), TimeUnit.MILLISECONDS)
          .minSize(env.getProperty("mongo.pool-min-size", Integer.class))
          .maxSize(env.getProperty("mongo.pool-max-size", Integer.class))
          .maintenanceFrequency(10, TimeUnit.MILLISECONDS)
          .maintenanceInitialDelay(11, TimeUnit.MILLISECONDS)
          .maxConnectionIdleTime(30, TimeUnit.SECONDS)
          .maxWaitTime(15, TimeUnit.MILLISECONDS);
    });
  }
}
```

### 10.4. `MongoTemplate` 简介

`MongoTemplate` 类位于 `org.springframework.data.mongodb.core` 包中，是 Spring 支持 MongoDB 的中心类，为与数据库交互提供了丰富的功能集。该 template 提供了创建、更新、删除和查询MongoDB文档的便利操作，并提供了你的domain对象和MongoDB文档之间的映射。

|      | 一旦配置好，`MongoTemplate` 是线程安全的，可以在多个实例中重复使用。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

MongoDB 文档和 domain 类之间的映射是通过委托给 `MongoConverter` 接口的实现完成的。Spring提供了 `MappingMongoConverter`，但你也可以编写自己的转换器。参见 “[自定义转换 - 覆盖默认映射](https://springdoc.cn/spring-data-mongodb/#mongo.custom-converters)” 以了解更多详细信息。

`MongoTemplate` 类实现了 `MongoOperations` 接口。在尽可能多的情况下， `MongoOperations` 上的方法是以 MongoDB 驱动 `Collection` 对象上的方法命名的，以使习惯于驱动API的现有 MongoDB 开发者熟悉该API。例如，你可以找到诸如 `find`、`findAndModify`、`findAndReplace`、`findOne`、`insert`、`remove`、`save`、`update` 和 `updateMulti` 等方法。设计目标是使其尽可能容易地在使用基本 MongoDB 驱动和 `MongoOperations` 之间过渡。这两个API之间的一个主要区别是，`MongoOperations` 可以被传递domain对象而不是 `Document` 。另外，`MongoOperations` 对 `Query`、`Criteria` 和 `Update` 操作有 fluent API，而不是通过填充 `Document` 来指定这些操作的参数。

|      | 引用 `MongoTemplate` 实例上的操作的首选方式是通过其接口 `MongoOperations`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

`MongoTemplate` 使用的默认转换器实现是 `MappingMongoConverter`。虽然 `MappingMongoConverter` 可以使用额外的元数据来指定对象到文档的映射，但它也可以通过使用一些约定来转换不包含额外元数据的对象，以实现ID和 collection 名称的映射。这些约定以及映射注解的使用在 “[映射（Mapping）](https://springdoc.cn/spring-data-mongodb/#mapping-chapter)” 章节中有解释。

`MongoTemplate` 的另一个核心功能是将MongoDB Java驱动抛出的异常转换为Spring的可移植数据访问异常层次结构。请参阅 “[异常（Exception）转换](https://springdoc.cn/spring-data-mongodb/#mongo.exception)” 以了解更多信息。

`MongoTemplate` 提供了许多方便的方法来帮助你轻松地执行常见的任务。然而，如果你需要直接访问MongoDB驱动API，你可以使用几个 `Execute` 回调方法中的一个。`execute` 回调给你一个对 `com.mongodb.client.MongoCollection` 或 `com.mongodb.client.MongoDatabase` 对象的引用。更多信息请参见 [" `Execution` 回调"](https://springdoc.cn/spring-data-mongodb/#mongo.executioncallback)部分。

下一节包含一个例子，说明如何在Spring容器的背景下使用 `MongoTemplate`。

#### 10.4.1. 实例化 `MongoTemplate`

你可以使用下面的配置来创建和注册 `MongoTemplate` 的实例，如下例所示。

Example 57. 注册 `com.mongodb.client.MongoClient` 对象并启用 Spring 的异常翻译支持

Java

XML

```java
@Configuration
class AppConfig {

  @Bean
  MongoClient mongoClient() {
      return MongoClients.create("mongodb://localhost:27017");
  }

  @Bean
  MongoTemplate mongoTemplate(MongoClient mongoClient) {
      return new MongoTemplate(mongoClient, "geospatial");
  }
}
```

`MongoTemplate` 有几个重载的构造函数。

- `MongoTemplate(MongoClient mongo, String databaseName)`: 接受 `MongoClient` 对象和默认的数据库名称来进行操作。
- `MongoTemplate(MongoDatabaseFactory mongoDbFactory)`: 接受一个 `MongoDbFactory` 对象，该对象封装了 `MongoClient` 对象、数据库名称以及用户名和密码。
- `MongoTemplate(MongoDatabaseFactory mongoDbFactory, MongoConverter mongoConverter)`: 添加一个用于映射的 `MongoConverter`。

在创建 `MongoTemplate` 时，你可能想设置的其他可选的属性是默认的 `WriteResultCheckingPolicy`、`WriteConcern` 和 `ReadPreference` 属性。

|      | 引用 `MongoTemplate` 实例上的操作的首选方式是通过其接口 `MongoOperations`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 10.4.2. `WriteResultChecking` 策略

在开发过程中，如果从任何 MongoDB 操作返回的 `com.mongodb.WriteResult` 包含一个错误，那么记录或抛出一个异常是很方便的。在开发过程中，忘记这样做是很常见的，然后最终得到一个看起来运行成功的应用程序，而事实上，数据库并没有按照你的预期进行修改。你可以将 `MongoTemplate` 的 `WriteResultChecking` 属性设置为以下值之一。`EXCEPTION` 或 `NONE`，分别用来抛出一个 `Exception` 或不做任何事情。默认是使用 `NONE` 的 `WriteResultChecking` 值。

#### 10.4.3. `WriteConcern`

如果尚未通过驱动程序在更高层次（如 `com.mongodb.client.MongoClient`）上指定，可以设置 `MongoTemplate` 用于写操作的 `com.mongodb.WriteConcern` 属性。如果没有设置 `WriteConcern` 属性，则默认为 MongoDB 驱动程序的 DB 或 Collection 设置中的设置。

#### 10.4.4. `WriteConcernResolver`

对于更高级的情况，即你想在每个操作的基础上设置不同的 `WriteConcern` 值（对于 remove, update, insert 和 save操作），可以在 `MongoTemplate` 上配置一个名为 `WriteConcernResolver` 的策略接口。由于 `MongoTemplate` 用于持久化POJO， `WriteConcernResolver` 让你创建一个策略，可以将特定的POJO类映射到 `WriteConcern` 值。下面的列表显示了 `WriteConcernResolver` 的接口。

```java
public interface WriteConcernResolver {
  WriteConcern resolve(MongoAction action);
}
```

你可以使用 `MongoAction` 参数来决定 `WriteConcern` 值，或者使用 Template 本身的值作为默认值。`MongoAction` 包含被写入的集合名称、POJO 的 `java.lang.Class`、转换后的 `Document`、操作（`REMOVE`、`UPDATE`、`INSERT`、`INSERT_LIST` 或 `SAVE`），以及其他一些上下文信息。下面的例子显示了两组类得到不同的 `WriteConcern` 设置。

```
private class MyAppWriteConcernResolver implements WriteConcernResolver {

  public WriteConcern resolve(MongoAction action) {
    if (action.getEntityClass().getSimpleName().contains("Audit")) {
      return WriteConcern.NONE;
    } else if (action.getEntityClass().getSimpleName().contains("Metadata")) {
      return WriteConcern.JOURNAL_SAFE;
    }
    return action.getDefaultWriteConcern();
  }
}
```

### 10.5. 保存、更新和删除文档

`MongoTemplate` 让你保存、更新和删除你的 domain 对象，并将这些对象映射到存储在MongoDB中的文档。

Consider the following class:

```java
public class Person {

  private String id;
  private String name;
  private int age;

  public Person(String name, int age) {
    this.name = name;
    this.age = age;
  }

  public String getId() {
    return id;
  }
  public String getName() {
    return name;
  }
  public int getAge() {
    return age;
  }

  @Override
  public String toString() {
    return "Person [id=" + id + ", name=" + name + ", age=" + age + "]";
  }

}
```

给出前面例子中的 `Person` 类，你可以保存、更新和删除该对象，如下例所示。

|      | `MongoOperations` 是 `MongoTemplate` 实现的接口。 |
| ---- | ------------------------------------------------- |
|      |                                                   |

```java
package org.spring.example;

public class MongoApp {

  private static final Log log = LogFactory.getLog(MongoApp.class);

  public static void main(String[] args) {

    MongoOperations mongoOps = new MongoTemplate(new SimpleMongoClientDbFactory(MongoClients.create(), "database"));

    Person p = new Person("Joe", 34);

    // Insert is used to initially store the object into the database.
    mongoOps.insert(p);
    log.info("Insert: " + p);

    // Find
    p = mongoOps.findById(p.getId(), Person.class);
    log.info("Found: " + p);

    // Update
    mongoOps.updateFirst(query(where("name").is("Joe")), update("age", 35), Person.class);
    p = mongoOps.findOne(query(where("name").is("Joe")), Person.class);
    log.info("Updated: " + p);

    // Delete
    mongoOps.remove(p);

    // Check that deletion worked
    List<Person> people =  mongoOps.findAll(Person.class);
    log.info("Number of people = : " + people.size());


    mongoOps.dropCollection(Person.class);
  }
}
```

前面的例子会产生以下日志输出（包括来自 `MongoTemplate` 的 debug 信息）。

```
DEBUG apping.MongoPersistentEntityIndexCreator:  80 - Analyzing class class org.spring.example.Person for index information.
DEBUG work.data.mongodb.core.MongoTemplate: 632 - insert Document containing fields: [_class, age, name] in collection: person
INFO               org.spring.example.MongoApp:  30 - Insert: Person [id=4ddc6e784ce5b1eba3ceaf5c, name=Joe, age=34]
DEBUG work.data.mongodb.core.MongoTemplate:1246 - findOne using query: { "_id" : { "$oid" : "4ddc6e784ce5b1eba3ceaf5c"}} in db.collection: database.person
INFO               org.spring.example.MongoApp:  34 - Found: Person [id=4ddc6e784ce5b1eba3ceaf5c, name=Joe, age=34]
DEBUG work.data.mongodb.core.MongoTemplate: 778 - calling update using query: { "name" : "Joe"} and update: { "$set" : { "age" : 35}} in collection: person
DEBUG work.data.mongodb.core.MongoTemplate:1246 - findOne using query: { "name" : "Joe"} in db.collection: database.person
INFO               org.spring.example.MongoApp:  39 - Updated: Person [id=4ddc6e784ce5b1eba3ceaf5c, name=Joe, age=35]
DEBUG work.data.mongodb.core.MongoTemplate: 823 - remove using query: { "id" : "4ddc6e784ce5b1eba3ceaf5c"} in collection: person
INFO               org.spring.example.MongoApp:  46 - Number of people = : 0
DEBUG work.data.mongodb.core.MongoTemplate: 376 - Dropped collection [database.person]
```

`MongoConverter` 通过识别（通过惯例）`Id` 属性名称，在 `String` 和存储在数据库中的 `ObjectId` 之间引起隐性转换。

|      | 前面的例子是为了展示 `MongoTemplate` 上保存、更新和删除操作的使用，而不是为了展示复杂的映射功能。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

前面的例子中使用的查询语法在 “[查询文档](https://springdoc.cn/spring-data-mongodb/#mongo.query)” 一节中有更详细的解释。

#### 10.5.1. 在映射层中如何处理 `_id` 字段

MongoDB要求你为所有的文档设置一个 `_id` 字段。如果你没有提供一个，驱动程序就会分配一个带有生成值的 `ObjectId`。当你使用 `MappingMongoConverter` 时，某些规则规定了如何将Java类中的属性映射到这个 `_id` 字段。

1. 用 `@Id`（`org.springframework.data.annotation.Id`）注解的属性或字段映射到 `_id` 字段。
2. 一个没有注解但名为 `id` 的属性或字段映射到 `_id` 字段。

下面概述了在使用 `MappingMongoConverter`（`MongoTemplate` 的默认值）时，对映射到 `_id` 文档字段的属性进行了哪些类型转换（如果有的话）。

1. 如果可能的话，通过使用Spring `Converter<String, ObjectId>`，将Java类中声明为 `String` 的 `id` 属性或字段转换为 `ObjectId` 并存储。有效的转换规则被委托给MongoDB Java驱动。如果它不能被转换为 `ObjectId`，那么该值将作为字符串存储在数据库中。
2. 通过使用Spring `Converter<BigInteger, ObjectId>`，在Java类中声明为 `BigInteger` 的 `id` 属性或字段被转换为 `ObjectId` 并存储。

如果Java类中没有前几组规则中指定的字段或属性，驱动程序会生成一个隐含的 `_id` 文档，但不会映射到Java类的某个属性或字段。

在查询和更新时，`MongoTemplate` 使用与前面保存文档的规则相对应的转换器（converter），以便在查询中使用的字段名和类型能够与你的domain类中的内容相匹配。

有些环境需要使用自定义的方法来映射 `Id` 值，例如存储在 MongoDB 中的数据没有经过Spring Data映射层。文档可以包含 `_id` 值，可以用 `ObjectId` 或 `String` 表示。从存储区读回 domain 类型的文档工作得很好。由于隐含的 `ObjectId` 转换，通过其 `id` 查询文档会很麻烦。因此不能以这种方式检索文档。对于这些情况，`@MongoId` 提供了对实际 `id` 映射尝试的更多控制。

Example 58. `@MongoId` 映射

```java
public class PlainStringId {
  @MongoId String id; 
}

public class PlainObjectId {
  @MongoId ObjectId id; 
}

public class StringToObjectId {
  @MongoId(FieldType.OBJECT_ID) String id; 
}
```

|      | id 被视为 `String`，无需进一步转换。                         |
| ---- | ------------------------------------------------------------ |
|      | id 被视为 `ObjectId'。                                       |
|      | 如果给定的 `String` 是一个有效的 `ObjectId` 十六进制，id将被视为 `ObjectId`，否则视为 `String`。对应于 `@Id` 的用法。 |

#### 10.5.2. 类型映射

MongoDB 集合（collection）可以包含代表各种类型实例的文档。如果你存储了一个类的层次结构，或者有一个具有 `Object` 类型属性的类，那么这个特性就很有用。在后一种情况下，当检索对象时，必须正确地读入该属性内的值。

为了实现这一点，`MappingMongoConverter` 使用一个 `MongoTypeMapper` 抽象，以 `DefaultMongoTypeMapper` 作为其主要实现。它的默认行为是全路径的 `classname` 存储在文档内的 `_class` 下。类型提示是为顶级文档以及每个值（如果它是一个复杂类型和声明的属性类型的子类型）编写的。

Example 59. 类型映射

```java
class Sample {
  Contact value;
}

abstract class Contact { … }

class Person extends Contact { … }

Sample sample = new Sample();
sample.value = new Person();

mongoTemplate.save(sample);

{
  "value" : { "_class" : "com.acme.Person" },
  "_class" : "com.acme.Sample"
}
```

Spring Data MongoDB 将类型信息作为实际 root 类以及嵌套类型的最后一个字段来存储（因为它是复杂的，是 `Contact` 的一个子类型）。因此，如果你现在使用 `mongoTemplate.findAll(Object.class, "sample")`，你可以发现存储的文档是一个 `Sample` 实例。你还可以发现值属性实际上是一个 `Person`。

##### 自定义类型映射

如果你想避免把整个Java类的名字写成类型信息，而是想使用一个key，你可以在实体类上使用 `@TypeAlias` 注解。如果你需要进一步定制映射，可以看看 `TypeInformationMapper` 接口。该接口的实例可以在 `DefaultMongoTypeMapper` 上配置，反过来，可以在 `MappingMongoConverter` 上配置。

Example 60. 为一个实体定义一个类型别名

```java
@TypeAlias("pers")
class Person {

}
```

请注意，所产生的文档中包含 `pers` 作为 `_class` 字段的值。

|      | 类型别名只有在映射上下文知道实际类型的情况下才起作用。所需的实体元数据要么在第一次保存时确定，要么必须通过配置的初始实体集提供。默认情况下，配置类会扫描 base package 以寻找潜在的候选者。`@Configuration class AppConfig extends AbstractMongoClientConfiguration {   @Override  protected Set<Class<?>> getInitialEntitySet() {    return Collections.singleton(Person.class);  }   // ... } ` |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 配置自定义类型映射

下面的例子显示了如何在 `MappingMongoConverter` 中配置一个自定义的 `MongoTypeMapper`。

```java
class CustomMongoTypeMapper extends DefaultMongoTypeMapper {
  //implement custom type mapping here
}
```

Example 61. 配置自定义的 `MongoTypeMapper`

Java

XML

```java
@Configuration
class SampleMongoConfiguration extends AbstractMongoClientConfiguration {

  @Override
  protected String getDatabaseName() {
    return "database";
  }

  @Bean
  @Override
  public MappingMongoConverter mappingMongoConverter(MongoDatabaseFactory databaseFactory,
			MongoCustomConversions customConversions, MongoMappingContext mappingContext) {
    MappingMongoConverter mmc = super.mappingMongoConverter();
    mmc.setTypeMapper(customTypeMapper());
    return mmc;
  }

  @Bean
  public MongoTypeMapper customTypeMapper() {
    return new CustomMongoTypeMapper();
  }
}
```

请注意，前面的例子扩展了 `AbstractMongoClientConfiguration` 类，并覆写了 `MappingMongoConverter` 的 bean 定义，我们在那里配置了我们的自定义 `MongoTypeMapper`。

#### 10.5.3. 保存和插入文档的方法

`MongoTemplate` 上有几个方便的方法，用于保存和插入你的对象。为了对转换过程有更精细的控制，你可以用 `MappingMongoConverter` 注册 Spring Converter — 例如 `Converter<Person, Document>` 和 `Converter<Document, Person>`。

|      | 插入（insert）和保存（save）操作之间的区别是，如果对象还没有持久化，保存操作会执行插入操作。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

使用保存操作的简单情况是保存一个POJO。在这种情况下，集合名称是由类的名称（非完全限定）决定的。你也可以用一个特定的集合名称来调用保存操作。你可以使用映射元数据来重写存储对象的集合。

当插入或保存时，如果 `Id` 属性没有被设置，假设它的值将由数据库自动生成。因此，为了成功地自动生成一个 `ObjectId`，你的类中的 `Id` 属性或字段的类型必须是一个 `String`，一个 `ObjectId`，或一个 `BigInteger`。

下面的例子显示了如何保存一个文档和检索其内容。

Example 62. 使用 MongoTemplate 插入和检索文档

```java
…

Person p = new Person("Bob", 33);
mongoTemplate.insert(p);

Person qp = mongoTemplate.findOne(query(where("age").is(33)), Person.class);
```

可以进行以下插入和保存操作。

- `void` **save** `(Object objectToSave)`: 将该对象保存到默认的集合中。
- `void` **save** `(Object objectToSave, String collectionName)`: 将该对象保存到指定的集合中。

也有一套类似的插入操作。

- `void` **insert** `(Object objectToSave)`: 将该对象插入到默认集合中。
- `void` **insert** `(Object objectToSave, String collectionName)`: 插入对象到指定的集合。

##### 我的文档被保存在哪个集合中？

有两种方法来管理用于文档的集合名称。默认使用的集合名是将类名改为以小写字母开头。所以一个 `com.test.Person` 类被存储在 `person` 集合中。你可以通过使用 `@Document` 注解提供一个不同的集合名称来定制它。你也可以通过提供你自己的集合名称作为选定的 `MongoTemplate` 方法调用的最后一个参数来覆盖该集合名称。

##### 插入或保存单个对象

MongoDB 驱动支持在单个操作中插入一个文档集合。`MongoOperations` 接口中的下列方法支持这一功能。

- **insert**: 插入一个对象。如果有一个具有相同 `id` 的现有文档，就会产生一个错误。
- **insertAll**: 接受一个 `Collection` 的集合作为第一个参数。该方法检查每个对象，并根据前面指定的规则将其插入到适当的集合中。
- **save**: 保存对象，覆盖任何可能有相同 `id` 的对象。

##### 批量插入对象

MongoDB驱动支持在一个操作中插入一个文档集合。`MongoOperations` 接口中的下列方法支持这一功能。

- **insert** 方法: 以一个 `Collection` 作为第一个参数。它们在一次批量写入数据库中插入一个对象的列表。

#### 10.5.4. 更新集合中的文档

对于更新，你可以通过使用 `MongoOperation.updateFirst` 来更新找到的第一个文档，或者你可以通过使用 `MongoOperation.updateMulti` 方法来更新所有找到的符合查询的文档。下面的例子显示了对所有 `SAVINGS` 账户的更新，我们通过使用 `$inc` 操作符在余额中添加一次性的50美元奖金。

Example 63. 通过使用 `MongoTemplate` 更新文档

```java
...

WriteResult wr = mongoTemplate.updateMulti(new Query(where("accounts.accountType").is(Account.Type.SAVINGS)),
  new Update().inc("accounts.$.balance", 50.00), Account.class);
```

除了前面讨论的 `Query` 之外，我们还通过使用 `Update` 对象来提供更新定义。 `Update` 类拥有与 MongoDB 可用的更新修改器（update modifier）相匹配的方法。 大多数方法都返回 `Update` 对象，为 API 提供一种 fluent 的风格。

##### 运行更新文档的方法

- **updateFirst**: 用更新后的文档更新第一个符合查询文档 criteria 的文档。
- **updateMulti**: 用更新后的文档更新所有符合查询文档 criteria 的对象。

|      | `updateFirst` 不支持排序。请使用 [findAndModify](https://springdoc.cn/spring-data-mongodb/#mongo-template.find-and-upsert) 来应用 `Sort`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### `Update` 类中的方法

你可以在 `Update` 类中使用一点 "'语法糖'"，因为它的方法是要被串联起来的。另外，你可以通过使用 `public static Update update(String key, Object value)` 和使用静态导入来启动一个新的 `Update` 实例的创建。

`Update` 类包含以下方法。

- `Update` **addToSet** `(String key, Object value)` 使用 `$addToSet` 更新修改器（update modifier）进行更新
- `Update` **currentDate** `(String key)` 使用 `$currentDate` 更新修改器进行更新
- `Update` **currentTimestamp** `(String key)` 使用 `$currentDate` 更新修改器与 `$type` `timestamp` 进行更新
- `Update` **inc** `(String key, Number inc)` 使用 `$inc` 更新修改器进行更新
- `Update` **max** `(String key, Object max)` 使用 `$max` 更新修改器进行更新
- `Update` **min** `(String key, Object min)` 使用 `$min` 更新修改器进行更新
- `Update` **multiply** `(String key, Number multiplier)` 使用 `$mul` 更新修改器进行更新
- `Update` **pop** `(String key, Update.Position pos)` 使用 `$pop` 更新修改器进行更新
- `Update` **pull** `(String key, Object value)` 使用 `$pull` 更新修改器进行更新
- `Update` **pullAll** `(String key, Object[] values)` 使用 `$pullAll` 更新修改器进行更新
- `Update` **push** `(String key, Object value)` 使用 `$push` 更新修改器进行更新
- `Update` **pushAll** `(String key, Object[] values)` 使用 `$pushAll` 更新修改器进行更新
- `Update` **rename** `(String oldName, String newName)` 使用 `$rename` 更新修改器进行更新
- `Update` **set** `(String key, Object value)` 使用 `$set` 更新修改器进行更新
- `Update` **setOnInsert** `(String key, Object value)` 使用 `$setOnInsert` 更新修改器进行更新
- `Update` **unset** `(String key)` 使用 `$unset` 更新修改器进行更新

一些更新修改器，如 `$push` 和 `$addToSet`，允许嵌套额外的操作符。

```
// { $push : { "category" : { "$each" : [ "spring" , "data" ] } } }
new Update().push("category").each("spring", "data")

// { $push : { "key" : { "$position" : 0 , "$each" : [ "Arya" , "Arry" , "Weasel" ] } } }
new Update().push("key").atPosition(Position.FIRST).each(Arrays.asList("Arya", "Arry", "Weasel"));

// { $push : { "key" : { "$slice" : 5 , "$each" : [ "Arya" , "Arry" , "Weasel" ] } } }
new Update().push("key").slice(5).each(Arrays.asList("Arya", "Arry", "Weasel"));

// { $addToSet : { "values" : { "$each" : [ "spring" , "data" , "mongodb" ] } } }
new Update().addToSet("values").each("spring", "data", "mongodb");
```

#### 10.5.5. 在一个集合中 “Upsert” 文档

与执行 `updateFirst` 操作相关，你也可以执行 “upsert” 操作，如果没有找到与查询相匹配的文档，它将执行插入操作。被插入的文档是查询文档和更新文档的组合。下面的例子显示了如何使用 `upsert` 方法。

```
template.update(Person.class)
  .matching(query(where("ssn").is(1111).and("firstName").is("Joe").and("Fraizer").is("Update"))
  .apply(update("address", addr))
  .upsert();
```

|      | `upsert` 不支持排序。请使用 [findAndModify](https://springdoc.cn/spring-data-mongodb/#mongo-template.find-and-upsert) 来应用 `Sort`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 10.5.6. 查和 Upsert 集合中的文档

`MongoCollection` 上的 `findAndModify(…)` 方法可以更新一个文档，并在一次操作中返回旧的或新更新的文档。`MongoTemplate` 提供了四个 `findAndModify` 重载方法，这些方法采用 `Query` 和 `Update` 类，并从 `Document` 转换为你的POJO。

```java
<T> T findAndModify(Query query, Update update, Class<T> entityClass);

<T> T findAndModify(Query query, Update update, Class<T> entityClass, String collectionName);

<T> T findAndModify(Query query, Update update, FindAndModifyOptions options, Class<T> entityClass);

<T> T findAndModify(Query query, Update update, FindAndModifyOptions options, Class<T> entityClass, String collectionName);
```

下面的例子在容器中插入了几个 `Person` 对象，并执行了 `findAndUpdate` 操作。

```java
template.insert(new Person("Tom", 21));
template.insert(new Person("Dick", 22));
template.insert(new Person("Harry", 23));

Query query = new Query(Criteria.where("firstName").is("Harry"));
Update update = new Update().inc("age", 1);

Person oldValue = template.update(Person.class)
  .matching(query)
  .apply(update)
  .findAndModifyValue(); // return's old person object

assertThat(oldValue.getFirstName()).isEqualTo("Harry");
assertThat(oldValue.getAge()).isEqualTo(23);

Person newValue = template.query(Person.class)
  .matching(query)
  .findOneValue();

assertThat(newValue.getAge()).isEqualTo(24);

Person newestValue = template.update(Person.class)
  .matching(query)
  .apply(update)
  .withOptions(FindAndModifyOptions.options().returnNew(true)) // Now return the newly updated document when updating
  .findAndModifyValue();

assertThat(newestValue.getAge()).isEqualTo(25);
```

`FindAndModifyOptions` 方法让你设置 `returnNew`、`upsert` 和 `remove` 的选项。下面是一个从前面的代码片断延伸出来的例子。

```java
Person upserted = template.update(Person.class)
  .matching(new Query(Criteria.where("firstName").is("Mary")))
  .apply(update)
  .withOptions(FindAndModifyOptions.options().upsert(true).returnNew(true))
  .findAndModifyValue()

assertThat(upserted.getFirstName()).isEqualTo("Mary");
assertThat(upserted.getAge()).isOne();
```

#### 10.5.7. 聚合管道（Aggregation Pipeline）更新

`MongoOperations` 和 `ReactiveMongoOperations` 暴露的更新方法也通过 `AggregationUpdate` 接受一个 [Aggregation Pipeline（聚合管道）](https://springdoc.cn/spring-data-mongodb/#mongo.aggregation)。使用 `AggregationUpdate` 可以在更新操作中利用 MongoDB 4.2 聚合。在更新中使用聚合允许通过用单个操作表达多个阶段和多个条件来更新一个或多个字段。

更新可以包括以下几个阶段。

- `AggregationUpdate.set(…).toValue(…)` → `$set : { … }`
- `AggregationUpdate.unset(…)` → `$unset : [ … ]`
- `AggregationUpdate.replaceWith(…)` → `$replaceWith : { … }`

Example 64. Update Aggregation

```java
AggregationUpdate update = Aggregation.newUpdate()
    .set("average").toValue(ArithmeticOperators.valueOf("tests").avg())     
    .set("grade").toValue(ConditionalOperators.switchCases(                 
        when(valueOf("average").greaterThanEqualToValue(90)).then("A"),
        when(valueOf("average").greaterThanEqualToValue(80)).then("B"),
        when(valueOf("average").greaterThanEqualToValue(70)).then("C"),
        when(valueOf("average").greaterThanEqualToValue(60)).then("D"))
        .defaultTo("F")
    );

template.update(Student.class)                                              
    .apply(update)
    .all();                                                                 
db.students.update(                                                         
   { },
   [
     { $set: { average : { $avg: "$tests" } } },                            
     { $set: { grade: { $switch: {                                          
                           branches: [
                               { case: { $gte: [ "$average", 90 ] }, then: "A" },
                               { case: { $gte: [ "$average", 80 ] }, then: "B" },
                               { case: { $gte: [ "$average", 70 ] }, then: "C" },
                               { case: { $gte: [ "$average", 60 ] }, then: "D" }
                           ],
                           default: "F"
     } } } }
   ],
   { multi: true }                                                          
)
```

|      | 第1个 `$set` 阶段根据 *tests* 字段的平均值（average）计算出一个新的字段 *average*。 |
| ---- | ------------------------------------------------------------ |
|      | 第2个 `$set` 阶段根据第一聚合阶段计算的 *average* 字段，计算新的字段 *grade*。 |
|      | 该管道在 *students* 集合上运行，并使用 `Student` 作为聚合字段的映射。 |
|      | 将更新应用于集合中所有匹配的文档。                           |

#### 10.5.8. 查和替换文档

替换整个 `Document` 的最直接的方法是通过其 `id` 使用 `save` 方法。 `findAndReplace` 提供了一个替代方法，允许通过一个简单的查询来确定要替换的文档。

Example 65. 查找和替换文档

```java
Optional<User> result = template.update(Person.class)      
    .matching(query(where("firstame").is("Tom")))          
    .replaceWith(new Person("Dick"))
    .withOptions(FindAndReplaceOptions.options().upsert()) 
    .as(User.class)                                        
    .findAndReplace();                                     
```

|      | 使用给定的 domain 类型的fluent更新API来映射查询并推导出集合名称，或者直接使用 `MongoOperations#findAndReplace`。 |
| ---- | ------------------------------------------------------------ |
|      | 针对给定的 domain 类型映射的实际匹配查询。通过查询提供 `sort`、`fields` 和 `collation` 设置。 |
|      | 额外的可选 hook，提供除默认值以外的选项，如 `upsert`。       |
|      | 用于映射操作结果的可选投影类型。如果没有，则使用初始 domain 类型。 |
|      | 触发实际处理。使用 `findAndReplaceValue` 来获得可能为 null 的结果，而不是一个 `Optional`。 |

|      | 请注意，替换的文档本身不能持有一个 `id`，因为现有的 `Document` 的 `id` 会被store本身带入替换的文档中。另外请记住，`findAndReplace` 只会根据可能给定的排序顺序，替换符合查询条件的第一个文档。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 10.5.9. 删除文档的方法

你可以使用五个重载方法中的一个来从数据库中删除一个对象。

```java
template.remove(tywin, "GOT");                                              

template.remove(query(where("lastname").is("lannister")), "GOT");           

template.remove(new Query().limit(3), "GOT");                               

template.findAllAndRemove(query(where("lastname").is("lannister"), "GOT");  

template.findAllAndRemove(new Query().limit(3), "GOT");                     
```

|      | 从关联的集合中移除由其 `_id` 指定的单个实体。                |
| ---- | ------------------------------------------------------------ |
|      | 从 `GOT` 集合中删除所有符合查询条件的文文档。                |
|      | 删除 `GOT` 集合中的前三个文档。与 <2> 不同的是，要删除的文档是通过它们的 `_id` 来识别的，运行给定的查询，先应用 `sort`、`limit` 和 `skip` 选项，然后在一个单独的步骤中一次性删除所有的文档。 |
|      | 从 `GOT` 集合中删除所有符合查询条件的文档。与<3>不同，文档不会被批量删除，而是一个一个地删除。 |
|      | 删除 `GOT` 集合中的前三个文档。与 <3> 不同，文档不会被批量删除，而是一个一个地删除。 |

#### 10.5.10. 乐观锁

`@Version` 注解提供了类似于 JPA 在 MongoDB 上下文中的语法，并确保更新只应用于具有匹配 version 的文档。因此，version 属性的实际值被添加到更新查询中，如果在此期间另一个操作改变了文档，那么更新就不会有任何影响。在这种情况下，会抛出一个 `OptimisticLockingFailureException`。下面的例子显示了这些特征。

```java
@Document
class Person {

  @Id String id;
  String firstname;
  String lastname;
  @Version Long version;
}

Person daenerys = template.insert(new Person("Daenerys"));                            

Person tmp = template.findOne(query(where("id").is(daenerys.getId())), Person.class); 

daenerys.setLastname("Targaryen");
template.save(daenerys);                                                              

template.save(tmp); // throws OptimisticLockingFailureException                       
```

|      | 部分插入文档。 `version` 设置为 `0`。                        |
| ---- | ------------------------------------------------------------ |
|      | 加载刚插入的文档。`version` 仍为 `0`。                       |
|      | 用 `version = 0` 来更新文件。设置 `lastname` 并将 `version` 自增到 `1`。 |
|      | 尝试更新先前加载的文件，该文件仍然是 `version = 0`。由于当前的 `version` 是 `1`，操作失败，出现了 `OptimisticLockingFailureException`。 |

乐观锁要求将 `WriteConcern` 设置为 `ACKNOWLEDGED`。否则， `OptimisticLockingFailureException` 会被吞掉。

|      | 从 `2.2` 版开始，`MongoOperations` 在从数据库中删除实体时也包括 `@Version` 属性。要删除一个没有版本检查的 `Document`，请使用 `MongoOperations#remove(Query,…)` 而不是 `MongoOperations#remove(Object)`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 从 2.2 版本开始，repository 在删除有 version 实体时检查确认的删除结果。如果一个有 version 的实体不能通过 `CrudRepository.delete(Object)` 被删除，就会引发一个 `OptimisticLockingFailureException`。在这种情况下，version 被改变了，或者该对象在这期间被删除了。使用 `CrudRepository.deleteById(ID)` 可以绕过乐观锁特性，删除对象而不考虑其版本。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 10.6. 查询文档

你可以使用 `Query` 和 `Criteria` 类来表达你的查询。它们的方法名称反映了MongoDB的本地操作符名称，如 `lt`、`lte`、`is` 等。`Query` 和 `Criteria` 类遵循 fluent API风格，因此你可以将多个 criteria 和 query 方法连在一起，同时拥有易于理解的代码。 为了提高可读性，静态导入让你避免使用 'new' 关键字来创建 `Query` 和 `Criteria` 实例。你也可以使用 `BasicQuery` 来从普通的 JSON 字符串中创建 `Query` 实例，如下例所示。

Example 66. 从一个普通的JSON字符串创建一个 Query 实例

```java
BasicQuery query = new BasicQuery("{ age : { $lt : 50 }, accounts.balance : { $gt : 1000.00 }}");
List<Person> result = mongoTemplate.find(query, Person.class);
```

Spring MongoDB 还支持 GeoSpatial 查询（见 [GeoSpatial查询](https://springdoc.cn/spring-data-mongodb/#mongo.geospatial)部分）和 Map-Reduce 操作（见 [Map-Reduce](https://springdoc.cn/spring-data-mongodb/#mongo.mapreduce) 部分）。

#### 10.6.1. 查询集合中的文档

在前面，我们看到了如何通过使用 `MongoTemplate` 上的 `findOne` 和 `findById` 方法来检索单个文档。这些方法返回一个单一的 domain 对象。我们也可以查询一个文档的集合，将其作为 domain 对象的列表返回。假设我们有一些带有 name 和 age 的 `Person` 对象作为文档存储在一个集合中，并且每个 person 都有一个带有 balance 的嵌入的 account 文档，我们现在可以使用下面的代码运行一个查询。

Example 67. 使用 `MongoTemplate` 查询文档

```java
// ...

List<Person> result = template.query(Person.class)
  .matching(query(where("age").lt(50).and("accounts.balance").gt(1000.00d)))
  .all();
```

所有的查找方法都需要一个 `Query` 对象作为参数。这个对象定义了用于执行查询的 criteria 和选项。criteria 是通过使用一个 `Criteria` 对象来指定的，该对象有一个静态工厂方法，名为 `where`，用来实例化一个新的 `Criteria` 对象。我们推荐使用 `org.springframework.data.mongodb.core.query.Criteria.where` 和 `Query.query` 的静态导入，使 query 更易读。

该查询应该返回一个符合指定条件（criteria）的 `Person` 对象的列表。本节的其余部分列出了 `Criteria` 和 `Query` 类的方法，这些方法对应于 MongoDB 中提供的操作符。大多数方法都返回 `Criteria` 对象，以便为API提供一种 fluent 的风格。

##### Criteria 类的方法

`Criteria` 类提供了以下方法，它们都对应于MongoDB中的操作符。

- `Criteria` **all** `(Object o)` 使用 `$all` 操作符创建一个 criterion。
- `Criteria` **and** `(String key)` 在当前的 `Criteria` 中添加一个带有指定 `key` 的 `Criteria` 调用链 ，并返回新创建的 `Criteria` 。
- `Criteria` **andOperator** `(Criteria… criteria)` 使用 `$and` 操作符为所有提供的 criteria 创建一个 and 查询（需要MongoDB 2.0 或更高版本）。
- `Criteria` **andOperator** `(Collection<Criteria> criteria)` 使用 `$and` 操作符为所有提供的 criteria 创建一个 and 查询（需要MongoDB 2.0 或更高版本）。
- `Criteria` **elemMatch** `(Criteria c)` 使用 `$elemMatch` 操作符创建一个 criteria。
- `Criteria` **exists** `(boolean b)` 使用 `$exists` 操作符创建一个 criteria
- `Criteria` **gt** `(Object o)` 使用 `$gt` 操作符创建一个 criteria
- `Criteria` **gte** `(Object o)` 使用 `$gte` 操作符创建一个 criteria
- `Criteria` **in** `(Object… o)` 使用 `$in` 操作符为可边长参数创建一个 criteria。
- `Criteria` **in** `(Collection<?> collection)` 使用 `$in` 操作符创建一个 criteria，使用一个集合。
- `Criteria` **is** `(Object o)` 使用字段匹配（`{ key:value }`）创建一个 criteria。如果指定的值是一个文档，那么字段的顺序和在文档中的完全相等就很重要。
- `Criteria` **lt** `(Object o)` 使用 `$lt` 操作符创建一个 criteria。
- `Criteria` **lte** `(Object o)` 使用 `$lte` 操作符创建一个 criteria。
- `Criteria` **mod** `(Number value, Number remainder)` 使用 `$mod` 操作符创建一个 criteria。
- `Criteria` **ne** `(Object o)` 使用 `$ne` 操作符创建一个 criteria。
- `Criteria` **nin** `(Object… o)` 使用 `$nin` 操作符创建一个 criteria。
- `Criteria` **norOperator** `(Criteria… criteria)` 使用 `$nor` 操作符对所有提供的 criteria 创建一个nor查询。
- `Criteria` **norOperator** `(Collection<Criteria> criteria)` 使用 `$nor` 操作符对所有提供的 criteria 创建一个nor查询。
- `Criteria` **not** `()` 使用 `$not` 元操作符创建一个criteria，该 criteria 会影响到后面直接的子句。
- `Criteria` **orOperator** `(Criteria… criteria)` 使用 `$or` 操作符为所有提供的 criteria 创建一个 or 查询。
- `Criteria` **orOperator** `(Collection<Criteria> criteria)` 使用 `$or` 操作符为所有提供的 criteria 创建一个 or 查询。
- `Criteria` **regex** `(String re)` 使用 `$regex` 操作符创建一个 criteria。
- `Criteria` **sampleRate** `(double sampleRate)` 使用 `$sampleRate` 操作符创建一个 criteria。
- `Criteria` **size** `(int s)` 使用 `$size` 操作符创建一个 criteria。
- `Criteria` **type** `(int t)` 使用 `$type` 操作符创建一个 criteria。
- `Criteria` **matchingDocumentStructure** `(MongoJsonSchema schema)` 使用 `$jsonSchema` 操作符为 [JSON schema criteria](https://springdoc.cn/spring-data-mongodb/#mongo.jsonSchema) 创建一个 criteria。 `$jsonSchema` 只能应用于查询的顶层，而不是特定的属性。使用 schema 的 `properties` 属性来匹配嵌套字段。
- `Criteria` **bits()** 是通往 [MongoDB bit 数查询操作符](https://docs.mongodb.com/manual/reference/operator/query-bitwise/)（如 `$bitsAllClear`）的 gateway。

Criteria 类还为地理空间（geospatial）查询提供了以下方法（见 [地理空间查询](https://springdoc.cn/spring-data-mongodb/#mongo.geospatial)一节，可以看到它们的实际应用）。

- `Criteria` **within** `(Circle circle)` 使用 `$geoWithin $center` 运算符创建一个地理空间 criteria。
- `Criteria` **within** `(Box box)` 使用 `$geoWithin $box` 运算符创建一个地理空间 criteria。
- `Criteria` **withinSphere** `(Circle circle)` 使用 `$geoWithin $center` 运算符创建一个地理空间 criteria。
- `Criteria` **near** `(Point point)` 使用 `$near` 运算符创建一个地理空间 criteria。
- `Criteria` **nearSphere** `(Point point)` 使用 `$nearSphere$center` 运算符创建一个地理空间criteria。这仅适用于MongoDB 1.7 及以上版本。
- `Criteria` **minDistance** `(double minDistance)` 使用 `$minDistance` 运算符创建一个地理空间criteria，供 `$near` 使用。
- `Criteria` **maxDistance** `(double maxDistance)` 使用 `$maxDistance` 运算符创建一个地理空间 criteria，供 `$near` 使用。

##### Query 类的方法

`Query` 类有一些额外的方法，为查询提供选项。

- `Query` **addCriteria** `(Criteria criteria)` 用来为query添加额外的criteria。
- `Field` **fields** `()` 用于定义要包含在query结果中的字段。
- `Query` **limit** `(int limit)` 用于将返回结果的大小限制在所提供的限度内（用于分页）。
- `Query` **skip** `(int skip)` 用来跳过结果中所提供的文档数量（用于分页）。
- `Query` **with** `(Sort sort)` 用来为结果提供排序定义。

##### 选择字段

MongoDB 支持 [对查询返回的字段进行投影](https://docs.mongodb.com/manual/tutorial/project-fields-from-query-results/)。一个投影可以根据字段的名称包括和排除字段（除非明确排除，否则 `_id` 字段总是包括在内）。

Example 68. 选择结果字段

```java
public class Person {

    @Id String id;
    String firstname;

    @Field("last_name")
    String lastname;

    Address address;
}

query.fields().include("lastname");              

query.fields().exclude("id").include("lastname") 

query.fields().include("address")                

query.fields().include("address.city")           
```

|      | 结果将同时包含 `_id` 和 `last_name`，`{ "last_name" : 1 }`。 |
| ---- | ------------------------------------------------------------ |
|      | 结果只包含 `last_name`，`{ "_id" : 0, "last_name" : 1 }`。   |
|      | 结果将包含 `_id` 和整个 `address` 对象，`{ "address" : 1 }`。 |
|      | 结果将包含 `_id` 和 `address` 对象，该对象只包含 `city` 字段， `{ "address.city" : 1 }`. |

从MongoDB 4.4 开始，你可以使用聚合表达式进行字段投影，如下所示。

Example 69. 使用表达式计算结果字段

```java
query.fields()
  .project(MongoExpression.create("'$toUpper' : '$last_name'"))         
  .as("last_name");                                                     

query.fields()
  .project(StringOperators.valueOf("lastname").toUpper())               
  .as("last_name");

query.fields()
  .project(AggregationSpELExpression.expressionOf("toUpper(lastname)")) 
  .as("last_name");
```

|      | 使用一个原生表达式。使用的字段名必须是指数据库文档中的字段名。 |
| ---- | ------------------------------------------------------------ |
|      | 指定表达式结果被投射到的字段名。由此产生的字段名不会针对domain model进行映射。 |
|      | 使用 `AggregationExpression`。除了原生的 `MongoExpression` 外，字段名被映射到 domain 模型中使用的字段名。 |
|      | 使用 SpEL 和 `AggregationExpression` 来调用表达式函数。字段名被映射到domain模型中使用的字段名。 |

`@Query(fields="…")` 允许在 `Repository` 级别使用表达式字段投影，如 [MongoDB基于JSON的查询方法和字段限制](https://springdoc.cn/spring-data-mongodb/#mongodb.repositories.queries.json-based) 中所述。

#### 10.6.2. 文档查询方法

这些查询方法需要指定返回的目标类型 `T`，而且它们被重载了，有一个明确的集合名称，用于查询应该操作的集合，而不是返回类型所指示的那个。下面的查询方法让你找到一个或多个文档。

- **findAll**: 从集合中查询类型为 `T` 的对象列表。
- **findOne**: 将集合上的临时查询结果映射到一个指定类型的对象的单个实例。
- **findById**: 返回一个具有给定ID和目标类的对象。
- **find**: 将集合上的临时查询结果映射到指定类型的 `List`。
- **findAndRemove**:将集合上的临时查询结果映射到一个指定类型的对象的单个实例。匹配查询的第一个文档被返回并从数据库中的集合中删除。

#### 10.6.3. 查询不同（Distinct ）的值

MongoDB 提供了一个操作，通过使用从结果文档中的查询来获取单个字段的不同值。结果值不需要具有相同的数据类型，该功能也不限于简单类型。对于检索来说，实际的结果类型对于 conversion 和 typing 来说的确很重要。下面的例子显示了如何查询不同的值。

Example 70. 检索不同的值

```java
template.query(Person.class)  
  .distinct("lastname")       
  .all();                     
```

|      | 查询 `Person` 集合。                                         |
| ---- | ------------------------------------------------------------ |
|      | 选择 `lastname` 字段的不同值。字段名根据 domain 类型属性声明进行映射，考虑到潜在的 `@Field` 注解。 |
|      | 以 `Object` 的 `List` 形式检索所有不同的值（由于没有明确指定结果类型）。 |

检索不同的值到 `Object` 的集合中是最灵活的方式，因为它试图确定 domain 类型的属性值，并将结果转换为所需的类型或映射 `Document` 结构。

有时，当所需字段的所有值都固定为某一类型时，直接获得一个正确类型的 `Collection` 更为方便，如下例所示。

Example 71. 检索强类型的不同值

```java
template.query(Person.class)  
  .distinct("lastname")       
  .as(String.class)           
  .all();                     
```

|      | 查询 `Person` 的集合。                                       |
| ---- | ------------------------------------------------------------ |
|      | 选择 `lastname` 字段的不同值。字段名根据domain类型属性声明进行映射，考虑到潜在的 `@Field` 注解。 |
|      | 检索到的值被转换为所需的目标类型—在本例中是 `String`。如果存储的字段包含一个文档，也有可能将值映射到一个更复杂的类型。 |
|      | 检索所有不同的值，作为一个 `List<String>` 。如果该类型不能被转换为所需的目标类型，该方法会抛出一个 `DataAccessException`。 |

#### 10.6.4. 地理空间（GeoSpatial）查询

MongoDB通过使用 `$near`、`$within`、`geoWithin` 和 `$nearSphere` 等操作符支持地理空间查询。地理空间查询的特定方法在 `Criteria` 类中可用。还有一些形状类（`Box`、`Circle` 和 `Point`）可以与地理空间相关的 `Criteria` 方法一起使用。

|      | 在 MongoDB 事务中使用 GeoSpatial 查询时需要注意，见 [事务内部的特殊行为](https://springdoc.cn/spring-data-mongodb/#mongo.transactions.behavior)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

为了了解如何进行 `GeoSpatial` 查询，请考虑以下 `Venue` 类（取自集成测试，并依赖于丰富的 `MappingMongoConverter`）。

```java
@Document(collection="newyork")
public class Venue {

  @Id
  private String id;
  private String name;
  private double[] location;

  @PersistenceConstructor
  Venue(String name, double[] location) {
    super();
    this.name = name;
    this.location = location;
  }

  public Venue(String name, double x, double y) {
    super();
    this.name = name;
    this.location = new double[] { x, y };
  }

  public String getName() {
    return name;
  }

  public double[] getLocation() {
    return location;
  }

  @Override
  public String toString() {
    return "Venue [id=" + id + ", name=" + name + ", location="
        + Arrays.toString(location) + "]";
  }
}
```

要找到一个 `Circle` 内的地点，你可以使用以下查询。

```java
Circle circle = new Circle(-73.99171, 40.738868, 0.01);
List<Venue> venues =
    template.find(new Query(Criteria.where("location").within(circle)), Venue.class);
```

要想用球面坐标找到 `Circle` 内的地点，可以使用以下查询。

```java
Circle circle = new Circle(-73.99171, 40.738868, 0.003712240453784);
List<Venue> venues =
    template.find(new Query(Criteria.where("location").withinSphere(circle)), Venue.class);
```

要在一个 `Box` 里找到地点，你可以使用以下查询。

```java
//lower-left then upper-right
Box box = new Box(new Point(-73.99756, 40.73083), new Point(-73.988135, 40.741404));
List<Venue> venues =
    template.find(new Query(Criteria.where("location").within(box)), Venue.class);
```

要找到一个 `Point` 附近的地点，你可以使用以下查询。

```java
Point point = new Point(-73.99171, 40.738868);
List<Venue> venues =
    template.find(new Query(Criteria.where("location").near(point).maxDistance(0.01)), Venue.class);
Point point = new Point(-73.99171, 40.738868);
List<Venue> venues =
    template.find(new Query(Criteria.where("location").near(point).minDistance(0.01).maxDistance(100)), Venue.class);
```

要使用球面坐标找到一个 `Point` 附近的位置，可以使用以下查询。

```java
Point point = new Point(-73.99171, 40.738868);
List<Venue> venues =
    template.find(new Query(
        Criteria.where("location").nearSphere(point).maxDistance(0.003712240453784)),
        Venue.class);
```

##### 地域附近查询

|      | **在 2.2 中有所改变!** [MongoDB 4.2](https://docs.mongodb.com/master/release-notes/4.2-compatibility/) 删除了对 `geoNear` 命令的支持，该命令之前被用于运行 `NearQuery`。Spring Data MongoDB 2.2 `MongoOperations#geoNear` 使用 `$geoNear` [聚合](https://docs.mongodb.com/manual/reference/operator/aggregation/geoNear/) 而不是 `geoNear` 命令来运行 `NearQuery`。以前在包装类型中返回的计算距离（使用 `geoNear` 命令时的 `dis`）现在被嵌入到结果文档中。如果给定的 domain 类型已经包含了一个具有该名称的属性，那么计算出的距离将被命名为带有潜在随机后缀的 `calculated-distance`。目标类型可以包含一个以返回的距离命名的属性，以（另外）直接读回 domain 类型中，如下图所示。`GeoResults<VenueWithDisField> = template.query(Venue.class)     .as(VenueWithDisField.class)                                .near(NearQuery.near(new GeoJsonPoint(-73.99, 40.73), KILOMETERS))    .all(); `用于识别目标集合和潜在查询映射的domain类型。目标类型包含一个 `Number` 类型的 `dis` 字段。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

MongoDB支持查询数据库中的地理位置，并同时计算与给定原点的距离。通过地理距离查询，你可以表达诸如 "找到周围10英里内的所有餐馆" 这样的查询。为了让你这样做， `MongoOperations` 提供了 `geoNear(…)` 方法，该方法以 `NearQuery` 为参数（以及已经熟悉的实体类型和集合），如下面的例子中所示。

```java
Point location = new Point(-73.99171, 40.738868);
NearQuery query = NearQuery.near(location).maxDistance(new Distance(10, Metrics.MILES));

GeoResults<Restaurant> = operations.geoNear(query, Restaurant.class);
```

我们使用 `NearQuery` builder API 来设置一个查询，以返回给定点周围10英里以内的所有 `Restaurant` 实例。这里使用的 `Metrics` 枚举实际上实现了一个接口，这样其他的度量（metric）也可以插入到一个距离中。一个 `Metric` 由一个乘数支持，用于将给定度量标准的距离值转换成本地距离。这里显示的例子会认为10是英里。使用一个内置的度量（英里和公里）会自动触发球形标志在查询中被设置。如果你想避免这种情况，可以将普通的 `double` 值传入 `maxDistance(…)`。欲了解更多信息，请参见 `NearQuery` 和 `Distance` 的 [JavaDoc](https://docs.spring.io/spring-data/mongodb/docs/4.1.0-SNAPSHOT/api/index.html)。

geo-near 操作返回一个封装 `GeoResult` 实例的 `GeoResults` 包装对象。包裹 `GeoResults` 可以访问所有结果的平均距离。一个 `GeoResult` 对象携带着所发现的实体以及它与原点的距离。

#### 10.6.5. GeoJSON 的支持

MongoDB支持地理空间数据的 [GeoJSON](https://geojson.org/) 和简单（传统）坐标对。这些格式既可用于存储，也可用于查询数据。请参阅 [MongoDB关于GeoJSON支持](https://docs.mongodb.org/manual/core/2dsphere/#geospatial-indexes-store-geojson/) 的手册，以了解有关要求和限制。

##### Domain 类中的 GeoJSON 类型

在 domain 类中使用 [GeoJSON](https://geojson.org/) 类型是很直接的。`org.springframework.data.mongodb.core.geo` 包包含了 `GeoJsonPoint`、 `GeoJsonPolygon` 和其他类型。这些类型是对现有 `org.springframework.data.geo` 类型的扩展。下面的例子使用了一个 `GeoJsonPoint`。

```java
public class Store {

	String id;

	/**
	 * location is stored in GeoJSON format.
	 * {
	 *   "type" : "Point",
	 *   "coordinates" : [ x, y ]
	 * }
	 */
	GeoJsonPoint location;
}
```

|      | 如果一个 `GeoJSON` 对象的坐标（`coordinates`）代表经度和纬度对，则首先是经度（*latitude*），然后是纬度（*longitude*）。 因此，`GeoJsonPoint` 将 `getX()` 视为经度，将 `getY()` 视为纬度。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### Repository 查询方法中的GeoJSON类型

使用 `GeoJSON` 类型作为 repository 的查询参数，在创建查询时必须使用 `$geometry` 操作符，如下例所示。

```java
public interface StoreRepository extends CrudRepository<Store, String> {

	List<Store> findByLocationWithin(Polygon polygon);  

}

/*
 * {
 *   "location": {
 *     "$geoWithin": {
 *       "$geometry": {
 *         "type": "Polygon",
 *         "coordinates": [
 *           [
 *             [-73.992514,40.758934],
 *             [-73.961138,40.760348],
 *             [-73.991658,40.730006],
 *             [-73.992514,40.758934]
 *           ]
 *         ]
 *       }
 *     }
 *   }
 * }
 */
repo.findByLocationWithin(                              
  new GeoJsonPolygon(
    new Point(-73.992514, 40.758934),
    new Point(-73.961138, 40.760348),
    new Point(-73.991658, 40.730006),
    new Point(-73.992514, 40.758934)));                 

/*
 * {
 *   "location" : {
 *     "$geoWithin" : {
 *        "$polygon" : [ [-73.992514,40.758934] , [-73.961138,40.760348] , [-73.991658,40.730006] ]
 *     }
 *   }
 * }
 */
repo.findByLocationWithin(                              
  new Polygon(
    new Point(-73.992514, 40.758934),
    new Point(-73.961138, 40.760348),
    new Point(-73.991658, 40.730006)));
```

|      | Repository 方法的定义使用共通类型，允许用GeoJSON和传统格式调用它。 |
| ---- | ------------------------------------------------------------ |
|      | 使用 GeoJSON 类型来使用 `$geometry` 运算符。                 |
|      | 请注意，GeoJSON 多边形需要定义一个封闭的环（closed ring）。  |
|      | 使用传统格式的 `$polygon` 操作符。                           |

##### 指标（Metric）和距离（Distance）计算

然后MongoDB的 `$geoNear` 运算符允许使用GeoJSON点或传统的坐标对。

```java
NearQuery.near(new Point(-73.99171, 40.738868))
{
  "$geoNear": {
    //...
    "near": [-73.99171, 40.738868]
  }
}
NearQuery.near(new GeoJsonPoint(-73.99171, 40.738868))
{
  "$geoNear": {
    //...
    "near": { "type": "Point", "coordinates": [-73.99171, 40.738868] }
  }
}
```

虽然在语法上不同，但无论集合中的目标文档使用什么格式，服务器都能接受这两种格式。

|      | 在距离计算方面有很大的不同。使用传统格式的操作 在一个类似于地球的球体上操作 *Radians*，而GeoJSON格式则使用 *Meters*。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

为了避免这种头痛的问题，请确保将 `Metric` 设置为所需的测量单位，以确保正确计算距离。

换言之。

假设你有5个像下面这样的文档。

```json
{
    "_id" : ObjectId("5c10f3735d38908db52796a5"),
    "name" : "Penn Station",
    "location" : { "type" : "Point", "coordinates" : [  -73.99408, 40.75057 ] }
}
{
    "_id" : ObjectId("5c10f3735d38908db52796a6"),
    "name" : "10gen Office",
    "location" : { "type" : "Point", "coordinates" : [ -73.99171, 40.738868 ] }
}
{
    "_id" : ObjectId("5c10f3735d38908db52796a9"),
    "name" : "City Bakery ",
    "location" : { "type" : "Point", "coordinates" : [ -73.992491, 40.738673 ] }
}
{
    "_id" : ObjectId("5c10f3735d38908db52796aa"),
    "name" : "Splash Bar",
    "location" : { "type" : "Point", "coordinates" : [ -73.992491, 40.738673 ] }
}
{
    "_id" : ObjectId("5c10f3735d38908db52796ab"),
    "name" : "Momofuku Milk Bar",
    "location" : { "type" : "Point", "coordinates" : [ -73.985839, 40.731698 ] }
}
```

使用GeoJSON获取从 `[-73.99171, 40.738868]` 的400米半径内的所有文档，看起来像这样。

Example 72. 使用GeoJSON的GeoNear

```json
{
    "$geoNear": {
        "maxDistance": 400, 
        "num": 10,
        "near": { type: "Point", coordinates: [-73.99171, 40.738868] },
        "spherical":true, 
        "key": "location",
        "distanceField": "distance"
    }
}
```

返回以下3个文档。

```json
{
    "_id" : ObjectId("5c10f3735d38908db52796a6"),
    "name" : "10gen Office",
    "location" : { "type" : "Point", "coordinates" : [ -73.99171, 40.738868 ] }
    "distance" : 0.0 
}
{
    "_id" : ObjectId("5c10f3735d38908db52796a9"),
    "name" : "City Bakery ",
    "location" : { "type" : "Point", "coordinates" : [ -73.992491, 40.738673 ] }
    "distance" : 69.3582262492474 
}
{
    "_id" : ObjectId("5c10f3735d38908db52796aa"),
    "name" : "Splash Bar",
    "location" : { "type" : "Point", "coordinates" : [ -73.992491, 40.738673 ] }
    "distance" : 69.3582262492474 
}
```

|      | 与中心点的最大距离，单位：米 (*Meters*)。 |
| ---- | ----------------------------------------- |
|      | GeoJSON总是在一个球体上操作。             |
|      | 与中心点的距离，单位是米(*Meters*)。      |

现在，当使用传统的坐标对时，就像之前讨论的那样，在 *Radians* 上操作。所以我们在构建 `$geoNear` 命令时使用 `Metrics#KILOMETERS`。`Metric` 确保距离乘数被正确设置。

Example 73. 带有传统坐标对的GeoNear

```json
{
    "$geoNear": {
        "maxDistance": 0.0000627142377, 
        "distanceMultiplier": 6378.137, 
        "num": 10,
        "near": [-73.99171, 40.738868],
        "spherical":true, 
        "key": "location",
        "distanceField": "distance"
    }
}
```

返回3个文档，就像GeoJSON的变体。

```json
{
    "_id" : ObjectId("5c10f3735d38908db52796a6"),
    "name" : "10gen Office",
    "location" : { "type" : "Point", "coordinates" : [ -73.99171, 40.738868 ] }
    "distance" : 0.0 
}
{
    "_id" : ObjectId("5c10f3735d38908db52796a9"),
    "name" : "City Bakery ",
    "location" : { "type" : "Point", "coordinates" : [ -73.992491, 40.738673 ] }
    "distance" : 0.0693586286032982 
}
{
    "_id" : ObjectId("5c10f3735d38908db52796aa"),
    "name" : "Splash Bar",
    "location" : { "type" : "Point", "coordinates" : [ -73.992491, 40.738673 ] }
    "distance" : 0.0693586286032982 
}
```

|      | 与中心点的最大距离，单位：弧度(*Radians*)。                  |
| ---- | ------------------------------------------------------------ |
|      | 距离的乘数，所以我们得到公里(*Kilometers*)作为结果距离。     |
|      | 确保我们对 2d_sphere 索引进行操作。                          |
|      | 与中心点的距离，以公里(*Kilometers*)为单位—取其1000倍以匹配GeoJSON变量的米(*Meters*)数。 |

##### GeoJSON Jackson 模块

通过使用 [Web 的支持](https://springdoc.cn/spring-data-mongodb/#core.web)，Spring Data 为 `ObjectMapper` 注册了额外的 Jackson Moduless，用于反/序列化常见的Spring Data Domain 类型。请参考 [Spring Data Jackson 模块](https://springdoc.cn/spring-data-mongodb/#core.web.basic.jackson-mappers) 部分，以了解更多关于此功能的基础设施设置。

MongoDB模块还通过其暴露于 `GeoJsonModule` 的 `GeoJsonConfiguration` 为以下GeoJSON类型注册了 `JsonDeserializer`。

```
org.springframework.data.mongodb.core.geo.GeoJsonPoint
org.springframework.data.mongodb.core.geo.GeoJsonMultiPoint
org.springframework.data.mongodb.core.geo.GeoJsonLineString
org.springframework.data.mongodb.core.geo.GeoJsonMultiLineString
org.springframework.data.mongodb.core.geo.GeoJsonPolygon
org.springframework.data.mongodb.core.geo.GeoJsonMultiPolygon
```

|      | `GeoJsonModule` 只注册了 `JsonDeserializer` ! 为了给 `ObjectMapper` 配备一套对称的 `JsonSerializer`，你需要为 `ObjectMapper` 手动配置这些，或者提供一个自定义的 `SpringDataJacksonModules` 配置，将 `GeoJsonModule.serializers()` 作为一个Spring Bean公开。`class GeoJsonConfiguration implements SpringDataJacksonModules { 	@Bean public Module geoJsonSerializers() { 	return GeoJsonModule.serializers(); } } ` |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 下一个主要版本（`4.0`）将同时注册 `JsonDeserializer` 和 `JsonSerializer`，默认为 GeoJSON 类型。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 10.6.6. 全文检索

从MongoDB的 2.6 版本开始，你可以通过使用 `$text` 操作符来运行全文检索。在 `TextQuery` 和 `TextCriteria` 中，有专门针对全文检索的方法和操作。在进行全文检索时，请参见 [MongoDB参考文献](https://docs.mongodb.org/manual/reference/operator/query/text/#behavior)，了解其行为和限制。

##### 全文搜索

在你真正使用全文搜索之前，你必须正确设置搜索索引。关于如何创建索引结构的更多细节，请参见 [文本索引](https://springdoc.cn/spring-data-mongodb/#mapping-usage-indexes.text-index)。下面的例子显示了如何设置全文搜索。

```javascript
db.foo.createIndex(
{
  title : "text",
  content : "text"
},
{
  weights : {
              title : 3
            }
}
)
```

一个搜索 `coffee cake` 的查询可以被定义并运行如下。

Example 74. 全文检索

```java
Query query = TextQuery
  .queryText(new TextCriteria().matchingAny("coffee", "cake"));

List<Document> page = template.find(query, Document.class);
```

要根据 `weights` 按相关性对结果进行排序，请使用 `TextQuery.sortByScore`。

Example 75. 全文查询 - 按Score排序

```java
Query query = TextQuery
  .queryText(new TextCriteria().matchingAny("coffee", "cake"))
  .sortByScore() 
  .includeScore(); 

List<Document> page = template.find(query, Document.class);
```

|      | 使用 score 属性按相关性对结果进行排序，这会触发 `.sort({'score': {'$meta': 'textScore'}})`。 |
| ---- | ------------------------------------------------------------ |
|      | 使用 `TextQuery.includeScore()` 将计算出的相关性包括在结果的 `Document` 中。 |

你可以通过在搜索词前加上 `-` 或者使用 `notMatching` 来排除搜索词，如下例所示（注意，这两行有相同的效果，因此是多余的）。

```java
// search for 'coffee' and not 'cake'
TextQuery.queryText(new TextCriteria().matching("coffee").matching("-cake"));
TextQuery.queryText(new TextCriteria().matching("coffee").notMatching("cake"));
```

`TextCriteria.matching` 将提供的术语作为原样。因此，你可以通过把短语放在双引号之间（例如，`\"coffee cake\"`)）或使用 `TextCriteria.phrase` 来定义短语。下面的例子显示了定义短语的两种方式。

```java
// search for phrase 'coffee cake'
TextQuery.queryText(new TextCriteria().matching("\"coffee cake\""));
TextQuery.queryText(new TextCriteria().phrase("coffee cake"));
```

你可以通过使用 `TextCriteria` 上的相应方法为 `$caseSensitive` 和 `$diacriticSensitive` 设置标志。请注意，这两个可选的标志在 MongoDB 3.2 中被引入，除非明确设置，否则不包括在查询中。

#### 10.6.7. Collations

从 3.4 版本开始，MongoDB 支持用于集合和索引创建以及各种查询操作的校对。Collations 定义了基于 [ICU collations](http://userguide.icu-project.org/collation/concepts) 的字符串比较规则。一个 collation 文档由各种属性组成，这些属性被封装在 `Collation` 中，如下面的列表所示。

```java
Collation collation = Collation.of("fr")         

  .strength(ComparisonLevel.secondary()          
    .includeCase())

  .numericOrderingEnabled()                      

  .alternate(Alternate.shifted().punct())        

  .forwardDiacriticSort()                        

  .normalizationEnabled();                       
```

|      | `Collation` 需要一个locale来创建。这可以是一个字符串表示的locale，一个 `Locale`（考虑到语言、国家和变体）或一个 `CollationLocale`。locale对于创建来说是强制性的。 |
| ---- | ------------------------------------------------------------ |
|      | Collation 强度定义了表示字符之间差异的比较级别。你可以根据选择的强度配置各种选项（大小写敏感度、大小写排序和其他）。 |
|      | 指定是将数字字符串作为数字还是字符串进行比较。               |
|      | 指定整理是否应将空白和标点符号作为比较的基础字符。           |
|      | 指定带有变音符的字符串是否从字符串的后面排序，如某些法语词典的排序。 |
|      | 指定是否检查文本是否需要规范化以及是否执行规范化。           |

Collation 可以被用来创建集合和索引。如果你创建了一个指定了 Collation 的集合，除非你指定了不同的 Collation，否则该 Collation 将应用于索引的创建和查询。一个 Collation 对整个操作是有效的，不能在每个字段的基础上指定。

像其他元数据一样，整理可以通过 `@Document` 注解的 `collation` 属性从domain类型中导出，并将在运行查询、创建集合或索引时直接应用。

|      | 当 MongoDB 在第一次交互时自动创建一个集合时，将不使用注解的排序。这将需要额外的存储互动，延迟整个过程。请在这些情况下使用 `MongoOperations.createCollection`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

```java
Collation french = Collation.of("fr");
Collation german = Collation.of("de");

template.createCollection(Person.class, CollectionOptions.just(collation));

template.indexOps(Person.class).ensureIndex(new Index("name", Direction.ASC).collation(german));
```

|      | 如果没有指定 collation （`Collation.simple()`），MongoDB 使用简单的二进制比较。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

在集合操作中使用 collation，只需在查询或操作选项中指定一个 `Collation` 实例，正如下面两个例子所示。

Example 76. 使用 `find` 的 collation

```java
Collation collation = Collation.of("de");

Query query = new Query(Criteria.where("firstName").is("Amél")).collation(collation);

List<Person> results = template.find(query, Person.class);
```

Example 77. 用 `aggregate` 的 collation

```java
Collation collation = Collation.of("de");

AggregationOptions options = AggregationOptions.builder().collation(collation).build();

Aggregation aggregation = newAggregation(
  project("tags"),
  unwind("tags"),
  group("tags")
    .count().as("count")
).withOptions(options);

AggregationResults<TagCount> results = template.aggregate(aggregation, "tags", TagCount.class);
```

|      | 只有当操作使用 collation 与索引 collation 匹配时，才会使用索引。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

[MongoDB Repository](https://springdoc.cn/spring-data-mongodb/#mongo.repositories) 通过 `@Query` 注解的 `collation` 属性支持 `Collation`。

Example 78. 对 Repository 的 Collation 的支持

```java
public interface PersonRepository extends MongoRepository<Person, String> {

  @Query(collation = "en_US")  
  List<Person> findByFirstname(String firstname);

  @Query(collation = "{ 'locale' : 'en_US' }") 
  List<Person> findPersonByFirstname(String firstname);

  @Query(collation = "?1") 
  List<Person> findByFirstname(String firstname, Object collation);

  @Query(collation = "{ 'locale' : '?1' }") 
  List<Person> findByFirstname(String firstname, String collation);

  List<Person> findByFirstname(String firstname, Collation collation); 

  @Query(collation = "{ 'locale' : 'en_US' }")
  List<Person> findByFirstname(String firstname, @Nullable Collation collation); 
}
```

|      | 静态 collation 定义导致 `{ 'locale' : 'en_US' }`。           |
| ---- | ------------------------------------------------------------ |
|      | 静态 collation 定义导致 `{ 'locale' : 'en_US' }`。           |
|      | 根据第2个方法参数进行动态 collation。允许的类型包括 `String`（例如：'en_US'），`Locacle`（例如：Locacle.US）。 和 `Document` (例如，new Document("locale", "en_US")) |
|      | 根据第2个方法参数进行动态 collation 。                       |
|      | 在查询中应用 `Collation` 方法参数。                          |
|      | 如果不为null，`Collation` 方法参数将覆盖来自 `@Query` 的默认 `collation`。 |

|      | 如果你启用了 repository 查找方法的自动索引创建功能，那么在创建索引时将包括潜在的静态 collation 定义，如（1）和（2）所示。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 最具体的 `Collation` 比潜在的其他定义更重要。这意味着方法参数优于查询方法注解优于domain类型注解。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

为了简化整个代码库中 collation 属性的使用，也可以使用 `@Collation` 注解，它可以作为上述属性的元注解。同样的规则和位置适用，此外，`@Collation` 的直接使用取代了在 `@Query` 和其他注释中定义的任何 collation 值。这意味着，如果通过 `@Query` 和 `@Collation` 声明了一个 collation，那么就从 `@Collation` 中挑选一个。

Example 79. 使用 `@Collation`

```java
@Collation("en_US") 
class Game {
  // ...
}

interface GameRepository extends Repository<Game, String> {

  @Collation("en_GB")  
  List<Game> findByTitle(String title);

  @Collation("de_AT")  
  @Query(collation="en_GB")
  List<Game> findByDescriptionContaining(String keyword);
}
```

|      | 代替 `@Document(collation=…)`。        |
| ---- | -------------------------------------- |
|      | 代替 `@Query(collation=…)`。           |
|      | 倾向于 `@Collation` 而不是meta的使用。 |

##### JSON Schema

从3.6版本开始，MongoDB支持根据提供的 [JSON Schema](https://docs.mongodb.com/manual/core/schema-validation/#json-schema) 验证文档的集合。schema 本身以及验证动作和级别都可以在创建集合时定义，如下面的例子所示。

Example 80. JSON schema 示例

```json
{
  "type": "object",                                                        

  "required": [ "firstname", "lastname" ],                                 

  "properties": {                                                          

    "firstname": {                                                         
      "type": "string",
      "enum": [ "luke", "han" ]
    },
    "address": {                                                           
      "type": "object",
      "properties": {
        "postCode": { "type": "string", "minLength": 4, "maxLength": 5 }
      }
    }
  }
}
```

|      | JSON schema 文档总是从根部开始描述整个文档。一个 schema 是一个 schema 对象本身，它可以包含描述属性和子文件的嵌入式 schema 对象。 |
| ---- | ------------------------------------------------------------ |
|      | `required` 是一个描述文档中哪些属性是必需的属性。它可以和其他 schema 约束一起被选择性地指定。参见 MongoDB 关于 [可用关键字的文档](https://docs.mongodb.com/manual/reference/operator/query/jsonSchema/#available-keywords)。 |
|      | `properties` 与描述一个 `object` 类型的 schema 对象有关。它包含特定属性 schema 式约束。 |
|      | `firstname` 为文档中的 `firsname` 字段指定约束。这里，它是一个基于字符串的 `properties` 元素，声明了可能的字段值。 |
|      | `address` 是一个子文档，为其 `postCode` 字段的值定义了一个 schema。 |

你可以通过指定schema文档（即使用Document API解析或构建文档对象）或使用 `org.springframework.data.mongodb.core.schema` 中的 Spring Data 的JSON schema工具构建schema来提供schema。`MongoJsonSchema` 是所有JSON schema相关操作的入口点。下面的示例展示了如何使用 `MongoJsonSchema.builder()` 来创建一个 JSON schema。

Example 81. 创建一个 JSON schema

```java
MongoJsonSchema.builder()                                                    
    .required("lastname")                                                    

    .properties(
                required(string("firstname").possibleValues("luke", "han")), 

                object("address")
                     .properties(string("postCode").minLength(4).maxLength(5)))

    .build();                                                                
```

|      | 获得一个schema builder，用fluent API配置模式。               |
| ---- | ------------------------------------------------------------ |
|      | 直接配置所需的 properties，如图所示，或如3中的更多细节。     |
|      | 配置所需的 String 类型的 `firstname` 字段，只允许 `luke` 和 `han` 值。Properties 可以是类型化的，也可以是未类型化的。使用 `JsonSchemaProperty` 的静态导入，使语法稍微紧凑一些，并获得 `string(…)` 等入口。 |
|      | 构建 schema 对象。使用该 schema 来创建一个集合或 [查询文档](https://springdoc.cn/spring-data-mongodb/#mongodb-template-query.criteria)。 |

已经有一些预定义和强类型的模式对象（`JsonSchemaObject` 和 `JsonSchemaProperty`）可以通过网关接口上的静态方法获得。然而，你可能需要建立自定义的 property 验证规则，这些规则可以通过 builder API创建，如下例所示。

```java
// "birthdate" : { "bsonType": "date" }
JsonSchemaProperty.named("birthdate").ofType(Type.dateType());

// "birthdate" : { "bsonType": "date", "description", "Must be a date" }
JsonSchemaProperty.named("birthdate").with(JsonSchemaObject.of(Type.dateType()).description("Must be a date"));
```

`CollectionOptions` 提供了对集合的 schema 支持的入口，正如下面的例子所示。

Example 82. 用 `$jsonSchema` 创建集合

```java
MongoJsonSchema schema = MongoJsonSchema.builder().required("firstname", "lastname").build();

template.createCollection(Person.class, CollectionOptions.empty().schema(schema));
```

###### 生成一个Schema

设置 schema 可能是一项耗时的工作，我们鼓励每个决定这样做的人，真的要花点时间。这很重要，schema 的改变可能很困难。然而，有时人们可能并不想与之纠缠不清，这就是 `JsonSchemaCreator` 发挥作用的地方。

`JsonSchemaCreator` 及其默认实现会从映射基础设施提供的domain类型元数据中生成 `MongoJsonSchema`。这意味着，[注解的属性](https://springdoc.cn/spring-data-mongodb/#mapping-usage-annotations) 以及潜在的 [自定义转换](https://springdoc.cn/spring-data-mongodb/#mapping-configuration) 都被考虑在内。

Example 83. 从domain 类型生成Json Schema

```java
public class Person {

    private final String firstname;                   
    private final int age;                            
    private Species species;                          
    private Address address;                          
    private @Field(fieldType=SCRIPT) String theForce; 
    private @Transient Boolean useTheForce;           

    public Person(String firstname, int age) {         

        this.firstname = firstname;
        this.age = age;
    }

    // gettter / setter omitted
}

MongoJsonSchema schema = MongoJsonSchemaCreator.create(mongoOperations.getConverter())
    .createSchemaFor(Person.class);

template.createCollection(Person.class, CollectionOptions.empty().schema(schema));
{
    'type' : 'object',
    'required' : ['age'],                     
    'properties' : {
        'firstname' : { 'type' : 'string' },  
        'age' : { 'bsonType' : 'int' }        
        'species' : {                         
            'type' : 'string',
            'enum' : ['HUMAN', 'WOOKIE', 'UNKNOWN']
        }
        'address' : {                         
            'type' : 'object'
            'properties' : {
                'postCode' : { 'type': 'string' }
            }
        },
        'theForce' : { 'type' : 'javascript'} 
     }
}
```

|      | 简单的对象properties 被认为是常规properties。 |
| ---- | --------------------------------------------- |
|      | 基本类型被认为是必要的属性。                  |
|      | 枚举被限制为可能的值。                        |
|      | 对象类型properties 被检查并表示为嵌套文档。   |
|      | 被转换器转换为 `Code` 的 `String` 类型属性。  |
|      | `@Transient` 属性在生成 schema 时被省略。     |

|      | 使用可转换为 `ObjectId` 的类型（如 `String`）的 `_id` 属性被映射为 `{ type : 'object' }`，除非通过 `@MongoId` 注解有更具体的信息。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

| Java         | Schema Type          | 说明                                                         |
| :----------- | :------------------- | :----------------------------------------------------------- |
| `Object`     | `type : object`      | with `properties` if metadata available.                     |
| `Collection` | `type : array`       | -                                                            |
| `Map`        | `type : object`      | -                                                            |
| `Enum`       | `type : string`      | with `enum` property holding the possible enumeration values. |
| `array`      | `type : array`       | simple type array unless it’s a `byte[]`                     |
| `byte[]`     | `bsonType : binData` | -                                                            |

上面的例子展示了如何从一个非常精确的类型源中推导出 schema。在 domain 模型中使用多态元素会导致 `Object` 和泛型 `<T>` 的 schema 表示不准确，这些类型很可能被表示为 `{ type : 'object' }` 而没有进一步说明。`MongoJsonSchemaCreator.property(..)` 允许定义额外的细节，例如在渲染 schema 时应考虑的嵌套文档类型。

Example 84. 为properties指定额外的类型

```java
class Root {
	Object value;
}

class A {
	String aValue;
}

class B {
	String bValue;
}
MongoJsonSchemaCreator.create()
    .property("value").withTypes(A.class, B.class) 
{
    'type' : 'object',
    'properties' : {
        'value' : {
            'type' : 'object',
            'properties' : {                       
                'aValue' : { 'type' : 'string' },
                'bValue' : { 'type' : 'string' }
            }
        }
    }
}
```

|      | 给定类型的Properties被合并为一个元素。 |
| ---- | -------------------------------------- |
|      |                                        |

MongoDB的无 schema 方法允许在一个集合中存储不同结构的文档。这些文档可以通过一个共同的基类进行建模。无论选择哪种方法，`MongoJsonSchemaCreator.merge(…)` 都可以帮助规避将多个 schema 合并为一个的需要。

Example 85. 将多个 Schema 合并为一个 Schema definition

```java
abstract class Root {
	String rootValue;
}

class A extends Root {
	String aValue;
}

class B extends Root {
	String bValue;
}

MongoJsonSchemaCreator.mergedSchemaFor(A.class, B.class) 
{
    'type' : 'object',
       'properties' : { 
           'rootValue' : { 'type' : 'string' },
           'aValue' : { 'type' : 'string' },
           'bValue' : { 'type' : 'string' }
       }
    }
}
```

|      | 给定类型的Properties （及其继承的属性）被合并为一个schema。 |
| ---- | ----------------------------------------------------------- |
|      |                                                             |

|      | 具有相同名称的Properties需要引用相同的JSON schema 才能被合并。下面的例子显示了一个因为数据类型不匹配而无法自动合并的definition。在这种情况下，必须向 `MongoJsonSchemaCreator` 提供一个 `ConflictResolutionFunction`。`class A extends Root { String value; } class B extends Root { Integer value; } ` |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

###### 查询一个集合以匹配JSON Schema

你可以使用 schema 来查询任何集合中符合JSON schema 所定义的给定结构的文档，如下面的例子所示。

Example 86. 查询符合 `$jsonSchema` 的文档

```java
MongoJsonSchema schema = MongoJsonSchema.builder().required("firstname", "lastname").build();

template.find(query(matchingDocumentStructure(schema)), Person.class);
```

###### 加密的字段

MongoDB 4.2 [字段级加密](https://docs.mongodb.com/master/core/security-client-side-encryption/) 允许直接对单个属性进行加密。

在设置JSON Schema 时，Properties 可以被包裹在一个加密的 Properties 中，如下例所示。

Example 87. 通过 Json Schema 的客户端字段级加密

```java
MongoJsonSchema schema = MongoJsonSchema.builder()
    .properties(
        encrypted(string("ssn"))
            .algorithm("AEAD_AES_256_CBC_HMAC_SHA_512-Deterministic")
            .keyId("*key0_id")
	).build();
```

我们可以利用 `@Encrypted` 注解来代替手动定义加密字段，如下文所示。

Example 88. 通过Json Schema的客户端字段级加密

```java
@Document
@Encrypted(keyId = "xKVup8B1Q+CkHaVRx+qa+g==", algorithm = "AEAD_AES_256_CBC_HMAC_SHA_512-Random") 
static class Patient {

    @Id String id;
    String name;

    @Encrypted 
    String bloodType;

    @Encrypted(algorithm = "AEAD_AES_256_CBC_HMAC_SHA_512-Deterministic") 
    Integer ssn;
}
```

|      | 将为 `encryptMetadata` 设置的默认加密设置。 |
| ---- | ------------------------------------------- |
|      | 使用默认加密设置的加密字段。                |
|      | 加密字段覆盖了默认的加密算法。              |

|      | `@Encrypted` 注解支持通过SpEL表达式解析keyIds。要做到这一点，需要额外的环境元数据（通过 `MappingContext`），并且必须提供。`@Document @Encrypted(keyId = "#{mongocrypt.keyId(#target)}") static class Patient {     @Id String id;    String name;     @Encrypted(algorithm = "AEAD_AES_256_CBC_HMAC_SHA_512-Random")    String bloodType;     @Encrypted(algorithm = "AEAD_AES_256_CBC_HMAC_SHA_512-Deterministic")    Integer ssn; } MongoJsonSchemaCreator schemaCreator = MongoJsonSchemaCreator.create(mappingContext); MongoJsonSchema patientSchema = schemaCreator    .filter(MongoJsonSchemaCreator.encryptedOnly())    .createSchemaFor(Patient.class); ``mongocrypt.keyId` 函数是通过 `EvaluationContextExtension` 定义的，如下面的片段所示。提供一个自定义扩展提供了计算 keyIds 的最灵活的方式。`public class EncryptionExtension implements EvaluationContextExtension {     @Override    public String getExtensionId() {        return "mongocrypt";    }     @Override    public Map<String, Function> getFunctions() {        return Collections.singletonMap("keyId", new Function(getMethod("computeKeyId", String.class), this));    }     public String computeKeyId(String target) {        // ... lookup via target element name    } } `要在Spring Boot应用程序中把派生加密设置与 `AutoEncryptionSettings` 结合起来，请使用 `MongoClientSettingsBuilderCustomizer`。`@Bean MongoClientSettingsBuilderCustomizer customizer(MappingContext mappingContext) {    return (builder) -> {         // ... keyVaultCollection, kmsProvider, ...         MongoJsonSchemaCreator schemaCreator = MongoJsonSchemaCreator.create(mappingContext);        MongoJsonSchema patientSchema = schemaCreator            .filter(MongoJsonSchemaCreator.encryptedOnly())            .createSchemaFor(Patient.class);         AutoEncryptionSettings autoEncryptionSettings = AutoEncryptionSettings.builder()            .keyVaultNamespace(keyVaultCollection)            .kmsProviders(kmsProviders)            .extraOptions(extraOpts)            .schemaMap(Collections.singletonMap("db.patient", patientSchema.schemaDocument().toBsonDocument()))            .build();         builder.autoEncryptionSettings(autoEncryptionSettings);    }; } ` |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 请确保将驱动 `com.mongodb.AutoEncryptionSettings` 设置为使用客户端加密。MongoDB不支持对所有字段类型进行加密。特定的数据类型需要确定性的加密，以保留平等比较（equality comparison）功能。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

###### JSON Schema 类型

下表显示了支持的JSON schema 类型。

| Schema Type | Java Type                            | Schema Properties                                            |
| :---------- | :----------------------------------- | :----------------------------------------------------------- |
| `untyped`   | -                                    | `description`, generated `description`, `enum`, `allOf`, `anyOf`, `oneOf`, `not` |
| `object`    | `Object`                             | `required`, `additionalProperties`, `properties`, `minProperties`, `maxProperties`, `patternProperties` |
| `array`     | any array except `byte[]`            | `uniqueItems`, `additionalItems`, `items`, `minItems`, `maxItems` |
| `string`    | `String`                             | `minLength`, `maxLentgth`, `pattern`                         |
| `int`       | `int`, `Integer`                     | `multipleOf`, `minimum`, `exclusiveMinimum`, `maximum`, `exclusiveMaximum` |
| `long`      | `long`, `Long`                       | `multipleOf`, `minimum`, `exclusiveMinimum`, `maximum`, `exclusiveMaximum` |
| `double`    | `float`, `Float`, `double`, `Double` | `multipleOf`, `minimum`, `exclusiveMinimum`, `maximum`, `exclusiveMaximum` |
| `decimal`   | `BigDecimal`                         | `multipleOf`, `minimum`, `exclusiveMinimum`, `maximum`, `exclusiveMaximum` |
| `number`    | `Number`                             | `multipleOf`, `minimum`, `exclusiveMinimum`, `maximum`, `exclusiveMaximum` |
| `binData`   | `byte[]`                             | (none)                                                       |
| `boolean`   | `boolean`, `Boolean`                 | (none)                                                       |
| `null`      | `null`                               | (none)                                                       |
| `objectId`  | `ObjectId`                           | (none)                                                       |
| `date`      | `java.util.Date`                     | (none)                                                       |
| `timestamp` | `BsonTimestamp`                      | (none)                                                       |
| `regex`     | `java.util.regex.Pattern`            | (none)                                                       |

|      | `untyped` 是一个被所有类型化 schema 类型所继承的通用类型。它为类型化 schema 类型提供所有 `untyped` schema 属性。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

欲了解更多信息，请参见 [$jsonSchema](https://docs.mongodb.com/manual/reference/operator/query/jsonSchema/#op._S_jsonSchema)。

#### 10.6.8. Fluent Template API

`MongoOperations` 接口是与MongoDB进行更多底层交互的核心组件之一。它提供了广泛的方法，涵盖了从集合创建、索引创建、CRUD操作到更高级的功能，如Map-Reduce和聚合。你可以为每个方法找到多个重载。它们中的大多数涵盖了API的可选或可忽略的部分。

`FluentMongoOperations` 为 `MongoOperations` 的常用方法提供了一个更窄的接口，并提供了一个更可读的、fluent API。入口点（`insert(…)`、`find(…)`、`update(…)` 等）遵循基于要运行的操作的自然命名模式。从入口点开始，API被设计成只提供与上下文相关的方法，这些方法会导致一个终止方法，调用实际的M `ongoOperations` 对应的方法—在下面的例子中是 `all` 方法。

```java
List<SWCharacter> all = ops.find(SWCharacter.class)
  .inCollection("star-wars")                        
  .all();
```

|      | 如果 `SWCharacter` 用 `@Document` 定义了 collection，或者你用类的名字作为 collection 的名字，那就跳过这一步。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

有时，MongoD 中的一个集合持有不同类型的实体，比如 `SWCharacters` 集合中的 `Jedi`。为了在查询和返回值映射中使用不同的类型，你可以使用 `as(Class<?> targetType)` 来对结果进行不同的映射，如下例所示。

```java
List<Jedi> all = ops.find(SWCharacter.class)    
  .as(Jedi.class)                               
  .matching(query(where("jedi").is(true)))
  .all();
```

|      | 查询字段是针对 `SWCharacter` 类型进行映射的。 |
| ---- | --------------------------------------------- |
|      | 结果文档被映射到 `Jedi`。                     |

|      | 你可以通过 `as(Class<?>)` 提供目标类型，直接将 [投影](https://springdoc.cn/spring-data-mongodb/#projections) 应用于结果文档。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 使用投影允许 `MongoTemplate` 通过将实际响应限制在投影目标类型所要求的字段上，来优化结果映射。只要 `Query` 本身不包含任何字段限制，并且目标类型是一个封闭的接口或DTO投影，这就适用。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 投影不得应用于 [DBRefs](https://springdoc.cn/spring-data-mongodb/#mapping-usage-references)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

你可以通过终止方法：`first()`、`one()`、`all()` 或 `stream()`，在检索单个实体和以 `List` 或 `Stream` 形式检索多个实体之间进行切换。

当用 `near(NearQuery)` 编写地理空间查询时，终止方法的数量被改变，只包括在 MongoDB 中运行 `geoNear` 命令的有效方法（在 `GeoResults` 中作为 `GeoResult` 获取实体），如下例所示。

```java
GeoResults<Jedi> results = mongoOps.query(SWCharacter.class)
  .as(Jedi.class)
  .near(alderaan) // NearQuery.near(-73.9667, 40.78).maxDis…
  .all();
```

#### 10.6.9. Kotlin的类型安全的查询

Kotlin 通过其语言语法和扩展系统拥抱特定领域的语言创建。Spring Data MongoDB提供了一个Kotlin扩展，用于使用 [Kotlin属性引用](https://kotlinlang.org/docs/reference/reflection.html#property-references) 来构建类型安全的查询。使用该扩展的查询通常会受益于可读性的提高。`Criteria` 上的大多数关键字都有匹配的Kotlin扩展，例如 `inValues` 和 `regex`。

考虑下面的例子，解释类型安全的查询。

```kotlin
mongoOperations.find<Book>(
  Query(Book::title isEqualTo "Moby-Dick")               
)

mongoOperations.find<Book>(
  Query(titlePredicate = Book::title exists true)
)

mongoOperations.find<Book>(
  Query(
    Criteria().andOperator(
      Book::price gt 5,
      Book::price lt 10
    ))
)

// Binary operators
mongoOperations.find<BinaryMessage>(
  Query(BinaryMessage::payload bits { allClear(0b101) }) 
)

// Nested Properties (i.e. refer to "book.author")
mongoOperations.find<Book>(
  Query(Book::author / Author::name regex "^H")          
)
```

|      | `isEqualTo()` 是一个 infix 扩展函数，接收器（receiver）类型为 `KProperty<T>`，返回 `Criteria`。 |
| ---- | ------------------------------------------------------------ |
|      | 对于位操作符，传递一个lambda参数，在这里你调用 `Criteria.BitwiseCriteriaOperators` 中的一个方法。 |
|      | 要构建嵌套属性，请使用 `/` 字符（重载操作符 `div`）。        |

#### 10.6.10. 其他查询选项

MongoDB提供了各种应用元信息的方法，如comment或batch size，到查询中。直接使用 `Query` API，有几种方法可以实现这些选项。

```java
Query query = query(where("firstname").is("luke"))
    .comment("find luke")         
    .cursorBatchSize(100)                                 
```

|      | 该comment 传播到 MongoDB profile 日志。  |
| ---- | ---------------------------------------- |
|      | 每个 response batch 中要返回的文档数量。 |

在 repository 层面，`@Meta` 注解提供了以声明的方式添加查询选项的方法。

```java
@Meta(comment = "find luke", cursorBatchSize = 100, flags = { SLAVE_OK })
List<Person> findByFirstname(String firstname);
```

### 10.7. Example 查询

#### 10.7.1. 介绍

本章介绍了 "Example 查询" 并解释了如何使用它。

Example 查询（QBE）是一种用户友好的查询技术，接口简单。它允许动态查询创建，不要求你写包含字段名的查询。事实上，"Example 查询" 根本不要求你通过使用store特定的查询语言来编写查询。

#### 10.7.2. 使用方式

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

Example 89. Sample Person object

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

|      | 将属性纳入 Example 查询的标准是基于非 null 的。使用原始类型（`int`, `double`, …）的属性总是被包括在内，除非 [`ExampleMatcher` 忽略了属性路径](https://springdoc.cn/spring-data-mongodb/#query-by-example.matchers)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

例子可以通过使用工厂方法或通过使用 [`ExampleMatcher`](https://springdoc.cn/spring-data-mongodb/#query-by-example.matchers) 来构建。`Example` 是不可改变的。下面的列表显示了一个简单的例子。

Example 90. Simple Example

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

Example 91. `QueryByExampleExecutor`

```java
public interface QueryByExampleExecutor<T> {

  <S extends T> S findOne(Example<S> example);

  <S extends T> Iterable<S> findAll(Example<S> example);

  // … more functionality omitted.
}
```

#### 10.7.3. Example Matcher

示例不限于默认设置。你可以通过使用 `ExampleMatcher` 为字符串匹配、null处理和特定属性设置指定你自己的默认值，如下面的例子所示。

Example 92. Example matcher with customized matching

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

Example 93. 配置 matcher 属性

```java
ExampleMatcher matcher = ExampleMatcher.matching()
  .withMatcher("firstname", endsWith())
  .withMatcher("lastname", startsWith().ignoreCase());
}
```

配置 matcher 选项的另一种方法是使用 `lambda` （在Java 8中引入）。这种方法创建一个回调，要求实现者修改matcher。你不需要返回matcher，因为配置选项被保存在matcher实例中。下面的例子显示了一个使用lambda的matcher。

Example 94. 使用 lambda 配置 matcher option

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

#### 10.7.4. Fluent API

`QueryByExampleExecutor` 还提供了一个方法，我们到目前为止还没有提到。`<S extends T, R> R findBy(Example<S> example, Function<FluentQuery.FetchableFluentQuery<S>, R> queryFunction)` 。和其他方法一样，它执行一个从 `Example` 派生的查询。然而，通过第二个参数，你可以控制该执行的各个方面，否则你无法动态地控制。你可以通过调用第二个参数中的 `FetchableFluentQuery` 的各种方法来做到这一点。 `sortBy` 让你为你的结果指定一个排序。`as` 让你指定你希望结果被转换的类型。`project` 限制了被查询的属性。`first`, `firstValue`, `one`, `oneValue`, `all`, `page`, `stream`, `count`, 和 `exists` 定义了你得到什么样的结果以及当超过预期结果数量时查询的行为方式。

Example 95. 使fluent API来获得可能是许多结果中的最后一个，按lastname排序。

```java
Optional<Person> match = repository.findBy(example,
    q -> q
        .sortBy(Sort.by("lastname").descending())
        .first()
);
```

#### 10.7.5. Example

下面的例子显示了在使用 repository（这里是指 `Person` 对象）时如何通过 example 进行查询。

Example 96. 使用 repository 的 Example 查询

```java
public interface PersonRepository extends QueryByExampleExecutor<Person> {

}

public class PersonService {

  @Autowired PersonRepository personRepository;

  public List<Person> findPeople(Person probe) {
    return personRepository.findAll(Example.of(probe));
  }
}
```

一个包含无类型的 `ExampleSpec` 的 `Example` 使用 `Repository` 类型和它的集合名称。泛型 `ExampleSpec` 实例使用其泛型作为结果类型，并使用来自 `Repository` 实例的集合名称。

|      | 当在 `ExampleSpec` 中包含 `null` 值时，Spring Data Mongo使用嵌入式文档匹配而不是点符号属性匹配。这样做是为了强迫所有的属性值和嵌入式文档中的属性顺序进行精确的文档匹配。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

Spring Data MongoDB 提供对以下匹配选项的支持。

| Matching                    | Logical result                                              |
| :-------------------------- | :---------------------------------------------------------- |
| `DEFAULT` (区分大小写)      | `{"firstname" : firstname}`                                 |
| `DEFAULT` (不区分大小写)    | `{"firstname" : { $regex: firstname, $options: 'i'}}`       |
| `EXACT` (区分大小写)        | `{"firstname" : { $regex: /^firstname$/}}`                  |
| `EXACT` (不区分大小写)      | `{"firstname" : { $regex: /^firstname$/, $options: 'i'}}`   |
| `STARTING` (区分大小写)     | `{"firstname" : { $regex: /^firstname/}}`                   |
| `STARTING` (不区分大小写)   | `{"firstname" : { $regex: /^firstname/, $options: 'i'}}`    |
| `ENDING` (区分大小写)       | `{"firstname" : { $regex: /firstname$/}}`                   |
| `ENDING` (不区分大小写)     | `{"firstname" : { $regex: /firstname$/, $options: 'i'}}`    |
| `CONTAINING` (区分大小写)   | `{"firstname" : { $regex: /.*firstname.*/}}`                |
| `CONTAINING` (不区分大小写) | `{"firstname" : { $regex: /.*firstname.*/, $options: 'i'}}` |
| `REGEX` (区分大小写)        | `{"firstname" : { $regex: /firstname/}}`                    |
| `REGEX` (不区分大小写)      | `{"firstname" : { $regex: /firstname/, $options: 'i'}}`     |

#### 10.7.6. Untyped Example

默认情况下，`Example` 是严格类型化的。这意味着映射的查询有一个包含的类型匹配，将其限制在可探测的可分配类型。例如，当坚持使用默认的类型键（`_class`）时，查询有如下限制（`_class : { $in : [ com.acme.Person] }`）。

通过使用 `UntypedExampleMatcher`，可以绕过默认行为，跳过类型限制。因此，只要字段名匹配，几乎所有的domain类型都可以作为创建引用的探针，正如下面的例子所示。

Example 97. Untyped Example Query

```java
class JustAnArbitraryClassWithMatchingFieldName {
  @Field("lastname") String value;
}

JustAnArbitraryClassWithMatchingFieldNames probe = new JustAnArbitraryClassWithMatchingFieldNames();
probe.value = "stark";

Example example = Example.of(probe, UntypedExampleMatcher.matching());

Query query = new Query(new Criteria().alike(example));
List<Person> result = template.find(query, Person.class);
```

|      | 如果你在一个集合中存储不同的实体或者选择不写 [类型提示（type hint）](https://springdoc.cn/spring-data-mongodb/#mongo-template.type-mapping)，那么 `UntypedExampleMatcher` 可能是你的正确选择。另外，请记住，使用 `@TypeAlias` 需要对 `MappingContext` 进行急切的初始化。为此，配置 `initialEntitySet`，以确保读取操作的别名解析正确。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 10.8. 统计文档

在SpringData MongoDB的3.x之前的版本中，count操作使用MongoDB的内部集合统计。随着 [MongoDB 事务](https://springdoc.cn/spring-data-mongodb/#mongo.transactions) 的引入，这已不再可能，因为统计数据无法正确反映事务过程中的潜在变化，需要采用基于聚合的count方法。因此，在2.x版本中，如果没有事务正在进行，`MongoOperations.count()` 将使用集合统计数据，如果有，则使用聚合变量。

从Spring Data MongoDB 3.x开始，任何 `count` 操作都会通过MongoDB的 `countDocuments` 使用基于聚合的 `count` 方法，而不考虑过滤条件的存在。如果应用程序对基于集合统计的工作限制没有意见，`MongoOperations.estimatedCount()` 提供了一个替代方案。

|      | 通过将 `MongoTemplate#useEstimatedCount(…)` 设置为 `true`，*MongoTemplate#count(…)* 操作，即使用空的过滤器查询，将被委托给 `estimatedCount`，只要没有事务活动和template没有绑定到 [session](https://springdoc.cn/spring-data-mongodb/#mongo.sessions)。仍然可以通过 `MongoTemplate#exactCount` 获得精确的数字，但可能会加快事情的进展。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | MongoDB的原生 `countDocuments` 方法和 `$match` 聚合，不支持 `$near` 和 `$nearSphere`，但需要 `$geoWithin` 以及不支持 `$minDistance` 的 `$center` 或 `$centerSphere`（见 https://jira.mongodb.org/browse/SERVER-37043）。因此，一个给定的 `Query` 将使用 `Reactive`-/`MongoTemplate` 重写 `count` 操作，以绕过这个问题，如下所示。`{ location : { $near : [-73.99171, 40.738868], $maxDistance : 1.1 } }  { location : { $geoWithin : { $center: [ [-73.99171, 40.738868], 1.1] } } }  { location : { $near : [-73.99171, 40.738868], $minDistance : 0.1, $maxDistance : 1.1 } }  {$and :[ { $nor :[ { location :{ $geoWithin :{ $center :[ [-73.99171, 40.738868 ], 0.01] } } } ]}, { location :{ $geoWithin :{ $center :[ [-73.99171, 40.738868 ], 1.1] } } } ] } `使用 `$near` 进行 Count source 查询。重写的 query 现在使用 `$geoWithin` 与 `$center`。使用 `$near` 与 `$minDistance` 和 `$maxDistance` 进行 Count source 查询。重写的 query 现在是 `$nor` `$geowithin` 标准的组合，以解决不支持 `$minDistance` 的问题。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 10.9. Map-Reduce 操作

你可以通过使用Map-Reduce来查询MongoDB，这对批量处理、数据聚合以及查询语言不能满足你的需求时非常有用。

Spring通过在 `MongoOperations` 上提供方法来简化Map-Reduce操作的创建和运行，提供了与MongoDB的Map-Reduce的集成。它可以将Map-Reduce操作的结果转换为POJO，并与Spring的 [Resource 抽象](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#resources) 集成。 这让你可以将你的JavaScript文件放在文件系统、classpath、HTTP服务器或任何其他Spring资源实现上，然后通过简单的URI风格的语法来引用JavaScript资源—例如， `classpath:reduce.js;`。在文件中外部化JavaScript代码通常比在代码中作为Java字符串嵌入更可取。注意，如果你愿意，你仍然可以将JavaScript代码作为Java字符串传递。

#### 10.9.1. 使用示例

为了了解如何进行Map-Reduce操作，我们使用 *MongoDB - The Definitive Guide* [[1](https://springdoc.cn/spring-data-mongodb/#_footnotedef_1)] 一书中的一个例子。在这个例子中，我们创建了三个文档，其值分别为 [a,b]、[b,c] 和 [c,d]。每个文档中的值都与key "x" 相关，如下例所示（假设这些文档都在一个名为 `jmr1` 的集合中）。

```
{ "_id" : ObjectId("4e5ff893c0277826074ec533"), "x" : [ "a", "b" ] }
{ "_id" : ObjectId("4e5ff893c0277826074ec534"), "x" : [ "b", "c" ] }
{ "_id" : ObjectId("4e5ff893c0277826074ec535"), "x" : [ "c", "d" ] }
```

下面的 map 函数对每个文件的每个字母在数组中的出现情况进行统计。

```java
function () {
    for (var i = 0; i < this.x.length; i++) {
        emit(this.x[i], 1);
    }
}
```

下面的 reduce 函数对所有文件中每个字母的出现次数进行了累加。

```java
function (key, values) {
    var sum = 0;
    for (var i = 0; i < values.length; i++)
        sum += values[i];
    return sum;
}
```

运行前面的函数会得到以下 collection。

```
{ "_id" : "a", "value" : 1 }
{ "_id" : "b", "value" : 2 }
{ "_id" : "c", "value" : 2 }
{ "_id" : "d", "value" : 1 }
```

假设 map 和 reduce 函数位于 `map.js` 和 `reduce.js` 中，并捆绑在你的jar中，因此它们在classpath上是可用的，你可以按如下方式运行Map-Reduce操作。

```java
MapReduceResults<ValueObject> results = mongoOperations.mapReduce("jmr1", "classpath:map.js", "classpath:reduce.js", ValueObject.class);
for (ValueObject valueObject : results) {
  System.out.println(valueObject);
}
```

前面的例子产生了以下输出。

```
ValueObject [id=a, value=1.0]
ValueObject [id=b, value=2.0]
ValueObject [id=c, value=2.0]
ValueObject [id=d, value=1.0]
```

`MapReduceResults` 类实现了 `Iterable`，并提供了对原始输出、时间和计数统计的访问。下面列出了 `ValueObject` 类。

```java
public class ValueObject {

  private String id;
  private float value;

  public String getId() {
    return id;
  }

  public float getValue() {
    return value;
  }

  public void setValue(float value) {
    this.value = value;
  }

  @Override
  public String toString() {
    return "ValueObject [id=" + id + ", value=" + value + "]";
  }
}
```

默认情况下，使用 `INLINE` 的输出类型，因此不需要指定输出集合。要指定额外的Map-Reduce选项，请使用一个重载方法，该方法需要一个额外的 `MapReduceOptions` 参数。 `MapReduceOptions` 类有一个fluent API，所以添加额外的选项可以用紧凑的语法完成。下面的例子将输出集合设置为 `jmr1_out`（注意，只设置输出集合是假设默认输出类型为 `REPLACE`）。

```java
MapReduceResults<ValueObject> results = mongoOperations.mapReduce("jmr1", "classpath:map.js", "classpath:reduce.js",
                                                                     new MapReduceOptions().outputCollection("jmr1_out"), ValueObject.class);
```

还有一个静态导入（`import static org.springframework.data.mongodb.core.mapreduce.MapReduceOptions.options;`），可以用来使语法稍微紧凑一些，如下例所示。

```java
MapReduceResults<ValueObject> results = mongoOperations.mapReduce("jmr1", "classpath:map.js", "classpath:reduce.js",
                                                                     options().outputCollection("jmr1_out"), ValueObject.class);
```

你也可以指定一个 query ，以减少送入Map-Reduce操作的数据集。下面的例子将包含 [a,b] 的文档从 Map-Reduce 操作的考虑中删除。

```java
Query query = new Query(where("x").ne(new String[] { "a", "b" }));
MapReduceResults<ValueObject> results = mongoOperations.mapReduce(query, "jmr1", "classpath:map.js", "classpath:reduce.js",
                                                                     options().outputCollection("jmr1_out"), ValueObject.class);
```

注意，你可以在查询上指定额外的 limit 和 sort 值，但你不能跳过这些值。

### 10.10. 脚本操作

|      | [MongoDB 4.2](https://docs.mongodb.com/master/release-notes/4.2-compatibility/) 删除了对 `ScriptOperations` 所使用的 `eval` 命令的支持。 对于被删除的功能，没有任何替代物。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

MongoDB允许通过直接发送脚本或调用存储的脚本在服务器上运行JavaScript函数。`ScriptOperations` 可以通过 `MongoTemplate` 访问，并为 `JavaScript` 的使用提供基本抽象。下面的例子展示了如何使用 `ScriptOperations` 类。

```java
ScriptOperations scriptOps = template.scriptOps();

ExecutableMongoScript echoScript = new ExecutableMongoScript("function(x) { return x; }");
scriptOps.execute(echoScript, "directly execute script");     

scriptOps.register(new NamedMongoScript("echo", echoScript)); 
scriptOps.call("echo", "execute script via name");            
```

|      | 直接运行脚本，而不在服务器端存储函数。                       |
| ---- | ------------------------------------------------------------ |
|      | 用 'echo' 作为它的名字来存储脚本。给定的名称可以识别该脚本，并允许以后调用它。 |
|      | 使用所提供的参数运行名称为 'echo' 的脚本。                   |

### 10.11. Group 操作

作为使用Map-Reduce进行数据聚合的替代方法，你可以使用 [`group` 操作](https://www.mongodb.org/display/DOCS/Aggregation#Aggregation-Group)，这感觉类似于使用SQL的按查询方式分组，所以与使用Map-Reduce相比，它可能感觉更容易接近。使用分组操作确实有一些限制，例如，在共享环境中不支持分组操作，而且它在一个BSON对象中返回全部结果集，所以结果应该很小，少于10000个键。

Spring通过在 MongoOperations 上提供方法来简化 group 操作的创建和运行，从而提供了与MongoDB的 group 操作的集成。它可以将 group 操作的结果转换为POJO，还可以与Spring的 [Resource 抽象](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#resources) 集成。这将让你把你的JavaScript文件放在文件系统、classpath、http服务器或任何其他Spring资源实现上，然后通过简单的URI风格的语法引用JavaScript资源，例如 `classpath:reduce.js;`。在文件中外部化JavaScript代码通常比在代码中作为Java字符串嵌入要好。请注意，如果你愿意，你仍然可以将JavaScript代码作为Java字符串传递。

#### 10.11.1. 使用示例

为了理解 group 操作是如何工作的，我们使用了下面的例子，这在一定程度上是有意而为之的。对于一个更真实的例子，请参考《MongoDB—权威指南》一书。创建了一个名为 `group_test_collection` 的集合，有以下几条记录。

```
{ "_id" : ObjectId("4ec1d25d41421e2015da64f1"), "x" : 1 }
{ "_id" : ObjectId("4ec1d25d41421e2015da64f2"), "x" : 1 }
{ "_id" : ObjectId("4ec1d25d41421e2015da64f3"), "x" : 2 }
{ "_id" : ObjectId("4ec1d25d41421e2015da64f4"), "x" : 3 }
{ "_id" : ObjectId("4ec1d25d41421e2015da64f5"), "x" : 3 }
{ "_id" : ObjectId("4ec1d25d41421e2015da64f6"), "x" : 3 }
```

我们想按每行中唯一的字段，即 `x` 字段进行分组，并汇总 `x` 的每个特定值出现的次数。要做到这一点，我们需要创建一个初始文件，其中包含我们的count变量和一个reduce函数，该函数将在每次遇到它时增加它。运行分组操作的Java代码如下所示。

```java
GroupByResults<XObject> results = mongoTemplate.group("group_test_collection",
                                                      GroupBy.key("x").initialDocument("{ count: 0 }").reduceFunction("function(doc, prev) { prev.count += 1 }"),
                                                      XObject.class);
```

第一个参数是要运行 group 操作的集合的名称，第二个参数是一个 fluent API，通过 `GroupBy` 类指定 group 操作的属性。在这个例子中，我们只使用 `intialDocument` 和 `reduceFunction` 方法。你也可以指定一个key-function，以及一个 finalizer 作为fluent API的一部分。如果你有多个 key 要 group，你可以传入一个逗号分隔的key列表。

group 操作的原始结果是一个JSON文档，看起来像这样：

```
{
  "retval" : [ { "x" : 1.0 , "count" : 2.0} ,
               { "x" : 2.0 , "count" : 1.0} ,
               { "x" : 3.0 , "count" : 3.0} ] ,
  "count" : 6.0 ,
  "keys" : 3 ,
  "ok" : 1.0
}
```

"retval" 字段下的文档被映射到 group 方法的第三个参数上，在这种情况下是 XObject，如下所示。

```java
public class XObject {

  private float x;

  private float count;


  public float getX() {
    return x;
  }

  public void setX(float x) {
    this.x = x;
  }

  public float getCount() {
    return count;
  }

  public void setCount(float count) {
    this.count = count;
  }

  @Override
  public String toString() {
    return "XObject [x=" + x + " count = " + count + "]";
  }
}
```

你也可以通过调用 `GroupByResults` 类上的 `getRawResults` 方法来获得原始结果作为一个 `Document`。

`MongoOperations` 的 group 方法有一个额外的方法重载，可以让你指定一个 `Criteria` 对象来选择一个行的子集。下面是一个使用 `Criteria` 对象的例子，其中使用了一些静态导入的语法，并通过Spring资源字符串引用了一个 key-function 和 reduce function javascript 文件。

```
import static org.springframework.data.mongodb.core.mapreduce.GroupBy.keyFunction;
import static org.springframework.data.mongodb.core.query.Criteria.where;

GroupByResults<XObject> results = mongoTemplate.group(where("x").gt(0),
                                        "group_test_collection",
                                        keyFunction("classpath:keyFunction.js").initialDocument("{ count: 0 }").reduceFunction("classpath:groupReduce.js"), XObject.class);
```

### 10.12. 聚合（Aggregation）框架的支持

Spring Data MongoDB 为 2.2 版本中引入 MongoDB 的聚合框架提供支持。

更多信息，请参见聚合框架和 MongoDB 的其他数据聚合工具的 [完整参考文档](https://docs.mongodb.org/manual/aggregation/)。

#### 10.12.1. 基本概念

Spring Data MongoDB中的聚合框架支持是基于以下关键抽象。`Aggregation`、`AggregationDefinition` 和 `AggregationResults`。

- `Aggregation`

  `Aggregation` 代表MongoDB `aggregate` 操作，并持有聚合管道（aggregation pipeline ）指令的描述。聚合是通过调用 `Aggregation` 类的适当的 `newAggregation(…)` 静态工厂方法来创建的，该方法接收一个 `AggregateOperation` 的列表和一个可选的输入类。

  实际的aggregate操作是 `由MongoTemplate` 的 `aggregate` 方法运行的，该方法将所需的输出类作为一个参数。

- `TypedAggregation`

  `TypedAggregation`，就像 `Aggregation` 一样，持有聚合管道的指令和对输入类型的引用，用于将domain属性映射到实际文档字段。

  在运行时，字段引用会根据给定的输入类型进行检查，并考虑到潜在的 `@Field` 注解。

在3.2中改变了，引用不存在的属性不会再引发错误。要恢复以前的行为，请使用 `AggregationOptions` 的 `strictMapping` 选项。

- `AggregationDefinition`

  `AggregationDefinition` 代表 MongoDB 聚合管道（aggregation pipeline）操作，并描述在此聚合步骤中应执行的处理。尽管你可以手动创建一个 `AggregationDefinition`，但我们建议使用 `Aggregate` 类提供的静态工厂方法来构建一个 `AggregateOperation`。

- `AggregationResults`

  `AggregationResults` 是聚合操作结果的容器。它提供了对原始聚合结果的访问，其形式是指向映射对象的 `Document` 和关于聚合的其他信息。

  下面的列表显示了使用Spring Data MongoDB支持MongoDB聚合框架的典型示例。

  ```java
  Aggregation agg = newAggregation(
      pipelineOP1(),
      pipelineOP2(),
      pipelineOPn()
  );
  
  AggregationResults<OutputType> results = mongoTemplate.aggregate(agg, "INPUT_COLLECTION_NAME", OutputType.class);
  List<OutputType> mappedResult = results.getMappedResults();
  ```

请注意，如果你提供一个输入类作为 `newAggregation` 方法的第一个参数，`MongoTemplate` 会从这个类派生出输入集合的名称。否则，如果你没有指定一个输入类，你必须明确提供输入集合的名称。如果同时提供了一个输入类和一个输入集合，则后者优先考虑。

#### 10.12.2. 支持的聚合操作

MongoDB聚合框架提供以下类型的聚合操作。

- 管道（Pipeline）聚合操作
- Group/Accumulator 聚合操作。
- Boolean 聚合操作。
- Comparison 聚合操作。
- Arithmetic 聚合操作。
- String 聚合操作。
- Date 聚合操作。
- Array 聚合操作。
- Conditional 聚合操作。
- Lookup 聚合操作。
- Convert 聚合操作。
- Object 聚合操作。
- Script 聚合操作。

在撰写本文时，我们为Spring Data MongoDB的以下聚合操作提供了支持。

| Pipeline 聚合操作。          | `bucket`, `bucketAuto`, `count`, `facet`, `geoNear`, `graphLookup`, `group`, `limit`, `lookup`, `match`, `project`, `rand`, `replaceRoot`, `skip`, `sort`, `unwind` |
| ---------------------------- | ------------------------------------------------------------ |
| Set 聚合操作。               | `setEquals`, `setIntersection`, `setUnion`, `setDifference`, `setIsSubset`, `anyElementTrue`, `allElementsTrue` |
| Group/Accumulator 聚合操作。 | `addToSet`, `bottom`, `bottomN`, `covariancePop`, `covarianceSamp`, `expMovingAvg`, `first`, `firstN`, `last`, `lastN` `max`, `maxN`, `min`, `minN`, `avg`, `push`, `sum`, `top`, `topN`, `count` (*), `stdDevPop`, `stdDevSamp` |
| Arithmetic 聚合操作。        | `abs`, `acos`, `acosh`, `add` (* via `plus`), `asin`, `asin`, `atan`, `atan2`, `atanh`, `ceil`, `cos`, `cosh`, `derivative`, `divide`, `exp`, `floor`, `integral`, `ln`, `log`, `log10`, `mod`, `multiply`, `pow`, `round`, `sqrt`, `subtract` (* via `minus`), `sin`, `sinh`, `tan`, `tanh`, `trunc` |
| String 聚合操作。            | `concat`, `substr`, `toLower`, `toUpper`, `strcasecmp`, `indexOfBytes`, `indexOfCP`, `regexFind`, `regexFindAll`, `regexMatch`, `replaceAll`, `replaceOne`, `split`, `strLenBytes`, `strLenCP`, `substrCP`, `trim`, `ltrim`, `rtim` |
| Comparison 聚合操作。        | `eq` (* via `is`), `gt`, `gte`, `lt`, `lte`, `ne`            |
| Array 聚合操作。             | `arrayElementAt`, `arrayToObject`, `concatArrays`, `filter`, `first`, `in`, `indexOfArray`, `isArray`, `last`, `range`, `reverseArray`, `reduce`, `size`, `sortArray`, `slice`, `zip` |
| Literal Operators            | `literal`                                                    |
| Date 聚合操作。              | `dateSubstract`, `dateTrunc`, `dayOfYear`, `dayOfMonth`, `dayOfWeek`, `year`, `month`, `week`, `hour`, `minute`, `second`, `millisecond`, `dateAdd`, `dateDiff`, `dateToString`, `dateFromString`, `dateFromParts`, `dateToParts`, `isoDayOfWeek`, `isoWeek`, `isoWeekYear`, `tsIncrement`, `tsSecond` |
| Variable Operators           | `map`                                                        |
| Conditional 聚合操作。       | `cond`, `ifNull`, `switch`                                   |
| Type 聚合操作。              | `type`                                                       |
| Convert 聚合操作。           | `convert`, `degreesToRadians`, `toBool`, `toDate`, `toDecimal`, `toDouble`, `toInt`, `toLong`, `toObjectId`, `toString` |
| Object 聚合操作。            | `objectToArray`, `mergeObjects`, `getField`, `setField`      |
| Script 聚合操作。            | `function`, `accumulator`                                    |

\* 该操作是由Spring Data MongoDB映射或添加的。

请注意，Spring Data MongoDB目前不支持这里未列出的聚合操作。Comparison 聚合操作。是以 `Criteria` 表达式来表示的。

|      | 不支持的聚合操作/运算符（如MongoDB Atlas的 [$search](https://www.mongodb.com/docs/atlas/atlas-search/query-syntax/)）可以通过实现 `AggregationOperation` 或 `AggregationExpression` 来提供。 `Aggregation.stage` 是通过提供 JSON 或 `Bson` 表示法来注册 pipeline stage 的快捷方式。`Aggregation.stage("""    { $search : {        "near": {          "path": "released",          "origin": { "$date": { "$numberLong": "..." } } ,          "pivot": 7        }      }    } """); ` |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 10.12.3. 投影表达式

投射表达式用于定义作为特定聚合步骤结果的字段。投影表达式可以通过 `Aggregation` 类的 `project` 方法来定义，可以通过传递一个 `String` 对象的列表或聚合框架 `Fields` 对象。投影可以通过 fluent API，通过使用 `and(String)` 方法和使用 `as(String)` 方法的别名来扩展额外的字段。注意，你也可以通过使用聚合框架的 `Fields.field` 静态工厂方法来定义带有别名的字段，然后你可以用它来构造一个新的 `Fields` 实例。在以后的聚合阶段对投影字段的引用只对包含的字段或其别名（包括新定义的字段及其别名）的字段名有效。未包括在投影中的字段不能在以后的聚合阶段被引用。下面的列表显示了投射表达式的例子。

Example 98. 投影表达的例子

```java
// generates {$project: {name: 1, netPrice: 1}}
project("name", "netPrice")

// generates {$project: {thing1: $thing2}}
project().and("thing1").as("thing2")

// generates {$project: {a: 1, b: 1, thing2: $thing1}}
project("a","b").and("thing1").as("thing2")
```

Example 99. 使用投影和排序的多级聚合

```java
// generates {$project: {name: 1, netPrice: 1}}, {$sort: {name: 1}}
project("name", "netPrice"), sort(ASC, "name")

// generates {$project: {name: $firstname}}, {$sort: {name: 1}}
project().and("firstname").as("name"), sort(ASC, "name")

// does not work
project().and("firstname").as("name"), sort(ASC, "firstname")
```

关于项目操作的更多例子可以在 `AggregationTests` 类中找到。注意，关于投影表达式的进一步细节可以在MongoDB聚合框架参考文档的 [相应部分](https://docs.mongodb.org/manual/reference/operator/aggregation/project/#pipe._S_project) 找到。

#### 10.12.4. 分面分类（faceted classification）

从3.4版本开始，MongoDB通过使用聚合框架支持分面分类。分面分类使用语义类别（一般或特定主题），这些类别被组合起来以创建完整的分类条目。流经聚合管道的文档被分类到桶（bucket）中。多面分类能够对同一组输入文档进行各种聚合，而不需要多次检索输入文档。

##### 桶（Bucket）

桶操作根据指定的表达式和桶的边界，将传入的文档分类到组，称为桶。桶操作需要一个分组字段或一个分组表达式。你可以通过使用 `Aggregate` 类的 `bucket()` 和 `bucketAuto()` 方法来定义它们。`BucketOperation` 和 `BucketAutoOperation` 可以暴露出基于输入文档的聚合表达式的累积。你可以通过使用 `with…()` 方法和 `andOutput(String)` 方法，通过 fluent API用额外的参数扩展桶操作。你可以通过使用 `as(String)` 方法来别名该操作。每个桶在输出中被表示为一个文档。

`BucketOperation` 采用一组定义好的边界，将收到的文档归入这些类别。边界（Boundaries）是需要被排序的。下面列出了一些桶操作的例子。

Example 100. 桶操作示例

```java
// generates {$bucket: {groupBy: $price, boundaries: [0, 100, 400]}}
bucket("price").withBoundaries(0, 100, 400);

// generates {$bucket: {groupBy: $price, default: "Other" boundaries: [0, 100]}}
bucket("price").withBoundaries(0, 100).withDefault("Other");

// generates {$bucket: {groupBy: $price, boundaries: [0, 100], output: { count: { $sum: 1}}}}
bucket("price").withBoundaries(0, 100).andOutputCount().as("count");

// generates {$bucket: {groupBy: $price, boundaries: [0, 100], 5, output: { titles: { $push: "$title"}}}
bucket("price").withBoundaries(0, 100).andOutput("title").push().as("titles");
```

`BucketAutoOperation` 确定边界，试图将文档平均分配到指定数量的桶中。`BucketAutoOperation` 可以选择取一个粒度值，指定使用的 [首选数字](https://en.wikipedia.org/wiki/Preferred_number) 系列，以确保计算出的边界边缘在首选整数或10的幂上结束。下面列出了桶操作的例子。

Example 101. Bucket 操作示例

```java
// generates {$bucketAuto: {groupBy: $price, buckets: 5}}
bucketAuto("price", 5)

// generates {$bucketAuto: {groupBy: $price, buckets: 5, granularity: "E24"}}
bucketAuto("price", 5).withGranularity(Granularities.E24).withDefault("Other");

// generates {$bucketAuto: {groupBy: $price, buckets: 5, output: { titles: { $push: "$title"}}}
bucketAuto("price", 5).andOutput("title").push().as("titles");
```

为了在桶中创建输出字段，桶操作可以通过 `andOutput()` 使用 `AggregationExpression`，通过 `andOutputExpression()` 使用 [SpEL表达](https://springdoc.cn/spring-data-mongodb/#mongo.aggregation.projection.expressions)。

Note that further details regarding bucket expressions can be found in the [`$bucket` section](https://docs.mongodb.org/manual/reference/operator/aggregation/bucket/) and [`$bucketAuto` section](https://docs.mongodb.org/manual/reference/operator/aggregation/bucketAuto/) of the MongoDB Aggregation Framework reference documentation.

##### 多面的聚合

多个聚合管道可用于创建多面聚合，在单一聚合阶段内跨多个维度（或面）描述数据的特征。多面聚合提供多个过滤器和分类，以指导数据浏览和分析。一个常见的面的实现是许多在线零售商通过应用产品价格、制造商、尺寸和其他因素的过滤器来提供缩小搜索结果的方法。

你可以通过使用 `Aggregation` 类的 `facet()` 方法来定义一个 `FacetOperation`。你可以通过使用 `and()` 方法来定制它的多个聚合管道。每个子管道在输出文档中都有自己的字段，其结果以文档数组的形式存储。

子管道可以在分组前对输入文档进行预测和过滤。常见的用例包括在归类前提取日期部分或计算结果。下面列出了分面操作的例子。

Example 102. Facet 操作示例

```java
// generates {$facet: {categorizedByPrice: [ { $match: { price: {$exists : true}}}, { $bucketAuto: {groupBy: $price, buckets: 5}}]}}
facet(match(Criteria.where("price").exists(true)), bucketAuto("price", 5)).as("categorizedByPrice"))

// generates {$facet: {categorizedByCountry: [ { $match: { country: {$exists : true}}}, { $sortByCount: "$country"}]}}
facet(match(Criteria.where("country").exists(true)), sortByCount("country")).as("categorizedByCountry"))

// generates {$facet: {categorizedByYear: [
//     { $project: { title: 1, publicationYear: { $year: "publicationDate"}}},
//     { $bucketAuto: {groupBy: $price, buckets: 5, output: { titles: {$push:"$title"}}}
// ]}}
facet(project("title").and("publicationDate").extractYear().as("publicationYear"),
      bucketAuto("publicationYear", 5).andOutput("title").push().as("titles"))
  .as("categorizedByYear"))
```

请注意，关于 facet 操作的进一步细节可以在MongoDB聚合框架参考文档的 [`$facet` 部分](https://docs.mongodb.org/manual/reference/operator/aggregation/facet/) 找到。

##### 按计数（Count）排序

按计数排序操作根据指定的表达式的值对进入的文档进行分组，计算每个不同组中文档的计数，并按计数对结果进行排序。当使用 [分面分类（faceted classification）](https://springdoc.cn/spring-data-mongodb/#mongo.aggregation.facet) 时，它提供了一个方便的快捷方式来应用排序。按计数排序的操作需要一个分组字段或分组表达式。下面的列表显示了一个按计数排序的例子。

Example 103. 按计数排序示例

```java
// generates { $sortByCount: "$country" }
sortByCount("country");
```

按计数排序的操作等同于以下BSON（二进制JSON）。

```
{ $group: { _id: <expression>, count: { $sum: 1 } } },
{ $sort: { count: -1 } }
```

##### 投射表达式中的Spring表达式支持

我们通过 `ProjectionOperation` 和 `BucketOperation` 类的 `andExpression` 方法支持在投影表达式中使用SpEL表达式。这个功能让你把所需的表达式定义为SpEL表达式。在运行查询时，SpEL表达式被翻译成相应的MongoDB投影表达式部分。这种安排使得表达复杂的计算变得更加容易。

###### 用SpEL表达式进行复杂计算

考虑以下SpEL表达式。

```java
1 + (q + 1) / (q - 1)
```

前面的表达式被翻译成下面的投影表达部分。

```javascript
{ "$add" : [ 1, {
    "$divide" : [ {
        "$add":["$q", 1]}, {
        "$subtract":[ "$q", 1]}
    ]
}]}
```

你可以在 [聚合框架实例5](https://springdoc.cn/spring-data-mongodb/#mongo.aggregation.examples.example5) 和 [聚合框架实例6](https://springdoc.cn/spring-data-mongodb/#mongo.aggregation.examples.example6) 中看到更多的例子。你可以在 `SpelExpressionTransformerUnitTests` 中找到支持的SpEL表达式构造的更多使用示例。下表显示了Spring Data MongoDB所支持的SpEL转换。

| SpEL Expression | Mongo Expression Part    |
| :-------------- | :----------------------- |
| a == b          | { $eq : [$a, $b] }       |
| a != b          | { $ne : [$a , $b] }      |
| a > b           | { $gt : [$a, $b] }       |
| a >= b          | { $gte : [$a, $b] }      |
| a < b           | { $lt : [$a, $b] }       |
| a ⇐ b           | { $lte : [$a, $b] }      |
| a + b           | { $add : [$a, $b] }      |
| a - b           | { $subtract : [$a, $b] } |
| a * b           | { $multiply : [$a, $b] } |
| a / b           | { $divide : [$a, $b] }   |
| a^b             | { $pow : [$a, $b] }      |
| a % b           | { $mod : [$a, $b] }      |
| a && b          | { $and : [$a, $b] }      |
| a \|\| b        | { $or : [$a, $b] }       |
| !a              | { $not : [$a] }          |

除了上表所示的转换之外，你还可以使用标准的SpEL操作，比如 `new` 来（例如）创建数组，并通过它们的名字（后面的括号里是要使用的参数）引用表达式。下面的例子显示了如何以这种方式创建一个数组。

```java
// { $setEquals : [$a, [5, 8, 13] ] }
.andExpression("setEquals(a, new int[]{5, 8, 13})");
```

##### 聚合框架实例

本节中的例子展示了MongoDB聚合框架与Spring Data MongoDB的使用模式。

###### 聚合框架实例1

在这个介绍性的例子中，我们想聚合一个标签列表，从MongoDB集合（称为 `tags`）中获得某个特定标签的出现次数，并按出现次数降序排序。这个例子演示了分组、排序、投影（选择）和分解（结果分割）的用法。

```java
class TagCount {
 String tag;
 int n;
}
Aggregation agg = newAggregation(
    project("tags"),
    unwind("tags"),
    group("tags").count().as("n"),
    project("n").and("tag").previousOperation(),
    sort(DESC, "n")
);

AggregationResults<TagCount> results = mongoTemplate.aggregate(agg, "tags", TagCount.class);
List<TagCount> tagCount = results.getMappedResults();
```

前面的列表使用了以下算法。

1. 通过使用 `newAggregation` 静态工厂方法创建一个新的聚合，我们向其传递一个聚合操作的列表。这些聚合操作定义了我们 `Aggregation` 的聚合管道。
2. 使用 `project` 操作，从输入集合中选择 `tags` 字段（这是一个字符串数组）。
3. 使用 `unwind` 操作，为 `tags` 数组中的每个标签生成一个新的文档。
4. 使用 `group` 操作为每个 `tags` 值定义一个组，我们对这些标签值的出现次数进行聚合（通过使用 `count` 聚合操作符并将结果收集到一个名为 `n` 的新字段中）。
5. 选择 `n` 字段，为前一个 group 操作（因此调用 `previousOperation()`）产生的 ID 字段创建一个别名，名称为 `tag`。
6. 使用 `sort` 操作，按出现次数降序对产生的标签列表进行排序。
7. 调用 `MongoTemplate` 上的 `aggregate` 方法，让MongoDB执行实际的聚合操作，将创建的 `Aggregation` 作为参数。

注意，输入集合被明确指定为 `aggregate` 方法的 `tags` 参数。如果没有明确指定输入集合的名称，它将从作为第一个参数传递给 `newAggreation` 方法的输入类中衍生出来。

###### 聚合框架实例2

这个例子是基于MongoDB聚合框架文档中 [按州划分的最大和最小的城市](https://docs.mongodb.org/manual/tutorial/aggregation-examples/#largest-and-smallest-cities-by-state)例子。我们增加了额外的排序，以便在不同的MongoDB版本中产生稳定的结果。在这里，我们想通过使用聚合框架来返回每个州的按人口计算的最小和最大的城市。这个例子演示了分组、排序和投影（选择）。

```java
class ZipInfo {
   String id;
   String city;
   String state;
   @Field("pop") int population;
   @Field("loc") double[] location;
}

class City {
   String name;
   int population;
}

class ZipInfoStats {
   String id;
   String state;
   City biggestCity;
   City smallestCity;
}
TypedAggregation<ZipInfo> aggregation = newAggregation(ZipInfo.class,
    group("state", "city")
       .sum("population").as("pop"),
    sort(ASC, "pop", "state", "city"),
    group("state")
       .last("city").as("biggestCity")
       .last("pop").as("biggestPop")
       .first("city").as("smallestCity")
       .first("pop").as("smallestPop"),
    project()
       .and("state").previousOperation()
       .and("biggestCity")
          .nested(bind("name", "biggestCity").and("population", "biggestPop"))
       .and("smallestCity")
          .nested(bind("name", "smallestCity").and("population", "smallestPop")),
    sort(ASC, "state")
);

AggregationResults<ZipInfoStats> result = mongoTemplate.aggregate(aggregation, ZipInfoStats.class);
ZipInfoStats firstZipInfoStats = result.getMappedResults().get(0);
```

注意，`ZipInfo` 类映射了给定的输入集合的结构。`ZipInfoStats` 类以所需的输出格式定义了该结构。

前面的列表使用了以下算法。

1. 使用 `group` 操作从输入-集合中定义一个组。分组的标准是 `state` 和 `city` 字段的组合，这形成了组的ID结构。我们通过使用 `sum` 运算从分组的元素中汇总 `population` 属性的值，并将结果保存在 `pop` 字段中。
2. 使用 `sort` 操作，按照 `pop`、`state` 和 `city` 字段对中间结果进行升序排序，使最小的城市位于结果的顶部，最大的城市位于底部。请注意，对 `state` 和 `city` 的排序是隐含地针对 group ID字段进行的（Spring Data MongoDB处理）。
3. 再次使用 `group` 操作，将中间结果按 `state` 分组。请注意，`state` 再次隐含地引用了一个 group ID字段。我们在 `project` 操作中分别调用 `last(…)` 和 `first(…)` 操作符来选择最大和最小的城市（city）的名称和人口数（population count）。
4. 选择前一个 `group` 操作中的 `state` 字段。请注意，`state` 再次隐含地引用了一个 group ID字段。因为我们不希望出现一个隐含生成的ID，所以我们通过使用 `and(previousOperation()).exclude()` 将 ID 从之前的操作中排除。因为我们想在我们的输出类中填充嵌套的 `City` 结构，我们必须通过使用嵌套方法来发出适当的子文档。
5. 在 `sort` 操作中，将得到的 `StateStats` 列表按其州名以升序排序。

注意，我们从作为第一个参数传递给 `newAggregation` 方法的 `ZipInfo` 类派生出输入集合的名称。

###### 聚合框架实例3

这个例子是基于 MongoDB 聚合框架文档中 [人口超过1000万的州](https://docs.mongodb.org/manual/tutorial/aggregation-examples/#states-with-populations-over-10-million) 的例子。我们增加了额外的排序，以便在不同的MongoDB版本中产生稳定的结果。在这里，我们想使用聚合框架返回所有人口超过1000万的州。这个例子演示了分组、排序和匹配（过滤）。

```java
class StateStats {
   @Id String id;
   String state;
   @Field("totalPop") int totalPopulation;
}
TypedAggregation<ZipInfo> agg = newAggregation(ZipInfo.class,
    group("state").sum("population").as("totalPop"),
    sort(ASC, previousOperation(), "totalPop"),
    match(where("totalPop").gte(10 * 1000 * 1000))
);

AggregationResults<StateStats> result = mongoTemplate.aggregate(agg, StateStats.class);
List<StateStats> stateStatsList = result.getMappedResults();
```

前面的列表使用了以下算法。

1. 按 `state` 字段对输入集合进行分组，并计算 `population` 字段的总和，将结果存储在新字段 `"totalPop"` 中。
2. 按照 `"totalPop"` 字段以外的前一个 group 操作的 id-reference 对中间结果进行升序排序。
3. 通过使用接受 `Criteria` query 作为参数的 `match` 操作来过滤中间结果。

注意，我们从作为第一个参数传递给 `newAggregation` 方法的 `ZipInfo` 类中导出了输入集合的名称。

###### 聚合框架实例4

这个例子演示了在投影操作中使用简单的算术运算。

```java
class Product {
    String id;
    String name;
    double netPrice;
    int spaceUnits;
}
TypedAggregation<Product> agg = newAggregation(Product.class,
    project("name", "netPrice")
        .and("netPrice").plus(1).as("netPricePlus1")
        .and("netPrice").minus(1).as("netPriceMinus1")
        .and("netPrice").multiply(1.19).as("grossPrice")
        .and("netPrice").divide(2).as("netPriceDiv2")
        .and("spaceUnits").mod(2).as("spaceUnitsMod2")
);

AggregationResults<Document> result = mongoTemplate.aggregate(agg, Document.class);
List<Document> resultList = result.getMappedResults();
```

请注意，我们从作为第一个参数传递给 `newAggregation` 方法的 `Product` 类中导出了输入集合的名称。

###### 聚合框架实例5

这个例子演示了在投影操作中使用从SpEL表达式衍生出来的简单算术操作。

```java
class Product {
    String id;
    String name;
    double netPrice;
    int spaceUnits;
}
TypedAggregation<Product> agg = newAggregation(Product.class,
    project("name", "netPrice")
        .andExpression("netPrice + 1").as("netPricePlus1")
        .andExpression("netPrice - 1").as("netPriceMinus1")
        .andExpression("netPrice / 2").as("netPriceDiv2")
        .andExpression("netPrice * 1.19").as("grossPrice")
        .andExpression("spaceUnits % 2").as("spaceUnitsMod2")
        .andExpression("(netPrice * 0.8  + 1.2) * 1.19").as("grossPriceIncludingDiscountAndCharge")

);

AggregationResults<Document> result = mongoTemplate.aggregate(agg, Document.class);
List<Document> resultList = result.getMappedResults();
```

###### 聚合框架实例6

这个例子展示了在投影操作中使用来自SpEL表达式的复杂算术操作。

注意：传递给 `addExpression` 方法的额外参数可以根据其位置用索引器表达式来引用。在这个例子中，我们用 `[0]` 引用参数数组的第一个参数。当SpEL表达式被转换为MongoDB聚合框架表达式时，外部参数表达式被替换为其各自的值。

```java
class Product {
    String id;
    String name;
    double netPrice;
    int spaceUnits;
}
double shippingCosts = 1.2;

TypedAggregation<Product> agg = newAggregation(Product.class,
    project("name", "netPrice")
        .andExpression("(netPrice * (1-discountRate)  + [0]) * (1+taxRate)", shippingCosts).as("salesPrice")
);

AggregationResults<Document> result = mongoTemplate.aggregate(agg, Document.class);
List<Document> resultList = result.getMappedResults();
```

请注意，我们也可以在SpEL表达式中引用文件的其他字段。

###### 聚合框架实例7

这个例子使用了条件投影。它源自 [$cond 参考文档](https://docs.mongodb.com/manual/reference/operator/aggregation/cond/)。

```java
public class InventoryItem {

  @Id int id;
  String item;
  String description;
  int qty;
}

public class InventoryItemProjection {

  @Id int id;
  String item;
  String description;
  int qty;
  int discount
}
TypedAggregation<InventoryItem> agg = newAggregation(InventoryItem.class,
  project("item").and("discount")
    .applyCondition(ConditionalOperator.newBuilder().when(Criteria.where("qty").gte(250))
      .then(30)
      .otherwise(20))
    .and(ifNull("description", "Unspecified")).as("description")
);

AggregationResults<InventoryItemProjection> result = mongoTemplate.aggregate(agg, "inventory", InventoryItemProjection.class);
List<InventoryItemProjection> stateStatsList = result.getMappedResults();
```

这个单步汇总使用了 `inventory` 集合的投影操作。我们通过对所有 `qty` 大于或等于 `250` 的库存项目进行条件性操作来预测 `discount` 字段。对 `description` 字段进行了第二个条件预测。我们对所有没有 `description` 字段的项目或描述为 `null` 的项目应用 `Unspecified` 的 description。

从MongoDB 3.6开始，可以通过使用条件表达式将字段从投影中排除。

Example 104. 有条件的聚合投影

```java
TypedAggregation<Book> agg = Aggregation.newAggregation(Book.class,
  project("title")
    .and(ConditionalOperators.when(ComparisonOperators.valueOf("author.middle")     
        .equalToValue(""))                                                          
        .then("$$REMOVE")                                                           
        .otherwiseValueOf("author.middle")                                          
    )
	.as("author.middle"));
```

|      | 如果字段 `author.middle` 的值                                |
| ---- | ------------------------------------------------------------ |
|      | 不包含一个值。                                               |
|      | 然后使用 [`$$REMOVE`](https://docs.mongodb.com/manual/reference/aggregation-variables/#variable.REMOVE) 来排除该字段。 |
|      | 否则，添加 `author.middle` 的字段值。                        |

### 10.13. 索引和 Collection 管理

`MongoTemplate` 为管理索引和集合提供了一些方法。这些方法被收集到一个名为 `IndexOperations` 的辅助接口中。你可以通过调用 `indexOps` 方法并传入集合名称或实体的 `java.lang.Class` 来访问这些操作（集合名称是由 `.class` 派生出来的，可以是名称，也可以是注解元数据）。

下面的列表显示了 `IndexOperations` 接口。

```java
public interface IndexOperations {

  void ensureIndex(IndexDefinition indexDefinition);

  void dropIndex(String name);

  void dropAllIndexes();

  void resetIndexCache();

  List<IndexInfo> getIndexInfo();
}
```

#### 10.13.1. 创建索引的方法

你可以通过使用 `MongoTemplate` 类在一个集合上创建一个索引来提高查询性能，如下例所示。

```java
mongoTemplate.indexOps(Person.class).ensureIndex(new Index().on("name",Order.ASCENDING));
```

`ensureIndex` 确保为所提供的 IndexDefinition 的索引在集合中存在。

你可以通过使用 `IndexDefinition`、`GeoSpatialIndex` 和 `TextIndexDefinition` 类来创建标准、地理空间和文本索引。例如，给定前一节中定义的 `Venue` 类，你可以声明一个地理空间查询，如下例所示。

```java
mongoTemplate.indexOps(Venue.class).ensureIndex(new GeospatialIndex("location"));
```

|      | `Index` 和 `GeospatialIndex` 支持配置 [collation](https://springdoc.cn/spring-data-mongodb/#mongo.collation)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 10.13.2. 获取索引信息

`IndexOperations` 接口有 `getIndexInfo` 方法，该方法返回一个 `IndexInfo` 对象的列表。这个列表包含在集合上定义的所有索引。下面的例子在 `Person` 类上定义了一个索引，该索引有一个 `age` 属性。

```java
template.indexOps(Person.class).ensureIndex(new Index().on("age", Order.DESCENDING).unique());

List<IndexInfo> indexInfoList = template.indexOps(Person.class).getIndexInfo();

// Contains
// [IndexInfo [fieldSpec={_id=ASCENDING}, name=_id_, unique=false, sparse=false],
//  IndexInfo [fieldSpec={age=DESCENDING}, name=age_-1, unique=true, sparse=false]]
```

#### 10.13.3. 使用 Collection 的方法

下面的例子显示了如何创建一个 collection。

Example 105. 通过使用 `MongoTemplate` 来处理collection

```java
MongoCollection<Document> collection = null;
if (!mongoTemplate.getCollectionNames().contains("MyNewCollection")) {
    collection = mongoTemplate.createCollection("MyNewCollection");
}

mongoTemplate.dropCollection("MyNewCollection");
```

- **getCollectionNames**: 返回一组 collection 的名称。
- **collectionExists**: 检查是否存在一个具有给定名称的collection 。
- **createCollection**: 创建一个无上限的collection。
- **dropCollection**: 删除collection.
- **getCollection**: 通过名字获取一个collection，如果它不存在，则创建它。

|      | Collection 创建允许使用 `CollectionOptions` 进行定制，并支持 [collation](https://springdoc.cn/spring-data-mongodb/#mongo.collation).。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 10.14. 运行命令

你可以通过使用 `MongoTemplate` 上的 `executeCommand(…)` 方法来获得MongoDB驱动程序的 `MongoDatabase.runCommand( )` 方法。这些方法还可以将异常转化为Spring的 `DataAccessException` 层次结构。

#### 10.14.1. 运行命令的方法

- `Document` **executeCommand** `(Document command)`: 运行一个MongoDB命令。
- `Document` **executeCommand** `(Document command, ReadPreference readPreference)`: 用给定的可忽略的MongoDB `ReadPreference` 运行一个MongoDB命令。
- `Document` **executeCommand** `(String jsonCommand)`: 运行一个以JSON字符串表示的MongoDB命令。

### 10.15. 生命周期事件

MongoDB映射框架包括几个 `org.springframework.context.ApplicationEvent` 事件，你的应用程序可以通过在 `ApplicationContext` 中注册特殊的Bean来响应。基于Spring的 `ApplicationContext` 事件基础设施，其他产品（如Spring Integration）可以轻松接收这些事件，因为它们是基于Spring的应用程序中众所周知的事件机制。

实体生命周期事件可能是昂贵的，当加载大型结果集时，你可能会注意到性能概况的变化。你可以在 [Template API](https://docs.spring.io/spring-data/mongodb/docs/4.1.0-SNAPSHOT/api/org/springframework/data/mongodb/core/MongoTemplate.html#setEntityLifecycleEventsEnabled(boolean)) 上停用生命周期事件。

要在一个对象通过转换过程（将你的domain对象变成 `org.bson.Document`）之前拦截它，你可以注册一个 `AbstractMongoEventListener` 的子类，重写 `onBeforeConvert` 方法。当事件被派发时，你的监听器被调用，并在domain对象进入转换器之前传递给它。下面的例子展示了如何做到这一点。

```java
public class BeforeConvertListener extends AbstractMongoEventListener<Person> {
  @Override
  public void onBeforeConvert(BeforeConvertEvent<Person> event) {
    ... does some auditing manipulation, set timestamps, whatever ...
  }
}
```

要在对象进入数据库之前对其进行拦截，你可以注册一个 `org.springframework.data.mongodb.core.mapping.event.AbstractMongoEventListener` 的子类，重写 `onBeforeSave` 方法。当事件被派发时，你的监听器被调用并传递给 domain 对象和转换后的 `com.mongodb.Document`。下面的例子展示了如何做到这一点。

```java
public class BeforeSaveListener extends AbstractMongoEventListener<Person> {
  @Override
  public void onBeforeSave(BeforeSaveEvent<Person> event) {
    … change values, delete them, whatever …
  }
}
```

在你的Spring `ApplicationContext` 中声明这些Bean会导致它们在事件被分派时被调用。

`AbstractMappingEventListener` 中存在以下回调方法。

- `onBeforeConvert`: 在 `MongoTemplate` `insert`、`insertList` 和 `save` 操作中，在对象被 `MongoConverter` 转换为 `Document` 之前被调用。
- `onBeforeSave`: 在数据库中插入或保存 `Document` 之前，在 `MongoTemplate` 的 `insert`、 `insertList` 和 `save` 操作中调用。
- `onAfterSave`: 在数据库中插入或保存 `Document` 后，在 `MongoTemplate` `insert`、`insertList` 和 `save` 操作中被调用。
- `onAfterLoad`: 在 `MongoTemplate` 的 `find`、`findAndRemove`、`findOne` 和 `getCollection` 方法中，在 `Document` 被从数据库中检索出来后被调用。
- `onAfterConvert`: 在 `MongoTemplate` 的 `find`、`findAndRemove`、`findOne` 和 `getCollection` 方法中，在 `Document` 从数据库中被检索出来并被转换为POJO后被调用。

|      | 生命周期事件只针对根级别的类型发布。在文档根中作为属性使用的复杂类型不受事件发布的影响，除非它们是用 `@DBRef` 注解的文档引用。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 生命周期事件依赖于 `ApplicationEventMulticaster`，在 `SimpleApplicationEventMulticaster` 的情况下，它可以用 `TaskExecutor` 来配置，因此在事件被处理时没有保证。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 10.16. 实体（Entity）回调

Spring Data基础设施提供了钩子（hooks），用于在某些方法被调用之前和之后修改实体。这些所谓的 `EntityCallback` 实例提供了一种方便的方式，以回调的方式检查和潜在地修改一个实体。 `EntityCallback` 看起来很像一个专门的 `ApplicationListener`。一些Spring Data模块发布了 store 特定事件（如 `BeforeSaveEvent`），允许修改给定的实体。在某些情况下，例如在处理不可变类型时，这些事件会造成麻烦。另外，事件发布依赖于 `ApplicationEventMulticaster`。如果将其与异步 `TaskExecutor` 配置在一起，可能会导致不可预测的结果，因为事件处理可以 fork 到一个线程。

实体回调提供了与同步和响应式API的集成点，以保证在处理链中定义好的检查点上的无序执行，返回可能修改的实体或响应式包装类型。

实体回调通常按API类型分开。这种分离意味着同步API只考虑同步的实体回调，而响应式实现只考虑响应式实体回调。

|      | 实体回调API是由Spring Data Commons 2.2引入的。它是应用实体修改的推荐方式。在调用可能注册的 `EntityCallback` 实例 **之前**，现有的 store 特定的 `ApplicationEvents` 仍然被发布。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 10.16.1. 实现实体回调

`EntityCallback` 通过其泛型类型参数与它的 domain 类型直接相关。每个Spring Data 模块通常都有一组预定义的 `EntityCallback` 接口，涵盖了实体的生命周期。

Example 106. Anatomy of an `EntityCallback`

```java
@FunctionalInterface
public interface BeforeSaveCallback<T> extends EntityCallback<T> {

	/**
	 * Entity callback method invoked before a domain object is saved.
	 * Can return either the same or a modified instance.
	 *
	 * @return the domain object to be persisted.
	 */
	T onBeforeSave(T entity <2>, String collection <3>); 
}
```

|      | `BeforeSaveCallback` 的具体方法，在实体被保存之前被调用。返回一个可能被修改的实例。 |
| ---- | ------------------------------------------------------------ |
|      | 在持久化之前的实体。                                         |
|      | 一些特定的 store 参数，如实体被持久化的集合。                |

Example 107. Anatomy of a reactive `EntityCallback`

```java
@FunctionalInterface
public interface ReactiveBeforeSaveCallback<T> extends EntityCallback<T> {

	/**
	 * Entity callback method invoked on subscription, before a domain object is saved.
	 * The returned Publisher can emit either the same or a modified instance.
	 *
	 * @return Publisher emitting the domain object to be persisted.
	 */
	Publisher<T> onBeforeSave(T entity <2>, String collection <3>); 
}
```

|      | `BeforeSaveCallback` 特定方法，在实体被保存之前被调用。发出一个可能被修改的实例。 |
| ---- | ------------------------------------------------------------ |
|      | 在持久化之前的实体。                                         |
|      | 一些特定的 store 参数，如实体被持久化的集合。                |

|      | 可选的实体回调参数由实现Spring Data模块定义，并从 `EntityCallback.callback()` 的调用点推断出来。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

实现适合你的应用需求的接口，如下面的例子所示。

Example 108. Example `BeforeSaveCallback`

```java
class DefaultingEntityCallback implements BeforeSaveCallback<Person>, Ordered {      

	@Override
	public Object onBeforeSave(Person entity, String collection) {                   

		if(collection == "user") {
		    return // ...
		}

		return // ...
	}

	@Override
	public int getOrder() {
		return 100;                                                                  
	}
}
```

|      | 根据你的要求实现回调。                                       |
| ---- | ------------------------------------------------------------ |
|      | 如果同一 domain 类型存在多个实体回调，则可能对其进行排序。排序遵循最低优先级。 |

#### 10.16.2. 注册实体回调

`EntityCallback` Bean 在 `ApplicationContext` 中注册的情况下被 store 的具体实现所接收。大多数模板API已经实现了 `ApplicationContextAware`，因此可以访问 `ApplicationContext`。

下面的例子解释了一个有效的实体回调注册的集合。

Example 109. Example `EntityCallback` Bean registration

```java
@Order(1)                                                           
@Component
class First implements BeforeSaveCallback<Person> {

	@Override
	public Person onBeforeSave(Person person) {
		return // ...
	}
}

@Component
class DefaultingEntityCallback implements BeforeSaveCallback<Person>,
                                                           Ordered { 

	@Override
	public Object onBeforeSave(Person entity, String collection) {
		// ...
	}

	@Override
	public int getOrder() {
		return 100;                                                  
	}
}

@Configuration
public class EntityCallbackConfiguration {

    @Bean
    BeforeSaveCallback<Person> unorderedLambdaReceiverCallback() {   
        return (BeforeSaveCallback<Person>) it -> // ...
    }
}

@Component
class UserCallbacks implements BeforeConvertCallback<User>,
                                        BeforeSaveCallback<User> {   

	@Override
	public Person onBeforeConvert(User user) {
		return // ...
	}

	@Override
	public Person onBeforeSave(User user) {
		return // ...
	}
}
```

|      | `BeforeSaveCallback` 从 `@Order` 注解中接收其顺序。          |
| ---- | ------------------------------------------------------------ |
|      | `BeforeSaveCallback` 通过 `Ordered` 接口的实现接收其顺序。   |
|      | `BeforeSaveCallback` 使用一个 lambda 表达式。默认情况下是无序的，最后调用。请注意，由lambda表达式实现的回调不暴露类型信息，因此用一个不可分配的实体调用这些回调会影响回调的吞吐量。使用一个 `class` 或 `enum` 来启用回调Bean的类型过滤。 |
|      | 将多个实体回调接口合并到一个实现类中。                       |

#### 10.16.3. Store 特定的实体回调（EntityCallback）

Spring Data MongoDB使用 `EntityCallback` API来支持审计，并对以下回调作出反应。

| 回调                            | 方法                                                         | 说明                                                         | 顺序                        |
| :------------------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- | :-------------------------- |
| Reactive/BeforeConvertCallback  | `onBeforeConvert(T entity, String collection)`               | 在 domain 对象被转换为 `org.bson.Document` 之前调用。        | `Ordered.LOWEST_PRECEDENCE` |
| Reactive/AfterConvertCallback   | `onAfterConvert(T entity, org.bson.Document target, String collection)` | 在 domain 对象被加载后调用。 可以在从 `org.bson.Document` 中读取 domain 对象后修改该对象。 | `Ordered.LOWEST_PRECEDENCE` |
| Reactive/AuditingEntityCallback | `onBeforeConvert(Object entity, String collection)`          | 标志着一个可审计的实体被创建或修改                           | 100                         |
| Reactive/BeforeSaveCallback     | `onBeforeSave(T entity, org.bson.Document target, String collection)` | 在 domain 对象被保存之前调用。 可以修改目标，要持久化的，包含所有映射的实体信息的 `Document`。 | `Ordered.LOWEST_PRECEDENCE` |
| Reactive/AfterSaveCallback      | `onAfterSave(T entity, org.bson.Document target, String collection)` | 在 domain 对象被保存之前调用。 可以修改目标，要持久化的，包含所有映射的实体信息的 `Document`。 | `Ordered.LOWEST_PRECEDENCE` |

### 10.17. 异常（Exception）转换

Spring框架为各种各样的数据库和映射技术提供了异常转换。这在传统上是针对JDBC和JPA的。Spring对MongoDB的支持通过提供 `org.springframework.dao.support.PersistenceExceptionTranslator` 接口的实现，将该功能扩展到了MongoDB数据库。

映射到 [Spring一致的数据访问异常层次结构](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#dao-exceptions) 背后的动机是，你能够编写可移植的、描述性的异常处理代码，而不需要借助于针对MongoDB错误代码的编码。Spring的所有数据访问异常都是从根 `DataAccessException` 类中继承的，因此你可以确保在一个 try-catch 块中捕捉所有与数据库相关的异常。请注意，并非所有由MongoDB驱动抛出的异常都继承自 `MongoException` 类。内部的异常和消息被保留下来，这样就不会丢失任何信息。

`MongoExceptionTranslator` 执行的一些映射是 `com.mongodb.Network` 到 `DataAccessResourceFailureException` 和 `MongoException` 错误代码1003、1201、12010、12011和12012到 `InvalidDataAccessApiUsageException`。查看实现，了解更多关于映射的细节。

### 10.18. execute 回调

所有Spring template 类的一个共同设计特点是，所有的功能都被路由到模板的一个 `execute` 回调方法中。这样做有助于确保异常和任何可能需要的资源管理都得到一致执行。虽然JDBC和JMS比MongoDB更需要这个功能，但它仍然提供了一个发生异常转换和记录的单一地点。因此，使用这些 `execute` 回调是访问 MongoDB 驱动程序的 `MongoDatabase` 和 `MongoCollection` 对象的首选方式，以执行 `MongoTemplate` 上没有暴露为方法的不常见操作。

下面的列表描述了 execute 回调方法。

- `<T> T` **execute** `(Class<?> entityClass, CollectionCallback<T> action)`: 为指定类别的实体 collection 运行给定的 `CollectionCallback`。
- `<T> T` **execute** `(String collectionName, CollectionCallback<T> action)`: 在给定名称的 collection 上运行给定的 `CollectionCallback`。
- `<T> T` **execute** `(DbCallback<T> action)`: 运行一个 `DbCallback`，必要时翻译任何异常。Spring Data MongoDB为2.2版本中引入MongoDB的聚合框架提供支持。
- `<T> T` **execute** `(String collectionName, DbCallback<T> action)`: 在给定名称的集合上运行一个 `DbCallback`，必要时翻译任何异常。
- `<T> T` **executeInSession** `(DbCallback<T> action)`: 在与数据库的同一连接中运行给定的 `DbCallback`，以确保在一个“多写”的环境中的一致性，你可能会读取你写的数据。

下面的例子使用 `CollectionCallback` 来返回关于一个索引的信息。

```java
boolean hasIndex = template.execute("geolocation", new CollectionCallbackBoolean>() {
  public Boolean doInCollection(Venue.class, DBCollection collection) throws MongoException, DataAccessException {
    List<Document> indexes = collection.getIndexInfo();
    for (Document document : indexes) {
      if ("location_2d".equals(document.get("name"))) {
        return true;
      }
    }
    return false;
  }
});
```

### 10.19. 支持 GridFS

MongoDB支持在其文件系统GridFS中存储二进制文件。Spring Data MongoDB提供了一个 `GridFsOperations` 接口以及相应的实现，即 `GridFsTemplate`，让你与文件系统互动。你可以通过给它一个 `MongoDatabaseFactory` 以及一个 `MongoConverter` 来设置一个 `GridFsTemplate` 实例，如下例所示。

Java

XML

```java
class GridFsConfiguration extends AbstractMongoClientConfiguration {

  // … further configuration omitted

  @Bean
  public GridFsTemplate gridFsTemplate() {
    return new GridFsTemplate(mongoDbFactory(), mappingMongoConverter());
  }
}
```

现在，该 template 可以被注入并用于执行存储和检索操作，如下例所示。

Example 110. 使用 GridFsTemplate 来存储文件

```java
class GridFsClient {

  @Autowired
  GridFsOperations operations;

  @Test
  public void storeFileToGridFs() {

    FileMetadata metadata = new FileMetadata();
    // populate metadata
    Resource file = … // lookup File or Resource

    operations.store(file.getInputStream(), "filename.txt", metadata);
  }
}
```

`store(…)` 操作接收一个 `InputStream`、一个文件名和（可选择的）关于要存储的文件的元数据信息。元数据可以是一个任意的对象，它将被配置在 `GridFsTemplate` 中的 `MongoConverter` 处理。另外，你也可以提供一个 `Document`。

你可以通过 `find(…)` 或 `getResources(…)` 方法从文件系统中读取文件。让我们先看一下 `find(…)` 方法。你可以找到一个文件，也可以找到符合 `Query` 条件的多个文件。你可以使用 `GridFsCriteria` helper 类来定义查询。它提供了静态的工厂方法来封装默认的元数据字段（比如 `whereFilename()` 和 `whereContentType()`），或者通过 `whereMetaData()` 封装一个自定义的元数据。下面的例子显示了如何使用 `GridFsTemplate` 来查询文件。

Example 111. 使用 GridFsTemplate 来查询文件

```java
class GridFsClient {

  @Autowired
  GridFsOperations operations;

  @Test
  public void findFilesInGridFs() {
    GridFSFindIterable result = operations.find(query(whereFilename().is("filename.txt")))
  }
}
```

|      | 目前，MongoDB 不支持在从 GridFS 检索文件时定义排序标准。出于这个原因，任何定义在 `Query` 实例上的排序标准都会被忽略，而这些排序标准会被交给 `find(…)` 方法来处理。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

从 GridFs 读取文件的另一个选择是使用 `ResourcePatternResolver` 接口引入的方法。它们允许在方法中输入 Ant 路径，因此可以检索到与给定 pattern 相匹配的文件。下面的例子显示了如何使用 `GridFsTemplate` 来读取文件。

Example 112. 使用 GridFsTemplate 来读取文件

```java
class GridFsClient {

  @Autowired
  GridFsOperations operations;

  @Test
  public void readFilesFromGridFs() {
    GridFsResources[] txtFiles = operations.getResources("*.txt");
  }
}
```

`GridFsOperations` 继承了 `ResourcePatternResolver`，并允许将 `GridFsTemplate`（例如）插入 `ApplicationContext`，以从 MongoDB 数据库读取Spring配置文件。

### 10.20. 具有可追踪游标的无限流

默认情况下，当客户端用完游标提供的所有结果时，MongoDB会自动关闭游标。在耗尽时关闭游标会使流变成一个有限的流。对于 [有上限的集合](https://docs.mongodb.com/manual/core/capped-collections/)，你可以使用一个 [Tailable Cursor](https://docs.mongodb.com/manual/core/tailable-cursors/)，它在客户端消耗了所有最初返回的数据后仍然保持开放。

|      | 可以用 `MongoOperations.createCollection` 来创建有上限的集合。要做到这一点，提供所需的 `CollectionOptions.empty().capped()`…. |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

可跟踪的游标可以通过强制性的和反应性的MongoDB API来使用。强烈建议使用响应式变体，因为它的资源密集度较低。然而，如果你不能使用响应式API，你仍然可以使用Spring生态系统中已经普遍存在的消息传递概念。

#### 10.20.1. 使用 `MessageListener` 的可追踪游标

使用同步驱动监听一个有上限的集合会产生一个长期运行的、阻塞的任务，需要委托给一个单独的组件。在这种情况下，我们需要首先创建一个 `MessageListenerContainer`，它将是运行特定的 `SubscriptionRequest` 的主要入口点。Spring Data MongoDB 已经提供了一个默认的实现，它在 `MongoTemplate` 上运行，能够为 `TailableCursorRequest` 创建和运行 `Task` 实例。

下面的例子显示了如何用 `MessageListener` 实例来使用可跟踪的游标。

Example 113. 具有 `MessageListener` 实例的可跟踪游标

```java
MessageListenerContainer container = new DefaultMessageListenerContainer(template);
container.start();                                                                  

MessageListener<Document, User> listener = System.out::println;                     

TailableCursorRequest request = TailableCursorRequest.builder()
  .collection("orders")                                                             
  .filter(query(where("value").lt(100)))                                            
  .publishTo(listener)                                                              
  .build();

container.register(request, User.class);                                            

// ...

container.stop();                                                                   
```

|      | 启动容器会启动资源并为已经注册的 `SubscriptionRequest` 实例启动 `Task` 实例。启动后添加的请求会立即运行。 |
| ---- | ------------------------------------------------------------ |
|      | 定义当收到一个 `Message` 时被调用的监听器。`Message#getBody()` 被转换为请求的 domain 类型。使用 `Document` 来接收未经转换的原始结果。 |
|      | 设置要听的集合。                                             |
|      | 为要接收的文档提供一个可选的过滤器。                         |
|      | 设置消息监听器来发布传入的 `Message`。                       |
|      | 注册该请求。返回的 `Subscription` 可以用来检查当前的 `Task` 状态，并取消它以释放资源。 |
|      | 一旦你确定你不再需要这个容器，不要忘记停止它。这样做可以停止容器内所有正在运行的 `Task` 实例。 |

#### 10.20.2. 响应式可跟踪游标

使用具有响应式数据类型的可跟踪游标可以构建无限的流。一个可跟踪的游标保持开放，直到它被外部关闭。当新的文档到达一个封顶的集合时，它就会释放出数据。

如果查询没有返回匹配，或者游标在集合的 "末端" 返回文档，而应用程序随后删除了该文档，那么可跟踪的游标可能会变成死的，或无效的。下面的例子显示了如何创建和使用一个无限流查询。

Example 114. 使用 ReactiveMongoOperations 的无限流查询

```java
Flux<Person> stream = template.tail(query(where("name").is("Joe")), Person.class);

Disposable subscription = stream.doOnNext(person -> System.out.println(person)).subscribe();

// …

// Later: Dispose the subscription to close the stream
subscription.dispose();
```

Spring Data MongoDB 响应式 repository 通过用 `@Tailable` 注解查询方法来支持无限的流。这适用于返回 `Flux` 和其他能够发出多个元素的响应式类型的方法，如下例所示。

Example 115. 使用 ReactiveMongoRepository 的无限流查询

```java
public interface PersonRepository extends ReactiveMongoRepository<Person, String> {

  @Tailable
  Flux<Person> findByFirstname(String firstname);

}

Flux<Person> stream = repository.findByFirstname("Joe");

Disposable subscription = stream.doOnNext(System.out::println).subscribe();

// …

// Later: Dispose the subscription to close the stream
subscription.dispose();
```

### 10.21. 变更流（Change Streams）

从 MongoDB 3.6 开始， [Change Streams](https://docs.mongodb.com/manual/changeStreams/) 可以让应用程序获得有关变化的通知，而不需要跟踪oplog。

|      | 变更流支持只适用于复制集或分片集群。 |
| ---- | ------------------------------------ |
|      |                                      |

Change Streams 可以用命令式和响应式的MongoDB Java驱动来消费。强烈建议使用响应式变体，因为它的资源密集度较低。然而，如果你不能使用响应式API，你仍然可以通过使用Spring生态系统中已经普遍存在的消息传递概念来获取变更事件。

可以在集合以及数据库级别上观察，而数据库级别的变体会发布来自数据库内所有集合的变化。当订阅数据库变化流时，确保为事件类型使用一个合适的类型，因为转换可能不会正确适用于不同的实体类型。如有疑问，请使用 `Document`。

#### 10.21.1. 用 `MessageListener` 改变数据流

通过 [使用同步驱动来监听变化流](https://docs.mongodb.com/manual/tutorial/change-streams-example/) 会产生一个长期运行的、阻塞的任务，需要委托给一个单独的组件。在这种情况下，我们需要首先创建一个 `MessageListenerContainer`，它将是运行特定的 `SubscriptionRequest` 任务的主要入口点。Spring Data MongoDB已经提供了一个默认的实现，它在 `MongoTemplate` 上运行，能够为 `ChangeStreamRequest` 创建和运行 `Task` 实例。

下面的例子显示了如何用 `MessageListener` 实例来使用Change Streams。

Example 116. 用 `MessageListener` 实例改变信息流

```java
MessageListenerContainer container = new DefaultMessageListenerContainer(template);
container.start();                                                                                        

MessageListener<ChangeStreamDocument<Document>, User> listener = System.out::println;                     
ChangeStreamRequestOptions options = new ChangeStreamRequestOptions("user", ChangeStreamOptions.empty()); 

Subscription subscription = container.register(new ChangeStreamRequest<>(listener, options), User.class); 

// ...

container.stop();                                                                                         
```

|      | 启动容器会初始化资源并为已经注册的 `SubscriptionRequest` 实例启动 `Task` 实例。启动后添加的请求会立即运行。 |
| ---- | ------------------------------------------------------------ |
|      | 定义当收到一个 `Message` 时被调用的监听器。`Message#getBody()` 被转换为请求的 domain 类型。使用 `Document` 来接收未经转换的原始结果。 |
|      | 设置要监听的集合，并通过 `ChangeStreamOptions` 提供附加选项。 |
|      | 注册该请求。返回的 `Subscription` 可以用来检查当前的 `Task` 状态，并取消它以释放资源。 |
|      | 一旦你确定你不再需要这个容器，不要忘记停止它。这样做可以停止容器内所有正在运行的 `Task` 实例。 |

|      | 处理过程中的错误会传递给 `org.springframework.util.ErrorHandler`。如果没有另外说明，默认情况下会应用日志追加的 `ErrorHandler`。 请使用 `register(request, body, errorHandler)` 来提供额外的功能。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 10.21.2. 响应式变更流（Change Streams）

用响应式API订阅Change Streams是一种更自然的方法来处理流的工作。尽管如此，基本的构建模块，如 `ChangeStreamOptions`，仍然是相同的。下面的例子展示了如何使用发射 `ChangeStreamEvent` 的Change Streams。

Example 117. 变化流发出 `ChangeStreamEvent`.

```java
Flux<ChangeStreamEvent<User>> flux = reactiveTemplate.changeStream(User.class) 
    .watchCollection("people")
    .filter(where("age").gte(38))                                              
    .listen();                                                                 
```

|      | 底层文档应该被转换为的事件目标类型。留出这一点是为了接收未经转换的原始结果。 |
| ---- | ------------------------------------------------------------ |
|      | 使用一个聚合管道或只是一个查询 `Criteria` 来过滤事件。       |
|      | 获得一个变化流事件的 `Flux`。`ChangeStreamEvent#getBody()` 被转换为(2)中要求的 domain 类型。 |

#### 10.21.3. 恢复变更流（Change Streams）

Change Streams 可以被恢复，并在你离开的地方恢复发射事件。要恢复流，你需要提供一个恢复令牌或最后已知的服务器时间（UTC）。使用 `ChangeStreamOptions` 来设置相应的值。

下面的例子显示了如何使用服务器时间设置恢复偏移。

Example 118. 恢复一个 Change Stream

```java
Flux<ChangeStreamEvent<User>> resumed = template.changeStream(User.class)
    .watchCollection("people")
    .resumeAt(Instant.now().minusSeconds(1)) 
    .listen();
```

|      | 你可以通过 `getTimestamp` 方法获得 `ChangeStreamEvent` 的服务器时间，或者使用通过 `getResumeToken` 暴露的 `resumeToken`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 在某些情况下，当恢复一个变化流时，`Instant` 可能不是一个足够精确的措施。为此，请使用MongoDB原生的 [BsonTimestamp](https://docs.mongodb.com/manual/reference/bson-types/#timestamps)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 10.22. 时间序列

MongoDB 5.0引入了 [时序](https://docs.mongodb.com/manual/core/timeseries-collections/) 集合，该集合被优化为有效地存储随时间变化的文档，如测量或事件。在插入任何数据之前，这些集合需要被创建成这样。可以通过运行 `createCollection` 命令、定义时序集合选项或从 `@TimeSeries` 注解中提取选项来创建集合，如下面的例子所示。

Example 119. 创建一个时序集合

通过MongoDB驱动创建一个时序

```
template.execute(db -> {

    com.mongodb.client.model.CreateCollectionOptions options = new CreateCollectionOptions();
    options.timeSeriesOptions(new TimeSeriesOptions("timestamp"));

    db.createCollection("weather", options);
    return "OK";
});
```

用 `CollectionOptions` 创建一个时间序列集合

```
template.createCollection("weather", CollectionOptions.timeSeries("timestamp"));
```

创建一个源自注解的时序集合

```
@TimeSeries(collection="weather", timeField = "timestamp")
public class Measurement {

    String id;
    Instant timestamp;
    // ...
}

template.createCollection(Measurement.class);
```

上面的片段可以很容易地转移到提供同样方法的响应式API。请确保正确 *subscribe* 返回的发布者（publisher）。

### 10.23. 可观察性（Observability）

Spring Data MongoDB目前拥有最新的代码，以支持MongoDB应用程序中的可观察性。 然而，这些变化还没有被Spring Boot所接受。 在这些变化被应用之前，如果你希望使用 Spring Data MongoDB 的 Observability 特性，你必须执行以下步骤。

1. 首先，你必须选择进入Spring Data MongoDB的 configuration 设置，通过你的 `@SpringBootApplication` 类或你的某个配置类定制 `MongoClientSettings`。

   Example 120. 注册 MongoDB Micrometer customizer 设置

   ```java
   @Bean
   MongoClientSettingsBuilderCustomizer mongoMetricsSynchronousContextProvider(ObservationRegistry registry) {
       return (clientSettingsBuilder) -> {
           clientSettingsBuilder.contextProvider(ContextProviderFactory.create(registry))
                                .addCommandListener(new MongoObservationCommandListener(registry));
       };
   }
   ```

2. 你的项目必须包括 **Spring Boot Actuator**。

3. 禁用Spring Boot自动配置的MongoDB命令监听器，并通过在 `application.properties` 中添加以下属性，手动启用跟踪。

   Example 121. 应用的自定义设置

   ```
   # Disable Spring Boot's autoconfigured tracing
   management.metrics.mongo.command.enabled=false
   # Enable it manually
   management.tracing.enabled=true
   ```

   请确保根据Micrometer的参考文档，添加配置你所使用的追踪器所需的任何其他相关设置。

这应该就可以了! 你现在正在使用 Spring Data MongoDB 对 Spring Observability 的 `Observation` API的使用。

Unresolved directive in reference/observability.adoc - include::../../../../target/_conventions.adoc[]

Unresolved directive in reference/observability.adoc - include::../../../../target/_metrics.adoc[]

Unresolved directive in reference/observability.adoc - include::../../../../target/_spans.adoc[]

请参阅 [OpenTelemetry语义公约](https://opentelemetry.io/docs/reference/specification/trace/semantic_conventions/database/#mongodb) 以进一步参考。

## 11. MongoDB 会话（Session）

从3.6版本开始，MongoDB支持会话的概念。会话的使用实现了MongoDB的 [Causal Consistency（因果一致性）](https://docs.mongodb.com/manual/core/read-isolation-consistency-recency/#causal-consistency) 模型，它保证了以尊重其因果关系的顺序运行操作。这些会话被分成 `ServerSession` 实例和 `ClientSession` 实例。在本节中，当我们谈到会话时，我们指的是 `ClientSession`。

|      | 客户端会话内的操作与会话外的操作没有隔离。 |
| ---- | ------------------------------------------ |
|      |                                            |

`MongoOperations` 和 `ReactiveMongoOperations` 都提供了将 `ClientSession` 绑定到操作上的网关方法。`MongoCollection` 和 `MongoDatabase` 使用实现 MongoDB 的集合和数据库接口的会话代理对象，因此你不需要在每次调用时添加会话。这意味着，对 `MongoCollection#find()` 的潜在调用被委托给 `MongoCollection#find(ClientSession)`。

|      | `(Reactive)MongoOperations#getCollection` 等方法返回原生 MongoDB Java Driver网关对象（如 `MongoCollection`），这些对象本身为 `ClientSession` 提供专用方法。这些方法不是会话代理的。在与 `MongoCollection` 或 `MongoDatabase` 直接交互时，你应该在需要时提供 `ClientSession`，而不是通过 `MongoOperations` 上的 `#execute` 回调。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 11.1. 支持同步的 `ClientSession`

下面的例子显示了一个会话的用法。

Example 122. `ClientSession` 和 `MongoOperations`

```java
ClientSessionOptions sessionOptions = ClientSessionOptions.builder()
    .causallyConsistent(true)
    .build();

ClientSession session = client.startSession(sessionOptions); 

template.withSession(() -> session)
    .execute(action -> {

        Query query = query(where("name").is("Durzo Blint"));
        Person durzo = action.findOne(query, Person.class);  

        Person azoth = new Person("Kylar Stern");
        azoth.setMaster(durzo);

        action.insert(azoth);                                

        return azoth;
    });

session.close()                                              
```

|      | 从服务器获取一个新的会话。                                   |
| ---- | ------------------------------------------------------------ |
|      | 像以前一样使用 `MongoOperation` 方法。`ClientSession` 会被自动应用。 |
|      | 确保关闭 `ClientSession`.                                    |
|      | 关闭 session.                                                |

|      | 当处理 `DBRef` 实例时，特别是懒加载的实例，在所有数据被加载之前不要关闭 `ClientSession` 是非常重要的。否则，懒加载会失败。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 11.2. 支持响应式的 `ClientSession`

如下面的例子所示，响应式的对应方法使用与命令式相同的构件。

Example 123. ClientSession with `ReactiveMongoOperations`

```java
ClientSessionOptions sessionOptions = ClientSessionOptions.builder()
    .causallyConsistent(true)
    .build();

Publisher<ClientSession> session = client.startSession(sessionOptions); 

template.withSession(session)
    .execute(action -> {

        Query query = query(where("name").is("Durzo Blint"));
        return action.findOne(query, Person.class)
            .flatMap(durzo -> {

                Person azoth = new Person("Kylar Stern");
                azoth.setMaster(durzo);

                return action.insert(azoth);                            
            });
    }, ClientSession::close)                                            
    .subscribe();                                                       
```

|      | 为新的会话检索获得一个 `Publisher` 。                        |
| ---- | ------------------------------------------------------------ |
|      | 像以前一样使用 `ReactiveMongoOperation` 方法。自动获得并应用 `ClientSession`。 |
|      | 确保关闭 `ClientSession`.                                    |
|      | 在你订阅之前什么都不会发生。详见 [《Project Reactor参考指南》](https://projectreactor.io/docs/core/release/reference/#reactive.subscribe)。 |

通过使用一个提供实际会话的 `Publisher`，你可以将会话的获取推迟到实际订阅的时候。但是，你仍然需要在完成后关闭会话，这样就不会让旧的会话污染服务器。当你不再需要会话时，使用 `execute` 上的 `doFinally` 钩子来调用 `ClientSession#close()`。如果你喜欢对会话本身有更多的控制，你可以通过驱动获得 `ClientSession`，并通过 `Supplier` 来提供它。

|      | `ClientSession` 的响应式使用仅限于 Template API 的使用。目前还没有与响应式 repository 的会话集成。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

## 12. MongoDB 事务

从版本4开始，MongoDB支持 [事务](https://www.mongodb.com/transactions)。事务是建立在 [会话](https://springdoc.cn/spring-data-mongodb/#mongo.sessions)之上的，因此，需要一个活跃的 `ClientSession`。

|      | 除非你在你的应用程序上下文中指定一个 `MongoTransactionManager`，否则事务支持是 **DISABLED**（禁用的）。你可以使用 `setSessionSynchronization(ALWAYS)` 来参与正在进行的非本地 MongoDB 事务。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

为了获得对事务的完全程序化控制，你可能想在 `MongoOperations` 上使用会话回调。

下面的例子显示了在一个 `SessionCallback` 中的程序化事务控制。

Example 124. 程序性事务

```java
ClientSession session = client.startSession(options);                   

template.withSession(session)
    .execute(action -> {

        session.startTransaction();                                     

        try {

            Step step = // ...;
            action.insert(step);

            process(step);

            action.update(Step.class).apply(Update.set("state", // ...

            session.commitTransaction();                                

        } catch (RuntimeException e) {
            session.abortTransaction();                                 
        }
    }, ClientSession::close)                                            
```

|      | 获得一个新的 `ClientSession`。   |
| ---- | -------------------------------- |
|      | 开始事务。                       |
|      | 如果一切按预期进行，就提交修改。 |
|      | 出现意外，所以要回滚一切。       |
|      | 完成后不要忘记关闭会话。         |

前面的例子让你完全控制事务行为，同时在回调中使用会话范围的 `MongoOperations` 实例，以确保会话被传递给每个服务器调用。为了避免这种方法带来的一些开销，你可以使用 `TransactionTemplate` 来消除手动事务流的一些噪音。

### 12.1. 事务和 `TransactionTemplate`

Spring Data MongoDB事务支持一个 `TransactionTemplate`。下面的例子展示了如何创建和使用 `TransactionTemplate`。

Example 125. 事务和 `TransactionTemplate`

```java
template.setSessionSynchronization(ALWAYS);                                     

// ...

TransactionTemplate txTemplate = new TransactionTemplate(anyTxManager);         

txTemplate.execute(new TransactionCallbackWithoutResult() {

    @Override
    protected void doInTransactionWithoutResult(TransactionStatus status) {     

        Step step = // ...;
        template.insert(step);

        process(step);

        template.update(Step.class).apply(Update.set("state", // ...
    };
});
```

|      | 在 Template API 配置中启用事务同步。                         |
| ---- | ------------------------------------------------------------ |
|      | 使用提供的 `PlatformTransactionManager` 创建 `TransactionTemplate`。 |
|      | 在回调中，`ClientSession` 和事务已经被注册。                 |

在运行期间改变 `MongoTemplate` 的状态（就像你可能认为在前面列表的第1项中可能发生的那样）会导致线程和可见性问题。

### 12.2. 事务和 `MongoTransactionManager`

`MongoTransactionManager` 是通往众所周知的Spring事务支持的网关。它可以让应用程序使用 [Spring的事务托管功能](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#transaction)。`MongoTransactionManager` 将一个 `ClientSession` 绑定到线程上。`MongoTemplate` 会检测会话，并相应地对这些与事务相关的资源进行操作。`MongoTemplate` 也可以参与到其他正在进行的事务中。下面的例子展示了如何用 `MongoTransactionManager` 创建和使用事务。

Example 126. 事务和 `MongoTransactionManager`

```java
@Configuration
static class Config extends AbstractMongoClientConfiguration {

    @Bean
    MongoTransactionManager transactionManager(MongoDatabaseFactory dbFactory) {  
        return new MongoTransactionManager(dbFactory);
    }

    // ...
}

@Component
public class StateService {

    @Transactional
    void someBusinessFunction(Step step) {                                        

        template.insert(step);

        process(step);

        template.update(Step.class).apply(Update.set("state", // ...
    };
});
```

|      | 在应用 application context 中注册 `MongoTransactionManager`。 |
| ---- | ------------------------------------------------------------ |
|      | 将方法标记为事务性。                                         |

|      | `@Transactional(readOnly = true)` 建议 `MongoTransactionManager` 也启动一个事务，将 `ClientSession` 添加到发出的请求中。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 12.3. 响应式事务

与支持响应式 `ClientSession` 一样，`ReactiveMongoTemplate` 提供了专门的方法，用于在事务中进行操作，而不必担心根据操作结果提交或停止操作。

|      | 除非你在你的 application context 中指定一个 `ReactiveMongoTransactionManager`，否则事务支持是 **DISABLED**（禁用的）。你可以使用 `setSessionSynchronization(ALWAYS)` 来参与正在进行的非本地MongoDB事务。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

使用普通的MongoDB响应式驱动API，在一个事务性流程中的 `delete` 可能看起来像这样。

Example 127. 原生驱动的支持

```java
Mono<DeleteResult> result = Mono
    .from(client.startSession())                                                             

    .flatMap(session -> {
        session.startTransaction();                                                          

        return Mono.from(collection.deleteMany(session, ...))                                

            .onErrorResume(e -> Mono.from(session.abortTransaction()).then(Mono.error(e)))   

            .flatMap(val -> Mono.from(session.commitTransaction()).then(Mono.just(val)))     

            .doFinally(signal -> session.close());                                           
      });
```

|      | 首先，我们显然需要启动session。                        |
| ---- | ------------------------------------------------------ |
|      | 一旦我们有了 `ClientSession`，就开始事务。             |
|      | 通过向操作传递 `ClientSession`，在事务中进行操作。     |
|      | 如果操作异常完成，我们需要停止事务并保留错误。         |
|      | 当然，也可以在成功的情况下提交更改。仍然保留操作结果。 |
|      | 最后，我们需要确保关闭会话。                           |

上述操作的罪魁祸首是在保留 main flow `DeleteResult`，而不是通过 `commitTransaction()` 或 `abortTransaction()` 发布的事务结果，这导致了相当复杂的设置。

### 12.4. 事务和 `TransactionalOperator`

Spring Data MongoDB事务支持一个 `TransactionalOperator`。下面的例子展示了如何创建和使用一个 `TransactionalOperator`。

Example 128. 事务和 `TransactionalOperator`

```java
template.setSessionSynchronization(ALWAYS);                                          

// ...

TransactionalOperator rxtx = TransactionalOperator.create(anyTxManager,
                                   new DefaultTransactionDefinition());              


Step step = // ...;
template.insert(step);

Mono<Void> process(step)
    .then(template.update(Step.class).apply(Update.set("state", …))
    .as(rxtx::transactional)                                                         
    .then();
```

|      | 为事务性参与启用事务同步。                                   |
| ---- | ------------------------------------------------------------ |
|      | 使用提供的 `ReactiveTransactionManager` 创建 `TransactionalOperator`。 |
|      | `TransactionalOperator.transactional(…)` 为所有上游操作提供事务管理。 |

### 12.5. 事务和 `ReactiveMongoTransactionManager`

`ReactiveMongoTransactionManager` 是通往众所周知的 [Spring事务支持](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#transaction) 的网关。它允许应用程序利用Spring的管理事务功能。`ReactiveMongoTransactionManager` 将 `ClientSession` 绑定到 subscriber `Context`。`ReactiveMongoTemplate` 会检测会话，并对这些与事务相关的资源进行相应操作。 `ReactiveMongoTemplate` 也可以参与其他正在进行的事务。下面的例子展示了如何用 `ReactiveMongoTransactionManager` 创建和使用事务。

Example 129. 事务和 `ReactiveMongoTransactionManager`

```java
@Configuration
public class Config extends AbstractReactiveMongoConfiguration {

    @Bean
    ReactiveMongoTransactionManager transactionManager(ReactiveMongoDatabaseFactory factory) {  
        return new ReactiveMongoTransactionManager(factory);
    }

    // ...
}

@Service
public class StateService {

    @Transactional
    Mono<UpdateResult> someBusinessFunction(Step step) {                                  

        return template.insert(step)
            .then(process(step))
            .then(template.update(Step.class).apply(Update.set("state", …));
    };
});
```

|      | 在application context中注册 `ReactiveMongoTransactionManager`。 |
| ---- | ------------------------------------------------------------ |
|      | 将方法标记为事务性的。                                       |

|      | `@Transactional(readOnly = true)` 建议 `ReactiveMongoTransactionManager` 也启动一个事务，将 `ClientSession` 添加到发出的请求中。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 12.6. 事务内部的特殊行为

在事务内部，MongoDB服务器有一个稍微不同的行为。

**Connection Settings**

MongoDB驱动提供了一个专门的副本集名称配置选项，使驱动进入自动检测模式。这个选项有助于识别主要的副本集节点和事务中的命令路由。

|      | 确保在MongoDB的URI中添加 `replicaSet`。请参考 [连接字符串选项](https://docs.mongodb.com/manual/reference/connection-string/#connections-connection-options) 以了解更多细节。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

**Collection Operations**

MongoDB不支持集合操作，例如在事务中创建集合。这也会影响到第一次使用时发生的即时集合创建。因此，请确保所有需要的结构都已到位。

**Transient Errors**

MongoDB可以为在事务性操作中出现的错误添加特殊标签。这些标签可能表示暂时性的故障，这些故障可能通过重试操作而消失。我们强烈推荐 [Spring Retry](https://github.com/spring-projects/spring-retry) 用于这些目的。然而，我们可以覆写 `MongoTransactionManager#doCommit(MongoTransactionObject)`，以实现MongoDB参考手册中所述的重试提交操作行为。

**Count**

MongoDB的 `count` 操作是基于集合统计的，可能无法反映事务中的实际情况。当在一个多文档事务中发出 `count` 命令时，服务器会响应 *error 50851*。一旦 `MongoTemplate` 检测到一个活动的事务，所有暴露的 `count()` 方法都会被转换，并使用 `$match` 和 `$count` 操作符委托给聚合框架，保留 `Query` 设置，如 `collation`。

在 aggregation count helper 中使用 geo 命令时，有一些限制。以下运算符不能使用，必须用不同的运算符代替。

- `$where` → `$expr`
- `$near` → `$geoWithin` with `$center`
- `$nearSphere` → `$geoWithin` with `$centerSphere`

使用 `Criteria.near(…)` 和 `Criteria.nearSphere(…)` 的查询必须改写为 `Criteria.within(…)` 各自的 `Criteria.withinSphere(…)`。同样适用于 repository 查询方法中的 `near` 查询关键字，必须改为 `within`。也请参见MongoDB JIRA ticket [DRIVERS-518](https://jira.mongodb.org/browse/DRIVERS-518) 以进一步参考。

下面的片段显示了会话绑定闭包内的 `count` 用法。

```javascript
session.startTransaction();

template.withSession(session)
    .execute(action -> {
        action.count(query(where("state").is("active")), Step.class)
        ...
```

上面的片段具体化为以下命令。

```javascript
db.collection.aggregate(
   [
      { $match: { state: "active" } },
      { $count: "totalEntityCount" }
   ]
)
```

而不是。

```javascript
db.collection.find( { state: "active" } ).count()
```

## 13. 响应式 MongoDB 支持

响应式MongoDB支持包含以下基本功能集。

- Spring配置支持，使用基于 Java 的 `@Configuration` 类、`MongoClient` 实例和副本集。
- `ReactiveMongoTemplate`，它是一个辅助类，通过以响应式方式使用 `MongoOperations` 来提高生产力。它包括 `Document` 实例和POJO之间的集成对象映射。
- 异常翻译成Spring的可移植数据访问异常（Data Access Exception）层次结构。
- 功能丰富的对象映射与Spring的 `ConversionService` 集成。
- 基于注解的映射元数据，可扩展到支持其他元数据格式。
- 持久性（Persistence）和映射生命周期事件。
- 基于Java的 `Query`、`Criteria` 和 `Update` DSL。
- 自动实现响应式 repositroy 接口，包括支持自定义查询方法。

对于大多数任务，你应该使用 `ReactiveMongoTemplate` 或 repository 支持，它们都使用丰富的映射功能。`ReactiveMongoTemplate` 是访问功能的地方，如递增计数器（incrementing）或临时 CRUD 操作。`ReactiveMongoTemplate` 还提供了回调方法，这样你就可以使用低级别的API工件（如 `MongoDatabase`）来直接与MongoDB进行通信。在各种API工件上的命名约定的目的是复制基础MongoDB Java驱动中的命名约定，以便你可以将现有知识映射到Spring API上。

### 13.1. 入门

支持Spring MongoDB需要MongoDB 2.6或更高版本和Java SE 8或更高版本。

首先，你需要设置一个正在运行的MongoDB服务器。请参考 [MongoDB快速入门指南](https://docs.mongodb.org/manual/core/introduction/)，了解如何启动MongoDB实例。安装完毕后，启动MongoDB通常只需运行以下命令即可： `${MONGO_HOME}/bin/mongod`

要在STS中创建一个Spring项目，进入 File → New → Spring Template Project → Simple Spring Utility Project，在提示时按Yes。然后输入一个项目和一个包的名称，比如 org.spring.mongodb.example。

然后在pom.xml dependencies 部分添加以下内容。

```xml
<dependencies>

  <!-- other dependency elements omitted -->

  <dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-mongodb</artifactId>
    <version>4.1.0-SNAPSHOT</version>
  </dependency>

  <dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongodb-driver-reactivestreams</artifactId>
    <version>4.9.0</version>
  </dependency>

  <dependency>
    <groupId>io.projectreactor</groupId>
    <artifactId>reactor-core</artifactId>
    <version>2022.0.2</version>
  </dependency>

</dependencies>
```

|      | MongoDB使用两种不同的驱动来进行阻塞式和响应式（非阻塞式）数据访问。虽然默认提供阻塞操作，但你可以选择加入响应式使用。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

为了开始一个工作实例，创建一个简单的 `Person` 类来进行持久化，如下所示。

```java
@Document
public class Person {

  private String id;
  private String name;
  private int age;

  public Person(String name, int age) {
    this.name = name;
    this.age = age;
  }

  public String getId() {
    return id;
  }
  public String getName() {
    return name;
  }
  public int getAge() {
    return age;
  }

  @Override
  public String toString() {
    return "Person [id=" + id + ", name=" + name + ", age=" + age + "]";
  }
}
```

然后创建一个应用程序来运行，如下所示。

```java
public class ReactiveMongoApp {

  private static final Logger log = LoggerFactory.getLogger(ReactiveMongoApp.class);

  public static void main(String[] args) throws Exception {

    CountDownLatch latch = new CountDownLatch(1);

    ReactiveMongoTemplate mongoOps = new ReactiveMongoTemplate(MongoClients.create(), "database");

    mongoOps.insert(new Person("Joe", 34))
          .flatMap(p -> mongoOps.findOne(new Query(where("name").is("Joe")), Person.class))
          .doOnNext(person -> log.info(person.toString()))
          .flatMap(person -> mongoOps.dropCollection("person"))
          .doOnComplete(latch::countDown)
          .subscribe();

    latch.await();
  }
}
```

运行前面的类产生以下输出。

```
2016-09-20 14:56:57,373 DEBUG .index.MongoPersistentEntityIndexCreator: 124 - Analyzing class class example.ReactiveMongoApp$Person for index information.
2016-09-20 14:56:57,452 DEBUG .data.mongodb.core.ReactiveMongoTemplate: 975 - Inserting Document containing fields: [_class, name, age] in collection: person
2016-09-20 14:56:57,541 DEBUG .data.mongodb.core.ReactiveMongoTemplate:1503 - findOne using query: { "name" : "Joe"} fields: null for class: class example.ReactiveMongoApp$Person in collection: person
2016-09-20 14:56:57,545 DEBUG .data.mongodb.core.ReactiveMongoTemplate:1979 - findOne using query: { "name" : "Joe"} in db.collection: database.person
2016-09-20 14:56:57,567  INFO                 example.ReactiveMongoApp:  43 - Person [id=57e1321977ac501c68d73104, name=Joe, age=34]
2016-09-20 14:56:57,573 DEBUG .data.mongodb.core.ReactiveMongoTemplate: 528 - Dropped collection [person]
```

即使在这个简单的例子中，也有一些事情需要注意。

- 你可以通过使用标准的 `com.mongodb.reactivestreams.client.MongoClient` 对象和要使用的数据库名称来实例化Spring Mongo的中心 helper 类（[`ReactiveMongoTemplate`](https://springdoc.cn/spring-data-mongodb/#mongo.reactive.template)）。
- 该 mapper 针对标准的POJO对象工作，不需要任何额外的元数据（尽管你可以选择性地提供这些信息。 见 [这里](https://springdoc.cn/spring-data-mongodb/#mapping-chapter)）。
- 惯例用于处理 ID 字段，在数据库中存储时将其转换为 `ObjectId`。
- 映射约定可以使用字段访问。请注意，`Person` 类只有 getter。
- 如果构造函数的参数名与存储文档的字段名相匹配，它们将被用来实例化对象。

有一个 [GitHub仓库，里面有几个例子](https://github.com/spring-projects/spring-data-examples)，你可以下载并玩一玩，感受一下这个库是如何工作的。

### 13.2. 用Spring和Reactive Streams驱动连接到MongoDB

在使用MongoDB和Spring时，首要任务之一是通过使用IoC容器创建一个 `com.mongodb.reactivestreams.client.MongoClient` 对象。

#### 13.2.1. 使用基于Java的元数据注册MongoClient实例

下面的例子展示了如何使用基于 Java 的 Bean 元数据来注册 `com.mongodb.reactivestreams.client.MongoClient` 的一个实例。

Example 130. 使用基于Java的bean元数据注册一个 `com.mongodb.reactivestreams.client.MongoClient` 对象

```java
@Configuration
public class AppConfig {

  /*
   * Use the Reactive Streams Mongo Client API to create a com.mongodb.reactivestreams.client.MongoClient instance.
   */
   public @Bean MongoClient reactiveMongoClient()  {
       return MongoClients.create("mongodb://localhost");
   }
}
```

这种方法可以让你使用标准的 `com.mongodb.reactivestreams.client.MongoClient` API（你可能已经知道）。

另一种方法是通过使用 Spring 的 `ReactiveMongoClientFactoryBean` 向容器注册 `com.mongodb.reactivestreams.client.MongoClient` 实例。与直接实例化 `com.mongodb.reactivestreams.client.MongoClient` 实例相比， `FactoryBean` 方法还有一个优势，即为容器提供 `ExceptionTranslator` 实现，该实现可将 MongoDB 异常转换为 Spring 的可移植 `DataAccessException` 层次结构中的异常，用于注解有 `@Repository` 的数据访问类。这种层次结构和 `@Repository` 的使用在 [Spring的DAO支持功能](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html) 中有所描述。

下面的例子显示了基于Java的Bean元数据，它支持 `@Repository` 注解的类上的异常翻译。

Example 131. 使用Spring的 `MongoClientFactoryBean` 注册 `com.mongodb.reactivestreams.client.MongoClient` 对象并启用Spring的异常翻译支持。

```java
@Configuration
public class AppConfig {

    /*
     * Factory bean that creates the com.mongodb.reactivestreams.client.MongoClient instance
     */
     public @Bean ReactiveMongoClientFactoryBean mongoClient() {

          ReactiveMongoClientFactoryBean clientFactory = new ReactiveMongoClientFactoryBean();
          clientFactory.setHost("localhost");

          return clientFactory;
     }
}
```

要访问由 `ReactiveMongoClientFactoryBean` 在其他 `@Configuration` 或你自己的类中创建的 `com.mongodb.reactivestreams.client.MongoClient` 对象，可从 context 中获取 `MongoClient`。

#### 13.2.2. ReactiveMongoDatabaseFactory 接口

虽然 `com.mongodb.reactivestreams.client.MongoClient` 是响应式 MongoDB 驱动程序 API 的入口，但连接到特定 MongoDB 数据库实例需要额外的信息，例如数据库名称。有了这些信息，你就可以获得 `com.mongodb.reactivestreams.client.MongoDatabase` 对象并访问特定MongoDB数据库实例的所有功能。Spring 提供了 `org.springframework.data.mongodb.core.ReactiveMongoDatabaseFactory` 接口来引导与数据库的连接。下面的列表显示了 `ReactiveMongoDatabaseFactory` 接口。

```java
public interface ReactiveMongoDatabaseFactory {

  /**
   * Creates a default {@link MongoDatabase} instance.
   *
   * @return
   * @throws DataAccessException
   */
  MongoDatabase getMongoDatabase() throws DataAccessException;

  /**
   * Creates a {@link MongoDatabase} instance to access the database with the given name.
   *
   * @param dbName must not be {@literal null} or empty.
   * @return
   * @throws DataAccessException
   */
  MongoDatabase getMongoDatabase(String dbName) throws DataAccessException;

  /**
   * Exposes a shared {@link MongoExceptionTranslator}.
   *
   * @return will never be {@literal null}.
   */
  PersistenceExceptionTranslator getExceptionTranslator();
}
```

`org.springframework.data.mongodb.core.SimpleReactiveMongoDatabaseFactory` 类实现了 `ReactiveMongoDatabaseFactory` 接口，并以标准的 `com.mongodb.reactivestreams.client.MongoClient` 实例和数据库名称创建。

你可以在标准的Java代码中使用它们，而不是使用IoC容器来创建 `ReactiveMongoTemplate` 的实例，如下所示。

```java
public class MongoApp {

  private static final Log log = LogFactory.getLog(MongoApp.class);

  public static void main(String[] args) throws Exception {

    ReactiveMongoOperations mongoOps = new ReactiveMongoOperations(new SimpleReactiveMongoDatabaseFactory(MongoClient.create(), "database"));

    mongoOps.insert(new Person("Joe", 34))
        .flatMap(p -> mongoOps.findOne(new Query(where("name").is("Joe")), Person.class))
        .doOnNext(person -> log.info(person.toString()))
        .flatMap(person -> mongoOps.dropCollection("person"))
        .subscribe();
  }
}
```

使用 `SimpleReactiveMongoDatabaseFactory` 是 [入门部分](https://springdoc.cn/spring-data-mongodb/#mongodb-reactive-getting-started) 所示清单的唯一区别。

#### 13.2.3. 通过使用基于Java的元数据注册 ReactiveMongoDatabaseFactory 实例

为了在容器中注册一个 `ReactiveMongoDatabaseFactory` 实例，你可以编写与前面代码清单中强调的差不多的代码，如下例所示。

```java
@Configuration
public class MongoConfiguration {

  public @Bean ReactiveMongoDatabaseFactory reactiveMongoDatabaseFactory() {
    return new SimpleReactiveMongoDatabaseFactory(MongoClients.create(), "database");
  }
}
```

为了定义用户名和密码，创建一个 MongoDB 连接字符串并将其传入工厂方法，正如下一个列表所示。下面的清单还显示了如何使用 `ReactiveMongoDatabaseFactory` 来向容器注册 `ReactiveMongoTemplate` 的实例。

```java
@Configuration
public class MongoConfiguration {

  public @Bean ReactiveMongoDatabaseFactory reactiveMongoDatabaseFactory() {
    return new SimpleReactiveMongoDatabaseFactory(MongoClients.create("mongodb://joe:secret@localhost"), "database");
  }

  public @Bean ReactiveMongoTemplate reactiveMongoTemplate() {
    return new ReactiveMongoTemplate(reactiveMongoDatabaseFactory());
  }
}
```

### 13.3. `ReactiveMongoTemplate` 简介

位于 `org.springframework.data.mongodb` 包中的 `ReactiveMongoTemplate` 类是Spring的Reactive MongoDB支持的中心类，它提供了丰富的功能集来与数据库交互。该模板提供了创建、更新、删除和查询MongoDB文档的便利操作，并提供了你的 domain 对象和MongoDB文档之间的映射。

|      | 一旦配置好，`ReactiveMongoTemplate` 是线程安全的，可以在多个实例中重复使用。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

MongoDB文档和domain类之间的映射是通过委托给 `MongoConverter` 接口的实现完成的。Spring提供了 `MongoMappingConverter` 的默认实现，但你也可以编写自己的转换器。更详细的信息请参见 [`MongoConverter` 实例部分](https://springdoc.cn/spring-data-mongodb/#mongo.custom-converters)。

`ReactiveMongoTemplate` 类实现了 `ReactiveMongoOperations` 接口。 `ReactiveMongoOperations` 上的方法尽可能地反映了MongoDB驱动 `Collection` 对象上的方法，以使习惯于驱动API的现有MongoDB开发人员熟悉该API。例如，你可以找到诸如 `find`、`findAndModify`、`findOne`、`insert`、`remove`、`save`、`update` 和 `updateMulti` 等方法。设计目标是使其尽可能容易地在使用基本MongoDB驱动和 `ReactiveMongoOperations` 之间过渡。这两个API之间的主要区别是，`ReactiveMongoOperations` 可以传递domain对象而不是 `Document`，而且 `Query`、`Criteria` 和 `Update` 操作有fluent API，而不是通过填充 `Document` 来指定这些操作的参数。

|      | 引用 `ReactiveMongoTemplate` 实例上的操作的首选方式是通过其 `ReactiveMongoOperations` 接口。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

`ReactiveMongoTemplate` 使用的默认转换器实现是 `MappingMongoConverter`。虽然 `MappingMongoConverter` 可以使用额外的元数据来指定对象到文档的映射，但它也可以通过使用一些惯例来转换ID和集合名称的映射，从而转换不包含额外元数据的对象。这些约定以及映射注解的使用在 [映射一章](https://springdoc.cn/spring-data-mongodb/#mapping-chapter) 中进行了解释。

`ReactiveMongoTemplate` 的另一个核心功能是将MongoDB Java驱动中抛出的异常转换为Spring的可移植数据访问异常（Data Access Exception）层次结构。更多信息请参见 [异常转换](https://springdoc.cn/spring-data-mongodb/#mongo.exception) 部分。

`ReactiveMongoTemplate` 上有许多便利方法，可以帮助你轻松地执行常见的任务。然而，如果你需要直接访问 MongoDB 驱动程序 API，以访问 `MongoTemplate` 没有明确暴露的功能，你可以使用几个 `execute` 回调方法之一来访问底层驱动程序 API。`execute` 回调给你一个对 `com.mongodb.reactivestreams.client.MongoCollection` 或 `com.mongodb.reactivestreams.client.MongoDatabase` 对象的引用。参见 [Execution 回调](https://springdoc.cn/spring-data-mongodb/#mongo.reactive.executioncallback) 以了解更多信息。

#### 13.3.1. 实例化 ReactiveMongoTemplate

你可以使用Java来创建和注册 `ReactiveMongoTemplate` 的实例，如下所示。

Example 132. 注册 `com.mongodb.reactivestreams.client.MongoClient` 对象并启用Spring的异常翻译支持。

```java
@Configuration
public class AppConfig {

  public @Bean MongoClient reactiveMongoClient() {
      return MongoClients.create("mongodb://localhost");
  }

  public @Bean ReactiveMongoTemplate reactiveMongoTemplate() {
      return new ReactiveMongoTemplate(reactiveMongoClient(), "mydatabase");
  }
}
```

`ReactiveMongoTemplate` 有几个重载构造函数，包括。

- `ReactiveMongoTemplate(MongoClient mongo, String databaseName)`: 接受 `com.mongodb.reactivestreams.client.MongoClient` 对象和默认的数据库名称来进行操作。
- `ReactiveMongoTemplate(ReactiveMongoDatabaseFactory mongoDatabaseFactory)`: 接收一个封装了 `com.mongodb.reactivestreams.client.MongoClient` 对象和数据库名称的 `ReactiveMongoDatabaseFactory` 对象。
- `ReactiveMongoTemplate(ReactiveMongoDatabaseFactory mongoDatabaseFactory, MongoConverter mongoConverter)`: 添加一个用于映射的 `MongoConverter`。

创建 `ReactiveMongoTemplate` 时，你可能还想设置以下属性。

- `WriteResultCheckingPolicy`
- `WriteConcern`
- `ReadPreference`

|      | 引用 `ReactiveMongoTemplate` 实例上的操作的首选方式是通过其 `ReactiveMongoOperations` 接口。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 13.3.2. `WriteResultChecking` 策略

在开发过程中，如果从任何MongoDB操作返回的 `com.mongodb.WriteResult` 包含一个错误，那么记录或抛出一个异常是很方便的。在开发过程中，忘记这样做是很常见的，然后最终得到一个看起来运行成功的应用程序，而事实上，数据库并没有按照你的预期进行修改。将 `MongoTemplate` `WriteResultChecking` 属性设置为一个枚举，其值为：`LOG`、`EXCEPTION` 或 `NONE`，以记录错误、抛出异常或不做任何事情。默认是使用 `NONE` 的 `WriteResultChecking` 值。

#### 13.3.3. `WriteConcern`

如果尚未通过驱动程序在更高层次（如 `MongoDatabase`）上指定，你可以设置 `ReactiveMongoTemplate` 用于写操作的 `com.mongodb.WriteConcern` 属性。如果 `ReactiveMongoTemplate` 的 `WriteConcern` 属性未被设置，则默认为 MongoDB 驱动程序的 `MongoDatabase` 或 `MongoCollection` 设置中的设置。

#### 13.3.4. `WriteConcernResolver`

对于更高级的情况，即你想在每个操作的基础上设置不同的 `WriteConcern` 值（对于删除、更新、插入和保存操作），可以在 `ReactiveMongoTemplate` 上配置一个叫做 `WriteConcernResolver` 的策略接口。由于 `ReactiveMongoTemplate` 是用来持久化POJO的，`WriteConcernResolver` 让你创建一个策略，可以将特定的POJO类映射到 `WriteConcern` 值。下面的列表显示了 `WriteConcernResolver` 的接口。

```java
public interface WriteConcernResolver {
  WriteConcern resolve(MongoAction action);
}
```

参数 `MongoAction` 决定了要使用的 `WriteConcern` 值以及是否使用模板本身的值作为默认值。`MongoAction` 包含被写入的集合名称、POJO的 `java.lang.Class`、转换后的 `DBObject`、作为 `MongoActionOperation` 枚举值的操作（`REMOVE`、`UPDATE`、`INSERT`、`INSERT_LIST` 和 `SAVE` 之一）以及其他一些上下文信息。下面的例子显示了如何创建一个 `WriteConcernResolver`。

```
private class MyAppWriteConcernResolver implements WriteConcernResolver {

  public WriteConcern resolve(MongoAction action) {
    if (action.getEntityClass().getSimpleName().contains("Audit")) {
      return WriteConcern.NONE;
    } else if (action.getEntityClass().getSimpleName().contains("Metadata")) {
      return WriteConcern.JOURNAL_SAFE;
    }
    return action.getDefaultWriteConcern();
  }
}
```

### 13.4. 保存、更新和删除文档

`ReactiveMongoTemplate` 让你可以保存、更新和删除你的domain对象，并将这些对象映射到存储在MongoDB中的文档。

考虑以下 `Person` 类。

```java
public class Person {

  private String id;
  private String name;
  private int age;

  public Person(String name, int age) {
    this.name = name;
    this.age = age;
  }

  public String getId() {
    return id;
  }
  public String getName() {
    return name;
  }
  public int getAge() {
    return age;
  }

  @Override
  public String toString() {
    return "Person [id=" + id + ", name=" + name + ", age=" + age + "]";
  }

}
```

下面的列表显示了你如何保存、更新和删除 `Person` 对象。

```java
public class ReactiveMongoApp {

  private static final Logger log = LoggerFactory.getLogger(ReactiveMongoApp.class);

  public static void main(String[] args) throws Exception {

    CountDownLatch latch = new CountDownLatch(1);

    ReactiveMongoTemplate mongoOps = new ReactiveMongoTemplate(MongoClients.create(), "database");

    mongoOps.insert(new Person("Joe", 34)).doOnNext(person -> log.info("Insert: " + person))
      .flatMap(person -> mongoOps.findById(person.getId(), Person.class))
      .doOnNext(person -> log.info("Found: " + person))
      .zipWith(person -> mongoOps.updateFirst(query(where("name").is("Joe")), update("age", 35), Person.class))
      .flatMap(tuple -> mongoOps.remove(tuple.getT1())).flatMap(deleteResult -> mongoOps.findAll(Person.class))
      .count().doOnSuccess(count -> {
        log.info("Number of people: " + count);
        latch.countDown();
      })

      .subscribe();

    latch.await();
  }
}
```

前面的例子包括存储在数据库中的 `String` 和 `ObjectId` 之间的隐式转换（通过使用 `MongoConverter`），并认识到属性 `Id` 名称的惯例。

|      | 前面的例子是为了展示在 `ReactiveMongoTemplate` 上使用save、update和remove操作，而不是为了展示复杂的映射或链式功能。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

“[Querying Documents](https://springdoc.cn/spring-data-mongodb/#mongo.query)” 更详细地解释了前面的例子中使用的查询语法。其他文档可以在 [阻塞式 `MongoTemplate`](https://springdoc.cn/spring-data-mongodb/#mongo-template)部分找到。

### 13.5. Execution 回调

所有Spring template 类的一个共同设计特点是，所有功能都被路由到运行回调方法的模板之一。这有助于确保异常和任何可能需要的资源管理的执行一致性。虽然这在JDBC和JMS的情况下比在MongoDB的情况下更有必要，但它仍然提供了一个发生异常转换和记录的单一地点。因此，使用 `execute` 回调是访问 MongoDB 驱动程序的 `MongoDatabase` 和 `MongoCollection` 对象的首选方式，以执行未在 `ReactiveMongoTemplate` 上作为方法公开的不常见操作。

这里有一个 `execute` 回调方法的列表。

- `<T> Flux<T>` **execute** `(Class<?> entityClass, ReactiveCollectionCallback<T> action)`: 为指定类别的实体集合运行给定的 `ReactiveCollectionCallback`。
- `<T> Flux<T>` **execute** `(String collectionName, ReactiveCollectionCallback<T> action)`: 在给定名称的集合上运行给定的 `ReactiveCollectionCallback`。
- `<T> Flux<T>` **execute** `(ReactiveDatabaseCallback<T> action)`: 运行一个 `ReactiveDatabaseCallback`，必要时翻译任何异常。

下面的例子使用 `ReactiveCollectionCallback` 来返回关于一个索引的信息。

```java
Flux<Boolean> hasIndex = operations.execute("geolocation",
    collection -> Flux.from(collection.listIndexes(Document.class))
      .filter(document -> document.get("name").equals("fancy-index-name"))
      .flatMap(document -> Mono.just(true))
      .defaultIfEmpty(false));
```

### 13.6. 支持GridFS

MongoDB支持在其文件系统GridFS内存储二进制文件。Spring Data MongoDB提供了一个 `ReactiveGridFsOperations` 接口以及相应的实现，即 `ReactiveGridFsTemplate`，让你与文件系统互动。你可以通过将 `ReactiveMongoDatabaseFactory` 以及 `MongoConverter` 交给它来设置一个 `ReactiveGridFsTemplate` 实例，如下例所示。

Example 133. 为 ReactiveGridFsTemplate 设置 JavaConfig

```java
class GridFsConfiguration extends AbstractReactiveMongoConfiguration {

  // … further configuration omitted

  @Bean
  public ReactiveGridFsTemplate reactiveGridFsTemplate() {
    return new ReactiveGridFsTemplate(reactiveMongoDbFactory(), mappingMongoConverter());
  }
}
```

现在，该 template 可以被注入并用于执行存储和检索操作，如下例所示。

Example 134. 使用 ReactiveGridFsTemplate 来存储文件

```java
class ReactiveGridFsClient {

  @Autowired
  ReactiveGridFsTemplate operations;

  @Test
  public Mono<ObjectId> storeFileToGridFs() {

    FileMetadata metadata = new FileMetadata();
    // populate metadata
    Publisher<DataBuffer> file = … // lookup File or Resource

    return operations.store(file, "filename.txt", metadata);
  }
}
```

`store(…)` 操作接收一个 `Publisher<DataBuffer>`、一个文件名以及（可选）关于要存储的文件的元数据信息。元数据可以是一个任意的对象，它将被配置有 `ReactiveGridFsTemplate` 的 `MongoConverter` 所处理。另外，你也可以提供一个 `Document`。

|      | MongoDB 的驱动程序使用 `AsyncInputStream` 和 `AsyncOutputStream` 接口来交换二进制流。Spring Data MongoDB 将这些接口调整为 `Publisher<DataBuffer>`。在 [Spring的参考文档](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#databuffers) 中阅读更多关于 `DataBuffer` 的信息。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

你可以通过 `find(…)` 或 `getResources(…)` 方法从文件系统中读取文件。让我们先看一下 `find(…)` 方法。你可以找到一个文件，也可以找到符合查询条件的多个文件。你可以使用 `GridFsCriteria` helper 类来定义查询。它提供了静态的工厂方法来封装默认的元数据字段（如 `whereFilename()` 和 `whereContentType()`），或者通过 `whereMetaData()` 封装一个自定义的元数据。下面的例子显示了如何使用 `ReactiveGridFsTemplate` 来查询文件。

Example 135. 使用 `ReactiveGridFsTemplate` 来查询文件

```java
class ReactiveGridFsClient {

  @Autowired
  ReactiveGridFsTemplate operations;

  @Test
  public Flux<GridFSFile> findFilesInGridFs() {
    return operations.find(query(whereFilename().is("filename.txt")))
  }
}
```

|      | 目前，MongoDB不支持在从GridFS检索文件时定义排序标准。出于这个原因，任何定义在 `Query` 实例上的排序 criteria 都会被忽略，而这些排序标准会被交给 `find(…)` 方法来处理。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

从GridFs读取文件的另一个选择是使用沿着 `ResourcePatternResolver` 的思路建模的方法。`ReactiveGridFsOperations` 使用响应式类型来推迟运行，而 `ResourcePatternResolver` 使用同步接口。这些方法允许将Ant路径交到方法中，从而可以检索到与给定模式（pattern）相匹配的文件。下面的例子显示了如何使用 `ReactiveGridFsTemplate` 来读取文件。

Example 136. 使用 `ReactiveGridFsTemplate` 来读取文件

```java
class ReactiveGridFsClient {

  @Autowired
  ReactiveGridFsOperations operations;

  @Test
  public void readFilesFromGridFs() {
     Flux<ReactiveGridFsResource> txtFiles = operations.getResources("*.txt");
  }
}
```

## 14. MongoDB Repository

本章指出了 MongoDB 的 repository 支持的特殊性。本章建立在 [与 Spring Data Repository 一起工作](https://springdoc.cn/spring-data-mongodb/#repositories) 中解释的核心 repository 支持的基础上。你应该对那里解释的基本概念有一个正确的理解。

### 14.1. 用法

为了访问存储在MongoDB中的domain实体，你可以使用我们复杂的 repository 支持，这大大简化了实施。要做到这一点，为你的 repository 创建一个接口，如下例所示。

Example 137. Person entity 示例

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

注意在前面的例子中显示的 domain 类型有一个名为 `id` 的 `String` 类型的属性。`MongoTemplate` 中使用的默认序列化机制（它支持 repository）将名为 `id` 的属性视为文档 ID。目前，我们支持 `String`、`ObjectId` 和 `BigInteger` 作为 ID 类型。请参阅 [ID映射](https://springdoc.cn/spring-data-mongodb/#mongo-template.id-handling) 以了解更多关于在映射层中如何处理 `id` 字段的信息。

现在我们有了一个 domain 对象，我们可以定义一个使用它的接口，如下所示。

Example 138. 持久保存 Person 实体的基本 repository 接口

```
public interface PersonRepository extends PagingAndSortingRepository<Person, String> {

  // additional custom query methods go here
}
```

现在，这个接口只起到提供类型信息的作用，但我们可以在以后为它添加额外的方法。

要开始使用repository，请使用 `@EnableMongoRepositories` 注解。该注解带有与命名空间元素相同的属性。如果没有配置基础包（base package），基础设施会扫描被注解的配置类的包。下面的例子展示了如何配置你的应用程序以使用MongoDB repository。

Java

XML

```java
@Configuration
@EnableMongoRepositories("com.acme.*.repositories")
class ApplicationConfig extends AbstractMongoClientConfiguration {

  @Override
  protected String getDatabaseName() {
    return "e-store";
  }

  @Override
  protected String getMappingBasePackage() {
    return "com.acme.*.repositories";
  }
}
```

这个命名空间元素会使基础包扫描扩展 `MongoRepository` 的接口，并为找到的每个接口创建 Spring Bean。默认情况下，存储库会得到一 `个MongoTemplate` Spring Bean wired，它被称为 `mongoTemplate`，所以你只需要在偏离这一惯例时明确配置 `mongo-template-ref`。

因为我们的 doamin repository 继承了 `PagingAndSortingRepository`，它为你提供了CRUD操作以及分页和排序访问实体的方法。使用 repository 实例只是将其依赖注入到客户端的问题。因此，在页面大小为10的情况下访问第二页的 `Person` 对象将类似于以下代码。

Example 139. 对 Person 实体的分页访问

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration
class PersonRepositoryTests {

    @Autowired PersonRepository repository;

    @Test
    void readsFirstPageCorrectly() {

      Page<Person> persons = repository.findAll(PageRequest.of(0, 10));
      assertThat(persons.isFirstPage()).isTrue();
    }
}
```

前面的例子用Spring的单元测试支持创建了一个 application context，它将基于注解的依赖注入到测试案例中。在测试方法中，我们使用 repository 来查询数据存储。我们交给 repository 一个 `PageRequest` 实例，请求页面大小为10的 `Person` 对象的第一页。

### 14.2. Query 方法

你通常在 repository 上触发的大多数数据访问操作都会导致对 MongoDB 数据库的查询被执行。定义这样的查询只需要在 repository 接口上声明一个方法，就像下面的例子所示。

Example 140. 具有 Query 方法的PersonRepository

```java
public interface PersonRepository extends PagingAndSortingRepository<Person, String> {

    List<Person> findByLastname(String lastname);                      

    Page<Person> findByFirstname(String firstname, Pageable pageable); 

    Person findByShippingAddresses(Address address);                   

    Person findFirstByLastname(String lastname)                        

    Stream<Person> findAllBy();                                        
}
```

|      | `findByLastname` 方法显示了一个对所有具有给定姓氏（last name）的人的查询。该查询是通过解析方法名称中可以与 `And` 和 `Or` 相连接的约束条件而得到的。因此，方法名称的结果是一个查询表达式 `{"lastname" : lastname}`。 |
| ---- | ------------------------------------------------------------ |
|      | 将分页应用到查询中。你可以为你的方法签名配备一个 `Pageable` 参数，并让方法返回一个 `Page` 实例，Spring Data 会自动对查询进行相应的分页。 |
|      | 显示你可以基于非原始类型的属性进行查询。如果发现一个以上的匹配，会抛出 `IncorrectResultSizeDataAccessException`。 |
|      | 使用 `First` 关键字，将查询限制在第一个结果上。与<3>不同的是，如果找到一个以上的匹配，这个方法不会抛出一个异常。 |
|      | 使用一个Java 8 `Stream`，在迭代流的同时读取和转换单个元素。  |

|      | 我们不支持引用在domain类中被映射为 `DBRef` 的参数。 |
| ---- | --------------------------------------------------- |
|      |                                                     |

下表显示了查询方法所支持的关键字。

| 关键字                               | 示例                                                         | 逻辑结果                                                     |
| :----------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `After`                              | `findByBirthdateAfter(Date date)`                            | `{"birthdate" : {"$gt" : date}}`                             |
| `GreaterThan`                        | `findByAgeGreaterThan(int age)`                              | `{"age" : {"$gt" : age}}`                                    |
| `GreaterThanEqual`                   | `findByAgeGreaterThanEqual(int age)`                         | `{"age" : {"$gte" : age}}`                                   |
| `Before`                             | `findByBirthdateBefore(Date date)`                           | `{"birthdate" : {"$lt" : date}}`                             |
| `LessThan`                           | `findByAgeLessThan(int age)`                                 | `{"age" : {"$lt" : age}}`                                    |
| `LessThanEqual`                      | `findByAgeLessThanEqual(int age)`                            | `{"age" : {"$lte" : age}}`                                   |
| `Between`                            | `findByAgeBetween(int from, int to)` `findByAgeBetween(Range<Integer> range)` | `{"age" : {"$gt" : from, "$lt" : to}}` lower / upper bounds (`$gt` / `$gte` & `$lt` / `$lte`) according to `Range` |
| `In`                                 | `findByAgeIn(Collection ages)`                               | `{"age" : {"$in" : [ages…]}}`                                |
| `NotIn`                              | `findByAgeNotIn(Collection ages)`                            | `{"age" : {"$nin" : [ages…]}}`                               |
| `IsNotNull`, `NotNull`               | `findByFirstnameNotNull()`                                   | `{"firstname" : {"$ne" : null}}`                             |
| `IsNull`, `Null`                     | `findByFirstnameNull()`                                      | `{"firstname" : null}`                                       |
| `Like`, `StartingWith`, `EndingWith` | `findByFirstnameLike(String name)`                           | `{"firstname" : name} (name as regex)`                       |
| `NotLike`, `IsNotLike`               | `findByFirstnameNotLike(String name)`                        | `{"firstname" : { "$not" : name }} (name as regex)`          |
| `Containing` on String               | `findByFirstnameContaining(String name)`                     | `{"firstname" : name} (name as regex)`                       |
| `NotContaining` on String            | `findByFirstnameNotContaining(String name)`                  | `{"firstname" : { "$not" : name}} (name as regex)`           |
| `Containing` on Collection           | `findByAddressesContaining(Address address)`                 | `{"addresses" : { "$in" : address}}`                         |
| `NotContaining` on Collection        | `findByAddressesNotContaining(Address address)`              | `{"addresses" : { "$not" : { "$in" : address}}}`             |
| `Regex`                              | `findByFirstnameRegex(String firstname)`                     | `{"firstname" : {"$regex" : firstname }}`                    |
| `(No keyword)`                       | `findByFirstname(String name)`                               | `{"firstname" : name}`                                       |
| `Not`                                | `findByFirstnameNot(String name)`                            | `{"firstname" : {"$ne" : name}}`                             |
| `Near`                               | `findByLocationNear(Point point)`                            | `{"location" : {"$near" : [x,y]}}`                           |
| `Near`                               | `findByLocationNear(Point point, Distance max)`              | `{"location" : {"$near" : [x,y], "$maxDistance" : max}}`     |
| `Near`                               | `findByLocationNear(Point point, Distance min, Distance max)` | `{"location" : {"$near" : [x,y], "$minDistance" : min, "$maxDistance" : max}}` |
| `Within`                             | `findByLocationWithin(Circle circle)`                        | `{"location" : {"$geoWithin" : {"$center" : [ [x, y], distance]}}}` |
| `Within`                             | `findByLocationWithin(Box box)`                              | `{"location" : {"$geoWithin" : {"$box" : [ [x1, y1], x2, y2]}}}` |
| `IsTrue`, `True`                     | `findByActiveIsTrue()`                                       | `{"active" : true}`                                          |
| `IsFalse`, `False`                   | `findByActiveIsFalse()`                                      | `{"active" : false}`                                         |
| `Exists`                             | `findByLocationExists(boolean exists)`                       | `{"location" : {"$exists" : exists }}`                       |
| `IgnoreCase`                         | `findByUsernameIgnoreCase(String username)`                  | `{"username" : {"$regex" : "^username$", "$options" : "i" }}` |

|      | 如果属性标准（property criterion）比较的是一个文档，那么字段的顺序和在文档中的完全平等（equality）就很重要。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 14.2.1. Repository Update 方法

你也可以使用前述表格中的关键字来创建查询，以识别匹配的文档，从而对其运行更新。实际的更新操作是由方法本身的 `@Update` 注解来定义的，如下表所示。注意，派生查询的命名模式以 `find` 开头。使用 `update`（如 `updateAllByLastname(…)`）只允许与 `@Query` 相结合。

更新适用于所有匹配的文档，不可能通过传入一个 `Page` 或使用任何 [限制性关键字](https://springdoc.cn/spring-data-mongodb/#repositories.limit-query-result) 来限制范围。返回类型可以是 `void`，也可以是数字类型，比如 `long`，用来保存修改的文档数量。

Example 141. Update 方法

```java
public interface PersonRepository extends CrudRepository<Person, String> {

  @Update("{ '$inc' : { 'visits' : 1 } }")
  long findAndIncrementVisitsByLastname(String lastname); 

  @Update("{ '$inc' : { 'visits' : ?1 } }")
  void findAndIncrementVisitsByLastname(String lastname, int increment); 

  @Update("{ '$inc' : { 'visits' : ?#{[1]} } }")
  long findAndIncrementVisitsUsingSpELByLastname(String lastname, int increment); 

  @Update(pipeline = {"{ '$set' : { 'visits' : { '$add' : [ '$visits', ?1 ] } } }"})
  void findAndIncrementVisitsViaPipelineByLastname(String lastname, int increment); 

  @Update("{ '$push' : { 'shippingAddresses' : ?1 } }")
  long findAndPushShippingAddressByEmail(String email, Address address); 

  @Query("{ 'lastname' : ?0 }")
  @Update("{ '$inc' : { 'visits' : ?1 } }")
  void updateAllByLastname(String lastname, int increment); 
}
```

|      | 更新的过滤查询来自于方法名称。更新是 "原样" 的，没有绑定任何参数。 |
| ---- | ------------------------------------------------------------ |
|      | 实际的增量值是由与 `?1` 占位符绑定的 `increment` 方法参数定义的。 |
|      | 使用Spring表达式语言（SpEL）进行参数绑定。                   |
|      | 使用 `pipeline` 属性来发布 [聚合管道更新](https://springdoc.cn/spring-data-mongodb/#mongo-template.aggregation-update)。 |
|      | 更新可能包含复杂的对象。                                     |
|      | 将一个 [基于字符串的查询](https://springdoc.cn/spring-data-mongodb/#mongodb.repositories.queries.json-based) 与更新结合起来。 |

|      | Repository 更新不会发出持久性或映射生命周期事件。 |
| ---- | ------------------------------------------------- |
|      |                                                   |

#### 14.2.2. Repository Delete 查询

上表中的关键词可以与 `delete…By` 或 `remove…By` 结合使用，创建删除匹配文档的查询。

Example 142. `Delete…By` 查询

```java
public interface PersonRepository extends MongoRepository<Person, String> {

  List <Person> deleteByLastname(String lastname);      

  Long deletePersonByLastname(String lastname);         

  @Nullable
  Person deleteSingleByLastname(String lastname);       

  Optional<Person> deleteByBirthdate(Date birthdate);   
}
```

|      | 使用 `List` 的返回类型可以在实际删除文档之前检索并返回所有匹配的文档。 |
| ---- | ------------------------------------------------------------ |
|      | 一个数字返回类型直接删除匹配的文档，返回被删除的文档总数。   |
|      | 单一 domain 类型的结果会检索并删除第一个匹配的文档。         |
|      | 与 3 中相同，但被包裹在一个 `Optional` 类型中。              |

#### 14.2.3. 地理空间（Geo） Repository 查询

正如你在前面的关键字表中看到的，有几个关键字在MongoDB查询中触发了地理空间操作。`Near` 关键字允许一些进一步的修改，正如接下来的几个例子所示。

下面的例子显示了如何定义一个 `near` 查询，找到与给定点有一定距离的所有人员。

Example 143. 高级 `Near` 查询

```java
public interface PersonRepository extends MongoRepository<Person, String> {

  // { 'location' : { '$near' : [point.x, point.y], '$maxDistance' : distance}}
  List<Person> findByLocationNear(Point location, Distance distance);
}
```

在查询方法中添加一个 `Distance` 参数可以将结果限制在给定的距离内。如果 `Distance` 被设置为包含一个 `Metric`，我们会透明地使用 `$nearSphere` 而不是 `$code`，如下例所示。

Example 144. 使用 `Distance` 和 `Metrics`

```java
Point point = new Point(43.7, 48.8);
Distance distance = new Distance(200, Metrics.KILOMETERS);
… = repository.findByLocationNear(point, distance);
// {'location' : {'$nearSphere' : [43.7, 48.8], '$maxDistance' : 0.03135711885774796}}
```

使用带有 `Metric` 的 `Distance` 会导致添加一个 `$nearSphere`（而不是普通的 `$near`）子句。除此之外，实际的距离会根据所使用的 `Metrics` 来计算。

(注意，`Metric` 并不是指公制计量单位。它可能是英里而不是公里。相反，`metric` 指的是一个测量系统的概念，不管你使用哪种系统)。

|      | 在目标属性上使用 `@GeoSpatialIndexed(type = GeoSpatialIndexType.GEO_2DSPHERE)` 可以强制使用 `$nearSphere` 操作符。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 地理附近（Geo-near）查询

Spring Data MongoDb支持地理范围内的查询，如下例所示。

```java
public interface PersonRepository extends MongoRepository<Person, String> {

  // {'geoNear' : 'location', 'near' : [x, y] }
  GeoResults<Person> findByLocationNear(Point location);

  // No metric: {'geoNear' : 'person', 'near' : [x, y], maxDistance : distance }
  // Metric: {'geoNear' : 'person', 'near' : [x, y], 'maxDistance' : distance,
  //          'distanceMultiplier' : metric.multiplier, 'spherical' : true }
  GeoResults<Person> findByLocationNear(Point location, Distance distance);

  // Metric: {'geoNear' : 'person', 'near' : [x, y], 'minDistance' : min,
  //          'maxDistance' : max, 'distanceMultiplier' : metric.multiplier,
  //          'spherical' : true }
  GeoResults<Person> findByLocationNear(Point location, Distance min, Distance max);

  // {'geoNear' : 'location', 'near' : [x, y] }
  GeoResults<Person> findByLocationNear(Point location);
}
```

#### 14.2.4. MongoDB基于JSON的查询方法和字段限制

通过将 `org.springframework.data.mongodb.repository.Query` 注解添加到你的 repository 查询方法中，你可以指定一个 MongoDB JSON 查询字符串来使用，而不是让查询从方法名称中导出，如下例所示。

```java
public interface PersonRepository extends MongoRepository<Person, String> {

  @Query("{ 'firstname' : ?0 }")
  List<Person> findByThePersonsFirstname(String firstname);

}
```

占位符 `?0` 让你把方法参数的值替换到JSON查询字符串中。

|      | `String` 参数值在绑定过程中被转义，这意味着不可能通过参数添加MongoDB特定的操作符。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

你也可以使用过滤器属性来限制被映射到Java对象中的属性集，如下例所示。

```java
public interface PersonRepository extends MongoRepository<Person, String> {

  @Query(value="{ 'firstname' : ?0 }", fields="{ 'firstname' : 1, 'lastname' : 1}")
  List<Person> findByThePersonsFirstname(String firstname);

}
```

前面例子中的查询只返回 `Person` 对象的 `firstname`、`lastname` 和 `Id` 属性。`age` 属性是一个 `java.lang.Integer`，没有被设置，因此它的值是 null。

#### 14.2.5. 对查询方法的结果进行排序

MongoDB repository 允许用各种方法来定义排序顺序。让我们看一下下面的例子。

Example 145. 对结果进行排序

```java
public interface PersonRepository extends MongoRepository<Person, String> {

  List<Person> findByFirstnameSortByAgeDesc(String firstname); 

  List<Person> findByFirstname(String firstname, Sort sort);   

  @Query(sort = "{ age : -1 }")
  List<Person> findByFirstname(String firstname);              

  @Query(sort = "{ age : -1 }")
  List<Person> findByLastname(String lastname, Sort sort);     
}
```

|      | 静态排序源于方法名称。`SortByAgeDesc` 的结果是 `{ age : -1 }` 为排序参数。 |
| ---- | ------------------------------------------------------------ |
|      | 使用一个方法参数进行动态排序。`Sort.by(DESC, "age")` 为排序参数创建 `{ age : -1 }`。 |
|      | 通过 `Query` 注解进行静态排序。按照 `sort` 属性中的规定应用排序参数。 |
|      | 通过 `Query` 注解的默认排序与通过方法参数的动态排序相结合。`Sort.unsorted()` 的结果是 `{ age : -1 }`。使用 `Sort.by(ASC, "age")` 覆盖了默认值并创建了 `{ age : 1 }`。`Sort.by(ASC, "firstname")` 改变了默认值，结果是 `{ age : -1, firstname : 1 }`。 |

#### 14.2.6. 用SpEL表达式进行基于JSON的查询

查询字符串和字段定义可与SpEL表达式一起使用，以便在运行时创建动态查询。SpEL表达式可以提供谓词值（predicate value），并可用于用子文档扩展谓词。

表达式通过一个包含所有参数的数组暴露方法参数。下面的查询使用 `[0]` 来声明 `lastname` 的谓语值（predicate value）（相当于 `?0` 的参数绑定）。

```java
public interface PersonRepository extends MongoRepository<Person, String> {

  @Query("{'lastname': ?#{[0]} }")
  List<Person> findByQueryWithExpression(String param0);
}
```

表达式可以用来调用函数、评估条件和构造值。与JSON一起使用的SpEL表达式揭示了一个副作用，因为SpEL内部的类似Map的声明读起来就像JSON一样，正如下面的例子所示。

```java
public interface PersonRepository extends MongoRepository<Person, String> {

  @Query("{'id': ?#{ [0] ? {$exists :true} : [1] }}")
  List<Person> findByQueryWithExpressionAndNestedObject(boolean param0, String param1);
}
```

|      | 查询字符串中的SpEL可以是增强查询的一种强大方式。然而，它们也可以接受广泛的不需要的参数。确保在将字符串传递给查询之前对其进行脱敏，以避免产生漏洞或对你的查询进行不必要的改变。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

表达式支持可通过查询SPI：`org.springframework.data.repository.query.spi.EvaluationContextExtension` 来扩展。查询SPI可以贡献属性和函数，并可以定制根对象。扩展是在构建查询时，在SpEL评估时从 application context 中检索出来的。下面的例子显示了如何使用 `EvaluationContextExtension`。

```java
public class SampleEvaluationContextExtension extends EvaluationContextExtensionSupport {

  @Override
  public String getExtensionId() {
    return "security";
  }

  @Override
  public Map<String, Object> getProperties() {
    return Collections.singletonMap("principal", SecurityContextHolder.getCurrent().getPrincipal());
  }
}
```

|      | 自己启动 `MongoRepositoryFactory` 并不了解应用程序的上下文，需要进一步配置以获取 Query SPI 扩展。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 响应式查询方法可以利用 `org.springframework.data.spel.spi.ReactiveEvaluationContextExtension`. |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 14.2.7. 类型安全的查询方法

MongoDB repository 支持与 [Querydsl](http://www.querydsl.com/) 项目集成，后者提供了一种执行类型安全查询的方法。引用项目描述中的话，"不是把查询写成内联字符串或外化成XML文件，而是通过 fluent API来构建。" 它提供了以下功能。

- 在IDE中的代码补全（所有的属性、方法和操作都可以在你喜欢的Java IDE中展开）。
- 几乎不允许语法上的无效查询（在所有层面上都是类型安全的）。
- Domain类型和属性可以被安全引用—不涉及字符串
- 能更好地适应Domain类型的重构变化。
- 增量查询的定义更容易。

关于如何使用Maven或Ant为基于APT的代码生成启动环境，请参阅 [QueryDSL文档](http://www.querydsl.com/static/querydsl/latest/reference/html/)。

QueryDSL可以让你编写如下的查询。

```java
QPerson person = new QPerson("person");
List<Person> result = repository.findAll(person.address.zipCode.eq("C0123"));

Page<Person> page = repository.findAll(person.lastname.contains("a"),
                                       PageRequest.of(0, 2, Direction.ASC, "lastname"));
```

`QPerson` 是一个由Java注解后处理工具生成的类。它是一个 `Predicate`，可以让你编写类型安全的查询。请注意，除了 `C0123` 的值之外，查询中没有任何字符串。

你可以通过使用 `QuerydslPredicateExecutor` 接口来使用生成的 `Predicate` 类，下面的列表显示了这一点。

```java
public interface QuerydslPredicateExecutor<T> {

  T findOne(Predicate predicate);

  List<T> findAll(Predicate predicate);

  List<T> findAll(Predicate predicate, OrderSpecifier<?>... orders);

  Page<T> findAll(Predicate predicate, Pageable pageable);

  Long count(Predicate predicate);
}
```

要在你的 repository 实现中使用它，需要把它添加到你的接口所继承的 repository 接口列表中，如下面的例子所示。

```java
public interface PersonRepository extends MongoRepository<Person, String>, QuerydslPredicateExecutor<Person> {

   // additional query methods go here
}
```

#### 14.2.8. 全文检索查询

MongoDB的全文搜索功能是特定于store的，因此，可以在 `MongoRepository` 上找到，而不是在更通用的 `CrudRepository` 上。我们需要一个有全文索引的文档（参见 “[文本索引](https://springdoc.cn/spring-data-mongodb/#mapping-usage-indexes.text-index)” 以了解如何创建全文索引）。

`MongoRepository` 上的其他方法将 `TextCriteria` 作为一个输入参数。除了这些明确的方法之外，还可以添加一个 `TextCriteria` 衍生的 repository 方法。这些标准（criteria）被添加为额外的 `AND` 标准。一旦实体包含一个 `@TextScore` 注解的属性，就可以检索到文档的全文得分。此外，`@TextScore` 注解也使得按文档的分数排序成为可能，正如下面的例子所示。

```java
@Document
class FullTextDocument {

  @Id String id;
  @TextIndexed String title;
  @TextIndexed String content;
  @TextScore Float score;
}

interface FullTextRepository extends Repository<FullTextDocument, String> {

  // Execute a full-text search and define sorting dynamically
  List<FullTextDocument> findAllBy(TextCriteria criteria, Sort sort);

  // Paginate over a full-text search result
  Page<FullTextDocument> findAllBy(TextCriteria criteria, Pageable pageable);

  // Combine a derived query with a full-text search
  List<FullTextDocument> findByTitleOrderByScoreDesc(String title, TextCriteria criteria);
}


Sort sort = Sort.by("score");
TextCriteria criteria = TextCriteria.forDefaultLanguage().matchingAny("spring", "data");
List<FullTextDocument> result = repository.findAllBy(criteria, sort);

criteria = TextCriteria.forDefaultLanguage().matching("film");
Page<FullTextDocument> page = repository.findAllBy(criteria, PageRequest.of(1, 1, sort));
List<FullTextDocument> result = repository.findByTitleOrderByScoreDesc("mongodb", criteria);
```

#### 14.2.9. 投影

Spring Data的查询方法通常会返回由 repository 管理的聚合根(aggregate root)的一个或多个实例。 然而，有时可能需要根据这些类型的某些属性来创建投影。 Spring Data允许建模专用的返回类型，以更有选择地检索管理的聚合体（aggregate）的部分视图。

想象一下，一个repository和聚合根类型，如下面的例子。

Example 146. aggregate 和 repository 的实例

```java
class Person {

  @Id UUID id;
  String firstname, lastname;
  Address address;

  static class Address {
    String zipCode, city, street;
  }
}

interface PersonRepository extends Repository<Person, UUID> {

  Collection<Person> findByLastname(String lastname);
}
```

现在想象一下，我们只想检索这个人的 name 属性。Spring Data提供了什么手段来实现这一点？本章的其余部分将回答这个问题。

##### 基于接口的投影

将查询结果限制在只有名称属性的最简单的方法是声明一个接口，为要读取的属性公开 accessor 方法，如下面的例子中所示。

Example 147. 检索一个属性子集的投影接口

```java
interface NamesOnly {

  String getFirstname();
  String getLastname();
}
```

这里重要的一点是，这里定义的属性与聚合根（aggregate root）中的属性完全匹配。这样做可以添加一个查询方法，如下所示。

Example 148. 一个使用基于接口的投影与查询方法的 repository

```java
interface PersonRepository extends Repository<Person, UUID> {

  Collection<NamesOnly> findByLastname(String lastname);
}
```

查询执行引擎在运行时为每个返回的元素创建该接口的代理实例，并将对公开方法的调用转发给目标对象。

|      | 在你的 `Repository` 中声明一个复写base方法的方法（例如在 `CrudRepository` 中声明的，一个特定store的repository接口，或 `Simple…Repository`），会导致对base方法的调用，不管声明的返回类型如何。请确保使用一个兼容的返回类型，因为base方法不能用于投影。一些store模块支持 `@Query` 注解，将重载的base方法变成查询方法，然后可以用来返回投影。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

投影可以被递归使用。如果你想同时包括一些 `Address` 信息，为其创建一个投影接口，并从 `getAddress()` 的声明中返回该接口，如以下例子所示。

Example 149. 检索一个属性子集的投影接口

```java
interface PersonSummary {

  String getFirstname();
  String getLastname();
  AddressSummary getAddress();

  interface AddressSummary {
    String getCity();
  }
}
```

在方法调用时，目标实例的 `address` 属性被获取，并依次封装成一个投影代理。

###### 封闭的投影

一个投影接口，其访问器（accessor）方法都与目标集合的属性相匹配，被认为是一个封闭的投影。下面的例子（我们在本章前面也用过）就是一个封闭投影。

Example 150. 一个封闭投影

```java
interface NamesOnly {

  String getFirstname();
  String getLastname();
}
```

如果你使用一个封闭的投影，Spring Data可以优化查询的执行，因为我们知道所有需要支持投影代理的属性。关于这一点的更多细节，请参见参考文档中的特定模块部分。

###### 开放的投影

投影接口中的访问器方法也可以通过使用 `@Value` 注解来计算新的值，如下面的例子中所示。

Example 151. 开放的投影

```java
interface NamesOnly {

  @Value("#{target.firstname + ' ' + target.lastname}")
  String getFullName();
  …
}
```

支持投影的聚合根（aggregate root）在 `target` 变量中可用。使用 `@Value` 的投影接口是一个开放的投影。在这种情况下，Spring Data不能应用查询执行优化，因为SpEL表达式可以使用聚合根的任何属性。

在 `@Value` 中使用的表达式不应过于复杂—你要避免在 `String` 变量中编程。对于非常简单的表达式，一种选择可能是求助于默认方法（在Java 8中引入），如下面的例子所示。

Example 152. 一个使用默认方法的自定义逻辑的投影接口

```java
interface NamesOnly {

  String getFirstname();
  String getLastname();

  default String getFullName() {
    return getFirstname().concat(" ").concat(getLastname());
  }
}
```

这种方法要求你能够纯粹地根据投影接口上暴露的其他访问器方法来实现逻辑。第二个更灵活的选择是在Spring Bean中实现自定义逻辑，然后从SpEL表达式中调用该逻辑，如下面的例子所示。

Example 153. Sample Person object

```java
@Component
class MyBean {

  String getFullName(Person person) {
    …
  }
}

interface NamesOnly {

  @Value("#{@myBean.getFullName(target)}")
  String getFullName();
  …
}
```

请注意SpEL表达式是如何引用 `myBean` 并调用 `getFullName(…)` 方法和转发投影目标作为方法参数的。由SpEL表达式评估支持的方法也可以使用方法参数，然后可以从表达式中引用这些参数。方法参数可以通过一个名为 `args` 的 `Object` 数组获得。下面的例子显示了如何从 `args` 数组中获取方法参数。

Example 154. Sample Person object

```java
interface NamesOnly {

  @Value("#{args[0] + ' ' + target.firstname + '!'}")
  String getSalutation(String prefix);
}
```

同样，对于更复杂的表达式，你应该使用Spring Bean并让表达式调用一个方法，如 [前](https://springdoc.cn/spring-data-mongodb/#projections.interfaces.open.bean-reference)所述。

###### Nullable Wrapper

投影接口中的Getter可以使用nullable的wrapper，以提高null的安全性。目前支持的wrapper类型有。

- `java.util.Optional`
- `com.google.common.base.Optional`
- `scala.Option`
- `io.vavr.control.Option`

Example 155. 使用 nullable wrapper 的投影接口

```java
interface NamesOnly {

  Optional<String> getFirstname();
}
```

如果底层投影值不是 `null` 的，那么将使用 wrapper 类型的当前表示法返回值。如果持有的值是 `null` 的，那么 getter 方法会返回所使用的 wrapper 类型的 `null` 表示。

##### 基于类的投影（DTO）

另一种定义投影的方法是使用value类型的DTO（数据传输对象），它持有应该被检索的字段的属性。这些DTO类型的使用方式与投影接口的使用方式完全相同，只是没有代理发生，也不能应用嵌套投影。

如果store通过限制要加载的字段来优化查询的执行，要加载的字段是由暴露出来的构造函数的参数名决定的。

下面的例子显示了一个投影的DTO。

Example 156. 投影 DTO

```java
class NamesOnly {

  private final String firstname, lastname;

  NamesOnly(String firstname, String lastname) {

    this.firstname = firstname;
    this.lastname = lastname;
  }

  String getFirstname() {
    return this.firstname;
  }

  String getLastname() {
    return this.lastname;
  }

  // equals(…) and hashCode() implementations
}
```

|      | 避免为投影DTO编写模板代码你可以通过使用 [Project Lombok](https://projectlombok.org/) 大大简化DTO的代码，它提供了一个 `@Value` 注解（不要与前面接口例子中显示的Spring的 `@Value` 注解混淆）。如果你使用Project Lombok的 `@Value` 注解，前面显示的DTO示例将变成如下。`@Value class NamesOnly { String firstname, lastname; } `默认情况下，字段是 `private final` 的，并且该类暴露了一个构造函数，该函数接收所有字段，并自动获得 `equals(…)` 和 `hashCode()` 方法的实现。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 动态投影

到目前为止，我们已经使用投影类型作为集合的返回类型或元素类型。然而，你可能想在调用时选择要使用的类型（这使它成为动态的）。为了应用动态投影，请使用一个查询方法，如下面的例子中所示。

Example 157. 使用动态投影参数的repository

```java
interface PersonRepository extends Repository<Person, UUID> {

  <T> Collection<T> findByLastname(String lastname, Class<T> type);
}
```

这样，该方法可用于获得原样或应用投影的聚合体（aggregate），如以下例子所示。

Example 158. 使用具有动态投影的repository

```java
void someMethod(PersonRepository people) {

  Collection<Person> aggregates =
    people.findByLastname("Matthews", Person.class);

  Collection<NamesOnly> aggregates =
    people.findByLastname("Matthews", NamesOnly.class);
}
```

|      | 对 `Class` 类型的查询参数进行检查，看它们是否符合动态投影参数的条件。如果查询的实际返回类型与 `Class` 参数的通用参数类型相同，那么匹配的 `Class` 参数就不能在查询或SpEL表达式中使用。如果你想使用一个 `Class` 参数作为查询参数，那么请确保使用不同的通用参数，例如 `Class<?>`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 14.2.10. Repository 聚合方法

repository 层提供了通过注解的 repository 查询方法与 [聚合框架](https://springdoc.cn/spring-data-mongodb/#mongo.aggregation) 交互的手段。与 [基于JSON的查询](https://springdoc.cn/spring-data-mongodb/#mongodb.repositories.queries.json-based) 类似，你可以使用 `org.springframework.data.mongodb.repository.Aggregation` 注解定义一个管道。该定义可以包含简单的占位符，如 `?0`，以及 [SpEL表达式](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#expressions) `?#{ … }`。

1. Repository 聚合方法

```java
public interface PersonRepository extends CrudRepository<Person, String> {

  @Aggregation("{ $group: { _id : $lastname, names : { $addToSet : $firstname } } }")
  List<PersonAggregate> groupByLastnameAndFirstnames();                            

  @Aggregation("{ $group: { _id : $lastname, names : { $addToSet : $firstname } } }")
  List<PersonAggregate> groupByLastnameAndFirstnames(Sort sort);                   

  @Aggregation("{ $group: { _id : $lastname, names : { $addToSet : ?0 } } }")
  List<PersonAggregate> groupByLastnameAnd(String property);                       

  @Aggregation("{ $group: { _id : $lastname, names : { $addToSet : ?0 } } }")
  Slice<PersonAggregate> groupByLastnameAnd(String property, Pageable page);       

  @Aggregation("{ $group: { _id : $lastname, names : { $addToSet : $firstname } } }")
  Stream<PersonAggregate> groupByLastnameAndFirstnamesAsStream();                  

  @Aggregation("{ $group : { _id : null, total : { $sum : $age } } }")
  SumValue sumAgeUsingValueWrapper();                                              

  @Aggregation("{ $group : { _id : null, total : { $sum : $age } } }")
  Long sumAge();                                                                   

  @Aggregation("{ $group : { _id : null, total : { $sum : $age } } }")
  AggregationResults<SumValue> sumAgeRaw();                                        

  @Aggregation("{ '$project': { '_id' : '$lastname' } }")
  List<String> findAllLastnames();                                                 
}
public class PersonAggregate {

  private @Id String lastname;                                                     
  private List<String> names;

  public PersonAggregate(String lastname, List<String> names) {
     // ...
  }

  // Getter / Setter omitted
}

public class SumValue {

  private final Long total;                                                         

  public SumValue(Long total) {
    // ...
  }

  // Getter omitted
}
```

|      | 聚合管道，在 `Person` 集合中通过 `lastname` 对名字进行分组，将这些名字作为 `PersonAggregate` 返回。 |
| ---- | ------------------------------------------------------------ |
|      | 如果存在 `Sort` 参数，`$sort` 被附加在已声明的管道阶段之后，这样它只影响到通过所有其他聚合阶段后的最终结果的顺序。因此，`Sort` 属性被映射到返回类型为 `PersonAggregate` 的方法上，该方法将 `Sort.by("lastname")` 变成 `{ $sort : { '_id', 1 } }`，因为 `PersonAggregate.lastname` 是用 `@Id` 来注解的。 |
|      | 用给定的 `property` 值替换动态聚合管道的 `?0`。              |
|      | `$skip`、`$limit` 和 `$sort` 可以通过一个 `Pageable` 参数来传递。和<2>中一样，操作符被附加到管道定义中。接受 `Pageable` 的方法可以返回 `Slice` 以方便分页。 |
|      | 聚合方法可以返回 `Stream`，直接从底层游标中消费结果。确保在消费完流之后关闭它，通过调用 `close()` 或通过 `try-with-resources` 释放服务器端的游标。 |
|      | 将返回单个 `Document` 的聚合结果映射到所需的 `SumValue` 目标类型的一个实例。 |
|      | 聚合产生的单个文档只持有一个累积结果，比如 `$sum` 可以直接从结果 `Document` 中提取。为了获得更多的控制，你可以考虑将 `AggregationResult` 作为方法的返回类型，如<7>所示。 |
|      | 获取映射到通用目标包装类型 `SumValue` 或 `org.bson.Document` 的原始 `AggregationResults`。 |
|      | 像<6>中一样，一个单一的值可以直接从多个结果 `Document` 中获得。 |

在某些情况下，聚合可能需要额外的选项，比如最大运行时间、额外的日志注释，或者允许暂时将数据写入磁盘。使用 `@Meta` 注解，通过 `maxExecutionTimeMs`、`comment` 或 `allowDiskUse` 来设置这些选项。

```java
interface PersonRepository extends CrudRepository<Person, String> {

  @Meta(allowDiskUse = true)
  @Aggregation("{ $group: { _id : $lastname, names : { $addToSet : $firstname } } }")
  List<PersonAggregate> groupByLastnameAndFirstnames();
}
```

或者使用 `@Meta` 来创建你自己的注解，如下面的例子所示。

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ ElementType.METHOD })
@Meta(allowDiskUse = true)
@interface AllowDiskUse { }

interface PersonRepository extends CrudRepository<Person, String> {

  @AllowDiskUse
  @Aggregation("{ $group: { _id : $lastname, names : { $addToSet : $firstname } } }")
  List<PersonAggregate> groupByLastnameAndFirstnames();
}
```

|      | 你也可以将 `@Aggregation` 用于 [Reactive Repository](https://springdoc.cn/spring-data-mongodb/#mongo.reactive.repositories)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 简单类型的单一结果检查返回的 `Document`，并检查以下内容。文档中只有一个条目，请退回。两个条目，一个是 `_id` 值。返回另一个。返回为第一个可分配给返回类型的值。如果上述情况都不适用，则抛出一个异常。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 使用 `@Aggregation` 的 repository 方法不支持 `Page` 的返回类型。然而，你可以使用 `Pageable` 参数，将 `$skip`、`$limit` 和 `$sort` 加入管道，让方法返回 `Slice`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 14.3. CDI 整合

repository 接口的实例通常由容器创建，而Spring是与Spring Data合作时最自然的选择。从1.3.0版本开始，Spring Data MongoDB带有一个自定义的CDI扩展，可以让你在CDI环境中使用 repository 抽象。该扩展是JAR的一部分。要激活它，请将Spring Data MongoDB JAR放入你的classpath。现在你可以通过为 `MongoTemplate` 实现CDI Producer来设置基础设施，如下面的例子所示。

```java
class MongoTemplateProducer {

    @Produces
    @ApplicationScoped
    public MongoOperations createMongoTemplate() {

        MongoDatabaseFactory factory = new SimpleMongoClientDatabaseFactory(MongoClients.create(), "database");
        return new MongoTemplate(factory);
    }
}
```

Spring Data MongoDB CDI扩展将 `MongoTemplate` 作为CDI Bean使用，并在容器请求 repository 类型的bean时为 Spring Data Repository 创建一个代理。因此，获取Spring Data Repository 的实例只是声明一个 `@Inject` 属性的问题，正如下面的例子所示。

```java
class RepositoryClient {

  @Inject
  PersonRepository repository;

  public void businessMethod() {
    List<Person> people = repository.findAll();
  }
}
```

## 15. 响应式 MongoDB Repository

本章描述了 MongoDB 的响应式 repository 支持的特性。本章建立在 [与 Spring Data Repository 一起工作](https://springdoc.cn/spring-data-mongodb/#repositories) 中解释的核心 repository 支持的基础上。你应该对那里解释的基本概念有一个正确的理解。

### 15.1. 响应式组合库

响应式空间提供了各种响应式组合库。最常见的库是 [RxJava](https://github.com/ReactiveX/RxJava) 和 [Project Reactor](https://projectreactor.io/)。

Spring Data MongoDB建立在 [MongoDB Reactive Streams](https://mongodb.github.io/mongo-java-driver-reactivestreams/) 驱动之上，通过依赖 [Reactive Streams](https://www.reactive-streams.org/) 倡议来提供最大的互操作性。静态API，如 `ReactiveMongoOperations`，是通过使用Project Reactor的 `Flux` 和 `Mono` 类型提供的。Project Reactor 提供了各种适配器（adapters）来转换响应式包装类型（`Flux` 到 `Observable`，反之亦然），但转换很容易使你的代码变得混乱。

Spring Data 的 Repository 抽象是一个动态的API，主要由你和你的需求在你声明查询方法时定义。响应式MongoDB Repository 可以通过使用 RxJava 或 Project Reactor 包装类型，从以下库的特定存 Repository 接口中扩展实现。

- `ReactiveCrudRepository`
- `ReactiveSortingRepository`
- `RxJava2CrudRepository`
- `RxJava2SortingRepository`
- `RxJava3CrudRepository`
- `RxJava3SortingRepository`

Spring Data在幕后转换了响应式包装类型，因此你可以坚持使用你最喜欢的组合库。

### 15.2. 用法

要访问存储在MongoDB数据库中的domain实体，你可以使用我们复杂的repository支持，它可以大大缓解实现这些实体。要做到这一点，为你的repository创建一个类似的接口。不过在这之前，你需要一个实体，比如下面这个例子中定义的实体。

Example 159. 简单的 `Person` 实体

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

请注意，前面的例子中定义的实体有一个名为 `id` 的 `String` 类型的属性。 `MongoTemplate` 中使用的默认序列化机制（它支持 repository）将名为 `id` 的属性视为文档 ID。目前，我们支持 `String`、`ObjectId` 和 `BigInteger` 作为 `id` 类型。请参阅 [ID映射](https://springdoc.cn/spring-data-mongodb/#mongo-template.id-handling) 以了解更多关于在映射层中如何处理 `id` 字段的信息。

下面的例子显示了如何创建一个接口，定义针对前述例子中的 `Person` 对象的查询。

Example 160. 持久保存 Person 实体的基本 repository 接口

```
public interface ReactivePersonRepository extends ReactiveSortingRepository<Person, String> {

  Flux<Person> findByFirstname(String firstname);                                   

  Flux<Person> findByFirstname(Publisher<String> firstname);                        

  Flux<Person> findByFirstnameOrderByLastname(String firstname, Pageable pageable); 

  Mono<Person> findByFirstnameAndLastname(String firstname, String lastname);       

  Mono<Person> findFirstByLastname(String lastname);                                
}
```

|      | 该方法显示了对所有具有给定 `lastname` 的人的查询。该查询是通过解析方法名称中可以与 `And` 和 `Or` 相连接的约束条件而得到的。因此，方法名称的结果是一个查询表达式 `{"lastname" : lastname}`。 |
| ---- | ------------------------------------------------------------ |
|      | 一旦给定的 `Publisher` 发出 `firstname` ，该方法将显示对所有具有给定 `firstname` 的人的查询。 |
|      | 使用 `Pageable` 向数据库传递 offset 和 sort 参数。           |
|      | 为给定的 criteria 寻找一个单一的实体。对于非唯一的结果，抛出 `IncorrectResultSizeDataAccessException`。 |
|      | 除非<4>，否则即使查询产生了更多的结果文档，第一个实体也总是被发出。 |

对于Java配置，使用 `@EnableReactiveMongoRepositories` 注解。该注解带有与命名空间元素相同的属性。如果没有配置基础包（base package），基础设施会扫描注解的配置类的包。

|      | MongoDB使用两种不同的驱动程序来进行强制性（同步/阻塞）和反应性（非阻塞）数据访问。您必须通过使用Reactive Streams驱动程序创建连接，以便为Spring Data的Reactive MongoDB支持提供所需的基础设施。因此，您必须为MongoDB的Reactive Streams驱动程序提供单独的配置。注意，如果你使用反应式和阻塞式Spring Data MongoDB模板和repository，你的应用程序会在两个不同的连接上运行。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

下面的列表显示了如何为一个 repository 使用Java配置。

Example 161. Repository 的 Java 配置

```java
@Configuration
@EnableReactiveMongoRepositories
class ApplicationConfig extends AbstractReactiveMongoConfiguration {

  @Override
  protected String getDatabaseName() {
    return "e-store";
  }

  @Override
  public MongoClient reactiveMongoClient() {
    return MongoClients.create();
  }

  @Override
  protected String getMappingBasePackage() {
    return "com.oreilly.springdata.mongodb";
  }
}
```

因为我们的 domain repository 继承了 `ReactiveSortingRepository`，它为你提供了CRUD操作以及对实体的排序访问方法。如下面的例子所示，使用 repository 实例是将其依赖注入到客户端的问题。

Example 162. 对 Person 实体的排序访问

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration
class PersonRepositoryTests {

    @Autowired ReactivePersonRepository repository;

    @Test
    public void sortsElementsCorrectly() {
      Flux<Person> persons = repository.findAll(Sort.by(new Order(ASC, "lastname")));
    }
}
```

|      | `Page` 的返回类型（如 `Mono<Page>`）不被响应式 repository 所支持。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

可以在派生的查找方法中使用 `Pageable`，将 `sort`、`limit` 和 `offset` 参数传递给查询，以减少负载和网络流量。返回的 `Flux` 将只发出声明范围内的数据。

Example 163. 使用响应式存储库的 Limit 和 Offset

```java
Pageable page = PageRequest.of(1, 10, Sort.by("lastname"));
Flux<Person> persons = repository.findByFirstnameOrderByLastname("luke", page);
```

### 15.3. 特性

与阻塞式 [MongoDB Repository](https://springdoc.cn/spring-data-mongodb/#mongo.repositories) 相比，Spring Data的Reactive MongoDB支持的功能集有所减少。

它支持以下功能。

- [使用字符串查询和查询派生的查询方法](https://springdoc.cn/spring-data-mongodb/#mongodb.repositories.queries)
- [地理空间 Repository 查询](https://springdoc.cn/spring-data-mongodb/#mongodb.reactive.repositories.queries.geo-spatial)
- [Repository Delete 查询](https://springdoc.cn/spring-data-mongodb/#mongodb.repositories.queries.delete)
- [MongoDB基于JSON的查询方法和字段限制](https://springdoc.cn/spring-data-mongodb/#mongodb.repositories.queries.json-based)
- [全文检索查询](https://springdoc.cn/spring-data-mongodb/#mongodb.repositories.queries.full-text)
- [类型安全的查询方法](https://springdoc.cn/spring-data-mongodb/#mongodb.reactive.repositories.queries.type-safe)
- [投影](https://springdoc.cn/spring-data-mongodb/#projections)

#### 15.3.1. 地理空间 Repository 查询

正如你在前面的 “[地理空间 Repository 查询](https://springdoc.cn/spring-data-mongodb/#mongodb.reactive.repositories.queries.geo-spatial)” 中所看到的，一些关键字在MongoDB查询中会触发地理空间操作。 `Near` 关键字允许一些进一步的修改，正如接下来的几个例子所示。

下面的例子显示了如何定义一个 `near` 查询，找到与给定点有一定距离的所有 person。

Example 164. 高级 `Near` 查询

```java
interface PersonRepository extends ReactiveMongoRepository<Person, String> {

  // { 'location' : { '$near' : [point.x, point.y], '$maxDistance' : distance}}
  Flux<Person> findByLocationNear(Point location, Distance distance);
}
```

在查询方法中添加一个 `Distance` 参数可以将结果限制在给定的距离内。如果 `Distance` 被设置为包含一个 `Metric`，我们会透明地使用 `$nearSphere` 而不是 `$code`，正如下面的例子所示。

Example 165. 使用 `Distance` 与 `Metrics`。

```java
Point point = new Point(43.7, 48.8);
Distance distance = new Distance(200, Metrics.KILOMETERS);
… = repository.findByLocationNear(point, distance);
// {'location' : {'$nearSphere' : [43.7, 48.8], '$maxDistance' : 0.03135711885774796}}
```

|      | 响应应式地理空间 repository 查询支持doamin类型和响应式包装类型中的 `GeoResult<T>` 结果。不支持 `GeoPage` 和 `GeoResults`，因为它们与预先计算平均距离的递延结果方法相矛盾。不过，你仍然可以传入一个 `Pageable` 参数来自行翻阅结果。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

使用带有 `Metric` 的 `Distance` 会导致添加一个 `$nearSphere`（而不是普通的 `$near`）子句。除此之外，实际的距离会根据所使用的 `Metrics` 来计算。

(请注意，`Metric` 并不是指公制的计量单位。它可能是英里而不是公里。相反，`metric` 指的是一个测量系统的概念，不管你使用哪种系统。)

|      | Using `@GeoSpatialIndexed(type = GeoSpatialIndexType.GEO_2DSPHERE)` on the target property forces usage of `$nearSphere` operator. |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 地理附近查询

Spring Data MongoDB 支持地理附近查询，如下面的例子所示。

```java
interface PersonRepository extends ReactiveMongoRepository<Person, String>  {

  // {'geoNear' : 'location', 'near' : [x, y] }
  Flux<GeoResult<Person>> findByLocationNear(Point location);

  // No metric: {'geoNear' : 'person', 'near' : [x, y], maxDistance : distance }
  // Metric: {'geoNear' : 'person', 'near' : [x, y], 'maxDistance' : distance,
  //          'distanceMultiplier' : metric.multiplier, 'spherical' : true }
  Flux<GeoResult<Person>> findByLocationNear(Point location, Distance distance);

  // Metric: {'geoNear' : 'person', 'near' : [x, y], 'minDistance' : min,
  //          'maxDistance' : max, 'distanceMultiplier' : metric.multiplier,
  //          'spherical' : true }
  Flux<GeoResult<Person>> findByLocationNear(Point location, Distance min, Distance max);

  // {'geoNear' : 'location', 'near' : [x, y] }
  Flux<GeoResult<Person>> findByLocationNear(Point location);
}
```

#### 15.3.2. 类型安全的查询方法

响应式 MongoDB repository 支持与 [Querydsl](http://www.querydsl.com/) 项目集成，后者提供了一种执行类型安全查询的方法。

> 不是把查询写成内联字符串或外化成XML文件，而是通过一个 fluent API来构建。

— Querydsl 团队

它提供了以下功能。

- 在IDE中的代码补全（所有的属性、方法和操作都可以在你喜欢的Java IDE中展开）。
- 几乎不允许语法上的无效查询（在所有层面上都是类型安全的）。
- Domain的类型和属性可以被安全地引用—不涉及字符串的问题。
- 能更好地适应domain类型的重构变化。
- 增量查询的定义更容易。

关于如何使用 Maven 或Ant为基于APT的代码生成启动环境，请参阅 [Querydsl文档](http://www.querydsl.com/static/querydsl/latest/reference/html/)。

Querydsl repository 支持让你编写和运行查询，例如以下内容。

```java
QPerson person = QPerson.person;

Flux<Person> result = repository.findAll(person.address.zipCode.eq("C0123"));
```

`QPerson` 是一个由Java注释后处理工具生成的类。它是一个 `Predicate`，可以让你编写类型安全的查询。请注意，除了 `C0123` 值之外，查询中没有任何字符串。

你可以通过使用 `ReactiveQuerydslPredicateExecutor` 接口来使用生成的 `Predicate` 类，下面的列表显示了这一点。

Example 166. Reactive Querydsl 的网关 - ReactiveQuerydslPredicateExecutor

```java
interface ReactiveQuerydslPredicateExecutor<T> {

	Mono<T> findOne(Predicate predicate);

	Flux<T> findAll(Predicate predicate);

	Flux<T> findAll(Predicate predicate, Sort sort);

	Flux<T> findAll(Predicate predicate, OrderSpecifier<?>... orders);

	Flux<T> findAll(OrderSpecifier<?>... orders);

	Mono<Long> count(Predicate predicate);

	Mono<Boolean> exists(Predicate predicate);
}
```

要在你的 repository 实现中使用它，需要把它添加到你的接口所继承的 repository 接口列表中，如下面的例子所示。

Example 167. Reactive Querydsl Respository Declaration

```java
interface PersonRepository extends ReactiveMongoRepository<Person, String>, ReactiveQuerydslPredicateExecutor<Person> {

   // additional query methods go here
}
```

|      | 请注意，响应式 MongoDB，不支持连接（DBRef）。 |
| ---- | --------------------------------------------- |
|      |                                               |

## 16. 审计

### 16.1. 基础

Spring Data提供了复杂的支持，可以透明地跟踪谁创建或更改了实体以及更改发生的时间。为了从该功能中获益，你必须为你的实体类配备审计元数据，这些元数据可以使用注解或实现接口来定义。此外，审计必须通过注解配置或XML配置来启用，以注册所需的基础设施组件。关于配置样本，请参考特定store部分。

|      | 只跟踪创建和修改日期的应用程序不需要使其实体实现 [`AuditorAware`](https://springdoc.cn/spring-data-mongodb/#auditing.auditor-aware)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 16.1.1. 基于注解的审计元数据

我们提供 `@CreatedBy` 和 `@LastModifiedBy` 来捕获创建或修改实体的用户，以及 `@CreatedDate` 和 `@LastModifiedDate` 来捕获变化发生的时间。

Example 168. An audited entity

```java
class Customer {

  @CreatedBy
  private User user;

  @CreatedDate
  private Instant createdDate;

  // … further properties omitted
}
```

正如你所看到的，注解可以有选择地应用，这取决于你想捕获哪些信息。这些注解，表示捕捉变化的时间，可以用在JDK8 date和time类型、`long`、`Long` 以及传统的Java `Date` 和 `Calendar` 的属性上。

审计元数据不一定需要存在于根级实体中，但可以添加到一个嵌入式实体中（取决于实际使用的store），如下面的片段所示。

Example 169. 嵌入实体中的审计元数据

```java
class Customer {

  private AuditMetadata auditingMetadata;

  // … further properties omitted
}

class AuditMetadata {

  @CreatedBy
  private User user;

  @CreatedDate
  private Instant createdDate;

}
```

#### 16.1.2. 基于接口的审计元数据

如果你不想使用注解来定义审计元数据，你可以让你的domain类实现 `Auditable` 接口。它为所有的审计属性暴露了 `setter` 方法。

#### 16.1.3. `AuditorAware`

如果你使用 `@CreatedBy` 或 `@LastModifiedBy`，审计基础设施需要以某种方式知道当前的principal。为此，我们提供了一个 `AuditorAware<T>` SPI接口，你必须实现这个接口来告诉基础设施谁是与应用程序交互的当前用户或系统。泛型 `T` 定义了用 `@CreatedBy` 或 `@LastModifiedBy` 注解的属性必须是什么类型。

下面的例子显示了一个使用Spring Security的 `Authentication` 对象的接口实现。

Example 170. 基于 Spring Security 的 `AuditorAware` 的实现

```java
class SpringSecurityAuditorAware implements AuditorAware<User> {

  @Override
  public Optional<User> getCurrentAuditor() {

    return Optional.ofNullable(SecurityContextHolder.getContext())
            .map(SecurityContext::getAuthentication)
            .filter(Authentication::isAuthenticated)
            .map(Authentication::getPrincipal)
            .map(User.class::cast);
  }
}
```

该实现访问由Spring Security提供的 `Authentication` 对象，并查找你在 `UserDetailsService` 实现中创建的自定义 `UserDetails` 实例。我们在这里假设你是通过 `UserDetails` 实现来暴露domain用户的，但根据找到的 `Authentication`，你也可以从任何地方查到它。

#### 16.1.4. `ReactiveAuditorAware`

当使用响应式基础设施时，你可能想利用上下文（Context）信息来提供 `@CreatedBy` 或 `@LastModifiedBy` 信息。我们提供了一个 `ReactiveAuditorAware<T>` SPI接口，你必须实现这个接口来告诉基础设施谁是当前与应用程序交互的用户或系统。泛型 `T` 定义了用 `@CreatedBy` 或 `@LastModifiedBy` 注释的属性必须是什么类型。

下面的例子显示了一个接口的实现，它使用了Spring Security的 `Authentication` 对象。

Example 171. 基于 Spring Security 的 `ReactiveAuditorAware` 的实现

```java
class SpringSecurityAuditorAware implements ReactiveAuditorAware<User> {

  @Override
  public Mono<User> getCurrentAuditor() {

    return ReactiveSecurityContextHolder.getContext()
                .map(SecurityContext::getAuthentication)
                .filter(Authentication::isAuthenticated)
                .map(Authentication::getPrincipal)
                .map(User.class::cast);
  }
}
```

该实现访问由Spring Security提供的 `Authentication` 对象，并查找你在 `UserDetailsService` 实现中创建的自定义 `UserDetails` 实例。我们在这里假设你是通过 `UserDetails` 实现来暴露domain用户的，但根据找到的 `Authentication`，你也可以从任何地方查到它。

### 16.2. MongoDB的一般审计配置

自Spring Data MongoDB 1.4以来，可以通过用 `@EnableMongoAuditing` 注解对配置类进行注解来启用审计，如下例所示。

Java

XML

```java
@Configuration
@EnableMongoAuditing
class Config {

  @Bean
  public AuditorAware<AuditableUser> myAuditorProvider() {
      return new AuditorAwareImpl();
  }
}
```

如果你将 `AuditorAware` 类型的 Bean 暴露给 `ApplicationContext`，审计基础设施会自动拾取它，并使用它来确定要在 domain 类型上设置的当前用户。如果你在 `ApplicationContext` 中注册了多个实现，你可以通过明确设置 `@EnableMongoAuditing` 的 `auditorAwareRef` 属性来选择要使用的一个。

要启用审计，利用响应式编程模型，使用 `@EnableReactiveMongoAuditing` 注解。 如果你向 `ApplicationContext` 暴露了一个 `ReactiveAuditorAware` 类型的bean，审计基础设施会自动接收它，并使用它来确定要在 domain 类型上设置的当前用户。如果你在 `ApplicationContext` 中注册了多个实现，你可以通过明确设置 `@EnableReactiveMongoAuditing` 的 `auditorAwareRef` 属性来选择要使用的那个。

Example 172. 使用 JavaConfig 激活响应式审计

```java
@Configuration
@EnableReactiveMongoAuditing
class Config {

  @Bean
  public ReactiveAuditorAware<AuditableUser> myAuditorProvider() {
      return new AuditorAwareImpl();
  }
}
```

## 17. 映射（Mapping）

丰富的映射支持是由 `MappingMongoConverter` 提供的。`MappingMongoConverter` 有一个丰富的元数据模型，提供完整的功能集，将 domain 对象映射到MongoDB文档。映射元数据模型是通过在你的 domain 对象上使用注解来填充的。然而，该基础设施并不限于使用注解作为元数据信息的唯一来源。`MappingMongoConverter` 还可以让你在不提供任何额外元数据的情况下，通过遵循一组约定，将对象映射到文档。

本节介绍了 `MappingMongoConverter` 的功能，包括基本原理，如何使用约定将对象映射到文档，以及如何用基于注解的映射元数据覆盖这些约定。

### 17.1. 对象映射的基础知识

本节涵盖了Spring Data对象映射、对象创建、字段和属性访问、可变性和不可变性的基本原理。注意，本节只适用于不使用底层数据存储的对象映射的Spring Data模块（如JPA）。此外，请务必查阅特定于存储的对象映射部分，如索引、自定义列或字段名或类似内容。

Spring Data对象映射的核心职责是创建domain对象的实例，并将存储的本地数据结构映射到这些对象上。这意味着我们需要两个基本步骤。

1. 通过使用暴露的构造函数之一来创建实例。
2. Instance population to materialize all exposed properties.

#### 17.1.1. Object 创建

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

#### 17.1.2. 属性填充

一旦实体的实例被创建，Spring Data就会填充该类的所有剩余持久化属性。除非已经由实体的构造函数填充（即通过其构造函数参数列表设置），否则 identifier 属性将首先被填充，以允许解决循环对象引用。之后，所有尚未被构造函数填充的非瞬时（non-transient）属性都被设置在实体实例上。为此，我们使用以下算法。

1. 如果该属性是不可变的，但暴露了一个 `with…` 方法（见下文），我们使用 `with…` 方法来创建一个具有新属性值的新实体实例。
2. 如果定义了属性 access（即通过getter和setter访问），我们就调用setter方法。
3. 如果该属性是可变的，我们直接设置该字段。
4. 如果属性是不可变的，我们就使用持久化操作（见 [Object 创建](https://springdoc.cn/spring-data-mongodb/#mapping.object-creation)）所使用的构造函数来创建一个实例的副本。
5. 默认情况下，我们直接设置字段的值。

属性填充的内部细节

与我们在[对象构造中的优化](https://springdoc.cn/spring-data-mongodb/#mapping.object-creation.details)类似，我们也使用Spring Data 运行时生成的 accessor 类来与实体实例进行交互。

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

Example 173. A generated Property Accessor

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

Example 174. A sample entity

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

#### 17.1.3. 一般建议

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

#### 17.1.4. 对 Kotlin 的支持

Spring Data 适应了 Kotlin 的具体特性，允许对象的创建和变异（mutation）。

##### Kotlin object 创建

Kotlin类支持实例化，所有的类默认是不可变的，需要明确的属性声明来定义可变的属性。

Spring Data会自动尝试检测一个持久化实体的构造函数，以用于将该类型的对象具体化。 该解析算法的工作原理如下。

1. 如果有一个构造函数被注解为 `@PersistenceCreator`，它将被使用。
2. 如果类型是 [Kotlin data cass](https://springdoc.cn/spring-data-mongodb/#mapping.kotlin)，则使用 primary 构造函数。
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

### 17.2. 基于常规的映射

当没有提供额外的映射元数据时，`MappingMongoConverter` 有一些惯例用于将对象映射到文档。这些约定如下：

- 短的Java类名以如下方式映射到集合名。`com.bigbank.SavingsAccount` 这个类映射到 `savingsAccount` 集合的名称。
- 所有的嵌套对象都作为文档中的嵌套对象而不是作为DBRefs来存储。
- converter 使用与它注册的任何Spring Converter来覆盖对象属性到文档字段和值的默认映射。
- 一个对象的字段被用来转换为文档中的字段和从文档中转换。public `JavaBean` 属性不被使用。
- 如果你有一个单一的非零参数构造器，其构造器参数名称与文档的顶级字段名称相匹配，则使用该构造器。否则，将使用零参数的构造函数。如果有一个以上的非零参数构造函数，将抛出一个异常。

#### 17.2.1. 在映射层中如何处理 `_id` 字段

MongoDB要求你为所有文档提供一个 `_id` 字段。如果你没有提供一个，驱动程序将分配一个生成值的ObjectId。 "_id" 字段可以是任何类型，而不是数组，只要它是唯一的。驱动程序自然支持所有原始类型和日期。当使用 `MappingMongoConverter` 时，有一些规则规定了如何将Java类的属性映射到 `_id` 字段。

下面概述了什么字段将被映射到 `_id` 文档字段。

- 一个用 `@Id`（`org.springframework.data.annotation.Id`）注解的字段将被映射到 `_id` 字段。
- 一个没有注解但名为 `id` 的字段将被映射到 `_id` 字段。
- id 的默认字段名是 `_id`，可以通过 `@Field` 注解进行自定义。

| 字段定义                       | 在MongoDB中产生的Id字段名称 |
| :----------------------------- | :-------------------------- |
| `String` id                    | `_id`                       |
| `@Field` `String` id           | `_id`                       |
| `@Field("x")` `String` id      | `x`                         |
| `@Id` `String` x               | `_id`                       |
| `@Field("x")` `@Id` `String` x | `_id`                       |

下面概述了将对映射到 `_id` 文档字段的属性进行何种类型转换（如果有的话）。

- 如果一个名为 `id` 的字段在Java类中被声明为 `String` 或 `BigInteger`，它将被转换为 `ObjectId` 并尽可能地存储。`ObjectId` 作为一个字段类型也是有效的。如果你在你的应用程序中为 `id` 指定了一个值，那么转换为 `ObjectId` 的过程会被MongoDB驱动检测到。如果指定的 `id` 值不能被转换为 `ObjectId`，那么该值将被原样存储在文档的 `_id` 字段中。这也适用于该字段被注解为 `@Id` 的情况。
- 如果一个字段在Java类中被 `@MongoId` 注解，它将被转换为使用其实际类型进行存储。除非 `@MongoId` 声明了一个想要的字段类型，否则不会发生进一步的转换。如果没有为 `id` 字段提供值，一个新的 `ObjectId` 将被创建并转换为属性类型。
- 如果一个字段在Java类中被注解为 `@MongoId(FieldType….)`，它将尝试将该值转换为声明的 `FieldType`。如果没有为 `id` 字段提供任何值，将创建一个新的 `ObjectId`，并转换为声明的类型。
- 如果一个名为 `id` 的字段在Java类中没有被声明为 String、BigInteger 或 ObjectID，那么你应该在你的应用程序中为它赋值，这样它就可以 "按原样" 存储在文档的 `_id` 字段中。
- 如果Java类中没有名为 `id` 的字段，那么驱动程序将生成一个隐含的 `_id` 文件，但不会映射到Java类的一个属性或字段。

当查询和更新时，`MongoTemplate` 将使用 converter 来处理 `Query` 和 `Update` 对象的转换，这些对象与上述保存文档的规则相对应，因此在你的查询中使用的字段名和类型将能够与你的domain类中的内容相匹配。

### 17.3. 数据映射和类型转换

本节解释了如何将类型映射到 MongoDB 的表示方法，以及如何从 MongoDB 的表示方法中获取类型。Spring Data MongoDB支持所有可表示为BSON（MongoDB的内部文档格式）的类型。除了这些类型外，Spring Data MongoDB 还提供了一组内置的转换器来映射其他类型。你可以提供您自己的转换器来调整类型转换。请参阅 [[mapping-explicit-converters\]](https://springdoc.cn/spring-data-mongodb/#mapping-explicit-converters) 以了解更多细节。

下面提供了每个可用类型转换的样本。

| 类型                                                         | 类型转换                                                     | 示例                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `String`                                                     | native                                                       | `{"firstname" : "Dave"}`                                     |
| `double`, `Double`, `float`, `Float`                         | native                                                       | `{"weight" : 42.5}`                                          |
| `int`, `Integer`, `short`, `Short`                           | native 32-bit integer                                        | `{"height" : 42}`                                            |
| `long`, `Long`                                               | native 64-bit integer                                        | `{"height" : 42}`                                            |
| `Date`, `Timestamp`                                          | native                                                       | `{"date" : ISODate("2019-11-12T23:00:00.809Z")}`             |
| `byte[]`                                                     | native                                                       | `{"bin" : { "$binary" : "AQIDBA==", "$type" : "00" }}`       |
| `java.util.UUID` (Legacy UUID)                               | native                                                       | `{"uuid" : { "$binary" : "MEaf1CFQ6lSphaa3b9AtlA==", "$type" : "03" }}` |
| `Date`                                                       | native                                                       | `{"date" : ISODate("2019-11-12T23:00:00.809Z")}`             |
| `ObjectId`                                                   | native                                                       | `{"_id" : ObjectId("5707a2690364aba3136ab870")}`             |
| Array, `List`, `BasicDBList`                                 | native                                                       | `{"cookies" : [ … ]}`                                        |
| `boolean`, `Boolean`                                         | native                                                       | `{"active" : true}`                                          |
| `null`                                                       | native                                                       | `{"value" : null}`                                           |
| `Document`                                                   | native                                                       | `{"value" : { … }}`                                          |
| `Decimal128`                                                 | native                                                       | `{"value" : NumberDecimal(…)}`                               |
| `AtomicInteger` calling `get()` before the actual conversion | converter 32-bit integer                                     | `{"value" : "741" }`                                         |
| `AtomicLong` calling `get()` before the actual conversion    | converter 64-bit integer                                     | `{"value" : "741" }`                                         |
| `BigInteger`                                                 | converter `String`                                           | `{"value" : "741" }`                                         |
| `BigDecimal`                                                 | converter `String`                                           | `{"value" : "741.99" }`                                      |
| `URL`                                                        | converter                                                    | `{"website" : "https://spring.io/projects/spring-data-mongodb/" }` |
| `Locale`                                                     | converter                                                    | `{"locale : "en_US" }`                                       |
| `char`, `Character`                                          | converter                                                    | `{"char" : "a" }`                                            |
| `NamedMongoScript`                                           | converter `Code`                                             | `{"_id" : "script name", value: (some javascript code)`}     |
| `java.util.Currency`                                         | converter                                                    | `{"currencyCode" : "EUR"}`                                   |
| `Instant` (Java 8)                                           | native                                                       | `{"date" : ISODate("2019-11-12T23:00:00.809Z")}`             |
| `Instant` (Joda, JSR310-BackPort)                            | converter                                                    | `{"date" : ISODate("2019-11-12T23:00:00.809Z")}`             |
| `LocalDate` (Joda, Java 8, JSR310-BackPort)                  | converter / native (Java8)[[2](https://springdoc.cn/spring-data-mongodb/#_footnotedef_2)] | `{"date" : ISODate("2019-11-12T00:00:00.000Z")}`             |
| `LocalDateTime`, `LocalTime` (Joda, Java 8, JSR310-BackPort) | converter / native (Java8)[[3](https://springdoc.cn/spring-data-mongodb/#_footnotedef_3)] | `{"date" : ISODate("2019-11-12T23:00:00.809Z")}`             |
| `DateTime` (Joda)                                            | converter                                                    | `{"date" : ISODate("2019-11-12T23:00:00.809Z")}`             |
| `ZoneId` (Java 8, JSR310-BackPort)                           | converter                                                    | `{"zoneId" : "ECT - Europe/Paris"}`                          |
| `Box`                                                        | converter                                                    | `{"box" : { "first" : { "x" : 1.0 , "y" : 2.0} , "second" : { "x" : 3.0 , "y" : 4.0}}` |
| `Polygon`                                                    | converter                                                    | `{"polygon" : { "points" : [ { "x" : 1.0 , "y" : 2.0} , { "x" : 3.0 , "y" : 4.0} , { "x" : 4.0 , "y" : 5.0}]}}` |
| `Circle`                                                     | converter                                                    | `{"circle" : { "center" : { "x" : 1.0 , "y" : 2.0} , "radius" : 3.0 , "metric" : "NEUTRAL"}}` |
| `Point`                                                      | converter                                                    | `{"point" : { "x" : 1.0 , "y" : 2.0}}`                       |
| `GeoJsonPoint`                                               | converter                                                    | `{"point" : { "type" : "Point" , "coordinates" : [3.0 , 4.0] }}` |
| `GeoJsonMultiPoint`                                          | converter                                                    | `{"geoJsonLineString" : {"type":"MultiPoint", "coordinates": [ [ 0 , 0 ], [ 0 , 1 ], [ 1 , 1 ] ] }}` |
| `Sphere`                                                     | converter                                                    | `{"sphere" : { "center" : { "x" : 1.0 , "y" : 2.0} , "radius" : 3.0 , "metric" : "NEUTRAL"}}` |
| `GeoJsonPolygon`                                             | converter                                                    | `{"polygon" : { "type" : "Polygon", "coordinates" : [[ [ 0 , 0 ], [ 3 , 6 ], [ 6 , 1 ], [ 0 , 0  ] ]] }}` |
| `GeoJsonMultiPolygon`                                        | converter                                                    | `{"geoJsonMultiPolygon" : { "type" : "MultiPolygon", "coordinates" : [     [ [ [ -73.958 , 40.8003 ] , [ -73.9498 , 40.7968 ] ] ],     [ [ [ -73.973 , 40.7648 ] , [ -73.9588 , 40.8003 ] ] ]  ] }}` |
| `GeoJsonLineString`                                          | converter                                                    | `{ "geoJsonLineString" : { "type" : "LineString", "coordinates" : [ [ 40 , 5 ], [ 41 , 6 ] ] }}` |
| `GeoJsonMultiLineString`                                     | converter                                                    | `{"geoJsonLineString" : { "type" : "MultiLineString", coordinates: [     [ [ -73.97162 , 40.78205 ], [ -73.96374 , 40.77715 ] ],     [ [ -73.97880 , 40.77247 ], [ -73.97036 , 40.76811 ] ]  ] }}` |

### 17.4. 映射配置

除非明确配置，否则在你创建 `MongoTemplate` 时，默认会创建 `MappingMongoConverter` 的实例。你可以创建你自己的 `MappingMongoConverter` 的实例。这样做可以让你决定在 classpath 中找到你的 domain 类，以便Spring Data MongoDB能够提取元数据并构建索引。此外，通过创建自己的实例，你可以注册Spring converter，以便将特定的类映射到数据库中，或从数据库中提取。

你可以通过使用基于Java或基于XML的元数据来配置 `MappingMongoConverter` 以及 `com.mongodb.client.MongoClient` 和 `MongoTemplate`。下面的例子显示了配置情况。

Java

XML

```java
@Configuration
public class MongoConfig extends AbstractMongoClientConfiguration {

  @Override
  public String getDatabaseName() {
    return "database";
  }

  // the following are optional

  @Override
  public String getMappingBasePackage() { 
    return "com.bigbank.domain";
  }

  @Override
  void configureConverters(MongoConverterConfigurationAdapter adapter) { 

  	adapter.registerConverter(new org.springframework.data.mongodb.test.PersonReadConverter());
  	adapter.registerConverter(new org.springframework.data.mongodb.test.PersonWriteConverter());
  }

  @Bean
  public LoggingEventListener<MongoMappingEvent> mappingEventsListener() {
    return new LoggingEventListener<MongoMappingEvent>();
  }
}
```

`AbstractMongoClientConfiguration` 要求你实现定义 `com.mongodb.client.MongoClient` 的方法，并提供一个数据库名称。`AbstractMongoClientConfiguration` 也有一个名为 `getMappingBasePackage(…)` 的方法，你可以覆盖该方法，以告诉 converter 在哪里扫描带有 `@Document` 注解的类。

你可以通过覆写 `customConversionsConfiguration` 方法向 converter 添加额外的 converter。MongoDB的本地JSR-310支持可以通过 `MongoConverterConfigurationAdapter.useNativeDriverJavaTimeCodecs()` 启用。前面的例子中还显示了一个 `LoggingEventListener`，它记录了发布到Spring的 `ApplicationContextEvent` 基础架构上的 `MongoMappingEvent` 实例。

|      | `AbstractMongoClientConfiguration` 创建了一个 `MongoTemplate` 实例，并以 `mongoTemplate` 的名字在容器中注册了它。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

`base-package` 属性告诉它在哪里扫描带有 `@org.springframework.data.mongodb.core.mapping.Document` 注解的类。

### 17.5. 基于元数据的映射

为了充分利用Spring Data MongoDB支持中的对象映射功能，你应该用 `@Document` 注解来注解你的映射对象。尽管映射框架没有必要拥有这个注解（即使没有任何注解，你的POJO也能正确映射），但它能让classpath扫描器找到并预处理你的领域对象，以提取必要的元数据。如果你不使用这个注解，你的应用程序在第一次存储 domain 对象时就会受到轻微的性能影响，因为映射框架需要建立其内部元数据模型，以便它知道你的 domain 对象的属性以及如何持久化它们。下面的例子显示了一个 domain 对象。

Example 175. domain 对象实例

```java
package com.mycompany.domain;

@Document
public class Person {

  @Id
  private ObjectId id;

  @Indexed
  private Integer ssn;

  private String firstName;

  @Indexed
  private String lastName;
}
```

|      | `@Id` 注解告诉映射器你想使用哪个属性作为 MongoDB `_id` 属性，而 `@Indexed` 注解告诉映射框架在你文档的那个属性上调用 `createIndex(…)` ，使搜索更快。自动索引创建只针对用 `@Document` 注解的类型。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 自动索引创建默认是禁用的（**disabled** ），需要通过配置启用（见 [创建索引](https://springdoc.cn/spring-data-mongodb/#mapping.index-creation)）。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 17.5.1. 创建索引

Spring Data MongoDB可以为用 `@Document` 注解的实体类型自动创建索引。自3.0版本以来，必须明确启用索引创建，以防止对集合生命周期和性能产生不希望看到的影响。在应用程序启动时以及在应用程序运行时首次访问实体类型时，会自动为初始实体集创建索引。

我们通常推荐显式索引的创建，以实现基于应用的索引控制，因为Spring Data不能自动为应用运行时重新创建的集合创建索引。

如果你想利用 `@Indexed` 注解，如 `@GeoSpatialIndexed`、`@TextIndexed`、 `@CompoundIndex` 和 `@WildcardIndexed`，`IndexResolver` 为编程式索引定义的创建提供了一个抽象。你可以使用带有 `IndexOperations` 的索引定义来创建索引。创建索引的一个好的时间点是在应用程序启动时，特别是在 application context 被刷新后，通过观察 `ContextRefreshedEvent` 触发的。这个事件保证了 context 是完全初始化的。请注意，此时其他组件，尤其是Bean Factory 可能会访问MongoDB数据库。

|      | 除非用 `@WildcardIndexed` 注解，否则类似 `Map` 的属性会被 `IndexResolver` 跳过，因为 *map key* 必须是索引定义的一部分。因为 map 的目的是使用动态的key和value，所以key不能从静态的映射元数据中解决。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

Example 176. 为单一 domain 类型创建程序化索引

```java
class MyListener {

  @EventListener(ContextRefreshedEvent.class)
  public void initIndicesAfterStartup() {

    MappingContext<? extends MongoPersistentEntity<?>, MongoPersistentProperty> mappingContext = mongoTemplate
                .getConverter().getMappingContext();

    IndexResolver resolver = new MongoPersistentEntityIndexResolver(mappingContext);

    IndexOperations indexOps = mongoTemplate.indexOps(DomainType.class);
    resolver.resolveIndexFor(DomainType.class).forEach(indexOps::ensureIndex);
  }
}
```

Example 177. 为所有初始实体创建程序化索引

```java
class MyListener{

  @EventListener(ContextRefreshedEvent.class)
  public void initIndicesAfterStartup() {

    MappingContext<? extends MongoPersistentEntity<?>, MongoPersistentProperty> mappingContext = mongoTemplate
        .getConverter().getMappingContext();

    // consider only entities that are annotated with @Document
    mappingContext.getPersistentEntities()
                            .stream()
                            .filter(it -> it.isAnnotationPresent(Document.class))
                            .forEach(it -> {

    IndexOperations indexOps = mongoTemplate.indexOps(it.getType());
    resolver.resolveIndexFor(it.getType()).forEach(indexOps::ensureIndex);
    });
  }
}
```

另外，如果你想在任何组件能够从你的应用程序访问你的数据库之前确保索引和集合的存在，为 `MongoTemplate` 声明一个 `@Bean` 方法，并在返回 `MongoTemplate` 对象之前包含上面的代码。

|      | 要打开自动索引创建功能，请在你的配置中覆写 `autoIndexCreation()`。`@Configuration public class Config extends AbstractMongoClientConfiguration {   @Override  public boolean autoIndexCreation() {    return true;  } // ... } ` |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 从3.0版本开始，自动索引创建默认为关闭。 |
| ---- | --------------------------------------- |
|      |                                         |

#### 17.5.2. 映射注释概述

`MappingMongoConverter` 可以使用元数据来驱动对象到文档的映射。以下注解是可用的。

- `@Id`: 应用在字段，以标记用于ID的字段。
- `@MongoId`: 在字段级应用，以标记用于ID的字段。接受一个可选的 `FieldType` 来定制ID转换。
- `@Document`: 应用在类的层面上，表示这个类是映射到数据库的候选对象。你可以指定存储数据的集合的名称。
- `@DBRef`: 应用在字段上，表明它将使用 `com.mongodb.DBRef` 来存储。
- `@DocumentReference`: 应用在字段上，表示它将作为一个指向另一个文档的指针被存储。这可以是一个单一的值（默认为 *id*），也可以是一个通过 converter 提供的 `Document`。
- `@Indexed`: 应用于字段级，描述如何对字段进行索引。
- `@CompoundIndex` (可重复): 在类型层面上应用于声明复合索引。
- `@GeoSpatialIndexed`: 应用于 domain 层面，描述如何对 domain 进行地理索引（geoindex ）。
- `@TextIndexed`: 在字段一级应用，以标记要包括在文本索引中的字段。
- `@HashIndexed`: 在字段级应用于哈希索引中的使用，以在分片集群中对数据进行分割。
- `@Language`: 应用于字段级，用于设置文本索引的语言覆盖属性。
- `@Transient`: 默认情况下，所有字段都被映射到文档中。这个注解排除了它所应用的字段被存储在数据库中。瞬时（Transient）属性不能在持久化构造函数中使用，因为 converter 不能为构造函数参数具体化一个值。
- `@PersistenceConstructor`: 标记一个给定的构造函数—即使是一个受包保护的构造函数—在从数据库实例化对象时使用。构造函数参数通过名称映射到检索的文档中的关键值。
- `@Value`: 这个注解是Spring框架的一部分。在映射框架内，它可以应用于构造器参数。这让你可以使用Spring表达式语言语句来转换在数据库中检索到的key值，然后再用于构造domain对象。为了引用一个给定文档的属性，我们必须使用这样的表达式。`@Value("#root.myProperty")` 其中root指的是给定文档的 `root`。
- `@Field`: 应用于字段级别，它允许描述字段的名称和类型，因为它将在MongoDB BSON文档中表示出来，因此允许名称和类型与类的字段名以及属性类型不同。
- `@Version`: 应用于字段级，用于乐观锁，并在保存操作中检查是否有修改。初始值为 `0`（原始类型为 `1`），在每次更新时都会自动增加。

映射元数据基础设施定义在一个独立的 spring-data-commons 项目中，该项目与技术无关。特定的子类在MongoDB支持中使用，以支持基于注解的元数据。如果有需求的话，其他的策略也是可以实施的。

下面是一个更复杂的映射的例子。

```java
@Document
@CompoundIndex(name = "age_idx", def = "{'lastName': 1, 'age': -1}")
public class Person<T extends Address> {

  @Id
  private String id;

  @Indexed(unique = true)
  private Integer ssn;

  @Field("fName")
  private String firstName;

  @Indexed
  private String lastName;

  private Integer age;

  @Transient
  private Integer accountTotal;

  @DBRef
  private List<Account> accounts;

  private T address;

  public Person(Integer ssn) {
    this.ssn = ssn;
  }

  @PersistenceConstructor
  public Person(Integer ssn, String firstName, String lastName, Integer age, T address) {
    this.ssn = ssn;
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
    this.address = address;
  }

  public String getId() {
    return id;
  }

  // no setter for Id.  (getter is only exposed for some unit testing)

  public Integer getSsn() {
    return ssn;
  }

// other getters/setters omitted
}
```

|      | `@Field(targetType=…)` 在映射基础设施推断出的本地MongoDB类型与预期类型不一致时可以派上用场。比如 `BigDecimal`，它被表示为 `String` 而不是 `Decimal128`，只是因为MongoDB服务器的早期版本没有对它的支持。`public class Balance {   @Field(targetType = DECIMAL128)  private BigDecimal value;   // ... } `你甚至可以考虑你自己的、自定义的注解。`@Target(ElementType.FIELD) @Retention(RetentionPolicy.RUNTIME) @Field(targetType = FieldType.DECIMAL128) public @interface Decimal128 { } // ... public class Balance {   @Decimal128  private BigDecimal value;   // ... } ` |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 17.5.3. 自定义 Object 构造

映射子系统允许通过用 `@PersistenceConstructor` 注解来定制对象的构造。用于构造器参数的值以下列方式解析。

- 如果一个参数被 `@Value` 注解，给定的表达式被评估，其结果被用作参数值。
- 如果Java类有一个属性，其名称与输入文件的给定字段相匹配，那么它的属性信息将被用来选择适当的构造函数参数来传递输入字段的值。这只有在参数名称信息存在于java `.class` 文件中时才起作用，这可以通过编译带有调试信息的源代码或使用Java 8中javac新的 `-parameters` 命令行开关来实现。
- 否则，将抛出一个 `MappingException`，表明所给的构造函数参数不能被绑定。

```java
class OrderItem {

  private @Id String id;
  private int quantity;
  private double unitPrice;

  OrderItem(String id, @Value("#root.qty ?: 0") int quantity, double unitPrice) {
    this.id = id;
    this.quantity = quantity;
    this.unitPrice = unitPrice;
  }

  // getters/setters ommitted
}

Document input = new Document("id", "4711");
input.put("unitPrice", 2.5);
input.put("qty",5);
OrderItem item = converter.read(OrderItem.class, input);
```

|      | 如果给定的属性路径无法解析，`quantity` 参数的 `@Value` 注解中的SpEL表达式就会回落到值 `0`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

在 [MappingMongoConverterUnitTests](https://github.com/spring-projects/spring-data-mongodb/blob/master/spring-data-mongodb/src/test/java/org/springframework/data/mongodb/core/convert/MappingMongoConverterUnitTests.java) 测试套件中可以找到使用 `@PersistenceConstructor` 注解的其他例子。

#### 17.5.4. 复合索引

复合索引也被支持。它们是在类的层面上定义的，而不是在单个属性上。

|      | 复合索引对于提高涉及多个字段 criteria 的查询性能非常重要 |
| ---- | -------------------------------------------------------- |
|      |                                                          |

下面是一个例子，它创建了一个升序的 `lastName` 和降序的 `age` 的复合索引。

Example 178. 复合索引使用实例

```java
package com.mycompany.domain;

@Document
@CompoundIndex(name = "age_idx", def = "{'lastName': 1, 'age': -1}")
public class Person {

  @Id
  private ObjectId id;
  private Integer age;
  private String firstName;
  private String lastName;

}
```

|      | `@CompoundIndex` 是可重复的，使用 `@CompoundIndexes` 作为其容器。`@Document @CompoundIndex(name = "cmp-idx-one", def = "{'firstname': 1, 'lastname': -1}") @CompoundIndex(name = "cmp-idx-two", def = "{'address.city': -1, 'address.street': 1}") public class Person {   String firstname;  String lastname;   Address address;   // ... } ` |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 17.5.5. 哈希索引

Hash索引允许在 sharded cluster 内进行基于hash的分片。使用hash字段值来分片的结果是一个更随机的分布。详情请参考 [MongoDB文档](https://docs.mongodb.com/manual/core/index-hashed/)。

下面是一个为 `_id` 创建Hash索引的例子。

Example 179. Hash索引使用实例

```java
@Document
public class DomainType {

  @HashIndexed @Id String id;

  // ...
}
```

Hash索引可以像下面所示的那样在其他索引定义的旁边创建，在这种情况下，两个索引都被创建。

Example 180. Hash索引与简单索引一起使用的例子

```java
@Document
public class DomainType {

  @Indexed
  @HashIndexed
  String value;

  // ...
}
```

如果上面的例子过于冗长，复合注解允许减少需要在一个属性上声明的注解的数量。

Example 181. 复合hash索引使用实例

```java
@Document
public class DomainType {

  @IndexAndHash(name = "idx...")                            
  String value;

  // ...
}

@Indexed
@HashIndexed
@Retention(RetentionPolicy.RUNTIME)
public @interface IndexAndHash {

  @AliasFor(annotation = Indexed.class, attribute = "name") 
  String name() default "";
}
```

|      | 有可能为元注解的某些属性注册一个别名。 |
| ---- | -------------------------------------- |
|      |                                        |

|      | 尽管通过注解创建索引在很多情况下都很方便，但考虑通过 `IndexOperations` 手动设置索引来接管更多的控制权。`mongoOperations.indexOpsFor(Jedi.class)  .ensureIndex(HashedIndex.hashed("useTheForce")); ` |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 17.5.6. 通配符索引

通配符索引（`WildcardIndex`）是一个索引，可以用来包括所有字段或基于给定（通配符）模式（pattern）的特定字段。详情请参考 [MongoDB文档](https://docs.mongodb.com/manual/core/index-wildcard/)。

该索引可以通过 `IndexOperations` 使用 `WildcardIndex` 进行编程设置。

Example 182. 程序化通配符索引设置

```java
mongoOperations
    .indexOps(User.class)
    .ensureIndex(new WildcardIndex("userMetadata"));
db.user.createIndex({ "userMetadata.$**" : 1 }, {})
```

`@WildcardIndex` 注解允许一个声明性的索引设置，可以与文档类型或属性一起使用。

如果放在一个root级 domain 实体的类上（一个用 `@Document` 注释的类），索引解析器将为它创建一个通配符索引。

Example 183. domain类的通配符索引

```java
@Document
@WildcardIndexed
public class Product {
	// …
}
db.product.createIndex({ "$**" : 1 },{})
```

通配符投影（`wildcardProjection`）可以用来指定索引中的包含/排除。

Example 184. 使用 `wildcardProjection` 的通配符索引

```java
@Document
@WildcardIndexed(wildcardProjection = "{ 'userMetadata.age' : 0 }")
public class User {
    private @Id String id;
    private UserMetadata userMetadata;
}
db.user.createIndex(
  { "$**" : 1 },
  { "wildcardProjection" :
    { "userMetadata.age" : 0 }
  }
)
```

通配符索引也可以通过直接向字段添加注解来表达。请注意，`wildcardProjection` 在嵌套路径上是不允许的，比如属性。在索引创建过程中，用 `@WildcardIndexed` 注解的类上的投影被省略。

Example 185. 属性上的通配符索引

```java
@Document
public class User {
    private @Id String id;

    @WildcardIndexed
    private UserMetadata userMetadata;
}
db.user.createIndex({ "userMetadata.$**" : 1 }, {})
```

#### 17.5.7. 文本索引

|      | 对于MongoDB v.2.4来说，文本索引功能默认是禁用的。 |
| ---- | ------------------------------------------------- |
|      |                                                   |

创建一个文本索引可以将几个字段累积成一个可搜索的全文索引。每个集合只能有一个文本索引，所以所有标有 `@TextIndexed` 的字段都被合并到这个索引中。属性可以被加权以影响排名结果的文档分数。文本索引的默认语言是英语。要改变默认语言，将 `language` 属性设置为你想要的任何语言（例如，`@Document(language="spanish")`）。使用一个叫做 `language` 或 `@Language` 的属性可以让你在每个文档的基础上定义一个语言覆盖。下面的例子显示了如何创建一个文本索引并将语言设置为西班牙语（Spanish）。

Example 186. 文本索引使用实例

```java
@Document(language = "spanish")
class SomeEntity {

    @TextIndexed String foo;

    @Language String lang;

    Nested nested;
}

class Nested {

    @TextIndexed(weight=5) String bar;
    String roo;
}
```

#### 17.5.8. 使用 DBRef

映射框架不一定要把子对象嵌入到文档中去存储。你也可以单独存储它们，并使用一个 `DBRef` 来引用该文档。当对象从MongoDB加载时，这些引用会被急切地解析，这样你得到的映射对象看起来就像它被嵌入到你的顶级文档中存储一样。

下面的例子使用一个 DBRef 来引用一个特定的文档，该文档独立于它被引用的对象而存在（为了简洁起见，两个类都是在线显示）。

```java
@Document
public class Account {

  @Id
  private ObjectId id;
  private Float total;
}

@Document
public class Person {

  @Id
  private ObjectId id;
  @Indexed
  private Integer ssn;
  @DBRef
  private List<Account> accounts;
}
```

你不需要使用 `@OneToMany` 或类似的机制，因为对象的列表告诉映射框架你想要一个一对多的关系。当对象被存储在MongoDB中时，有一个DBRefs的列表，而不是 `Account` 对象本身。当涉及到加载 `DBRef` 的集合时，建议将集合类型中持有的引用限制在一个特定的MongoDB集合。这允许批量加载所有的引用，而指向不同MongoDB集合的引用则需要一个一个地解析。

|      | 映射框架不处理级联保存。如果你改变一个被 `Person` 对象引用的 `Account` 对象，你必须单独保存 `Account` 对象。在 `Person` 对象上调用保存并不会自动保存 `Account` 属性中的 `Account` 对象。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

`DBRef` 也可以被延迟地解析。在这种情况下，实际的 `Object` 或引用 `Collection` 在第一次访问该属性时被解析。使用 `@DBRef` 的 `lazy` 属性来指定这一点。同样被定义为懒加载 `DBRef` 并被用作构造函数参数的必要属性也会用懒加载代理进行装饰，以确保尽可能地减少对数据库和网络的压力。

|      | 懒加载 `DBRef` 可能很难调试。确保工具不会通过调用 `toString()` 或一些内联 debug 渲染调用属性 getter 而意外地触发代理解析。请考虑为 `org.springframework.data.mongodb.core.convert.DefaultDbRefResolver` 启用 *trace* logging，以深入了解 `DBRef` 的解析情况。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 懒加载可能需要类代理，而类代理又可能需要访问jdk内部，由于 [JEP 396](https://openjdk.java.net/jeps/396) 的规定，从Java 16+开始，这些内部是不开放的。默认情况下强封装JDK内部。对于这些情况，请考虑退回到一个接口类型（例如，从 `ArrayList` 切换到 `List`）或提供必要的 `--add-opens` 参数。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 17.5.9. 使用文档引用

使用 `@DocumentReference` 提供了一种在MongoDB中引用实体的灵活方式。虽然目标与使用 [DBRef](https://springdoc.cn/spring-data-mongodb/#mapping-usage-references) 时相同，但存储的表示方法却不同。`DBRef` 解析到一个具有固定结构的文档，如 [MongoDB参考文档](https://docs.mongodb.com/manual/reference/database-references/)中所述。
文档引用，不遵循特定的格式。它们实际上可以是任何东西，一个单一的值，整个文档，基本上所有可以存储在MongoDB中的东西。默认情况下，映射层将使用被引用实体的 *id* 值进行存储和检索，就像下面的例子。

```java
@Document
class Account {

  @Id
  String id;
  Float total;
}

@Document
class Person {

  @Id
  String id;

  @DocumentReference                                   
  List<Account> accounts;
}
Account account = …

tempate.insert(account);                               

template.update(Person.class)
  .matching(where("id").is(…))
  .apply(new Update().push("accounts").value(account)) 
  .first();
{
  "_id" : …,
  "accounts" : [ "6509b9e" … ]                        
}
```

|      | 标记要引用的 `Account` 值的集合。                          |
| ---- | ---------------------------------------------------------- |
|      | 映射框架不处理级联保存，所以要确保单独持久化被引用的实体。 |
|      | 添加对现有实体的引用。                                     |
|      | 被引用的 `Account` 实体被表示为其 `_id` 值的数组。         |

上面的例子使用基于 `_id` 的 fetch query（`{ '_id' : ?#{#target} }`）进行数据检索，并急切地解析链接实体。可以使用 `@DocumentReference` 的属性来改变解析默认值（如下所列）。

| 属性         | 说明                                                         | 默认值                                                       |
| :----------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `db`         | 用于集合查询的目标数据库名称。                               | `MongoDatabaseFactory.getMongoDatabase()`                    |
| `collection` | 目标集合的名称。                                             | 被注解的属性的domain类型，如果是类似于 `Collection` 或 `Map` 的属性，则分别是值类型，集合名称。 |
| `lookup`     | 单个文档查找查询通过SpEL表达式评估占位符，使用 `#target` 作为给定源值的标记。类似于 `Collection` 或 `Map` 的属性通过 `$or` 运算符将单个查找结合起来。 | 一个基于 `_id` 字段的查询（`{ '_id' : ?#{#target} }`），使用加载的 source value。 |
| `sort`       | 用于在服务器端对结果文档进行排序。                           | 默认为 None。类似于 `Collection` 的属性的结果顺序会根据所使用的 lookup query 在尽力的基础上进行恢复。 |
| `lazy`       | 如果设置为 `true`，则在第一次访问该属性时延迟解析。          | 默认情况下，急切地解析属性。                                 |

|      | 懒加载可能需要类代理，而类代理又可能需要访问jdk内部（internal），由于 [JEP 396](https://openjdk.java.net/jeps/396) 的规定，从Java 16+开始，这些内部是不开放的。默认情况下强封装JDK内部。对于这些情况，请考虑退回到一个接口类型（例如，从 `ArrayList` 切换到 `List`）或提供必要的 `--add-opens` 参数。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

`@DocumentReference(lookup)` 允许定义与 `_id` 字段不同的过滤查询，因此提供了一种灵活的方式来定义实体之间的引用，正如下面的例子所展示的那样，book 的 `Publisher` 被其 `acronym` 而不是内部 `id` 所引用。

```java
@Document
class Book {

  @Id
  ObjectId id;
  String title;
  List<String> author;

  @Field("publisher_ac")
  @DocumentReference(lookup = "{ 'acronym' : ?#{#target} }") 
  Publisher publisher;
}

@Document
class Publisher {

  @Id
  ObjectId id;
  String acronym;                                            
  String name;

  @DocumentReference(lazy = true)                            
  List<Book> books;

}
```

`Book` 文档

```json
{
  "_id" : 9a48e32,
  "title" : "The Warded Man",
  "author" : ["Peter V. Brett"],
  "publisher_ac" : "DR"
}
```

`Publisher` 文档

```json
{
  "_id" : 1a23e45,
  "acronym" : "DR",
  "name" : "Del Rey",
  …
}
```

|      | 使用 `acronym` 字段来查询Publisher集合中的实体。 |
| ---- | ------------------------------------------------ |
|      | 懒加载回对 `Book` 集合的引用。                   |

上面的片段显示了使用自定义引用对象时的读取情况。写作需要一些额外的设置，因为映射信息并不表达 `#target` 来自哪里。映射层需要在目标文档和 `DocumentPointer` 之间注册一个 `Converter`，就像下面这个。

```java
@WritingConverter
class PublisherReferenceConverter implements Converter<Publisher, DocumentPointer<String>> {

	@Override
	public DocumentPointer<String> convert(Publisher source) {
		return () -> source.getAcronym();
	}
}
```

如果没有提供 `DocumentPointer` converter，目标参考文档可以根据给定的查找查询进行计算。在这种情况下，关联的目标属性被评估，如下面的例子所示。

```java
@Document
class Book {

  @Id
  ObjectId id;
  String title;
  List<String> author;

  @DocumentReference(lookup = "{ 'acronym' : ?#{acc} }")  
  Publisher publisher;
}

@Document
class Publisher {

  @Id
  ObjectId id;
  String acronym;                                        
  String name;

  // ...
}
{
  "_id" : 9a48e32,
  "title" : "The Warded Man",
  "author" : ["Peter V. Brett"],
  "publisher" : {
    "acc" : "DOC"
  }
}
```

|      | 使用 `acronym` 字段来查询 `Publisher` 集合中的实体。        |
| ---- | ----------------------------------------------------------- |
|      | lookup query 的字段值占位符（如 `acc`）被用来形成引用文档。 |

也可以使用 `@ReadonlyProperty` 和 `@DocumentReference` 的组合来建立关系式的一对多引用。这种方法允许链接类型（link type）不在拥有的文档中存储链接值，而是在引用的文档中存储，如下面的例子所示。

```java
@Document
class Book {

  @Id
  ObjectId id;
  String title;
  List<String> author;

  ObjectId publisherId;                                        
}

@Document
class Publisher {

  @Id
  ObjectId id;
  String acronym;
  String name;

  @ReadOnlyProperty                                            
  @DocumentReference(lookup="{'publisherId':?#{#self._id} }")  
  List<Book> books;
}
```

`Book` 文档

```json
{
  "_id" : 9a48e32,
  "title" : "The Warded Man",
  "author" : ["Peter V. Brett"],
  "publisherId" : 8cfb002
}
```

`Publisher` 文档

```json
{
  "_id" : 8cfb002,
  "acronym" : "DR",
  "name" : "Del Rey"
}
```

|      | 通过在 `Book` 文档中存储 `Publisher.id`，设置从 `Book`（引用）到 `Publisher`（所有者）的链接。 |
| ---- | ------------------------------------------------------------ |
|      | 将持有引用的属性标记为只读。这可以防止在 `Publisher` 文档中存储对个别 `Book` 的引用。 |
|      | 使用 `#self` 变量来访问 `Publisher` 文档中的值，并在此检索具有匹配 `publisherId` 的 `Books`。 |

有了以上所有的东西，就有可能对实体之间的各种关联进行建模。请看下面这个不完全的样本列表，以获得对可能的东西的感觉。

Example 187. 使用 *id* 字段的简单文档引用

```java
class Entity {
  @DocumentReference
  ReferencedObject ref;
}
// entity
{
  "_id" : "8cfb002",
  "ref" : "9a48e32" 
}

// referenced object
{
  "_id" : "9a48e32" 
}
```

|      | MongoDB的简单类型可以直接使用，无需进一步配置。 |
| ---- | ----------------------------------------------- |
|      |                                                 |

Example 188. 使用 *id* 字段的简单文档参考，带有明确的 lookup query。

```java
class Entity {
  @DocumentReference(lookup = "{ '_id' : '?#{#target}' }") 
  ReferencedObject ref;
}
// entity
{
  "_id" : "8cfb002",
  "ref" : "9a48e32"                                        
}

// referenced object
{
  "_id" : "9a48e32"
}
```

|      | *target* 定义了引用值本身。 |
| ---- | --------------------------- |
|      |                             |

Example 189. 文档引用提取查询的 `refKey` 字段

```java
class Entity {
  @DocumentReference(lookup = "{ '_id' : '?#{refKey}' }")   
  private ReferencedObject ref;
}
@WritingConverter
class ToDocumentPointerConverter implements Converter<ReferencedObject, DocumentPointer<Document>> {
	public DocumentPointer<Document> convert(ReferencedObject source) {
		return () -> new Document("refKey", source.id);    
	}
}
// entity
{
  "_id" : "8cfb002",
  "ref" : {
    "refKey" : "9a48e32"                                   
  }
}

// referenced object
{
  "_id" : "9a48e32"
}
```

|      | 用于获取引用值的 key 必须是写入时使用的 key 。 |
| ---- | ---------------------------------------------- |
|      | `refKey` 是 `target.refKey` 的缩写。           |

Example 190. 有多个值的文件引用，形成 lookup query

```java
class Entity {
  @DocumentReference(lookup = "{ 'firstname' : '?#{fn}', 'lastname' : '?#{ln}' }")  
  ReferencedObject ref;
}
// entity
{
  "_id" : "8cfb002",
  "ref" : {
    "fn" : "Josh",           
    "ln" : "Long"            
  }
}

// referenced object
{
  "_id" : "9a48e32",
  "firstname" : "Josh",      
  "lastname" : "Long",       
}
```

|      | 根据 lookup query，从链接文档中读取/写入key `fn` & `ln`。 |
| ---- | --------------------------------------------------------- |
|      | 使用非 *id* 字段来查找目标文档。                          |

Example 191. 从目标集合中读取文档引用

```java
class Entity {
  @DocumentReference(lookup = "{ '_id' : '?#{id}' }", collection = "?#{collection}") 
  private ReferencedObject ref;
}
@WritingConverter
class ToDocumentPointerConverter implements Converter<ReferencedObject, DocumentPointer<Document>> {
	public DocumentPointer<Document> convert(ReferencedObject source) {
		return () -> new Document("id", source.id)                                   
                           .append("collection", … );                                
	}
}
// entity
{
  "_id" : "8cfb002",
  "ref" : {
    "id" : "9a48e32",                                                                
    "collection" : "…"                                                               
  }
}
```

|      | 从参考文档中读取/写入key `_id`，以便在 lookup query 中使用它们。 |
| ---- | ------------------------------------------------------------ |
|      | 集合的名称可以通过引用文档的 key 来读取。                    |

|      | 我们知道，在 lookup query 中使用各种MongoDB查询操作符是很诱人的，这也很好。但是有几个方面需要考虑。请确保有支持你的查询的索引。注意，解决这个问题需要一个服务器rountrip引起的延迟，考虑延迟策略。使用 `$or` 操作符批量加载一个文档引用集合。 原始元素的顺序会在内存中以最佳方式恢复。只有在使用等价表达式时才能恢复顺序，而在使用MongoDB查询操作符时不能恢复。在这种情况下，结果将在从store收到时或通过提供的 `@DocumentReference(sort)` 属性进行排序。再谈几句一般性的意见。你是否使用循环引用？问问你自己是否需要它们。懒加载文档引用是很难调试的。确保工具不会通过调用 `toString()` 等方式意外地触发代理解析。没有支持使用响应式基础设施来阅读文档引用。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 17.5.10. 映射框架事件

事件在映射过程的整个生命周期中被触发。这将在 [生命周期事件](https://springdoc.cn/spring-data-mongodb/#mongodb.mapping-usage.events) 部分进行描述。

在你的 Spring ApplicationContext 中声明这些Bean会导致它们在事件被调度时被调用。

### 17.6. 解构的类型

解构的实体用于在你的Java domain 模型中设计 value 对象，其属性被平铺到父方的MongoDB文档中。

#### 17.6.1. 解构类型映射

考虑以下domain模型，其中 `User.name` 被注解为 `@Unwrapped`。`@Unwrapped` 注解预示着 `UserName` 的所有属性应该被平铺到拥有 `name` 属性的 `user` 文档中。

Example 192. 解构对象的示例代码

```java
class User {

    @Id
    String userId;

    @Unwrapped(onEmpty = USE_NULL) 
    UserName name;
}

class UserName {

    String firstname;

    String lastname;

}
{
  "_id" : "1da2ba06-3ba7",
  "firstname" : "Emma",
  "lastname" : "Frost"
}
```

|      | 当加载 `name` 属性时，如果 `firstname` 和 `lastname` 都是 `null` 的或不存在，其值将被设置为 `null`。通过使用 `onEmpty=USE_EMPTY`，一个空的 `UserName` 将被创建，其属性值可能为 `null`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

对于不那么冗长的可嵌入类型声明，请使用 `@Unwrapped.Nullable` 和 `@Unwrapped.Empty` 来代替 `@Unwrapped(onEmpty = USE_NULL)` 和 `@Unwrapped(onEmpty = USE_EMPTY)` 。这两个注解都用JSR-305的 `@javax.annotation.Nonnull` 进行了元注解，以帮助进行无效性检查。

|      | 在一个解构的对象中使用复杂类型是可能的。但是，这些类型本身不能是，也不能包含解构的字段。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 17.6.2. 解构的类型字段名

通过使用 `@Unwrapped` 注解的可选 `prefix` 属性，一个值对象可以被多次解构。通过这样做，所选择的前缀会被前置到解构对象中的每个属性或 `@Field("…")` 名称上。请注意，如果多个属性渲染到相同的字段名，值将相互覆盖。

Example 193. 带有名称前缀的解构对象的示例代码

```java
class User {

    @Id
    String userId;

    @Unwrapped.Nullable(prefix = "u_") 
    UserName name;

    @Unwrapped.Nullable(prefix = "a_") 
    UserName name;
}

class UserName {

    String firstname;

    String lastname;
}
{
  "_id" : "a6a805bd-f95f",
  "u_firstname" : "Jean",             
  "u_lastname" : "Grey",
  "a_firstname" : "Something",        
  "a_lastname" : "Else"
}
```

|      | `UserName` 的所有属性都以 `u_` 为前缀。 |
| ---- | --------------------------------------- |
|      | `UserName` 的所有属性都以 `a_` 为前缀。 |

而在同一个属性上将 `@Field` 注解和 `@Unwrapped` 结合起来是没有意义的，因此会导致错误。在任何一个解构类型的属性上使用 `@Field` 是一个完全有效的方法。

Example 194. 使用 `@Field` 注解解构对象的示例代码

```java
public class User {

	@Id
    private String userId;

    @Unwrapped.Nullable(prefix = "u-") 
    UserName name;
}

public class UserName {

	@Field("first-name")              
    private String firstname;

	@Field("last-name")
    private String lastname;
}
{
  "_id" : "2647f7b9-89da",
  "u-first-name" : "Barbara",         
  "u-last-name" : "Gordon"
}
```

|      | `UserName` 的所有属性都以 `u-` 为前缀。                      |
| ---- | ------------------------------------------------------------ |
|      | 最终的字段名是 `@Unwrapped(prefix)` 和 `@Field(name)` 连接的结果。 |

#### 17.6.3. 对解构的对象进行查询

在类型和字段级别上可以定义对解构的属性的查询，因为提供的 `Criteria` 与domain类型相匹配。前缀和潜在的自定义字段名在渲染实际查询时将被考虑。使用解构对象的属性名称与所有包含的字段进行匹配，如下面的例子所示。

Example 195. 对解构的对象进行查询

```java
UserName userName = new UserName("Carol", "Danvers")
Query findByUserName = query(where("name").is(userName));
User user = template.findOne(findByUserName, User.class);
db.collection.find({
  "firstname" : "Carol",
  "lastname" : "Danvers"
})
```

也可以直接使用其属性名称来寻址解构对象的任何字段，如下面的片段所示。

Example 196. 对解构的对象的字段进行查询

```java
Query findByUserFirstName = query(where("name.firstname").is("Shuri"));
List<User> users = template.findAll(findByUserFirstName, User.class);
db.collection.find({
  "firstname" : "Shuri"
})
```

##### 按解构的字段排序。

解除包装的对象的字段可以通过其属性路径用于排序，如下面的例子所示。

Example 197. 对解构的字段进行排序

```java
Query findByUserLastName = query(where("name.lastname").is("Romanoff"));
List<User> user = template.findAll(findByUserName.withSort(Sort.by("name.firstname")), User.class);
db.collection.find({
  "lastname" : "Romanoff"
}).sort({ "firstname" : 1 })
```

|      | 尽管有可能，但使用解构对象本身作为排序标准包括其所有字段的不可预测的顺序，并可能导致不准确的排序。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 解构对象的字段投影

解构的对象的字段可以作为一个整体或通过单个字段进行投影，如下面的示例所示。

Example 198. 解构对象的投影

```java
Query findByUserLastName = query(where("name.firstname").is("Gamora"));
findByUserLastName.fields().include("name");                             
List<User> user = template.findAll(findByUserName, User.class);
db.collection.find({
  "lastname" : "Gamora"
},
{
  "firstname" : 1,
  "lastname" : 1
})
```

|      | 在一个解构的对象上的字段投影包括其所有的属性。 |
| ---- | ---------------------------------------------- |
|      |                                                |

Example 199. 投影在一个解构的对象的字段上。

```java
Query findByUserLastName = query(where("name.lastname").is("Smoak"));
findByUserLastName.fields().include("name.firstname");                   
List<User> user = template.findAll(findByUserName, User.class);
db.collection.find({
  "lastname" : "Smoak"
},
{
  "firstname" : 1
})
```

|      | 在一个解构的对象上的字段投影包括其所有的属性。 |
| ---- | ---------------------------------------------- |
|      |                                                |

##### 对解构的对象进行 Example 查询

解构的对象可以像其他类型的对象一样，在一个 `Example` probe 中使用。请查看 [通过 Example 查询](https://springdoc.cn/spring-data-mongodb/#query-by-example.running) 部分，以了解更多关于这一功能的信息。

##### Repository 对解构对象的查询。

`Repository` 抽象允许衍生出对解构对象的字段以及整个对象的查询。

Example 200. Repository 对解构对象的查询。

```java
interface UserRepository extends CrudRepository<User, String> {

	List<User> findByName(UserName username);         

	List<User> findByNameFirstname(String firstname); 
}
```

|      | 与解构对象的所有字段相匹配。 |
| ---- | ---------------------------- |
|      | 与 `firstname` 相匹配。。    |

|      | 即使 repository 的 `create-query-indexes` 命名空间属性被设置为 `true`，也会暂停创建解构对象的索引。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 17.6.4. 解构对象的更新

解构的对象可以像任何其他属于domain模型的对象一样被更新。映射层负责将结构平铺到其周围环境中。如下面的例子所示，可以更新解构对象的单个属性，也可以更新整个值。

Example 201. 更新一个解构的对象的单个字段。

```java
Update update = new Update().set("name.firstname", "Janet");
template.update(User.class).matching(where("id").is("Wasp"))
   .apply(update).first()
db.collection.update({
  "_id" : "Wasp"
},
{
  "$set" { "firstname" : "Janet" }
},
{ ... }
)
```

Example 202. 更新一个解构对象。

```java
Update update = new Update().set("name", new Name("Janet", "van Dyne"));
template.update(User.class).matching(where("id").is("Wasp"))
   .apply(update).first()
db.collection.update({
  "_id" : "Wasp"
},
{
  "$set" {
    "firstname" : "Janet",
    "lastname" : "van Dyne",
  }
},
{ ... }
)
```

#### 17.6.5. 对解构对象进行聚合

[聚合框架](https://springdoc.cn/spring-data-mongodb/#mongo.aggregation) 将尝试映射类型聚合的解构值。在引用其中一个值时，请确保与包括 wrapper 对象在内的属性路径一起工作。除此以外，不需要特别的操作。

#### 17.6.6. 解构对象的索引

我们可以将 `@Indexed` 注解附加到解构类型的属性上，就像对普通对象那样。不可能在拥有的属性上同时使用 `@Indexed` 和 `@Unwrapped` 注解。

```java
public class User {

	@Id
    private String userId;

    @Unwrapped(onEmpty = USE_NULL)
    UserName name;                    

    // Invalid -> InvalidDataAccessApiUsageException
    @Indexed                          
    @Unwrapped(onEmpty = USE_Empty)
    Address address;
}

public class UserName {

    private String firstname;

    @Indexed
    private String lastname;           
}
```

|      | 在 `users` 集合中为 `lastname` 创建索引。    |
| ---- | -------------------------------------------- |
|      | 无效的 `@Indexed` 和 `@Unwrapped` 一起使用。 |

### 17.7. 自定义转换 - 覆盖默认映射

影响映射结果的最简单的方法是通过 `@Field` 注解指定所需的原生MongoDB目标类型。这允许在 domain 模型中使用非MongoDB类型，如 `BigDecimal`，同时以本地 `org.bson.types.Decimal128` 格式持久化数值。

Example 203. 明确的目标类型映射

```java
public class Payment {

  @Id String id; 

  @Field(targetType = FieldType.DECIMAL128) 
  BigDecimal value;

  Date date; 

}
{
  "_id"   : ObjectId("5ca4a34fa264a01503b36af8"), 
  "value" : NumberDecimal(2.099), 
  "date"   : ISODate("2019-04-03T12:11:01.870Z") 
}
```

|      | 代表有效 `ObjectId` 的字符串 *id* 值会自动转换。详见 [在映射层中如何处理 `_id` 字段](https://springdoc.cn/spring-data-mongodb/#mongo-template.id-handling) 。 |
| ---- | ------------------------------------------------------------ |
|      | 所需的目标类型被明确定义为 `Decimal128`，翻译成 `NumberDecimal`。否则 `BigDecimal` 的值就会被截成一个 `String`。 |
|      | `Date` 值由MongoDB驱动本身处理，并存储为 `ISODate`。         |

上面的片段对于提供简单的类型提示很方便。为了获得对映射过程更精细的控制，你可以用 `MongoConverter` 实现注册 Spring converter，例如 `MappingMongoConverter`。

`MappingMongoConverter` 在尝试映射对象本身之前，会检查是否有Spring converter 可以处理特定的类。要 "劫持" `MappingMongoConverter` 的正常映射策略，也许是为了提高性能或其他自定义映射需求，你首先需要创建一个Spring `Converter` 接口的实现，然后将其与 `MappingConverter` 注册。

|      | 关于Spring类型转换服务的更多信息，请看 [这里](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#validation)的参考文档。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 17.7.1. 通过使用注册的Spring Converter来节约成本

下面的例子显示了一个 `Converter` 的实现，它将 `Person` 对象转换为 `org.bson.Document`。

```java
public class PersonWriteConverter implements Converter<Person, Document> {

  public Document convert(Person source) {
    Document document = new Document();
    document.put("_id", source.getId());
    document.put("name", source.getFirstName());
    document.put("age", source.getAge());
    return document;
  }
}
```

#### 17.7.2. 通过使用 Spring Converter 进行读取

下面的例子显示了一个 `Converter` 的实现，它可以从 `Document` 转换为 `Person` 对象。

```java
public class PersonReadConverter implements Converter<Document, Person> {

  public Person convert(Document source) {
    Person p = new Person((ObjectId) source.get("_id"), (String) source.get("name"));
    p.setAge((Integer) source.get("age"));
    return p;
  }
}
```

#### 17.7.3. 用 `MongoConverter` 注册 Spring Converter

```java
class MyMongoConfiguration extends AbstractMongoClientConfiguration {

	@Override
	public String getDatabaseName() {
		return "database";
	}

	@Override
	protected void configureConverters(MongoConverterConfigurationAdapter adapter) {
		adapter.registerConverter(new com.example.PersonReadConverter());
		adapter.registerConverter(new com.example.PersonWriteConverter());
	}
}
```

下面是一个Spring `Converter` 实现的例子，它将一个 `String` 转换为一个自定义的 `Email` 值对象。

```java
@ReadingConverter
public class EmailReadConverter implements Converter<String, Email> {

  public Email convert(String source) {
    return Email.valueOf(source);
  }
}
```

如果你编写的 `Converter` 的源类型和目标类型都是原生类型，我们无法确定我们应该把它看作是一个读或写的 `Converter`。将 `Converter` 实例注册为这两种类型可能会导致不必要的结果。例如，一个 `Converter<String, Long>` 是模棱两可的，尽管在写入时试图将所有的 `String` 实例转换成 `Long` 实例可能没有意义。为了让你强制基础设施只为一种方式注册转换器，我们提供了 `@ReadingConverter` 和 `@WritingConverter` 注解，以便在转换器实现中使用。

Converter 需要明确注册，因为实例不是从classpath或容器扫描中获取的，以避免在转换服务中进行不必要的注册以及这种注册产生的副作用。Converter 是通过 `CustomConversions` 注册的，它是一个中央设施，允许根据源和目标类型注册和查询已注册的 Converter。

`CustomConversions` 带有一套预先定义的转换器注册（converter registration）。

- JSR-310 Converter 用于 `java.time`、`java.util.Date` 和 `String` 类型之间的转换。

|      | 本地时间类型的默认 converter（例如，`LocalDateTime` 到 `java.util.Date`）依靠系统默认的时区设置在这些类型之间进行转换。你可以通过注册你自己的convert来覆盖默认的convert。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

###### Converter 歧义

一般来说，我们会检查 `Converter` 的实现，看他们转换的源类型和目标类型。根据这些类型中是否有一个是底层数据访问API可以原生处理的类型，我们将 converter 实例注册为读或写转换器。下面的例子显示了一个写 converter 和一个读 converter（注意区别在于 converter 上修饰词的顺序）。

```java
// Write converter as only the target type is one that can be handled natively
class MyConverter implements Converter<Person, String> { … }

// Read converter as only the source type is one that can be handled natively
class MyConverter implements Converter<String, Person> { … }
```

### 17.8. 属性转换器 - 映射特定字段

虽然 [基于类型的转换](https://springdoc.cn/spring-data-mongodb/#mongo.custom-converters) 已经提供了影响目标 store 中某些类型的转换和表示的方法，但当只有特定类型的某些值或属性应被考虑转换时，它有局限性。基于属性的转换器允许在每个属性的基础上配置转换规则，无论是声明性的（通过 `@ValueConverter`）还是程序性的（通过为特定属性注册一个 `PropertyValueConverter`）。

一个 `PropertyValueConverter` 可以将一个给定的值转化为它的存储表示（写）和返回（读），如下表所示。附加的 `ValueConversionContext` 提供了额外的信息，如映射元数据和直接 `read` / `write` 方法。

Example 204. 一个简单的 PropertyValueConverter

```java
class ReversingValueConverter implements PropertyValueConverter<String, String, ValueConversionContext> {

  @Override
  public String read(String value, ValueConversionContext context) {
    return reverse(value);
  }

  @Override
  public String write(String value, ValueConversionContext context) {
    return reverse(value);
  }
}
```

你可以通过委托给 `PropertyValueConversions`，从 `CustomConversions#getPropertyValueConverter(…)` 获得 `PropertyValueConverter` 实例，通常是通过使用 `PropertyValueConverterFactory` 来提供实际 converter。根据你的应用程序的需求，你可以链式或装饰 `PropertyValueConverterFactory` 的多个实例 - 例如，应用缓存。默认情况下，Spring Data MongoDB使用一个缓存实现，可以为具有默认构造函数或枚举值的类型提供服务。一组预定义的工厂可以通过 `PropertyValueConverterFactory` 中的工厂方法获得。你可以使用 `PropertyValueConverterFactory.beanFactoryAware(…)` 来从 `ApplicationContext` 获取 `PropertyValueConverter` 实例。

你可以通过 `ConverterConfiguration` 改变默认行为。

#### 17.8.1. 声明性 Value Converter

`PropertyValueConverter` 最直接的用法是用定义转换器类型的 `@ValueConverter` 注解来注解属性。

Example 205. 声明性的 PropertyValueConverter

```java
class Person {

  @ValueConverter(ReversingValueConverter.class)
  String ssn;
}
```

#### 17.8.2. 程序化 Value Converter 注册

程序化注册通过使用 `PropertyValueConverterRegistrar` 为实体模型中的属性注册 `PropertyValueConverter` 实例，正如下面的例子所示。声明式注册和编程式注册的区别在于，编程式注册完全发生在实体模型之外。如果你不能或不想注解实体模型，这样的方法就很有用。

Example 206. 编程式 PropertyValueConverter 注册

```java
PropertyValueConverterRegistrar registrar = new PropertyValueConverterRegistrar();

registrar.registerConverter(Address.class, "street", new PropertyValueConverter() { … }); 

// type safe registration
registrar.registerConverter(Person.class, Person::getSsn())                               
  .writing(value -> encrypt(value))
  .reading(value -> decrypt(value));
```

|      | 为由其名称确定的字段注册一个转换器。                    |
| ---- | ------------------------------------------------------- |
|      | 类型安全的变体，允许注册一个转换器和它的转换 function。 |

|      | 在注册 converter 时，不支持用圆点符号（如 `registerConverter(Person.class, "address.street", …)`）来唠叨跨属性到子文档的问题。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 17.8.3. MongoDB的属性值转换

前面的章节概述了 `PropertyValueConverters` 的目的和整体结构。本节重点讨论MongoDB的具体方面。

##### MongoValueConverter 和 MongoConversionContext

`MongoValueConverter` 提供了一个预类型的 `PropertyValueConverter` 接口，该接口使用 `MongoConversionContext`。

##### MongoCustomConversions 配置

默认情况下，`MongoCustomConversions` 可以处理声明式 value 转换，这取决于配置的 `PropertyValueConverterFactory`。`MongoConverterConfigurationAdapter` 有助于设置编程式 value 转换或定义要使用的 `PropertyValueConverterFactory`。

Example 207. 配置示例

```java
MongoCustomConversions.create(configurationAdapter -> {

    SimplePropertyValueConversions valueConversions = new SimplePropertyValueConversions();
    valueConversions.setConverterFactory(…);
    valueConversions.setValueConverterRegistry(new PropertyValueConverterRegistrar()
        .registerConverter(…)
        .buildRegistry());

    configurationAdapter.setPropertyValueConversions(valueConversions);
});
```

## 18. 分片

MongoDB通过分片支持大型数据集，这是一种将数据分布在多个数据库服务器上的方法。请参考 [MongoDB文档](https://docs.mongodb.com/manual/sharding/)，了解如何设置分片集群、其要求和限制。

Spring Data MongoDB 使用 `@Sharded` 注解来识别存储在sharded集合中的实体，如下图所示。

```java
@Document("users")
@Sharded(shardKey = { "country", "userId" }) 
public class User {

	@Id
	Long id;

	@Field("userid")
	String userId;

	String country;
}
```

|      | shardKey 的属性会被映射到实际的字段名。 |
| ---- | --------------------------------------- |
|      |                                         |

### 18.1. 集合的分片

Spring Data MongoDB 不会自动设置集合的分片，也不会设置所需的索引。下面的片段显示了如何使用MongoDB客户端API来实现这一目的。

```java
MongoDatabase adminDB = template.getMongoDbFactory()
    .getMongoDatabase("admin");                                     

adminDB.runCommand(new Document("enableSharding", "db"));           

Document shardCmd = new Document("shardCollection", "db.users")     
	.append("key", new Document("country", 1).append("userid", 1)); 

adminDB.runCommand(shardCmd);
```

|      | 分片命令需要针对 *admin* 数据库运行。        |
| ---- | -------------------------------------------- |
|      | 如有必要，为特定的数据库启用分片。           |
|      | 分片是数据库中启用了分片的集合。             |
|      | 指定分片key。这个例子使用基于 range 的分片。 |

### 18.2. 分片 key 处理

分片key由一个或多个属性组成，这些属性必须存在于目标集合中的每个文档中。它被用来在不同的分片中分配文档。

将 `@Sharded` 注解添加到实体中，使Spring Data MongoDB能够应用分片场景所需的最佳努力优化。这意味着在上传实体时，如果尚未存在的话，基本上可以将所需的分片key信息添加到 `replaceOne` 过滤器查询中。这可能需要一个额外的服务器往返，以确定当前分片key的实际值。

|      | 通过设置 `@Sharded(immutableKey = true)`，Spring Data不会试图检查一个实体的分片key是否被改变。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

请参阅 [MongoDB文档](https://docs.mongodb.com/manual/reference/method/db.collection.replaceOne/#upsert) 以了解更多细节。下面的列表包含了哪些操作有资格被分片key自动包含。

- `(Reactive)CrudRepository.save(…)`
- `(Reactive)CrudRepository.saveAll(…)`
- `(Reactive)MongoTemplate.save(…)`

## 19. Kotlin 的支持

[Kotlin](https://kotlinlang.org/) 是一种静态类型的语言，以JVM（和其他平台）为目标，可以编写简洁优雅的代码，同时与现有的Java编写的库有很好的 [互操作性](https://kotlinlang.org/docs/reference/java-interop.html)。

Spring Data为Kotlin提供了一流的支持，让开发者在编写Kotlin应用程序时，几乎就像Spring Data是一个Kotlin 原生框架一样。

用Kotlin构建Spring应用程序的最简单方法是利用Spring Boot及其 [专门的Kotlin支持](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-kotlin.html)。本综合 [教程](https://spring.io/guides/tutorials/spring-boot-kotlin/)将教你如何使用 [start.springboot.io](https://start.springboot.io/#!language=kotlin&type=gradle-project) 用 Kotlin 构建 Spring Boot 应用程序。

### 19.1. 要求

Spring Data支持Kotlin 1.3，要求classpath上有 [`kotlin-stdlib`](https://bintray.com/bintray/jcenter/org.jetbrains.kotlin%3Akotlin-stdlib)（或其变体之一，如 [`kotlin-stdlib-jdk8`](https://bintray.com/bintray/jcenter/org.jetbrains.kotlin%3Akotlin-stdlib-jdk8)）和 [`kotlin-reflect`](https://bintray.com/bintray/jcenter/org.jetbrains.kotlin%3Akotlin-reflect)。如果你通过 [start.springboot.io](https://start.springboot.io/#!language=kotlin&type=gradle-project) 启动一个Kotlin项目，这些是默认提供的。

### 19.2. 空值安全（Null Safety）

Kotlin的关键特性之一是 [null safety](https://kotlinlang.org/docs/reference/null-safety.html)，它在编译时干净地处理 `null` 值。这使得应用程序通过 nullability 声明和 "值或无值" 语义的表达更加安全，而不需要支付封装器的费用，比如 `Optional`。（Kotlin允许使用带有 nullable 值的功能结构。请看这个 [关于Kotlin空值安全的综合指南](https://www.baeldung.com/kotlin-null-safety)）。

尽管Java不允许你在其类型系统中表达 null 安全，但Spring Data API在 `org.springframework.lang` 包中声明了 [JSR-305](https://jcp.org/en/jsr/detail?id=305) 工具友好注解。默认情况下，Kotlin中使用的Java API中的类型被识别为 [platform types](https://kotlinlang.org/docs/reference/java-interop.html#null-safety-and-platform-types)，对于这些类型，`null` 值检查被放松了。 [Kotlin对JSR-305注解的支持](https://kotlinlang.org/docs/reference/java-interop.html#jsr-305-support) 和Spring nullability注解为Kotlin开发者提供了整个Spring Data API的空值安全，其优点是可以在编译时处理 `null` 值相关问题。

参见 [Repository 方法的 Null 处理](https://springdoc.cn/spring-data-mongodb/#repositories.nullability)， 空值安全如何应用于 Spring Data Repository。

|      | 你可以通过添加 `-Xjsr305` 编译标志和以下选项来配置 JSR-305 检查：`-Xjsr305={strict|warn|ignore}`。对于 Kotlin 1.1 以上版本，默认行为与 `-Xjsr305=warn` 相同。`strict` 值需要考虑到Spring Data API的null-safety。Kotlin类型是从Spring API中推断出来的，但在使用时应注意Spring API 的 nullability 声明可能会发生变化，甚至在次要版本之间，未来可能会添加更多检查。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 目前还不支持泛型类型参数、varargs 和数组元素的 nullability，但在即将发布的版本中应该会支持。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 19.3. 对象映射

关于Kotlin对象如何被具体化的细节，请参见 [对 Kotlin 的支持](https://springdoc.cn/spring-data-mongodb/#mapping.kotlin)。

### 19.4. 扩展

Kotlin [extensions](https://kotlinlang.org/docs/reference/extensions.html) 提供了用额外功能扩展现有类的能力。Spring Data Kotlin APIs使用这些扩展来为现有的Spring APIs添加新的Kotlin特定便利。

|      | 请记住，Kotlin扩展需要被导入才能使用。与静态导入类似，IDE应该在大多数情况下自动建议导入。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

例如， [Kotlin的统一类型参数](https://kotlinlang.org/docs/reference/inline-functions.html#reified-type-parameters) 为 [JVM的泛型类型擦除](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html) 提供了一种变通方法，Spring Data提供了一些扩展来利用这一特性。这使得Kotlin的API更加完善。

要在Java中检索 `SWCharacter` 对象的列表，你通常会写如下。

```java
Flux<SWCharacter> characters  = template.find(SWCharacter.class).inCollection("star-wars").all()
```

使用Kotlin和Spring Data扩展，你可以写出以下内容。

```kotlin
val characters = template.find<SWCharacter>().inCollection("star-wars").all()
// or (both are equivalent)
val characters : Flux<SWCharacter> = template.find().inCollection("star-wars").all()
```

和Java一样，Kotlin中的 `characters` 是强类型的，但Kotlin巧妙的类型推理使语法更短。

Spring Data MongoDB提供了以下扩展。

- 对 `MongoOperations`、`ReactiveMongoOperations`、`FluentMongoOperations`、`ReactiveFluentMongoOperations` 和 `Criteria` 的 Reified Generics 支持。
- [Kotlin的类型安全的查询](https://springdoc.cn/spring-data-mongodb/#mongo.query.kotlin-support)
- `ReactiveFluentMongoOperations` 的 [协程（Coroutine）](https://springdoc.cn/spring-data-mongodb/#kotlin.coroutines) 扩展。

### 19.5. 协程（Coroutine）

Kotlin [Coroutines](https://kotlinlang.org/docs/reference/coroutines-overview.html) 是轻量级的线程，允许写非阻塞的代码。在语言方面，`suspend` 函数为异步操作提供了一个抽象，而在库方面， [kotlinx.coroutines](https://github.com/Kotlin/kotlinx.coroutines) 提供了 [`async { }`](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/async.html) 等函数和 [`Flow`](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/-flow/index.html) 等类型。

Spring Data 模块在以下范围内提供对Coroutines的支持。

- 在Kotlin扩展中支持 [Deferred](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-deferred/index.html) 和 [Flow](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/-flow/index.html) 返回值

#### 19.5.1. 依赖

当 `kotlinx-coroutines-core`、`kotlinx-coroutines-reactive` 和 `kotlinx-coroutines-reactor` 依赖项在 classpath 中时，coroutines 支持被启用。

Example 208. Dependencies to add in Maven pom.xml

```xml
<dependency>
	<groupId>org.jetbrains.kotlinx</groupId>
	<artifactId>kotlinx-coroutines-core</artifactId>
</dependency>

<dependency>
	<groupId>org.jetbrains.kotlinx</groupId>
	<artifactId>kotlinx-coroutines-reactive</artifactId>
</dependency>

<dependency>
	<groupId>org.jetbrains.kotlinx</groupId>
	<artifactId>kotlinx-coroutines-reactor</artifactId>
</dependency>
```

|      | 支持 `1.3.0` 及以上版本。 |
| ---- | ------------------------- |
|      |                           |

#### 19.5.2. 响应式（Reactive）是如何转化为 Coroutine 的？

对于返回值，从Reactive到 Coroutine APIs的转换如下。

- `fun handler(): Mono<Void>` 变成 `suspend fun handler()`
- `fun handler(): Mono<T>` 变成 `suspend fun handler(): T` 或 `suspend fun handler(): T?` 取决于 `Mono` 是否可以为空（具有更多静态类型的优势）
- `fun handler(): Flux<T>` 变成 `fun handler(): Flow<T>`

[`Flow`](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/-flow/index.html) 是 Coroutine 世界中的 `Flux` 等价物，适用于热（hot）流或冷（cold）流，有限流或无限流，主要区别如下。

- `Flow` 是基于推的，而 `Flux` 是推拉混合的。
- 背压是通过暂停（suspending）功能实现的。
- `Flow` 只有 [单一暂停的 `collect` 方法](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/-flow/collect.html)，操作者是作为 [扩展](https://kotlinlang.org/docs/reference/extensions.html) 实现的。
- 由于有了 Coroutine， [操作者很容易实现](https://github.com/Kotlin/kotlinx.coroutines/tree/master/kotlinx-coroutines-core/common/src/flow/operators)。
- 扩展功能允许向 `Flow` 添加自定义操作符。
- Collect 操作是暂停（suspending ）功能
- [`map` 操作符](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/map.html) 支持异步操作（不需要 `flatMap`），因为它需要一个暂停（suspending）函数参数

阅读这篇关于 [使用Spring、Coroutines和Kotlin Flow 实现响应式的博客](https://spring.io/blog/2019/04/12/going-reactive-with-spring-coroutines-and-kotlin-flow)，了解更多细节，包括如何使用 Coroutine 并发运行代码。

#### 19.5.3. Repository

下面是一个 Coroutine repository 的例子。

```kotlin
interface CoroutineRepository : CoroutineCrudRepository<User, String> {

    suspend fun findOne(id: String): User

    fun findByFirstname(firstname: String): Flow<User>

    suspend fun findAllByFirstname(id: String): List<User>
}
```

Coroutine repository 是建立在响应式repository上的，通过Kotlin的 Coroutine 暴露了数据访问的非阻塞特性。Coroutine repository 上的方法可以由查询方法或自定义实现来支持。如果自定义方法是可暂停（`suspend`）的，调用自定义实现方法会将 Coroutine 调用传播给实际的实现方法，而不需要实现方法返回一个响应式类型，如 `Mono` 或 `Flux`。

请注意，根据方法声明的不同，coroutine上下文（context）可能可用也可能不可用。为了保留对上下文的访问，要么使用 `suspend` 声明你的方法，要么返回一个能够传播上下文的类型，比如 `Flow`。

- `suspend fun findOne(id: String): User`: 通过挂起（suspending）的方式一次性同步地检索数据。
- `fun findByFirstname(firstname: String): Flow<User>`: 检索一个数据流。`Flow` 是急于创建的，而数据是在 `Flow` 交互时获取的（`Flow.collect(…)`）。
- `fun getUser(): User`: 检索数据时，会阻塞线程，并且没有上下文传播。这应该被避免。

|      | 只有当 Repository 继承了 `CoroutineCrudRepository` 接口时，才能发现 Coroutine Repository。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

## 20. 支持 JMX

对MongoDB的JMX支持暴露了在单个MongoDB服务器实例的管理数据库上运行 "serverStatus" 命令的结果。它还公开了一个管理MBean，即 `MongoAdmin`，让你执行管理操作，如删除或创建一个数据库。JMX功能建立在Spring框架中可用的JMX功能集之上。更多细节见 [这里](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/integration.html#jmx)。

### 20.1. MongoDB JMX 配置

Spring的Mongo命名空间可以让你启用JMX功能，如下例所示。

Example 209. 配置MongoDB的XML schema

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:mongo="http://www.springframework.org/schema/data/mongo"
  xsi:schemaLocation="
    http://www.springframework.org/schema/context
    https://www.springframework.org/schema/context/spring-context-3.0.xsd
    http://www.springframework.org/schema/data/mongo
    https://www.springframework.org/schema/data/mongo/spring-mongo-1.0.xsd
    http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

    <!-- Default bean name is 'mongo' -->
    <mongo:mongo-client host="localhost" port="27017"/>

    <!-- by default look for a Mongo object named 'mongo' -->
    <mongo:jmx/>

    <context:mbean-export/>

    <!-- To translate any MongoExceptions thrown in @Repository annotated classes -->
    <context:annotation-config/>

    <bean id="registry" class="org.springframework.remoting.rmi.RmiRegistryFactoryBean" p:port="1099" />

    <!-- Expose JMX over RMI -->
    <bean id="serverConnector" class="org.springframework.jmx.support.ConnectorServerFactoryBean"
        depends-on="registry"
        p:objectName="connector:name=rmi"
        p:serviceUrl="service:jmx:rmi://localhost/jndi/rmi://localhost:1099/myconnector" />

</beans>
```

前面的代码暴露了几个 MBean。

- `AssertMetrics`
- `BackgroundFlushingMetrics`
- `BtreeIndexCounters`
- `ConnectionMetrics`
- `GlobalLockMetrics`
- `MemoryMetrics`
- `OperationCounters`
- `ServerInfo`
- `MongoAdmin`

下面是JConsole的截图，显示了所产生的配置。

![jconsole](images/MongoDB笔记/jconsole.png)

# 附录

## Appendix A: 命名空间参考

### `<repositories />` 元素

`<repositories />` 元素触发了Spring Data资源库基础设施的设置。最重要的属性是 `base-package`，它定义了扫描 Spring Data Repository 接口的包。参见 “[XML 配置](https://springdoc.cn/spring-data-mongodb/#repositories.create-instances.xml)”。下表描述了 `<repositories />` 元素的属性。

| Name                           | Description                                                  |
| :----------------------------- | :----------------------------------------------------------- |
| `base-package`                 | 定义在自动检测模式下扫描扩展 `*Repository`（实际接口由特定的Spring Data模块决定）的 repository 接口的包。在配置的包下面的所有包也会被扫描到。允许使用通配符。 |
| `repository-impl-postfix`      | 定义用于自动检测自定义 repository 实现的后缀。名称以配置的后缀结尾的类被认为是候选类。默认为 `Impl`。 |
| `query-lookup-strategy`        | 确定用于创建 finder 查询的策略。详见 “[Query 的查询策略](https://springdoc.cn/spring-data-mongodb/#repositories.query-methods.query-lookup-strategies)” 。默认为 `create-if-not-found`。 |
| `named-queries-location`       | 定义了搜索包含外部定义的查询的属性（Properties）文件的位置。 |
| `consider-nested-repositories` | 是否应该考虑嵌套的 repository 接口定义。默认为 `false`。     |

## Appendix B: Populators 命名空间参考

### <populator /> 元素

`<populator />` 元素允许通过Spring Data Repository 基础设施填充data store。[[4](https://springdoc.cn/spring-data-mongodb/#_footnotedef_4)]

| Name        | Description                                                  |
| :---------- | :----------------------------------------------------------- |
| `locations` | 在哪里可以找到从 repository 中读取对象的文件，应将其填充到 repository 中。 |

## Appendix C: Repository query 关键字

### 支持的 query method subject 关键词

下表列出了Spring Data Repository 查询推导机制通常支持的subject关键字，以表达谓词。有关支持的关键字的确切列表，请查阅特定 store 的文档，因为这里列出的一些关键字可能在特定 store 中不被支持。

| Keyword                                                      | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `find…By`, `read…By`, `get…By`, `query…By`, `search…By`, `stream…By` | 一般查询方法，通常返回 repository 类型、`Collection` 或 `Streamable` 子类型或result wrapper，如 `Page`、`GeoResults` 或任何其他store特定的result wrapper。可以作为 `findBy…`、`findMyDomainTypeBy…` 或与其他关键字结合使用。 |
| `exists…By`                                                  | Exists 投影，通常返回一个 `boolean` 结果。                   |
| `count…By`                                                   | 返回数字结果的count投影。                                    |
| `delete…By`, `remove…By`                                     | 删除查询方法要么不返回结果（void），要么返回删除数量。       |
| `…First<number>…`, `…Top<number>…`                           | 将查询结果限制在第一个 `<number>` 的结果。这个关键词可以出现在主语中 `find`（以及其他关键词）和 `by` 之间的任何地方。 |
| `…Distinct…`                                                 | 使用 distinct 查询，只返回唯一的结果。请咨询特定store的文档是否支持该功能。这个关键字可以出现在 `find`（和其他关键字）和 `by` 之间的任何地方。 |

### 支持的查询方法 predicate 关键字和修饰语

下表列出了Spring Data Repository 查询推导机制通常支持的谓词（predicate）。但是，请查阅特定store的文档以了解支持的关键字的确切列表，因为这里列出的一些关键字可能在特定的store中不被支持。

| 逻辑关键字            | 关键字表达式                                   |
| :-------------------- | :--------------------------------------------- |
| `AND`                 | `And`                                          |
| `OR`                  | `Or`                                           |
| `AFTER`               | `After`, `IsAfter`                             |
| `BEFORE`              | `Before`, `IsBefore`                           |
| `CONTAINING`          | `Containing`, `IsContaining`, `Contains`       |
| `BETWEEN`             | `Between`, `IsBetween`                         |
| `ENDING_WITH`         | `EndingWith`, `IsEndingWith`, `EndsWith`       |
| `EXISTS`              | `Exists`                                       |
| `FALSE`               | `False`, `IsFalse`                             |
| `GREATER_THAN`        | `GreaterThan`, `IsGreaterThan`                 |
| `GREATER_THAN_EQUALS` | `GreaterThanEqual`, `IsGreaterThanEqual`       |
| `IN`                  | `In`, `IsIn`                                   |
| `IS`                  | `Is`, `Equals`, (or no keyword)                |
| `IS_EMPTY`            | `IsEmpty`, `Empty`                             |
| `IS_NOT_EMPTY`        | `IsNotEmpty`, `NotEmpty`                       |
| `IS_NOT_NULL`         | `NotNull`, `IsNotNull`                         |
| `IS_NULL`             | `Null`, `IsNull`                               |
| `LESS_THAN`           | `LessThan`, `IsLessThan`                       |
| `LESS_THAN_EQUAL`     | `LessThanEqual`, `IsLessThanEqual`             |
| `LIKE`                | `Like`, `IsLike`                               |
| `NEAR`                | `Near`, `IsNear`                               |
| `NOT`                 | `Not`, `IsNot`                                 |
| `NOT_IN`              | `NotIn`, `IsNotIn`                             |
| `NOT_LIKE`            | `NotLike`, `IsNotLike`                         |
| `REGEX`               | `Regex`, `MatchesRegex`, `Matches`             |
| `STARTING_WITH`       | `StartingWith`, `IsStartingWith`, `StartsWith` |
| `TRUE`                | `True`, `IsTrue`                               |
| `WITHIN`              | `Within`, `IsWithin`                           |

除了过滤谓词（predicate）外，还支持以下修饰语列表。

| Keyword                            | Description                                                  |
| :--------------------------------- | :----------------------------------------------------------- |
| `IgnoreCase`, `IgnoringCase`       | 与谓语关键字一起使用，用于不区分大小写的比较。               |
| `AllIgnoreCase`, `AllIgnoringCase` | 忽略所有合适的属性的情况。在查询方法谓语的某处使用。         |
| `OrderBy…`                         | 指定一个静态的排序顺序，后面是属性路径和方向（如 `OrderByFirstnameAscLastnameDesc`）。 |

## Appendix D: Repository 查询返回类型

### 支持的查询返回类型

下表列出了Spring Data Repository 通常支持的返回类型。但是，请查阅store的具体文档以了解支持的返回类型的确切列表，因为这里列出的一些类型可能不被特定的store所支持。

|      | 地理空间类型（如 `GeoResult`、`GeoResults` 和 `GeoPage`）仅适用于支持地理空间查询的data store。一些store模块可以定义他们自己的结果包装类型。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

| 返回类型                                                     | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `void`                                                       | 表示没有返回值。                                             |
| Primitives                                                   | Java 基本类型。                                              |
| Wrapper types                                                | Java包装类型。                                               |
| `T`                                                          | 一个唯一的实体。期望查询方法最多返回一个结果。如果没有找到结果，则返回 `null`。多于一个结果会触发 `IncorrectResultSizeDataAccessException`。 |
| `Iterator<T>`                                                | 迭代器 `Iterator`。                                          |
| `Collection<T>`                                              | 集合 `Collection`。                                          |
| `List<T>`                                                    | 列表 `List`.                                                 |
| `Optional<T>`                                                | 一个Java 8或 Guava 的 `Optional`。期待查询方法最多返回一个结果。如果没有结果，会返回 `Optional.empty()` 或 `Optional.absent()`。多于一个结果会触发 `IncorrectResultSizeDataAccessException`。 |
| `Option<T>`                                                  | Scala 或 Vavr 的 `Option` 类型。在语义上与前面介绍的Java 8的 `Optional` 行为相同。 |
| `Stream<T>`                                                  | Java 8 的 `Stream`.                                          |
| `Streamable<T>`                                              | `Iterable` 的一个方便的扩展，它直接暴露了stream、map和过滤（filter）结果的方法，并将它们连接起来等等。 |
| 实现 `Streamable` 并接受 `Streamable` 构造函数或工厂方法参数的类型 | 暴露了一个构造函数或 `…of(…)`/`…valueOf(…)` 工厂方法的类型，以 `Streamable` 作为参数。详见 [返回自定义的 Streamable Wrapper 类型](https://springdoc.cn/spring-data-mongodb/#repositories.collections-and-iterables.streamable-wrapper)。 |
| Vavr `Seq`, `List`, `Map`, `Set`                             | Vavr集合类型。详见 [支持 Vavr 集合](https://springdoc.cn/spring-data-mongodb/#repositories.collections-and-iterables.vavr)。 |
| `Future<T>`                                                  | 一个 `Future`。期待一个方法被 `@Async` 注解，并要求启用Spring的异步方法执行能力。 |
| `CompletableFuture<T>`                                       | 一个Java 8 `CompletableFuture`。期待一个方法被 `@Async` 注解，并要求启用Spring的异步方法执行能力。 |
| `Slice<T>`                                                   | 有大小的数据块，显示是否有更多的数据可用。需要一个 `Pageable` 方法参数。 |
| `Page<T>`                                                    | 带有附加信息的 `Slice`，例如结果的总数。需要一个 `Pageable` 方法参数。 |
| `GeoResult<T>`                                               | 带有附加信息的结果条目，如与参考位置的距离。                 |
| `GeoResults<T>`                                              | 带有附加信息的 `GeoResult<T>` list，例如与参考位置的平均距离。 |
| `GeoPage<T>`                                                 | 带有 `GeoResult<T>` 的 Page ，例如到参考位置的平均距离。     |
| `Mono<T>`                                                    | Project Reactor `Mono`，使用响应式 repository 发出零或一个元素。期待查询方法最多返回一个结果。如果没有结果，会返回 `Mono.empty()`。多于一个结果会触发 `IncorrectResultSizeDataAccessException`。 |
| `Flux<T>`                                                    | Project Reactor `Flux`，使用响应式 repository 发出零、一或许多元素。返回 `Flux` 的查询也可以发射出无限多的元素。 |
| `Single<T>`                                                  | RxJava `Single`，使用响应式 repository 发出一个单一元素。期望查询方法最多返回一个结果。如果没有结果，会返回 `Mono.empty()`。多于一个结果会触发 `IncorrectResultSizeDataAccessException`。 |
| `Maybe<T>`                                                   | RxJava `Maybe`，使用响应式 repository 来发射零或一个元素。期望查询方法最多返回一个结果。如果没有结果，会返回 `Mono.empty()`。多于一个结果会触发 `IncorrectResultSizeDataAccessException`。 |
| `Flowable<T>`                                                | RxJava `Flowable`，使用响应式 repository 发出零、一或许多元素。返回 `Flowable` 的查询也可以发射出无限多的元素。 |

------

[1](https://springdoc.cn/spring-data-mongodb/#_footnoteref_1). Kristina Chodorow. *MongoDB - The Definitive Guide*. O’Reilly Media, 2013

[2](https://springdoc.cn/spring-data-mongodb/#_footnoteref_2). Uses UTC zone offset. Configure via [MongoConverterConfigurationAdapter](https://springdoc.cn/spring-data-mongodb/#mapping-configuration)

[3](https://springdoc.cn/spring-data-mongodb/#_footnoteref_3). Uses UTC zone offset. Configure via [MongoConverterConfigurationAdapter](https://springdoc.cn/spring-data-mongodb/#mapping-configuration)

[4](https://springdoc.cn/spring-data-mongodb/#_footnoteref_4). 见 [XML 配置](https://springdoc.cn/spring-data-mongodb/#repositories.create-instances.xml)

Version 4.1.0-SNAPSHOT
Last updated 2023-02-14 20:03:34 +0800

[主页](https://springdoc.cn/docs/)

## 4. 集群

以后再说



