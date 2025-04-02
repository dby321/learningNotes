# Spring Data JPA 中文文档

version 3.1.0-SNAPSHOT,2023-09-10

- [1. 前言](https://springdoc.cn/spring-data-jpa/#preface)

- [2. Spring Data 升级](https://springdoc.cn/spring-data-jpa/#upgrading)

- [3. 依赖](https://springdoc.cn/spring-data-jpa/#dependencies)

- 4. 与 Spring Data Repository 一起工作

  - [4.1. 核心概念](https://springdoc.cn/spring-data-jpa/#repositories.core-concepts)
  - [4.2. Query 方法](https://springdoc.cn/spring-data-jpa/#repositories.query-methods)
  - [4.3. 定义 Repository 接口](https://springdoc.cn/spring-data-jpa/#repositories.definition)
  - 4.4. 定义 Query 方法
    - [4.4.1. Query 的查询策略](https://springdoc.cn/spring-data-jpa/#repositories.query-methods.query-lookup-strategies)
    - [4.4.2. Query 创建](https://springdoc.cn/spring-data-jpa/#repositories.query-methods.query-creation)
    - [4.4.3. 属性表达式](https://springdoc.cn/spring-data-jpa/#repositories.query-methods.query-property-expressions)
    - [4.4.4. 特殊参数处理](https://springdoc.cn/spring-data-jpa/#repositories.special-parameters)
    - [4.4.5. 限制查询结果](https://springdoc.cn/spring-data-jpa/#repositories.limit-query-result)
    - [4.4.6. Repository 方法返回 Collection 或 Iterable](https://springdoc.cn/spring-data-jpa/#repositories.collections-and-iterables)
    - [4.4.7. Repository 方法的 Null 处理](https://springdoc.cn/spring-data-jpa/#repositories.nullability)
    - [4.4.8. 流式（Stream）查询结果](https://springdoc.cn/spring-data-jpa/#repositories.query-streaming)
    - [4.4.9. 异步（Asynchronous）查询结果](https://springdoc.cn/spring-data-jpa/#repositories.query-async)
  - [4.5. 创建 Repository 实例](https://springdoc.cn/spring-data-jpa/#repositories.create-instances)
  - [4.6. 自定义 Spring Data Repository 的实现](https://springdoc.cn/spring-data-jpa/#repositories.custom-implementations)
  - [4.7. 从 Aggregate Root （聚合ROOT）中发布事件](https://springdoc.cn/spring-data-jpa/#core.domain-events)
  - [4.8. Spring Data 扩展](https://springdoc.cn/spring-data-jpa/#core.extensions)

- [5. 参考文档](https://springdoc.cn/spring-data-jpa/#reference)

- [6. 附录](https://springdoc.cn/spring-data-jpa/#appendix)

- [Appendix A: 命名空间参考](https://springdoc.cn/spring-data-jpa/#repositories.namespace-reference)

- [Appendix B: Populators 命名空间参考](https://springdoc.cn/spring-data-jpa/#populator.namespace-reference)

- [Appendix C: Repository query 关键字](https://springdoc.cn/spring-data-jpa/#repository-query-keywords)

- [Appendix D: Repository 查询返回类型](https://springdoc.cn/spring-data-jpa/#repository-query-return-types)

- [Appendix E: 常见问题](https://springdoc.cn/spring-data-jpa/#faq)

- [Appendix F: 术语表](https://springdoc.cn/spring-data-jpa/#glossary)

Spring Data JPA 是 Spring Data 项目的一部分，它提供了一种简化的数据访问方式，用于与关系型数据库进行交互。它基于 Java Persistence API（JPA） 标准，并提供了一套简洁的 API 和注解，使开发人员能够通过简单的 Java 对象来表示数据库表，并通过自动生成的 SQL 语句执行常见的 CRUD 操作。Spring Data JPA 通过封装 JPA 的复杂性，简化了数据访问层的开发工作，使开发人员能够更专注于业务逻辑的实现。它还提供了丰富的查询方法的定义、分页和排序支持、事务管理等功能，使开发人员能够更方便地进行数据访问和操作。

© 2008-2022 The original authors.

|      | 本站([springdoc.cn](https://springdoc.cn/))中的内容来源于 [spring.io](https://spring.io/) ，原始版权归属于 [spring.io](https://spring.io/)。由 [springdoc.cn](https://springdoc.cn/) 进行翻译，整理。可供个人学习、研究，未经许可，不得进行任何转载、商用或与之相关的行为。 商标声明：Spring 是 Pivotal Software, Inc. 在美国以及其他国家的商标。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

## 1. 前言

Spring Data JPA 为 Jakarta Persistence API（JPA）提供 repository 支持。它简化了需要访问JPA数据源的应用程序的开发。

### 1.1. 项目元数据

- 版本控制: https://github.com/spring-projects/spring-data-jpa
- Bug跟踪: https://github.com/spring-projects/spring-data-jpa/issues
- Release repository: https://repo.spring.io/libs-release
- Milestone repository: https://repo.spring.io/libs-milestone
- Snapshot repository: https://repo.spring.io/libs-snapshot

## 2. Spring Data 升级

关于如何从Spring Data的早期版本升级的说明在项目 [wiki](https://github.com/spring-projects/spring-data-commons/wiki)上提供。按照 [发布说明部分](https://github.com/spring-projects/spring-data-commons/wiki#release-notes) 的链接，找到你要升级的版本。

升级说明总是在发行说明中的第一项。如果你落后一个以上的版本，请确保你也查看你跳过的版本的发行说明。

## 3. 依赖

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

### 3.1. 使用Spring Boot的依赖管理

Spring Boot会为你选择一个最新版本的Spring Data模块。如果你仍然想升级到较新的版本，将 `spring-data-releasetrain.version` 属性设置为你想使用的[train version 和 iteration](https://springdoc.cn/spring-data-jpa/#dependencies.train-version)。

### 3.2. Spring Framework

当前版本的Spring Data模块需要Spring框架 6.0.4 或更高的版本。这些模块也可能在该次要版本的较早的错误修复版本中工作。但是，强烈建议使用那一代中的最新版本。

## 4. 与 Spring Data Repository 一起工作

Spring Data Repository 抽象的目标是大大减少为各种持久性store实现数据访问层所需的模板代码量。

|      | *Spring Data Repository 文档和你的模块*本章解释了Spring Data Repository 的核心概念和接口。本章的信息是从Spring Data Commons模块中提取的。它使用了Jakarta Persistence API（JPA）模块的配置和代码样本。 如果你想使用XML配置，你应该将XML命名空间声明和要扩展的类型调整为你使用的特定模块的等价物。“[命名空间参考](https://springdoc.cn/spring-data-jpa/#repositories.namespace-reference)” 涵盖了XML配置，所有支持 Repository API的Spring Data模块都支持这种配置。 “[Repository query 关键字](https://springdoc.cn/spring-data-jpa/#repository-query-keywords)” 涵盖了 Repository 抽象所支持的一般的查询方法关键字。关于你的模块的具体功能的详细信息，请参阅本文档中关于该模块的章节。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 4.1. 核心概念

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

### 4.2. Query 方法

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

3. 设置Spring为这些接口创建代理实例，可以用[JavaConfig](https://springdoc.cn/spring-data-jpa/#repositories.create-instances.java-config)或[XML 配置](https://springdoc.cn/spring-data-jpa/#repositories.create-instances)。

   Java

   XML

   ```java
   @EnableJpaRepositories
   class Config { … }
   ```

   本例中使用的是JPA namespace。如果你对任何其他store使用 Repository 抽象，你需要把它改为你的store模块的适当 namespace 声明。换句话说，你应该把 `jpa` 换成，例如 `mongodb`。

   请注意，JavaConfig 并没有明确地配置 `package`，因为被注解的类的 `package` 是默认使用的。要自定义要扫描的包，请使用数据store特定库的 `@EnableJpaRepositories` 注解的 `basePackage…` 属性之一。

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

- [定义 Repository 接口](https://springdoc.cn/spring-data-jpa/#repositories.definition)
- [定义 Query 方法](https://springdoc.cn/spring-data-jpa/#repositories.query-methods.details)
- [创建 Repository 实例](https://springdoc.cn/spring-data-jpa/#repositories.create-instances)
- [自定义 Spring Data Repository 的实现](https://springdoc.cn/spring-data-jpa/#repositories.custom-implementations)

### 4.3. 定义 Repository 接口

要定义一个 repository 接口，你首先需要定义一个domain类专用的 repository 接口。该接口必须继承 `Repository`，并将其泛型设置为domain类和ID类。如果你想为该domain类公开CRUD方法，你可以继承 `CrudRepository`，或其变体，而不是 `Repository`。

#### 4.3.1. 稍微修改 Repository 的定义

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

#### 4.3.2. 在多个Spring数据模块中使用 Repository

在你的应用程序中使用一个独特的Spring Data模块使事情变得简单，因为定义范围内的所有 repository 接口都绑定到Spring Data模块。有时，应用程序需要使用一个以上的Spring Data模块。在这种情况下，repository 定义必须区分持久化技术。当它检测到类路径上有多个 repository 工厂时，Spring Data会进入严格的 repository 配置模式。严格的配置使用 repository 或domain类的细节来决定 repository 定义的Spring Data模块绑定。

1. 如果 repository 定义 [继承了特定模块的 repository](https://springdoc.cn/spring-data-jpa/#repositories.multiple-modules.types)，那么它就是特定Spring Data模块的有效候选 repository。
2. 如果 domain 类被注解了 [模块特定的类型注解](https://springdoc.cn/spring-data-jpa/#repositories.multiple-modules.annotations)，它就是特定Spring Data模块的有效候选者。Spring Data模块接受第三方注解（如JPA的 `@Entity`）或提供自己的注解（如Spring Data MongoDB和Spring Data Elasticsearch的 `@Document`）。

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

[Repository 类型细节](https://springdoc.cn/spring-data-jpa/#repositories.multiple-modules.types)和[区分domain类注解](https://springdoc.cn/spring-data-jpa/#repositories.multiple-modules.annotations)用于严格的repository库配置，以确定特定Spring Data模块的repository候选者。在同一domain类型上使用多个持久化技术的特定注解是可能的，并且能够在多个持久化技术中重复使用domain类型。然而，Spring Data就不能再确定一个唯一的模块来绑定repository了。

区分 repository 的最后一个方法是通过对 repository base package的扫描。base package 定义了扫描 repository 接口定义的起点，这意味着将 repository 的定义放在适当的包中。默认情况下，注解驱动的配置使用配置类所在的base package。基于[XML的配置中的base package](https://springdoc.cn/spring-data-jpa/#repositories.create-instances.xml)，需要手动强制配置。

下面的例子显示了注解驱动的 base package 的配置。

Example 12. 注解驱动的 base package 的配置

```java
@EnableJpaRepositories(basePackages = "com.acme.repositories.jpa")
@EnableMongoRepositories(basePackages = "com.acme.repositories.mongo")
class Configuration { … }
```

### 4.4. 定义 Query 方法

repository 代理有两种方法可以从方法名中推导出 repository 特定的查询。

- 通过直接从方法名派生出查询。
- 通过使用手动定义的查询。

可用的选项取决于实际的store。然而，必须有一个策略来决定创建什么样的实际查询。下一节将介绍可用的选项。

#### 4.4.1. Query 的查询策略

下列策略可用于 repository 基础设施解析查询。 对于 XML 配置，你可以通过 `query-lookup-strategy` 属性在命名空间配置策略。 对于 Java 配置，你可以使用 `EnableJpaRepositories` 注解的 `queryLookupStrategy` 属性。有些策略可能不支持特定的datastore。

- `CREATE` 试图从查询方法名称中构建一个特定的存储查询。一般的做法是从方法名中删除一组已知的前缀，然后解析方法的其余部分。你可以在 “[Query 创建](https://springdoc.cn/spring-data-jpa/#repositories.query-methods.query-creation)” 中阅读更多关于查询构建的信息。 `USE_DECLARED_QUERY` 试图找到一个已声明的查询，如果找不到就会抛出一个异常。查询可以由某处的注解来定义，也可以通过其他方式来声明。请参阅特定store的文档以找到该store的可用选项。如果 repository 基础设施在启动时没有为该方法找到一个已声明的查询，则会失败。
- `CREATE_IF_NOT_FOUND` (默认) 结合了 `CREATE` 和 `USE_DECLARED_QUERY`。它首先查找一个已声明的查询， 如果没有找到已声明的查询， 它将创建一个基于方法名的自定义查询。这是默认的查询策略，因此，如果你没有明确地配置任何东西，就会使用这种策略。它允许通过方法名快速定义查询，但也可以根据需要通过引入已声明的查询对这些查询进行自定义调整。

#### 4.4.2. Query 创建

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

解析查询方法名称分为主语和谓语。第一部分（`find…By`, `exists…By`）定义了查询的主语，第二部分形成谓语。引入句（主语）可以包含进一步的表达。在 `find`（或其他引入关键词）和 `By` 之间的任何文本都被认为是描述性的，除非使用一个限制结果的关键词，如 `Distinct` 在要创建的查询上设置一个不同的标志，或 [`Top` / `First` 来限制查询结果](https://springdoc.cn/spring-data-jpa/#repositories.limit-query-result)。

附录中包含了 [查询方法主语关键词](https://springdoc.cn/spring-data-jpa/#appendix.query.method.subject) 和 [查询方法谓语关键词的完整列表](https://springdoc.cn/spring-data-jpa/#appendix.query.method.predicate)，包括排序和字母修饰语。然而，第一个 `By` 作为分界符，表示实际条件谓词的开始。在一个非常基本的层面上，你可以在实体属性上定义条件，并用 `And` 和 `Or` 来连接它们。

解析方法的实际结果取决于你为之创建查询的持久性store。然而，有一些东西需要注意。

- 表达式通常是属性遍历与可以串联的运算符的组合。你可以用 `AND` 和 `OR` 来组合属性表达式。你还可以得到对属性表达式的运算符的支持，如 `Between`, `LessThan`, `GreaterThan`, 和 `Like`。支持的运算符可能因 datastore 的不同而不同，所以请查阅参考文档的适当部分。
- 方法解析器支持为单个属性（例如，`findByLastnameIgnoreCase(…)`）或支持忽略大小写的类型的所有属性（通常是字符串实例—例如，`findByLastnameAndFirstnameAllIgnoreCase`(…)）设置忽略大小写标志。是否支持忽略大小写可能因store而异，所以请查阅参考文档中的相关章节，了解特定store的查询方法。
- 你可以通过在引用属性的查询方法中附加一个 `OrderBy` 子句，并提供一个排序方向（`Asc` 或 `Desc`）来应用静态排序。要创建一个支持动态排序的查询方法，请参阅 “[特殊参数处理](https://springdoc.cn/spring-data-jpa/#repositories.special-parameters)”。

#### 4.4.3. 属性表达式

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

#### 4.4.4. 特殊参数处理

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

#### 4.4.5. 限制查询结果

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

#### 4.4.6. Repository 方法返回 Collection 或 Iterable

返回多个结果的查询方法可以使用标准的Java `Iterable`、`List` 和 `Set`。除此之外，我们还支持返回Spring Data的 `Streamable`，这是 `Iterable` 的一个自定义扩展，以及 [Vavr](https://www.vavr.io/) 提供的 collection 类型。请参考附录中对所有可能的 [查询方法返回类型](https://springdoc.cn/spring-data-jpa/#appendix.query.return.types)的解释。

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

#### 4.4.7. Repository 方法的 Null 处理

从Spring Data 2.0开始，返回单个聚合实例的 repository CRUD方法使用Java 8的 `Optional` 来表示可能没有的值。除此之外，Spring Data还支持在查询方法上返回以下 wrapper 类型。

- `com.google.common.base.Optional`
- `scala.Option`
- `io.vavr.control.Option`

另外，查询方法可以选择完全不使用 wrapper 类型。没有查询结果的话会通过返回 `null` 来表示。Repository 方法返回集合、集合替代物、wrapper和流时，保证不会返回 `null`，而是返回相应的空（Empty）表示。详见 “[Repository 查询返回类型](https://springdoc.cn/spring-data-jpa/#repository-query-return-types)”。

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

#### 4.4.8. 流式（Stream）查询结果

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

#### 4.4.9. 异步（Asynchronous）查询结果

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

### 4.5. 创建 Repository 实例

本节介绍了如何为定义的 repository 接口创建实例和Bean定义。

#### 4.5.1. Java 配置

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

#### 4.5.2. XML 配置

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

#### 4.5.3. 使用 Filter

默认情况下，基础架构会抓取每个扩展了位于配置的 base package 下的持久化技术特定的 `Repository` 子接口的接口，并为其创建一个Bean实例。然而，你可能想要更精细地控制哪些接口为其创建Bean实例。要做到这一点，可以在 Repository 声明中使用 filter 元素。其语义与Spring的组件过滤器中的元素完全等同。详情请见 [Spring参考文档](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/core.html#beans-scanning-filters) 中的这些元素。

例如，为了排除某些接口作为 Repository Bean 的实例化，你可以使用以下配置。

Example 27. 使用 Filter

Java

XML

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

#### 4.5.4. 单独使用

你也可以在Spring容器之外使用资源库基础设施—例如，在CDI环境中。你仍然需要在你的classpath中使用一些Spring库，但是，一般来说，你也可以通过编程来设置 Repository。Repository 支持的Spring Data模块都有一个特定于持久化技术的 `RepositoryFactory`，你可以使用，如下所示。

Example 28. repository factory 的独立使用

```java
RepositoryFactorySupport factory = … // Instantiate factory here
UserRepository repository = factory.getRepository(UserRepository.class);
```

### 4.6. 自定义 Spring Data Repository 的实现

Spring Data提供了各种选项来创建查询方法，只需少量编码。但当这些选项不符合你的需求时，你也可以为 repository 方法提供你自己的自定义实现。本节介绍了如何做到这一点。

#### 4.6.1. 自定义个别 Repository

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

Spring Data Repository 是通过使用形成 repository 组合的片段来实现的。片段是基础repository、功能方面（如 [QueryDsl](https://springdoc.cn/spring-data-jpa/#core.extensions.querydsl)），以及自定义接口和它们的实现。每当你为你的repository接口添加一个接口，你就通过添加一个片段来增强组合。基础repository和repository方面的实现是由每个Spring Data模块提供的。

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
@EnableJpaRepositories(repositoryImplementationPostfix = "MyPostfix")
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

如果你的自定义实现只使用基于注解的配置和自动注入，前面所示的方法很好用，因为它被当作任何其他Spring Bean。如果你的实现片段Bean需要特殊的注入，你可以根据[前文](https://springdoc.cn/spring-data-jpa/#repositories.single-repository-behaviour.ambiguity)所述的约定来声明Bean并为其命名。然后，基础设施通过名称来引用手动定义的Bean定义，而不是自己创建一个。下面的例子展示了如何手动注入一个自定义的实现。

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

#### 4.6.2. 自定义 Base Repository

当你想定制基础 repository 的行为时，[上一节](https://springdoc.cn/spring-data-jpa/#repositories.manual-wiring)描述的方法需要定制每个 repository 的接口，以便所有的 repository 都受到影响。为了改变所有 repository 的行为，你可以创建一个扩展持久化技术特定 repository 基类的实现。然后这个类作为 repository 代理的自定义基类，如下面的例子所示。

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
@EnableJpaRepositories(repositoryBaseClass = MyRepositoryImpl.class)
class ApplicationConfiguration { … }
```

### 4.7. 从 Aggregate Root （聚合ROOT）中发布事件

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

### 4.8. Spring Data 扩展

本节记录了一组Spring Data扩展，这些扩展使Spring Data能够在各种情况下使用。目前，大部分的集成都是针对Spring MVC的。

#### 4.8.1. Querydsl 扩展

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

#### 4.8.2. Web 的支持

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

[上一节](https://springdoc.cn/spring-data-jpa/#core.web)所示的配置注册了一些基本组件。

- [使用 `DomainClassConverter` 类](https://springdoc.cn/spring-data-jpa/#core.web.basic.domain-class-converter) 让Spring MVC从请求参数或路径变量中解析 Repository 管理的 domain 类实例。
- [`HandlerMethodArgumentResolver`](https://springdoc.cn/spring-data-jpa/#core.web.basic.paging-and-sorting) 的实现，让Spring MVC从请求参数中解析 `Pageable` 和 `Sort` 实例。
- [Jackson模块](https://springdoc.cn/spring-data-jpa/#core.web.basic.jackson-mappers) 对 `Point` 和 `Distance` 等类型进行序列化/反序列化，或存储特定的类型，这取决于使用的Spring数据模块。

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

[上一节](https://springdoc.cn/spring-data-jpa/#core.web.basic.domain-class-converter) 中的配置片段还注册了一个 `PageableHandlerMethodArgumentResolver` 以及一个 `SortHandlerMethodArgumentResolver` 的实例。注册后，`Pageable` 和 `Sort` 可以作为有效的controller方法参数，如下图所示。

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

核心模块和一些特定的存储模块与一组Jackson模块一起发布，用于Spring Data domain 域使用的类型，如 `org.springframework.data.geo.Distance` 和 `org.springframework.data.geo.Point`。 一旦启用 [web支持](https://springdoc.cn/spring-data-jpa/#core.web) 和 `com.fasterxml.jackson.databind.ObjectMapper` 可用，就会导入这些模块。

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

你可以通过使用 [JSONPath](https://goessner.net/articles/JsonPath/) 表达式（需要 [Jayway JsonPath](https://github.com/json-path/JsonPath)）或 [XPath](https://www.w3.org/TR/xpath-31/) 表达式（需要 [XmlBeam](https://xmlbeam.org/)）来使用 Spring Data 投影（在 [投影](https://springdoc.cn/spring-data-jpa/#projections) 中描述）来绑定传入的请求的payload，如下例所示。

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

如 [投影](https://springdoc.cn/spring-data-jpa/#projections) 中所述，支持嵌套投影。如果该方法返回一个复杂的、非接口类型，则使用Jackson `ObjectMapper` 来映射最终值。

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

#### 4.8.3. Repository 填充

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

## 5. 参考文档

### 5.1. JPA Repository

本章指出了 JPA 的 repository 支持的专业性。这建立在 “[与 Spring Data Repository 一起工作](https://springdoc.cn/spring-data-jpa/#repositories)” 中解释的核心 repository 支持的基础上。请确保你对那里解释的基本概念有良好的理解。

#### 5.1.1. 简介

本节介绍了通过以下两种方式配置Spring Data JPA的基础知识。

- “[Spring 命名空间](https://springdoc.cn/spring-data-jpa/#jpa.namespace)” (XML 配置)
- “[基于注解的配置](https://springdoc.cn/spring-data-jpa/#jpa.java-config)” (Java 配置)

##### 基于注解的配置

Spring Data JPA Repository 的支持可以通过 JavaConfig 以及自定义XML命名空间来激活，如下例所示。

Example 52. Spring Data JPA repositories using JavaConfig

```java
@Configuration
@EnableJpaRepositories
@EnableTransactionManagement
class ApplicationConfig {

  @Bean
  public DataSource dataSource() {

    EmbeddedDatabaseBuilder builder = new EmbeddedDatabaseBuilder();
    return builder.setType(EmbeddedDatabaseType.HSQL).build();
  }

  @Bean
  public LocalContainerEntityManagerFactoryBean entityManagerFactory() {

    HibernateJpaVendorAdapter vendorAdapter = new HibernateJpaVendorAdapter();
    vendorAdapter.setGenerateDdl(true);

    LocalContainerEntityManagerFactoryBean factory = new LocalContainerEntityManagerFactoryBean();
    factory.setJpaVendorAdapter(vendorAdapter);
    factory.setPackagesToScan("com.acme.domain");
    factory.setDataSource(dataSource());
    return factory;
  }

  @Bean
  public PlatformTransactionManager transactionManager(EntityManagerFactory entityManagerFactory) {

    JpaTransactionManager txManager = new JpaTransactionManager();
    txManager.setEntityManagerFactory(entityManagerFactory);
    return txManager;
  }
}
```

|      | 你必须创建 `LocalContainerEntityManagerFactoryBean`，而不是直接创建 `EntityManagerFactory`，因为前者除了创建 `EntityManagerFactory` 外，还参与了异常转换机制。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

前面的配置类通过使用 `spring-jdbc` 的 `EmbeddedDatabaseBuilder` API设置了一个嵌入式HSQL数据库。然后，Spring Data设置了一个 `EntityManagerFactory`，并使用Hibernate作为样本持久化提供者。这里声明的最后一个基础设施组件是 `JpaTransactionManager`。最后，该示例通过使用 `@EnableJpaRepositories` 注解来激活Spring Data JPA Repository，该注解本质上与XML命名空间携带相同的属性。如果没有配置 base package，它就使用配置类所在的包。

##### Spring 命名空间

Spring Data 的 JPA 模块包含一个自定义命名空间，允许定义 repository bean。它还包含JPA的某些特殊功能和元素属性。一般来说，JPA repository 可以通过使用 `repositories` 元素来设置，如下面的例子所示。

Example 53. 通过使用命名空间设置JPA repository

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:jpa="http://www.springframework.org/schema/data/jpa"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/data/jpa
    https://www.springframework.org/schema/data/jpa/spring-jpa.xsd">

  <jpa:repositories base-package="com.acme.repositories" />

</beans>
```

|      | JavaConfig和XML哪个更好？XML是很久以前配置Spring的方式。在今天这个Java、record类型、注解等快速发展的时代，新项目通常尽可能多地使用纯Java。虽然没有立即删除XML支持的计划，但一些最新的功能可能无法通过XML获得。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

正如 “[创建 Repository 实例](https://springdoc.cn/spring-data-jpa/#repositories.create-instances)” 中所述，使用 `repositories` 元素可以查找Spring Data Repository 。除此之外，它还为所有用 `@Repository` 注解的Bean激活了持久化异常转换，以便让JPA持久化提供者抛出的异常被转换为Spring的 `DataAccessException` 层次结构。

###### 自定义命名空间属性

除了 `repositories` 元素的默认属性，JPA命名空间还提供了额外的属性，让你对 repository 的设置获得更详细的控制。

| `entity-manager-factory-ref` | 明确地将 `EntityManagerFactory` 与被 `repositories` 元素检测到的 Repository 一起使用。通常在应用程序中使用多个 `EntityManagerFactory` Bean时使用。如果没有配置，Spring Data会自动在 `ApplicationContext` 中查找名称为 `entityManagerFactory` 的 `EntityManagerFactory` bean。 |
| ---------------------------- | ------------------------------------------------------------ |
| `transaction-manager-ref`    | 明确地将 `PlatformTransactionManager` 与被 `repositories` 元素检测到的 Repository 连接起来。通常只有在配置了多个事务管理器或 `EntityManagerFactory` Bean 时才需要。默认为当前 `ApplicationContext` 内的单个定义的 `PlatformTransactionManager`。 |

|      | 如果没有明确定义 `transaction-manager-ref`，Spring Data JPA 需要一个名为 `transactionManager` 的 `PlatformTransactionManager` bean存在。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### Bootstrap Mode

默认情况下，Spring Data JPA Repository 是默认的Spring Bean。它们是单例和立即初始化的。在启动过程中，它们已经与JPA `EntityManager` 交互，以达到验证和元数据分析的目的。Spring Framework支持在后台线程中初始化JPA `EntityManagerFactory`，因为这个过程通常会在Spring应用程序中占用大量的启动时间。为了有效地利用该后台初始化，我们需要确保JPA Repository 尽可能晚地被初始化。

从 Spring Data JPA 2.1 开始，你现在可以配置一个 `BootstrapMode`（通过 `@EnableJpaRepositories` 注解或XML命名空间），它可以取以下值。

- `DEFAULT` (默认) — 除非明确地用 `@Lazy` 注解，否则 repository 是立即实例化的。懒加载只在没有客户Bean需要 repository 的实例时才有效，因为这需要初始化 repository Bean。
- `LAZY` — 隐式声明所有的 Repository Bean 都是懒加载的，也会导致懒加载的初始化代理被创建，注入到客户端Bean中。这意味着，如果客户Bean只是将实例存储在一个字段中，而不是在初始化过程中使用Repository，那么Repository将不会被实例化。Repository实例将在与Repository的第一次交互时被初始化和验证。
- `DEFERRED` — 基本上与LAZY的操作模式相同，但在响应 `ContextRefreshedEvent` 时触发 Repository 的初始化，因此在应用程序完全启动之前就对 Repository 进行验证。

###### 建议

如果你不使用异步 JPA bootstrap，请坚持使用默认的 bootstrap mode。

如果你以异步方式引导（bootstrap）JPA，`DEFERRED` 是一个合理的默认值，因为它将确保 Spring Data JPA 引导只等待 `EntityManagerFactory` 的设置，如果这本身比初始化所有其他应用程序组件所需的时间更长。尽管如此，它仍能确保在应用程序发出启动信号之前，repository 被正确地初始化和验证。

对于测试场景和本地开发来说，`LAZY` 是一个不错的选择。一旦你确信 repository 可以正常启动，或者你正在测试应用程序的其他部分，为所有 repository 运行验证可能会不必要地增加启动时间。这同样适用于本地开发，在本地开发中，你只需要访问可能需要初始化一个 repository 的应用程序的部分。

#### 5.1.2. 持久化实体

本节介绍了如何用Spring Data JPA持久化（保存）实体。

##### 保存实体

保存一个实体可以通过 `CrudRepository.save(…)` 方法进行。它通过使用底层的JPA `EntityManager` 来持久化或合并给定的实体。如果实体还没有被持久化，Spring Data JPA会通过调用 `entityManager.persist(…)` 方法来保存实体。否则，它会调用 `entityManager.merge(…)` 方法。

###### 实体状态检测策略

Spring Data JPA提供了以下策略来检测一个实体是否是新的。

1. Version 属性和Id 属性检查（默认）。默认情况下，Spring Data JPA首先检查是否有一个非原始类型的Version属性。如果有的话，如果该属性的值是 `null`，那么该实体就被认为是新的。如果没有这样的Version属性，Spring Data JPA会检查给定实体的Id属性。如果Id属性是 `null`，那么该实体被认为是新的。否则，它就被认为是不新的。
2. 实现 `Persistable`。如果一个实体实现了 `Persistable`，Spring Data JPA 会将“是否是新的”的检测委托给实体的 `isNew(..)` 方法。详情请参见 [JavaDoc](https://docs.spring.io/spring-data/data-commons/docs/current/api/index.html?org/springframework/data/domain/Persistable.html)。
3. 实现 `EntityInformation`。你可以通过创建 `JpaRepositoryFactory` 的子类并重写 `getEntityInformation(…)` 方法来定制 `SimpleJpaRepository` 实现中使用的 `EntityInformation` 抽象。然后你必须将 `JpaRepositoryFactory` 的自定义实现注册为Spring Bean。请注意，这应该是很少需要的。详见 [JavaDoc](https://docs.spring.io/spring-data/data-jpa/docs/current/api/index.html?org/springframework/data/jpa/repository/support/JpaRepositoryFactory.html)。

对于那些使用手动分配id和没有version属性的实体来说，选项1不是一个选项，因为对于那些实体来说，id总是非 `null` 的。在这种情况下，一个常见的模式是使用一个普通的基类，它有一个默认为新实例的 transient 标志，并使用JPA生命周期回调来翻转（flip）持久化操作的标志。

Example 54. 一个用于手动分配id的实体的基类

```java
@MappedSuperclass
public abstract class AbstractEntity<ID> implements Persistable<ID> {

  @Transient
  private boolean isNew = true; 

  @Override
  public boolean isNew() {
    return isNew; 
  }

  @PrePersist 
  @PostLoad
  void markNotNew() {
    this.isNew = false;
  }

  // More code…
}
```

|      | 声明一个标志来保持新的状态。暂时性的（Transient），这样它就不会持久化到数据库中。 |
| ---- | ------------------------------------------------------------ |
|      | 在 `Persistable.isNew()` 的实现中返回该标志，以便Spring Data Repository 知道是调用 `EntityManager.persist()` 还是 `….merge()` 。 |
|      | 使用JPA实体回调声明一个方法，以便在 repository 调用 `save(…)` 或 persistence provider 创建实例后，标志被切换为表示现有实体。 |

#### 5.1.3. 查询（Query）方法

本节介绍了用Spring Data JPA创建查询的各种方法。

##### 查询（Query）的查找策略

JPA模块支持将查询手动定义为一个字符串，或者让它从方法名称中衍生出来。

带有谓词 `IsStartingWith`, `StartingWith`, `StartsWith`, `IsEndingWith`, `EndingWith`, `EndsWith`, `IsNotContaining`, `NotContaining`, `NotContains`, `IsContaining`, `Containing`, `Contains` 的衍生查询，这些查询的各自参数将被“消毒”处理。这意味着如果参数实际上包含 `LIKE` 识别为通配符的字符，这些参数将被转义，所以它们只作为字面意义上的字符匹配。使用的转义字符可以通过设置 `@EnableJpaRepositories` 注解的 `escapeCharacter` 来进行配置。与 [使用 SpEL 表达式](https://springdoc.cn/spring-data-jpa/#jpa.query.spel-expressions) 比较。

###### 声明查询

尽管从方法名中获取查询是非常方便的，但是我们可能会面临这样的情况：要么方法名解析器不支持我们想要使用的关键字，要么方法名会变得不必要的丑陋。因此，你可以通过命名惯例使用JPA命名的查询（更多信息请参见[使用JPA的命名查询](https://springdoc.cn/spring-data-jpa/#jpa.query-methods.named-queries) ），或者用 `@Query` 注解你的查询方法（详见[使用 `@Query`](https://springdoc.cn/spring-data-jpa/#jpa.query-methods.at-query)）。

##### 查询创建

一般来说，JPA的查询创建机制如 “[Query 方法](https://springdoc.cn/spring-data-jpa/#repositories.query-methods)” 中描述的那样工作。下面的例子显示了JPA的查询方法转化为什么。

Example 55. 从方法名创建查询

```
public interface UserRepository extends Repository<User, Long> {

  List<User> findByEmailAddressAndLastname(String emailAddress, String lastname);
}
```

我们使用JPA标准API从中创建一个查询，但基本上，这转化为以下查询：`select u from User u where u.emailAddress = ?1 and u.lastname = ?2`。 Spring Data JPA做了一个属性检查并遍历嵌套属性，如 “[属性表达式](https://springdoc.cn/spring-data-jpa/#repositories.query-methods.query-property-expressions)” 中描述。

下表描述了JPA支持的关键字，以及包含该关键字的方法所转换的内容。

| 关键字                 | 示例                                                         | JPQL片段                                                     |
| :--------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `Distinct`             | `findDistinctByLastnameAndFirstname`                         | `select distinct … where x.lastname = ?1 and x.firstname = ?2` |
| `And`                  | `findByLastnameAndFirstname`                                 | `… where x.lastname = ?1 and x.firstname = ?2`               |
| `Or`                   | `findByLastnameOrFirstname`                                  | `… where x.lastname = ?1 or x.firstname = ?2`                |
| `Is`, `Equals`         | `findByFirstname`,`findByFirstnameIs`,`findByFirstnameEquals` | `… where x.firstname = ?1`                                   |
| `Between`              | `findByStartDateBetween`                                     | `… where x.startDate between ?1 and ?2`                      |
| `LessThan`             | `findByAgeLessThan`                                          | `… where x.age < ?1`                                         |
| `LessThanEqual`        | `findByAgeLessThanEqual`                                     | `… where x.age <= ?1`                                        |
| `GreaterThan`          | `findByAgeGreaterThan`                                       | `… where x.age > ?1`                                         |
| `GreaterThanEqual`     | `findByAgeGreaterThanEqual`                                  | `… where x.age >= ?1`                                        |
| `After`                | `findByStartDateAfter`                                       | `… where x.startDate > ?1`                                   |
| `Before`               | `findByStartDateBefore`                                      | `… where x.startDate < ?1`                                   |
| `IsNull`, `Null`       | `findByAge(Is)Null`                                          | `… where x.age is null`                                      |
| `IsNotNull`, `NotNull` | `findByAge(Is)NotNull`                                       | `… where x.age not null`                                     |
| `Like`                 | `findByFirstnameLike`                                        | `… where x.firstname like ?1`                                |
| `NotLike`              | `findByFirstnameNotLike`                                     | `… where x.firstname not like ?1`                            |
| `StartingWith`         | `findByFirstnameStartingWith`                                | `… where x.firstname like ?1` (用 `%` 绑定参数)              |
| `EndingWith`           | `findByFirstnameEndingWith`                                  | `… where x.firstname like ?1` (用 `%` 绑定参数)              |
| `Containing`           | `findByFirstnameContaining`                                  | `… where x.firstname like ?1` (用 `%` 包裹参数)              |
| `OrderBy`              | `findByAgeOrderByLastnameDesc`                               | `… where x.age = ?1 order by x.lastname desc`                |
| `Not`                  | `findByLastnameNot`                                          | `… where x.lastname <> ?1`                                   |
| `In`                   | `findByAgeIn(Collection<Age> ages)`                          | `… where x.age in ?1`                                        |
| `NotIn`                | `findByAgeNotIn(Collection<Age> ages)`                       | `… where x.age not in ?1`                                    |
| `True`                 | `findByActiveTrue()`                                         | `… where x.active = true`                                    |
| `False`                | `findByActiveFalse()`                                        | `… where x.active = false`                                   |
| `IgnoreCase`           | `findByFirstnameIgnoreCase`                                  | `… where UPPER(x.firstname) = UPPER(?1)`                     |

|      | `In` 和 `NotIn` 也接受任何 `Collection` 的子类作为参数，以及数组或 varargs。对于同一逻辑运算符的其他语法版本，请查看 “[Repository query 关键字](https://springdoc.cn/spring-data-jpa/#repository-query-keywords)”。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | `DISTINCT` 可能很棘手，并不总是产生你期望的结果。例如，`select distinct u from User u` 与 `select distinct u.lastname from User u` 产生的结果完全不同。在第一种情况下，由于你包括 `User.id`，没有什么会重复，因此你会得到整个表，而且是 `User` 对象。然而，后一个查询将把重点缩小到 `User.lastname`，并找到该表的所有唯一的 lastname。这也会产生一个 `List<String>` 结果集，而不是 `List<User>` 结果集。`countDistinctByLastname(String lastname)` 也会产生意外的结果。Spring Data JPA 将导出 `select count(distinct u.id) from User u where u.lastname = ?1`。 同样，由于 `u.id` 不会碰到任何重复的，这个查询将计算出所有具有绑定 lastname 的用户。这与 `countByLastname(String lastname)` 相同!这个查询的重点到底是什么？要找到具有给定 lastname 的人数？找出具有该绑定 lastname 的不同人的数量？要找到不同 lastname 的人数？ （最后一个是一个完全不同的查询！）使用 `distinct` 的有时需要手工编写查询，并使用 `@Query` 来最好地捕捉你所寻求的信息，因为你可能还需要一个投影来捕捉结果集。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

###### 基于注解的配置

基于注解的配置的优点是不需要编辑另一个配置文件，降低了维护工作量。但是，你需要为每一个新的命名查询重新编译你的 domain 类。

Example 56. 基于注解的命名查询配置

```java
@Entity
@NamedQuery(name = "User.findByEmailAddress",
  query = "select u from User u where u.emailAddress = ?1")
public class User {

}
```

##### 使用JPA的命名查询

|      | 这些例子使用 `<named-query />` 元素和 `@NamedQuery` 注解。这些配置元素的查询必须用JPA查询语言来定义。当然，你也可以使用 `<named-native-query />` 或 `@NamedNativeQuery`。这些元素让你在失去数据库平台独立性的情况下用本地SQL定义查询。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

###### XML命名的查询定义

要使用XML配置，将必要的 `<named-query />` 元素添加到位于classpath的 `META-INF` 文件夹中的 `orm.xml` JPA配置文件中。通过使用一些定义好的命名惯例来启用命名查询的自动调用。更多细节，见下文。

Example 57. XML命名查询配置

```xml
<named-query name="User.findByLastname">
  <query>select u from User u where u.lastname = ?1</query>
</named-query>
```

查询需要指定一个名字，用于在运行时解析它。

###### 声明接口

要允许这些命名的查询，请按如下方式指定 `UserRepositoryWithRewriter`。

Example 58. 在UserRepository中的查询方法声明

```java
public interface UserRepository extends JpaRepository<User, Long> {

  List<User> findByLastname(String lastname);

  User findByEmailAddress(String emailAddress);
}
```

Spring Data 试图将对这些方法的调用解析为一个命名的查询，以配置的 domain 类的简单名称开始，后面是用点分隔的方法名称。所以前面的例子会使用前面定义的命名查询，而不是试图从方法名中创建一个查询。

##### 使用 `@Query`

使用命名的查询来声明对实体的查询是一种有效的方法，对于少量的查询来说效果也不错。由于查询本身与运行它们的Java方法相联系，你实际上可以通过使用Spring Data JPA `@Query` 注解来直接绑定它们，而不是将它们注解到 domain 类中。这就把 domain 类从持久化的特定信息中解放出来，并把查询与 repository 接口放在一起。

对查询方法进行注解的查询优先于使用 `@NamedQuery` 定义的查询或在 `orm.xml` 中声明的命名查询。

下面的例子显示了一个用 `@Query` 注解创建的查询。

Example 59. 在查询方法中使用 `@Query` 声明查询。

```java
public interface UserRepository extends JpaRepository<User, Long> {

  @Query("select u from User u where u.emailAddress = ?1")
  User findByEmailAddress(String emailAddress);
}
```

###### 使用 QueryRewriter

有时，无论你尝试应用多少功能，似乎都不可能让Spring Data JPA在查询被发送到 `EntityManager` 之前将你想要的东西都应用到其中。

你有能力在查询被发送到 `EntityManager` 之拿到它，并 "重写" 它。也就是说，你可以在最后时刻做任何改动。

Example 60. 使用 `@Query` 声明一个QueryRewriter

```java
public interface MyRepository extends JpaRepository<User, Long> {

		@Query(value = "select original_user_alias.* from SD_USER original_user_alias",
                nativeQuery = true,
				queryRewriter = MyQueryRewriter.class)
		List<User> findByNativeQuery(String param);

		@Query(value = "select original_user_alias from User original_user_alias",
                queryRewriter = MyQueryRewriter.class)
		List<User> findByNonNativeQuery(String param);
}
```

这个例子既展示了本地（纯SQL）重写器（rewriter），也展示了JPQL查询，两者都利用了同一个 `QueryRewriter`。在这种情况下，Spring Data JPA将寻找在 application context 中注册的相应类型的bean。

你可以这样写一个查询重写器。

Example 61. `QueryRewriter` 示例

```java
public class MyQueryRewriter implements QueryRewriter {

     @Override
     public String rewrite(String query, Sort sort) {
         return query.replaceAll("original_user_alias", "rewritten_user_alias");
     }
}
```

你必须确保你的 `QueryRewriter` 在应用上下文中被注册，无论是通过应用Spring Framework的 `@Component` 系列注解之一，还是将其作为 `@Configuration` 类中的 `@Bean` 方法的一部分。

另一个选择是让 repository 本身实现该接口。

Example 62. 提供 `QueryRewriter` 的 Repository

```java
public interface MyRepository extends JpaRepository<User, Long>, QueryRewriter {

		@Query(value = "select original_user_alias.* from SD_USER original_user_alias",
                nativeQuery = true,
				queryRewriter = MyRepository.class)
		List<User> findByNativeQuery(String param);

		@Query(value = "select original_user_alias from User original_user_alias",
                queryRewriter = MyRepository.class)
		List<User> findByNonNativeQuery(String param);

		@Override
		default String rewrite(String query, Sort sort) {
			return query.replaceAll("original_user_alias", "rewritten_user_alias");
		}
}
```

根据你对你的 `QueryRewriter` 所做的工作，最好是有多个，每个都在 application context 中注册。

|      | 在基于CDI的环境中，Spring Data JPA将在 `BeanManager` 中搜索你实现的 `QueryRewriter` 的实例。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

###### 使用高级的 `LIKE` 表达式

用 `@Query` 创建的手动定义查询的查询运行机制允许在查询定义内定义高级 `LIKE` 表达式，如下面的例子所示。

Example 63. 在 `@Query` 中 `like` 的高级表达式

```java
public interface UserRepository extends JpaRepository<User, Long> {

  @Query("select u from User u where u.firstname like %?1")
  List<User> findByFirstnameEndsWith(String firstname);
}
```

在前面的例子中，`LIKE` 分隔符（`%`）被识别，查询被转化为有效的 JPQL 查询（去除 `%`）。在运行查询时，传递给方法调用的参数被增加了先前识别的 `LIKE` 模式。

###### 原生查询

`@Query` 注解允许通过设置 `nativeQuery` 标志为 `true` 来运行原生查询，如下例所示。

Example 64. 使用 `@Query` 在查询方法中声明一个原生查询

```java
public interface UserRepository extends JpaRepository<User, Long> {

  @Query(value = "SELECT * FROM USERS WHERE EMAIL_ADDRESS = ?1", nativeQuery = true)
  User findByEmailAddress(String emailAddress);
}
```

|      | Spring Data JPA目前不支持原生查询的动态排序，因为它必须对实际声明的查询进行操作，而对于原生SQL来说，它无法可靠地做到这一点。然而，你可以通过自己指定count查询来使用原生查询进行分页，如下面的例子中所示。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

Example 65. 通过使用 `@Query` 在查询方法中声明用于分页的原生 count 查询

```java
public interface UserRepository extends JpaRepository<User, Long> {

  @Query(value = "SELECT * FROM USERS WHERE LASTNAME = ?1",
    countQuery = "SELECT count(*) FROM USERS WHERE LASTNAME = ?1",
    nativeQuery = true)
  Page<User> findByLastname(String lastname, Pageable pageable);
}
```

类似的方法也适用于原生的命名查询（named native query），方法是将 `.count` 后缀添加到查询的副本中。不过，你可能需要为你的 count 查询注册一个结果集映射。

##### 使用 Sort（排序）

排序可以通过提供一个 `PageRequest` 或者直接使用 `Sort` 来完成。在 `Sort` 的 `Order` 实例中实际使用的属性需要与你的 domain model 相匹配，这意味着它们需要解析到查询中使用的一个属性或一个别名。JPQL 将其定义为一个状态字段路径表达式。

|      | 使用任何不可引用的路径表达式都会导致一个 `Exception`。 |
| ---- | ------------------------------------------------------ |
|      |                                                        |

然而，与 [`@Query`](https://springdoc.cn/spring-data-jpa/#jpa.query-methods.at-query) 一起使用 `Sort` 可以让你在 `ORDER BY` 子句中潜入含有函数的非路径检查的 `Order` 实例。这是有可能的，因为 `Order` 被附加到了给定的查询字符串中。默认情况下，Spring Data JPA拒绝任何包含函数调用的 `Order` 实例，但你可以使用 `JpaSort.unsafe` 来添加潜在的不安全排序。

下面的例子使用了 `Sort` 和 `JpaSort`，包括 `JpaSort` 上的一个 unsafe 选项。

Example 66. 使用 `Sort` 和 `JpaSort`

```java
public interface UserRepository extends JpaRepository<User, Long> {

  @Query("select u from User u where u.lastname like ?1%")
  List<User> findByAndSort(String lastname, Sort sort);

  @Query("select u.id, LENGTH(u.firstname) as fn_len from User u where u.lastname like ?1%")
  List<Object[]> findByAsArrayAndSort(String lastname, Sort sort);
}

repo.findByAndSort("lannister", Sort.by("firstname"));                
repo.findByAndSort("stark", Sort.by("LENGTH(firstname)"));            
repo.findByAndSort("targaryen", JpaSort.unsafe("LENGTH(firstname)")); 
repo.findByAsArrayAndSort("bolton", Sort.by("fn_len"));               
```

|      | 有效的 `Sort` 表达式，指向 domain model 中的属性。 |
| ---- | -------------------------------------------------- |
|      | 包含函数调用的无效的 `Sort`。抛出异常。            |
|      | 有效的 `Sort`，包含明确的 unsafe `Order`。         |
|      | 有效的 `Sort` 表达式，指向别名的函数。             |

##### 使用命名参数

默认情况下，Spring Data JPA使用基于位置的参数绑定，正如前面所有例子中所描述的那样。这使得查询方法在关于参数位置的重构中有点容易出错。为了解决这个问题，你可以使用 `@Param` 注解来给方法参数一个具体的名字，并在查询中绑定这个名字，如下面的例子所示。

Example 67. 使用命名参数

```java
public interface UserRepository extends JpaRepository<User, Long> {

  @Query("select u from User u where u.firstname = :firstname or u.lastname = :lastname")
  User findByLastnameOrFirstname(@Param("lastname") String lastname,
                                 @Param("firstname") String firstname);
}
```

|      | 方法参数是根据它们在定义的查询中的顺序来切换的。 |
| ---- | ------------------------------------------------ |
|      |                                                  |

|      | 从第4版开始，Spring完全支持Java 8的基于 `-parameters` 编译器标志的参数名称发现（parameter name discovery）。通过在你的构建中使用这个标志作为 debug 信息的替代，你可以省略命名参数的 `@Param` 注解。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 使用 SpEL 表达式

从Spring Data JPA 1.4版本开始，我们支持在用 `@Query` 定义的手动查询中使用受限的SpEL模板表达式。在查询运行时，这些表达式会针对一组预定义的变量进行评估。Spring Data JPA支持一个名为 `entityName` 的变量。它的用法是 `select x from #{#entityName} x`。它插入了与给定 repository 相关的 domain 类型的 `entityName`。`entityName` 的解析方法如下。如果 domain 类已经在 `@Entity` 注解上设置了名称属性，它就会被使用。否则，将使用 doamin 类的简单类名。

下面的例子演示了查询字符串中 `#{#entityName}` 表达式的一个用例，你想用查询方法和手动定义的查询来定义一个 repository 接口。

Example 68. 在 repository 查询方法中使用SpEL表达式 - entityName

```java
@Entity
public class User {

  @Id
  @GeneratedValue
  Long id;

  String lastname;
}

public interface UserRepository extends JpaRepository<User,Long> {

  @Query("select u from #{#entityName} u where u.lastname = ?1")
  List<User> findByLastname(String lastname);
}
```

为了避免在 `@Query` 注解的查询字符串中说明实际的 entity name，你可以使用 `#{#entityName}` 变量。

|      | `entityName` 可以通过使用 `@Entity` 注解来定制。SpEL 表达式不支持在 `orm.xml` 中的自定义。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

当然，你可以直接在查询声明中使用 `User`，但这也需要你改变查询的方式。对 `#entityName` 的引用可以捕捉到未来潜在的 `User` 类对不同实体名称的重新映射（例如，通过使用 `@Entity(name = "MyUser")`）。

查询字符串中 `#{#entityName}` 表达式的另一个用例是，如果你想为一个具体的domain类型定义一个通用的 repository 接口与专门的 repository 接口。为了不重复定义具体接口上的自定义查询方法，你可以在通用 repository 接口的 `@Query` 注解的查询字符串中使用实体名称（entity name）表达式，如下例所示。

Example 69. 在 repository 查询方法中使用SpEL表达式 - 具有继承性的 entityName

```java
@MappedSuperclass
public abstract class AbstractMappedType {
  …
  String attribute
}

@Entity
public class ConcreteType extends AbstractMappedType { … }

@NoRepositoryBean
public interface MappedTypeRepository<T extends AbstractMappedType>
  extends Repository<T, Long> {

  @Query("select t from #{#entityName} t where t.attribute = ?1")
  List<T> findAllByAttribute(String attribute);
}

public interface ConcreteRepository
  extends MappedTypeRepository<ConcreteType> { … }
```

在前面的例子中，`MappedTypeRepository` 接口是一些扩展 `AbstractMappedType` 的 domain 类型的共同父接口。它还定义了通用的 `findAllByAttribute(…)` 方法，它可以在专门的 repository 接口实例上使用。如果你现在在 `ConcreteRepository` 上调用 `findByAllAttribute(…)` ，查询会变成 `select t from ConcreteType t where t.attribute = ?1`。

处理参数的SpEL表达式也可以用来处理方法参数。在这些SpEL表达式中，实体名称（entity name）是不可用的，但是参数是可用的。它们可以通过名称或索引来访问，正如下面的例子所展示的那样。

Example 70. 在 repository 查询方法中使用SpEL表达式—访问参数。

```java
@Query("select u from User u where u.firstname = ?1 and u.firstname=?#{[0]} and u.emailAddress = ?#{principal.emailAddress}")
List<User> findByFirstnameAndCurrentUserWithCustomQuery(String firstname);
```

对于 `like` 条件，人们经常希望在一个字符串值的参数的开头或结尾处添加 `%`。这可以通过在绑定参数标记或SpEL表达式中附加或前缀 `%` 来实现。下面的例子再次证明了这一点。

Example 71. 在 repository 查询方法中使用SpEL表达式—通配符快捷方式。

```java
@Query("select u from User u where u.lastname like %:#{[0]}% and u.lastname like %:lastname%")
List<User> findByLastnameWithSpelExpression(@Param("lastname") String lastname);
```

当使用 `like` 条件的值来自不安全的来源时，这些值应该被“净化”，所以它们不能包含任何通配符，从而使攻击者能够选择更多的数据。为了这个目的，`escape(String)` 方法在SpEL上下文中可用。它将第一个参数中所有 `_` 和 `%` 的实例用第二个参数中的单个字符进行前缀。与JPQL和标准SQL中的 `like` 表达式的 `escape` 结合起来，可以轻松地清理绑定的参数。

Example 72. 在 repository 查询方法中使用SpEL表达式—对输入值进行消毒

```java
@Query("select u from User u where u.firstname like %?#{escape([0])}% escape ?#{escapeCharacter()}")
List<User> findContainingEscaped(String namePart);
```

在 repository 接口中给出这个方法声明，`findContainingEscaped("Peter_")` 会找到 `Peter_Parker`，而不是 `Peter Parker`。使用的转义字符可以通过设置 `@EnableJpaRepositories` 注解的 `escapeCharacter` 来配置。请注意，在SpEL上下文中可用的方法 `escape(String)` 将只转义SQL和JPQL标准通配符 `_` 和 `%`。如果底层数据库或JPA实现支持额外的通配符，这些通配符将不会被转义。

##### 修改查询

前面所有的章节都描述了如何声明查询来访问一个给定的实体或实体集合。你可以通过使用 “[自定义 Spring Data Repository 的实现](https://springdoc.cn/spring-data-jpa/#repositories.custom-implementations)” 中描述的自定义方法设施添加自定义修改行为。由于这种方法对于全面的自定义功能是可行的，你可以通过用 `@Modifying` 来注解查询方法来修改只需要参数绑定的查询，如下面的例子所示。

Example 73. Declaring manipulating queries

```java
@Modifying
@Query("update User u set u.firstname = ?1 where u.lastname = ?2")
int setFixedFirstnameFor(String firstname, String lastname);
```

这样做会触发被注解到该方法的查询，作为一个update查询而不是select查询。由于在执行修改查询后， `EntityManager` 可能包含过时的实体，我们不会自动清除它（详见 `EntityManager.clear()` 的 [JavaDoc](https://jakarta.ee/specifications/persistence/2.2/apidocs/javax/persistence/entitymanager)），因为这将有效地丢弃 `EntityManager` 中仍然待定的所有非刷新变化。如果你希望 `EntityManager` 被自动清除，你可以将 `@Modifying` 注解的 `clearAutomatically` 属性设置为 `true`。

`@Modifying` 注解只与 `@Query` 注解结合使用。派生的查询方法或自定义方法不需要这个注解。

###### 派生的删除查询

Spring Data JPA还支持派生的删除查询，让你不必明确地声明JPQL查询，如下面的例子所示。

Example 74. 使用派生的删除查询

```java
interface UserRepository extends Repository<User, Long> {

  void deleteByRoleId(long roleId);

  @Modifying
  @Query("delete from User u where u.role.id = ?1")
  void deleteInBulkByRoleId(long roleId);
}
```

尽管 `deleteByRoleId(…)` 方法看起来基本上与 `deleteInBulkByRoleId(…)` 产生了相同的结果，但这两个方法声明在运行方式上有一个重要区别。顾名思义，后一个方法针对数据库发出一个单一的JPQL查询（注解中定义的查询）。这意味着即使是当前加载的 `User` 实例也不会看到生命周期的回调被调用。

为了确保生命周期查询被实际调用，对 `deleteByRoleId(…)` 的调用会运行一个查询，然后逐一删除返回的实例，这样持久化提供者就可以在这些实体上实际调用 `@PreRemove` 的回调。

事实上，派生的删除查询是运行查询的捷径，然后对结果调用 `CrudRepository.delete(Iterable<User> users)`，并与 `CrudRepository` 中其他 `delete(…)` 方法的实现保持行为的同步性。

##### 应用查询提示 （Query Hints）

为了将JPA查询提示应用到你的 repository 接口中声明的查询，你可以使用 `@QueryHints` 注解。它需要一个JPA `@QueryHint` 注解的数组，加上一个布尔标志，以潜在地禁用应用分页时触发的额外count查询的hints，如下面的例子所示。

Example 75. 在repository 方法中使用QueryHints

```java
public interface UserRepository extends Repository<User, Long> {

  @QueryHints(value = { @QueryHint(name = "name", value = "value")},
              forCounting = false)
  Page<User> findByLastname(String lastname, Pageable pageable);
}
```

前面的声明将为该实际查询应用配置的 `@QueryHint`，但省略了应用于为计算总页数而触发的count查询。

###### 在查询中添加注释

有时，你需要根据数据库性能来调试一个查询。你的数据库管理员向你展示的查询可能与你使用 `@Query` 编写的查询有很大的不同，或者它可能与你推测的Spring Data JPA生成的关于自定义查找器的查询完全不同，或者如果你使用 example 查询。

为了使这个过程更容易，你可以在几乎所有的JPA操作中插入自定义注释，无论是查询还是其他操作，都可以通过应用 `@Meta` 注释来实现。

Example 76. 将 `@Meta` 注解应用于 repository 操作中

```java
public interface RoleRepository extends JpaRepository<Role, Integer> {

	@Meta(comment = "find roles by name")
	List<Role> findByName(String name);

	@Override
	@Meta(comment = "find roles using QBE")
	<S extends Role> List<S> findAll(Example<S> example);

	@Meta(comment = "count roles for a given name")
	long countByName(String name);

	@Override
	@Meta(comment = "exists based on QBE")
	<S extends Role> boolean exists(Example<S> example);
}
```

这个样本 repository 混合了自定义的查找器，以及覆盖了 `JpaRepository` 的继承操作。无论哪种方式， `@Meta` 注解都可以让你添加一个 `comment`，这个注释将在查询被发送到数据库之前被插入。

同样重要的是要注意，这个功能并不仅仅局限于查询。它延伸到 `count` 和 `exists` 操作。虽然没有显示，但它也延伸到了某些 `delete` 操作。

|      | 虽然我们试图在所有可能的地方应用这一特性，但底层 `EntityManager` 的一些操作并不支持注释。例如， `entityManager.createQuery()` 被明确记录为支持注释，但 `entityManager.find()` 操作却不支持。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

JPQL日志和SQL日志都不是JPA的标准，所以每个提供者都需要自定义配置，如下所示。

激活 Hibernate 注释

要在 Hibernate 中激活查询注释，必须将 `hibernate.use_sql_comments` 设置为 `true`。

如果你使用的是基于Java的配置设置，可以这样做。

Example 77. 基于Java的JPA配置

```java
@Bean
public Properties jpaProperties() {

	Properties properties = new Properties();
	properties.setProperty("hibernate.use_sql_comments", "true");
	return properties;
}
```

如果你有一个 `persistence.xml` 文件，你可以在那里应用它。

Example 78. 基于 `persistence.xml` 的配置

```xml
<persistence-unit name="my-persistence-unit">

   ...registered classes...

	<properties>
		<property name="hibernate.use_sql_comments" value="true" />
	</properties>
</persistence-unit>
```

最后，如果你使用的是Spring Boot，那么你可以在你的 `application.properties` 文件中进行设置。

Example 79. 基于Spring Boot属性的配置

```
spring.jpa.properties.hibernate.use_sql_comments=true
```

激活EclipseLink注释

要激活 EclipseLink 中的查询注释，你必须将 `eclipselink.logging.level.sql` 设置为 `FINE`。

如果你使用的是基于Java的配置设置，可以这样做。

Example 80. 基于Java的JPA配置

```java
@Bean
public Properties jpaProperties() {

	Properties properties = new Properties();
	properties.setProperty("eclipselink.logging.level.sql", "FINE");
	return properties;
}
```

如果你有一个 `persistence.xml` 文件，你可以在那里应用它。

Example 81. 基于 `persistence.xml` 的配置

```xml
<persistence-unit name="my-persistence-unit">

   ...registered classes...

	<properties>
		<property name="eclipselink.logging.level.sql" value="FINE" />
	</properties>
</persistence-unit>
```

最后，如果你使用的是Spring Boot，那么你可以在你的 `application.properties` 文件中进行设置。

Example 82. 基于Spring Boot属性的配置

```
spring.jpa.properties.eclipselink.logging.level.sql=FINE
```

##### 配置 Fetch- 和 LoadGraphs

JPA 2.1规范引入了对指定 Fetch- 和 LoadGraphs 的支持，我们也用 `@EntityGraph` 注解来支持，它允许你引用 `@NamedEntityGraph` 定义。你可以在实体上使用该注解来配置结果查询的获取计划。获取的类型（`Fetch` 或 `Load`）可以通过使用 `@EntityGraph` 注解的 `type` 属性来配置。请参阅JPA 2.1 Spec 3.7.4以获得进一步的参考。

下面的例子显示了如何在一个实体上定义一个命名的实体图（named entity graph）。

Example 83. 在一个实体上定义一个命名实体图。

```java
@Entity
@NamedEntityGraph(name = "GroupInfo.detail",
  attributeNodes = @NamedAttributeNode("members"))
public class GroupInfo {

  // default fetch mode is lazy.
  @ManyToMany
  List<GroupMember> members = new ArrayList<GroupMember>();

  …
}
```

下面的例子显示了如何在 repository 的查询方法上引用一个命名的实体图。

Example 84. 在 repository 的查询方法上引用命名实体图的定义。

```java
public interface GroupRepository extends CrudRepository<GroupInfo, String> {

  @EntityGraph(value = "GroupInfo.detail", type = EntityGraphType.LOAD)
  GroupInfo getByGroupName(String name);

}
```

也可以通过使用 `@EntityGraph` 来定义特殊的实体图。提供的 `attributePaths` 会被翻译成相应的 `EntityGraph`，而不需要明确地将 `@NamedEntityGraph` 添加到你的 domain 类型中，如以下例子所示。

Example 85. 在 repository 查询方法上使用AD-HOC实体图定义。

```java
public interface GroupRepository extends CrudRepository<GroupInfo, String> {

  @EntityGraph(attributePaths = { "members" })
  GroupInfo getByGroupName(String name);

}
```

##### 投影

Spring Data的查询方法通常会返回由 repository 管理的聚合根(aggregate root)的一个或多个实例。 然而，有时可能需要根据这些类型的某些属性来创建投影。 Spring Data允许建模专用的返回类型，以更有选择地检索管理的聚合体（aggregate）的部分视图。

想象一下，一个repository和聚合根类型，如下面的例子。

Example 86. aggregate 和 repository 的实例

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

###### 基于接口的投影

将查询结果限制在只有名称属性的最简单的方法是声明一个接口，为要读取的属性公开 accessor 方法，如下面的例子中所示。

Example 87. 检索一个属性子集的投影接口

```java
interface NamesOnly {

  String getFirstname();
  String getLastname();
}
```

这里重要的一点是，这里定义的属性与聚合根（aggregate root）中的属性完全匹配。这样做可以添加一个查询方法，如下所示。

Example 88. 一个使用基于接口的投影与查询方法的 repository

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

Example 89. 检索一个属性子集的投影接口

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

封闭的投影

一个投影接口，其访问器（accessor）方法都与目标集合的属性相匹配，被认为是一个封闭的投影。下面的例子（我们在本章前面也用过）就是一个封闭投影。

Example 90. 一个封闭投影

```java
interface NamesOnly {

  String getFirstname();
  String getLastname();
}
```

如果你使用一个封闭的投影，Spring Data可以优化查询的执行，因为我们知道所有需要支持投影代理的属性。关于这一点的更多细节，请参见参考文档中的特定模块部分。

开放的投影

投影接口中的访问器方法也可以通过使用 `@Value` 注解来计算新的值，如下面的例子中所示。

Example 91. 开放的投影

```java
interface NamesOnly {

  @Value("#{target.firstname + ' ' + target.lastname}")
  String getFullName();
  …
}
```

支持投影的聚合根（aggregate root）在 `target` 变量中可用。使用 `@Value` 的投影接口是一个开放的投影。在这种情况下，Spring Data不能应用查询执行优化，因为SpEL表达式可以使用聚合根的任何属性。

在 `@Value` 中使用的表达式不应过于复杂—你要避免在 `String` 变量中编程。对于非常简单的表达式，一种选择可能是求助于默认方法（在Java 8中引入），如下面的例子所示。

Example 92. 一个使用默认方法的自定义逻辑的投影接口

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

Example 93. Sample Person object

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

Example 94. Sample Person object

```java
interface NamesOnly {

  @Value("#{args[0] + ' ' + target.firstname + '!'}")
  String getSalutation(String prefix);
}
```

同样，对于更复杂的表达式，你应该使用Spring Bean并让表达式调用一个方法，如 [前](https://springdoc.cn/spring-data-jpa/#projections.interfaces.open.bean-reference)所述。

Nullable Wrapper

投影接口中的Getter可以使用nullable的wrapper，以提高null的安全性。目前支持的wrapper类型有。

- `java.util.Optional`
- `com.google.common.base.Optional`
- `scala.Option`
- `io.vavr.control.Option`

Example 95. 使用 nullable wrapper 的投影接口

```java
interface NamesOnly {

  Optional<String> getFirstname();
}
```

如果底层投影值不是 `null` 的，那么将使用 wrapper 类型的当前表示法返回值。如果持有的值是 `null` 的，那么 getter 方法会返回所使用的 wrapper 类型的 `null` 表示。

###### 基于类的投影（DTO）

另一种定义投影的方法是使用value类型的DTO（数据传输对象），它持有应该被检索的字段的属性。这些DTO类型的使用方式与投影接口的使用方式完全相同，只是没有代理发生，也不能应用嵌套投影。

如果store通过限制要加载的字段来优化查询的执行，要加载的字段是由暴露出来的构造函数的参数名决定的。

下面的例子显示了一个投影的DTO。

Example 96. 投影 DTO

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

Unresolved directive in ../../../../spring-data-commons-doc/src/main/asciidoc/repository-projections.adoc - include::../../../../spring-data-jpa/src/main/asciidoc/repository-projections-dto-limitations.adoc[]

###### 动态投影

到目前为止，我们已经使用投影类型作为集合的返回类型或元素类型。然而，你可能想在调用时选择要使用的类型（这使它成为动态的）。为了应用动态投影，请使用一个查询方法，如下面的例子中所示。

Example 97. 使用动态投影参数的repository

```java
interface PersonRepository extends Repository<Person, UUID> {

  <T> Collection<T> findByLastname(String lastname, Class<T> type);
}
```

这样，该方法可用于获得原样或应用投影的聚合体（aggregate），如以下例子所示。

Example 98. 使用具有动态投影的repository

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

#### 5.1.4. 存储过程

JPA 2.1规范引入了对通过使用JPA标准查询API调用存储过程的支持。我们引入了 `@Procedure` 注解，用于在 repository 方法上声明存储过程元数据。

下面的例子使用以下存储过程。

Example 99. HSQL DB中 `plus1inout` 过程的定义。

```sql
/;
DROP procedure IF EXISTS plus1inout
/;
CREATE procedure plus1inout (IN arg int, OUT res int)
BEGIN ATOMIC
 set res = arg + 1;
END
/;
```

存储过程的元数据可以通过在实体类型上使用 `NamedStoredProcedureQuery` 注解来配置。

Example 100. 一个实体上的 StoredProcedure 元数据定义。

```java
@Entity
@NamedStoredProcedureQuery(name = "User.plus1", procedureName = "plus1inout", parameters = {
  @StoredProcedureParameter(mode = ParameterMode.IN, name = "arg", type = Integer.class),
  @StoredProcedureParameter(mode = ParameterMode.OUT, name = "res", type = Integer.class) })
public class User {}
```

请注意，`@NamedStoredProcedureQuery` 为存储过程提供了两个不同的名称。`name` 是JPA使用的名称。 `procedureName` 是存储过程在数据库中的名称。

你可以通过多种方式从 repository 方法中引用存储过程。要调用的存储过程可以通过使用 `@Procedure` 注解的 `value` 或 `procedureName` 属性直接定义。这直接指的是数据库中的存储过程，而忽略了通过 `@NamedStoredProcedureQuery` 的任何配置。

或者，你可以指定 `@NamedStoredProcedureQuery.name` 属性作为 `@Procedure.name` 属性。如果既没有配置 `value`、`procedureName`，也没有配置 `name`，那么 repository 方法的名称将被用作 `name` 属性。

下面的例子显示了如何引用一个明确映射的存储过程。

Example 101. 引用数据库中名称为 "plus1inout" 的存储过程。

```java
@Procedure("plus1inout")
Integer explicitlyNamedPlus1inout(Integer arg);
```

下面的例子与前面的例子相当，但使用了 `procedureName` 的别名。

通过 `procedureName` 别名引用数据库中名称为 "plus1inout" 的隐式存储过程。

```java
@Procedure(procedureName = "plus1inout")
Integer callPlus1InOut(Integer arg);
```

下面的内容同样等同于前两个，但使用了方法名而不是一个明确的注解属性。

Example 102. 在 `EntityManager` 中通过使用方法名称引用隐式映射的命名存储过程 "User.plus1"。

```java
@Procedure
Integer plus1inout(@Param("arg") Integer arg);
```

下面的例子显示了如何通过引用 `@NamedStoredProcedureQuery.name` 属性来引用一个存储过程。

Example 103. 在 `EntityManager` 中引用了明确映射的命名存储过程 "User.plus1IO"。

```java
@Procedure(name = "User.plus1IO")
Integer entityAnnotatedCustomNamedProcedurePlus1IO(@Param("arg") Integer arg);
```

如果被调用的存储过程有一个单一的输出参数，该参数可以作为方法的返回值返回。如果在 `@NamedStoredProcedureQuery` 注解中指定了多个输出参数，这些参数可以作为 `Map` 返回，其关键是 `@NamedStoredProcedureQuery` 注解中给出的参数名称。

#### 5.1.5. Specification

JPA 2引入了一个 criteria API，你可以用它来以编程方式建立查询。通过编写一个 `criteria`，你定义了一个 domain 类的查询的 where 子句。再退一步说，这些 criteria 可以被看作是对 JPA criteria API约束所描述的实体的谓词。

Spring Data JPA从Eric Evans的《领域驱动设计》一书中提取了 specification 的概念，遵循相同的语义，并提供了一个API来用 JPA criteria API 定义这种 specification。为了支持 specification，你可以用 `JpaSpecificationExecutor` 接口扩展你的 repository 接口，如下所示。

```java
public interface CustomerRepository extends CrudRepository<Customer, Long>, JpaSpecificationExecutor<Customer> {
 …
}
```

附加接口有一些方法，让你以各种方式运行 specification。例如，`findAll` 方法返回所有符合 specification 的实体，如下面的例子中所示。

```java
List<T> findAll(Specification<T> spec);
```

`Specification` 接口的定义如下。

```java
public interface Specification<T> {
  Predicate toPredicate(Root<T> root, CriteriaQuery<?> query,
            CriteriaBuilder builder);
}
```

Specification 可以很容易地用于在实体之上建立一个可扩展的谓语（predicate）集，然后可以与 `JpaRepository` 组合使用，而不需要为每一个需要的组合声明一个查询（方法），如以下例子所示。

Example 104. Customer 的 Specification

```java
public class CustomerSpecs {


  public static Specification<Customer> isLongTermCustomer() {
    return (root, query, builder) -> {
      LocalDate date = LocalDate.now().minusYears(2);
      return builder.lessThan(root.get(Customer_.createdAt), date);
    };
  }

  public static Specification<Customer> hasSalesOfMoreThan(MonetaryAmount value) {
    return (root, query, builder) -> {
      // build query here
    };
  }
}
```

`Customer_` 类型是使用JPA元模型生成器生成的元模型类型（见 [Hibernate实现文档中的例子](https://docs.jboss.org/hibernate/jpamodelgen/1.0/reference/en-US/html_single/#whatisit)）。所以表达式，`Customer_.createdAt`，假设 `Customer` 有一个 `Date` 类型的 `createdAt` 属性。除此之外，我们在业务需求的抽象层次上表达了一些 criteria，并创建了可执行的 `Specifications`。所以客户端可能会使用如下的 `Specification`。

Example 105. 使用一个简单的 Specification

```java
List<Customer> customers = customerRepository.findAll(isLongTermCustomer());
```

为什么不为这种数据访问创建一个查询？与普通的查询声明相比，使用单一的 `Specification` 并不能获得很多好处。当你把它们组合起来创建新的 `Specification` `对象时，Specification` 的力量才真正发挥出来。你可以通过我们提供的 `Specification` 的默认方法来实现这一点，以建立类似于下面的表达式。

Example 106. 组合 Specification

```java
MonetaryAmount amount = new MonetaryAmount(200.0, Currencies.DOLLAR);
List<Customer> customers = customerRepository.findAll(
  isLongTermCustomer().or(hasSalesOfMoreThan(amount)));
```

`Specification` 提供了一些 "胶水代码" 的默认方法来连锁和组合 `Specification` 实例。这些方法让你通过创建新的 `Specification` 实现并将它们与已有的实现结合起来来扩展你的数据访问层。

而在JPA 2.1中，`CriteriaBuilder` API引入了 `CriteriaDelete`。这是由 `JpaSpecificationExecutor` 的 `delete(Specification)` API提供的。

Example 107. 使用 `Specification` 来删除条目。

```java
Specification<User> ageLessThan18 = (root, query, cb) -> cb.lessThan(root.get("age").as(Integer.class), 18)

userRepository.delete(ageLessThan18);
```

该 `Specification` 建立了一个 criteria，其中 `age` 字段（强制转换为integer）小于 `18`。传递给 `userRepository`，它将使用 JPA 的 `CriteriaDelete` 功能来生成正确的 `DELETE` 操作。然后它返回被删除的实体的数量。

#### 5.1.6. Example 查询

##### 介绍

本章介绍了 "Example 查询" 并解释了如何使用它。

Example 查询（QBE）是一种用户友好的查询技术，接口简单。它允许动态查询创建，不要求你写包含字段名的查询。事实上，"Example 查询" 根本不要求你通过使用store特定的查询语言来编写查询。

##### 使用方式

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

Example 108. Sample Person object

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

|      | 将属性纳入 Example 查询的标准是基于非 null 的。使用原始类型（`int`, `double`, …）的属性总是被包括在内，除非 [`ExampleMatcher` 忽略了属性路径](https://springdoc.cn/spring-data-jpa/#query-by-example.matchers)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

例子可以通过使用工厂方法或通过使用 [`ExampleMatcher`](https://springdoc.cn/spring-data-jpa/#query-by-example.matchers) 来构建。`Example` 是不可改变的。下面的列表显示了一个简单的例子。

Example 109. Simple Example

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

Example 110. `QueryByExampleExecutor`

```java
public interface QueryByExampleExecutor<T> {

  <S extends T> S findOne(Example<S> example);

  <S extends T> Iterable<S> findAll(Example<S> example);

  // … more functionality omitted.
}
```

##### Example Matcher

示例不限于默认设置。你可以通过使用 `ExampleMatcher` 为字符串匹配、null处理和特定属性设置指定你自己的默认值，如下面的例子所示。

Example 111. Example matcher with customized matching

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

Example 112. 配置 matcher 属性

```java
ExampleMatcher matcher = ExampleMatcher.matching()
  .withMatcher("firstname", endsWith())
  .withMatcher("lastname", startsWith().ignoreCase());
}
```

配置 matcher 选项的另一种方法是使用 `lambda` （在Java 8中引入）。这种方法创建一个回调，要求实现者修改matcher。你不需要返回matcher，因为配置选项被保存在matcher实例中。下面的例子显示了一个使用lambda的matcher。

Example 113. 使用 lambda 配置 matcher option

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

##### Fluent API

`QueryByExampleExecutor` 还提供了一个方法，我们到目前为止还没有提到。`<S extends T, R> R findBy(Example<S> example, Function<FluentQuery.FetchableFluentQuery<S>, R> queryFunction)` 。和其他方法一样，它执行一个从 `Example` 派生的查询。然而，通过第二个参数，你可以控制该执行的各个方面，否则你无法动态地控制。你可以通过调用第二个参数中的 `FetchableFluentQuery` 的各种方法来做到这一点。 `sortBy` 让你为你的结果指定一个排序。`as` 让你指定你希望结果被转换的类型。`project` 限制了被查询的属性。`first`, `firstValue`, `one`, `oneValue`, `all`, `page`, `stream`, `count`, 和 `exists` 定义了你得到什么样的结果以及当超过预期结果数量时查询的行为方式。

Example 114. 使fluent API来获得可能是许多结果中的最后一个，按lastname排序。

```java
Optional<Person> match = repository.findBy(example,
    q -> q
        .sortBy(Sort.by("lastname").descending())
        .first()
);
```

##### 运行 Example

在Spring Data JPA中，你可以使用 Repository 的Query by Example，如以下例子所示。

Example 115. 使用 Repository 的 Example 查询

```java
public interface PersonRepository extends JpaRepository<Person, String> { … }

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

属性指定器（property specifier）接受属性名称（如 `firstname` 和 `lastname`）。你可以通过将属性用点连接起来进行导航（`address.city`）。你还可以用匹配选项和大小写敏感性来调整它。

下表显示了你可以使用的各种 `StringMatcher` 选项以及在一个名为 `firstname` 的字段上使用这些选项的结果。

| Matching                    | Logical result                                |
| :-------------------------- | :-------------------------------------------- |
| `DEFAULT` (区分大小写)      | `firstname = ?0`                              |
| `DEFAULT` (不区分大小写)    | `LOWER(firstname) = LOWER(?0)`                |
| `EXACT` (区分大小写)        | `firstname = ?0`                              |
| `EXACT` (不区分大小写)      | `LOWER(firstname) = LOWER(?0)`                |
| `STARTING` (区分大小写)     | `firstname like ?0 + '%'`                     |
| `STARTING` (不区分大小写)   | `LOWER(firstname) like LOWER(?0) + '%'`       |
| `ENDING` (区分大小写)       | `firstname like '%' + ?0`                     |
| `ENDING` (不区分大小写)     | `LOWER(firstname) like '%' + LOWER(?0)`       |
| `CONTAINING` (区分大小写)   | `firstname like '%' + ?0 + '%'`               |
| `CONTAINING` (不区分大小写) | `LOWER(firstname) like '%' + LOWER(?0) + '%'` |

#### 5.1.7. 事务

默认情况下，从 [`SimpleJpaRepository`](https://docs.spring.io/spring-data/data-jpa/docs/current/api/org/springframework/data/jpa/repository/support/SimpleJpaRepository.html) 继承的 repository 实例上的 CRUD 方法是事务性的。对于读操作，事务配置的 `readOnly` 标志被设置为 `true`。所有其他的都被配置为普通的 `@Transactional`，所以默认的事务配置也适用。由事务性 repository 片段支持的 repository 方法继承了实际片段方法的事务性属性。

如果你需要调整 repository 中声明的某个方法的事务配置，可以在你的 repository 接口中重新声明该方法，如下所示。

Example 116. 用于CRUD的自定义事务配置

```java
public interface UserRepository extends CrudRepository<User, Long> {

  @Override
  @Transactional(timeout = 10)
  public List<User> findAll();

  // Further query method declarations
}
```

这样做会导致 `findAll()` 方法以10秒的超时运行，并且没有 `readOnly` 标志。

另一种改变事务行为的方式是使用一个 facade 或 service 实现，（通常）涵盖一个以上的 repository。它的目的是为非 CRUD 操作定义事务性的边界。下面的例子展示了如何为一个以上的 repository 使用这样的 facade。

Example 117. 使用facade来定义多个repository调用的事务

```java
@Service
public class UserManagementImpl implements UserManagement {

  private final UserRepository userRepository;
  private final RoleRepository roleRepository;

  public UserManagementImpl(UserRepository userRepository,
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
}
```

这个例子导致对 `addRoleToAllUsers(…)` 的调用在一个事务中运行（参与到一个现有的事务中，或者在没有事务的情况下创建一个新事务）。repository 的事务配置就被忽略了，因为外部的事务配置决定了实际使用的配置。请注意，你必须激活 `<tx:annotation-driven />` 或明确使用 `@EnableTransactionManagement`，才能让基于注解的外设配置发挥作用。这个例子假设你使用组件扫描。

请注意，从JPA的角度来看，对 `save` 的调用并不是严格必要的，但为了与Spring Data提供的 repository 抽象保持一致，还是应该存在的。

##### 事务性的查询方法

要让你的查询方法是事务性的，在你定义的 repository 接口处使用 `@Transactional`，如下例所示。

Example 118. 在查询方法中使用 @Transactional

```java
@Transactional(readOnly = true)
interface UserRepository extends JpaRepository<User, Long> {

  List<User> findByLastname(String lastname);

  @Modifying
  @Transactional
  @Query("delete from User u where u.active = false")
  void deleteInactiveUsers();
}
```

通常情况下，你希望 `readOnly` 标志被设置为 `true`，因为大多数的查询方法只读数据。与此相反， `deleteInactiveUsers()` 使用了 `@Modifying` 注解并覆盖了事务配置。因此，该方法在运行时，`readOnly` 标志被设置为 `false`。

|      | 你可以将事务用于只读查询，并通过设置 `readOnly` 标志将其标记为只读。然而，这样做并不是为了检查你是否触发了操作性查询（尽管有些数据库在只读事务中拒绝 `INSERT` 和 `UPDATE` 语句）。`readOnly` 标志是作为一种提示传播给底层的JDBC驱动程序，以便进行性能优化。此外，Spring对底层JPA提供者进行了一些优化。例如，当与Hibernate一起使用时，当你将事务配置为 `readOnly` 时，flush mode 被设置为 `NEVER`，这使得Hibernate跳过脏（dirty）检查（在大型对象树上有明显的改进）。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 5.1.8. 锁

为了指定要使用的锁模式，你可以在查询方法上使用 `@Lock` 注解，如下例所示。

Example 119. 在查询方法上定义锁的元数据

```java
interface UserRepository extends Repository<User, Long> {

  // Plain query method
  @Lock(LockModeType.READ)
  List<User> findByLastname(String lastname);
}
```

这个方法声明导致被触发的查询配备了 `READ` 的 `LockModeType`。你也可以通过在你的 repository 接口中重新声明 CRUD 方法并添加 `@Lock` 注解来定义锁，如下面例子所示。

Example 120. 在CRUD方法上定义锁元数据

```java
interface UserRepository extends Repository<User, Long> {

  // Redeclaration of a CRUD method
  @Lock(LockModeType.READ)
  List<User> findAll();
}
```

#### 5.1.9. 审计

##### 基础

Spring Data提供了复杂的支持，可以透明地跟踪谁创建或更改了实体以及更改发生的时间。为了从该功能中获益，你必须为你的实体类配备审计元数据，这些元数据可以使用注解或实现接口来定义。此外，审计必须通过注解配置或XML配置来启用，以注册所需的基础设施组件。关于配置样本，请参考特定store部分。

|      | 只跟踪创建和修改日期的应用程序不需要使其实体实现 [`AuditorAware`](https://springdoc.cn/spring-data-jpa/#auditing.auditor-aware)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

###### 基于注解的审计元数据

我们提供 `@CreatedBy` 和 `@LastModifiedBy` 来捕获创建或修改实体的用户，以及 `@CreatedDate` 和 `@LastModifiedDate` 来捕获变化发生的时间。

Example 121. An audited entity

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

Example 122. 嵌入实体中的审计元数据

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

###### 基于接口的审计元数据

如果你不想使用注解来定义审计元数据，你可以让你的domain类实现 `Auditable` 接口。它为所有的审计属性暴露了 `setter` 方法。

###### `AuditorAware`

如果你使用 `@CreatedBy` 或 `@LastModifiedBy`，审计基础设施需要以某种方式知道当前的principal。为此，我们提供了一个 `AuditorAware<T>` SPI接口，你必须实现这个接口来告诉基础设施谁是与应用程序交互的当前用户或系统。泛型 `T` 定义了用 `@CreatedBy` 或 `@LastModifiedBy` 注解的属性必须是什么类型。

下面的例子显示了一个使用Spring Security的 `Authentication` 对象的接口实现。

Example 123. 基于 Spring Security 的 `AuditorAware` 的实现

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

###### `ReactiveAuditorAware`

当使用响应式基础设施时，你可能想利用上下文（Context）信息来提供 `@CreatedBy` 或 `@LastModifiedBy` 信息。我们提供了一个 `ReactiveAuditorAware<T>` SPI接口，你必须实现这个接口来告诉基础设施谁是当前与应用程序交互的用户或系统。泛型 `T` 定义了用 `@CreatedBy` 或 `@LastModifiedBy` 注释的属性必须是什么类型。

下面的例子显示了一个接口的实现，它使用了Spring Security的 `Authentication` 对象。

Example 124. 基于 Spring Security 的 `ReactiveAuditorAware` 的实现

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

还有一个方便的基类，`AbstractAuditable`，你可以扩展它以避免手动实现接口方法。这样做会增加你的domain类与Spring Data的耦合度，这可能是你想避免的事情。通常情况下，基于注解的定义审计元数据的方式更受欢迎，因为它的侵入性更小，也更灵活。

#### 5.1.10. JPA 审计

##### 一般的审计配置

Spring Data JPA 提供了一个实体监听器（entity listener），可以用来触发审计信息的捕获。首先，你必须注册 `AuditingEntityListener`，以便在你的 `orm.xml` 文件中为持久化上下文中的所有实体使用，如下面的例子所示。

Example 125. 审计配置 orm.xml

```xml
<persistence-unit-metadata>
  <persistence-unit-defaults>
    <entity-listeners>
      <entity-listener class="….data.jpa.domain.support.AuditingEntityListener" />
    </entity-listeners>
  </persistence-unit-defaults>
</persistence-unit-metadata>
```

你也可以通过使用 `@EntityListeners` 注解在每个实体的基础上启用 `AuditingEntityListener`，如下所示。

```java
@Entity
@EntityListeners(AuditingEntityListener.class)
public class MyEntity {

}
```

|      | 审计功能需要添加 `spring-aspects.jar`。 |
| ---- | --------------------------------------- |
|      |                                         |

在对 `orm.xml` 进行适当修改，并在classpath上添加 `spring-aspects.jar` 后，激活审计功能只需在配置中添加Spring Data JPA `auditing` 命名空间元素，如下所示。

Example 126. 使用XML配置激活审计

```xml
<jpa:auditing auditor-aware-ref="yourAuditorAwareBean" />
```

从Spring Data JPA 1.5开始，你可以通过给配置类加上 `@EnableJpaAuditing` 注解来启用审计。你仍然必须修改 `orm.xml` 文件，并且在 classpath 上有 `spring-aspects.jar`。下面的例子展示了如何使用 `@EnableJpaAuditing` 注解。

Example 127. 用Java配置激活审计

```java
@Configuration
@EnableJpaAuditing
class Config {

  @Bean
  public AuditorAware<AuditableUser> auditorProvider() {
    return new AuditorAwareImpl();
  }
}
```

如果你将 `AuditorAware` 类型的Bean暴露给 `ApplicationContext`，审计基础设施会自动拾取它并使用它来确定要在 domain 类型上设置的当前用户。如果你在 `ApplicationContext` 中注册了多个实现，你可以通过明确设置 `@EnableJpaAuditing` 的 `auditorAwareRef` 属性来选择要使用的一个。

### 5.2. 其他考虑因素

#### 5.2.1. 在自定义实现中使用 `JpaContext`

当使用多个 `EntityManager` 实例和 [自定义 repository 实现](https://springdoc.cn/spring-data-jpa/#repositories.custom-implementations) 时，你需要将正确的 `EntityManager` 接入 repository 实现类。你可以通过在 `@PersistenceContext` 注解中明确命名 `EntityManager` 来做到这一点，如果 `EntityManager` 是 `@Autowired`，则可以使用 `@Qualifier`。

从Spring Data JPA 1.9开始，Spring Data JPA包括一个名为 `JpaContext` 的类，它可以让你通过管理domain类获得 `EntityManager`，假设它只由应用程序中的一个 `EntityManager` 实例管理。下面的例子展示了如何在一个自定义的 repository 中使用 `JpaContext`。

Example 128. 在自定义存储库的实现中使用 `JpaContext'。

```java
class UserRepositoryImpl implements UserRepositoryCustom {

  private final EntityManager em;

  @Autowired
  public UserRepositoryImpl(JpaContext context) {
    this.em = context.getEntityManagerByManagedType(User.class);
  }

  …
}
```

这种方法的优点是，如果 domain 类型被分配给不同的持久化单元（persistence unit），repository 不必被触动以改变对持久化单元的引用。

#### 5.2.2. 合并持久化单元（persistence unit）

Spring支持拥有多个持久化单元。然而，有时你可能想将你的应用程序模块化，但仍要确保所有这些模块在一个持久化单元内运行。为了实现这种行为，Spring Data JPA提供了一个 `PersistenceUnitManager` 实现，它可以根据名字自动合并持久化单元，如下面的例子所示。

Example 129. 使用 MergingPersistenceUnitmanager

```xml
<bean class="….LocalContainerEntityManagerFactoryBean">
  <property name="persistenceUnitManager">
    <bean class="….MergingPersistenceUnitManager" />
  </property>
</bean>
```

##### 对 @Entity 类和JPA映射文件进行类路径扫描

一个普通的JPA设置要求所有注解映射的实体类被列在 `orm.xml` 中。这同样适用于XML映射文件。Spring Data JPA提供了一个 `ClasspathScanningPersistenceUnitPostProcessor`，它获得了一个配置好的 base package，并可选择接受一个映射文件名模式。然后，它在给定的包中扫描带有 `@Entity` 或 `@MappedSuperclass` 注解的类，加载符合文件名模式的配置文件，并将它们交给JPA配置。后处理程序必须按以下方式配置。

Example 130. 使用 ClasspathScanningPersistenceUnitPostProcessor

```xml
<bean class="….LocalContainerEntityManagerFactoryBean">
  <property name="persistenceUnitPostProcessors">
    <list>
      <bean class="org.springframework.data.jpa.support.ClasspathScanningPersistenceUnitPostProcessor">
        <constructor-arg value="com.acme.domain" />
        <property name="mappingFileNamePattern" value="**/*Mapping.xml" />
      </bean>
    </list>
  </property>
</bean>
```

|      | 从Spring 3.1开始，可以直接在 `LocalContainerEntityManagerFactoryBean` 上配置要扫描的包，以启用实体类的 classpath 扫描。详见 [JavaDoc](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/orm/jpa/LocalContainerEntityManagerFactoryBean.html#setPackagesToScan(java.lang.String...))。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 5.2.3. CDI 整合

repository 接口的实例通常由一个容器来创建，在使用Spring Data时，Spring是最自然的选择。Spring为创建Bean实例提供了复杂的支持，如 [创建 Repository 实例](https://springdoc.cn/spring-data-jpa/#repositories.create-instances) 中所记载的。从1.1.0版本开始，Spring Data JPA带有一个自定义的CDI扩展，允许在CDI环境中使用 repository 抽象。该扩展是JAR的一部分。要激活它，请在你的classpath上包含 Spring Data JPA JAR。

现在你可以通过为 `EntityManagerFactory` 和 `EntityManager` 实现一个 CDI Producer 来设置基础设施，如下面的例子所示

```java
class EntityManagerFactoryProducer {

  @Produces
  @ApplicationScoped
  public EntityManagerFactory createEntityManagerFactory() {
    return Persistence.createEntityManagerFactory("my-persistence-unit");
  }

  public void close(@Disposes EntityManagerFactory entityManagerFactory) {
    entityManagerFactory.close();
  }

  @Produces
  @RequestScoped
  public EntityManager createEntityManager(EntityManagerFactory entityManagerFactory) {
    return entityManagerFactory.createEntityManager();
  }

  public void close(@Disposes EntityManager entityManager) {
    entityManager.close();
  }
}
```

必要的设置会根据 JavaEE 环境的不同而不同。你可能只需要把一个 `EntityManager` 重新声明为CDI Bean，如下所示。

```java
class CdiConfig {

  @Produces
  @RequestScoped
  @PersistenceContext
  public EntityManager entityManager;
}
```

在前面的例子中，容器必须有能力自己创建JPA `EntityManagers`。配置所做的就是把JPA `EntityManager` 作为CDI Bean重新输出。

Spring Data JPA CDI扩展将所有可用的 `EntityManager` 实例作为 CDI Bean，并在容器请求某个 repository 类型的 bean 时为 Spring Data Repository 创建一个代理。因此，获取Spring Data Repository 的实例只需声明一个 `@Injected` 属性，如下面的例子所示。

```java
class RepositoryClient {

  @Inject
  PersonRepository repository;

  public void businessMethod() {
    List<Person> people = repository.findAll();
  }
}
```

### 5.3. Spring Data Envers

#### 5.3.1. Spring Data Envers 是什么？

Spring Data Envers 使典型的 Envers 查询在Spring Data JPA的 repository 中可用。它与其他Spring Data 模块的不同之处在于，它总是与另一个 Spring Data 模块结合使用：Spring Data JPA。

#### 5.3.2. Envers 是什么？

Envers是一个 [Hibernate模块](https://hibernate.org/orm/envers/)，为JPA实体增加了审计功能。本文档假设你熟悉 Envers，就像 Spring Data Envers 依赖于 Envers 的正确配置。

#### 5.3.3. 配置

作为使用Spring Data Envers的起点，你需要一个在 classpath 上有Spring Data JPA的项目和一个额外的 `spring-data-envers` 依赖。

```xml
<dependencies>

  <!-- other dependency elements omitted -->

  <dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-envers</artifactId>
    <version>3.1.0-SNAPSHOT</version>
  </dependency>

</dependencies>
```

这也将 `hibernate-envers` 作为一个临时（transient）的依赖项带入项目。

为了启用Spring Data Envers和Spring Data JPA，我们需要配置两个bean和一个特殊的 `repositoryFactoryBeanClass`。

```java
@Configuration
@EnableEnversRepositories
@EnableTransactionManagement
public class EnversDemoConfiguration {

	@Bean
	public DataSource dataSource() {

		EmbeddedDatabaseBuilder builder = new EmbeddedDatabaseBuilder();
		return builder.setType(EmbeddedDatabaseType.HSQL).build();
	}

	@Bean
	public LocalContainerEntityManagerFactoryBean entityManagerFactory() {

		HibernateJpaVendorAdapter vendorAdapter = new HibernateJpaVendorAdapter();
		vendorAdapter.setGenerateDdl(true);

		LocalContainerEntityManagerFactoryBean factory = new LocalContainerEntityManagerFactoryBean();
		factory.setJpaVendorAdapter(vendorAdapter);
		factory.setPackagesToScan("example.springdata.jpa.envers");
		factory.setDataSource(dataSource());
		return factory;
	}

	@Bean
	public PlatformTransactionManager transactionManager(EntityManagerFactory entityManagerFactory) {

		JpaTransactionManager txManager = new JpaTransactionManager();
		txManager.setEntityManagerFactory(entityManagerFactory);
		return txManager;
	}
}
```

要实际使用 Spring Data Envers，要把一个或多个 Repository 作为扩展接口添加到 {spring-data-commons-javadoc-base}/org/springframework/data/repository/history/RevisionRepository.html[`RevisionRepository`] 中。

```java
interface PersonRepository
    extends CrudRepository<Person, Long>,
    RevisionRepository<Person, Long, Long> 
{}
```

|      | 第一个类型参数（`Person`）表示实体类型，第二个（`Long`）表示id属性的类型，最后一个（`Long`）是 revision number 的类型。对于默认配置中的 Envers，revision number 参数应该是 `Integer` 或 `Long`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

该 repository 的实体必须是启用了 Envers 审计的实体（也就是说，它必须有一个 `@Audited` 注解）。

```java
@Entity
@Audited
class Person {

	@Id @GeneratedValue
	Long id;
	String name;
	@Version Long version;
}
```

#### 5.3.4. 用法

现在你可以使用 `RevisionRepository` 的方法来查询实体的 revision，如下测试案例所示。

```java
@ExtendWith(SpringExtension.class)
@Import(EnversDemoConfiguration.class) 
class EnversIntegrationTests {

	final PersonRepository repository;
	final TransactionTemplate tx;

	EnversIntegrationTests(@Autowired PersonRepository repository, @Autowired PlatformTransactionManager tm) {
		this.repository = repository;
		this.tx = new TransactionTemplate(tm);
	}

	@Test
	void testRepository() {

		Person updated = preparePersonHistory();

		Revisions<Long, Person> revisions = repository.findRevisions(updated.id);

		Iterator<Revision<Long, Person>> revisionIterator = revisions.iterator();

		checkNextRevision(revisionIterator, "John", RevisionType.INSERT);
		checkNextRevision(revisionIterator, "Jonny", RevisionType.UPDATE);
		checkNextRevision(revisionIterator, null, RevisionType.DELETE);
		assertThat(revisionIterator.hasNext()).isFalse();

	}

	/**
    * Checks that the next element in the iterator is a Revision entry referencing a Person
    * with the given name after whatever change brought that Revision into existence.
    * <p>
    * As a side effect the Iterator gets advanced by one element.
    *
    * @param revisionIterator the iterator to be tested.
    * @param name the expected name of the Person referenced by the Revision.
    * @param revisionType the type of the revision denoting if it represents an insert, update or delete.
    */
	private void checkNextRevision(Iterator<Revision<Long, Person>> revisionIterator, String name,
			RevisionType revisionType) {

		assertThat(revisionIterator.hasNext()).isTrue();
		Revision<Long, Person> revision = revisionIterator.next();
		assertThat(revision.getEntity().name).isEqualTo(name);
		assertThat(revision.getMetadata().getRevisionType()).isEqualTo(revisionType);
	}

	/**
    * Creates a Person with a couple of changes so it has a non-trivial revision history.
    * @return the created Person.
    */
	private Person preparePersonHistory() {

		Person john = new Person();
		john.setName("John");

		// create
		Person saved = tx.execute(__ -> repository.save(john));
		assertThat(saved).isNotNull();

		saved.setName("Jonny");

		// update
		Person updated = tx.execute(__ -> repository.save(saved));
		assertThat(updated).isNotNull();

		// delete
		tx.executeWithoutResult(__ -> repository.delete(updated));
		return updated;
	}
}
```

|      | 这引用了前面介绍的应用环境配置（在 [配置](https://springdoc.cn/spring-data-jpa/#envers.configuration) 部分）。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 5.3.5. 其他资源

你可以在 [Spring Data实例库](https://github.com/spring-projects/spring-data-examples) 中下载Spring Data Envers的例子，并进行玩耍以了解该库的工作原理。

你也应该看看 {spring-data-commons-javadoc-base}/org/springframework/data/repository/history/RevisionRepository.html[`RevisionRepository` 和相关类的 Javadoc]。

你可以使用 `spring-data-envers` 标签在 [Stackoverflow](https://stackoverflow.com/questions/tagged/spring-data-envers) 提问。

Spring Data Envers 的源代码和 issue tracker 托管在 [GitHub](https://github.com/spring-projects/spring-data-envers)。

## 6. 附录

## Appendix A: 命名空间参考

### `<repositories />` 元素

`<repositories />` 元素触发了Spring Data资源库基础设施的设置。最重要的属性是 `base-package`，它定义了扫描 Spring Data Repository 接口的包。参见 “[XML 配置](https://springdoc.cn/spring-data-jpa/#repositories.create-instances.xml)”。下表描述了 `<repositories />` 元素的属性。

| Name                           | Description                                                  |
| :----------------------------- | :----------------------------------------------------------- |
| `base-package`                 | 定义在自动检测模式下扫描扩展 `*Repository`（实际接口由特定的Spring Data模块决定）的 repository 接口的包。在配置的包下面的所有包也会被扫描到。允许使用通配符。 |
| `repository-impl-postfix`      | 定义用于自动检测自定义 repository 实现的后缀。名称以配置的后缀结尾的类被认为是候选类。默认为 `Impl`。 |
| `query-lookup-strategy`        | 确定用于创建 finder 查询的策略。详见 “[Query 的查询策略](https://springdoc.cn/spring-data-jpa/#repositories.query-methods.query-lookup-strategies)” 。默认为 `create-if-not-found`。 |
| `named-queries-location`       | 定义了搜索包含外部定义的查询的属性（Properties）文件的位置。 |
| `consider-nested-repositories` | 是否应该考虑嵌套的 repository 接口定义。默认为 `false`。     |

## Appendix B: Populators 命名空间参考

### <populator /> 元素

`<populator />` 元素允许通过Spring Data Repository 基础设施填充data store。[[1](https://springdoc.cn/spring-data-jpa/#_footnotedef_1)]

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
| 实现 `Streamable` 并接受 `Streamable` 构造函数或工厂方法参数的类型 | 暴露了一个构造函数或 `…of(…)`/`…valueOf(…)` 工厂方法的类型，以 `Streamable` 作为参数。详见 [返回自定义的 Streamable Wrapper 类型](https://springdoc.cn/spring-data-jpa/#repositories.collections-and-iterables.streamable-wrapper)。 |
| Vavr `Seq`, `List`, `Map`, `Set`                             | Vavr集合类型。详见 [支持 Vavr 集合](https://springdoc.cn/spring-data-jpa/#repositories.collections-and-iterables.vavr)。 |
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

## Appendix E: 常见问题

### 普通的

1. *我想获得更详细的日志信息，比如说 `JpaRepository` 里面调用了哪些方法。我怎样才能获得这些信息呢？*

   你可以利用Spring提供的 `CustomizableTraceInterceptor`，如以下例子所示。

   ```xml
   <bean id="customizableTraceInterceptor" class="
     org.springframework.aop.interceptor.CustomizableTraceInterceptor">
     <property name="enterMessage" value="Entering $[methodName]($[arguments])"/>
     <property name="exitMessage" value="Leaving $[methodName](): $[returnValue]"/>
   </bean>
   
   <aop:config>
     <aop:advisor advice-ref="customizableTraceInterceptor"
       pointcut="execution(public * org.springframework.data.jpa.repository.JpaRepository+.*(..))"/>
   </aop:config>
   ```

### 审计

1. *我想使用Spring Data的JPA审计功能，但我的数据库已经被配置为在实体上设置修改和创建日期。我怎样才能防止Spring Data以编程方式设置日期？*

   将 `auditing` 命名空间元素的 `set-dates` 属性设置为 `false`。

## Appendix F: 术语表

- AOP

  面向切面编程

- Commons DBCP

  Commons DataBase Connection Pools - 一个来自Apache基金会的库，提供了数据源接口的池化实现。

- CRUD

  创建（Create）、读取（Read）、更新（Update）、删除（Delete ） - 基本持久化操作。

- DAO

  数据访问对象（Data Access Object） - 将持久化逻辑与要持久化的对象分离的模式。

- Dependency Injection

  模式，将一个组件的依赖关系从外部交给该组件，使该组件可以自己查找依赖关系。更多信息，见 https://en.wikipedia.org/wiki/Dependency_Injection。

- EclipseLink

  实现JPA的对象关系映射器 - https://www.eclipse.org/eclipselink/

- Hibernate

  实现JPA的对象关系映射器 - https://hibernate.org/

- JPA

  Jakarta Persistence（持久化） API

- Spring

  Java 应用框架 - https://spring.io/projects/spring-framework

------

[1](https://springdoc.cn/spring-data-jpa/#_footnoteref_1). 见 [XML 配置](https://springdoc.cn/spring-data-jpa/#repositories.create-instances.xml)

Version 3.1.0-SNAPSHOT
Last updated 2023-09-10 14:07:03 +0800

[主页](https://springdoc.cn/docs/)