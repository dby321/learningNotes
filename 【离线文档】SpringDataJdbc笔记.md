# Spring Data JDBC 中文文档

version 3.1.0-SNAPSHOT,2023-09-10

- [序言](https://springdoc.cn/spring-data-jdbc/#preface)
- [参考文档](https://springdoc.cn/spring-data-jdbc/#reference)
- [附录](https://springdoc.cn/spring-data-jdbc/#appendix)

© 2018-2023 The original authors.

Spring Data JDBC 是 Spring Data 项目的一部分，它提供了一种简化的数据访问方式，用于与关系型数据库进行交互。与传统的 ORM 框架相比，Spring Data JDBC 更加轻量级，并且更注重直接映射数据库表结构和 SQL 查询的控制。它基于 JDBC 驱动程序，提供了一套简洁的 API 和注解，使开发人员能够通过简单的 Java 对象来表示数据库表，并通过自动生成的 SQL 语句执行常见的 CRUD 操作。Spring Data JDBC 还支持复杂的关联关系、查询方法的定义和自定义 SQL 查询等功能，使开发人员能够更灵活地操作数据库。

|      | 本站([springdoc.cn](https://springdoc.cn/))中的内容来源于 [spring.io](https://spring.io/) ，原始版权归属于 [spring.io](https://spring.io/)。由 [springdoc.cn](https://springdoc.cn/) 进行翻译，整理。可供个人学习、研究，未经许可，不得进行任何转载、商用或与之相关的行为。 商标声明：Spring 是 Pivotal Software, Inc. 在美国以及其他国家的商标。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

# 序言

Spring Data JDBC 项目将 Spring 的核心概念应用于开发使用符合 [领域驱动设计原则](https://springdoc.cn/spring-data-jdbc/#jdbc.domain-driven-design)的JDBC数据库的解决方案。我们提供了一个 “template”，作为存储和查询聚合体（aggregate）的高级抽象。

本文档是Spring Data JDBC支持的参考指南。它解释了概念和语义以及语法。

本节提供了一些基本介绍。文件的其余部分只提到Spring Data JDBC的功能，并假设用户熟悉SQL和Spring的概念。

## 1. 学习Spring

ffSpring Data使用Spring框架的 [核心](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html)功能，包括。

- [IoC](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#beans) 容器
- [类型转换系统](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#validation)
- [表达式语言](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#expressions)
- [JMX 整合](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/integration.html#jmx)
- [DAO异常的层次结构](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#dao-exceptions).

虽然你不需要知道Spring的API，但理解它们背后的概念是很重要的。至少，你应该熟悉控制反转（IoC）背后的理念，而且你应该熟悉你选择使用的任何IoC容器。

JDBC Aggregate支持的核心功能可以直接使用，而不需要调用Spring容器的IoC服务。这很像 `JdbcTemplate`，它可以 "独立" 使用，不需要Spring容器的任何其他服务。为了利用Spring Data JDBC的所有功能，例如对 repository 的支持，你需要配置库（library）的某些部分来使用Spring。

要了解更多关于Spring的信息，你可以参考详细解释Spring框架的综合文档。有很多关于这个主题的文章、博客和书籍。更多信息请参见Spring框架 [主页](https://spring.io/docs)。

## 2. 要求

Spring Data JDBC二进制文件需要JDK 8.0及以上版本和 [Spring Framework](https://spring.io/docs) 6.0.4 及以上版本。

在数据库方面，Spring Data JDBC 需要一种 [方言](https://springdoc.cn/spring-data-jdbc/#jdbc.dialects) 来抽象出通用的SQL功能，而不是供应商特定的风格。Spring Data JDBC 包括对以下数据库的直接支持。

- DB2
- H2
- HSQLDB
- MariaDB
- Microsoft SQL Server
- MySQL
- Oracle
- Postgres

如果你使用一个不同的数据库，那么你的应用程序将无法启动。[方言](https://springdoc.cn/spring-data-jdbc/#jdbc.dialects) 部分包含在这种情况下如何进行的进一步细节。

## 3. 额外的帮助资源

学习一个新的框架并不总是简单明了的。在本节中，我们试图提供一个我们认为是简单易行的指南，让大家开始学习Spring Data JDBC模块。然而，如果你遇到问题或需要建议，请随时使用以下链接。

|      | 欢迎在 [Spring Boot 中文社区](https://springboot.io/) 发帖探讨。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

- Stack Overflow

  [Stack Overflow](https://stackoverflow.com/questions/tagged/spring-data) 上的Spring Data是所有Spring Data（不仅仅是Document）用户分享信息和相互帮助的一个 tag。请注意，只有在发帖时才需要注册。

- 专业的支持

  [Pivotal Sofware, Inc.](https://pivotal.io/) 是Spring Data和Spring背后的公司，可以提供专业的、来自源头的支持，并保证响应时间。

## 4. 关注进展

有关Spring Data JDBC源代码库、每晚构建和快照工件的信息，请参阅Spring Data JDBC [主页](https://spring.io/projects/spring-data-jdbc/)。您可以通过 [Stack Overflow](https://stackoverflow.com/questions/tagged/spring-data) 上的社区与开发人员互动，帮助Spring Data最好地满足Spring社区的需求。如果你遇到了一个错误或想提出改进建议，请在 [Spring Data issue tracker](https://github.com/spring-projects/spring-data-jdbc/issues)上创建一个ticket。要想了解Spring生态中的最新新闻和公告，请订阅Spring社区 [门户](https://spring.io/)。你也可以在Twitter（ [SpringData](https://twitter.com/SpringData)）上关注Spring [博客](https://spring.io/blog) 或项目团队。

## 5. 项目元数据

- Release repository: https://repo.spring.io/libs-release
- Milestone repository: https://repo.spring.io/libs-milestone
- Snapshot repository: https://repo.spring.io/libs-snapshot

## 6. Spring Data 升级

关于如何从Spring Data的早期版本升级的说明在项目 [wiki](https://github.com/spring-projects/spring-data-commons/wiki)上提供。按照 [发布说明部分](https://github.com/spring-projects/spring-data-commons/wiki#release-notes) 的链接，找到你要升级的版本。

升级说明总是在发行说明中的第一项。如果你落后一个以上的版本，请确保你也查看你跳过的版本的发行说明。

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

Spring Boot会为你选择一个最新版本的Spring Data模块。如果你仍然想升级到较新的版本，将 `spring-data-releasetrain.version` 属性设置为你想使用的[train version 和 iteration](https://springdoc.cn/spring-data-jdbc/#dependencies.train-version)。

### 7.2. Spring Framework

当前版本的Spring Data模块需要Spring框架 6.0.4 或更高的版本。这些模块也可能在该次要版本的较早的错误修复版本中工作。但是，强烈建议使用那一代中的最新版本。

## 8. 与 Spring Data Repository 一起工作

Spring Data Repository 抽象的目标是大大减少为各种持久性store实现数据访问层所需的模板代码量。

|      | *Spring Data Repository 文档和你的模块*本章解释了Spring Data Repository 的核心概念和接口。本章的信息是从Spring Data Commons模块中提取的。它使用了Jakarta Persistence API（JPA）模块的配置和代码样本。 “[Repository query 关键字](https://springdoc.cn/spring-data-jdbc/#repository-query-keywords)” 涵盖了 Repository 抽象所支持的一般的查询方法关键字。关于你的模块的具体功能的详细信息，请参阅本文档中关于该模块的章节。 |
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

3. 设置Spring为这些接口创建代理实例，可以用[JavaConfig](https://springdoc.cn/spring-data-jdbc/#repositories.create-instances.java-config)或[XML 配置](https://springdoc.cn/spring-data-jdbc/#repositories.create-instances)。

   Java

   ```java
   @EnableJpaRepositories
   class Config { … }
   ```

   \+ 请注意，JavaConfig 并没有明确地配置 `package`，因为被注解的类的 `package` 是默认使用的。要自定义要扫描的包，请使用数据store特定库的 `@EnableJpaRepositories` 注解的 `basePackage…` 属性之一。

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

- [定义 Repository 接口](https://springdoc.cn/spring-data-jdbc/#repositories.definition)
- [定义 Query 方法](https://springdoc.cn/spring-data-jdbc/#repositories.query-methods.details)
- [创建 Repository 实例](https://springdoc.cn/spring-data-jdbc/#repositories.create-instances)
- [自定义 Spring Data Repository 的实现](https://springdoc.cn/spring-data-jdbc/#repositories.custom-implementations)

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

1. 如果 repository 定义 [继承了特定模块的 repository](https://springdoc.cn/spring-data-jdbc/#repositories.multiple-modules.types)，那么它就是特定Spring Data模块的有效候选 repository。
2. 如果 domain 类被注解了 [模块特定的类型注解](https://springdoc.cn/spring-data-jdbc/#repositories.multiple-modules.annotations)，它就是特定Spring Data模块的有效候选者。Spring Data模块接受第三方注解（如JPA的 `@Entity`）或提供自己的注解（如Spring Data MongoDB和Spring Data Elasticsearch的 `@Document`）。

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

[Repository 类型细节](https://springdoc.cn/spring-data-jdbc/#repositories.multiple-modules.types)和[区分domain类注解](https://springdoc.cn/spring-data-jdbc/#repositories.multiple-modules.annotations)用于严格的repository库配置，以确定特定Spring Data模块的repository候选者。在同一domain类型上使用多个持久化技术的特定注解是可能的，并且能够在多个持久化技术中重复使用domain类型。然而，Spring Data就不能再确定一个唯一的模块来绑定repository了。

区分 repository 的最后一个方法是通过对 repository base package的扫描。base package 定义了扫描 repository 接口定义的起点，这意味着将 repository 的定义放在适当的包中。默认情况下，注解驱动的配置使用配置类所在的base package。基于[XML的配置中的base package](https://springdoc.cn/spring-data-jdbc/#repositories.create-instances.xml)，需要手动强制配置。

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

下列策略可用于 repository 基础设施解析查询。 对于 Java 配置，你可以使用 `EnableJpaRepositories` 注解的 `queryLookupStrategy` 属性。有些策略可能不支持特定的datastore。

- `CREATE` 试图从查询方法名称中构建一个特定的存储查询。一般的做法是从方法名中删除一组已知的前缀，然后解析方法的其余部分。你可以在 “[Query 创建](https://springdoc.cn/spring-data-jdbc/#repositories.query-methods.query-creation)” 中阅读更多关于查询构建的信息。 `USE_DECLARED_QUERY` 试图找到一个已声明的查询，如果找不到就会抛出一个异常。查询可以由某处的注解来定义，也可以通过其他方式来声明。请参阅特定store的文档以找到该store的可用选项。如果 repository 基础设施在启动时没有为该方法找到一个已声明的查询，则会失败。
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

解析查询方法名称分为主语和谓语。第一部分（`find…By`, `exists…By`）定义了查询的主语，第二部分形成谓语。引入句（主语）可以包含进一步的表达。在 `find`（或其他引入关键词）和 `By` 之间的任何文本都被认为是描述性的，除非使用一个限制结果的关键词，如 `Distinct` 在要创建的查询上设置一个不同的标志，或 [`Top` / `First` 来限制查询结果](https://springdoc.cn/spring-data-jdbc/#repositories.limit-query-result)。

附录中包含了 [查询方法主语关键词](https://springdoc.cn/spring-data-jdbc/#appendix.query.method.subject) 和 [查询方法谓语关键词的完整列表](https://springdoc.cn/spring-data-jdbc/#appendix.query.method.predicate)，包括排序和字母修饰语。然而，第一个 `By` 作为分界符，表示实际条件谓词的开始。在一个非常基本的层面上，你可以在实体属性上定义条件，并用 `And` 和 `Or` 来连接它们。

解析方法的实际结果取决于你为之创建查询的持久性store。然而，有一些东西需要注意。

- 表达式通常是属性遍历与可以串联的运算符的组合。你可以用 `AND` 和 `OR` 来组合属性表达式。你还可以得到对属性表达式的运算符的支持，如 `Between`, `LessThan`, `GreaterThan`, 和 `Like`。支持的运算符可能因 datastore 的不同而不同，所以请查阅参考文档的适当部分。
- 方法解析器支持为单个属性（例如，`findByLastnameIgnoreCase(…)`）或支持忽略大小写的类型的所有属性（通常是字符串实例—例如，`findByLastnameAndFirstnameAllIgnoreCase`(…)）设置忽略大小写标志。是否支持忽略大小写可能因store而异，所以请查阅参考文档中的相关章节，了解特定store的查询方法。
- 你可以通过在引用属性的查询方法中附加一个 `OrderBy` 子句，并提供一个排序方向（`Asc` 或 `Desc`）来应用静态排序。要创建一个支持动态排序的查询方法，请参阅 “[特殊参数处理](https://springdoc.cn/spring-data-jdbc/#repositories.special-parameters)”。

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

返回多个结果的查询方法可以使用标准的Java `Iterable`、`List` 和 `Set`。除此之外，我们还支持返回Spring Data的 `Streamable`，这是 `Iterable` 的一个自定义扩展，以及 [Vavr](https://www.vavr.io/) 提供的 collection 类型。请参考附录中对所有可能的 [查询方法返回类型](https://springdoc.cn/spring-data-jdbc/#appendix.query.return.types)的解释。

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

另外，查询方法可以选择完全不使用 wrapper 类型。没有查询结果的话会通过返回 `null` 来表示。Repository 方法返回集合、集合替代物、wrapper和流时，保证不会返回 `null`，而是返回相应的空（Empty）表示。详见 “[Repository 查询返回类型](https://springdoc.cn/spring-data-jdbc/#repository-query-return-types)”。

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

在Java配置类上使用store特有的 `@EnableJpaRepositories` 注解来定义 repository 激活的配置。关于基于Java的Spring容器配置的介绍，请参见 [Spring参考文档中的JavaConfig](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#beans-java)。

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

#### 8.5.2. 使用 Filter

默认情况下，基础架构会抓取每个扩展了位于配置的 base package 下的持久化技术特定的 `Repository` 子接口的接口，并为其创建一个Bean实例。然而，你可能想要更精细地控制哪些接口为其创建Bean实例。要做到这一点，可以在 Repository 声明中使用 filter 元素。其语义与Spring的组件过滤器中的元素完全等同。详情请见 [Spring参考文档](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#beans-scanning-filters) 中的这些元素。

例如，为了排除某些接口作为 Repository Bean 的实例化，你可以使用以下配置。

Example 26. 使用 Filter

Java

```java
@Configuration
@EnableJpaRepositories(basePackages = "com.acme.repositories",
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

#### 8.5.3. 单独使用

你也可以在Spring容器之外使用资源库基础设施—例如，在CDI环境中。你仍然需要在你的classpath中使用一些Spring库，但是，一般来说，你也可以通过编程来设置 Repository。Repository 支持的Spring Data模块都有一个特定于持久化技术的 `RepositoryFactory`，你可以使用，如下所示。

Example 27. repository factory 的独立使用

```java
RepositoryFactorySupport factory = … // Instantiate factory here
UserRepository repository = factory.getRepository(UserRepository.class);
```

### 8.6. 自定义 Spring Data Repository 的实现

Spring Data提供了各种选项来创建查询方法，只需少量编码。但当这些选项不符合你的需求时，你也可以为 repository 方法提供你自己的自定义实现。本节介绍了如何做到这一点。

#### 8.6.1. 自定义个别 Repository

要用自定义的功能来丰富 repository，你必须首先定义一个片段（fragment）接口和自定义功能的实现，如下所示。

Example 28. 定制 repository 功能的接口

```java
interface CustomizedUserRepository {
  void someCustomMethod(User user);
}
```

Example 29. 实现自定义 repository 的功能

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

Example 30. 改变你的 repository 接口

```java
interface UserRepository extends CrudRepository<User, Long>, CustomizedUserRepository {

  // Declare query methods here
}
```

用你的存储库接口继承片段接口，结合了CRUD和自定义功能，并使其对客户端可用。

Spring Data Repository 是通过使用形成 repository 组合的片段来实现的。片段是基础repository、功能方面（如 [QueryDsl](https://springdoc.cn/spring-data-jdbc/#core.extensions.querydsl)），以及自定义接口和它们的实现。每当你为你的repository接口添加一个接口，你就通过添加一个片段来增强组合。基础repository和repository方面的实现是由每个Spring Data模块提供的。

下面的例子显示了自定义接口和它们的实现。

Example 31. 片段及其实现

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

Example 32. 修改你的 repository 接口

```java
interface UserRepository extends CrudRepository<User, Long>, HumanRepository, ContactRepository {

  // Declare query methods here
}
```

Repository可以由多个自定义实现组成，这些自定义实现按其声明的顺序被导入。自定义实现的优先级高于基础实现和Repository 切面。这种排序可以让你覆盖基础Repository和切面的方法，并在两个片段贡献了相同的方法签名时解决歧义。Repository片段不限于在单一存储库接口中使用。多个Repository可以使用一个片段接口，让你在不同的 Repository 中重复使用自定义的内容。

下面的例子显示了一个 Repository 片段及其实现。

Example 33. Fragments overriding `save(…)`

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

Example 34. 自定义 repository 接口

```java
interface UserRepository extends CrudRepository<User, Long>, CustomizedSave<User> {
}

interface PersonRepository extends CrudRepository<Person, Long>, CustomizedSave<Person> {
}
```

##### 配置

repository基础设施试图通过扫描发现repository的包下面的类来自动检测自定义实现片段。这些类需要遵循后缀默认为 `Impl` 的命名惯例。

下面的例子显示了一个使用默认后缀的 repository 和一个为后缀设置了自定义值的 repository。

Example 35. 配置示例

Java

```java
@EnableJpaRepositories(repositoryImplementationPostfix = "MyPostfix")
class Configuration { … }
```

前面例子中的第一个配置试图查找一个叫做 `com.acme.repository.CustomizedUserRepositoryImpl` 的类，作为一个自定义的 repository 实现。第二个例子试图查找 `com.acme.repository.CustomizedUserRepositoryMyPostfix`。

###### 消除歧义

如果在不同的包中发现了具有匹配类名的多个实现，Spring Data会使用Bean名称来确定使用哪一个。

考虑到前面显示的 `CustomizedUserRepository` 的以下两个自定义实现，第一个实现被使用。它的Bean名是 `customedUserRepositoryImpl`，与片段接口（`CustomizedUserRepository`）加后缀 `Impl` 的名字一致。

Example 36. 解决模棱两可的实现

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

如果你的自定义实现只使用基于注解的配置和自动注入，前面所示的方法很好用，因为它被当作任何其他Spring Bean。如果你的实现片段Bean需要特殊的注入，你可以根据[前文](https://springdoc.cn/spring-data-jdbc/#repositories.single-repository-behaviour.ambiguity)所述的约定来声明Bean并为其命名。然后，基础设施通过名称来引用手动定义的Bean定义，而不是自己创建一个。下面的例子展示了如何手动注入一个自定义的实现。

Example 37. 手动注入一个自定义实现

Java

```java
class MyClass {
  MyClass(@Qualifier("userRepositoryImpl") UserRepository userRepository) {
    …
  }
}
```

#### 8.6.2. 自定义 Base Repository

当你想定制基础 repository 的行为时，[上一节](https://springdoc.cn/spring-data-jdbc/#repositories.manual-wiring)描述的方法需要定制每个 repository 的接口，以便所有的 repository 都受到影响。为了改变所有 repository 的行为，你可以创建一个扩展持久化技术特定 repository 基类的实现。然后这个类作为 repository 代理的自定义基类，如下面的例子所示。

Example 38. 自定义 repository base 类

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

Example 39. 配置一个自定义 repository base 类

Java

```java
@Configuration
@EnableJpaRepositories(repositoryBaseClass = MyRepositoryImpl.class)
class ApplicationConfiguration { … }
```

### 8.7. 从 Aggregate Root （聚合ROOT）中发布事件

由 Repository 管理的实体是 aggregate root。在领域驱动设计应用程序中，这些aggregate root通常会发布 domain 事件。Spring Data提供了一个名为 `@DomainEvents` 的注解，你可以在 aggregate root 的一个方法上使用该注解，以使这种发布尽可能地简单，如下例所示。

Example 40. 从aggregate root中暴露domain 事件

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

Example 41. QuerydslPredicateExecutor interface

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

Example 42. Repository 上的 Querydsl 整合

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

Example 43. 启用 Spring Data web 支持

Java

```java
@Configuration
@EnableWebMvc
@EnableSpringDataWebSupport
class WebConfiguration {}
```

XML

```xml
<bean class="org.springframework.data.web.config.SpringDataWebConfiguration" />

<!-- If you use Spring HATEOAS, register this one *instead* of the former -->
<bean class="org.springframework.data.web.config.HateoasAwareSpringDataWebConfiguration" />
```

`@EnableSpringDataWebSupport` 注解注册了一些组件。我们将在本节后面讨论这些组件。它还会检测classpath上的Spring HATEOAS，并为其注册整合组件（如果存在）。

##### 基本的 Web 支持

在XML中启用Spring Data Web支持

[上一节](https://springdoc.cn/spring-data-jdbc/#core.web)所示的配置注册了一些基本组件。

- [使用 `DomainClassConverter` 类](https://springdoc.cn/spring-data-jdbc/#core.web.basic.domain-class-converter) 让Spring MVC从请求参数或路径变量中解析 Repository 管理的 domain 类实例。
- [`HandlerMethodArgumentResolver`](https://springdoc.cn/spring-data-jdbc/#core.web.basic.paging-and-sorting) 的实现，让Spring MVC从请求参数中解析 `Pageable` 和 `Sort` 实例。
- [Jackson模块](https://springdoc.cn/spring-data-jdbc/#core.web.basic.jackson-mappers) 对 `Point` 和 `Distance` 等类型进行序列化/反序列化，或存储特定的类型，这取决于使用的Spring数据模块。

###### 使用 `DomainClassConverter` 类

`DomainClassConverter` 类让你在Spring MVC Controller 方法签名中直接使用 domain 类型，这样你就不需要通过 repository 手动查找实例了，如下例所示。

Example 44. 一个在方法签名中使用 domain 类型的Spring MVC controller

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

[上一节](https://springdoc.cn/spring-data-jdbc/#core.web.basic.domain-class-converter) 中的配置片段还注册了一个 `PageableHandlerMethodArgumentResolver` 以及一个 `SortHandlerMethodArgumentResolver` 的实例。注册后，`Pageable` 和 `Sort` 可以作为有效的controller方法参数，如下图所示。

Example 45. 使用 Pageable 作为 controller 方法参数

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

Example 46. 使用 PagedResourcesAssembler 作为 controller 方法参数

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

核心模块和一些特定的存储模块与一组Jackson模块一起发布，用于Spring Data domain 域使用的类型，如 `org.springframework.data.geo.Distance` 和 `org.springframework.data.geo.Point`。 一旦启用 [web支持](https://springdoc.cn/spring-data-jdbc/#core.web) 和 `com.fasterxml.jackson.databind.ObjectMapper` 可用，就会导入这些模块。

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

你可以通过使用 [JSONPath](https://goessner.net/articles/JsonPath/) 表达式（需要 [Jayway JsonPath](https://github.com/json-path/JsonPath)）或 [XPath](https://www.w3.org/TR/xpath-31/) 表达式（需要 [XmlBeam](https://xmlbeam.org/)）来使用 Spring Data 投影（在 [投影](https://springdoc.cn/spring-data-jdbc/#projections) 中描述）来绑定传入的请求的payload，如下例所示。

Example 47. 使用JSONPath 或 XPath 表达式来绑定HTTP payload

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

如 [投影](https://springdoc.cn/spring-data-jdbc/#projections) 中所述，支持嵌套投影。如果该方法返回一个复杂的、非接口类型，则使用Jackson `ObjectMapper` 来映射最终值。

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

# 参考文档

## 9. JDBC Repositroy

本章介绍了JDBC Repository 支持的特性。这建立在[与 Spring Data Repository 一起工作](https://springdoc.cn/spring-data-jdbc/#repositories)中解释的核心 repository 支持之上。你应该对那里解释的基本概念有一个正确的理解。

### 9.1. 为什么选择 Spring Data JDBC?

在Java世界中，关系型数据库的主要持久化API当然是JPA，它有自己的Spring Data模块。为什么会有另一个呢？

JPA做了很多事情，以帮助开发者。在其他方面，它跟踪实体的变化。它为你做延迟加载。它让你将大量的对象结构映射到同样广泛的数据库设计中。

这很好，使很多事情变得非常容易。只要看一下基本的JPA教程就可以了。但是，对于JPA为什么要做某件事情，往往会变得非常混乱。另外，概念上非常简单的事情在JPA中却变得相当困难。

Spring Data JDBC的目标是在概念上更加简单，它采用了以下设计决策。

- 如果你加载一个实体，SQL语句就会被运行。一旦这样做了，你就有了一个完全加载的实体。没有进行延迟加载或缓存。
- 如果你保存一个实体，它就会被保存。如果你不这样做，它就不会。没有dirty tracking，也没有session。
- 有一个如何将实体映射到表的简单模型。它可能只适用于相当简单的情况。如果你不喜欢这样，你应该编写你自己的策略。Spring Data JDBC只提供了非常有限的支持，即用注解来定制策略。

### 9.2. 领域驱动设计和关系型数据库

所有的Spring Data模块都受到领域驱动设计中 “repository”、“aggregate” 和 “aggregate root” 等概念的启发。这些对于 Spring Data JDBC 来说可能更加重要，因为它们在某种程度上与使用关系型数据库时的正常做法相反。

aggregate 是一组实体，它被保证在对其进行原子性修改时是一致的。一个典型的例子是一个带有 `OrderItems` 的 `Order`。`Order` 上的一个属性（例如，`numberOfItems` 与 `OrderItems` 的实际数量一致）在变化时保持一致。

aggregate 之间的引用并不保证在任何时候都是一致的。它们保证最终会变得一致。

每个 aggregate 都有一个 aggregate root，它是 aggregate 的一个实体。aggregate 只通过 aggregate root 的方法被操纵。这些都是前面提到的原子性变化。

repository 是持久化存储（persistent store）的一个抽象，它看起来像某一类型的所有aggregate的集合。对于Spring Data来说，这意味着你希望每个aggregate root有一个 `Repository`。此外，对于 Spring Data JDBC 来说，这意味着所有可以从aggregate root到达的实体都被认为是该aggregate root的一部分。Spring Data JDBC假定只有aggregate对存储aggregate体的非 root 实体的表有一个外键，并且没有其他实体指向非 root 实体。

|      | 在目前的实现中，从 aggregate root 引用的实体被Spring Data JDBC删除和重新创建。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

你可以用符合你工作和设计数据库风格的实现来覆写 repository 的方法。

### 9.3. 入门

建立工作环境的一个简单方法是在 [STS](https://spring.io/tools/sts) 中或从 [Spring Initializr](https://start.springboot.io/) 中创建一个基于Spring的项目。

首先，你需要设置一个正在运行的数据库服务器。关于如何配置你的数据库以实现JDBC访问，请参考你的供应商文档。

在STS中创建一个Spring项目。

1. 进入 File → New → Spring Template Project → Simple Spring Utility Project，并在出现提示时按 "Yes"。然后输入一个项目和包的名称，如 `org.spring.jdbc.example`。

2. 在 `pom.xml` 文件的 `dependencies` 元素中添加以下内容。

   ```xml
   <dependencies>
   
     <!-- other dependency elements omitted -->
   
     <dependency>
       <groupId>org.springframework.data</groupId>
       <artifactId>spring-data-jdbc</artifactId>
       <version>3.1.0-SNAPSHOT</version>
     </dependency>
   
   </dependencies>
   ```

3. 将 pom.xml 中 Sprin g的 version 改为

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

repository 也可 [在此浏览](https://repo.spring.io/milestone/org/springframework/data/)。

### 9.4. 示例库

有一个 [GitHub仓库](https://github.com/spring-projects/spring-data-examples)，里面有几个例子，你可以下载并玩一玩，感受一下这个库是如何工作的。

### 9.5. 基于注解的配置

Spring Data JDBC Repository 可以通过Java配置的注解来激活，如下例所示。

Example 48. Spring Data JDBC repositories using Java configuration

```java
@Configuration
@EnableJdbcRepositories                                                                
class ApplicationConfig extends AbstractJdbcConfiguration {                            

    @Bean
    DataSource dataSource() {                                                         

        EmbeddedDatabaseBuilder builder = new EmbeddedDatabaseBuilder();
        return builder.setType(EmbeddedDatabaseType.HSQL).build();
    }

    @Bean
    NamedParameterJdbcOperations namedParameterJdbcOperations(DataSource dataSource) { 
        return new NamedParameterJdbcTemplate(dataSource);
    }

    @Bean
    TransactionManager transactionManager(DataSource dataSource) {                     
        return new DataSourceTransactionManager(dataSource);
    }
}
```

|      | `@EnableJdbcRepositories` 为源自 `Repository` 的接口创建实现。 |
| ---- | ------------------------------------------------------------ |
|      | `AbstractJdbcConfiguration` 提供了Spring Data JDBC所需的各种默认Bean类。 |
|      | 创建一个连接到 `DataSource` 的数据源。以下两个Bean方法需要这样做。 |
|      | 创建 Spring Data JDBC 用来访问数据库的 `NamedParameterJdbcOperations`。 |
|      | Spring Data JDBC利用了Spring JDBC提供的事务管理（transaction management ）。 |

前面例子中的配置类通过使用 `spring-jdbc` 的 `EmbeddedDatabaseBuilder` API设置了一个嵌入式HSQL数据库。然后使用 `DataSource` 来设置 `NamedParameterJdbcOperations` 和 `TransactionManager`。我们最后通过使用 `@EnableJdbcRepositories` 来激活 Spring Data JDBC Repository。如果没有配置 base package，它就使用配置类所在的包。继承 `AbstractJdbcConfiguration` 确保各种Bean被注册。覆盖其方法可以用来定制设置（见下文）。

通过使用Spring Boot，这种配置可以进一步简化。在Spring Boot中，只要在依赖中添加 `spring-boot-starter-data-jdbc` starter，有一个 `DataSource`。其他一切都由Spring Boot完成。

在这个设置中，有几件事人们可能想要定制。

#### 9.5.1. 方言（Dialect）

Spring Data JDBC 使用 `Dialect` 接口的实现来封装特定于数据库或其JDBC驱动程序的行为。默认情况下，`AbstractJdbcConfiguration` 会尝试确定正在使用的数据库并注册正确的 `Dialect`。这个行为可以通过覆写 `jdbcDialect(NamedParameterJdbcOperations)` 来改变。

如果你使用一个没有方言（Dialect）的数据库，那么你的应用程序将无法启动。在这种情况下，你将不得不要求你的供应商提供一个方言实现。另外，你也可以：

1. 实现你自己的 `Dialect`。
2. 实现 `JdbcDialectProvider`，返回 `Dialect`。
3. 通过在 `META-INF` 下创建 `spring.factories` 资源来注册提供者，并通过添加一行来执行注册 `org.springframework.data.jdbc.repository.config.DialectResolver$JdbcDialectProvider=<JdbcDialectProvider 的全路径名称>`。

### 9.6. 持久化实体

保存一个 aggregate 可以用 `CrudRepository.save(…)` 方法进行。如果 aggregate 是新的，这将导致对 aggregate root 的插入，然后是对所有直接或间接引用的实体的插入语句。

如果 aggregate root 不是新的，所有被引用的实体会被删除，aggregate root 会被更新，所有被引用的实体会被重新插入。请注意，一个实例是否是新的是该实例状态的一部分。

|      | 这种方法有一些明显的弊端。如果只有少数被引用的实体被实际改变了，那么删除和插入就是浪费了。虽然这个过程可以而且可能会被改进，但Spring Data JDBC能提供的东西有一定的局限性。它不知道一个 aggregate 的先前状态。因此，任何更新过程都必须采取它在数据库中发现的任何东西，并确保它将其转换为传递给 save 方法的实体的任何状态。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 9.6.1. 对象映射的基础知识

本节涵盖了Spring Data对象映射、对象创建、字段和属性访问、可变性和不可变性的基本原理。注意，本节只适用于不使用底层数据存储的对象映射的Spring Data模块（如JPA）。此外，请务必查阅特定于存储的对象映射部分，如索引、自定义列或字段名或类似内容。

Spring Data对象映射的核心职责是创建domain对象的实例，并将存储的本地数据结构映射到这些对象上。这意味着我们需要两个基本步骤。

1. 通过使用暴露的构造函数之一来创建实例。
2. Instance population to materialize all exposed properties.

##### Object 创建

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

##### 属性填充

一旦实体的实例被创建，Spring Data就会填充该类的所有剩余持久化属性。除非已经由实体的构造函数填充（即通过其构造函数参数列表设置），否则 identifier 属性将首先被填充，以允许解决循环对象引用。之后，所有尚未被构造函数填充的非瞬时（non-transient）属性都被设置在实体实例上。为此，我们使用以下算法。

1. 如果该属性是不可变的，但暴露了一个 `with…` 方法（见下文），我们使用 `with…` 方法来创建一个具有新属性值的新实体实例。
2. 如果定义了属性 access（即通过getter和setter访问），我们就调用setter方法。
3. 如果该属性是可变的，我们直接设置该字段。
4. 如果属性是不可变的，我们就使用持久化操作（见 [Object 创建](https://springdoc.cn/spring-data-jdbc/#mapping.object-creation)）所使用的构造函数来创建一个实例的副本。
5. 默认情况下，我们直接设置字段的值。

属性填充的内部细节

与我们在[对象构造中的优化](https://springdoc.cn/spring-data-jdbc/#mapping.object-creation.details)类似，我们也使用Spring Data 运行时生成的 accessor 类来与实体实例进行交互。

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

Example 49. A generated Property Accessor

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

Example 50. A sample entity

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

##### 一般建议

- *尽量坚持使用不可变的对象* --不可变的对象创建起来很简单，因为具体化一个对象只需要调用其构造函数即可。同时，这也避免了你的domain对象充满了允许客户端代码操纵对象状态的 setter 方法。如果你需要这些，最好使它们受到 `package` 的保护，这样它们就只能被有限的共存类型所调用。纯构造函数实例化属性比填充快30%。
- *提供一个全参数构造函数* — 即使你不能或不想将你的实体建模为不可变的值，提供一个将实体的所有属性作为参数的构造函数仍有价值，包括可变的属性，因为这允许对象映射跳过属性填充以获得最佳性能。
- *使用工厂方法而不是重载构造函数，以避免 `@PersistenceCreator`* — 由于需要全参数构造函数以获得最佳性能，我们通常希望暴露更多的应用用例特定的构造函数，省略自动生成的ID等东西。宁愿使用静态工厂方法来暴露这些全参数构造函数的变体，这是一种既定的模式。
- *确保你遵守允许生成的实例化器（instantiator）和属性访问器（accessor）类被使用的约束。* — 
- *对于要生成的ID，仍然使用 final 字段与全参数持久化构造函数（首选）或 `with…` 方法相结合* — 
- *使用Lombok来避免模板代码* --由于持久化操作通常需要一个接受所有参数的构造器，它们的声明变成了繁琐的重复的模板参数到字段的分配，通过使用 Lombok 的 `@AllArgsConstructor` 可以最好地避免。

###### 属性覆盖

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

##### 对 Kotlin 的支持

Spring Data 适应了 Kotlin 的具体特性，允许对象的创建和变异（mutation）。

###### Kotlin object 创建

Kotlin类支持实例化，所有的类默认是不可变的，需要明确的属性声明来定义可变的属性。

Spring Data会自动尝试检测一个持久化实体的构造函数，以用于将该类型的对象具体化。 该解析算法的工作原理如下。

1. 如果有一个构造函数被注解为 `@PersistenceCreator`，它将被使用。
2. 如果类型是 [Kotlin data cass](https://springdoc.cn/spring-data-jdbc/#mapping.kotlin)，则使用 primary 构造函数。
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

###### Kotlin data class 的属性填充

在Kotlin中，所有的类默认都是不可变的，需要明确的属性声明来定义可变属性。考虑下面这个 `data` class `Person`。

```kotlin
data class Person(val id: String, val name: String)
```

这个类实际上是不可变的。它允许创建新的实例，因为 Kotlin 生成了一个 `copy(…)` 方法，该方法创建了新的对象实例，复制了现有对象的所有属性值并应用了作为参数提供给该方法的属性值。

###### Kotlin 属性覆盖

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

#### 9.6.2. 实体中支持的类型

目前支持以下类型的属性。

- 所有基本数据类型和它们的包装类型（`int`, `float`, `Integer`, `Float`, 等等）。
- 枚举会被映射到它们的名字（name）上。
- `String`。
- `java.util.Date`， `java.time.LocalDate`， `java.time.LocalDateTime` 和 `java.time.LocalTime`。
- 如果你的数据库支持，上述类型的数组（Array）和集合（Collection）可以被映射到数组类型的列中。
- 任何你的数据库驱动程序接受的东西。
- 对其他实体的引用。它们被认为是一对一的关系，或者说是一种嵌入的类型。对于一对一关系的实体来说，拥有一个 `id` 属性是可选的。被引用实体的表应该有一个额外的列，其名称是基于引用实体的，请看 [反向引用（Back References）](https://springdoc.cn/spring-data-jdbc/#jdbc.entity-persistence.types.backrefs)。嵌入的实体不需要一个 `id`。如果有的话，它会被忽略。
- `Set<some entity>` 被认为是一种一对多的关系。被引用实体的表应该有一个额外的列，其名称基于引用实体的名称，见 [反向引用（Back References）](https://springdoc.cn/spring-data-jdbc/#jdbc.entity-persistence.types.backrefs)。
- `Map<simple type, some entity>` 被认为是一个合格的一对多的关系。被引用实体的表应该有两个额外的列。一个是基于外键的引用实体而命名的（见 [反向引用（Back References）](https://springdoc.cn/spring-data-jdbc/#jdbc.entity-persistence.types.backrefs)），一个是具有相同名称和额外 `_key` 后缀的映射键。你可以通过实现 `NamingStrategy.getReverseColumnName(PersistentPropertyPathExtension path)` 和 `NamingStrategy.getKeyColumn(RelationalPersistentProperty property)` 分别改变这种行为。或者你可以用 `@MappedCollection(idColumn="your_column_name", keyColumn="your_key_column_name")` 来注解该属性。
- `List<some entity>` 被映射为 `Map<Integer, some entity>`。

##### 被引用的实体

对被引用实体的处理是有限的。这是以上面描述的 aggregate root 的概念为基础的。如果你引用另一个实体，根据定义，该实体是你的 aggregate 的一部分。所以，如果你删除了引用，之前被引用的实体就会被删除。这也意味着引用是 1-1 或 1-n，但不是 n-1 或 n-m。

如果你有 n-1 或 n-m 引用，根据定义，你是在处理两个独立的aggregate。这些之间的引用可以被编码为简单的 `id` 值，这与 Spring Data JDBC 正确映射。一个更好的编码方式是让它们成为 `AggregateReference` 的实例。`AggregateReference` 是一个围绕 `id` 值的封装，它将该值标记为对不同 aggregate 的引用。另外，该 aggregate 的类型被编码在泛型参数中。

##### 反向引用（Back References）

aggregate 的所有引用都会在数据库中产生一个相反方向的外键关系。默认情况下，外键列的名称是引用实体的表名。

或者你可以选择让它们以引用实体的实体名称命名，忽略 `@Table` 注释。你可以通过在 `RelationalMappingContext` 上调用 `setForeignKeyNaming(ForeignKeyNaming.IGNORE_RENAMING)` 来激活这个行为。

对于 `List` 和 `Map` 引用，需要一个额外的列来保存list索引或map key。它以外键列为基础，加上一个额外的 `_KEY` 后缀。

如果你想用完全不同的方式来命名这些反向引用，你可以用符合你需求的方式来实现 `NamingStrategy.getReverseColumnName(PersistentPropertyPathExtension path)`。

Example 51. 声明并设置一个 `AggregateReference`。

```java
class Person {
	@Id long id;
	AggregateReference<Person, Long> bestFriend;
}

// ...

Person p1, p2 = // some initialization

p1.bestFriend = AggregateReference.to(p2.id);
```

- 你注册的适合的类型 。

#### 9.6.3. `NamingStrategy`

当你使用 Spring Data JDBC 提供的 `CrudRepository` 的标准实现时，它们期望有一个特定的表结构。你可以通过在你的 application context 中提供 [`NamingStrategy`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/NamingStrategy.html) 来进行调整。

#### 9.6.4. `自定义表名`

当 `NamingStrategy` 不匹配你的数据库表名时，你可以用 [`@Table`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/Table.html) 注解来定制名称。该注解的 `value` 值提供了自定义表的名称。下面的例子将 `MyEntity` 类映射到数据库中的 `CUSTOM_TABLE_NAME` 表。

```java
@Table("CUSTOM_TABLE_NAME")
class MyEntity {
    @Id
    Integer id;

    String name;
}
```

#### 9.6.5. `自定义列名`

当 `NamingStrategy` 不匹配你的数据库列名时，你可以用 [`@Column`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/Column.html) 注解来定制名称。这个注解的 `value` 值提供了自定义的列名。下面的例子将 `MyEntity` 类的 `name` 属性映射到数据库中的 `CUSTOM_COLUMN_NAME` 列。

```java
class MyEntity {
    @Id
    Integer id;

    @Column("CUSTOM_COLUMN_NAME")
    String name;
}
```

[`@MappedCollection`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/MappedCollection.html) 注解可用于引用类型（一对一的关系）或Set、List和Map（一对多的关系）。注解的 `idColumn` 元素为引用对方表中 `id` 列的外键列提供了一个自定义名称。在下面的例子中，`MySubEntity` 类的对应表有一个 `NAME` 列，而 `MyEntity` id 的 `CUSTOM_MY_ENTITY_ID_COLUMN_NAME` 列是出于关系的原因。

```java
class MyEntity {
    @Id
    Integer id;

    @MappedCollection(idColumn = "CUSTOM_MY_ENTITY_ID_COLUMN_NAME")
    Set<MySubEntity> subEntities;
}

class MySubEntity {
    String name;
}
```

当使用 `List` 和 `Map` 时，你必须为 `List` 中数据集的 position 或 Map 中实体的 key 值设置一个额外的列。这个额外的列名可以通过 [`@MappedCollection`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/MappedCollection.html) 注解的 `keyColumn` 元素来定制。

```java
class MyEntity {
    @Id
    Integer id;

    @MappedCollection(idColumn = "CUSTOM_COLUMN_NAME", keyColumn = "CUSTOM_KEY_COLUMN_NAME")
    List<MySubEntity> name;
}

class MySubEntity {
    String name;
}
```

#### 9.6.6. 嵌入式实体

嵌入式实体用于在你的java数据模型中的 value 对象，即使你的数据库中只有一个表。在下面的例子中，你可以看到 `MyEntity` 是用 `@Embedded` 注解来映射的。这样做的结果是，在数据库中，预期会有一个带有两列 `id` 和 `name`（来自 `EmbeddedEntity` 类）的表 `my_entity`。

然而，如果 `name` 列在结果集中实际上是 `null`，根据 `@Embedded` 的 `onEmpty`，整个属性 `embeddedEntity` 将被设置为 `null`，当所有嵌套的属性都是 `null` 的时候，它就会将对象设置为 `null`。 与这种行为相反，`USE_EMPTY` 试图使用默认的构造函数或接受结果集的可空参数值的构造函数来创建一个新的实例。

Example 52. 嵌入对象的示例代码

```java
class MyEntity {

    @Id
    Integer id;

    @Embedded(onEmpty = USE_NULL) 
    EmbeddedEntity embeddedEntity;
}

class EmbeddedEntity {
    String name;
}
```

|      | 如果 `name` 为 `null`，则将 `embeddedEntity` 设置为null。使用 `USE_EMPTY` 来实例化 `name` 属性可能为 `null` 的 `embeddedEntity。` |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

如果你在一个实体中多次需要一个 value 对象，这可以通过 `@Embedded` 注解的可选的 `prefix` 元素来实现。这个元素代表一个前缀，并为嵌入对象中的每一列名称预置。

|      | 利用 `@Embedded.Nullable` & `@Embedded.Empty` 为 `@Embedded(onEmpty = USE_NULL)` 和 `@Embedded(onEmpty = USE_EMPTY)` 的快捷方式来减少冗长的语言，同时相应地设置 JSR-305 `@javax.annotation.Nonnull`。`class MyEntity {     @Id    Integer id;     @Embedded.Nullable     EmbeddedEntity embeddedEntity; } ``@Embedded(onEmpty = USE_NULL)` 的快捷方式。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

包含一个 `Collection` 或 `Map` 的嵌入式实体将总是被认为是非空的，因为它们至少会包含空的Collection或Map。因此，即使使用 `@Embedded(onEmpty = USE_NULL)`，这样的实体也不会为 `null`。

#### 9.6.7. 实体状态检测策略

下表描述了Spring Data为检测一个实体是否是新的提供的策略。

| `@Id`-属性检查(默认)                  | 默认情况下，Spring Data会检查给定实体的id属性。如果id属性为 `null`，或者在基本类型的情况下为 `0`，那么该实体就被认为是新的。否则，它就被认为不是新的。 |
| ------------------------------------- | ------------------------------------------------------------ |
| `@Version`-属性检查                   | 如果带有 `@Version` 注解的属性存在且为 `null`，或者如果是基本类型的version属性为 `0`，则实体被认为是新的。如果version属性存在但有不同的值，那么该实体就被认为不是新的。如果没有version属性存在，Spring Data就会返回到对id属性的检查。 |
| 实现 `Persistable`                    | 如果一个实体实现了 `Persistable`，Spring Data会将“is new”的检测委托给实体的 `isNew(…)` 方法。详情请参见 [Javadoc](https://docs.spring.io/spring-data/data-commons/docs/current/api/index.html?org/springframework/data/domain/Persistable.html)。*Note: 如果你使用 `AccessType.PROPERTY`，`Persistable` 的属性将被检测并持久化。为了避免这种情况，请使用 `@Transient`。* |
| 提供自定义的 `EntityInformation` 实现 | 你可以通过创建特定模块的 repository 工厂的子类并重写 `getEntityInformation(…)` 方法来定制 repository 基础实现中使用的 `EntityInformation` 抽象。然后，你必须将特定模块 repository 工厂的自定义实现注册为Spring Bean。请注意，这应该是很少需要的。 |

#### 9.6.8. ID 生成

Spring Data JDBC 使用ID来识别实体。实体的 ID 必须用 Spring Data 的 [`@Id`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/annotation/Id.html) 注解来注解。

当你的数据库中的ID列有一个自增列时，在将实体插入数据库后，生成的值会被设置到实体中。

一个重要的约束条件是，在保存一个实体之后，该实体必须不再是新的了。请注意，一个实体是否是新的是实体状态的一部分。对于自动递增列，这种情况会自动发生，因为Spring Data会用ID列的值来设置ID。如果你不使用自动递增列，你可以使用 `BeforeConvert` 监听器，它可以设置实体的ID（在本文后面会提到）。

#### 9.6.9. 只读属性

用 `@ReadOnlyProperty` 注解的属性不会被Spring Data JDBC写入数据库，但它们会在实体被加载时被读取。

Spring Data JDBC在写入实体后不会自动重新加载它。因此，如果你想看到在数据库中为这些列生成的数据，你必须明确地重新加载它。

如果被注解的属性是一个实体或实体的集合，那么它将由单独表中的一个或多个单独的行来表示。Spring Data JDBC将不会对这些行进行任何插入、删除或更新。

#### 9.6.10. 只“插入”属性

用 `@InsertOnlyProperty` 注解的属性将只在插入操作期间由 Spring Data JDBC 写入数据库。对于更新，这些属性将被忽略。

`@InsertOnlyProperty` 仅支持 aggregate root。

#### 9.6.11. 乐观锁

Spring Data JDBC 通过在 aggregate root 上用 [`@Version`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/annotation/Version.html) 注解的数字属性来支持乐观锁。每当Spring Data JDBC保存一个带有这种 version 属性的aggregate 时，就会发生两件事。aggregate root 的更新语句将包含一个where子句，检查存储在数据库中的版本是否真的没有变化。如果不是这样，就会抛出 `OptimisticLockingFailureException`。同时， version 属性在实体和数据库中都会被增加，所以一个并发的动作会注意到这个变化，并抛出一个 `OptimisticLockingFailureException`（如果适用），如上所述。

这个过程也适用于插入新的聚合体，`null` 或 `0` version 表示一个新的实例，之后增加的实例标志着该实例不再是新的，这使得在对象构建过程中产生的 id 相当好，例如使用UUID时。

在删除过程中，版本检查也适用，但不会增加版本。

### 9.7. 查询方法

本节提供了一些关于Spring Data JDBC的实现和使用的具体信息。

你通常在 repository 上触发的大多数数据访问操作都会导致对数据库运行查询。定义这样的查询只需要在 repository 接口上声明一个方法，就像下面的例子所示。

Example 53. PersonRepository with query methods

```java
interface PersonRepository extends PagingAndSortingRepository<Person, String> {

  List<Person> findByFirstname(String firstname);                                   

  List<Person> findByFirstnameOrderByLastname(String firstname, Pageable pageable); 

  Slice<Person> findByLastname(String lastname, Pageable pageable);                 

  Page<Person> findByLastname(String lastname, Pageable pageable);                  

  Person findByFirstnameAndLastname(String firstname, String lastname);             

  Person findFirstByLastname(String lastname);                                      

  @Query("SELECT * FROM person WHERE lastname = :lastname")
  List<Person> findByLastname(String lastname);                                     
  @Query("SELECT * FROM person WHERE lastname = :lastname")
  Stream<Person> streamByLastname(String lastname);                                     

  @Query("SELECT * FROM person WHERE username = :#{ principal?.username }")
  Person findActiveUser();															
}
```

|      | 该方法显示了对所有具有给定 `firstname` 的人的查询。这个查询是通过解析方法名称中可以与 `And` 和 `Or` 相连接的约束条件而得到的。因此，方法名称的结果是一个查询表达式：`SELECT … FROM person WHERE firstname = :firstname`。 |
| ---- | ------------------------------------------------------------ |
|      | 使用 `Pageable` 向数据库传递 offset 和 sort 参数。           |
|      | 返回一个 `Slice<Person>`。选择 `LIMIT+1` 行以确定是否有更多的数据需要消费。不支持自定义 `ResultSetExtractor`。 |
|      | 运行一个分页查询，返回 `Page<Person>`。只选择给定 page 范围内的数据，并有可能进行 count 查询以确定总数。不支持自定义 `ResultSetExtractor`。 |
|      | 为给定的 criteria 寻找一个单一的实体。对于非唯一的结果，抛出 `IncorrectResultSizeDataAccessException` 异常。 |
|      | 与<3>相反，如果查询结果有多个，则只返回第一个。              |
|      | `findByLastname` 方法显示对所有具有给定 `lastname` 的人的查询。 |
|      | `streamByLastname` 方法返回一个 `Stream`，这使得 value 从数据库中返回时就有可能。 |
|      | 你可以使用Spring表达式语言来动态地解析参数。在这个例子中，Spring Security被用来解析当前用户的用户名。 |

下表显示了查询方法所支持的关键字。

| 关键字                               | 示例                                        | 逻辑结果                              |
| :----------------------------------- | :------------------------------------------ | :------------------------------------ |
| `After`                              | `findByBirthdateAfter(Date date)`           | `birthdate > date`                    |
| `GreaterThan`                        | `findByAgeGreaterThan(int age)`             | `age > age`                           |
| `GreaterThanEqual`                   | `findByAgeGreaterThanEqual(int age)`        | `age >= age`                          |
| `Before`                             | `findByBirthdateBefore(Date date)`          | `birthdate < date`                    |
| `LessThan`                           | `findByAgeLessThan(int age)`                | `age < age`                           |
| `LessThanEqual`                      | `findByAgeLessThanEqual(int age)`           | `age <= age`                          |
| `Between`                            | `findByAgeBetween(int from, int to)`        | `age BETWEEN from AND to`             |
| `NotBetween`                         | `findByAgeNotBetween(int from, int to)`     | `age NOT BETWEEN from AND to`         |
| `In`                                 | `findByAgeIn(Collection<Integer> ages)`     | `age IN (age1, age2, ageN)`           |
| `NotIn`                              | `findByAgeNotIn(Collection ages)`           | `age NOT IN (age1, age2, ageN)`       |
| `IsNotNull`, `NotNull`               | `findByFirstnameNotNull()`                  | `firstname IS NOT NULL`               |
| `IsNull`, `Null`                     | `findByFirstnameNull()`                     | `firstname IS NULL`                   |
| `Like`, `StartingWith`, `EndingWith` | `findByFirstnameLike(String name)`          | `firstname LIKE name`                 |
| `NotLike`, `IsNotLike`               | `findByFirstnameNotLike(String name)`       | `firstname NOT LIKE name`             |
| `Containing` on String               | `findByFirstnameContaining(String name)`    | `firstname LIKE '%' + name + '%'`     |
| `NotContaining` on String            | `findByFirstnameNotContaining(String name)` | `firstname NOT LIKE '%' + name + '%'` |
| `(No keyword)`                       | `findByFirstname(String name)`              | `firstname = name`                    |
| `Not`                                | `findByFirstnameNot(String name)`           | `firstname != name`                   |
| `IsTrue`, `True`                     | `findByActiveIsTrue()`                      | `active IS TRUE`                      |
| `IsFalse`, `False`                   | `findByActiveIsFalse()`                     | `active IS FALSE`                     |

|      | 查询的推导仅限于可以在 `WHERE` 子句中使用的属性，而不使用 join。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 9.7.1. Query 查找策略

JDBC模块支持将查询手动定义为 `@Query` 注解中的一个字符串或在 property 文件中的命名查询。

从方法的名称衍生出的查询目前仅限于简单的属性，也就是直接存在于 aggregate root 中的属性。另外，这种方法只支持 select 查询。

#### 9.7.2. 使用 `@Query`

下面的例子显示了如何使用 `@Query` 来声明一个查询方法。

Example 54. Declare a query method by using @Query

```java
interface UserRepository extends CrudRepository<User, Long> {

  @Query("select firstName, lastName from User u where u.emailAddress = :email")
  User findByEmailAddress(@Param("email") String email);
}
```

对于将查询结果转换为实体，默认使用与Spring Data JDBC自己生成的查询相同的 `RowMapper`。你提供的查询必须符合 `RowMapper` 所期望的格式。必须提供在实体的构造函数中使用的所有属性的列。通过 setter、with 或字段访问而被设置的属性的列是可选的。在结果中没有匹配列的属性将不会被设置。该查询用于填充aggregate root、嵌入式实体和一对一的关系，包括原始类型的数组，这些数组被存储和加载为SQL-array-types。对于实体的map、list、collection和数组会产生单独的查询。

|      | Spring完全支持Java 8的基于 `-parameters` 编译参数的名称发现。通过在你的构建中使用这个标志作为 debug 信息的替代，你可以省略命名参数的 `@Param` 注解。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | Spring Data JDBC只支持命名参数。 |
| ---- | -------------------------------- |
|      |                                  |

#### 9.7.3. 命名查询

如果在上一节所述的注解中没有给出查询，Spring Data JDBC将尝试定位一个命名的查询。有两种方法可以确定查询的名称。默认的方法是取查询的domain类，即资源库的aggregate root，取其简称，并附加以 `.` 分隔的方法的名称。另外，`@Query` 注解有一个 `name` 属性，可以用来指定要查找的查询的名称。

命名查询应该在classpath上的 `META-INF/jdbc-named-queries.properties` property 文件中提供。

该文件的位置可以通过给 `@EnableJdbcRepositories.namedQueriesLocation` 设置一个值来改变.

##### 流（Stream）式结果

当你指定Stream作为查询方法的返回类型时，Spring Data JDBC在元素可用时立即返回。当处理大量的数据时，这适合于减少延迟和内存需求。

该stream包含一个到数据库的开放连接。为了避免内存泄漏，最终需要通过关闭stream来关闭该连接。推荐的方法是一个 `try-with-resource` 子句。这也意味着，一旦与数据库的连接被关闭，stream就不能再获得更多的元素，并可能抛出一个异常。

##### 自定义 `RowMapper`

你可以通过使用 `@Query(rowMapperClass = ….)` 或者通过注册一个 `RowMapperMap` Bean并为每个方法的返回类型注册一个 `RowMapper` 来配置使用哪个 `RowMapper`。下面的例子显示了如何注册 `DefaultQueryMappingConfiguration`。

```java
@Bean
QueryMappingConfiguration rowMappers() {
  return new DefaultQueryMappingConfiguration()
    .register(Person.class, new PersonRowMapper())
    .register(Address.class, new AddressRowMapper());
}
```

当决定为一个方法使用哪个 `RowMapper` 时，根据方法的返回类型，要遵循以下步骤。

1. 如果类型是一个基本数据类型，则不使用 `RowMapper`。

   反之，查询被期望返回一个单列单行数据，并且对该值进行返回类型的转换。

2. `QueryMappingConfiguration` 中的实体类被遍历，直到找到一个是相关返回类型的父类或接口。为该类注册的 `RowMapper` 被使用。

   迭代是按照注册的顺序进行的，所以要确保在特定类型之后注册更多的泛型。

如果适用的话，诸如 `Collection` 或 `Optional` 的包装类型会被解除包装。因此，一个 `Optional<Person>` 的返回类型在前面的过程中使用 `Person` 类型。

|      | 通过 `QueryMappingConfiguration`、`@Query(rowMapperClass=…)` 或自定义 `ResultSetExtractor` 使用自定义 `RowMapper`，可以禁用实体回调和生命周期事件，因为如果需要，结果映射可以发出自己的事件/回调。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### modify 查询

你可以通过使用 `@Modifying` 注解将一个查询标记为修改性查询，如下图所示。

```java
@Modifying
@Query("UPDATE DUMMYENTITY SET name = :name WHERE id = :id")
boolean updateName(@Param("id") Long id, @Param("name") String name);
```

你可以指定以下返回类型。

- `void`
- `int` (受影响的行数)
- `boolean`(是否更新成功)

修改的查询是直接针对数据库执行的。没有事件或回调被调用。因此，如果有审计注解的字段在注解的查询中没有被更新，也不会被更新。

### 9.8. Example 查询

#### 9.8.1. 介绍

本章介绍了 "Example 查询" 并解释了如何使用它。

Example 查询（QBE）是一种用户友好的查询技术，接口简单。它允许动态查询创建，不要求你写包含字段名的查询。事实上，"Example 查询" 根本不要求你通过使用store特定的查询语言来编写查询。

#### 9.8.2. 使用方式

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

Example 55. Sample Person object

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

|      | 将属性纳入 Example 查询的标准是基于非 null 的。使用原始类型（`int`, `double`, …）的属性总是被包括在内，除非 [`ExampleMatcher` 忽略了属性路径](https://springdoc.cn/spring-data-jdbc/#query-by-example.matchers)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

例子可以通过使用工厂方法或通过使用 [`ExampleMatcher`](https://springdoc.cn/spring-data-jdbc/#query-by-example.matchers) 来构建。`Example` 是不可改变的。下面的列表显示了一个简单的例子。

Example 56. Simple Example

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

Example 57. `QueryByExampleExecutor`

```java
public interface QueryByExampleExecutor<T> {

  <S extends T> S findOne(Example<S> example);

  <S extends T> Iterable<S> findAll(Example<S> example);

  // … more functionality omitted.
}
```

#### 9.8.3. Example Matcher

示例不限于默认设置。你可以通过使用 `ExampleMatcher` 为字符串匹配、null处理和特定属性设置指定你自己的默认值，如下面的例子所示。

Example 58. Example matcher with customized matching

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

Example 59. 配置 matcher 属性

```java
ExampleMatcher matcher = ExampleMatcher.matching()
  .withMatcher("firstname", endsWith())
  .withMatcher("lastname", startsWith().ignoreCase());
}
```

配置 matcher 选项的另一种方法是使用 `lambda` （在Java 8中引入）。这种方法创建一个回调，要求实现者修改matcher。你不需要返回matcher，因为配置选项被保存在matcher实例中。下面的例子显示了一个使用lambda的matcher。

Example 60. 使用 lambda 配置 matcher option

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

#### 9.8.4. Fluent API

`QueryByExampleExecutor` 还提供了一个方法，我们到目前为止还没有提到。`<S extends T, R> R findBy(Example<S> example, Function<FluentQuery.FetchableFluentQuery<S>, R> queryFunction)` 。和其他方法一样，它执行一个从 `Example` 派生的查询。然而，通过第二个参数，你可以控制该执行的各个方面，否则你无法动态地控制。你可以通过调用第二个参数中的 `FetchableFluentQuery` 的各种方法来做到这一点。 `sortBy` 让你为你的结果指定一个排序。`as` 让你指定你希望结果被转换的类型。`project` 限制了被查询的属性。`first`, `firstValue`, `one`, `oneValue`, `all`, `page`, `stream`, `count`, 和 `exists` 定义了你得到什么样的结果以及当超过预期结果数量时查询的行为方式。

Example 61. 使fluent API来获得可能是许多结果中的最后一个，按lastname排序。

```java
Optional<Person> match = repository.findBy(example,
    q -> q
        .sortBy(Sort.by("lastname").descending())
        .first()
);
```

#### 9.8.5. 运行一个示例

在Spring Data JDBC中，你可以使用 Repository 的 Query by Example，如以下例子所示。

Example 62. Query by Example using a Repository

```java
public interface PersonRepository
        extends CrudRepository<Person, String>,
        QueryByExampleExecutor<Person> { … }

public class PersonService {

  @Autowired PersonRepository personRepository;

  public List<Person> findPeople(Person probe) {
    return personRepository.findAll(Example.of(probe));
  }
}
```

|      | 目前，只有 `SingularAttribute` 属性可用于属性匹配。 |
| ---- | --------------------------------------------------- |
|      |                                                     |

property specifier （属性指定器）接受属性名称（如 `firstname` 和 `lastname`）。你可以通过将属性用点连接起来进行导航（`address.city`）。你还可以用匹配选项和大小写敏感性来调整它。

下表显示了你可以使用的各种 `StringMatcher` 选项以及在一个名为 `firstname` 的字段上使用这些选项的结果。

| Matching                        | Logical result                                |
| :------------------------------ | :-------------------------------------------- |
| `DEFAULT` (case-sensitive)      | `firstname = ?0`                              |
| `DEFAULT` (case-insensitive)    | `LOWER(firstname) = LOWER(?0)`                |
| `EXACT` (case-sensitive)        | `firstname = ?0`                              |
| `EXACT` (case-insensitive)      | `LOWER(firstname) = LOWER(?0)`                |
| `STARTING` (case-sensitive)     | `firstname like ?0 + '%'`                     |
| `STARTING` (case-insensitive)   | `LOWER(firstname) like LOWER(?0) + '%'`       |
| `ENDING` (case-sensitive)       | `firstname like '%' + ?0`                     |
| `ENDING` (case-insensitive)     | `LOWER(firstname) like '%' + LOWER(?0)`       |
| `CONTAINING` (case-sensitive)   | `firstname like '%' + ?0 + '%'`               |
| `CONTAINING` (case-insensitive) | `LOWER(firstname) like '%' + LOWER(?0) + '%'` |

#### 9.8.6. 投影

Spring Data的查询方法通常会返回由 repository 管理的聚合根(aggregate root)的一个或多个实例。 然而，有时可能需要根据这些类型的某些属性来创建投影。 Spring Data允许建模专用的返回类型，以更有选择地检索管理的聚合体（aggregate）的部分视图。

想象一下，一个repository和聚合根类型，如下面的例子。

Example 63. aggregate 和 repository 的实例

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

Example 64. 检索一个属性子集的投影接口

```java
interface NamesOnly {

  String getFirstname();
  String getLastname();
}
```

这里重要的一点是，这里定义的属性与聚合根（aggregate root）中的属性完全匹配。这样做可以添加一个查询方法，如下所示。

Example 65. 一个使用基于接口的投影与查询方法的 repository

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

Example 66. 检索一个属性子集的投影接口

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

Example 67. 一个封闭投影

```java
interface NamesOnly {

  String getFirstname();
  String getLastname();
}
```

如果你使用一个封闭的投影，Spring Data可以优化查询的执行，因为我们知道所有需要支持投影代理的属性。关于这一点的更多细节，请参见参考文档中的特定模块部分。

###### 开放的投影

投影接口中的访问器方法也可以通过使用 `@Value` 注解来计算新的值，如下面的例子中所示。

Example 68. 开放的投影

```java
interface NamesOnly {

  @Value("#{target.firstname + ' ' + target.lastname}")
  String getFullName();
  …
}
```

支持投影的聚合根（aggregate root）在 `target` 变量中可用。使用 `@Value` 的投影接口是一个开放的投影。在这种情况下，Spring Data不能应用查询执行优化，因为SpEL表达式可以使用聚合根的任何属性。

在 `@Value` 中使用的表达式不应过于复杂—你要避免在 `String` 变量中编程。对于非常简单的表达式，一种选择可能是求助于默认方法（在Java 8中引入），如下面的例子所示。

Example 69. 一个使用默认方法的自定义逻辑的投影接口

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

Example 70. Sample Person object

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

Example 71. Sample Person object

```java
interface NamesOnly {

  @Value("#{args[0] + ' ' + target.firstname + '!'}")
  String getSalutation(String prefix);
}
```

同样，对于更复杂的表达式，你应该使用Spring Bean并让表达式调用一个方法，如 [前](https://springdoc.cn/spring-data-jdbc/#projections.interfaces.open.bean-reference)所述。

###### Nullable Wrapper

投影接口中的Getter可以使用nullable的wrapper，以提高null的安全性。目前支持的wrapper类型有。

- `java.util.Optional`
- `com.google.common.base.Optional`
- `scala.Option`
- `io.vavr.control.Option`

Example 72. 使用 nullable wrapper 的投影接口

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

Example 73. 投影 DTO

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

Example 74. 使用动态投影参数的repository

```java
interface PersonRepository extends Repository<Person, UUID> {

  <T> Collection<T> findByLastname(String lastname, Class<T> type);
}
```

这样，该方法可用于获得原样或应用投影的聚合体（aggregate），如以下例子所示。

Example 75. 使用具有动态投影的repository

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

### 9.9. MyBatis 整合

CRUD操作和查询方法可以被委托给MyBatis。本节描述了如何配置Spring Data JDBC以与MyBatis集成，以及要遵循哪些约定来移交查询的运行以及映射到库中。

#### 9.9.1. 配置

将MyBatis正确插入Spring Data JDBC的最简单方法是将 `MyBatisJdbcConfiguration` 导入到你的应用程序配置中。

```java
@Configuration
@EnableJdbcRepositories
@Import(MyBatisJdbcConfiguration.class)
class Application {

  @Bean
  SqlSessionFactoryBean sqlSessionFactoryBean() {
    // Configure MyBatis here
  }
}
```

正如你所看到的，你所需要声明的是一个 `SqlSessionFactoryBean`，因为 `MyBatisJdbcConfiguration` 依赖于一个 `SqlSession` Bean，最终在 `ApplicationContext` 中可用。

#### 9.9.2. 使用约定

对于 `CrudRepository` 中的每个操作，Spring Data JDBC运行多个语句。如果 application context 中有一个 [`SqlSessionFactory`](https://github.com/mybatis/mybatis-3/blob/master/src/main/java/org/apache/ibatis/session/SqlSessionFactory.java)，Spring Data 就会为每个步骤检查 `SessionFactory` 是否提供一个语句。如果找到了一个，就会使用该语句（包括其配置的对实体的映射）。

语句的名称是由实体类型的完全名称与 `Mapper.` 和决定语句类型的String连接而成的。例如，如果要插入 `org.example.User` 的实例，Spring Data JDBC会寻找一个名为 `org.example.UserMapper.insert` 的语句。

当语句运行时，[`MyBatisContext`] 的一个实例被作为参数传递，这使得各种参数对语句可用。

下表描述了可用的MyBatis语句。

| Name                               | 目的                                                         | CrudRepository的方法可能会触发这个语句                       | `MyBatisContext` 中的可用属性                                |
| :--------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `insert`                           | 插入一个单一的实体。这也适用于由a ggregate root 引用的实体。 | `save`, `saveAll`.                                           | `getInstance`: 要保存的实例。`getDomainType`: 要保存的实体的类型。`get(<key>)`: 引用实体的ID，其中 `<key>` 是由 `NamingStrategy` 提供的反向引用列的名称。 |
| `update`                           | 更新一个单一的实体。这也适用于被 aggregate root 引用的实体。 | `save`, `saveAll`.                                           | `getInstance`: 要保存的实例。`getDomainType`: 要保存的实体的类型。 |
| `delete`                           | 删除一个单一的实体。                                         | `delete`, `deleteById`.                                      | `getId`: 要删除的实例的ID。`getDomainType`: 要删除的实体的类型。 |
| `deleteAll-<propertyPath>`         | 删除由任何类型的 aggregate root 所引用的所有实体，这些aggregate root 与给定的属性路径用作前缀。注意，用来作为语句名称前缀的类型是aggregate root 的名称，而不是要删除的实体的名称。 | `deleteAll`.                                                 | `getDomainType`: 要删除的实体的类型。                        |
| `deleteAll`                        | 删除所有作为前缀的类型的aggregate root。                     | `deleteAll`.                                                 | `getDomainType`: 要删除的实体的类型。                        |
| `delete-<propertyPath>`            | 删除由 aggregate root 引用的所有实体，并给出属性路径(propertyPath)。 | `deleteById`.                                                | `getId`: 要删除被引用实体的 aggregate root 的ID。`getDomainType`: 要删除的实体的类型。 |
| `findById`                         | 通过ID select 一个aggregate root                             | `findById`.                                                  | `getId`: 要加载的实体的ID。`getDomainType`: 要加载的实体的类型。 |
| `findAll`                          | select 所有aggregate root                                    | `findAll`.                                                   | `getDomainType`: 要加载的实体的类型。                        |
| `findAllById`                      | 按ID值select一组aggregate roots。                            | `findAllById`.                                               | `getId`: 一个要加载的实体的ID值的列表。`getDomainType`: 要加载的实体的类型。 |
| `findAllByProperty-<propertyName>` | 选择一个被另一个实体引用的实体集合。引用实体的类型被用作前缀。被引用实体的类型作为后缀。此方法已被废弃。请使用 `findAllByPath` 代替 | 如果没有为 `findAllByPath` 定义查询，那么就是所有的 `find*` 方法 | `getId`: 要加载的实体的ID。`getDomainType`: 要加载的实体的类型。 |
| `findAllByPath-<propertyPath>`     | 选择一个被另一个实体通过属性路径引用的实体集合。.            | 所有 `find*` 方法。                                          | `getIdentifier`: 持有 aggregate root 的ID的 `Identifier`，加上所有路径元素的key和集合索引。`getDomainType`: 要加载的实体的类型。 |
| `findAllSorted`                    | Select 所有aggregate root，并进行排序                        | `findAll(Sort)`.                                             | `getSort`: 排序。                                            |
| `findAllPaged`                     | 分页 select aggregate root，可以选择排序。                   | `findAll(Page)`.                                             | `getPageable`: 分页。                                        |
| `count`                            | 计算作为前缀的类型的 aggregate root 的数量。                 | `count`                                                      | `getDomainType`: 要计算count的aggregate root的类型。         |

### 9.10. 生命周期事件

Spring Data JDBC 触发的事件会被发布到 application context 中任何匹配的 `ApplicationListener` Bean 上。事件和回调只对 aggregate root 进行触发。如果你想处理非 root 实体，你需要通过包含 aggregate root 的监听器（listener）来完成。

实体生命周期事件可能是昂贵的，当加载大型结果集时，你可能会注意到性能概况的变化。你可以在 [Template API](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/jdbc/core/JdbcAggregateTemplate.html#setEntityLifecycleEventsEnabled(boolean)) 上停用生命周期事件。

例如，在一个集合被保存之前，下面的监听器会被调用。

```java
@Bean
ApplicationListener<BeforeSaveEvent<Object>> loggingSaves() {

	return event -> {

		Object entity = event.getEntity();
		LOG.info("{} is getting saved.", entity);
	};
}
```

如果你想只处理特定 domain 类型的事件，你可以从 `AbstractRelationalEventListener` 派生出你的监听器，并覆写一个或多个 `onXXX` 方法，其中 `XXX` 代表一个事件类型。回调方法将只被调用于与 domain 类型及其子类型相关的事件。

```java
class PersonLoadListener extends AbstractRelationalEventListener<Person> {

	@Override
	protected void onAfterLoad(AfterLoadEvent<Person> personLoad) {
		LOG.info(personLoad.getEntity());
	}
}
```

下表描述了可用的事件。关于过程步骤之间的确切关系的更多细节，请看 [可用回调的说明](https://springdoc.cn/spring-data-jdbc/#jdbc.entity-callbacks)，它1:1地映射到事件。

| 事件                                                         | 事件发布时机                                                 |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`BeforeDeleteEvent`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/event/BeforeDeleteEvent.html) | 在一个aggregate root被删除之前。                             |
| [`AfterDeleteEvent`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/event/AfterDeleteEvent.html) | 在一个 aggregate root 被删除后。                             |
| [`BeforeConvertEvent`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api//org/springframework/data/relational/core/mapping/event/BeforeConvertEvent.html) | 在 aggregate root 被转换为执行SQL语句的计划之前，但在决定aggregate是否是新的（new）之后，也就是说，如果更新或插入是有序的。如果你想以编程方式设置一个id，这就是正确的事件。 |
| [`BeforeSaveEvent`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api//org/springframework/data/relational/core/mapping/event/BeforeSaveEvent.html) | 在 aggregate root 被保存之前（即插入或更新，但在决定是否被插入或更新之后）。不要用它来为新 aggregate 创建Ids。使用 `BeforeConvertEvent` 或者更好的 `BeforeConvertCallback` 来代替。 |
| [`AfterSaveEvent`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/event/AfterSaveEvent.html) | 在一个 aggregate root 被保存（即插入或更新）后。             |
| [`AfterLoadEvent`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/event/AfterLoadEvent.html) | 在从数据库 `ResultSet` 中创建一个 aggregate root 并设置其所有属性后。 注意：这已被废弃。使用 `AfterConvert` 代替。 |
| [`AfterConvertEvent`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/event/AfterConvertEvent.html) | 在从数据库 `ResultSet` 中创建一个 aggregate root 并设置其所有属性后。 |

|      | 生命周期事件依赖于 `ApplicationEventMulticaster`，在 `SimpleApplicationEventMulticaster` 的情况下，它可以用 `TaskExecutor` 来配置，因此在事件被处理时没有保证。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 9.10.1. Store 特有的 EntityCallback

Spring Data JDBC 使用 `EntityCallback` API来支持其审计，并对下表中列出的回调作出反应。

| 过程                                                         | `EntityCallback` / 处理步骤                                  | 说明             |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :--------------- |
| Delete                                                       | [`BeforeDeleteCallback`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/event/BeforeDeleteCallback.html) | 在实际删除之前。 |
| aggregate root 和该 aggregate 的所有实体被从数据库中删除。   |                                                              |                  |
| [`AfterDeleteCallback`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/event/AfterDeleteCallback.html) | 在一个 aggregate 被删除后。                                  |                  |
| Save                                                         | 确定是否要对 aggregate 进行插入或更新，取决于它是否是新的。  |                  |
| [`BeforeConvertCallback`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api//org/springframework/data/relational/core/mapping/event/BeforeConvertCallback.html) | 如果你想以编程方式设置一个ID，这就是正确的回调。在上一步中，新的 aggregate 被检测到，在这一步中生成的Id将在下一步中使用。 |                  |
| 将 aggregate 转换为 aggregate change，它是一连串的SQL语句，要针对数据库执行。在这一步中，决定是否由 aggregate 提供一个Id，或者Id仍然是空的，并期望由数据库生成。 |                                                              |                  |
| [`BeforeSaveCallback`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api//org/springframework/data/relational/core/mapping/event/BeforeSaveCallback.html) | 对 aggregate root 的改变可能会被考虑，但是否将id值发送到数据库的决定已经在上一步中做出。 |                  |
| 上面确定的SQL语句会针对数据库执行。                          |                                                              |                  |
| [`AfterSaveCallback`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/event/AfterSaveCallback.html) | 在一个 aggregate root 被保存（即插入或更新）后。             |                  |
| Load                                                         | 使用1个或多个SQL查询加载 aggregate 。从结果集中构建 aggregate。 |                  |
| [`AfterConvertCallback`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/relational/core/mapping/event/AfterConvertCallback.html) |                                                              |                  |

我们鼓励使用回调而不是事件，因为它们支持使用不可变的类，因此比事件更强大、更通用。

### 9.11. 实体（Entity）回调

Spring Data基础设施提供了钩子（hooks），用于在某些方法被调用之前和之后修改实体。这些所谓的 `EntityCallback` 实例提供了一种方便的方式，以回调的方式检查和潜在地修改一个实体。 `EntityCallback` 看起来很像一个专门的 `ApplicationListener`。一些Spring Data模块发布了 store 特定事件（如 `BeforeSaveEvent`），允许修改给定的实体。在某些情况下，例如在处理不可变类型时，这些事件会造成麻烦。另外，事件发布依赖于 `ApplicationEventMulticaster`。如果将其与异步 `TaskExecutor` 配置在一起，可能会导致不可预测的结果，因为事件处理可以 fork 到一个线程。

实体回调提供了与同步和响应式API的集成点，以保证在处理链中定义好的检查点上的无序执行，返回可能修改的实体或响应式包装类型。

实体回调通常按API类型分开。这种分离意味着同步API只考虑同步的实体回调，而响应式实现只考虑响应式实体回调。

|      | 实体回调API是由Spring Data Commons 2.2引入的。它是应用实体修改的推荐方式。在调用可能注册的 `EntityCallback` 实例 **之前**，现有的 store 特定的 `ApplicationEvents` 仍然被发布。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 9.11.1. 实现实体回调

`EntityCallback` 通过其泛型类型参数与它的 domain 类型直接相关。每个Spring Data 模块通常都有一组预定义的 `EntityCallback` 接口，涵盖了实体的生命周期。

Example 76. Anatomy of an `EntityCallback`

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

Example 77. Anatomy of a reactive `EntityCallback`

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

Example 78. Example `BeforeSaveCallback`

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

#### 9.11.2. 注册实体回调

`EntityCallback` Bean 在 `ApplicationContext` 中注册的情况下被 store 的具体实现所接收。大多数模板API已经实现了 `ApplicationContextAware`，因此可以访问 `ApplicationContext`。

下面的例子解释了一个有效的实体回调注册的集合。

Example 79. Example `EntityCallback` Bean registration

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

### 9.12. 自定义转换（Conversion）

Spring Data JDBC允许注册自定义转换器（converter），以影响数据库中值的映射方式。目前，转换器只适用于属性级别。

#### 9.12.1. 使用 Spring Converter 进行写

下面的例子显示了一个 `Converter` 的实现，它将一个 `Boolean` 对象转换为一个 `String` 值。

```java
@WritingConverter
public class BooleanToStringConverter implements Converter<Boolean, String> {

    @Override
    public String convert(Boolean source) {
        return source != null && source ? "T" : "F";
    }
}
```

这里有几件事情需要注意。`Boolean` 和 `String` 都是简单的类型，因此Spring Data 需要提示这个converter应该在哪个方向上应用（读取或写入）。通过用 `@WritingConverter` 来注解这个converter，指示 Spring Data在数据库中将每个 `Boolean` 属性写成 `String`。

#### 9.12.2. 使用 Spring Converter 进行读

下面的例子显示了一个 `Converter` 的实现，它将 `String` 转换为 `Boolean`。

```java
@ReadingConverter
public class StringToBooleanConverter implements Converter<String, Boolean> {

    @Override
    public Boolean convert(String source) {
        return source != null && source.equalsIgnoreCase("T") ? Boolean.TRUE : Boolean.FALSE;
    }
}
```

这里有几件事情需要注意。`String` 和 `Boolean` 都是简单的类型，因此Spring Data需要提示这个converter应该在哪个方向上应用（读或写）。通过用 `@ReadingConverter` 来注解这个converter，指示Spring Data从数据库中转换每个应该被分配给 `Boolean` 属性的 `String` 值。

#### 9.12.3. 用 `JdbcConverter` 注册Spring Converter

```java
class MyJdbcConfiguration extends AbstractJdbcConfiguration {

    // …

    @Override
    protected List<?> userConverters() {
	return Arrays.asList(new BooleanToStringConverter(), new StringToBooleanConverter());
    }

}
```

|      | 在Spring Data JDBC的先前版本中，建议直接覆写 `AbstractJdbcConfiguration.jdbcCustomConversions()`。这不再是必要的，甚至不建议这样做，因为该方法集合了用于所有数据库的conversion、由所用 `Dialect` 注册的 conversion 和由用户注册的 conversion。如果你从旧版本的Spring Data JDBC迁移过来，并且 `AbstractJdbcConfiguration.jdbcCustomConversions()` 被覆写，那么来自你的 `Dialect` 的 conversion 将不会被注册。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 9.12.4. JdbcValue

值转换（Value conversion）使用 `JdbcValue` 来充实传播到JDBC操作中的具有 `java.sql.Types` 类型的值。如果你需要指定一个JDBC特定的类型，而不是使用类型派生，请注册一个自定义write converter。这个转换器应该将值转换为 `JdbcValue`，它有一个 value 和实际的 `JDBCType` 的字段。

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

### 9.13. 日志

Spring Data JDBC 本身几乎不做任何日志记录。相反，`JdbcTemplate` 发布SQL语句的机制提供了日志记录。因此，如果你想检查运行了哪些SQL语句，请激活 Spring 的 [`NamedParameterJdbcTemplate`](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#jdbc-JdbcTemplate) 或 [MyBatis](https://www.mybatis.org/mybatis-3/logging.html) 的日志。

### 9.14. 事务

`CrudRepository` 实例的方法默认是事务性的。对于读取操作，事务配置的 `readOnly` 标志被设置为 `true`。所有其他的都是用普通的 `@Transactional` 注解来配置的，所以默认的事务配置是适用的。详情请见 [`SimpleJdbcRepository`](https://docs.spring.io/spring-data/jdbc/docs/3.1.0-SNAPSHOT/api/org/springframework/data/jdbc/repository/support/SimpleJdbcRepository.html) 的 Javadoc。如果你需要调整 repository 中声明的某个方法的事务配置，请在你的 repository 接口中重新声明该方法，如下所示。

Example 80. CRUD的自定义事务配置

```java
interface UserRepository extends CrudRepository<User, Long> {

  @Override
  @Transactional(timeout = 10)
  List<User> findAll();

  // Further query method declarations
}
```

前面的内容导致 `findAll()` 方法以10秒的超时时间运行，并且没有 `readOnly` 标志。

另一种改变事务行为的方式是通过使用一个通常涵盖一个以上 repository 的 facade 或 service 实现。它的目的是为非 CRUD 操作定义事务性的边界。下面的例子展示了如何创建这样一个 facade。

Example 81. 使用 facade 来定义多个 repository 调用的事务

```java
@Service
public class UserManagementImpl implements UserManagement {

  private final UserRepository userRepository;
  private final RoleRepository roleRepository;

  UserManagementImpl(UserRepository userRepository,
    RoleRepository roleRepository) {
    this.userRepository = userRepository;
    this.roleRepository = roleRepository;
  }

  @Transactional
  public void addRoleToAllUsers(String roleName) {

    Role role = roleRepository.findByName(roleName);

    for (User user : userRepository.findAll()) {
      user.addRole(role);
      userRepository.save(user);
    }
}
```

前面的例子导致对 `addRoleToAllUsers(…)` 的调用在一个事务中运行（参与现有的事务或在没有事务的情况下创建一个新的事务）。repository 的事务配置被忽略了，因为外部事务配置决定了实际使用的 repository。请注意，你必须明确地激活 `<tx:annotation-driven />` 或使用 `@EnableTransactionManagement` 来使基于注解的配置的 facade 工作。注意，前面的例子假设你使用组件扫描。

#### 9.14.1. 事务性查询方法

要让你的查询方法是事务性的，在你定义的 repository 接口处使用 `@Transactional`，如下例所示。

Example 82. 在查询方法中使用 @Transactional

```java
@Transactional(readOnly = true)
interface UserRepository extends CrudRepository<User, Long> {

  List<User> findByLastname(String lastname);

  @Modifying
  @Transactional
  @Query("delete from User u where u.active = false")
  void deleteInactiveUsers();
}
```

通常情况下，你希望 `readOnly` 标志被设置为 `true`，因为大多数的查询方法只读取数据。与此相反， `deleteInactiveUsers()` 使用了 `@Modifying` 注解并覆盖了事务配置。因此，该方法的 `readOnly` 标志被设置为 `false`。

|      | 强烈建议使查询方法成为事务性的。这些方法可能会执行一个以上的查询，以填充一个实体。如果没有一个共同的事务，Spring Data JDBC就会在不同的连接中执行这些查询。这可能会给连接池带来过大的压力，甚至可能导致死锁，当多个方法请求一个新的连接，而同时又坚持使用一个连接时。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 通过设置 `readOnly` 标志将只读查询标记为只读是绝对合理的。然而，这并不是作为一种检查，以确保你不触发操纵性查询（尽管一些数据库拒绝在只读事务中的 `INSERT` 和 `UPDATE` 语句）。相反，`readOnly` 标志是作为一种提示传播给底层的JDBC驱动程序，以便进行性能优化。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 9.15. 审计

#### 9.15.1. 基础

Spring Data提供了复杂的支持，可以透明地跟踪谁创建或更改了实体以及更改发生的时间。为了从该功能中获益，你必须为你的实体类配备审计元数据，这些元数据可以使用注解或实现接口来定义。此外，审计必须通过注解配置或XML配置来启用，以注册所需的基础设施组件。关于配置样本，请参考特定store部分。

|      | 只跟踪创建和修改日期的应用程序不需要使其实体实现 [`AuditorAware`](https://springdoc.cn/spring-data-jdbc/#auditing.auditor-aware)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 基于注解的审计元数据

我们提供 `@CreatedBy` 和 `@LastModifiedBy` 来捕获创建或修改实体的用户，以及 `@CreatedDate` 和 `@LastModifiedDate` 来捕获变化发生的时间。

Example 83. An audited entity

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

Example 84. 嵌入实体中的审计元数据

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

##### 基于接口的审计元数据

如果你不想使用注解来定义审计元数据，你可以让你的domain类实现 `Auditable` 接口。它为所有的审计属性暴露了 `setter` 方法。

##### `AuditorAware`

如果你使用 `@CreatedBy` 或 `@LastModifiedBy`，审计基础设施需要以某种方式知道当前的principal。为此，我们提供了一个 `AuditorAware<T>` SPI接口，你必须实现这个接口来告诉基础设施谁是与应用程序交互的当前用户或系统。泛型 `T` 定义了用 `@CreatedBy` 或 `@LastModifiedBy` 注解的属性必须是什么类型。

下面的例子显示了一个使用Spring Security的 `Authentication` 对象的接口实现。

Example 85. 基于 Spring Security 的 `AuditorAware` 的实现

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

##### `ReactiveAuditorAware`

当使用响应式基础设施时，你可能想利用上下文（Context）信息来提供 `@CreatedBy` 或 `@LastModifiedBy` 信息。我们提供了一个 `ReactiveAuditorAware<T>` SPI接口，你必须实现这个接口来告诉基础设施谁是当前与应用程序交互的用户或系统。泛型 `T` 定义了用 `@CreatedBy` 或 `@LastModifiedBy` 注释的属性必须是什么类型。

下面的例子显示了一个接口的实现，它使用了Spring Security的 `Authentication` 对象。

Example 86. 基于 Spring Security 的 `ReactiveAuditorAware` 的实现

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

### 9.16. JDBC 审计

为了激活审计，在配置中添加 `@EnableJdbcAuditing`，如下例所示。

Example 87. 用Java配置激活审计

```java
@Configuration
@EnableJdbcAuditing
class Config {

  @Bean
  AuditorAware<AuditableUser> auditorProvider() {
    return new AuditorAwareImpl();
  }
}
```

如果你将 `AuditorAware` 类型的 bean 暴露给 `ApplicationContext`，审计基础设施会自动拾取它，并使用它来确定要在 domain 类型上设置的当前用户。如果你在 `ApplicationContext` 中注册了多个实现，你可以通过明确设置 `@EnableJdbcAuditing` 的 `auditorAwareRef` 属性来选择要使用的一个。

### 9.17. JDBC 锁

Spring Data JDBC支持对派生查询方法进行加锁。为了在 repository 中启用给定的派生查询方法的锁，你可以用 `@Lock` 来注释它。`LockMode` 类型的要求值提供两个值。`PESSIMISTIC_READ`，保证你正在读取的数据不会被修改；`PESSIMISTIC_WRITE`，获得一个锁来修改数据。有些数据库不做这种区分。在这种情况下，两种模式都等同于 `PESSIMISTIC_WRITE`。

Example 88. 在派生查询方法上使用 @Lock

```java
interface UserRepository extends CrudRepository<User, Long> {

  @Lock(LockMode.PESSIMISTIC_READ)
  List<User> findByLastname(String lastname);
}
```

正如你在上面看到的，`findByLastname(String lastname)` 方法将以悲观锁的方式执行。如果你使用的是MySQL方言的数据库，这将导致例如以下的查询。

Example 89. MySQL方言的Sql查询结果

```sql
Select * from user u where u.lastname = lastname LOCK IN SHARE MODE
```

除了 `LockMode.PESSIMISTIC_READ`，你可以使用 `LockMode.PESSIMISTIC_WRITE`。

# 附录

## Appendix A: 术语表

- AOP

  面向切面编程。

- CRUD

  Create, Read, Update, Delete - 基本的持久化操作

- Dependency Injection（依赖注入）

  模式，将一个组件的依赖关系从外部交给该组件，使该组件可以自己查找依赖关系。更多信息，见 https://en.wikipedia.org/wiki/Dependency_Injection。

- JPA

  Java Persistence（持久化） API

- Spring

  Java 应用框架 — https://projects.spring.io/spring-framework

## Appendix B: Populators 命名空间参考

### <populator /> 元素

`<populator />` 元素允许通过Spring Data Repository 基础设施填充data store。[[1](https://springdoc.cn/spring-data-jdbc/#_footnotedef_1)]

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
| 实现 `Streamable` 并接受 `Streamable` 构造函数或工厂方法参数的类型 | 暴露了一个构造函数或 `…of(…)`/`…valueOf(…)` 工厂方法的类型，以 `Streamable` 作为参数。详见 [返回自定义的 Streamable Wrapper 类型](https://springdoc.cn/spring-data-jdbc/#repositories.collections-and-iterables.streamable-wrapper)。 |
| Vavr `Seq`, `List`, `Map`, `Set`                             | Vavr集合类型。详见 [支持 Vavr 集合](https://springdoc.cn/spring-data-jdbc/#repositories.collections-and-iterables.vavr)。 |
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

[1](https://springdoc.cn/spring-data-jdbc/#_footnoteref_1). 见 [[repositories.create-instances.xml\]](https://springdoc.cn/spring-data-jdbc/#repositories.create-instances.xml)

Version 3.1.0-SNAPSHOT
Last updated 2023-09-10 13:54:58 +0800

[主页](https://springdoc.cn/docs/)