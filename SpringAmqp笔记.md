# Spring AMQP 中文文档

Spring AMQP是一个基于AMQP协议的消息中间件框架，它提供了一个简单的API来发送和接收异步、可靠的消息。它是Spring框架的一部分，可以与Spring Boot和其他Spring项目一起使用。Spring AMQP支持多种消息协议，包括RabbitMQ、Apache ActiveMQ和Qpid等。它提供了一个高级的消息模型，包括消息确认、事务和消息监听器等功能，使得开发者可以轻松地编写可靠的消息应用程序。Spring AMQP还提供了一些高级特性，如消息转换器、消息路由、消息过滤和消息拦截等。这些特性可以帮助开发者更加灵活地处理不同类型的消息，同时也提高了应用程序的性能和可靠性。总之，Spring AMQP是一个强大的消息中间件框架，它可以帮助开发者轻松地构建可靠的消息应用程序，并提供了许多高级特性来满足不同的需求。

- [1. 前言](https://springdoc.cn/spring-amqp/#前言)
- [2. 最新动态](https://springdoc.cn/spring-amqp/#whats-new)
- [3. 简介](https://springdoc.cn/spring-amqp/#简介)
- [4. 参考文档](https://springdoc.cn/spring-amqp/#参考文档)
- [5. Spring Integration - 参考](https://springdoc.cn/spring-amqp/#spring-integration-参考)
- [6. 其他资源](https://springdoc.cn/spring-amqp/#resources)
- [Appendix A: Micrometer Observation 文档](https://springdoc.cn/spring-amqp/#observation-gen)
- [Appendix B: 原生镜像](https://springdoc.cn/spring-amqp/#native-images)
- [Appendix C: 更改历史](https://springdoc.cn/spring-amqp/#change-history)

**3.0.4-SNAPSHOT**

|      | 本站([springdoc.cn](https://springdoc.cn/))中的内容来源于 [spring.io](https://spring.io/) ，原始版权归属于 [spring.io](https://spring.io/)。由 [springboot.io - Spring Boot中文社区](https://springboot.io/) 进行翻译，整理。可供个人学习、研究，未经许可，不得进行任何转载、商用或与之相关的行为。 商标声明：Spring 是 Pivotal Software, Inc. 在美国以及其他国家的商标。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

## 1. 前言

Spring AMQP 项目将 Spring 的核心概念应用于基于 AMQP 的消息传递解决方案的开发。我们提供一个 “template” 作为发送和接收消息的高级抽象。我们还提供了对消息驱动的POJO的支持。这些库促进了 AMQP 资源的管理，同时促进了依赖注入和声明式配置的使用。在所有这些情况下，你可以看到与 Spring 框架中的 JMS 支持的相似之处。有关其他项目的信息，请访问Spring AMQP项目 [主页](https://projects.spring.io/spring-amqp/)。

## 2. 最新动态

### 2.1. 3.0中自2.4以来的变化

#### 2.1.1. Java 17, Spring Framework 6.0

这个版本需要Spring Framework 6.0和Java 17

#### 2.1.2. 远程

远程功能（使用RMI）已不再被支持。

#### 2.1.3. 观察

现在支持使用 `Micrometer` 对 timer 和追踪进行观察。更多信息见 [Micrometer 观察](https://springdoc.cn/spring-amqp/#micrometer-observation)。

#### 2.1.4. 原生镜像

提供了对创建原生镜像的支持。更多信息见 [原生镜像](https://springdoc.cn/spring-amqp/#native-images) 。

#### 2.1.5. AsyncRabbitTemplate

|      | `AsyncRabbitTemplate` 现在返回 `CompletableFuture`，而不是 `ListenableFuture`。更多信息参见 [Async Rabbit Template](https://springdoc.cn/spring-amqp/#async-template) 。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 2.1.6. Stream 支持的变化

|      | `RabbitStreamOperations` 和 `RabbitStreamTemplate` 方法现在返回 `CompletableFuture` 而不是 `ListenableFuture`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

现在支持 Super Stream 和其上的单一活动消费者（active consumer）。

更多信息见 [使用 RabbitMQ Stream 插件](https://springdoc.cn/spring-amqp/#stream-support)。

#### 2.1.7. `@RabbitListener` 的变化

Batch listener 现在可以消费 `Collection<?>` 以及 `List<?>`。batch messaging adapter 现在确保该方法适合于批量消费。当设置容器工厂 `consumerBatchEnabled` 为 `true` 时，`batchListener` 属性也被设置为 `true`。更多信息见 [带有批处理功能的 @RabbitListener](https://springdoc.cn/spring-amqp/#receiving-batch)。

`MessageConverter` 现在可以为 null 值返回 `Optional.empty()`；目前这是由 `Jackson2JsonMessageConverter` 实现的。更多信息见 [从 `Message` 转换](https://springdoc.cn/spring-amqp/#Jackson2JsonMessageConverter-from-message)。

你现在可以通过容器工厂而不是通过 `@RabbitListener` 上的属性来配置 `ReplyPostProcessor`。更多信息请参见 [回复管理](https://springdoc.cn/spring-amqp/#async-annotation-driven-reply)。

#### 2.1.8. Connection Factory 的变化

`AbstractConnectionFactory` 中默认的 `addressShuffleMode` 现在是 `RANDOM`。这导致在提供多个地址时，连接到一个随机的主机。更多信息见 [连接到群集](https://springdoc.cn/spring-amqp/#cluster) 。

`LocalizedQueueConnectionFactory` 不再使用 RabbitMQ `http-client` 库来确定哪个节点是一个队列的 leader。请参阅 [Queue 亲和性和 `LocalizedQueueConnectionFactory`](https://springdoc.cn/spring-amqp/#queue-affinity) 了解更多信息。

## 3. 简介

这篇参考文档的第一部分是对 Spring AMQP 和基础概念的高层次概述。它包括一些代码片段，以使你尽快启动和运行。

### 3.1. 快速教程

#### 3.1.1. 简介

这就是开始使用 Spring AMQP 的5分钟之旅。

先决条件：安装并运行 RabbitMQ broker（ https://www.rabbitmq.com/download.html）。然后抓取 spring-rabbit JAR 及其所有依赖项 - 最简单的方法是在你的构建工具中声明一个依赖项。例如，对于 Maven，你可以做类似于以下的事情：

```xml
<dependency>
  <groupId>org.springframework.amqp</groupId>
  <artifactId>spring-rabbit</artifactId>
  <version>3.0.4-SNAPSHOT</version>
</dependency>
```

对于Gradle，你可以做类似于以下的事情：

```groovy
compile 'org.springframework.amqp:spring-rabbit:3.0.4-SNAPSHOT'
```

##### 兼容性

最小的 Spring 框架依赖版本是 5.2.0。

最小的 `amqp-client` Java 客户端库版本是 5.7.0。

用于 stream queue 的最小 `stream-client` Java客户端库是 0.7.0。

##### 非常，非常快

本节提供了最快的介绍。

首先，添加下面的 `import` 语句，以使本节后面的例子能够发挥作用：

下面的例子使用普通的、命令式的Java来发送和接收一个消息：

```java
ConnectionFactory connectionFactory = new CachingConnectionFactory();
AmqpAdmin admin = new RabbitAdmin(connectionFactory);
admin.declareQueue(new Queue("myqueue"));
AmqpTemplate template = new RabbitTemplate(connectionFactory);
template.convertAndSend("myqueue", "foo");
String foo = (String) template.receiveAndConvert("myqueue");
```

请注意，在本地 Java Rabbit 客户端中也有一个 `ConnectionFactory`。我们在前面的代码中使用Spring的抽象。它缓存了 channel（和可选的连接）以便重复使用。我们依赖于 broker 中的 default exchange（因为在发送中没有指定），以及所有队列通过其名称与 default exchange 的默认绑定（因此，我们可以在发送中使用队列名称作为 routing （路由） key ）。这些行为是在AMQP规范中定义的。

##### 使用XML配置

下面的例子与前面的例子相同，但将资源配置外部化为XML：

```java
ApplicationContext context =
    new GenericXmlApplicationContext("classpath:/rabbit-context.xml");
AmqpTemplate template = context.getBean(AmqpTemplate.class);
template.convertAndSend("myqueue", "foo");
String foo = (String) template.receiveAndConvert("myqueue");
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:rabbit="http://www.springframework.org/schema/rabbit"
       xsi:schemaLocation="http://www.springframework.org/schema/rabbit
           https://www.springframework.org/schema/rabbit/spring-rabbit.xsd
           http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">

    <rabbit:connection-factory id="connectionFactory"/>

    <rabbit:template id="amqpTemplate" connection-factory="connectionFactory"/>

    <rabbit:admin connection-factory="connectionFactory"/>

    <rabbit:queue name="myqueue"/>

</beans>
```

默认情况下，`<rabbit:admin/>` 声明会自动寻找 `Queue`、`Exchange` 和 `Binding` 类型的 bean，并代表用户向 broker 声明它们。因此，你不需要在简单的 Java 驱动中明确使用该 Bean。在 XML schema 中，有很多选项可以配置组件的属性。你可以使用你的XML编辑器的自动补全功能来探索它们并查看它们的文档。

##### 使用Java配置

下面的例子重复了与前面的例子相同的例子，但用Java定义的外部配置：

```java
ApplicationContext context =
    new AnnotationConfigApplicationContext(RabbitConfiguration.class);
AmqpTemplate template = context.getBean(AmqpTemplate.class);
template.convertAndSend("myqueue", "foo");
String foo = (String) template.receiveAndConvert("myqueue");

........

@Configuration
public class RabbitConfiguration {

    @Bean
    public CachingConnectionFactory connectionFactory() {
        return new CachingConnectionFactory("localhost");
    }

    @Bean
    public RabbitAdmin amqpAdmin() {
        return new RabbitAdmin(connectionFactory());
    }

    @Bean
    public RabbitTemplate rabbitTemplate() {
        return new RabbitTemplate(connectionFactory());
    }

    @Bean
    public Queue myQueue() {
       return new Queue("myqueue");
    }
}
```

##### 使用Spring Boot自动配置和异步POJO监听器

Spring Boot 会自动配置基础设施Bean，如下例所示：

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public ApplicationRunner runner(AmqpTemplate template) {
        return args -> template.convertAndSend("myqueue", "foo");
    }

    @Bean
    public Queue myQueue() {
        return new Queue("myqueue");
    }

    @RabbitListener(queues = "myqueue")
    public void listen(String in) {
        System.out.println(in);
    }

}
```

## 4. 参考文档

参考文档的这一部分详细介绍了组成Spring AMQP的各种组件。[主要章节](https://springdoc.cn/spring-amqp/#amqp) 涵盖了开发AMQP应用程序的核心类。这一部分还包括关于 [示例应用程序](https://springdoc.cn/spring-amqp/#sample-apps) 的章节。

### 4.1. 使用 Spring AMQP

本章探讨了作为使用Spring AMQP开发应用程序的基本组件的接口和类。

#### 4.1.1. AMQP 抽象

Spring AMQP 由两个模块组成（每个模块在发行版中用一个JAR表示）：`spring-amqp` 和 `spring-rabbit`。'spring-amqp' 模块包含 `org.springframework.amqp.core` 包。在该包中，你可以找到代表 AMQP 核心 “model” 的类。我们的目的是提供通用抽象，不依赖于任何特定的 AMQP broker 实现或客户端库。终端用户的代码可以在不同厂商的实现中更加便携，因为它可以只针对抽象层进行开发。然后，这些抽象是由特定的 broker 模块实现的，如 'spring-rabbit'。目前只有一个 RabbitMQ 实现。然而，除了 RabbitMQ 之外，这些抽象已经在 .NET 中使用 Apache Qpid 进行了验证。由于 AMQP 在协议级别上运行，原则上，你可以将 RabbitMQ 客户端与任何支持相同协议版本的 broker 一起使用，但我们目前没有测试任何其他 broker。

本概述假定你已经熟悉了 AMQP 规范的基础知识。如果没有，请看一下 [其他资源](https://springdoc.cn/spring-amqp/#resources) 中列出的资源。

##### `Message`

0-9-1 AMQP规范并没有定义一个 `Message` 类或接口。相反，当执行诸如 `basicPublish()` 这样的操作时，内容被作为一个字节数组的参数传递，额外的 properties 被作为单独的参数传递进来。Spring AMQP定义了一个 `Message` 类，作为更普遍的 AMQP domain model 表示的一部分。 `Message` 类的目的是将 body 和 properties 封装在一个单一的实例中，这样反过来，API 就可以更简单。下面的例子显示了 `Message` 类的定义：

```java
public class Message {

    private final MessageProperties messageProperties;

    private final byte[] body;

    public Message(byte[] body, MessageProperties messageProperties) {
        this.body = body;
        this.messageProperties = messageProperties;
    }

    public byte[] getBody() {
        return this.body;
    }

    public MessageProperties getMessageProperties() {
        return this.messageProperties;
    }
}
```

`MessageProperties` 接口定义了几个常见的属性，如 'messageId'、'timestamp'、'contentType'，以及其他几个属性。你还可以通过调用 `setHeader(String key, Object value)` 方法用用户定义的 'header' 来扩展这些属性。

|      | 从 `1.5.7`、`1.6.11`、`1.7.4` 和 `2.0.0` 版本开始，如果一个 message body 是一个序列化的 `Serializable` 的java对象，在执行 `toString()` 操作（如在日志消息中）时，它不再被反序列化（默认）。这是为了防止不安全的反序列化。默认情况下，只有 `java.util` 和 `java.lang` 类被反序列化。要恢复到以前的行为，你可以通过调用 `Message.addAllowedListPatterns(…)` 来添加允许的类/包 pattern。支持一个简单的通配符，例如 `com.something.*, *.MyClass`。不能被反序列化的 body 在日志消息中用 `byte[<size>]` 表示。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### Exchange

`Exchange` 接口代表一个AMQP Exchange，它是消息生产者发送的对象。broker 的虚拟主机（virtual host）中的每个 Exchange 都有一个唯一的名字以及其他一些属性。下面的例子显示了 `Exchange` 的接口：

```java
public interface Exchange {

    String getName();

    String getExchangeType();

    boolean isDurable();

    boolean isAutoDelete();

    Map<String, Object> getArguments();

}
```

正如你所看到的，一个 `Exchange` 也有一个 'type'，由 `ExchangeTypes` 中定义的常量代表。基本的类型有：`direct`、`topic`、`fanout` 和 `headers`。在 core 包中，你可以找到这些类型中每个类型的 `Exchange` 接口的实现。这些 `Exchange` 类型的行为在它们如何处理与队列的绑定方面是不同的。例如，`Direct` exchange 允许队列被一个固定的路由key（通常是队列的名字）绑定。`Topic` exchange 支持与路由 pattern 的绑定，这些路由 pattern 可能包括 '*' 和 '#' 通配符，分别表示 '正好是一个' 和 '零或更多'。`Fanout` exchange 向所有与之绑定的队列发布，而不考虑任何路由 key。关于这些和其他 Exchange 类型的更多信息，见 [其他资源](https://springdoc.cn/spring-amqp/#resources)。

|      | AMQP 规范还要求任何 broker 提供一个没有名字的 “default” direct exchange。所有被声明的队列都被绑定到这个默认的 `Exchange` 上，并以其名称作为路由 key。你可以在 [`AmqpTemplate`](https://springdoc.cn/spring-amqp/#amqp-template) 中了解更多关于 default Exchange 在 Spring AMQP 中的用法。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### Queue

`Queue` 类代表了消息消费者接收消息的组件。像各种 `Exchange` 类一样，我们的实现旨在成为这个AMQP核心类型的抽象代表。下面的列表显示了 `Queue` 类：

```java
public class Queue  {

    private final String name;

    private volatile boolean durable;

    private volatile boolean exclusive;

    private volatile boolean autoDelete;

    private volatile Map<String, Object> arguments;

    /**
     * The queue is durable, non-exclusive and non auto-delete.
     *
     * @param name the name of the queue.
     */
    public Queue(String name) {
        this(name, true, false, false);
    }

    // Getters and Setters omitted for brevity

}
```

注意，构造函数需要队列名称（queue name）。根据不同的实现，admin template 可以提供生成唯一命名的队列的方法。这样的队列可以作为一个 “reply-to” 地址或在其他临时情况下很有用。出于这个原因，自动生成的队列的 `exclusive` 和 `autoDelete` 属性都将被设置为 `true`。

|      | 关于通过使用命名空间支持来声明队列的信息，包括队列参数，请参见 [配置 Broker](https://springdoc.cn/spring-amqp/#broker-configuration) 中关于队列的部分。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### Binding

鉴于生产者向 exchange 发送，消费者从队列（queue）中接收，连接队列和 exchange 的绑定对于通过消息传递连接这些生产者和消费者至关重要。在 Spring AMQP 中，我们定义了一个 `Binding` 类来表示这些连接。本节回顾了将队列与 exchange 绑定的基本选项。

你可以用一个固定的路由 key 将一个队列绑定到 `DirectExchange` 上，如下例所示：

```java
new Binding(someQueue, someDirectExchange, "foo.bar");
```

你可以用一个路由 pattern 将一个队列绑定到 `TopicExchange` 上，如下例所示：

```java
new Binding(someQueue, someTopicExchange, "foo.*");
```

你可以将一个队列绑定到一个没有路由 key 的 `FanoutExchange`，如下例所示：

```java
new Binding(someQueue, someFanoutExchange);
```

我们还提供了一个 `BindingBuilder` 来提供 “fluent API” 风格的操作，如下面的例子所示：

```java
Binding b = BindingBuilder.bind(someQueue).to(someTopicExchange).with("foo.*");
```

|      | 为了清楚起见，前面的例子显示的是 `BindingBuilder` 类，但这种风格在为 `bind()` 方法使用静态导入时效果很好。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

就其本身而言，`Binding` 类的一个实例只持有关于一个连接关系的数据。换句话说，它不是一个 "活动" 的组件。然而，正如你将在后面的 [配置 Broker](https://springdoc.cn/spring-amqp/#broker-configuration) 中看到的，`AmqpAdmin` 类可以使用 `Binding` 实例来实际触发 broker 上的绑定动作。另外，正如你在同一章节看到的，你可以通过在 `@Configuration` 类中使用 Spring 的 `@Bean` 注解来定义 `Binding` 实例。还有一个方便的基类，它进一步简化了生成 AMQP 相关 Bean 定义的方法，并识别了 queue、exchange 和 binding，以便它们在应用启动时都在 AMQP broker上被声明。

`AmqpTemplate` 也被定义在核心包中。作为参与实际 AMQP 消息传递的主要组件之一，它将在自己的章节中详细讨论（见 [`AmqpTemplate`](https://springdoc.cn/spring-amqp/#amqp-template)）。

#### 4.1.2. 连接和资源管理

我们在上一节中描述的 AMQP 模型是通用的，适用于所有实现，而当我们进入资源管理时，细节是特定于broker实现的。因此，在本节中，我们将重点讨论仅存在于我们的 “spring-rabbit” 模块中的代码，因为在这一点上，RabbitMQ 是唯一被支持的实现。

管理到 RabbitMQ broker 的连接的中心组件是 `ConnectionFactory` 接口。`ConnectionFactory` 实现的职责是提供 `org.springframework.amqp.rabbit.connection.Connection` 的实例，它是 `com.rabbitmq.client.Connection` 的包装器（wrapper）。

##### 选择一个 Connection Factory

有三种连接工厂可供选择：

- `PooledChannelConnectionFactory`
- `ThreadChannelConnectionFactory`
- `CachingConnectionFactory`

前两个是在2.3版本中添加的。

对于大多数使用情况，应该使用 `PooledChannelConnectionFactory`。如果你想确保严格的消息排序而不需要使用 [Scope 操作](https://springdoc.cn/spring-amqp/#scoped-operations)，可以使用 `ThreadChannelConnectionFactory`。如果你想使用相关的发布者确认，或者你想通过其 `CacheMode` 打开多个连接，就应该使用 `CachingConnectionFactory`。

三个工厂都支持简单的发布者确认。

在配置 `RabbitTemplate` 以使用 [单独的连接](https://springdoc.cn/spring-amqp/#separate-connection) 时，从 2.3.2 版开始，你现在可以将 publishing connection factory 配置为不同的类型。默认情况下，publishing factory 是同一类型，在 main factory 上设置的任何属性也会传播到 publishing factory。

###### `PooledChannelConnectionFactory`

这个 factory 管理着一个 connection 和两个 channel 池，基于 Apache Pool2。一个池用于事务 channel，另一个用于非事务性 channel。这些池是具有默认配置的 `GenericObjectPool`；提供了一个回调来配置这些池；更多信息请参考Apache文档。

Apache `commons-pool2` jar 必须在 classpath 上才能使用这个 factory。

```java
@Bean
PooledChannelConnectionFactory pcf() throws Exception {
    ConnectionFactory rabbitConnectionFactory = new ConnectionFactory();
    rabbitConnectionFactory.setHost("localhost");
    PooledChannelConnectionFactory pcf = new PooledChannelConnectionFactory(rabbitConnectionFactory);
    pcf.setPoolConfigurer((pool, tx) -> {
        if (tx) {
            // configure the transactional pool
        }
        else {
            // configure the non-transactional pool
        }
    });
    return pcf;
}
```

###### `ThreadChannelConnectionFactory`

该 factory 管理一个连接和两个 `ThreadLocal`，一个用于事务性 channel，另一个用于非事务性 channel。这个 factory 确保同一线程上的所有操作都使用同一个 channel（只要它保持开放）。这有利于严格的消息排序，而不需要使用 [Scope 操作](https://springdoc.cn/spring-amqp/#scoped-operations)。为了避免内存泄漏，如果你的应用程序使用了许多短命的线程，你必须调用 factory 的 `closeThreadChannel()` 来释放 channel 资源。从 2.3.7 版本开始，一个线程可以将其 channel 转移给另一个线程。更多信息请参见 [多线程环境下严格的消息排序](https://springdoc.cn/spring-amqp/#multi-strict) 。

###### `CachingConnectionFactory`

提供的第三个实现是 `CachingConnectionFactory`，默认情况下，它建立了一个可以被应用程序共享的单一连接broker。共享连接是可能的，因为AMQP的消息传递的 “unit of work” 实际上是一个 “channel”（在某些方面，这类似于JMS中连接和会话之间的关系）。连接实例提供了一个 `createChannel` 方法。`CachingConnectionFactory` 的实现支持对这些 channel 进行缓存，它根据 channel 是否是事务性的，为其维护单独的缓存。当创建 `CachingConnectionFactory` 的实例时，你可以通过构造函数提供 'hostname'。你还应该提供 'username' 和 'password' 属性。要配置 channel 缓存的大小（默认是25），你可以调用 `setChannelCacheSize()` 方法。

从1.3版本开始，你可以配置 `CachingConnectionFactory` 来缓存连接，也可以只缓存 channel。在这种情况下，对 `createConnection()` 的每次调用都会创建一个新的连接（或者从缓存中检索一个空闲的连接）。关闭一个连接会将其返回到缓存中（如果没有达到缓存的大小）。在这些连接上创建的 channel 也被缓存起来。在一些环境中，使用单独的连接可能是有用的，比如从 HA 集群中消费，与负载均衡器一起，连接到不同的集群成员，以及其他。要缓存连接，将 `cacheMode` 设置为 `CacheMode.CONNECTION`。

|      | 这并不限制连接的数量。相反，它指定了允许多少个空闲的开放连接。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

从 1.5.5 版本开始，提供了一个名为 `connectingLimit` 的新属性。当这个属性被设置时，它限制了允许的连接总数。当设置后，如果达到了限制，`channelCheckoutTimeLimit` 会被用来等待连接变得空闲。如果超过了这个时间，就会抛出一个 `AmqpTimeoutException`。

|      | 当 cache mode 为 `CONNECTION` 时，不支持队列和其他的自动声明（见 [Exchange、 Queue 和 Binding 的自动声明](https://springdoc.cn/spring-amqp/#automatic-declaration)）。另外，在写这篇文章的时候，`amqp-client` 库默认为每个连接创建一个固定的线程池（默认大小： `Runtime.getRuntime().availableProcessors() * 2` 个线程）。当使用大量的连接时，你应该考虑在 `CachingConnectionFactory` 上设置一个自定义的 `executor`。然后，所有连接都可以使用同一个 `executor`，其线程可以共享。`executor` 的线程池应该是无界的，或者根据预期的使用情况适当设置（通常，每个连接至少有一个线程）。如果在每个连接上创建了多个 channel，那么线程池的大小就会影响并发性，所以一个可变的（或简单的缓存）thread pool executor 是最合适的。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

重要的是要明白，缓存的大小（默认情况下）不是一个限制，而只是可以被缓存的 channel 数量。缓存的大小，比如说，10，任何数量的 channel 实际上都可以被使用。如果有超过10个 channel 被使用，并且它们都被送回了缓冲区，那么10个 channel 将被放入缓冲区。其余的则被物理关闭。

从1.6版本开始，默认的 channel 缓存大小已经从1增加到25。在大容量、多线程的环境中，一个小的缓存意味着 channel 的创建和关闭速度很高。增加默认的 cache size 可以避免这种开销。你应该通过 RabbitMQ 管理用户界面监控使用中的 channel ，如果你看到许多 channel 被创建和关闭，请考虑进一步增加缓存大小。缓存仅按需增长（以适应应用程序的并发性要求），因此这一更改不会影响现有的低容量应用程序。

从1.4.2版本开始，`CachingConnectionFactory` 有一个属性叫 `channelCheckoutTimeout`。当这个属性大于 0 时，`channelCacheSize` 成为一个连接上可以创建的 channel 数量的限制。如果达到限制，调用线程就会阻塞，直到有 channel 可用或达到这个超时，在这种情况下会抛出 `AmqpTimeoutException`。

|      | 在框架内使用的 channel（例如，`RabbitTemplate`）会可靠地返回到缓存中。如果你在框架之外创建 channel，（例如，通过直接访问连接并调用 `createChannel()`），你必须可靠地返回它们（通过关闭），也许是在一个 `finally` 块中，以避免 channel 逃逸。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

下面的例子显示了如何创建一个新的 `connection`：

```java
CachingConnectionFactory connectionFactory = new CachingConnectionFactory("somehost");
connectionFactory.setUsername("guest");
connectionFactory.setPassword("guest");

Connection connection = connectionFactory.createConnection();
```

当使用 XML 时，配置可能看起来像下面的例子：

```xml
<bean id="connectionFactory"
      class="org.springframework.amqp.rabbit.connection.CachingConnectionFactory">
    <constructor-arg value="somehost"/>
    <property name="username" value="guest"/>
    <property name="password" value="guest"/>
</bean>
```

|      | 还有一个 `SingleConnectionFactory` 的实现，只在框架的单元测试代码中可用。它比 `CachingConnectionFactory` 更简单，因为它不缓存 channel，但由于其缺乏性能和弹性，它并不打算用于简单测试之外的实际使用。如果你因为某些原因需要实现自己的 `ConnectionFactory`，`AbstractConnectionFactory` 基类可以提供一个很好的起点。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

通过使用 `rabbit` 命名空间，可以快速、方便地创建一个 `ConnectionFactory`，如下所示：

```xml
<rabbit:connection-factory id="connectionFactory"/>
```

在大多数情况下，这种方法是比较好的，因为框架可以为你选择最佳的默认值。创建的实例是一个 `CachingConnectionFactory`。请记住，channel 的默认缓存大小是 25。如果你想缓存更多的 channel，可以通过设置 `channelCacheSize` 属性来设置一个更大的值。在XML中，它看起来像如下：

```xml
<bean id="connectionFactory"
      class="org.springframework.amqp.rabbit.connection.CachingConnectionFactory">
    <constructor-arg value="somehost"/>
    <property name="username" value="guest"/>
    <property name="password" value="guest"/>
    <property name="channelCacheSize" value="50"/>
</bean>
```

另外，通过命名空间，你可以添加 `channel-cache-size` 属性，如下所示：

```xml
<rabbit:connection-factory
    id="connectionFactory" channel-cache-size="50"/>
```

默认的 cache mode 是 `CHANNEL`，但你可以把它配置为缓存 connection。在下面的例子中，我们使用 `connection-cache-size`：

```xml
<rabbit:connection-factory
    id="connectionFactory" cache-mode="CONNECTION" connection-cache-size="25"/>
```

你可以通过使用命名空间提供 `host` 和 `port` 属性，如下所示：

```xml
<rabbit:connection-factory
    id="connectionFactory" host="somehost" port="5672"/>
```

另外，如果在一个集群环境中运行，你可以使用 `addresses` 属性，如下所示：

```xml
<rabbit:connection-factory
    id="connectionFactory" addresses="host1:5672,host2:5672" address-shuffle-mode="RANDOM"/>
```

关于 `address-shuffle-mode` 的信息，请参见 [连接到群集](https://springdoc.cn/spring-amqp/#cluster)。

下面的例子有一个自定义的 thread factory，它将线程名称用 `rabbitmq-` 作为前缀：

```xml
<rabbit:connection-factory id="multiHost" virtual-host="/bar" addresses="host1:1234,host2,host3:4567"
    thread-factory="tf"
    channel-cache-size="10" username="user" password="password" />

<bean id="tf" class="org.springframework.scheduling.concurrent.CustomizableThreadFactory">
    <constructor-arg value="rabbitmq-" />
</bean>
```

##### AddressResolver

从 2.1.15 版本开始，你现在可以使用 `AddressResolver` 来解析连接地址。这将覆盖任何 `addresses` 和 `host/port` 属性的设置。

##### 命名连接

从 1.7 版开始，提供了一个 `ConnectionNameStrategy`，用于注入到 `AbstractionConnectionFactory` 中。生成的名称用于目标 RabbitMQ 连接的特定应用程序识别。如果 RabbitMQ 服务器支持，该连接名称将显示在管理 UI 中。此值不必是唯一的，并且不能用作连接标识符 - 例如，在 HTTP API 请求中。该值应该是人类可读的，是 `ClientProperties` 中 `connection_name` key 下的一部分。你可以使用一个简单的 Lambda，如下所示：

```java
connectionFactory.setConnectionNameStrategy(connectionFactory -> "MY_CONNECTION");
```

`ConnectionFactory` 参数可用于通过一些逻辑来区分目标连接名称。默认情况下， `AbstractConnectionFactory` 的 `beanName`、代表该对象的十六进制字符串和一个内部计数器被用来生成 `connection_name`。`<rabbit:connection-factory>` 命名空间组件也被提供了 `connection-name-strategy` 属性。

`SimplePropertyValueConnectionNameStrategy` 的实现将连接名称设置为一个 application property。你可以把它声明为一个 `@Bean`，并把它注入到 connection factory 中，如下例所示：

```java
@Bean
public SimplePropertyValueConnectionNameStrategy cns() {
    return new SimplePropertyValueConnectionNameStrategy("spring.application.name");
}

@Bean
public ConnectionFactory rabbitConnectionFactory(ConnectionNameStrategy cns) {
    CachingConnectionFactory connectionFactory = new CachingConnectionFactory();
    ...
    connectionFactory.setConnectionNameStrategy(cns);
    return connectionFactory;
}
```

该 property 必须存在于 application context 的 `Environment` 中。

|      | 当使用 Spring Boot 及其自动配置的 connection factory 时，你只需要声明 `ConnectionNameStrategy` `@Bean`。Boot 会自动检测 Bean 并将其接入 factory。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 堵塞的连接和资源限制

连接可能会被阻塞，无法与 [Memory Alarm](https://www.rabbitmq.com/memory.html) 对应的 broker 进行交互。从2.0版本开始，`org.springframework.amqp.rabbit.connection.Connection` 可以提供 `com.rabbitmq.client.BlockedListener` 实例，以获得连接阻塞和解阻塞事件的通知。此外，`AbstractConnectionFactory` 通过其内部的 `BlockedListener` 实现，分别发出 `ConnectionBlockedEvent` 和 `ConnectionUnblockedEvent` 事件。这些让你提供应用逻辑，对 broker 上的问题做出适当的反应，（例如）采取一些纠正措施。

|      | 当应用程序被配置为单一的 `CachingConnectionFactory` 时，就像 Spring Boot 自动配置中默认的那样，当连接被 Broker 阻塞时，应用程序就会停止工作。而当它被 Broker 阻塞时，它的任何客户端都会停止工作。如果我们在同一个应用程序中有生产者和消费者，那么当生产者阻塞连接（因为 Broker 上已经没有资源了）而消费者无法释放资源（因为连接被阻塞）时，我们可能会出现死锁。为了缓解这个问题，我们建议再建立一个具有相同选项的独立的 `CachingConnectionFactory` 实例，一个用于生产者，一个用于消费者。对于在消费者线程上执行的事务性生产者来说，单独的 `CachingConnectionFactory` 是不可能的，因为他们应该重复使用与消费者事务相关的 `Channel`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

从 2.0.2 版开始，`RabbitTemplate` 有一个配置选项，以自动使用第二个连接工厂，除非正在使用事务。请参阅 [使用一个单独的连接](https://springdoc.cn/spring-amqp/#separate-connection) 以了解更多信息。用于发布者连接的 `ConnectionNameStrategy` 与主策略（primary strategy）相同，并将 `.publisher` 附加到调用该方法的结果上。

从 1.7.7 版本开始，提供了一个 `AmqpResourceNotAvailableException`，当 `SimpleConnection.createChannel()` 不能创建一个 `Channel` 时（例如，因为达到 `channelMax` 的限制，并且缓存中没有可用的 channel），就会抛出这个异常。你可以在 `RetryPolicy` 中使用这个异常，以便在一些 back-off 后恢复操作。

##### 配置底层客户端连接工厂（Connection Factory）

`CachingConnectionFactory` 使用 Rabbit 客户端 `ConnectionFactory` 的一个实例。在设置 `CachingConnectionFactory` 上的等效属性时，一些配置属性被传递过去（例如，`host`、`port`、`userName`、`password`、 `requestedHeartBeat` 和 `connectionTimeout`）。要设置其他属性（例如 `clientProperties`），你可以定义 Rabbit 工厂的一个实例，并通过使用 `CachingConnectionFactory` 的适当构造函数提供对它的引用。当使用命名空间时（如 [前](https://springdoc.cn/spring-amqp/#connections) 所述），你需要在 `connection-factory` 属性中提供对配置的工厂的引用。为了方便起见，我们提供了一个工厂Bean，以协助在Spring application context 中配置连接工厂，这将在 [下一节](https://springdoc.cn/spring-amqp/#rabbitconnectionfactorybean-configuring-ssl) 讨论。

```xml
<rabbit:connection-factory
      id="connectionFactory" connection-factory="rabbitConnectionFactory"/>
```

|      | 4.0.x 客户端默认启用自动恢复功能。虽然与此功能兼容，但Spring AMQP有自己的恢复机制，一般不需要客户端恢复功能。我们建议禁用 `amqp-client` 自动恢复功能，以避免在 broker 可用但连接尚未恢复时得到 `AutoRecoverConnectionNotCurrentlyOpenException` 实例。你可能会注意到这种异常，例如，当 `RabbitTemplate` 中配置了 `RetryTemplate` 时，甚至在故障转移到集群中的另一个 broker 时也是如此。由于自动恢复连接是在 timer 上恢复的，因此通过使用 Spring AMQP 的恢复机制可以更快恢复连接。从 1.7.1 版开始，Spring AMQP 禁用了 `amqp-client` 自动恢复，除非你明确地创建自己的 RabbitMQ 连接工厂并将其提供给 `CachingConnectionFactory`。由 `RabbitConnectionFactoryBean` 创建的 RabbitMQ `ConnectionFactory` 实例也默认禁用该选项。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### `RabbitConnectionFactoryBean` 以及配置 SSL

从 1.4 版本开始，提供了一个方便的 `RabbitConnectionFactoryBean`，以便通过使用依赖注入在底层客户端连接工厂上方便地配置 SSL 属性。其他 setter 将委托给底层工厂。以前，你必须以编程方式配置 SSL 选项。下面的示例显示了如何配置 `RabbitConnectionFactoryBean`：

```xml
<rabbit:connection-factory id="rabbitConnectionFactory"
    connection-factory="clientConnectionFactory"
    host="${host}"
    port="${port}"
    virtual-host="${vhost}"
    username="${username}" password="${password}" />

<bean id="clientConnectionFactory"
        class="org.springframework.amqp.rabbit.connection.RabbitConnectionFactoryBean">
    <property name="useSSL" value="true" />
    <property name="sslPropertiesLocation" value="file:/secrets/rabbitSSL.properties"/>
</bean>
```

有关配置 SSL 的信息，请参阅 [RabbitMQ 文档](https://www.rabbitmq.com/ssl.html)。省略 `keyStore` 和 `trustStore` 配置以通过 SSL 连接而不进行证书验证。下一个示例显示了如何提供 key 和 trust store 配置。

`sslPropertiesLocation` 属性是一个 Spring `Resource`，指向一个包含以下 key 的 properties 文件：

```
keyStore=file:/secret/keycert.p12
trustStore=file:/secret/trustStore
keyStore.passPhrase=secret
trustStore.passPhrase=secret
```

`keyStore` 和 `truststore` 是指向 store 的Spring `Resources`。通常情况下，这个 properties 文件是由操作系统保护的，应用程序有读取权限。

从 Spring AMQP 1.5 版本开始，你可以直接在 factory Bean 上设置这些属性。如果同时提供了分离的属性和 `sslPropertiesLocation`，后者的属性将覆盖分离的值。

|      | 从 2.0 版本开始，服务器证书默认是经过验证的，因为它更安全。如果你出于某种原因希望跳过该验证，请将 factory Bean 的 `skipServerCertificateValidation` 属性设置为 `true`。从 2.1 版开始，`RabbitConnectionFactoryBean` 现在默认调用 `enableHostnameVerification()`。要恢复到以前的行为，请将 `enableHostnameVerification` 属性设置为 `false`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 从 2.2.5 版本开始，factory bean 将始终默认使用TLS v1.2；以前，它在某些情况下使用v1.1，在其他情况下使用v1.2（取决于其他属性）。如果你因为某些原因需要使用v1.1，请设置 `sslAlgorithm` 属性：`setSslAlgorithm("TLSv1.1")`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 连接到群集

要连接到一个集群，配置 `CachingConnectionFactory` 的 `addresses` 属性：

```java
@Bean
public CachingConnectionFactory ccf() {
    CachingConnectionFactory ccf = new CachingConnectionFactory();
    ccf.setAddresses("host1:5672,host2:5672,host3:5672");
    return ccf;
}
```

从 3.0 版本开始，每当建立一个新的连接时，底层连接工厂将通过选择一个随机地址来尝试连接到一个主机。要恢复到以前的行为，即尝试从第一个到最后一个连接，请将 `addressShuffleMode` 属性设置为 `AddressShuffleMode.NONE`。

从 2.3 版开始，增加了 `INORDER` shuffle mode，这意味着在创建连接后将第一个地址移到最后。如果你希望从所有节点上的所有分片中进行消费，你可能希望与 [RabbitMQ Sharding Plugin](https://github.com/rabbitmq/rabbitmq-sharding) 一起使用这种模式，并使用 `CacheMode.CONNECTION` 和适当的并发性。

```java
@Bean
public CachingConnectionFactory ccf() {
    CachingConnectionFactory ccf = new CachingConnectionFactory();
    ccf.setAddresses("host1:5672,host2:5672,host3:5672");
    ccf.setAddressShuffleMode(AddressShuffleMode.INORDER);
    return ccf;
}
```

##### 路由 Connection Factory

从1.3版本开始，`AbstractRoutingConnectionFactory` 已经被引入。这个工厂提供了一种机制，可以为几个 `ConnectionFactory` 配置映射，并在运行时通过一些 `lookupKey` 确定目标 `ConnectionFactory`。通常情况下，该实现会检查一个线程绑定的 context。为了方便，Spring AMQP 提供了 `SimpleRoutingConnectionFactory`，它从 `SimpleResourceHolder` 中获取当前线程绑定的 `lookupKey`。下面的例子展示了如何在 XML 和 Java 中配置一个 `SimpleRoutingConnectionFactory`：

```xml
<bean id="connectionFactory"
      class="org.springframework.amqp.rabbit.connection.SimpleRoutingConnectionFactory">
    <property name="targetConnectionFactories">
        <map>
            <entry key="#{connectionFactory1.virtualHost}" ref="connectionFactory1"/>
            <entry key="#{connectionFactory2.virtualHost}" ref="connectionFactory2"/>
        </map>
    </property>
</bean>

<rabbit:template id="template" connection-factory="connectionFactory" />
public class MyService {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    public void service(String vHost, String payload) {
        SimpleResourceHolder.bind(rabbitTemplate.getConnectionFactory(), vHost);
        rabbitTemplate.convertAndSend(payload);
        SimpleResourceHolder.unbind(rabbitTemplate.getConnectionFactory());
    }

}
```

重要的是，在使用后要解除对资源的绑定。欲了解更多信息，请参见 `AbstractRoutingConnectionFactory` 的 [JavaDoc](https://docs.spring.io/spring-amqp/docs/latest-ga/api/org/springframework/amqp/rabbit/connection/AbstractRoutingConnectionFactory.html)。

从 1.4 版开始，`RabbitTemplate` 支持 SpEL `sendConnectionFactorySelectorExpression` 和 `receiveConnectionFactorySelectorExpression` 属性，它们在每个 AMQP 协议交互操作（`send`、`sendAndReceive`、`receive` 或 `receiveAndReply`）中被评估，解析为所提供的 `AbstractRoutingConnectionFactory` 的 `lookupKey` 值。你可以使用 Bean 引用，例如表达式中的 `@vHostResolver.getVHost(#root)`。对于 `send` 操作，要发送的消息是 root evaluation object。对于 `receive` 操作，`queueName` 是 root evaluation object。

路由算法如下： 如果选 selector 达式为 `null`，或者被评估为 `null`，或者提供的 `ConnectionFactory` 不是 `AbstractRoutingConnectionFactory` 的实例，一切都像以前一样，依靠提供的 `ConnectionFactory` 实现。如果评估结果不是 `null` 的，但该 `lookupKey` 没有目标 `ConnectionFactory`，并且 `AbstractRoutingConnectionFactory` 被配置为 `lenientFallback = true`，也会发生同样的情况。在 `AbstractRoutingConnectionFactory` 的情况下，它确实会根据 `determineCurrentLookupKey()` fallback 到其 `routing` 实现。然而，如果 `lenientFallback = false`，就会抛出一个 `IllegalStateException`。

命名空间支持还提供了 `<rabbit:template>` 组件上的 `send-connection-factory-selector-expression` 和 `receive-connection-factory-selector-expression` 属性。

另外，从 1.4 版本开始，你可以在监听器容器中配置一个路由连接工厂。在这种情况下，队列名称的列表被用作 lookup key。例如，如果你用 `setQueueNames("thing1", "thing2")` 配置容器，lookup key 是 `[thing1,thing]`（注意，key 中没有空格）。

从 1.6.9 版本开始，你可以通过在监听器容器上使用 `setLookupKeyQualifier` 来为 lookup key 添加 qualifier。这样做可以实现，例如，监听具有相同名称但在不同虚拟主机中的队列（在那里你将为每个队列建立一个连接工厂）。

例如，用 lookup key qualifier `thing1` 和监听队列 `thing2` 的容器，你可以用 lookup key 注册目标连接工厂，可以是 `thing1[thing2]`。

|      | 目标（和默认，如果提供的话）连接工厂必须对发布者确认（confirm）和返回（return）有相同的设置。参见 [发布者消息确认和返回](https://springdoc.cn/spring-amqp/#cf-pub-conf-ret)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

从 2.4.4 版本开始，这种验证可以被禁用。如果你有 confirm 和 return 的值需要不相等的情况，你可以使用 `AbstractRoutingConnectionFactory#setConsistentConfirmsReturns` 来打开验证。注意，添加到 `AbstractRoutingConnectionFactory` 的第一个连接工厂将决定 `confirm` 和 `return` 的一般值。

如果你有这样的情况，你想检查某些信息的 confirm/return，而另一些则没有，这可能是有用的。比如说：

```java
@Bean
public RabbitTemplate rabbitTemplate() {
    final com.rabbitmq.client.ConnectionFactory cf = new com.rabbitmq.client.ConnectionFactory();
    cf.setHost("localhost");
    cf.setPort(5672);

    CachingConnectionFactory cachingConnectionFactory = new CachingConnectionFactory(cf);
    cachingConnectionFactory.setPublisherConfirmType(CachingConnectionFactory.ConfirmType.CORRELATED);

    PooledChannelConnectionFactory pooledChannelConnectionFactory = new PooledChannelConnectionFactory(cf);

    final Map<Object, ConnectionFactory> connectionFactoryMap = new HashMap<>(2);
    connectionFactoryMap.put("true", cachingConnectionFactory);
    connectionFactoryMap.put("false", pooledChannelConnectionFactory);

    final AbstractRoutingConnectionFactory routingConnectionFactory = new SimpleRoutingConnectionFactory();
    routingConnectionFactory.setConsistentConfirmsReturns(false);
    routingConnectionFactory.setDefaultTargetConnectionFactory(pooledChannelConnectionFactory);
    routingConnectionFactory.setTargetConnectionFactories(connectionFactoryMap);

    final RabbitTemplate rabbitTemplate = new RabbitTemplate(routingConnectionFactory);

    final Expression sendExpression = new SpelExpressionParser().parseExpression(
            "messageProperties.headers['x-use-publisher-confirms'] ?: false");
    rabbitTemplate.setSendConnectionFactorySelectorExpression(sendExpression);
}
```

这样，带有 header `x-use-publisher-confirms: true` 的消息将通过缓存连接发送，你可以确保消息的交付。关于确保消息传递的更多信息，请参见 [发布者消息确认和返回](https://springdoc.cn/spring-amqp/#cf-pub-conf-ret) 。

##### Queue 亲和性和 `LocalizedQueueConnectionFactory`

在集群中使用 HA 队列时，为了获得最佳性能，你可能想连接到 lead 队列所在的物理 broker。`CachingConnectionFactory` 可以配置多个 broker 地址。这是为了故障转移，客户端尝试按顺序连接。`LocalizedQueueConnectionFactory` 使用管理插件提供的 REST API 来确定哪个节点是该队列的 leader。然后，它创建（或从缓存中检索）一个 `CachingConnectionFactory`，只连接到该节点。如果连接失败，就会确定新的 lead 节点，消费者就会连接到它。`LocalizedQueueConnectionFactory` 被配置了一个默认的连接工厂，以备无法确定队列的物理位置，在这种情况下，它会像正常一样连接到集群。

`LocalizedQueueConnectionFactory` 是一个 `RoutingConnectionFactory`， `SimpleMessageListenerContainer` 使用队列名称作为 lookup key，正如上面 [路由 Connection Factory](https://springdoc.cn/spring-amqp/#routing-connection-factory) 中讨论的那样。

|      | 由于这个原因（使用队列名称进行查找），只有当容器被配置为监听单个队列时，才能使用 `LocalizedQueueConnectionFactory` 。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 必须在每个节点上启用 RabbitMQ 管理插件。 |
| ---- | ---------------------------------------- |
|      |                                          |

|      | 该连接工厂旨在用于长期连接，例如 `SimpleMessageListenerContainer` 所使用的连接。它不打算用于短期连接的使用，例如使用 `RabbitTemplate`，因为在建立连接之前调用 REST API 的开销很大。另外，对于 publish 操作，队列是未知的，而且无论如何消息都会 publish 给所有集群成员，所以查找节点的逻辑没有什么价值。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

下面的配置示例显示了如何配置工厂：

```java
@Autowired
private ConfigurationProperties props;

@Bean
public CachingConnectionFactory defaultConnectionFactory() {
    CachingConnectionFactory cf = new CachingConnectionFactory();
    cf.setAddresses(this.props.getAddresses());
    cf.setUsername(this.props.getUsername());
    cf.setPassword(this.props.getPassword());
    cf.setVirtualHost(this.props.getVirtualHost());
    return cf;
}

@Bean
public LocalizedQueueConnectionFactory queueAffinityCF(
        @Qualifier("defaultConnectionFactory") ConnectionFactory defaultCF) {
    return new LocalizedQueueConnectionFactory(defaultCF,
            StringUtils.commaDelimitedListToStringArray(this.props.getAddresses()),
            StringUtils.commaDelimitedListToStringArray(this.props.getAdminUris()),
            StringUtils.commaDelimitedListToStringArray(this.props.getNodes()),
            this.props.getVirtualHost(), this.props.getUsername(), this.props.getPassword(),
            false, null);
}
```

注意，前三个参数是 `addresses`、`adminUris` 和 `nodes` 的数组。这些都是位置性的，当一个容器试图连接到一个队列时，它使用 admin API 来确定哪个节点是该队列的 lead，并连接到与该节点在同一数组位置的地址。

|      | 从 3.0 版开始，RabbitMQ `http-client` 不再用于访问 Rest API。相反，默认情况下，如果 `spring-webflux` 在类路径上，则使用 Spring Webflux 的 `WebClient`；否则将使用 `RestTemplate`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

要将 `WebFlux` 添加到 class path 中：

Example 1. Maven

```xml
<dependency>
  <groupId>org.springframework.amqp</groupId>
  <artifactId>spring-rabbit</artifactId>
</dependency>
```

Example 2. Gradle

```groovy
compile 'org.springframework.amqp:spring-rabbit'
```

你也可以通过实现 `LocalizedQueueConnectionFactory.NodeLocator` 并重写其 `createClient`、`restCall` 和可选的 `close` 方法来使用其他REST技术。

```java
lqcf.setNodeLocator(new NodeLocator<MyClient>() {

    @Override
    public MyClient createClient(String userName, String password) {
        ...
    }

    @Override
    public HashMap<String, Object> restCall(MyClient client, URI uri) {
        ...
    });

});
```

该框架提供了 `WebFluxNodeLocator` 和 `RestTemplateNodeLocator`，其默认值如上所述。

##### 发布者消息确认和返回

通过将 `CachingConnectionFactory` 属性 `publisherConfirmType` 设置为 `ConfirmType.CORRELATED`，`publisherReturns` 属性设置为 `true`，可以支持确认（有关联）和返回的消息。

当这些选项被设置时，由工厂创建的 `Channel` 实例被包裹在一个 `PublisherCallbackChannel` 中，它被用来促进回调。当获得这样一个 channel 时，客户端可以向该 `Channel` 注册一个 `PublisherCallbackChannel.Listener`。 `PublisherCallbackChannel` 的实现包含了将确认或返回发送到适当的监听器的逻辑。这些功能将在下面的章节中进一步解释。

另请参见 [Scope 操作](https://springdoc.cn/spring-amqp/#scoped-operations) 中的 `simplePublisherConfirms`。

|      | 有关更多背景信息，请参见 RabbitMQ 团队发表的题为 [发布者确认的介绍](https://www.rabbitmq.com/blog/2011/02/10/introducing-publisher-confirms/) 的博文。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 连接和 Channel 监听器

连接工厂支持注册 `ConnectionListener` 和 `ChannelListener` 实现。这允许你接收连接和 channel 相关事件的通知。(`ConnectionListener` 被 `RabbitAdmin` 用于在建立连接时执行声明 - 有关详细信息，请参阅 [Exchange、 Queue 和 Binding 的自动声明](https://springdoc.cn/spring-amqp/#automatic-declaration) ）。下面的列表显示了 `ConnectionListener` 接口的定义：

```java
@FunctionalInterface
public interface ConnectionListener {

    void onCreate(Connection connection);

    default void onClose(Connection connection) {
    }

    default void onShutDown(ShutdownSignalException signal) {
    }

}
```

从2.0版本开始，`org.springframework.amqp.rabbit.connection.Connection` 对象可以被提供给 `com.rabbitmq.client.BlockedListener` 实例，以获得连接阻塞和解阻塞事件的通知。下面的例子显示了 `ChannelListener` 接口的定义：

```java
@FunctionalInterface
public interface ChannelListener {

    void onCreate(Channel channel, boolean transactional);

    default void onShutDown(ShutdownSignalException signal) {
    }

}
```

请参阅 "[异步发布 - 如何检测成功和失败](https://springdoc.cn/spring-amqp/#publishing-is-async) "，了解你可能想要注册 `ChannelListener` 的一个场景。

##### 记录 Channel 关闭事件

1.5 版本引入了一个机制，使用户能够控制日志记录级别。

`CachingConnectionFactory` 使用一个默认的策略来记录 channel 关闭，如下所示：

- 正常的 channel 关闭（200 OK）不会被记录下来。
- I如果一个 channel 由于被动队列声明失败而被关闭，它会被记录在 debug 级别。
- 如果一个 channel 被关闭，因为 `basic.consume` 排他性消费条件而被拒绝，它被记录在 INFO 级别。
- 所有其他的都记录在 ERROR 级别上。

为了修改这种行为，你可以在 `CachingConnectionFactory` 的 `closeExceptionLogger` 属性中注入一个自定义的 `ConditionalExceptionLogger`。

另见 [消费者事件](https://springdoc.cn/spring-amqp/#consumer-events)。

##### 运行时缓存属性

从 1.6 版本开始，`CachingConnectionFactory` 现在通过 `getCacheProperties()` 方法提供缓存统计数据。这些统计数据可以用来调整缓存，以在生产中优化它。例如，高水位（high water marks）可以用来确定是否应该增加缓存的大小。如果它等于缓存的大小，你可能要考虑进一步增加。下表描述了 `CacheMode.CHANNEL` 属性：

| 属性                         | 说明                                                         |
| :--------------------------- | :----------------------------------------------------------- |
| `connectionName`             | 由 `ConnectionNameStrategy` 生成的连接的名称。               |
| `channelCacheSize`           | 当前配置的允许空闲的最多 channel。                           |
| `localPort`                  | 接的本地端口（如果可用）。这可用于与 RabbitMQ 管理用户界面上的连接和 channel 相关联。 |
| `idleChannelsTx`             | 目前闲置（缓存的）的事务 channel 的数量。                    |
| `idleChannelsNotTx`          | 目前闲置（缓存的）的非事务性 channel 的数量。                |
| `idleChannelsTxHighWater`    | 已并发闲置（缓存的）的事务 channel 的最大数量。              |
| `idleChannelsNotTxHighWater` | 已并发闲置（缓存的）的非事务 channel 的最大数量。            |

下表描述了 `CacheMode.CONNECTION` 属性：

| 属性                                     | 说明                                                         |
| :--------------------------------------- | :----------------------------------------------------------- |
| `connectionName:<localPort>`             | 由 `ConnectionNameStrategy` 生成的连接的名称。               |
| `openConnections`                        | 代表与 broker 连接的连接对象的数量。                         |
| `channelCacheSize`                       | 当前配置的允许空闲的最多 channel。                           |
| `connectionCacheSize`                    | 当前配置的允许空闲的最大连接。                               |
| `idleConnections`                        | 目前处于空闲状态的连接数。                                   |
| `idleConnectionsHighWater`               | 已并发空闲的最大连接数。                                     |
| `idleChannelsTx:<localPort>`             | 此连接当前闲置（缓存）的事务性 channel 的数量。你可以使用该属性名称的 `localPort` 部分来与 RabbitMQ 管理用户界面上的连接和 channel 相关联。 |
| `idleChannelsNotTx:<localPort>`          | 此连接当前闲置（缓存）的非事务性 channel 的数量。该属性名称的 `localPort` 部分可用于与 RabbitMQ 管理用户界面上的连接和channel相关联。 |
| `idleChannelsTxHighWater:<localPort>`    | 已并发闲置（缓存）的事务性 channel 的最大数量。该属性名称的 `localPort` 部分可用于与 RabbitMQ 管理用户界面上的连接和 channel 相关联。 |
| `idleChannelsNotTxHighWater:<localPort>` | 已并发闲置（缓存）的非事务性 channel 的最大数量。你可以使用该属性名称的 `localPort` 部分来与 RabbitMQ 管理用户界面上的连接和 channel 相关联。 |

`cacheMode` 属性（`CHANNEL` 或 `CONNECTION`）也被包括在内。

##### RabbitMQ 自动连接/拓扑恢复

JVisualVM 示例

自 Spring AMQP 的第一个版本以来，该框架已在 broker 失败的情况下提供了自己的连接和 channel 恢复。此外，正如在 [配置 Broker](https://springdoc.cn/spring-amqp/#broker-configuration) 中所讨论的，`RabbitAdmin` 在重新建立连接时重新声明任何基础设施 Bean（队列和其他）。因此，它不依赖于现在由 `amqp-client` 库提供的 [auto-recovery](https://www.rabbitmq.com/api-guide.html#recovery)（自动恢复） 功能。`amqp-client` 默认启用了自动恢复功能。这两种恢复机制之间存在一些不兼容的情况，因此默认情况下，Spring 将底层 `RabbitMQ connectionFactory` 上的 `automaticRecoveryEnabled` 属性设置为 `false`。即使该属性为 `true`，Spring 也会通过立即关闭任何恢复的连接来有效地禁用它。

|      | 默认情况下，只有被定义为Bean的元素（queue、exchange、binding）会在连接失败后被重新声明。关于如何改变这种行为，请参见 [恢复 Auto-Delete 的声明](https://springdoc.cn/spring-amqp/#declarable-recovery)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 4.1.3. 添加自定义的客户端连接属性

`CachingConnectionFactory` 现在可以让你访问底层连接工厂，以允许，例如，设置自定义客户端属性。下面的例子显示了如何做到这一点：

```java
connectionFactory.getRabbitConnectionFactory().getClientProperties().put("thing1", "thing2");
```

在查看连接时，这些属性会出现在 RabbitMQ Admin UI 中。

#### 4.1.4. `AmqpTemplate`

与Spring框架和相关项目提供的许多其他高级抽象一样，Spring AMQP提供了一个 "template"，发挥了核心作用。定义主要操作的接口被称为 `AmqpTemplate`。这些操作涵盖了发送和接收消息的一般行为。换句话说，它们对任何实现都不是唯一的—因此名称中的 "AMQP"。另一方面，该接口的一些实现是与AMQP协议的实现相联系的。与JMS不同的是，AMQP本身是一个接口级的API，它是一个 wire-level 协议。该协议的实现提供了他们自己的客户端库，所以模板接口的每个实现都依赖于一个特定的客户端库。目前，只有一种实现： `RabbitTemplate`。在下面的例子中，我们经常使用 `AmqpTemplate`。但是，当你查看配置示例或任何实例化模板或调用 setter 的代码摘录时，你可以看到实现类型（例如 `RabbitTemplate`）。

如前所述，`AmqpTemplate` 接口定义了所有发送和接收消息的基本操作。我们将在 [发送消息](https://springdoc.cn/spring-amqp/#sending-messages) 和 [接收消息](https://springdoc.cn/spring-amqp/#receiving-messages) 中分别探讨消息的发送和接收。

另请参见 [Async Rabbit Template](https://springdoc.cn/spring-amqp/#async-template)。

##### 添加重试能力

从 1.3 版开始，你现在可以将 `RabbitTemplate` 配置为使用 `RetryTemplate` 以帮助处理broker连接问题。请参阅 [spring-retry](https://github.com/spring-projects/spring-retry) 项目以了解完整信息。下面只是一个使用指数回退策略和默认 `SimpleRetryPolicy` 的示例，它在向调用者抛出异常之前进行了三次尝试。

下面的例子使用了XML命名空间：

```xml
<rabbit:template id="template" connection-factory="connectionFactory" retry-template="retryTemplate"/>

<bean id="retryTemplate" class="org.springframework.retry.support.RetryTemplate">
    <property name="backOffPolicy">
        <bean class="org.springframework.retry.backoff.ExponentialBackOffPolicy">
            <property name="initialInterval" value="500" />
            <property name="multiplier" value="10.0" />
            <property name="maxInterval" value="10000" />
        </bean>
    </property>
</bean>
```

下面的例子在 Java 中使用了 `@Configuration` 注解：

```java
@Bean
public RabbitTemplate rabbitTemplate() {
    RabbitTemplate template = new RabbitTemplate(connectionFactory());
    RetryTemplate retryTemplate = new RetryTemplate();
    ExponentialBackOffPolicy backOffPolicy = new ExponentialBackOffPolicy();
    backOffPolicy.setInitialInterval(500);
    backOffPolicy.setMultiplier(10.0);
    backOffPolicy.setMaxInterval(10000);
    retryTemplate.setBackOffPolicy(backOffPolicy);
    template.setRetryTemplate(retryTemplate);
    return template;
}
```

从 1.4 版本开始，除了 `retryTemplate` 属性外，`RabbitTemplate` 上还支持 `recoveryCallback` 选项。它被用作 `RetryTemplate.execute(RetryCallback<T, E> retryCallback, RecoveryCallback<T> recoveryCallback)` 的第二个参数。

|      | `RecoveryCallback` 有一定的局限性，因为 retry context 只包含 `lastThrowable` 字段。对于更复杂的用例，你应该使用一个外部的 `RetryTemplate`，这样你就可以通过 context 的属性向 `RecoveryCallback` 传达额外的信息。下面的例子显示了如何做到这一点： |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

```java
retryTemplate.execute(
    new RetryCallback<Object, Exception>() {

        @Override
        public Object doWithRetry(RetryContext context) throws Exception {
            context.setAttribute("message", message);
            return rabbitTemplate.convertAndSend(exchange, routingKey, message);
        }

    }, new RecoveryCallback<Object>() {

        @Override
        public Object recover(RetryContext context) throws Exception {
            Object message = context.getAttribute("message");
            Throwable t = context.getLastThrowable();
            // Do something with message
            return null;
        }
    });
}
```

在这种情况下，你不会将一个 `RetryTemplate` 注入 `RabbitTemplate`。

##### 异步发布 - 如何检测成功和失败

发布消息是一种异步机制，默认情况下，不能被路由的消息会被 RabbitMQ 丢弃。对于成功发布，你可以收到一个异步确认，如 [相关的 Publisher（发布者）确认和返回](https://springdoc.cn/spring-amqp/#template-confirms) 中所述。考虑两种失败情况：

- 发布到一个 exchange，但没有匹配的目标队列。
- 发布到一个不存在的 exchange。

第一种情况是由 publisher 的返回所涵盖的，如 [相关的 Publisher（发布者）确认和返回](https://springdoc.cn/spring-amqp/#template-confirms) 中所述。

对于第二种情况，消息被丢弃，不产生返回。底层 channel 被关闭，并出现一个异常。默认情况下，这个异常会被记录下来，但你可以在 `CachingConnectionFactory` 中注册一个 `ChannelListener`，以获得此类事件的通知。下面的例子显示了如何添加一个 `ConnectionListener`：

```java
this.connectionFactory.addConnectionListener(new ConnectionListener() {

    @Override
    public void onCreate(Connection connection) {
    }

    @Override
    public void onShutDown(ShutdownSignalException signal) {
        ...
    }

});
```

你可以检查 signal 的 `reason` 属性来确定发生的问题。

要在发送线程上检测异常，你可以在 `RabbitTemplate` 上设置 `ChannelTransacted(true)`，并且在 `txCommit()` 上检测异常。但是，事务会明显阻碍性能，因此在为这一用例启用事务之前，请仔细考虑这一点。

##### 相关的 Publisher（发布者）确认和返回

`AmqpTemplate` 的 `RabbitTemplate` 实现支持发布者确认和返回。

对于返回的消息，template 的 `mandatory` 属性必须被设置为 `true`，或者对于特定的消息，`mandatory-expression` 必须评估为 `true`。此功能要求 `CachingConnectionFactory` 将其 `publisherReturns` 属性设置为 `true`（请参阅 [发布者消息确认和返回](https://springdoc.cn/spring-amqp/#cf-pub-conf-ret)）。通过调用 `setReturnsCallback(ReturnsCallback callback)` 注册 `RabbitTemplate.ReturnsCallback`，将返回发送到客户端。该回调必须实现以下方法：

```java
void returnedMessage(ReturnedMessage returned);
```

`ReturnedMessage` 有以下属性：

- `message` - 返回的消息本身
- `replyCode` - 表示返回原因的代码
- `replyText` - 返回的文本原因 - 例如：`NO_ROUTE`
- `exchange` - 发出该信息的exchange
- `routingKey` - 所用的路由key

每个 `RabbitTemplate` 只支持一个 `ReturnsCallback`。另请参阅 [回复超时](https://springdoc.cn/spring-amqp/#reply-timeout)。

对于发布者确认，该 template 需要一个 `CachingConnectionFactory`，它的 `publisherConfirm` 属性被设置为 `ConfirmType.CORRELATED`。确认是通过它通过调用 `setConfirmCallback(ConfirmCallback callback)` 注册 `RabbitTemplate.ConfirmCallback` 发送给客户端。该回调必须实现此方法：

```java
void confirm(CorrelationData correlationData, boolean ack, String cause);
```

`CorrelationData` 是客户端在发送原始消息时提供的一个对象。`ack` 对于一个 `ack` 来说是 `true`，对于一个 `nack` 来说是 `false`。对于 `nack` 实例，`cause` 可能包含 `nack` 的原因，如果它在生成 `nack` 时是可用的。一个例子是当向一个不存在的 exchange 发送消息时。在这种情况下，broker 会关闭该 channel。关闭的原因包括在 `cause` 中。`cause` 是在1.4版本中添加的。

一个 `RabbitTemplate` 只支持一个 `ConfirmCallback`。

|      | 当 rabbit template 发送操作完成后，channel 被关闭。当连接工厂的缓存已满时，这就排除了对确认或返回的接收（当缓存中有空间时，channel 没有被物理关闭，返回和确认正常进行）。当缓存已满时，框架会将关闭时间推迟五秒，以便有时间接收确认和返回。当使用确认时，channel 在收到最后一个确认时被关闭。当只使用返回时，channel 在整整五秒钟内保持开放。我们通常建议将连接工厂的 `channelCacheSize` 设置为一个足够大的值，这样发布消息的 channel 就会被返回到缓存中而不是被关闭。你可以通过使用 RabbitMQ 管理插件来监控 channel 使用情况。如果你看到 channel 被快速打开和关闭，你应该考虑增加缓存大小以减少服务器上的开销。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 在2.1版本之前，为发布者确认而启用的 channel 在收到确认之前就被返回到缓存中。一些其他进程可以检索出 channel，并执行一些导致 channel 关闭的操作—例如向一个不存在的 exchange 发布消息。这可能导致确认被丢失。2.1 版和更高版本不再在确认未完成时将 channel 返回到缓存中。`RabbitTemplate` 在每次操作后对 channel 执行逻辑 `close()`。一般来说，这意味着一个 channel 上一次只有一个确认是未完成的。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 从 2.2 版开始，回调在连接工厂的一个 `executor` 线程上被调用。这是为了避免在回调中执行 Rabbit 操作时出现潜在的死锁。在以前的版本中，回调是直接在 `amqp-client` 连接的I/O线程上调用的；如果你执行一些 RPC 操作（例如打开一个新 channel），这将导致死锁，因为I/O线程会阻塞等待结果，但结果需要由I/O线程本身处理。在这些版本中，有必要在回调中把工作（如发送消息）移交给另一个线程。由于框架现在将回调调用移交给了 executor，所以这不再是必要的。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 只要返回回调在60秒或更短的时间内执行，在 ack 之前收到返回信息的保证仍然保持。确认被安排在返回回调退出后或60秒后（以先到者为准）交付。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

`CorrelationData` 对象有一个 `CompletableFuture`，你可以用它来获取结果，而不是在 template 上使用 `ConfirmCallback`。下面的例子显示了如何配置一个 `CorrelationData` 实例：

```java
CorrelationData cd1 = new CorrelationData();
this.templateWithConfirmsEnabled.convertAndSend("exchange", queue.getName(), "foo", cd1);
assertTrue(cd1.getFuture().get(10, TimeUnit.SECONDS).isAck());
```

因为它是一个 `CompletableFuture<Confirm>`，你可以在准备好的时候 `get()` 结果，或者使用 `whenComplete()` 进行异步回调。`Confirm` 对象是一个简单的 Bean，有2个属性：`ack` 和 `reason`（对于 `nack` 实例）。对于 broker 生成的 `nack` 实例，reason 不被填充。它被填充到由框架生成的 `nack` 实例中（例如，在 `ack` 实例未完成时关闭连接）。

此外，当确认和返回都被启用时，只要 `CorrelationData` 有一个唯一的 `id`， `CorrelationData` 就会被填充到返回的消息中；从2.3版开始，默认情况下总是这样的。保证返回的消息在未来被设置为 `ack` 之前被设置。

关于等待发布者确认的更简单的机制，也请参见 [Scope 操作](https://springdoc.cn/spring-amqp/#scoped-operations)。

##### Scope 操作

通常情况下，当使用 template 时，一个 `Channel` 从缓存中检查出来（或创建），用于操作，并返回到缓存中重新使用。在一个多线程的环境中，不能保证下一个操作使用同一个 channel。然而，有时你可能想对 channel 的使用有更多的控制，并确保一些操作都是在同一个 channel 上进行的。

从 2.0 版本开始，提供了一个名为 `invoke` 的新方法，该方法有一个 `OperationsCallback`。在回调 scope 内和在所提供的 `RabbitOperations` 参数上执行的任何操作都使用相同的专用 `Channel`，它将在结束时被关闭（不返回到缓存）。如果该 channel 是 `PublisherCallbackChannel`，它将在收到所有确认后返回到缓存中（请参阅 [相关的 Publisher（发布者）确认和返回](https://springdoc.cn/spring-amqp/#template-confirms)）。

```java
@FunctionalInterface
public interface OperationsCallback<T> {

    T doInRabbit(RabbitOperations operations);

}
```

你可能需要这样做的一个例子是，如果你想在底层 `Channel` 上使用 `waitForConfirms()` 方法。这个方法以前没有被 Spring API 公开，因为正如前面所讨论的，channel 通常是缓存和共享的。`RabbitTemplate` 现在提供 `waitForConfirms(long timeout)` 和 `waitForConfirmsOrDie(long timeout)`，它们委托给在 `OperationsCallback` scope 内使用的专用 channel。由于这些原因，很明显这些方法不能在该 scope 之外使用。

请注意，在其他地方提供了一个更高层次的抽象，让你将确认与请求相关联（见 [相关的 Publisher（发布者）确认和返回](https://springdoc.cn/spring-amqp/#template-confirms)）。如果你只想等待 broker 确认交付，你可以使用下面例子中的技术：

```java
Collection<?> messages = getMessagesToSend();
Boolean result = this.template.invoke(t -> {
    messages.forEach(m -> t.convertAndSend(ROUTE, m));
    t.waitForConfirmsOrDie(10_000);
    return true;
});
```

如果你希望 `RabbitAdmin` 操作在 `OperationsCallback` 的 scope 内的同一 channel 上被调用，则 admin 必须通过使用用于 `invoke` 操作的相同 `RabbitTemplate` 来构建。

|      | 如果 template 操作已经在现有事务的 scope 内执行，那么前面的讨论就没有意义了—例如，当在一个事务性监听器容器线程上运行并对一个事务性 template 执行操作时。在这种情况下，操作在该 channel 上执行，并在线程返回容器时提交。在这种情况下，没有必要使用 `invoke`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

当以这种方式使用确认时，许多为将确认与请求相关联而设置的基础设施其实是不需要的（除非也启用了返回）。从2.2版本开始，连接工厂支持一个叫做 `publisherConfirmType` 的新属性。当这个属性被设置为 `ConfirmType.SIMPLE` 时，就可以避免使用这些基础设施，并且确认处理可以更加有效。

此外，`RabbitTemplate` 在发送的 message `MessageProperties` 中设置 `publisherSequenceNumber` 属性。如果你希望检查（或log或以其他方式使用）特定的确认（confirm），你可以用重载的 `invoke` 方法这样做，如下面的示例所示：

```java
public <T> T invoke(OperationsCallback<T> action, com.rabbitmq.client.ConfirmCallback acks,
        com.rabbitmq.client.ConfirmCallback nacks);
```

|      | 这些 `ConfirmCallback` 对象（对于 `ack` 和 `nack` 实例）是 Rabbit 客户端回调，而不是 template 回调。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

下面的例子记录了 `ack` 和 `nack` 的实例：

```java
Collection<?> messages = getMessagesToSend();
Boolean result = this.template.invoke(t -> {
    messages.forEach(m -> t.convertAndSend(ROUTE, m));
    t.waitForConfirmsOrDie(10_000);
    return true;
}, (tag, multiple) -> {
        log.info("Ack: " + tag + ":" + multiple);
}, (tag, multiple) -> {
        log.info("Nack: " + tag + ":" + multiple);
}));
```

|      | Scope 操作被绑定到一个线程。关于多线程环境下严格的消息排序的讨论，请参 [多线程环境下严格的消息排序](https://springdoc.cn/spring-amqp/#multi-strict) 。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 多线程环境下严格的消息排序

[Scope 操作](https://springdoc.cn/spring-amqp/#scoped-operations) 中的讨论只适用于在同一线程上执行的操作。

考虑以下情况：

- `thread-1` 向队列发送消息，并将工作移交给`thread-2`。
- `thread-2` 向同一个队列发送一个消息。

由于 RabbitMQ 的异步性质和对缓存 channel 的使用；不确定是否会使用同一 channel，因此不保证消息到达队列的顺序。（在大多数情况下，它们将按顺序到达，但不按顺序交付的概率不为零）。为了解决这个用例，你可以使用一个大小为 `1` 的有界 channel 缓存（连同 `channelCheckoutTimeout`），以确保消息总是在同一个 channel 上发布，而且顺序会得到保证。要做到这一点，如果你对连接工厂有其他用途，比如消费者，你应该为 template 使用一个专门的连接工厂，或者将模板配置为使用嵌入在主连接工厂中的发布者连接工厂（参见 [使用一个单独的连接](https://springdoc.cn/spring-amqp/#separate-connection)）。

这一点最好用一个简单的Spring Boot应用来说明：

```java
@SpringBootApplication
public class Application {

	private static final Logger log = LoggerFactory.getLogger(Application.class);

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}

	@Bean
	TaskExecutor exec() {
		ThreadPoolTaskExecutor exec = new ThreadPoolTaskExecutor();
		exec.setCorePoolSize(10);
		return exec;
	}

	@Bean
	CachingConnectionFactory ccf() {
		CachingConnectionFactory ccf = new CachingConnectionFactory("localhost");
		CachingConnectionFactory publisherCF = (CachingConnectionFactory) ccf.getPublisherConnectionFactory();
		publisherCF.setChannelCacheSize(1);
		publisherCF.setChannelCheckoutTimeout(1000L);
		return ccf;
	}

	@RabbitListener(queues = "queue")
	void listen(String in) {
		log.info(in);
	}

	@Bean
	Queue queue() {
		return new Queue("queue");
	}


	@Bean
	public ApplicationRunner runner(Service service, TaskExecutor exec) {
		return args -> {
			exec.execute(() -> service.mainService("test"));
		};
	}

}

@Component
class Service {

	private static final Logger LOG = LoggerFactory.getLogger(Service.class);

	private final RabbitTemplate template;

	private final TaskExecutor exec;

	Service(RabbitTemplate template, TaskExecutor exec) {
		template.setUsePublisherConnection(true);
		this.template = template;
		this.exec = exec;
	}

	void mainService(String toSend) {
		LOG.info("Publishing from main service");
		this.template.convertAndSend("queue", toSend);
		this.exec.execute(() -> secondaryService(toSend.toUpperCase()));
	}

	void secondaryService(String toSend) {
		LOG.info("Publishing from secondary service");
		this.template.convertAndSend("queue", toSend);
	}

}
```

即使发布是在两个不同的线程上进行的，它们都将使用同一个channel，因为缓存的上限是一个channel。

从2.3.7版本开始，`ThreadChannelConnectionFactory` 支持使用 `prepareContextSwitch` 和 `switchContext` 方法，将一个线程的 channel 转移到另一个线程。第一个方法返回一个 context，该 context 被传递给第二个线程，后者调用第二个方法。一个线程可以有一个非事务 channel 或一个事务 channel （或两者之一）与之绑定；你不能单独转移它们，除非你使用两个连接工厂。下面是一个例子：

```java
@SpringBootApplication
public class Application {

	private static final Logger log = LoggerFactory.getLogger(Application.class);

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}

	@Bean
	TaskExecutor exec() {
		ThreadPoolTaskExecutor exec = new ThreadPoolTaskExecutor();
		exec.setCorePoolSize(10);
		return exec;
	}

	@Bean
	ThreadChannelConnectionFactory tccf() {
		ConnectionFactory rabbitConnectionFactory = new ConnectionFactory();
		rabbitConnectionFactory.setHost("localhost");
		return new ThreadChannelConnectionFactory(rabbitConnectionFactory);
	}

	@RabbitListener(queues = "queue")
	void listen(String in) {
		log.info(in);
	}

	@Bean
	Queue queue() {
		return new Queue("queue");
	}


	@Bean
	public ApplicationRunner runner(Service service, TaskExecutor exec) {
		return args -> {
			exec.execute(() -> service.mainService("test"));
		};
	}

}

@Component
class Service {

	private static final Logger LOG = LoggerFactory.getLogger(Service.class);

	private final RabbitTemplate template;

	private final TaskExecutor exec;

	private final ThreadChannelConnectionFactory connFactory;

	Service(RabbitTemplate template, TaskExecutor exec,
			ThreadChannelConnectionFactory tccf) {

		this.template = template;
		this.exec = exec;
		this.connFactory = tccf;
	}

	void mainService(String toSend) {
		LOG.info("Publishing from main service");
		this.template.convertAndSend("queue", toSend);
		Object context = this.connFactory.prepareSwitchContext();
		this.exec.execute(() -> secondaryService(toSend.toUpperCase(), context));
	}

	void secondaryService(String toSend, Object threadContext) {
		LOG.info("Publishing from secondary service");
		this.connFactory.switchContext(threadContext);
		this.template.convertAndSend("queue", toSend);
		this.connFactory.closeThreadChannel();
	}

}
```

|      | 一旦 `prepareSwitchContext` 被调用，如果当前线程再执行任何操作，它们将在一个新的 channel 上执行。当不再需要线程绑定的 channel 时，关闭它是很重要的。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### Messaging 整合

从 1.4 版本开始，`RabbitMessagingTemplate`（建立在 `RabbitTemplate` 之上）提供了与 Spring Framework messaging 抽象的整合 - 即 `org.springframework.messaging.Message`。这使你可以通过使用 spring-messaging `Message<?>` 抽象来发送和接收消息。这个抽象被其他Spring项目所使用，比如 Spring Integration 和 Spring 的 STOMP 支持。这里涉及两个消息转换器（message converter）：一个用于在 spring-messaging `Message<?>` 和 Spring AMQP 的 `Message` 抽象之间进行转换，另一个用于在 Spring AMQP 的 `Message` 抽象和底层 RabbitMQ 客户端库所要求的格式之间进行转换。默认情况下，消息 payload 由提供的 `RabbitTemplate` 实例的消息转换器转换。另外，你可以注入一个自定义的 `MessagingMessageConverter` 与一些其他 payload 转换器，如下例所示：

```java
MessagingMessageConverter amqpMessageConverter = new MessagingMessageConverter();
amqpMessageConverter.setPayloadConverter(myPayloadConverter);
rabbitMessagingTemplate.setAmqpMessageConverter(amqpMessageConverter);
```

##### 验证的 User Id

从1.6版本开始， template 现在支持一个 `user-id-expression` （使用Java配置时为 `userIdExpression`）。如果有消息被发送，在评估这个表达式后，user id 属性被设置（如果还没有设置）。评估的根对象（root object）是要发送的消息。

下面的例子显示了如何使用 `user-id-expression` 属性：

```xml
<rabbit:template ... user-id-expression="'guest'" />

<rabbit:template ... user-id-expression="@myConnectionFactory.username" />
```

第一个例子是一个字面表达式。第二个例子是从 application context 中的连接工厂 Bean中获取 `username` 属性。

##### 使用一个单独的连接

从2.0.2版本开始，你可以将 `usePublisherConnection` 属性设置为 `true`，以便在可能的情况下使用与监听器容器使用的不同的连接。这是为了避免消费者在生产者因任何原因被阻断时被阻断。连接工厂为此维护了第二个内部连接工厂；默认情况下，它与主工厂的类型相同，但如果你希望使用不同的工厂类型进行发布，可以明确设置。如果 rabbit template 在由监听器容器启动的事务中运行，那么就会使用该容器的 channel，与此设置无关。

|      | 一般来说，你不应该使用带有将此设置为 `true` 的 template 的 `RabbitAdmin`。使用接收连接工厂的 `RabbitAdmin` 构造函数。如果你使用接收 template 的其他构造函数，请确保 template 的属性为 `false`。这是因为，通常情况下，admin 被用来为监听器容器声明队列。使用属性设置为 `true` 的 template 将意味着独占队列（如 `AnonymousQueue`）将被声明在与监听器容器使用的连接不同的连接上。在这种情况下，这些队列不能被容器使用。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 4.1.5. 发送消息

发送信息时，你可以使用以下任何一种方法：

```java
void send(Message message) throws AmqpException;

void send(String routingKey, Message message) throws AmqpException;

void send(String exchange, String routingKey, Message message) throws AmqpException;
```

我们可以从前面列表中的最后一个方法开始讨论，因为它实际上是最明确的。它允许在运行时提供一个 AMQP exchange 名称（以及一个路由 key）。最后一个参数是负责实际创建消息实例的回调。使用这个方法来发送消息的例子可能是这样的： 下面的例子显示了如何使用 `send` 方法来发送一个消息：

```java
amqpTemplate.send("marketData.topic", "quotes.nasdaq.THING1",
    new Message("12.34".getBytes(), someProperties));
```

如果你打算大部分时间或全部时间使用该 template 实例发送至同一 exchange，你可以在 template 本身上设置 `exchange` 属性。在这种情况下，你可以使用前面列表中的第二个方法。下面的例子在功能上等同于前面的例子：

```java
amqpTemplate.setExchange("marketData.topic");
amqpTemplate.send("quotes.nasdaq.FOO", new Message("12.34".getBytes(), someProperties));
```

如果在模板上同时设置了 `exchange` 和 `routingKey` 属性，你可以使用只接受 `Message` 的方法。下面的例子显示了如何做到这一点：

```java
amqpTemplate.setExchange("marketData.topic");
amqpTemplate.setRoutingKey("quotes.nasdaq.FOO");
amqpTemplate.send(new Message("12.34".getBytes(), someProperties));
```

对 exchange 和 routing key 属性更好的思考方式是，显式方法参数总是覆盖 template 的默认值。事实上，即使你不在 template 上明确设置这些属性，也总是有默认值。在这两种情况下，默认值是一个空的 `String`，但这实际上是一个合理的默认值。至于 routing key，首先它并不总是必要的（例如，对于一个 `Fanout` exchange ）。此外，一个队列可能被绑定到一个空的 `String` 的 exchange。这些都是依赖 template 的 routing key 属性的默认空 `String` 值的合法情况。就 exchange 名称而言，空 `String` 通常被使用，因为AMQP规范将 "默认 exchange" 定义为没有名称。由于所有队列都被自动绑定到该默认 exchange （这是一个 direct exchange），使用它们的名称作为绑定值，前面列出的第二种方法可用于通过默认 exchange 所向任何队列发送简单的点到点消息。你可以提供队列名称作为 `routingKey`，或者在运行时提供方法参数。下面的例子显示了如何做到这一点：

```java
RabbitTemplate template = new RabbitTemplate(); // using default no-name Exchange
template.send("queue.helloWorld", new Message("Hello World".getBytes(), someProperties));
```

另外，你可以创建一个 template，主要或专门用于发布到一个队列。下面的例子显示了如何做到这一点：

```java
RabbitTemplate template = new RabbitTemplate(); // using default no-name Exchange
template.setRoutingKey("queue.helloWorld"); // but we'll always send to this Queue
template.send(new Message("Hello World".getBytes(), someProperties));
```

##### Message Builder API

从 1.3 版本开始，message builder API 由 `MessageBuilder` 和 `MessagePropertiesBuilder` 提供。这些方法为创建消息或消息属性提供了一个方便的 “fluent” 风格。下面的例子显示了 fluent API 的作用：

```java
Message message = MessageBuilder.withBody("foo".getBytes())
    .setContentType(MessageProperties.CONTENT_TYPE_TEXT_PLAIN)
    .setMessageId("123")
    .setHeader("bar", "baz")
    .build();
MessageProperties props = MessagePropertiesBuilder.newInstance()
    .setContentType(MessageProperties.CONTENT_TYPE_TEXT_PLAIN)
    .setMessageId("123")
    .setHeader("bar", "baz")
    .build();
Message message = MessageBuilder.withBody("foo".getBytes())
    .andProperties(props)
    .build();
```

在 [`MessageProperties`](https://docs.spring.io/spring-amqp/docs/latest-ga/api/org/springframework/amqp/core/MessageProperties.html) 上定义的每个属性都可以被设置。其他方法包括 `setHeader(String key, String value)`, `removeHeader(String key)`, `removeHeaders()`, 和 `copyProperties(MessageProperties properties)`。每个属性设置方法都有一个 `set*IfAbsent()` 变体。在存在默认初始值的情况下，该方法被命名为 `set*IfAbsentOrDefault()`。

提供了五个静态方法来创建一个初始的 message builder：

```java
public static MessageBuilder withBody(byte[] body) 

public static MessageBuilder withClonedBody(byte[] body) 

public static MessageBuilder withBody(byte[] body, int from, int to) 

public static MessageBuilder fromMessage(Message message) 

public static MessageBuilder fromClonedMessage(Message message) 
```

|      | 由 builder 创建的消息有一个 body ，是对参数的直接引用。      |
| ---- | ------------------------------------------------------------ |
|      | 由 builder 创建的消息有一个 body ，它是一个新的数组，包含参数中字节的副本。 |
|      | 由 builder 创建的消息有一个 body ，它是一个新的数组，包含参数中的字节范围。更多细节请参见 [`Arrays.copyOfRange()`](https://docs.oracle.com/javase/7/docs/api/java/util/Arrays.html)。 |
|      | 由 builder 创建的消息有一个 body，是对参数 body 的直接引用。参数的属性被复制到一个新的 `MessageProperties` 对象。 |
|      | 由 builder 创建的消息有一个 body，它是一个新的数组，包含一个参数主体的副本。参数的属性被复制到一个新的 `MessageProperties` 对象。 |

提供了三个静态方法来创建一个 `MessagePropertiesBuilder` 实例：

```java
public static MessagePropertiesBuilder newInstance() 

public static MessagePropertiesBuilder fromProperties(MessageProperties properties) 

public static MessagePropertiesBuilder fromClonedProperties(MessageProperties properties) 
```

|      | 一个新的消息属性（message properties）对象被初始化为默认值。 |
| ---- | ------------------------------------------------------------ |
|      | builder 被初始化，并且 `build()` 将返回所提供的属性对象。    |
|      | 参数的属性被复制到一个新的 `MessageProperties` 对象。        |

通过 `AmqpTemplate` 的 `RabbitTemplate` 实现，每个 `send()` 方法都有一个重载版本，它接收一个额外的 `CorrelationData` 对象。当发布者确认被启用时，该对象将在 [`AmqpTemplate`](https://springdoc.cn/spring-amqp/#amqp-template) 中描述的回调中返回。这让发送者将确认（`ack` 或 `nack`）与发送的消息相关联。

从 1.6.7 版本开始，引入了 `CorrelationAwareMessagePostProcessor` 接口，允许在消息被转换后修改相关数据。下面的例子显示了如何使用它：

```java
Message postProcessMessage(Message message, Correlation correlation);
```

在 2.0 版本中，这个接口被废弃了。该方法已被移至 `MessagePostProcessor`，其默认实现是委托给 `postProcessMessage(Message message)`。

从1.6.7版本开始，还提供了一个新的回调接口，称为 `CorrelationDataPostProcessor`。它在所有的 `MessagePostProcessor` 实例（在 `send()` 方法中提供的以及在 `setBeforePublishPostProcessors()` 中提供的那些实例）之后被调用。实现可以更新或替换 `send()` 方法中提供的相关数据（如果有的话）。`Message` 和原始 `CorrelationData`（如果有的话）被作为参数提供。下面的例子显示了如何使用 `postProcess` 方法：

```java
CorrelationData postProcess(Message message, CorrelationData correlationData);
```

##### Publisher 返回

当 template 的 `mandatory` 属性为 `true` 时，返回的消息由 [`AmqpTemplate`](https://springdoc.cn/spring-amqp/#amqp-template) 中描述的 callback 提供。

从 1.4 版开始，`RabbitTemplate` 支持 SpEL `mandatoryExpression` 属性，它作为根评估对象针对每个请求消息进行评估，解析为一个 `boolean` 值。Bean 引用，如 `@myBean.isMandatory(#root)`，可以在表达式中使用。

在发送和接收操作中，`RabbitTemplate` 也可以在内部使用 Publisher 返回。请参阅 [回复超时](https://springdoc.cn/spring-amqp/#reply-timeout) 以了解更多信息。

##### 批处理

1.4.2版本引入了 `BatchingRabbitTemplate`。这是 `RabbitTemplate` 的一个子类，具有一个重载的 `send` 方法，可根据 `BatchingStrategy` 对消息进行批处理。只有当批处理完成时，才将消息发送到 RabbitMQ。以下列表显示了 `BatchingStrategy` 接口的定义：

```java
public interface BatchingStrategy {

    MessageBatch addToBatch(String exchange, String routingKey, Message message);

    Date nextRelease();

    Collection<MessageBatch> releaseBatches();

}
```

|      | 批量的数据被保存在内存中。在系统发生故障时，未发送的信息可能会丢失。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

提供了一个 `SimpleBatchingStrategy`。它支持向单个 exchange 或 routing key 发送消息。它有以下属性：

- `batchSize`: 在发送前，一个批次的消息数量。
- `bufferLimit`: 批量消息的最大尺寸。如果超过了 `batchSize`，这就会抢占先机，导致部分批次被发送。
- `timeout`: 当没有新的活动添加消息到批处理时，部分批处理被发送的时间。

`SimpleBatchingStrategy` 通过在每个嵌入的消息前添加一个四字节的二进制长度来格式化批处理。这将通过将 `springBatchFormat` 消息属性设置为 `lengthHeader4` 传达给接收系统。

|      | 分批的消息默认情况下被监听器容器自动去掉分批（通过使用 `springBatchFormat` message header）。拒绝一个批处理中的任何消息都会导致整个批处理被拒绝。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

然而，更多信息请参见 [带有批处理功能的 @RabbitListener](https://springdoc.cn/spring-amqp/#receiving-batch)。

#### 4.1.6. 接收消息

信息接收总是比发送要复杂一些。有两种方法来接收一个 `Message`。比较简单的方法是通过调用轮询方法，一次轮询一个 `Message`。更复杂但更常见的方法是注册一个监听器，按需、异步地接收 `Message`。在接下来的两个小节中，我们将考虑每种方法的一个例子。

##### 消费者轮询

`AmqpTemplate` 本身可以用于轮询 `Message` 的接收。默认情况下，如果没有可用的消息，会立即返回 `null`。不存在阻塞问题。从1.5版本开始，你可以设置一个接收时间（`receiveTimeout`），以毫秒为单位，接收方法最多阻塞这么长时间，等待消息。一个小于0的值意味着无限期地阻塞（或者至少到与 broker 的连接丢失为止）。1.6版本引入了 `receive` 方法的变体，允许在每次调用时传入 timeout 时间。

|      | 由于接收操作为每个消息创建一个新的 `QueueingConsumer`，这种技术其实并不适合大批量的环境。可以考虑使用异步消费者或 `receiveTimeout` 为零的方式来处理这些用例。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

从2.4.8版本开始，当使用非零 timeout 时，你可以指定传入 `basicConsume` 方法的参数，用于将消费者与 channel 联系起来。例如：`template.addConsumerArg("x-priority", 10)`。

有四种简单的 `receive` 方法可用。与发送端的 `Exchange` 一样，有一种方法要求直接在 template 上设置默认队列属性，还有一种方法在运行时接受队列参数。1.6 版本引入了接受 `timeoutMillis` 的变体，在每个请求的基础上覆盖 `receiveTimeout`。下面的列表显示了这四个方法的定义：

```java
Message receive() throws AmqpException;

Message receive(String queueName) throws AmqpException;

Message receive(long timeoutMillis) throws AmqpException;

Message receive(String queueName, long timeoutMillis) throws AmqpException;
```

和发送消息的情况一样，`AmqpTemplate` 有一些方便的方法来接收 POJO 而不是 `Message` 实例，实现提供了一种方法来定制用于创建返回的 `Object` 的 `MessageConverter`： 下面的列表显示了这些方法：

```java
Object receiveAndConvert() throws AmqpException;

Object receiveAndConvert(String queueName) throws AmqpException;

Object receiveAndConvert(long timeoutMillis) throws AmqpException;

Object receiveAndConvert(String queueName, long timeoutMillis) throws AmqpException;
```

从2.0版本开始，这些方法有一些变体，接受一个额外的 `ParameterizedTypeReference` 参数来转换复杂类型。该 template 必须配置有 `SmartMessageConverter`。有关详细信息，请参阅 [使用 `RabbitTemplate` 从 `Message` 转换](https://springdoc.cn/spring-amqp/#json-complex)。

与 `sendAndReceive` 方法类似，从1.3版本开始，`AmqpTemplate` 有几个方便的 `receiveAndReply` 方法用于同步接收、处理和回复消息。下面的列表显示了这些方法的定义：

```java
<R, S> boolean receiveAndReply(ReceiveAndReplyCallback<R, S> callback)
       throws AmqpException;

<R, S> boolean receiveAndReply(String queueName, ReceiveAndReplyCallback<R, S> callback)
     throws AmqpException;

<R, S> boolean receiveAndReply(ReceiveAndReplyCallback<R, S> callback,
    String replyExchange, String replyRoutingKey) throws AmqpException;

<R, S> boolean receiveAndReply(String queueName, ReceiveAndReplyCallback<R, S> callback,
    String replyExchange, String replyRoutingKey) throws AmqpException;

<R, S> boolean receiveAndReply(ReceiveAndReplyCallback<R, S> callback,
     ReplyToAddressCallback<S> replyToAddressCallback) throws AmqpException;

<R, S> boolean receiveAndReply(String queueName, ReceiveAndReplyCallback<R, S> callback,
            ReplyToAddressCallback<S> replyToAddressCallback) throws AmqpException;
```

`AmqpTemplate` 的实现负责 `receive` 和 `reply` 阶段的工作。在大多数情况下，你应该只提供一个 `ReceiveAndReplyCallback` 的实现，以便为收到的消息执行一些业务逻辑，并在需要时建立一个回复对象或消息。注意，`ReceiveAndReplyCallback` 可以返回 `null`。在这种情况下，没有回复被发送，`receiveAndReply` 的工作方式与 `receive` 方法类似。这使得同一个队列可以用于混合的消息，其中一些可能不需要回复（reply）。

只有当提供的回调不是 `ReceiveAndReplyMessageCallback` 的实例时，才会应用自动消息（请求和回复）转换，该回调提供了一个原始消息交换契约。

`ReplyToAddressCallback` 对于需要自定义逻辑以在运行时根据收到的消息和来自 `ReceiveAndReplyCallback` 的回复确定 `replyTo` 地址的情况非常有用。默认情况下，请求消息中的 `replyTo` 信息被用于路由回复。

下面列出了一个基于POJO的接收和回复的例子：

```java
boolean received =
        this.template.receiveAndReply(ROUTE, new ReceiveAndReplyCallback<Order, Invoice>() {

                public Invoice handle(Order order) {
                        return processOrder(order);
                }
        });
if (received) {
        log.info("We received an order!");
}
```

##### 异步消费者

|      | Spring AMQP 还通过使用 `@RabbitListener` 注解支持注解的监听器端点，并提供了一个开放的基础设施来以编程方式注册端点。这是迄今为止设置异步消费者的最方便的方法。请参阅 [注解驱动的 Listener 端点](https://springdoc.cn/spring-amqp/#async-annotation-driven) 以了解更多细节。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | prefetch 的默认值曾经是1，这可能导致有效消费者的利用不足。从2.0版本开始，默认的 prefetch 值现在是250，这应该在大多数常见的情况下使消费者保持忙碌，从而提高吞吐量。然而，在有些情况下，prefetch 值应该很低：对于大型消息，特别是在处理速度较慢的情况下（消息可能会在客户端进程中增加大量的内存）。当需要进行严格的消息排序时（在这种情况下，prefetch 值应被设回1）。其他特殊情况另外，对于低容量的消息传递和多个消费者（包括单个监听器容器实例内的并发性），你可能希望减少 prefetch，以便在消费者之间获得更均匀的消息分布。见 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes)。有关 prefetch 的更多背景，请参阅这篇关于 [RabbitMQ 中消费者利用率](https://www.rabbitmq.com/blog/2014/04/14/finding-bottlenecks-with-rabbitmq-3-3/) 的文章和这篇关于 [队列理论](https://www.rabbitmq.com/blog/2012/05/11/some-queuing-theory-throughput-latency-and-bandwidth/) 的文章。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

###### 消息监听器

对于异步 `Message` 的接收，涉及到一个专门的组件（不是 `AmqpTemplate`）。该组件是一个消费 `Message` 的回调的容器。我们将在本节后面考虑这个容器和它的属性。但首先，我们应该看一下回调，因为这是你的应用程序代码与消息系统集成的地方。回调有几个选项，首先是 `MessageListener` 接口的实现，下面的列表显示了这一点：

```java
public interface MessageListener {
    void onMessage(Message message);
}
```

如果你的回调逻辑由于任何原因依赖于 AMQP Channel 实例，你可以使用 `ChannelAwareMessageListener`。它看起来类似，但有一个额外的参数。下面的列表显示了 `ChannelAwareMessageListener` 的接口定义：

```java
public interface ChannelAwareMessageListener {
    void onMessage(Message message, Channel channel) throws Exception;
}
```

|      | 在 2.1 版本中，这个接口从 `o.s.amqp.rabbit.core` 包转移到 `o.s.amqp.rabbit.listener.api`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

###### `MessageListenerAdapter`

如果你希望在你的应用逻辑和消息传递API之间保持更严格的分离，你可以依靠框架提供的适配器（adapter）实现。这通常被称为 "消息驱动的POJO" 支持。

|      | 1.5 版本为POJO消息传递引入了一个更灵活的机制，即 `@RabbitListener` 注解。更多信息请参见 [注解驱动的 Listener 端点](https://springdoc.cn/spring-amqp/#async-annotation-driven)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

当使用适配器时，你只需要提供一个对适配器本身应该调用的实例的引用。下面的例子显示了如何做到这一点：

```java
MessageListenerAdapter listener = new MessageListenerAdapter(somePojo);
listener.setDefaultListenerMethod("myMethod");
```

你可以对 adapter 进行子类化，并提供 `getListenerMethodName()` 的实现，以根据消息动态地选择不同的方法。这个方法有两个参数，`originalMessage` 和 `extractedMessage`，后者是任何转换的结果。默认情况下，配置了一个 `SimpleMessageConverter`。参见 [`SimpleMessageConverter`](https://springdoc.cn/spring-amqp/#simple-message-converter)，了解更多信息和其他可用的转换器（converter）的信息。

从1.4.2版本开始，original message 有 `consumerQueue` 和 `consumerTag` 属性，可以用来确定接收消息的队列。

从 1.5 版本开始，你可以配置一个消费者队列或标签到方法名称的映射，以动态地选择要调用的方法。如果 map 中没有条目，我们就会返回到默认的监听器方法。默认的监听器方法（如果没有设置）是 `handleMessage`。

从 2.0 版本开始，我们提供了一个方便的 `FunctionalInterface`。下面列出了 `FunctionalInterface` 的定义：

```java
@FunctionalInterface
public interface ReplyingMessageListener<T, R> {

    R handleMessage(T t);

}
```

这个接口通过使用Java 8 lambdas方便地配置适配器，如下面的例子所示：

```java
new MessageListenerAdapter((ReplyingMessageListener<String, String>) data -> {
    ...
    return result;
}));
```

从2.2版本开始，`buildListenerArguments(Object)` 已经被弃用，新的 `buildListenerArguments(Object, Channel, Message)` 已经被引入。这个新方法帮助监听器获得 `Channel` 和 `Message` 参数来做更多的事情，比如在手动确认模式下调用 `channel.basicReject(long, boolean)`。下面的列表显示了最基本的例子：

```java
public class ExtendedListenerAdapter extends MessageListenerAdapter {

    @Override
    protected Object[] buildListenerArguments(Object extractedMessage, Channel channel, Message message) {
        return new Object[]{extractedMessage, channel, message};
    }

}
```

现在，如果你需要接收 “channel” 和 “message”，你可以将 `ExtendedListenerAdapter` 配置成与 `MessageListenerAdapter` 一样。listener 的参数应该被设置为 `buildListenerArguments(Object, Channel, Message)` 返回，正如下面的 listener 例子所示：

```java
public void handleMessage(Object object, Channel channel, Message message) throws IOException {
    ...
}
```

###### 容器

现在你已经看到了 `Message` 监听回调的各种选项，我们可以把注意力转向容器。基本上，容器处理 "主动" 的责任，以便监听器回调可以保持被动。容器是一个 "生命周期" 组件的例子。它提供了启动和停止的方法。当配置容器时，你基本上是在AMQP队列和 `MessageListener` 实例之间架起了桥梁。你必须提供对 `ConnectionFactory` 的引用和队列名称或队列实例，该监听器应该从这些队列中消费消息。

在 2.0 版本之前，有一个监听器容器，即 `SimpleMessageListenerContainer`。现在有了第二个容器，`DirectMessageListenerContainer`。容器之间的区别以及你在选择使用哪个容器时可能应用的标准在 [选择容器](https://springdoc.cn/spring-amqp/#choose-container) 中有所描述。

下面的列表显示了最基本的例子，它通过使用，`SimpleMessageListenerContainer` 来工作：

```java
SimpleMessageListenerContainer container = new SimpleMessageListenerContainer();
container.setConnectionFactory(rabbitConnectionFactory);
container.setQueueNames("some.queue");
container.setMessageListener(new MessageListenerAdapter(somePojo));
```

作为一个 "活动" 组件，最常见的是用Bean定义来创建监听器容器，这样它就可以在后台运行。下面的例子显示了用XML来做的一种方法：

```xml
<rabbit:listener-container connection-factory="rabbitConnectionFactory">
    <rabbit:listener queues="some.queue" ref="somePojo" method="handle"/>
</rabbit:listener-container>
```

下面的列表显示了用XML的另一种方式：

```xml
<rabbit:listener-container connection-factory="rabbitConnectionFactory" type="direct">
    <rabbit:listener queues="some.queue" ref="somePojo" method="handle"/>
</rabbit:listener-container>
```

前面的两个例子都创建了一个 `DirectMessageListenerContainer`（注意 `type` 属性—它默认为 `simple`）。

另外，你可能更喜欢使用Java配置，它看起来与前面的代码片断相似：

```java
@Configuration
public class ExampleAmqpConfiguration {

    @Bean
    public SimpleMessageListenerContainer messageListenerContainer() {
        SimpleMessageListenerContainer container = new SimpleMessageListenerContainer();
        container.setConnectionFactory(rabbitConnectionFactory());
        container.setQueueName("some.queue");
        container.setMessageListener(exampleListener());
        return container;
    }

    @Bean
    public CachingConnectionFactory rabbitConnectionFactory() {
        CachingConnectionFactory connectionFactory =
            new CachingConnectionFactory("localhost");
        connectionFactory.setUsername("guest");
        connectionFactory.setPassword("guest");
        return connectionFactory;
    }

    @Bean
    public MessageListener exampleListener() {
        return new MessageListener() {
            public void onMessage(Message message) {
                System.out.println("received: " + message);
            }
        };
    }
}
```

###### 消费者优先级

从 RabbitMQ 3.2 版开始，该broker现在支持消费者优先级（参见 [使用 RabbitMQ 的消费者优先级](https://www.rabbitmq.com/blog/2013/12/16/using-consumer-priorities-with-rabbitmq/)）。这可通过在消费者上设置 `x-priority` 参数来启用。`SimpleMessageListenerContainer` 现在支持设置消费者参数，如以下示例所示：

```java
container.setConsumerArguments(Collections.
<String, Object> singletonMap("x-priority", Integer.valueOf(10)));
```

为了方便，命名空间在 `listener` 元素上提供了 `priority` 属性，如下例所示：

```xml
<rabbit:listener-container connection-factory="rabbitConnectionFactory">
    <rabbit:listener queues="some.queue" ref="somePojo" method="handle" priority="10" />
</rabbit:listener-container>
```

从 1.3 版开始，你可以修改容器在运行时监听的队列。参见 [监听器容器队列](https://springdoc.cn/spring-amqp/#listener-queues)。

###### `auto-delete` 队列

当一个容器被配置为监听 `auto-delete` 队列时，队列有一个 `x-expires` 选项，或者在 Broker 上配置了 [Time-To-Live](https://www.rabbitmq.com/ttl.html) 策略，当容器停止时（也就是取消最后一个消费者时），队列会被broker删除。在 1.3 版之前，由于队列丢失，容器不能被重新启动。`RabbitAdmin` 只在连接关闭或打开时自动重新声明队列等，这在容器停止和启动时不会发生。

从 1.3 版开始，容器在启动期间使用 `RabbitAdmin` 来重新声明任何缺失的队列。

你也可以使用条件性声明（见 [条件式声明](https://springdoc.cn/spring-amqp/#conditional-declaration)）和 `auto-startup="false"` admin 一起，将队列声明推迟到容器启动之后。下面的例子显示了如何做到这一点：

```xml
<rabbit:queue id="otherAnon" declared-by="containerAdmin" />

<rabbit:direct-exchange name="otherExchange" auto-delete="true" declared-by="containerAdmin">
    <rabbit:bindings>
        <rabbit:binding queue="otherAnon" key="otherAnon" />
    </rabbit:bindings>
</rabbit:direct-exchange>

<rabbit:listener-container id="container2" auto-startup="false">
    <rabbit:listener id="listener2" ref="foo" queues="otherAnon" admin="containerAdmin" />
</rabbit:listener-container>

<rabbit:admin id="containerAdmin" connection-factory="rabbitConnectionFactory"
    auto-startup="false" />
```

在这种情况下，queue 和 exchange 是由 `containerAdmin` 声明的，它有 `auto-startup="false"`，所以这些元素在 context 初始化时不会被声明。另外，由于同样的原因，容器也没有被启动。当容器后来被启动时，它使用它对 `containerAdmin` 的引用来声明这些元素。

##### 批处理

分批的消息（由生产者创建）被监听器容器自动去掉分批（使用 `springBatchFormat` message header）。拒绝一个批处理中的任何消息都会导致整个批处理被拒绝。关于批处理的更多信息，见 [批处理](https://springdoc.cn/spring-amqp/#template-batching)。

从 2.2 版本开始，`SimpleMessageListenerContainer` 可以用来在消费者端创建批次（生产者发送不连续的消息）。

设置容器属性 `consumerBatchEnabled` 以启用该功能。`deBatchingEnabled` 也必须为 `true`，以便容器负责处理两种类型的批次。当 `consumerBatchEnabled` 为 `true` 时，实现 `BatchMessageListener` 或 `ChannelAwareBatchMessageListener`。从 2.2.7 版本开始，`SimpleMessageListenerContainer` 和 `DirectMessageListenerContainer` 都可以以 `List<Message>` 的形式对 [生产者创建的批次](https://springdoc.cn/spring-amqp/#template-batching) 进行去掉分批。请参阅 [带有批处理功能的 @RabbitListener](https://springdoc.cn/spring-amqp/#receiving-batch) 了解关于使用 `@RabbitListener` 的这个功能的信息。

##### 消费者事件

每当一个 listener 遇到某种异常时，容器就会发布应用事件。事件 `ListenerContainerConsumerFailedEvent` 有以下属性：

- `container`: 消费者遇到异常的 listener 容器。
- `reason`: 失败的文本原因。
- `fatal`: 一个布尔值，表示异常是否是致命的。对于非致命的异常，容器会根据 `recoveryInterval` 或 `recoveryBackoff`（对于 `SimpleMessageListenerContainer`）或 `monitorInterval`（对于 `DirectMessageListenerContainer`）尝试重新启动消费者。
- `throwable`: 被捕获的 `Throwable`。

这些事件可以通过实现 `ApplicationListener<ListenerContainerConsumerFailedEvent>` 来消费。

|      | 当 `concurrentConsumers` 大于1时，全系统的事件（如连接失败）由所有消费者发布。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

如果一个消费者因为它的一个队列被 exclusively （独占的）而失败，默认情况下，以及发布事件，会发出一个 `WARN` 日志。要改变这种记录行为，在 `SimpleMessageListenerContainer` 实例的 `exclusiveConsumerExceptionLogger` 属性中提供一个自定义的 `ConditionalExceptionLogger`。也请看 [记录 Channel 关闭事件](https://springdoc.cn/spring-amqp/#channel-close-logging)。

致命的错误总是被记录在 `ERROR` 级别。这是不可以修改的。

其他几个事件在容器生命周期的不同阶段被发布：

- `AsyncConsumerStartedEvent`: 当消费者被启动时。
- `AsyncConsumerRestartedEvent`: 当消费者在失败后被重新启动时 - 仅限 `SimpleMessageListenerContainer`。
- `AsyncConsumerTerminatedEvent`: 当一个消费者被正常停止时。
- `AsyncConsumerStoppedEvent`: 当消费者被停止时 - 仅限 `SimpleMessageListenerContainer。`
- `ConsumeOkEvent`: 当收到来自 broker 的 `consumerOk` 时，包含队列名称和 `consumerTag`。
- `ListenerContainerIdleEvent`: 见 [检测闲置的异步消费者](https://springdoc.cn/spring-amqp/#idle-containers)。
- `MissingQueueEvent`: 当检测到一个缺失的队列时。

##### 消费者标签（Tag）

你可以提供一个策略来生成消费者标签（consumer tag）。默认情况下，消费者标签是由 broker 生成的。下面的列表显示了 `ConsumerTagStrategy` 的接口定义：

```java
public interface ConsumerTagStrategy {

    String createConsumerTag(String queue);

}
```

该队列是可用的，以便它可以（可选择地）在标签中使用。

见 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes)。

##### 注解驱动的 Listener 端点

异步接收消息的最简单方法是使用注解的 listener 端点基础设施。简而言之，它允许你将一个托管 Bean 的方法作为 Rabbit listener 端点公开。下面的例子显示了如何使用 `@RabbitListener` 注解：

```java
@Component
public class MyService {

    @RabbitListener(queues = "myQueue")
    public void processOrder(String data) {
        ...
    }

}
```

前面例子的意思是，只要名为 `myQueue` 的队列上有一个消息，`processOrder` 方法就会被相应地调用（在这种情况下，用消息的 payload）。

注解的端点基础设施通过使用 `RabbitListenerContainerFactory`，在幕后为每个注解的方法创建一个消息监听器容器。

在前面的例子中，`myQueue` 必须已经存在并被绑定到一些 exchange 中。只要 application context 中存在一个 `RabbitAdmin`，就可以自动声明和绑定队列。

|      | 属性占位符（`${some.property}`）或SpEL表达式（`#{someExpression}`）可以为注解属性（`queues` 等）指定。关于为什么使用SpEL而不是属性占位符的例子，请参见 [监听多个队列](https://springdoc.cn/spring-amqp/#annotation-multiple-queues)。下面的列表显示了如何声明 Rabbit listener 的三个例子： |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

```java
@Component
public class MyService {

  @RabbitListener(bindings = @QueueBinding(
        value = @Queue(value = "myQueue", durable = "true"),
        exchange = @Exchange(value = "auto.exch", ignoreDeclarationExceptions = "true"),
        key = "orderRoutingKey")
  )
  public void processOrder(Order order) {
    ...
  }

  @RabbitListener(bindings = @QueueBinding(
        value = @Queue,
        exchange = @Exchange(value = "auto.exch"),
        key = "invoiceRoutingKey")
  )
  public void processInvoice(Invoice invoice) {
    ...
  }

  @RabbitListener(queuesToDeclare = @Queue(name = "${my.queue}", durable = "true"))
  public String handleWithSimpleDeclare(String data) {
      ...
  }

}
```

在第一个例子中，如果需要的话，一个队列 `myQueue` 和exchange一起被自动声明（durable），并通过 routing key 绑定到 exchange。在第二个例子中，一个匿名（独占、自动删除）队列被声明和绑定；队列名称是由框架使用 `Base64UrlNamingStrategy` 创建。你不能用这种技术声明 broker 命名的队列；它们需要被声明为 bean 定义；见 [容器和 Broker 命名的队列](https://springdoc.cn/spring-amqp/#containers-and-broker-named-queues)。可以提供多个 `QueueBinding` 项，让监听器监听多个队列。在第三个例子中，声明了一个队列，其名称是从属性 `my.queue` 中检索出来的，如果有必要的话，用队列名称作为路由键（routing key），将默认绑定到默认 exchange。

从2.0版本开始，`@Exchange` 注解支持任何 exchange 类型，包括自定义。欲了解更多信息，请参见 [AMQP概念](https://www.rabbitmq.com/tutorials/amqp-concepts.html)。

当你需要更高级的配置时，你可以使用普通的 `@Bean` 定义。

注意第一个例子中 exchange 的 `ignoreDeclarationExceptions`。例如，这允许绑定到可能有不同设置（如内部）的现有 exchange。默认情况下，一个现有 exchange 的属性必须匹配。

从 2.0 版本开始，你现在可以把一个队列绑定到一个有多个 routing key 的 exchange 中，如下例所示：

```java
...
    key = { "red", "yellow" }
...
```

你也可以在 `@QueueBinding` 注解中为 queue、exchange 和 binding 指定参数，如下例所示：

```java
@RabbitListener(bindings = @QueueBinding(
        value = @Queue(value = "auto.headers", autoDelete = "true",
                        arguments = @Argument(name = "x-message-ttl", value = "10000",
                                                type = "java.lang.Integer")),
        exchange = @Exchange(value = "auto.headers", type = ExchangeTypes.HEADERS, autoDelete = "true"),
        arguments = {
                @Argument(name = "x-match", value = "all"),
                @Argument(name = "thing1", value = "somevalue"),
                @Argument(name = "thing2")
        })
)
public String handleWithHeadersExchange(String foo) {
    ...
}
```

注意，`x-message-ttl` 参数被设置为10秒的队列。由于参数类型不是 `String`，我们必须指定其类型—在本例中是 `Integer`。与所有此类声明一样，如果队列已经存在，参数必须与队列上的参数相匹配。对于 header exchange，我们将 binding 参数设置为匹配那些将 `thing1` header 设置为 `somevalue` 的消息，而 `thing2` header 必须以任何值出现。`x-match` 参数意味着两个条件都必须满足。

参数名称、值和类型可以是属性占位符（`${…}`）或SpEL表达式（`#{…}`）。`name` 必须解析为一个 `String`。`type` 的表达式必须解析为一个 `Class` 或一个类的全名称。`value` 必须解析为可以被 `DefaultConversionService` 转换为该类型的东西（比如前面例子中的 `x-message-ttl`）。

如果一个名字被解析为 `null` 或空的 `String`，该 `@Argument` 将被忽略。

###### 元注解

有时你可能想对多个监听器（listener）使用相同的配置。为了减少模板配置，你可以使用元注解来创建你自己的监听器注解。下面的例子展示了如何做到这一点：

```java
@Target({ElementType.TYPE, ElementType.METHOD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@RabbitListener(bindings = @QueueBinding(
        value = @Queue,
        exchange = @Exchange(value = "metaFanout", type = ExchangeTypes.FANOUT)))
public @interface MyAnonFanoutListener {
}

public class MetaListener {

    @MyAnonFanoutListener
    public void handle1(String foo) {
        ...
    }

    @MyAnonFanoutListener
    public void handle2(String foo) {
        ...
    }

}
```

在前面的例子中，由 `@MyAnonFanoutListener` 注解创建的每个监听器都将一个匿名的、自动删除的队列绑定到 fanout exchange，`metaFanout`。从2.2.3版本开始，支持 `@AliasFor`，允许覆盖元注解的属性。另外，用户注解现在可以是 `@Repeatable`，允许为一个方法创建多个容器。

```java
@Component
static class MetaAnnotationTestBean {

    @MyListener("queue1")
    @MyListener("queue2")
    public void handleIt(String body) {
    }

}


@RabbitListener
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Repeatable(MyListeners.class)
static @interface MyListener {

    @AliasFor(annotation = RabbitListener.class, attribute = "queues")
    String[] value() default {};

}

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
static @interface MyListeners {

    MyListener[] value();

}
```

###### 启用 Listener Endpoint 注解

要启用对 `@RabbitListener` 注解的支持，你可以在你的一个 `@Configuration` 类中添加 `@EnableRabbit`。下面的例子展示了如何做到这一点：

```java
@Configuration
@EnableRabbit
public class AppConfig {

    @Bean
    public SimpleRabbitListenerContainerFactory rabbitListenerContainerFactory() {
        SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
        factory.setConnectionFactory(connectionFactory());
        factory.setConcurrentConsumers(3);
        factory.setMaxConcurrentConsumers(10);
        factory.setContainerCustomizer(container -> /* customize the container */);
        return factory;
    }
}
```

从 2.0 版本开始，`DirectMessageListenerContainerFactory` 也可用。它可以创建 `DirectMessageListenerContainer` 实例。

|      | 关于帮助你在 `SimpleRabbitListenerContainerFactory` 和 `DirectRabbitListenerContainerFactory` 之间进行选择的信息，请参阅 [选择容器](https://springdoc.cn/spring-amqp/#choose-container)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

从 2.2.2 版开始，你可以提供一个 `ContainerCustomizer` 实现（如上图所示）。这可以用来在容器被创建和配置后进一步配置它；例如，你可以用它来设置容器工厂没有暴露的属性。

2.4.8 版提供了 `CompositeContainerCustomizer`，以满足你希望应用多个自定义器的情况。

默认情况下，基础设施会寻找一个名为 `rabbitListenerContainerFactory` 的 bean 作为工厂的来源，以用于创建消息监听器容器。在这种情况下，并忽略 RabbitMQ 基础设施的设置，`processOrder` 方法可以在核心轮询规模为三个线程和最大池规模为十个线程时被调用。

你可以自定义要为每个注解使用的监听器容器工厂，或者你可以通过实现 `RabbitListenerConfigurer` 接口来配置一个显式默认。只有在至少有一个端点被注册而没有特定的容器工厂时，才需要默认。请参阅 [Javadoc](https://docs.spring.io/spring-amqp/docs/latest-ga/api/org/springframework/amqp/rabbit/annotation/RabbitListenerConfigurer.html) 以了解完整的细节和示例。

容器工厂提供了添加 `MessagePostProcessor` 实例的方法，这些实例在接收消息之后（在调用监听器之前）和发送回复之前被应用。

关于回复的信息，请参见 [回复管理](https://springdoc.cn/spring-amqp/#async-annotation-driven-reply)。

从 2.0.6 版本开始，你可以向监听器容器工厂添加 `RetryTemplate` 和 `RecoveryCallback`。它在发送回复时使用。`RecoveryCallback` 在重试用尽时被调用。你可以使用 `SendRetryContextAccessor` 来从上下文中获取信息。下面的例子展示了如何做到这一点：

```java
factory.setRetryTemplate(retryTemplate);
factory.setReplyRecoveryCallback(ctx -> {
    Message failed = SendRetryContextAccessor.getMessage(ctx);
    Address replyTo = SendRetryContextAccessor.getAddress(ctx);
    Throwable t = ctx.getLastThrowable();
    ...
    return null;
});
```

如果你喜欢XML配置，你可以使用 `<rabbit:annotation-driven>` 元素。任何用 `@RabbitListener` 注解的 bean 都会被检测到。

对于 `SimpleRabbitListenerContainer` 实例，你可以使用类似以下的XML：

```xml
<rabbit:annotation-driven/>

<bean id="rabbitListenerContainerFactory"
      class="org.springframework.amqp.rabbit.config.SimpleRabbitListenerContainerFactory">
    <property name="connectionFactory" ref="connectionFactory"/>
    <property name="concurrentConsumers" value="3"/>
    <property name="maxConcurrentConsumers" value="10"/>
</bean>
```

对于 `DirectMessageListenerContainer` 实例，你可以使用类似以下的XML：

```xml
<rabbit:annotation-driven/>

<bean id="rabbitListenerContainerFactory"
      class="org.springframework.amqp.rabbit.config.DirectRabbitListenerContainerFactory">
    <property name="connectionFactory" ref="connectionFactory"/>
    <property name="consumersPerQueue" value="3"/>
</bean>
```

从 2.0 版本开始，`@RabbitListener` 注解有一个 `concurrency` 属性。它支持SpEL表达式（`#{…}`）和属性占位符（`${…}`）。它的含义和允许的值取决于容器的类型，如下所示：

- 对于 `DirectMessageListenerContainer`，该值必须是一个单一的 integer 值，它设置容器上的 `consumerPerQueue` 属性。
- 对于 `SimpleRabbitListenerContainer` 来说，这个值可以是一个单一的 integer 值，用来设置容器上的 `concurrentConsumers` 属性，或者它的形式是 `m-n`，其中 `m` 是 `concurrentConsumers` 属性，`n` 是 `maxConcurrentConsumers` 属性。

在这两种情况下，这个设置都会覆盖工厂的设置。以前，如果你有需要不同 concurrency 的监听器，你就必须定义不同的容器工厂。

该注解还允许通过 `autoStartup` 和 `executor`（自2.2起）注解属性覆盖工厂的 `autoStartup` 和 `taskExecutor` 属性。为每个人使用不同的 executor 可能有助于在日志和线程 dump 中识别与每个监听器相关的线程。

2.2 版本还添加了 `ackMode` 属性，允许你覆盖容器工厂的 `acknowledgeMode` 属性。

```java
@RabbitListener(id = "manual.acks.1", queues = "manual.acks.1", ackMode = "MANUAL")
public void manual1(String in, Channel channel,
    @Header(AmqpHeaders.DELIVERY_TAG) long tag) throws IOException {

    ...
    channel.basicAck(tag, false);
}
```

###### 注解方法的消息转换

在调用监听器之前，流水线（pipeline）上有两个转换步骤。第一步使用 `MessageConverter` 将传入的 Spring AMQP `Message` 转换为 Spring-messaging `Message`。当目标方法被调用时，如果有必要，消息的 payload 将被转换为方法的参数类型。

第一步的默认 `MessageConverter` 是一个 Spring AMQP `SimpleMessageConverter`，它可以处理转换为 `String` 和 `java.io.Serializable` 对象。其他的都保持为 `byte[]`。在下面的讨论中，我们称其为 "消息转换器"（message converter）。

第二步的默认转换器是一个 `GenericMessageConverter`，它委托给一个转换服务（`DefaultFormattingConversionService` 的一个实例）。在下面的讨论中，我们称其为 "方法参数转换器"（method argument converter）。

要改变消息转换器，你可以把它作为一个属性添加到容器工厂bean中。下面的例子显示了如何做到这一点：

```java
@Bean
public SimpleRabbitListenerContainerFactory rabbitListenerContainerFactory() {
    SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
    ...
    factory.setMessageConverter(new Jackson2JsonMessageConverter());
    ...
    return factory;
}
```

这配置了一个 `Jackson2` 转换器，它期望有 header 信息来指导转换。

你也可以使用一个 `ContentTypeDelegatingMessageConverter`，它可以处理不同内容类型的转换。

从 2.3 版本开始，你可以通过在 `messageConverter` 属性中指定一个 bean 名称来覆盖工厂转换器。

```java
@Bean
public Jackson2JsonMessageConverter jsonConverter() {
    return new Jackson2JsonMessageConverter();
}

@RabbitListener(..., messageConverter = "jsonConverter")
public void listen(String in) {
    ...
}
```

这就避免了为了改变转换器（converter）而必须声明一个不同的容器工厂。

在大多数情况下，没有必要定制方法参数转换器，除非，例如，你想使用一个自定义的 `ConversionService`。

在 1.6 之前的版本中，转换 JSON 的类型信息必须在消息 header 中提供，或者需要一个自定义的 `ClassMapper`。从 1.6 版本开始，如果没有类型信息 header，类型可以从目标方法参数中推断出来。

|      | 这种类型推导只对方法级别的 `@RabbitListener` 有效。 |
| ---- | --------------------------------------------------- |
|      |                                                     |

更多信息见 [Jackson2JsonMessageConverter](https://springdoc.cn/spring-amqp/#json-message-converter)。

如果你想定制方法参数转换器，你可以按以下方式进行：

```java
@Configuration
@EnableRabbit
public class AppConfig implements RabbitListenerConfigurer {

    ...

    @Bean
    public DefaultMessageHandlerMethodFactory myHandlerMethodFactory() {
        DefaultMessageHandlerMethodFactory factory = new DefaultMessageHandlerMethodFactory();
        factory.setMessageConverter(new GenericMessageConverter(myConversionService()));
        return factory;
    }

    @Bean
    public DefaultConversionService myConversionService() {
        DefaultConversionService conv = new DefaultConversionService();
        conv.addConverter(mySpecialConverter());
        return conv;
    }

    @Override
    public void configureRabbitListeners(RabbitListenerEndpointRegistrar registrar) {
        registrar.setMessageHandlerMethodFactory(myHandlerMethodFactory());
    }

    ...

}
```

|      | 对于多方法监听器（见 [多方法监听器](https://springdoc.cn/spring-amqp/#annotation-method-selection)），方法的选择是基于消息转换后的消息的 payload。方法参数转换器只有在方法被选择后才会被调用。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

###### 在 @RabbitListener 中添加一个自定义的 `HandlerMethodArgumentResolver`

从 2.3.7 版开始，你能够添加你自己的 `HandlerMethodArgumentResolver` 并解析自定义方法参数。你所需要的是实现 `RabbitListenerConfigurer` 并使用来自类 `RabbitListenerEndpointRegistrar` 的 `setCustomMethodArgumentResolvers()` 方法。

```java
@Configuration
class CustomRabbitConfig implements RabbitListenerConfigurer {

    @Override
    public void configureRabbitListeners(RabbitListenerEndpointRegistrar registrar) {
        registrar.setCustomMethodArgumentResolvers(
				new HandlerMethodArgumentResolver() {

					@Override
					public boolean supportsParameter(MethodParameter parameter) {
						return CustomMethodArgument.class.isAssignableFrom(parameter.getParameterType());
					}

					@Override
					public Object resolveArgument(MethodParameter parameter, org.springframework.messaging.Message<?> message) {
						return new CustomMethodArgument(
								(String) message.getPayload(),
								message.getHeaders().get("customHeader", String.class)
						);
					}

				}
			);
    }

}
```

###### 编程式端点注册

`RabbitListenerEndpoint` 提供了一个 Rabbit 端点的模型，并负责为该模型配置容器。除了由 `RabbitListener` 注解检测到的端点外，该基础设施还允许你以编程方式配置端点。下面的示例显示了如何做到这一点：

```java
@Configuration
@EnableRabbit
public class AppConfig implements RabbitListenerConfigurer {

    @Override
    public void configureRabbitListeners(RabbitListenerEndpointRegistrar registrar) {
        SimpleRabbitListenerEndpoint endpoint = new SimpleRabbitListenerEndpoint();
        endpoint.setQueueNames("anotherQueue");
        endpoint.setMessageListener(message -> {
            // processing
        });
        registrar.registerEndpoint(endpoint);
    }
}
```

在前面的例子中，我们使用了 `SimpleRabbitListenerEndpoint`，它提供了实际的 `MessageListener` 来调用，但你也可以建立你自己的端点变量来描述一个自定义的调用机制。

应该注意的是，你也可以完全跳过 `@RabbitListener` 的使用，并通过 `RabbitListenerConfigurer` 以编程方式注册你的端点。

###### 注解端点的方法签名

到目前为止，我们一直在我们的端点中注入一个简单的 `String`，但它实际上可以有一个非常灵活的方法签名。下面的例子重写了它，注入带有自定义 header 的 `Order`：

```java
@Component
public class MyService {

    @RabbitListener(queues = "myQueue")
    public void processOrder(Order order, @Header("order_type") String orderType) {
        ...
    }
}
```

下面的列表显示了可与监听器（listener）端点中的参数相匹配的参数：

- 原始的 `org.springframework.amqp.core.Message`。
- 来自原始 `Message` 的 `MessageProperties`。
- 接收消息的 `com.rabbitmq.client.Channel`。
- `org.springframework.messaging.Message` 由传入的 AMQP message 转换而来。
- `@Header` 注解的方法参数，提取特定的 header 值，包括标准的 AMQP header。
- `@Headers` 注解的参数，也必须可分配给 `java.util.Map` 以获得对所有 header 的访问。
- 转换后的 payload。

不属于支持的类型（即 `Message`、`MessageProperties`、`Message<?>` 和 `Channel`）的非注解元素将与 payload 匹配。你可以通过给参数加上 `@Payload` 的注解来明确这一点。你也可以通过添加一个额外的 `@Valid` 来打开验证功能。

注入 Spring 的 message abstraction 的能力特别有用，可以从存储在 transport 专用消息中的所有信息中获益，而不必依赖 transport 专用的API。下面的例子展示了如何做到这一点：

```java
@RabbitListener(queues = "myQueue")
public void processOrder(Message<Order> order) { ...
}
```

方法参数的处理由 `DefaultMessageHandlerMethodFactory` 提供，你可以进一步定制以支持额外的方法参数。转换和验证支持也可以在这里定制。

例如，如果我们想在处理 `Order` 之前确保它是有效的，我们可以用 `@Valid` 注解 payload，并配置必要的 validator，如下所示：

```java
@Configuration
@EnableRabbit
public class AppConfig implements RabbitListenerConfigurer {

    @Override
    public void configureRabbitListeners(RabbitListenerEndpointRegistrar registrar) {
        registrar.setMessageHandlerMethodFactory(myHandlerMethodFactory());
    }

    @Bean
    public DefaultMessageHandlerMethodFactory myHandlerMethodFactory() {
        DefaultMessageHandlerMethodFactory factory = new DefaultMessageHandlerMethodFactory();
        factory.setValidator(myValidator());
        return factory;
    }
}
```

###### @RabbitListener @Payload 验证

从 2.3.7 版本开始，现在更容易添加 `Validator` 来验证 `@RabbitListener` 和 `@RabbitHandler` `@Payload` 参数。现在，你可以简单地将 validator 添加到注册器本身。

```java
@Configuration
@EnableRabbit
public class Config implements RabbitListenerConfigurer {
    ...
    @Override
    public void configureRabbitListeners(RabbitListenerEndpointRegistrar registrar) {
      registrar.setValidator(new MyValidator());
    }
}
```

|      | 当使用 Spring Boot 与 validation starter 时，会自动配置一个 `LocalValidatorFactoryBean`： |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

```java
@Configuration
@EnableRabbit
public class Config implements RabbitListenerConfigurer {
    @Autowired
    private LocalValidatorFactoryBean validator;
    ...
    @Override
    public void configureRabbitListeners(RabbitListenerEndpointRegistrar registrar) {
      registrar.setValidator(this.validator);
    }
}
```

进行验证：

```java
public static class ValidatedClass {
  @Max(10)
  private int bar;
  public int getBar() {
    return this.bar;
  }
  public void setBar(int bar) {
    this.bar = bar;
  }
}
```

以及

```java
@RabbitListener(id="validated", queues = "queue1", errorHandler = "validationErrorHandler",
      containerFactory = "jsonListenerContainerFactory")
public void validatedListener(@Payload @Valid ValidatedClass val) {
    ...
}
@Bean
public RabbitListenerErrorHandler validationErrorHandler() {
    return (m, e) -> {
        ...
    };
}
```

###### 监听多个队列

当你使用 `queues` 属性时，你可以指定相关的容器可以监听多个队列。你可以使用 `@Header` 注解来使 POJO 方法可以得到接收消息的队列名称。下面的例子显示了如何做到这一点：

```java
@Component
public class MyService {

    @RabbitListener(queues = { "queue1", "queue2" } )
    public void processOrder(String data, @Header(AmqpHeaders.CONSUMER_QUEUE) String queue) {
        ...
    }

}
```

从 1.5 版本开始，你可以通过使用属性占位符和SpEL来外化队列名称。下面的例子显示了如何做到这一点：

```java
@Component
public class MyService {

    @RabbitListener(queues = "#{'${property.with.comma.delimited.queue.names}'.split(',')}" )
    public void processOrder(String data, @Header(AmqpHeaders.CONSUMER_QUEUE) String queue) {
        ...
    }

}
```

在 1.5 版本之前，只有一个队列可以这样指定。每个队列都需要一个单独的属性。

###### 回复管理

`MessageListenerAdapter` 的现有支持已经允许你的方法有一个非 void 的返回类型。在这种情况下，调用的结果被封装在一条 message 中，发送到原始消息的 `ReplyToAddress` header 中指定的地址，或者发送到 listener 上配置的默认地址。你可以通过使用 messaging abstraction 的 `@SendTo` 注解来设置该默认地址。

假设我们的 `processOrder` 方法现在应该返回一个 `OrderStatus`，我们可以这样写，以自动发送一个回复：

```java
@RabbitListener(destination = "myQueue")
@SendTo("status")
public OrderStatus processOrder(Order order) {
    // order processing
    return status;
}
```

如果你需要以独立于 transport 的方式设置额外的 header 信息，你可以返回一个 `Message` 来代替，就像下面这样：

```java
@RabbitListener(destination = "myQueue")
@SendTo("status")
public Message<OrderStatus> processOrder(Order order) {
    // order processing
    return MessageBuilder
        .withPayload(status)
        .setHeader("code", 1234)
        .build();
}
```

另外，你可以在 `beforeSendReplyMessagePostProcessors` 容器工厂属性中使用一个 `MessagePostProcessor` 来添加更多的 header。从2.2.3版本开始，被调用的 Bean/Method 在回复 message 中是可用的，它可以在 message 后处理器中使用，将 message 传回给调用者：

```java
factory.setBeforeSendReplyPostProcessors(msg -> {
    msg.getMessageProperties().setHeader("calledBean",
            msg.getMessageProperties().getTargetBean().getClass().getSimpleName());
    msg.getMessageProperties().setHeader("calledMethod",
            msg.getMessageProperties().getTargetMethod().getName());
    return m;
});
```

从 2.2.5 版本开始，你可以配置一个 `ReplyPostProcessor` 来在发送前修改回复 message；它在 `correlationId` header 被设置为与请求相匹配之后被调用。

```java
@RabbitListener(queues = "test.header", group = "testGroup", replyPostProcessor = "echoCustomHeader")
public String capitalizeWithHeader(String in) {
    return in.toUpperCase();
}

@Bean
public ReplyPostProcessor echoCustomHeader() {
    return (req, resp) -> {
        resp.getMessageProperties().setHeader("myHeader", req.getMessageProperties().getHeader("myHeader"));
        return resp;
    };
}
```

从 3.0 版本开始，你可以在容器工厂而不是注解上配置后置处理器。

```java
factory.setReplyPostProcessorProvider(id -> (req, resp) -> {
    resp.getMessageProperties().setHeader("myHeader", req.getMessageProperties().getHeader("myHeader"));
    return resp;
});
```

`id` 参数是 listener ID。

注解上的设置将取代工厂的设置。

`@SendTo` 值被假定为遵循 `exchange/routingKey` 模式的回复 `exchange` 和 `routingKey` 对，其中有一个部分可以省略。有效值如下：

- `thing1/thing2`: `replyTo` exchange 和 `routingKey`。
- `thing1/`: `replyTo` exchange 和默认（空）`routingKey`。
- `thing2` 或 `/thing2`: `replyTo` `routingKey` 和默认（空）exchange。
- `/` 或空: `replyTo` 默认的 exchange 和默认的 `routingKey`。

另外，你可以使用 `@SendTo` 而不使用一个 `value` 属性。这种情况相当于一个空的 `sendTo` 模式。只有在入站 message 没有 `replyToAddress` 属性时，才会使用 `@SendTo`。

从 1.5 版本开始，`@SendTo` 的值可以是一个 bean 初始化的 SpEL 表达式，如下例所示：

```java
@RabbitListener(queues = "test.sendTo.spel")
@SendTo("#{spelReplyTo}")
public String capitalizeWithSendToSpel(String foo) {
    return foo.toUpperCase();
}
...
@Bean
public String spelReplyTo() {
    return "test.sendTo.reply.spel";
}
```

该表达式必须评估为一个 `String`，它可以是一个简单的队列名称（发送到默认的 exchange），也可以是前面的例子中讨论的 `exchange/routingKey` 的形式。

|      | `#{…}` 表达式在初始化时被评估一次。 |
| ---- | ----------------------------------- |
|      |                                     |

对于动态回复路由，消息发送者应该包括一个 `reply_to` message 属性或使用备用的运行时SpEL表达式（在下一个例子后描述）。

从 1.6 版本开始，`@SendTo` 可以是一个SpEL表达式，在运行时针对请求和回复进行评估，如下例所示：

```java
@RabbitListener(queues = "test.sendTo.spel")
@SendTo("!{'some.reply.queue.with.' + result.queueName}")
public Bar capitalizeWithSendToSpel(Foo foo) {
    return processTheFooAndReturnABar(foo);
}
```

SpEL表达式的运行时性质用 `!{…}` 分隔符表示。表达式的评估环境 `#root` 对象有三个属性：

- `request`: `o.s.amqp.core.Message` 请求对象。
- `source`: 转换后的 `o.s.messaging.Message<?>`。
- `result`: 该方法的结果。

context 有一个 map 属性访问器、一个标准的类型转换器和一个 Bean 解析器，它可以让 context 中的其他Bean被引用（例如，`@someBeanName.determineReplyQ(request, result)`）。

总之，`#{…}` 在初始化过程中被评估一次，`#root` 对象是 application context。bean是通过其名称来引用的。`!{…}` 在运行时对每个 message 进行评估，root 对象具有前面列出的属性。bean是用它们的名字来引用的，前缀是 `@`。

从 2.1 版本开始，也支持简单的属性占位符（例如，`${some.reply.to}`）。对于早期的版本，可以用下面的方法来解决，如下例所示：

```java
@RabbitListener(queues = "foo")
@SendTo("#{environment['my.send.to']}")
public String listen(Message in) {
    ...
    return ...
}
```

###### 回复内容类型（ContentType）

如果你使用一个复杂的消息转换器，比如 `ContentTypeDelegatingMessageConverter`，你可以通过在监听器上设置 `replyContentType` 属性来控制回复的内容类型。这允许转换器为回复选择适当的委托转换器（delegate converter）。

```java
@RabbitListener(queues = "q1", messageConverter = "delegating",
        replyContentType = "application/json")
public Thing2 listen(Thing1 in) {
    ...
}
```

默认情况下，为了向后兼容，转换器设置的任何内容类型属性在转换后都会被这个值覆盖。转换器（如 `SimpleMessageConverter`）使用回复类型（reply type）而不是内容类型（content type）来确定需要的转换，并在回复 message 中适当地设置内容类型。这可能不是所需的操作，可以通过将 `converterWinsContentType` 属性设置为 `false` 来重写。例如，如果你返回一个包含 JSON 的 `String`，`SimpleMessageConverter` 将把回复中的内容类型设置为 `text/plain`。下面的配置将确保内容类型被正确设置，即使使用了 `SimpleMessageConverter`。

```java
@RabbitListener(queues = "q1", replyContentType = "application/json",
        converterWinsContentType = "false")
public String listen(Thing in) {
    ...
    return someJsonString;
}
```

这些属性（`replyContentType` 和 `converterWinsContentType`）不适用于返回类型为 Spring AMQP `Message` 或 Spring Messaging `Message<?>` 的情况。在第一种情况下，不涉及转换；只需设置 `contentType` message 属性。在第二种情况下，该行为是通过 message header 来控制的：

```java
@RabbitListener(queues = "q1", messageConverter = "delegating")
@SendTo("q2")
public Message<String> listen(String in) {
    ...
    return MessageBuilder.withPayload(in.toUpperCase())
            .setHeader(MessageHeaders.CONTENT_TYPE, "application/xml")
            .build();
}
```

这个 content type 将在 `MessageProperties` 中传递给转换器。默认情况下，为了向后兼容，转换器设置的任何 content type 属性在转换后都会被这个值覆盖。如果你想覆盖这一行为，也可以将 `AmqpHeaders.CONTENT_TYPE_CONVERTER_WINS` 设置为 `true`，转换器设置的任何值将被保留。

###### 多方法监听器

从 1.5.0 版本开始，你可以在类的层面上指定 `@RabbitListener` 注解。与新的 `@RabbitHandler` 注解一起，这可以让一个监听器根据传入消息的 payload 类型来调用不同的方法。这最好用一个例子来描述：

```java
@RabbitListener(id="multi", queues = "someQueue")
@SendTo("my.reply.queue")
public class MultiListenerBean {

    @RabbitHandler
    public String thing2(Thing2 thing2) {
        ...
    }

    @RabbitHandler
    public String cat(Cat cat) {
        ...
    }

    @RabbitHandler
    public String hat(@Header("amqp_receivedRoutingKey") String rk, @Payload Hat hat) {
        ...
    }

    @RabbitHandler(isDefault = true)
    public String defaultMethod(Object object) {
        ...
    }

}
```

在这种情况下，如果转换后的 payload 是 `Thing2`、`Cat` 或 `Hat`，就会调用各个 `@RabbitHandler` 方法。你应该明白，系统必须能够根据 payload 的类型来识别一个独特的方法。该类型被检查为可分配给一个没有注解或被 `@Payload` 注解的单一参数。注意，同样的方法签名也适用，就像在方法级的 `@RabbitListener` 中讨论的那样（[前面描述过](https://springdoc.cn/spring-amqp/#message-listener-adapter)）。

从 2.0.3 版本开始，一个 `@RabbitHandler` 方法可以被指定为默认方法，如果其他方法不匹配，就会调用该方法。最多有一个方法可以被这样指定。

|      | `@RabbitHandler` 仅用于处理转换后的消息 payload，如果你希望接收未转换的原始 `Message` 对象，你必须在方法上使用 `@RabbitListener`，而不是在类上。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

###### `@Repeatable` `@RabbitListener`

从 1.6 版本开始，`@RabbitListener` 注解被标记为 `@Repeatable`。这意味着该注解可以多次出现在同一个被注解的元素（方法或类）上。在这种情况下，为每个注解创建一个单独的监听器容器，每个容器都会调用同一个监听器 `@Bean`。可重复注解可以在 Java 8 或以上版本中使用。

###### 代理 `@RabbitListener` 和泛型

如果你的服务打算被代理（例如，在 `@Transactional` 的情况下），当接口有泛型参数时，你应该牢记一些注意事项。考虑一下下面的例子：

```java
interface TxService<P> {

   String handle(P payload, String header);

}

static class TxServiceImpl implements TxService<Foo> {

    @Override
    @RabbitListener(...)
    public String handle(Thing thing, String rk) {
         ...
    }

}
```

对于一个泛型接口和一个特定的实现，你不得不切换到 CGLIB 目标类代理，因为接口 `handle` 方法的实际实现是一个桥接方法。在事务管理的情况下，CGLIB 的使用是通过使用注解选项来配置的： `@EnableTransactionManagement(proxyTargetClass = true)`。而在这种情况下，所有的注解都必须在实现中的目标方法上声明，正如下面的例子所示：

```java
static class TxServiceImpl implements TxService<Foo> {

    @Override
    @Transactional
    @RabbitListener(...)
    public String handle(@Payload Foo foo, @Header("amqp_receivedRoutingKey") String rk) {
        ...
    }

}
```

###### 处理异常

默认情况下，如果注解的监听器方法抛出一个异常，它将被抛给容器，并且消息将被重新排队并重新交付、丢弃或被路由到一个死信 exchange，这取决于容器和 broker 的配置。没有什么会返回给发送者。

从 2.0 版本开始，`@RabbitListener` 注解有两个新属性：`errorHandler` 和 `returnExceptions`。

这些都是默认不配置的。

你可以使用 `errorHandler` 来提供 `RabbitListenerErrorHandler` 实现的 bean 名称。该 functional interface 有一个方法，如下所示：

```java
@FunctionalInterface
public interface RabbitListenerErrorHandler {

    Object handleError(Message amqpMessage, org.springframework.messaging.Message<?> message,
              ListenerExecutionFailedException exception) throws Exception;

}
```

正如你所看到的，你可以访问从容器中收到的原始消息，由消息转换器产生的 spring-messaging `Message<?>` 对象，以及由 listener 抛出的异常（被封装在 `ListenerExecutionFailedException` 中）。error handler 可以返回一些结果（作为回复发送），或者抛出原始的或新的异常（根据 `returnExceptions` 的设置，被抛给容器或返回给发送者）。

`returnExceptions` 属性为 `true` 时，会导致异常被返回给发送者。异常被封装在一个 `RemoteInvocationResult` 对象中。在发送方，有一个可用的 `RemoteInvocationAwareMessageConverterAdapter`，如果配置到 `RabbitTemplate` 中，它将重新抛出服务器端的异常，并将其封装为 `AmqpRemoteException`。服务器异常的 stack trace 是通过合并服务器和客户端的 stack trace 来合成的。

|      | 这种机制通常只适用于默认的 `SimpleMessageConverter`，它使用 Java serialization。异常通常不是 "Jackson友好" 的，不能被序列化为JSON。如果你使用JSON，考虑使用 `errorHandler`，当抛出异常时，返回一些其他对 Jackson 友好的 `Error` 对象。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 在 2.1 版本中，这个接口从 `o.s.amqp.rabbit.listener` 包转移到 `o.s.amqp.rabbit.listener.api`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

从 2.1.7 版本开始，`Channel` 可以在消息 header 中使用；当使用 `AcknowledgeMode.MANUAL` 时，这允许你对失败的消息进行确认或否定：

```java
public Object handleError(Message amqpMessage, org.springframework.messaging.Message<?> message,
          ListenerExecutionFailedException exception) {
              ...
              message.getHeaders().get(AmqpHeaders.CHANNEL, Channel.class)
                  .basicReject(message.getHeaders().get(AmqpHeaders.DELIVERY_TAG, Long.class),
                               true);
          }
```

从 2.2.18 版本开始，如果抛出一个消息转换异常，error handler 将被调用，`message` 参数为 `null`。这允许应用程序向调用者发送一些结果，表明收到了一个格式不好的消息。以前，这种错误是由容器抛出并处理的。

###### 容器管理

为注解创建的容器不在 application context 中注册。你可以通过调用 `RabbitListenerEndpointRegistry` Bean 上的 `getListenerContainers()` 来获得所有容器的集合。然后你可以遍历此集合，例如，停止或启动所有容器，或调用 registry 本身的 `Lifecycle` 方法，这将调用每个容器上的操作。

你也可以通过使用它的 `id` 来获得单个容器的引用，使用 `getListenerContainer(String id)` — 例如，`registry.getListenerContainer("multi")` 用于上面的片段所创建的容器。

从 1.5.2 版本开始，你可以通过 `getListenerContainerIds()` 获得注册容器的 `id` 值。

从 1.5 版开始，你现在可以为 `RabbitListener` 端点上的容器分配一个 `group`。这提供了一种机制来获取对容器子集的引用。添加 `group` 属性会导致一个类型为 `Collection<MessageListenerContainer>` 的 Bean 以该组名称在 context 中注册。

##### 带有批处理功能的 @RabbitListener

当接收 [一批](https://springdoc.cn/spring-amqp/#template-batching) 消息时，通常由容器进行去批处理，而监听器被调用，每次调用一条消息。从 2.2 版本开始，你可以配置监听器容器工厂和监听器，以便在一次调用中接收整个批次，只需设置工厂的 `batchListener` 属性，并使方法的 payload 参数为 `List` 或 `Collection`：

```java
@Bean
public SimpleRabbitListenerContainerFactory rabbitListenerContainerFactory() {
    SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
    factory.setConnectionFactory(connectionFactory());
    factory.setBatchListener(true);
    return factory;
}

@RabbitListener(queues = "batch.1")
public void listen1(List<Thing> in) {
    ...
}

// or

@RabbitListener(queues = "batch.2")
public void listen2(List<Message<Thing>> in) {
    ...
}
```

将 `batchListener` 属性设置为 `true` 会自动关闭工厂创建的容器中的 `deBatchingEnabled` 容器属性（除非 `consumerBatchEnabled` 为 `true`，见下文）。实际上，去批处理被从容器移到了 listener adapter 上，adapter 创建的列表被传递给 listener。

启用批处理的工厂不能与 [多方法 listener](https://springdoc.cn/spring-amqp/#annotation-method-selection) 一起使用。

同样从2.2版开始，当逐一接收分批消息时，最后一条消息包含一个 boolean header，设置为 `true`。这个头可以通过添加 `@Header(AmqpHeaders.LAST_IN_BATCH) boolean last` 参数到你的监听器方法来获得。这个 header 是由 `MessageProperties.isLastInBatch()` 映射出来的。此外，`AmqpHeaders.BATCH_SIZE` 被填充为每个消息片段中的批次大小。

此外，一个新的属性 `consumerBatchEnabled` 已经被添加到 `SimpleMessageListenerContainer` 中。当该属性为 `true` 时，容器将创建一批消息，数量不超过 `batchSize`；如果 `receiveTimeout` 过后没有新的消息到达，将交付部分批次。如果收到生产者创建的批处理，它就会被拆分并添加到消费者侧的批处理中；因此，实际交付的消息数可能超过 `batchSize`，它代表从 broker 那里收到的消息数。 当 `consumerBatchEnabled` 为 `true` 时，`deBatchingEnabled` 必须为 `true`；容器工厂将执行这一要求。

```java
@Bean
public SimpleRabbitListenerContainerFactory consumerBatchContainerFactory() {
    SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
    factory.setConnectionFactory(rabbitConnectionFactory());
    factory.setConsumerTagStrategy(consumerTagStrategy());
    factory.setBatchListener(true); // configures a BatchMessageListenerAdapter
    factory.setBatchSize(2);
    factory.setConsumerBatchEnabled(true);
    return factory;
}
```

当使用 `consumerBatchEnabled` 和 `@RabbitListener` 时：

```java
@RabbitListener(queues = "batch.1", containerFactory = "consumerBatchContainerFactory")
public void consumerBatch1(List<Message> amqpMessages) {
    ...
}

@RabbitListener(queues = "batch.2", containerFactory = "consumerBatchContainerFactory")
public void consumerBatch2(List<org.springframework.messaging.Message<Invoice>> messages) {
    ...
}

@RabbitListener(queues = "batch.3", containerFactory = "consumerBatchContainerFactory")
public void consumerBatch3(List<Invoice> strings) {
    ...
}
```

- 第一个调用的是收到的未经转换的 `org.springframework.amqp.core.Message`。
- 第二个调用的是 `org.springframework.messaging.Message<?>`，并带有转换的 payload 和映射的 header/properties。
- 第三个是用转换后的 payload 来调用，不能访问 header/properties。

你还可以添加一个 `Channel` 参数，通常在使用 `MANUAL` ack 模式时使用。这在第三个例子中不是很有用，因为你不能访问 `delivery_tag` 属性。

Spring Boot 为 `consumerBatchEnabled` 和 `batchSize` 提供了一个配置属性，但没有为 `batchListener` 提供。从 3.0 版本开始，在容器工厂上将 `consumerBatchEnabled` 设置为 `true`，也将 `batchListener` 设置为 `true`。当 `consumerBatchEnabled` 为 `true` 时，listener 必须是一个批处理 listener。

Starting with version 3.0, listener methods can consume `Collection<?>` or `List<?>`.

从 3.0 版本开始，listener 方法可以消费 `Collection<?>` 或 `List<?>`。

##### 使用容器工厂

Listener 容器工厂被引入以支持 `@RabbitListener` 和用 `RabbitListenerEndpointRegistry` 注册容器，正如在 [编程式端点注册](https://springdoc.cn/spring-amqp/#async-annotation-driven-registration) 中讨论的那样。

从 2.1 版本开始，它们可以被用来创建任何监听器（listener）容器 — 甚至是没有监听器的容器（比如在Spring Integration 中使用）。当然，监听器必须在容器启动前被添加。

有两种方法来创建这种容器：

- 使用 `SimpleRabbitListenerEndpoint`
- 创建后添加监听器

下面的例子显示了如何使用 `SimpleRabbitListenerEndpoint` 来创建一个监听器容器：

```java
@Bean
public SimpleMessageListenerContainer factoryCreatedContainerSimpleListener(
        SimpleRabbitListenerContainerFactory rabbitListenerContainerFactory) {
    SimpleRabbitListenerEndpoint endpoint = new SimpleRabbitListenerEndpoint();
    endpoint.setQueueNames("queue.1");
    endpoint.setMessageListener(message -> {
        ...
    });
    return rabbitListenerContainerFactory.createListenerContainer(endpoint);
}
```

下面的例子显示了如何在创建后添加监听器：

```java
@Bean
public SimpleMessageListenerContainer factoryCreatedContainerNoListener(
        SimpleRabbitListenerContainerFactory rabbitListenerContainerFactory) {
    SimpleMessageListenerContainer container = rabbitListenerContainerFactory.createListenerContainer();
    container.setMessageListener(message -> {
        ...
    });
    container.setQueueNames("test.no.listener.yet");
    return container;
}
```

在这两种情况下，监听器也可以是 `ChannelAwareMessageListener`，因为它现在是 `MessageListener` 的一个子接口。

如果你希望创建几个具有类似属性的容器，或者使用一个预配置的容器工厂，比如Spring Boot自动配置提供的容器工厂，或者两者兼而有之，那么这些技术就很有用。

|      | 以这种方式创建的容器是普通的 `@Bean` 实例，并且没有在 `RabbitListenerEndpointRegistry` 中注册。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 异步的 `@RabbitListener` 返回类型

`@RabbitListener`（和 `@RabbitHandler`）方法可以指定异步返回类型 `CompletableFuture<?>` 和 `Mono<?>`，让回复被异步发送。`ListenableFuture<?>` 不再被支持；它已被 Spring Framework 废弃。

|      | 监听器容器工厂必须被配置为 `AcknowledgeMode.MANUAL`，这样消费者线程就不会对消息进行确认；相反，异步 completion 将在异步操作完成后对消息进行确认或拒绝。当异步结果完成时出现错误，消息是否被重新获取取决于抛出的异常类型、容器配置和容器错 error handler。默认情况下，消息将被重新排队，除非容器的 `defaultRequeueRejected` 属性被设置为 `false`（默认为 `true`）。如果异步结果以 `AmqpRejectAndDontRequeueException` 完成，那么消息将不会被重新获取。如果容器的 `defaultRequeueRejected` 属性为 `false`，你可以通过将 future 的异常设置为 `ImmediateRequeueException` 来覆盖它，消息将被重新获取。如果在监听器方法中发生了一些异常，导致无法创建异步结果对象，那么你必须捕获该异常，并返回一个适当的返回对象，这将导致消息被确认或重新获取。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

从 2.2.21、2.3.13、2.4.1 版本开始，当检测到异步返回类型时，`AcknowledgeMode` 将自动设置为 `MANUAL`。此外，带有致命异常的传入消息将被单独否定确认，之前任何未被确认的消息也被否定确认。

##### 线程和异步消费者

许多不同的线程都涉及到异步消费者。

在 `SimpleMessageListenerContainer` 中配置的 `TaskExecutor` 的线程被用来在 `RabbitMQ Client` 交付新消息时调用 `MessageListener`。如果没有配置，则使用 `SimpleAsyncTaskExecutor`。如果你使用池式 executor，你需要确保池的大小足以处理配置的并发性。通过 `DirectMessageListenerContainer`，`MessageListener` 被直接在 `RabbitMQ Client` 线程上调用。在这种情况下，`taskExecutor` 被用于监视消费者的任务。

|      | 当使用默认的 `SimpleAsyncTaskExecutor` 时，对于监听器被调用的线程，监听器容器的 `beanName` 被用于 `threadNamePrefix`。这对日志分析很有用。我们通常建议在日志应用者配置中始终包括线程名称。当通过容器上的 `taskExecutor` 属性特别提供了一个 `TaskExecutor` 时，它被原封不动地使用，无需修改。建议你使用类似的技术来命名由自定义 `TaskExecutor` Bean定义创建的线程，以帮助在日志消息中识别线程。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

在 `CachingConnectionFactory` 中配置的 `Executor` 在创建连接时被传入 `RabbitMQ Client`，其线程被用来向监听器容器传递新消息。如果没有配置，客户端将使用一个内部线程池执行器，（在编写本文时）每个连接的线程池大小为 `Runtime.getRuntime().availableProcessors() * 2`。

如果你有大量的工厂或使用 `CacheMode.CONNECTION`，你可能会考虑使用一个共享的 `ThreadPoolTaskExecutor`，它有足够的线程来满足你的工作负荷。

|      | 对于 `DirectMessageListenerContainer`，你需要确保连接工厂配置了一个 task executor，该 task executor 有足够的线程来支持你所期望的跨所有使用该工厂的 listener 容器的并发性。默认的池大小（在撰写本文时）是 `Runtime.getRuntime().availableProcessors() * 2`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

The `RabbitMQ client` uses a `ThreadFactory` to create threads for low-level I/O (socket) operations. To modify this factory, you need to configure the underlying RabbitMQ `ConnectionFactory`, as discussed in [配置底层客户端连接工厂（Connection Factory）](https://springdoc.cn/spring-amqp/#connection-factory).

`RabbitMQ client` 使用 `ThreadFactory` 来为底层 I/O（套接字）操作创建线程。要修改该工厂，你需要配置底层 RabbitMQ `ConnectionFactory`，如 [配置底层客户端连接工厂（Connection Factory）](https://springdoc.cn/spring-amqp/#connection-factory) 中所讨论的。

##### 选择容器

2.0 版本引入了 `DirectMessageListenerContainer`（DMLC）。在此之前，只有 `SimpleMessageListenerContainer`（SMLC）可用。SMLC为每个消费者使用一个内部队列和一个专用线程。如果一个容器被配置为监听多个队列，同一个消费者线程被用来处理所有的队列。并发性是由 `concurrentConsumers` 和其它属性控制的。当消息从 RabbitMQ 客户端到达时，客户端线程通过队列将它们移交给消费者线程。之所以需要这种架构，是因为在 RabbitMQ 客户端的早期版本中，不可能有多个并发的交付。新版本的客户端有一个修订的线程模型，现在可以支持并发。这允许引入 DMLC，其中监听器现在直接在 RabbitMQ 客户端线程上被调用。因此，其架构实际上比 SMLC "更简单"。然而，这种方法有一些限制，SMLC 的某些功能在 DMLC 中是不可用的。另外，并发性是由 `consumersPerQueue`（和客户端库的线程池）控制的。`concurrentConsumers` 和相关属性在这个容器中是不可用的。

以下是SMLC的功能，但不是DMLC：

- `batchSize`: 对于SMLC，你可以设置它来控制在一个事务中传递多少消息，或者减少 ack 的数量，但它可能导致失败后重复传递的数量增加。（DMLC确实有 `messagesPerAck`，你可以用它来减少ack，与 `batchSize` 和SMLC一样，但它不能与事务一起使用—每条消息都是在一个单独的事务中传递和ack的）。
- `consumerBatchEnabled`: 启用消费者中离散消息的批处理；更多信息见 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes)。
- `maxConcurrentConsumers` 和消费者缩放的时间间隔或触发器 - 在DMLC中没有自动缩放。然而，它确实让你以编程方式改变 `consumersPerQueue` 属性，消费者也会相应地被调整。

然而，与SMLC相比，DMLC有以下好处：

- 在运行时添加和删除队列是更有效的。使用SMLC，整个消费者线程被重新启动（所有消费者被取消并重新创建）。有了DMLC，未受影响的消费者就不会被取消。
- 避免了 RabbitMQ 客户端线程和消费者线程之间的上下文切换。
- 线程是在消费者之间共享的，而不是在SMLC中为每个消费者提供一个专用线程。但是，请看 [线程和异步消费者](https://springdoc.cn/spring-amqp/#threading) 中关于连接工厂配置的重要说明。

请参阅 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes)，了解哪些配置属性适用于每个容器。

##### 检测闲置的异步消费者

虽然高效，但异步消费者的一个问题是检测它们何时闲置—如果一段时间内没有消息到达，用户可能想采取一些行动。

从 1.6 版本开始，现在可以配置监听器容器，使其在一段时间没有消息传递时发布 `ListenerContainerIdleEvent`。当容器处于空闲状态时，每隔几毫秒就会发布一个事件，即 `idleEventInterval`。

为了配置这个功能，在容器上设置 `idleEventInterval`。下面的例子显示了如何在XML和Java中这样做（对于 `SimpleMessageListenerContainer` 和 `SimpleRabbitListenerContainerFactory`）：

```xml
<rabbit:listener-container connection-factory="connectionFactory"
        ...
        idle-event-interval="60000"
        ...
        >
    <rabbit:listener id="container1" queue-names="foo" ref="myListener" method="handle" />
</rabbit:listener-container>
@Bean
public SimpleMessageListenerContainer(ConnectionFactory connectionFactory) {
    SimpleMessageListenerContainer container = new SimpleMessageListenerContainer(connectionFactory);
    ...
    container.setIdleEventInterval(60000L);
    ...
    return container;
}
@Bean
public SimpleRabbitListenerContainerFactory rabbitListenerContainerFactory() {
    SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
    factory.setConnectionFactory(rabbitConnectionFactory());
    factory.setIdleEventInterval(60000L);
    ...
    return factory;
}
```

在上述每种情况下，当容器处于空闲状态时，每分钟发布一次事件。

###### 事件消费

你可以通过实现 `ApplicationListener` 来捕获空闲事件—要么是一个泛型的监听器，要么是一个缩小的只接收这个特定事件的监听器。你也可以使用 `@EventListener`，在Spring Framework 4.2 中引入。

下面的例子将 `@RabbitListener` 和 `@EventListener` 合并为一个类。你需要明白，应用程序监听器会获得所有容器的事件，所以如果你想根据哪个容器的空闲情况采取特定的行动，你可能需要检查 listener ID。你也可以为此目的使用 `@EventListener` `condition` 。

这些事件有四个属性：

- `source`: 监听器容器实例。
- `id`: 监听器ID（或容器Bean名称）。
- `idleTime`: 事件发布时，容器闲置的时间。
- `queueNames`: 容器所监听的队列的名称。

下面的例子展示了如何通过使用 `@RabbitListener` 和 `@EventListener` 注解来创建监听器：

```Java
public class Listener {

    @RabbitListener(id="someId", queues="#{queue.name}")
    public String listen(String foo) {
        return foo.toUpperCase();
    }

    @EventListener(condition = "event.listenerId == 'someId'")
    public void onApplicationEvent(ListenerContainerIdleEvent event) {
        ...
    }

}
```

|      | 事件监听器会看到所有容器的事件。因此，在前面的例子中，我们根据 listener ID 来缩小所收到的事件。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 如果你想使用空闲事件（idle event）来停止 listener 容器，你不应该在调用监听器的线程上调用 `container.stop()`。这样做总是导致延迟和不必要的日志信息。相反，你应该把事件交给一个不同的线程，然后让它停止容器。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 监测监听器的性能

从2.2版本开始，如果在类路径上检测到 `Micrometer`，并且在应用上下文中存在一个 `MeterRegistry`（或者正好有一个被注解为 `@Primary`，比如使用Spring Boot时），监听器容器将为监听器自动创建并更新Micrometer `Timer`。timer 可以通过设置容器属性 `micrometerEnabled` 为 `false` 来禁用。

有两个 timer 被维护—一个是成功调用监听器的 timer，一个是失败的 timer。对于一个简单的 `MessageListener`，每个配置的队列都有一对 timer。

这些 timer 被命名为 `spring.rabbitmq.listener`，并具有以下标签：

- `listenerId` : (监听器ID或容器Bean名称)
- `queue` : (当 `consumerBatchEnabled` 为 `true` 时，简单监听器的队列名称或配置的队列名称列表—因为一个批次可能包含来自多个队列的消息)
- `result` : `success` 或 `failure`
- `exception` : `none` 或 `ListenerExecutionFailedException`

你可以使用 `micrometerTags` 容器属性添加额外的标签。

另见 [Micrometer 观察](https://springdoc.cn/spring-amqp/#micrometer-observation)。

##### Micrometer 观察

从 3.0 版本开始，`RabbitTemplate` 和监听器容器现在支持使用 Micrometer 进行观察。

在每个组件上设置 `observationEnabled` 以启用观察；这将禁用 [Micrometer Timer](https://springdoc.cn/spring-amqp/#micrometer)，因为现在 timer 将随着每次观察而被管理。

更多信息请参考 [Micrometer Tracing](https://micrometer.io/docs/tracing)。

要向 timer/trace 添加tag，请分别向 template 或 listener container 配置自定义 `RabbitTemplateObservationConvention` 或 `RabbitListenerObservationConvention`。

默认实现为 template observation 添加 `bean.name` 标签，为容器添加 `listener.id` tag。

你可以对 `DefaultRabbitTemplateObservationConvention` 或 `DefaultRabbitListenerObservationConvention` 进行子类化，或者提供全新的实现。

更多细节见 [Micrometer Observation 文档](https://springdoc.cn/spring-amqp/#observation-gen)。

#### 4.1.7. 容器和 Broker 命名的队列

虽然最好使用 `AnonymousQueue` 实例作为自动删除队列，但从 2.1 版开始，你可以使用带有监听器容器的 broker 命名队列。下面的例子显示了如何做到这一点：

```java
@Bean
public Queue queue() {
    return new Queue("", false, true, true);
}

@Bean
public SimpleMessageListenerContainer container() {
    SimpleMessageListenerContainer container = new SimpleMessageListenerContainer(cf());
    container.setQueues(queue());
    container.setMessageListener(m -> {
        ...
    });
    container.setMissingQueuesFatal(false);
    return container;
}
```

注意名称的空 `String`。当 `RabbitAdmin` 声明队列时，它用 broker 返回的名称更新 `Queue.actualName` 属性。你必须在配置容器时使用 `setQueues()` 以使其发挥作用，这样容器就可以在运行时访问声明的名称。仅仅设置这些名称是不够的。

|      | 当容器运行时，你不能将 broker 命名的队列添加到容器中。 |
| ---- | ------------------------------------------------------ |
|      |                                                        |

|      | 当连接被重置并建立一个新的连接时，新的队列会得到一个新的名字。由于在容器重启和队列被重新声明之间存在一个竞赛条件，所以必须将容器的 `missingQueuesFatal` 属性设置为 `false`，因为容器最初可能会尝试重新连接到旧队列。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 4.1.8. 消息转换器

`AmqpTemplate` 还定义了几个用于发送和接收消息的方法，这些方法委托给 `MessageConverter`。`MessageConverter` 为每个方向提供了一个方法：一个用于转换为 `Message`，另一个用于从 `Message` 转换。注意，当转换到一个 `Message` 时，除了 object 外，你还可以提供属性。`object` 参数通常与 `Message` body 相对应。下面的列表显示了 `MessageConverter` 的接口定义：

```java
public interface MessageConverter {

    Message toMessage(Object object, MessageProperties messageProperties)
            throws MessageConversionException;

    Object fromMessage(Message message) throws MessageConversionException;

}
```

`AmqpTemplate` 上的相关 `Message` 发送方法比我们之前讨论的方法更简单，因为它们不需要 `Message` 例。相反，`MessageConverter` 负责 "创建" 每个 `Message`，将提供的对象转换为 `Message` body 的字节数组，然后添加任何提供的 `MessageProperties`。下面的列表显示了各种方法的定义：

```java
void convertAndSend(Object message) throws AmqpException;

void convertAndSend(String routingKey, Object message) throws AmqpException;

void convertAndSend(String exchange, String routingKey, Object message)
    throws AmqpException;

void convertAndSend(Object message, MessagePostProcessor messagePostProcessor)
    throws AmqpException;

void convertAndSend(String routingKey, Object message,
    MessagePostProcessor messagePostProcessor) throws AmqpException;

void convertAndSend(String exchange, String routingKey, Object message,
    MessagePostProcessor messagePostProcessor) throws AmqpException;
```

在接收端，只有两个方法：一个接受队列名称，一个依赖于模板的 “queue” 属性已被设置。下面的列表显示了这两个方法的定义：

```java
Object receiveAndConvert() throws AmqpException;

Object receiveAndConvert(String queueName) throws AmqpException;
```

|      | [异步消费者](https://springdoc.cn/spring-amqp/#async-consumer) 中提到的 `MessageListenerAdapter` 也使用了一个 `MessageConverter`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### `SimpleMessageConverter`

`MessageConverter` 策略的默认实现被称为 `SimpleMessageConverter`。如果你没有明确配置替代方案，这就是 `RabbitTemplate` 实例使用的转换器。它处理基于文本的内容、序列化的 Java 对象和字节数组。

###### 从 `Message` 转换

如果输入 `Message` 的 content type 以 "text" 开头（例如，"text/plain"），它也会检查 content-encoding 属性，以确定将 `Message` body 字节数组转换为 Java `String` 时要使用的字符集。如果在输入的 `Message` 上没有设置 content-encoding 属性，它默认使用 UTF-8 字符集。如果你需要覆盖该默认设置，你可以配置 `SimpleMessageConverter` 的一个实例，设置其 `defaultCharset` 属性，并将其注入 `RabbitTemplate` 实例中。

如果输入 `Message` 的 content-type 属性值被设置为 "application/x-java-serialized-object"，`SimpleMessageConverter` 就会尝试将字节数组反序列化（rehydrate）成一个Java对象。虽然这对于简单的原型设计可能很有用，但我们不建议依赖Java 序列化，因为它导致生产者和消费者之间的紧密耦合。当然，这也排除了非Java系统在任何一方的使用。由于 AMQP 是一个 wire-level 协议，如果因为这样的限制而失去大部分的优势，那将是很不幸的。在接下来的两节中，我们将探讨一些替代方案，以便在不依赖Java序列化的情况下传递丰富的 domain 对象内容。

对于所有其他的 content-type，`SimpleMessageConverter` 直接以字节数的形式返回 `Message` body 内容。

重要信息见 [Java 反序列化](https://springdoc.cn/spring-amqp/#java-deserialization)。

###### 转化为 `Message`

当从一个任意的Java对象转换为一个 `Message` 时，`SimpleMessageConverter` 也同样处理字节数组、字符串和可序列化的实例。它把这些东西都转换为字节（在字节数组的情况下，没有什么可转换的），并相应地设置内容类型属性。如果要转换的 `Object` 不符合这些类型之一，则 `Message` body 为 null。

##### `SerializerMessageConverter`

这个 converter 与 `SimpleMessageConverter` 类似，只是它可以与其他Spring框架的 `Serializer` 和 `Deserializer` 实现一起配置，用于 `application/x-java-serialized-object` 转换。

重要信息见 [Java 反序列化](https://springdoc.cn/spring-amqp/#java-deserialization) 。

##### Jackson2JsonMessageConverter

本节涵盖了使用 `Jackson2JsonMessageConverter` 来转换 `Message`。它有以下几个部分：

- [转换为 `Message`](https://springdoc.cn/spring-amqp/#Jackson2JsonMessageConverter-to-message)
- [从 `Message` 转换](https://springdoc.cn/spring-amqp/#Jackson2JsonMessageConverter-from-message)

###### 转换为 `Message`

如上一节所述，一般不建议依赖Java序列化。一种相当常见的替代方法是 JSON（JavaScript Object Notation），它在不同的语言和平台上更加灵活和可移植。可以在任何 `RabbitTemplate` 实例上配置该转换器，以覆盖其对 `SimpleMessageConverter` 默认的使用。`Jackson2JsonMessageConverter` 使用 `com.fasterxml.jackson` 2.x 库。下面的示例配置了一个 `Jackson2JsonMessageConverter`：

```xml
<bean class="org.springframework.amqp.rabbit.core.RabbitTemplate">
    <property name="connectionFactory" ref="rabbitConnectionFactory"/>
    <property name="messageConverter">
        <bean class="org.springframework.amqp.support.converter.Jackson2JsonMessageConverter">
            <!-- if necessary, override the DefaultClassMapper -->
            <property name="classMapper" ref="customClassMapper"/>
        </bean>
    </property>
</bean>
```

如上所示，`Jackson2JsonMessageConverter` 默认使用一个 `DefaultClassMapper`。类型信息被添加到（并从）`MessageProperties` 中检索。如果一个入站的消息在 `MessageProperties` 中不包含类型信息，但你知道预期的类型，你可以通过使用 `defaultType` 属性配置一个静态类型，如下例所示：

```xml
<bean id="jsonConverterWithDefaultType"
      class="o.s.amqp.support.converter.Jackson2JsonMessageConverter">
    <property name="classMapper">
        <bean class="org.springframework.amqp.support.converter.DefaultClassMapper">
            <property name="defaultType" value="thing1.PurchaseOrder"/>
        </bean>
    </property>
</bean>
```

此外，你可以从 `*TypeId*` header 中的值提供自定义映射。下面的例子显示了如何做到这一点：

```java
@Bean
public Jackson2JsonMessageConverter jsonMessageConverter() {
    Jackson2JsonMessageConverter jsonConverter = new Jackson2JsonMessageConverter();
    jsonConverter.setClassMapper(classMapper());
    return jsonConverter;
}

@Bean
public DefaultClassMapper classMapper() {
    DefaultClassMapper classMapper = new DefaultClassMapper();
    Map<String, Class<?>> idClassMapping = new HashMap<>();
    idClassMapping.put("thing1", Thing1.class);
    idClassMapping.put("thing2", Thing2.class);
    classMapper.setIdClassMapping(idClassMapping);
    return classMapper;
}
```

现在，如果发送系统将 header 设置为 `thing1`，转换器就会创建一个 `Thing1` 对象，以此类推。请参阅 [从非Spring应用程序接收JSON](https://springdoc.cn/spring-amqp/#spring-rabbit-json) 示例应用程序，了解关于从非 Spring 应用程序转换消息的完整讨论。

从 2.4.3 版本开始，如果 `supportedMediaType` 有一个 `charset` 参数，转换器将不添加 `contentEncoding` 消息属性；这也用于编码。一个新的方法 `setSupportedMediaType` 已经被添加：

```java
String utf16 = "application/json; charset=utf-16";
converter.setSupportedContentType(MimeTypeUtils.parseMimeType(utf16));
```

###### 从 `Message` 转换

入站信息根据发送系统添加到 header 的类型信息被转换为对象。

从 2.4.3 版本开始，如果没有 `contentEncoding` 消息属性，转换器将尝试检测 `contentType` 消息属性中的 `charset` 参数并使用该参数。如果两者都不存在，如果s `upportedMediaType` 有一个 `charset` 参数，它将被用于解码，最后退到 `defaultCharset` 属性。一个新的方法 `setSupportedMediaType` 已经被添加：

```java
String utf16 = "application/json; charset=utf-16";
converter.setSupportedContentType(MimeTypeUtils.parseMimeType(utf16));
```

在1.6之前的版本中，如果类型信息不存在，转换会失败。从1.6版本开始，如果类型信息缺失，转换器通过使用Jackson的默认值（通常是一个Map）来转换JSON。

另外，从 1.6 版本开始，当你使用 `@RabbitListener` 注解（在方法上）时，推断的类型信息被添加到 `MessageProperties` 中。这让转换器转换为目标方法的参数类型。这只适用于有一个没有注解的参数或有 `@Payload` 注解的单个参数。`Message` 类型的参数在解析过程中会被忽略。

|      | 默认情况下，推断的类型信息将覆盖由发送系统创建的入站 `*TypeId*` 和相关 header 信息。这让接收系统自动转换为不同的 domain 对象。这只适用于参数类型是具体的（不是抽象的或接口的）或者是来自 `java.util` 包。在所有其他情况下，使用 `*TypeId*` 和相关 header。在有些情况下，你可能希望覆盖默认行为，并始终使用 `*TypeId*` 信息。例如，假设你有一个 `@RabbitListener`，它接受一个 `Thing1` 的参数，但消息中包含一个 `Thing2`，它是 `Thing1` 的一个子类（它是具体的）。推断出的类型将是不正确的。为了处理这种情况，将 `Jackson2JsonMessageConverter` 上的 `TypePrecedence` 属性设置为 `TYPE_ID` 而不是默认的 `INFERRED`。（这个属性实际上是在转换器的 `DefaultJackson2JavaTypeMapper` 上，但是为了方便，在转换器上提供了一个 setter）。如果你注入一个自定义的 type mapper，你应该在 mapper 上设置该属性。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 当从 `Message` 转换时，传入的 `MessageProperties.getContentType()` 必须是符合JSON标准的（`contentType.contains("json")` 用于检查）。从2.2版本开始，如果没有 `contentType` 属性，或者它的默认值为 `application/octet-stream`，则假定为 `application/json`。要恢复到以前的行为（返回一个未转换的 `byte[]`），将转换器的 `assumeSupportedContentType` 属性设置为 `false`。如果内容类型不被支持，就会发出一条 `WARN` 日志消息：`Could not convert incoming message with content-type […]`，并且 `message.getBody()` 会原样返回 — 作为一个 `byte[]`。因此，为了满足消费者方面的 `Jackson2JsonMessageConverter` 要求，生产者必须添加 `contentType` 消息属性—例如，作为 `application/json` 或 `text/x-json`，或者通过使用 `Jackson2JsonMessageConverter`，它自动设置 header。下面的列表显示了一些 converter 的调用： |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

```java
@RabbitListener
public void thing1(Thing1 thing1) {...}

@RabbitListener
public void thing1(@Payload Thing1 thing1, @Header("amqp_consumerQueue") String queue) {...}

@RabbitListener
public void thing1(Thing1 thing1, o.s.amqp.core.Message message) {...}

@RabbitListener
public void thing1(Thing1 thing1, o.s.messaging.Message<Foo> message) {...}

@RabbitListener
public void thing1(Thing1 thing1, String bar) {...}

@RabbitListener
public void thing1(Thing1 thing1, o.s.messaging.Message<?> message) {...}
```

在前面列出的前四种情况中，converter 试图转换为 `Thing1` 类型。第五个例子是无效的，因为我们不能确定哪个参数应该接受消息的 payload。对于第六个例子，由于泛型类型是通配符类型，Jackson 的默认值适用。

然而，你可以创建一个自定义 converter，并使用 `targetMethod` message property 来决定将JSON转换为哪种类型。

|      | 这种类型推断只有在 `@RabbitListener` 注解在方法级别声明时才能实现。通过类级的 `@RabbitListener`，转换后的类型被用来选择调用哪个 `@RabbitHandler` 方法。出于这个原因，基础设施提供了 `targetObject` message property，你可以在自定义 converter 中使用它来确定类型。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 从1.6.11版本开始，`Jackson2JsonMessageConverter` 以及 `DefaultJackson2JavaTypeMapper`（`DefaultClassMapper`）提供了 `trustedPackages` 选项，以克服 [Serialization Gadgets](https://pivotal.io/security/cve-2017-4995) 漏洞。默认情况下，为了向后兼容， `Jackson2JsonMessageConverter` 信任所有的包，也就是说，它使用 `*` 这个选项。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

从 2.4.7 版本开始，如果 Jackson 在反序列化消息体后返回 `null`，converter 可以被配置为返回 `Optional.empty()`。这有利于 `@RabbitListener` 接收 null payload，有两种方式：

```java
@RabbitListener(queues = "op.1")
void listen(@Payload(required = false) Thing payload) {
    handleOptional(payload); // payload might be null
}

@RabbitListener(queues = "op.2")
void listen(Optional<Thing> optional) {
    handleOptional(optional.orElse(this.emptyThing));
}
```

要启用该功能，请将 `setNullAsOptionalEmpty` 设置为 `true`；当 `false`（默认）时，converter 会返回到原始 message body （`byte[]`）。

```java
@Bean
Jackson2JsonMessageConverter converter() {
    Jackson2JsonMessageConverter converter = new Jackson2JsonMessageConverter();
    converter.setNullAsOptionalEmpty(true);
    return converter;
}
```

###### 反序列化抽象类

在 2.2.8 版之前，如果 `@RabbitListener` 的推断类型是一个抽象类（包括接口），converter 将退回到寻找 header 中的类型信息，如果存在，就使用该信息；如果不存在，它将尝试创建该抽象类。当使用配置有自定义反序列化器的自定义 `ObjectMapper` 来处理抽象类时，这就造成了一个问题，但传入的消息有无效的类型 header。

从 2.2.8 版本开始，以前的行为被默认保留。如果你有这样一个自定义的 `ObjectMapper`，并且你想忽略类型 header，总是使用推断类型进行转换，把 `alwaysConvertToInferredType` 设置为 `true`。这是向后兼容所需要的，并且避免在转换失败时尝试转换的开销（用一个标准的 `ObjectMapper`）。

###### 使用 Spring Data 投影接口

从 2.2 版本开始，你可以将JSON转换为 Spring Data 投影接口而不是具体类型。这允许对数据进行非常有选择性的、低耦合的绑定，包括从JSON文档的多个地方查找值。例如，以下接口可以被定义为 message payload 类型：

```java
interface SomeSample {

  @JsonPath({ "$.username", "$.user.name" })
  String getUsername();

}
@RabbitListener(queues = "projection")
public void projection(SomeSample in) {
    String username = in.getUsername();
    ...
}
```

默认情况下，accessor 方法将用于查找接收到的 JSON 文档中作为字段的属性名称。 `@JsonPath` 表达式允许自定义查找值，甚至可以定义多个JSON路径表达式，从多个地方查找值，直到一个表达式返回一个实际值。

要启用该功能，请在 message converter 上将 `useProjectionForInterfaces` 设置为 `true`。你还必须将 `spring-data:spring-data-commons` 和 `com.jayway.jsonpath:json-path` 添加到 class path 中。

当被用作 `@RabbitListener` 方法的参数时，接口类型会像平常一样自动传递给 converter。

###### 使用 `RabbitTemplate` 从 `Message` 转换

如前所述，类型信息是在消息 header 中传达的，以便在从消息转换时协助 converter。这在大多数情况下运作良好。然而，当使用泛型类型时，它只能转换简单对象和已知的 "容器" 对象（list、array 和 map）。从 2.0 版本开始，`Jackson2JsonMessageConverter` 实现了 `SmartMessageConverter`，这使得它可以与接受 `ParameterizedTypeReference` 参数的新 `RabbitTemplate` 方法一起使用。这允许对复杂的泛型类型进行转换，如下面的例子所示：

```java
Thing1<Thing2<Cat, Hat>> thing1 =
    rabbitTemplate.receiveAndConvert(new ParameterizedTypeReference<Thing1<Thing2<Cat, Hat>>>() { });
```

|      | 从2.1版本开始，`AbstractJsonMessageConverter` 类已被删除。它不再是 `Jackson2JsonMessageConverter` 的基类。它已被 `AbstractJackson2MessageConverter` 取代。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### `MarshallingMessageConverter`

还有一个选择是 `MarshallingMessageConverter`。它委托给 Spring OXM 库的 `Marshaller` 和 `Unmarshaller` 策略接口的实现。你可以在 [这里](https://docs.spring.io/spring/docs/current/spring-framework-reference/html/oxm.html) 阅读更多关于该库的信息。在配置方面，最常见的是只提供构造参数，因为大多数 `Marshaller` 的实现也实现了 `Unmarshaller`。下面的例子展示了如何配置一个 `MarshallingMessageConverter`：

```xml
<bean class="org.springframework.amqp.rabbit.core.RabbitTemplate">
    <property name="connectionFactory" ref="rabbitConnectionFactory"/>
    <property name="messageConverter">
        <bean class="org.springframework.amqp.support.converter.MarshallingMessageConverter">
            <constructor-arg ref="someImplemenationOfMarshallerAndUnmarshaller"/>
        </bean>
    </property>
</bean>
```

##### `Jackson2XmlMessageConverter`

这个类是在2.1版本中引入的，可以用来将信息从XML转换为XML。

`Jackson2XmlMessageConverter` 和 `Jackson2JsonMessageConverter` 都有同一个基类： `AbstractJackson2MessageConverter`。

|      | `AbstractJackson2MessageConverter` 类的引入是为了取代一个被移除的类： `AbstractJsonMessageConverter`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

`Jackson2XmlMessageConverter` 使用 `com.fasterxml.jackson` 2.x库。

你可以以与 `Jackson2JsonMessageConverter` 相同的方式使用它，只是它支持XML而不是JSON。下面的例子配置了一个 `Jackson2JsonMessageConverter`：

```xml
<bean id="xmlConverterWithDefaultType"
        class="org.springframework.amqp.support.converter.Jackson2XmlMessageConverter">
    <property name="classMapper">
        <bean class="org.springframework.amqp.support.converter.DefaultClassMapper">
            <property name="defaultType" value="foo.PurchaseOrder"/>
        </bean>
    </property>
</bean>
```

更多信息见 [Jackson2JsonMessageConverter](https://springdoc.cn/spring-amqp/#json-message-converter)。

|      | 从 2.2 版开始，如果没有 `contentType` 属性，或者它的默认值是 `application/octet-stream`，就会假定 `application/xml`。要恢复到以前的行为（返回一个未转换的 `byte[]`），将转换器的 `assumeSupportedContentType` 属性设置为 `false`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### `ContentTypeDelegatingMessageConverter`

该类在 1.4.2 版本中引入，允许根据 `MessageProperties` 中的内容类型属性委托给一个特定的 `MessageConverter`。默认情况下，如果没有 `contentType` 属性或者有一个与配置的转换器都不匹配的值，它就会委托给一个 `SimpleMessageConverter`。下面的例子配置了一个 `ContentTypeDelegatingMessageConverter`：

```xml
<bean id="contentTypeConverter" class="ContentTypeDelegatingMessageConverter">
    <property name="delegates">
        <map>
            <entry key="application/json" value-ref="jsonMessageConverter" />
            <entry key="application/xml" value-ref="xmlMessageConverter" />
        </map>
    </property>
</bean>
```

##### Java 反序列化

本节介绍如何反序列化Java对象。

|      | 当从不可信任的来源反序列化java对象时，可能存在一个漏洞。如果你接受来自不受信任来源的 `content-type` 为 `application/x-java-serialized-object` 的消息，你应该考虑配置哪些包和类被允许被反序列化。这适用于 `SimpleMessageConverter` 和 `SerializerMessageConverter`，当它被配置为隐含地或通过配置使用 `DefaultDeserializer`。默认情况下，允许的列表是空的，意味着所有的类都被反序列化了。你可以设置一个模式列表，如 `thing1.*`、`thing1.thing2.Cat` 或 `*.MySafeClass`。这些模式被依次检查，直到找到一个匹配的模式。如果没有匹配，就会抛出一个 `SecurityException`。你可以使用这些 converter 上的 `allowedListPatterns` 属性来设置这些模式。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### Message Properties 转换器

`MessagePropertiesConverter` 策略接口用于在 Rabbit Client `BasicProperties` 和 Spring AMQP `MessageProperties` 之间转换。默认实现（`DefaultMessagePropertiesConverter`）通常足以满足大多数目的，但如果需要，你可以实现你自己的。当大小不超过 `1024` 字节时，默认的属性转换器将 `LongString` 类型的 `BasicProperties` 元素转换为 `String` 实例。更大的 `LongString` 实例不会被转换（见下一段）。这个限制可以通过构造函数参数来重写。

从1.6版本开始，长度超过长字符串限制（默认：`1024`）的 header 现在被 `DefaultMessagePropertiesConverter` 默认为 `LongString` 实例。你可以通过 `getBytes[]`、`toString()` 或 `getStream()` 方法访问其内容。

以前，`DefaultMessagePropertiesConverter` 将这类 header "转换" 为 `DataInputStream`（实际上它只是引用了 `LongString` 实例的 `DataInputStream`）。在输出时，这个 header 没有被转换（除了转换为一个字符串—例如，通过在 stream 上调用 `toString()`，转换为 `java.io.DataInputStream@1d057a39`）。

传入的大的 `LongString` header 现在也能在输出时正确 "转换"（默认）。

提供了一个新的构造函数，让你配置 converter，使其像以前一样工作。下面的列表显示了该方法的 Javadoc 注释和声明：

```java
/**
 * Construct an instance where LongStrings will be returned
 * unconverted or as a java.io.DataInputStream when longer than this limit.
 * Use this constructor with 'true' to restore pre-1.6 behavior.
 * @param longStringLimit the limit.
 * @param convertLongLongStrings LongString when false,
 * DataInputStream when true.
 * @since 1.6
 */
public DefaultMessagePropertiesConverter(int longStringLimit, boolean convertLongLongStrings) { ... }
```

同样从 1.6 版开始，一个名为 `correlationIdString` 的新属性已被添加到 `MessageProperties` 中。以前，当与 RabbitMQ client 使用的 `BasicProperties` 进行转换时，会进行不必要的 `byte[] <→ String` 转换，因为 `MessageProperties.correlationId` 是一个 `byte[]` ，但 `BasicProperties` 使用一个 `String`。（最终，RabbitMQ client 使用 UTF-8 将 `String` 转换为字节以放入协议消息中）。

为了提供最大的向后兼容性，在 `DefaultMessagePropertiesConverter` 中添加了一个名为 `correlationIdPolicy` 的新属性。这需要一个 `DefaultMessagePropertiesConverter.CorrelationIdPolicy` 枚举参数。默认情况下，它被设置为 `BYTES`，它复制了以前的行为。

对于入站消息：

- `STRING`: 只有 `correlationIdString` 属性被映射。
- `BYTES`: 只有 `correlationId` 属性被映射。
- `BOTH`: 这两个属性都被映射。

对于出站消息：

- `STRING`: 只有 `correlationIdString` 属性被映射。
- `BYTES`: 只有 `correlationId` 属性被映射。
- `BOTH`: 这两个属性都会被考虑，其中 `String` 属性优先。

同样从 1.6 版本开始，入站的 `deliveryMode` 属性不再被映射到 `MessageProperties.deliveryMode`。它被映射到 `MessageProperties.receivedDeliveryMode`。另外，入站的 `userId` 属性不再被映射到 `MessageProperties.userId`。它被映射到 `MessageProperties.receivedUserId`。这些变化是为了避免在同一 `MessageProperties` 对象被用于出站消息时这些属性的意外传播。

从2.2版开始，`DefaultMessagePropertiesConverter` 使用 `getName()` 而不是 `toString()` 转换任何具有 `Class<?>` 类型值的自定义 header；这避免了消费应用程序必须从 `toString()` 表示中解析出类名。对于滚动升级，你可能需要改变你的消费者来理解这两种格式，直到所有生产者都升级。

#### 4.1.9. 修改消息 - 压缩和其他

存在一些扩展点。它们允许你在消息上进行一些处理，要么在它被发送到 `RabbitMQ` 之前，要么在它被接收之后立即进行。

在 [消息转换器](https://springdoc.cn/spring-amqp/#message-converters) 中可以看到，一个这样的扩展点是在 `AmqpTemplate` `convertAndReceive` 操作中，你可以提供一个 `MessagePostProcessor`。例如，在你的POJO被转换后，`MessagePostProcessor` 可以让你在 `Message` 上设置自定义 header 或属性。

从 1.4.2 版开始，额外的扩展点已被添加到 `RabbitTemplate` 中 - `setBeforePublishPostProcessors()` 和 `setAfterReceivePostProcessors()` 。第一个扩展点使后处理器能够在发送至 RabbitMQ 之前立即运行。当使用批处理（见 [批处理](https://springdoc.cn/spring-amqp/#template-batching)）时，这将在批处理组装后和批处理发送前被调用。第二个是在收到消息后立即调用的。

这些扩展点用于压缩等功能，为此，提供了几个 `MessagePostProcessor` 实现。`GZipPostProcessor`、`ZipPostProcessor` 和 `DeflaterPostProcessor` 在发送前压缩消息，而 `GUnzipPostProcessor`、`UnzipPostProcessor` 和 `InflaterPostProcessor` 对收到的消息进行解压。

|      | 从 2.1.5 版本开始，`GZipPostProcessor` 可以配置 `copyProperties = true` 选项，以制作原始消息属性的副本。默认情况下，由于性能原因，这些属性被重新使用，并通过压缩内容编码和可选的 `MessageProperties.SPRING_AUTO_DECOMPRESS` header 进行修改。如果你保留了对原始出站消息的引用，其属性也会改变。因此，如果你的应用程序使用这些消息后处理程序保留了一份出站消息的副本，请考虑打开 `copyProperties` 选项。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 从 2.2.12 版本开始，你可以配置压缩后处理器在内容编码元素之间使用的分隔符。在 2.2.11 及以前的版本中，这被硬编码为 `:`，现在它被默认设置为 `,`。decompressors 将使用这两种定界符。然而，如果你用 2.3 或更高版本发布消息，用 2.2.11 或更早的版本消费，你必须将 compressor 上的 `encodingDelimiter` 属性设置为 `:`。当你的消费者升级到 2.2.11 或更高版本时，你可以恢复到默认的 `,`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

同样，`SimpleMessageListenerContainer` 也有一个 `setAfterReceivePostProcessors()` 方法，让解压在容器收到消息后进行。

从 2.1.4 版开始，`addBeforePublishPostProcessors()` 和 `addAfterReceivePostProcessors()` 已被添加到 `RabbitTemplate` 中，以允许将新的后处理器分别追加到发布前和接收后处理器的列表中。此外，还提供了一些方法来删除后处理器。同样，`AbstractMessageListenerContainer` 也添加了 `addAfterReceivePostProcessors()` 和 `removeAfterReceivePostProcessor()` 方法。请参阅 `RabbitTemplate` 和 `AbstractMessageListenerContainer` 的 Javadoc 以了解更多细节。

#### 4.1.10. 请求/回复消息

`AmqpTemplate` 还提供了各种 `sendAndReceive` 方法，它们接受与前面描述的单向发送操作（`exchange`、`routingKey` 和 `Message`）相同的参数选项。这些方法对请求-回复的情况相当有用，因为它们在发送前处理必要的 `reply-to` 属性的配置，并可以在内部为此目的创建的专属队列上监听回复消息。

类似的请求-回复方法也可用，其中 `MessageConverter` 同时应用于请求和回复。这些方法被命名为 `convertSendAndReceive`。更多细节见 [`AmqpTemplate` 的Javadoc](https://docs.spring.io/spring-amqp/docs/latest-ga/api/org/springframework/amqp/core/AmqpTemplate.html)。

从1.5.0版本开始，`sendAndReceive` 方法的每一个变体都有一个重载版本，它接受 `CorrelationData`。与正确配置的连接工厂一起，这可以使操作的发送方收到发布者的确认。请参阅 [相关的 Publisher（发布者）确认和返回](https://springdoc.cn/spring-amqp/#template-confirms) 以及 [`RabbitOperations` 的 Javadoc](https://docs.spring.io/spring-amqp/docs/latest-ga/api/org/springframework/amqp/rabbit/core/RabbitOperations.html) 以了解更多信息。

从 2.0 版本开始，这些方法有一些变体（`convertSendAndReceiveAsType`），采取一个额外的 `ParameterizedTypeReference` 参数来转换复杂的返回类型。该 template 必须配置有 `SmartMessageConverter`。请参阅 [使用 `RabbitTemplate` 从 `Message` 转换](https://springdoc.cn/spring-amqp/#json-complex) 获取更多信息。

从 2.1 版开始，你可以使用 `noLocalReplyConsumer` 选项配置 `RabbitTemplate`，以控制回复消费者的 `noLocal` 标志。这在默认情况下是 `false`。

##### 回复超时

默认情况下，发送和接收方法在5秒后超时并返回 null。你可以通过设置 `replyTimeout` 属性来修改这一行为。从 1.5 版本开始，如果你把 `mandatory` 属性设置为 `true`（或者对于一个特定的消息，`mandatory-expression` 评估为 `true`），如果消息不能被传递到队列中，就会抛出一个 `AmqpMessageReturnedException`。这个异常有 `returnedMessage`、`replyCode` 和 `replyText` 属性，以及用于发送的 `exchange` 和 `routingKey`。

|      | 该功能使用发布者返回。你可以通过在 `CachingConnectionFactory` 上将 `publisherReturns` 设置为 `true` 来启用它（请参阅 [发布者消息确认和返回](https://springdoc.cn/spring-amqp/#cf-pub-conf-ret)）。此外，你必须没有向 `RabbitTemplate` 注册你自己的 `ReturnCallback`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

从 2.1.2 版本开始，增加了一个 `replyTimedOut` 方法，让子类被告知超时的情况，以便他们可以清理任何保留状态。

从 2.0.11 和 2.1.3 版本开始，当你使用默认的 `DirectReplyToMessageListenerContainer` 时，你可以通过设置 template 的 `replyErrorHandler` 属性添加一个 error handler 。这个 error handler 会在任何失败的交付中被调用，比如迟来的回复和收到的没有相关 header 的消息。传入的异常是 `ListenerExecutionFailedException`，它有一个 `failedMessage` 属性。

##### RabbitMQ Direct reply-to

|      | 从 3.4.0 版开始，RabbitMQ 服务器支持 [direct reply-to](https://www.rabbitmq.com/direct-reply-to.html)。这消除了固定回复队列的主要原因（以避免为每个请求创建一个临时队列）。从 Spring AMQP 1.4.1 版本开始，direct reply-to 被默认使用（如果服务器支持），而不是创建临时 reply 队列。当没有提供 `replyQueue`（或者它被设置为 `amq.rabbitmq.reply-to` 的名称）时， `RabbitTemplate` 会自动检测是否支持 direct reply-to 并使用它或退回到使用临时 reply 队列。当使用 direct reply-to 时，不需要并且不应该配置 `reply-listener`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

Reply listener 仍然支持命名的队列（除 `amq.rabbitmq.reply-to` 外），允许控制回复的并发性等。

从 1.6 版本开始，如果你希望为每个回复使用一个临时的、exclusive（独占）、auto-delete（自动删除）的队列，请将 `useTemporaryReplyQueues` 属性设置为 `true`。如果你设置了一个 `replyAddress`，这个属性就会被忽略。

你可以通过子类化 `RabbitTemplate` 并重载 `useDirectReplyTo()` 来改变决定是否使用 direct reply-to 的 criteria 以检查不同的 criteria。该方法只被调用一次，当第一次请求被发送时。

在 2.0 版本之前，`RabbitTemplate` 为每个请求创建一个新的消费者，并在收到回复（或超时）时取消该消费者。现在，模板使用 `DirectReplyToMessageListenerContainer` 代替，让消费者被重复使用。template 仍然负责关联回复，所以不存在晚回复到不同发布者的危险。如果你想恢复到以前的行为，把 `useDirectReplyToContainer`（使用XML配置 `direct-reply-to-container`）属性设置为 `false`。

`AsyncRabbitTemplate` 没有这样的选项。当使用 direct reply-to 时，它总是使用 `DirectReplyToContainer` 进行回复。

从 2.3.7 版本开始，该 template 有一个新的属性 `useChannelForCorrelation`。当这个属性为 `true` 时，服务器不必将请求消息 header 中的相关ID复制到回复消息中。相反，用于发送请求的 channel 被用来将回复与请求相关联。

##### 带有回复队列的消息相关性

当使用固定的回复队列（除 `amq.rabbitmq.reply-to` 外）时，你必须提供相关数据，以便回复可以与请求相关联。请参阅 [RabbitMQ 远程过程调用 (RPC)](https://www.rabbitmq.com/tutorials/tutorial-six-java.html)。默认情况下，标准的 `correlationId` 属性被用来保存相关数据。但是，如果你希望使用自定义属性来保存相关数据，你可以在 `<rabbit-template/>` 上设置 `correlation-key` 属性。明确地将该属性设置为 `correlationId` 与省略该属性是一样的。客户端和服务器必须为相关数据使用相同的 header。

|      | Spring AMQP 1.1版本为该数据使用了一个名为 `spring_reply_correlation` 的自定义属性。如果你希望在当前版本中恢复到这种行为（也许是为了保持与另一个使用1.1版本的应用程序的兼容性），你必须将该属性设置为 `spring_reply_correlation`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

默认情况下，template 会生成它自己的相关 ID（忽略任何用户提供的值）。如果你希望使用你自己的相关 ID，请将 `RabbitTemplate` 实例的 `userCorrelationId` 属性设置为 `true`。

|      | 相关的（correlation）ID必须是唯一的，以避免为一个请求返回错误的答复的可能性。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 回复 Listener 容器

当使用 3.4.0 之前的 RabbitMQ 版本时，每个回复都会使用一个新的临时队列。但是，可以在模板上配置一个单一的回复队列，这可能更有效率，并且还可以让你在该队列上设置参数。然而，在这种情况下，你还必须提供一个 `<reply-listener/>` 子元素。这个元素为回复队列提供了一个监听器容器，而 template 就是监听器。所有 `<listener-container/>` 上允许的 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes) 属性在该元素上都是允许的，除了 `connection-factory` 和 `message-converter`，它们是从 template 的配置中继承的。

|      | 如果你运行你的应用程序的多个实例或使用多个 `RabbitTemplate` 实例，你必须为每个实例使用一个独特的回复队列。RabbitMQ 没有从队列中选择消息的能力，因此，如果它们都使用同一个队列，每个实例都会争夺回复，而不一定能收到它们自己的回复。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

下面的例子定义了一个带有连接工厂的 rabbit template：

```xml
<rabbit:template id="amqpTemplate"
        connection-factory="connectionFactory"
        reply-queue="replies"
        reply-address="replyEx/routeReply">
    <rabbit:reply-listener/>
</rabbit:template>
```

虽然容器和模板共享一个连接工厂，但它们并不共享一个 Channel。因此，请求和回复不会在同一个事务中进行（如果是事务性的）。

|      | 在 1.5.0 版本之前，`reply-address` 属性是不可用的。回复总是通过使用默认的 exchange 和 `reply-queue` 作为 routing key 来进行路由。这仍然是默认的，但你现在可以指定新的 `reply-address` 属性。`reply-address` 可以包含一个形式为 `<exchange>/<routingKey>` 的地址，回复被路由到指定的 exchange，并被路由到与 routing key 绑定的队列。`reply-address` 比 `reply-queue` 有优先权。当只使用 `reply-address` 时，`<reply-listener>` 必须被配置为一个单独的 `<listener-container>` 组件。`reply-address` 和 `reply-queue`（或 `<listener-container>` 上的 `queues` 属性）必须在逻辑上指向同一个队列。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

通过这种配置，一个 `SimpleListenerContainer` 被用来接收回复，而 `RabbitTemplate` 是 `MessageListener`。当用 `<rabbit:template/>` 命名空间元素定义 template 时，如前面的例子所示，解析器将 template 中的容器和 wires 定义为 listener。

|      | 当 template 不使用固定 `replyQueue`（或使用 direct `reply-to` - 参见 [RabbitMQ Direct reply-to](https://springdoc.cn/spring-amqp/#direct-reply-to)）时，不需要监听器容器。当使用 RabbitMQ 3.4.0 或更高版本时，Direct `reply-to` 是首选机制。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

如果你将 `RabbitTemplate` 定义为 `<bean/>` 或使用 `@Configuration` 类将其定义为 `@Bean`，或者当你以编程方式创建 template 时，你需要自己定义和注入 reply listener container。如果你没有这样做，template 将永远不会收到回复，并最终超时并返回 `null` 作为对 `sendAndReceive` 方法的调用的回复。

从 1.5 版开始，`RabbitTemplate` 会检测它是否已被配置为接收回复的 `MessageListener`。如果没有，尝试发送和接收带有 reply address 的消息会以 `IllegalStateException` 失败（因为从未收到回复）。

此外，如果使用简单的 `replyAddress`（队列名称），reply listener container 会验证它是否在监听具有相同名称的队列。如果 reply address 是一个 exchange 和 routing key，就不能进行这种检查，而且要写一个 debug 日志消息。

|      | 当自己注入 reply listener 和 template 时，必须确保模板的 `replyAddress` 和容器的 `queues`（或 `queueNames`）属性指的是同一个队列。模板会将 reply address 插入到出站消息的 `replyTo` 属性中。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

下面列出了如何手动注入的例子：

```xml
<bean id="amqpTemplate" class="org.springframework.amqp.rabbit.core.RabbitTemplate">
    <constructor-arg ref="connectionFactory" />
    <property name="exchange" value="foo.exchange" />
    <property name="routingKey" value="foo" />
    <property name="replyQueue" ref="replyQ" />
    <property name="replyTimeout" value="600000" />
    <property name="useDirectReplyToContainer" value="false" />
</bean>

<bean class="org.springframework.amqp.rabbit.listener.SimpleMessageListenerContainer">
    <constructor-arg ref="connectionFactory" />
    <property name="queues" ref="replyQ" />
    <property name="messageListener" ref="amqpTemplate" />
</bean>

<rabbit:queue id="replyQ" name="my.reply.queue" />
    @Bean
    public RabbitTemplate amqpTemplate() {
        RabbitTemplate rabbitTemplate = new RabbitTemplate(connectionFactory());
        rabbitTemplate.setMessageConverter(msgConv());
        rabbitTemplate.setReplyAddress(replyQueue().getName());
        rabbitTemplate.setReplyTimeout(60000);
        rabbitTemplate.setUseDirectReplyToContainer(false);
        return rabbitTemplate;
    }

    @Bean
    public SimpleMessageListenerContainer replyListenerContainer() {
        SimpleMessageListenerContainer container = new SimpleMessageListenerContainer();
        container.setConnectionFactory(connectionFactory());
        container.setQueues(replyQueue());
        container.setMessageListener(amqpTemplate());
        return container;
    }

    @Bean
    public Queue replyQueue() {
        return new Queue("my.reply.queue");
    }
```

在 [这个测试案例](https://github.com/spring-projects/spring-amqp/tree/main/spring-rabbit/src/test/java/org/springframework/amqp/rabbit/listener/JavaConfigFixedReplyQueueTests.java) 中显示了一个完整的 `RabbitTemplate` 与一个固定的回复队列连接的例子，以及一个处理请求和返回回复的 "远程" 监听器容器。

|      | 当回复超时（`replyTimeout`）时，`sendAndReceive()` 方法返回 null。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

在 1.3.6 版本之前，超时消息的迟来的回复只被记录下来。现在，如果收到迟到的回复，就会被拒绝（template 会抛出一个 `AmqpRejectAndDontRequeueException`）。如果回复队列被配置为将被拒绝的消息发送到一个死信 exchange，那么回复可以被检索出来，以便以后分析。要做到这一点，将一个队列与配置的死信 exchange 绑定，其 routing key 等于回复队列的名称。

请参阅 [RabbitMQ Dead Letter 文档](https://www.rabbitmq.com/dlx.html) 以了解有关配置死信的更多信息。你还可以查看 `FixedReplyQueueDeadLetterTests` 测试案例，以获得一个示例。

##### Async Rabbit Template

1.6 版本引入了 `AsyncRabbitTemplate`。它有类似于 [`AmqpTemplate`](https://springdoc.cn/spring-amqp/#amqp-template) 上的 `sendAndReceive`（和 `convertSendAndReceive`）方法。然而，它们不是阻塞的，而是返回一个 `CompletableFuture`。

`sendAndReceive` 方法返回一个 `RabbitMessageFuture`。`convertSendAndReceive` 方法返回一个 `RabbitConverterFuture`。

你可以通过调用 future 上的 `get()`，稍后同步地检索结果，或者你可以注册一个 callback，用结果异步地调用。下面的列表显示了两种方法：

```java
@Autowired
private AsyncRabbitTemplate template;

...

public void doSomeWorkAndGetResultLater() {

    ...

    CompletableFuture<String> future = this.template.convertSendAndReceive("foo");

    // do some more work

    String reply = null;
    try {
        reply = future.get(10, TimeUnit.SECONDS);
    }
    catch (ExecutionException e) {
        ...
    }

    ...

}

public void doSomeWorkAndGetResultAsync() {

    ...

    RabbitConverterFuture<String> future = this.template.convertSendAndReceive("foo");
    future.whenComplete((result, ex) -> {
        if (ex == null) {
            // success
        }
        else {
            // failure
        }
    });

    ...

}
```

如果设置了 `mandatory`，并且消息不能被传递，future 会抛出一个 cause 为 `AmqpMessageReturnedException` 的 `ExecutionException`，它封装了返回的消息和有关返回的信息。

如果 `enableConfirms` 被设置，future 一个名为 `confirm` 的属性，它本身是一个 `CompletableFuture<Boolean>`，其中 `true` 表示成功发布。如果 confirm future 是 `false`，`RabbitFuture` 还有一个叫做 `nackCause` 的属性，它包含失败的原因（如果可用）。

|      | 如果发布者确认是在回复之后收到的，将被丢弃，因为回复意味着发布成功。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

你可以在 template 上设置 `receiveTimeout` 属性来超时回复（默认为 `30000` - 30 秒）。如果发生超时，future 将以 `AmqpReplyTimeoutException` 完成（completed ）。

该 template 实现了 `SmartLifecycle`。在有待处理的回复时停止 template 会导致待处理的 `Future` 实例被取消（canceled）。

从 2.0 版本开始，异步 template 现在支持 [direct reply-to](https://www.rabbitmq.com/direct-reply-to.html)，而不是配置的回复队列。要启用这一功能，请使用以下构造函数之一：

```java
public AsyncRabbitTemplate(ConnectionFactory connectionFactory, String exchange, String routingKey)

public AsyncRabbitTemplate(RabbitTemplate template)
```

请参阅 [RabbitMQ Direct reply-to](https://springdoc.cn/spring-amqp/#direct-reply-to) 以使用同步 `RabbitTemplate` 的直接回复。

2.0 版本引入了这些方法的变体（`convertSendAndReceiveAsType`），它接受一个额外的 `ParameterizedTypeReference` 参数以转换复杂的返回类型。你必须用 `SmartMessageConverter` 配置底层 `RabbitTemplate`。请参阅 [使用 `RabbitTemplate` 从 `Message` 转换](https://springdoc.cn/spring-amqp/#json-complex) 获取更多信息。

|      | 从 3.0 版本开始，`AsyncRabbitTemplate` 方法现在返回 `CompletableFuture` 而不是 `ListenableFuture`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### AMQP 和 Spring Remoting

由于 Spring 框架已经删除了该功能，所以不再支持 Spring remoting 。

使用 `RabbitTemplate`（客户端）和 `@RabbitListener` 代替 `sendAndReceive` 操作。

#### 4.1.11. 配置 Broker

AMQP 规范描述了如何使用该协议来配置 broker 上的队列、exchange 和 binding。这些操作（可从 0.8 规范和更高版本移植）存在于 `org.springframework.amqp.core` 包中的 `AmqpAdmin` 接口。该类的 `RabbitMQ` 实现是位于 `org.springframework.amqp.rabbit.core` 包中的 `RabbitAdmin`。

`AmqpAdmin` 接口是基于使用 Spring AMQP domain 抽象，在下面的列表中显示：

```java
public interface AmqpAdmin {

    // Exchange Operations

    void declareExchange(Exchange exchange);

    void deleteExchange(String exchangeName);

    // Queue Operations

    Queue declareQueue();

    String declareQueue(Queue queue);

    void deleteQueue(String queueName);

    void deleteQueue(String queueName, boolean unused, boolean empty);

    void purgeQueue(String queueName, boolean noWait);

    // Binding Operations

    void declareBinding(Binding binding);

    void removeBinding(Binding binding);

    Properties getQueueProperties(String queueName);

}
```

另见 [Scope 操作](https://springdoc.cn/spring-amqp/#scoped-operations)。

`getQueueProperties()` 方法返回关于队列的一些有限信息（消息计数和消费者计数）。返回的属性的 key 可作为 `RabbitTemplate` 中的常量（`QUEUE_NAME`、`QUEUE_MESSAGE_COUNT` 和 `QUEUE_CONSUMER_COUNT`）。[RabbitMQ REST API](https://springdoc.cn/spring-amqp/#management-rest-api) 在 `QueueInfo` 对象中提供了更多信息。

无参 `declareQueue()` 方法在 broker 上定义了一个队列，名字是自动生成的。这个自动生成的队列的附加属性是 `exclusive=true`，`autoDelete=true`，以及 `durable=false`。

`declareQueue(Queue queue)` 方法接收一个 `Queue` 对象并返回所声明的队列的名称。如果提供的 `Queue` 的 `name` 属性是一个空的 `String`，broker 就会用一个生成的名字声明队列。该名称会返回给调用者。该名称也被添加到 `Queue` 的 `actualName` 属性中。你只能通过直接调用 `RabbitAdmin` 来以编程方式使用此功能。当在 application context 中声明性地定义队列时使用 admin 的自动声明时，你可以将 name 属性设置为 `""` （空字符串）。然后 broker 会创建这个名字。从 2.1 版开始，监听器容器可以使用这种类型的队列。更多信息请参见 [容器和 Broker 命名的队列](https://springdoc.cn/spring-amqp/#containers-and-broker-named-queues)。

这与 `AnonymousQueue` （匿名队列）相反，在匿名队列中，框架会生成一个唯一的（`UUID`）名称，并将 `durable` 设置为 `false` ，`exclusive`，`autoDelete` 为 `true`。一个 `name` 属性为空（或缺失）的 `<rabbit:queue/>` 总是创建一个 `AnonymousQueue`。

参见 [`AnonymousQueue`](https://springdoc.cn/spring-amqp/#anonymous-queue)，了解为什么 `AnonymousQueue` 比 broker 生成的队列名称更受欢迎，以及如何控制名称的格式。从2.1版开始，匿名队列在声明时，参数 `Queue.X_QUEUE_LEADER_LOCATOR` 默认设置为 `client-local`。这可以确保队列是在应用程序所连接的节点上声明的。声明式队列必须有固定的名称，因为它们可能在上下文的其他地方被引用 — 比如下面例子中所示的监听器：

```xml
<rabbit:listener-container>
    <rabbit:listener ref="listener" queue-names="#{someQueue.name}" />
</rabbit:listener-container>
```

见 [Exchange、 Queue 和 Binding 的自动声明](https://springdoc.cn/spring-amqp/#automatic-declaration)。

该接口的 `RabbitMQ` 实现是 `RabbitAdmin`，通过使用 Spring XML 进行配置时，它类似于下面的示例：

```xml
<rabbit:connection-factory id="connectionFactory"/>

<rabbit:admin id="amqpAdmin" connection-factory="connectionFactory"/>
```

当 `CachingConnectionFactory` 缓存模式为 `CHANNEL`（默认）时，`RabbitAdmin` 实现对在同一 `ApplicationContext` 中声明的队列、exchange 和 binding 进行自动懒声明。一旦向 broker 打开一个 `Connection` ，这些组件就会被声明。有一些命名空间特性使这非常方便 - 例如，在 Stocks 示例应用程序中，我们有以下内容：

```xml
<rabbit:queue id="tradeQueue"/>

<rabbit:queue id="marketDataQueue"/>

<fanout-exchange name="broadcast.responses"
                 xmlns="http://www.springframework.org/schema/rabbit">
    <bindings>
        <binding queue="tradeQueue"/>
    </bindings>
</fanout-exchange>

<topic-exchange name="app.stock.marketdata"
                xmlns="http://www.springframework.org/schema/rabbit">
    <bindings>
        <binding queue="marketDataQueue" pattern="${stocks.quote.pattern}"/>
    </bindings>
</topic-exchange>
```

在前面的例子中，我们使用了匿名队列（实际上，在内部，只是队列的名字由框架生成，而不是由 broker 生成），并通过 ID 来指代它们。我们也可以用显式名称声明队列，这也是 context 中其 bean 定义的标识符。下面的例子配置了一个有明确名称的队列：

```xml
<rabbit:queue name="stocks.trade.queue"/>
```

|      | 你可以同时提供 `id` 和 `name` 属性。这让你可以通过独立于队列名称的 ID 来引用队列（例如，在 binding 中）。它还允许标准的 Spring 特性（如属性占位符和队列名称的 SpEL 表达式）。当你使用名称作为 Bean 标识符时，这些功能是不可用的。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

队列可以配置额外的参数—例如，`x-message-ttl`。当你使用命名空间支持时，它们是以 argument-name/argument-value 对的 `Map` 形式提供的，这些参数是通过使用 `<rabbit:queue-arguments>` 元素定义的。下面的例子展示了如何做到这一点：

```xml
<rabbit:queue name="withArguments">
    <rabbit:queue-arguments>
        <entry key="x-dead-letter-exchange" value="myDLX"/>
        <entry key="x-dead-letter-routing-key" value="dlqRK"/>
    </rabbit:queue-arguments>
</rabbit:queue>
```

默认情况下，参数被认为是字符串。对于其他类型的参数，你必须提供其类型。下面的例子显示了如何指定类型：

```xml
<rabbit:queue name="withArguments">
    <rabbit:queue-arguments value-type="java.lang.Long">
        <entry key="x-message-ttl" value="100"/>
    </rabbit:queue-arguments>
</rabbit:queue>
```

当提供混合类型的参数时，你必须为每个条目元素提供类型。下面的例子显示了如何做到这一点：

```xml
<rabbit:queue name="withArguments">
    <rabbit:queue-arguments>
        <entry key="x-message-ttl">
            <value type="java.lang.Long">100</value>
        </entry>
        <entry key="x-dead-letter-exchange" value="myDLX"/>
        <entry key="x-dead-letter-routing-key" value="dlqRK"/>
    </rabbit:queue-arguments>
</rabbit:queue>
```

在 Spring Framework 3.2 及以后的版本中，可以更简洁地声明这一点，如下：

```xml
<rabbit:queue name="withArguments">
    <rabbit:queue-arguments>
        <entry key="x-message-ttl" value="100" value-type="java.lang.Long"/>
        <entry key="x-ha-policy" value="all"/>
    </rabbit:queue-arguments>
</rabbit:queue>
```

当你使用 Java 配置时，`Queue.X_QUEUE_LEADER_LOCATOR` 参数通过 `Queue` 类的 `setLeaderLocator()` 方法作为第一类属性被支持。从2.1版开始，匿名队列在声明时默认将该属性设置为 `client-local`。这确保了队列是在应用程序所连接的节点上声明的。

|      | RabbitMQ broker 不允许声明具有不匹配参数的队列。例如，如果一个 `queue` 已经存在且没有 `time to live` 参数，而你试图用（例如）`key="x-message-ttl" value="100"` 来声明它，则会抛出一个异常。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

默认情况下，当发生任何异常时，`RabbitAdmin` 立即停止处理所有声明。这可能会导致下游问题，例如监听器容器无法初始化，因为另一个队列（定义在出错的队列之后）未被声明。

可以通过在 `RabbitAdmin` 实例上将 `ignore-declaration-exceptions` 属性设置为 `true` 来修改此行为。此选项指示 `RabbitAdmin` 记录该异常并继续声明其他元素。在使用 Java 配置 `RabbitAdmin` 时，该属性被称为 `ignoreDeclarationExceptions`。这是一个全局设置，适用于所有元素。队列、exchange 和 binding 有一个类似的属性，仅适用于这些元素。

在 1.6 版本之前，这个属性只有在 channel 上发生 `IOException` 时才生效，比如当前属性和期望属性不匹配时。现在，这个属性对任何异常都有效，包括 `TimeoutException` 和其他。

此外，任何声明异常都会导致 `DeclarationExceptionEvent` 事件的发布，它是一个 `ApplicationEvent`，可以被上下文中的任何 `ApplicationListener` 所消费。该事件包含对 admin、被声明的元素和 `Throwable` 的引用。

##### Headers Exchange

从1.3版本开始，你可以配置 `HeadersExchange` 来匹配多个 header 信息。你还可以指定是否必须匹配任何或所有 header。下面的例子显示了如何做到这一点：

```xml
<rabbit:headers-exchange name="headers-test">
    <rabbit:bindings>
        <rabbit:binding queue="bucket">
            <rabbit:binding-arguments>
                <entry key="foo" value="bar"/>
                <entry key="baz" value="qux"/>
                <entry key="x-match" value="all"/>
            </rabbit:binding-arguments>
        </rabbit:binding>
    </rabbit:bindings>
</rabbit:headers-exchange>
```

从 1.6 版开始，你可以用 `internal` 标志（默认为 `false`）配置 `Exchanges`，这样的 `Exchange` 通过 `RabbitAdmin`（如果 application context 中存在一个）在 Broker 上正确配置。如果 exchange 的 `internal` 标志为 `true`，则 RabbitMQ 不会让客户端使用该 exchange。这对于死信 exchange 或 exchange 到 exchange 的绑定非常有用，因为你不希望 exchange 被发布者直接使用。

要了解如何使用 Java 来配置 AMQP 基础设施，请查看 Stock 示例应用程序，其中有 `@Configuration` 类 `AbstractStockRabbitConfiguration`，它又有 `RabbitClientConfiguration` 和 `RabbitServerConfiguration` 子类。下面的列表显示了 `AbstractStockRabbitConfiguration` 的代码：

```java
@Configuration
public abstract class AbstractStockAppRabbitConfiguration {

    @Bean
    public CachingConnectionFactory connectionFactory() {
        CachingConnectionFactory connectionFactory =
            new CachingConnectionFactory("localhost");
        connectionFactory.setUsername("guest");
        connectionFactory.setPassword("guest");
        return connectionFactory;
    }

    @Bean
    public RabbitTemplate rabbitTemplate() {
        RabbitTemplate template = new RabbitTemplate(connectionFactory());
        template.setMessageConverter(jsonMessageConverter());
        configureRabbitTemplate(template);
        return template;
    }

    @Bean
    public Jackson2JsonMessageConverter jsonMessageConverter() {
        return new Jackson2JsonMessageConverter();
    }

    @Bean
    public TopicExchange marketDataExchange() {
        return new TopicExchange("app.stock.marketdata");
    }

    // additional code omitted for brevity

}
```

在Stock应用程序中，服务器是通过使用以下 `@Configuration` 类来配置的：

```java
@Configuration
public class RabbitServerConfiguration extends AbstractStockAppRabbitConfiguration  {

    @Bean
    public Queue stockRequestQueue() {
        return new Queue("app.stock.request");
    }
}
```

这就是 `@Configuration` 类的整个继承链的结束。最终的结果是，`TopicExchange` 和 `Queue` 在应用程序启动时被声明给 broker。在服务器配置中没有将 `TopicExchange` 绑定到队列，因为这是在客户端应用程序中完成的。然而，stock 请求队列被自动绑定到AMQP的默认 exchange。这个行为是由规范定义的。

客户端的 `@Configuration` 类更有趣一些。它的声明如下：

```java
@Configuration
public class RabbitClientConfiguration extends AbstractStockAppRabbitConfiguration {

    @Value("${stocks.quote.pattern}")
    private String marketDataRoutingKey;

    @Bean
    public Queue marketDataQueue() {
        return amqpAdmin().declareQueue();
    }

    /**
     * Binds to the market data exchange.
     * Interested in any stock quotes
     * that match its routing key.
     */
    @Bean
    public Binding marketDataBinding() {
        return BindingBuilder.bind(
                marketDataQueue()).to(marketDataExchange()).with(marketDataRoutingKey);
    }

    // additional code omitted for brevity

}
```

客户端通过 `AmqpAdmin` 的 `declareQueue()` 方法声明另一个队列。它将该队列与市场数据 exchange 的 routing pattern 绑定在一起，该 routing pattern 被外化在一个属性文件中。

##### Queue 和 Exchange 的 Builder API

1.6版本引入了一个方便的 fluent API，用于在使用Java配置时配置 `Queue` 和 `Exchange` 对象。下面的例子展示了如何使用它：

```java
@Bean
public Queue queue() {
    return QueueBuilder.nonDurable("foo")
        .autoDelete()
        .exclusive()
        .withArgument("foo", "bar")
        .build();
}

@Bean
public Exchange exchange() {
  return ExchangeBuilder.directExchange("foo")
      .autoDelete()
      .internal()
      .withArgument("foo", "bar")
      .build();
}
```

更多信息见 [`org.springframework.amqp.core.QueueBuilder`](https://docs.spring.io/spring-amqp/docs/latest-ga/api/org/springframework/amqp/core/QueueBuilder.html) 和 [`org.springframework.amqp.core.ExchangeBuilder`](https://docs.spring.io/spring-amqp/docs/latest-ga/api/org/springframework/amqp/core/ExchangeBuilder.html) 的 Javadoc。

从2.0版本开始，`ExchangeBuilder` 现在默认创建持久的 exchange，以便与各个 `AbstractExchange` 类上的简单构造函数保持一致。要用 builder 创建一个非持久的 exchange，请在调用 `.build()` 之前使用 `.durable(false)`。不再提供没有参数的 `durable()` 方法。

2.2 版本引入了 fluent API，以增加 "众所周知的" exchange 和 queue 参数…

```java
@Bean
public Queue allArgs1() {
    return QueueBuilder.nonDurable("all.args.1")
            .ttl(1000)
            .expires(200_000)
            .maxLength(42)
            .maxLengthBytes(10_000)
            .overflow(Overflow.rejectPublish)
            .deadLetterExchange("dlx")
            .deadLetterRoutingKey("dlrk")
            .maxPriority(4)
            .lazy()
            .leaderLocator(LeaderLocator.minLeaders)
            .singleActiveConsumer()
            .build();
}

@Bean
public DirectExchange ex() {
    return ExchangeBuilder.directExchange("ex.with.alternate")
            .durable(true)
            .alternate("alternate")
            .build();
}
```

##### 声明 Exchange、Queue 和 Binding 的集合

你可以在 `Declarables` 对象中包装 `Declarable` 对象（`Queue`、`Exchange` 和 `Binding`）的集合。`RabbitAdmin` 在 application context 中检测此类 Bean（以及离散的 `Declarable` Bean），并在建立连接时（最初和连接失败后）在 broker 上声明包含的对象。下面的示例显示了如何做到这一点：

```java
@Configuration
public static class Config {

    @Bean
    public CachingConnectionFactory cf() {
        return new CachingConnectionFactory("localhost");
    }

    @Bean
    public RabbitAdmin admin(ConnectionFactory cf) {
        return new RabbitAdmin(cf);
    }

    @Bean
    public DirectExchange e1() {
        return new DirectExchange("e1", false, true);
    }

    @Bean
    public Queue q1() {
        return new Queue("q1", false, false, true);
    }

    @Bean
    public Binding b1() {
        return BindingBuilder.bind(q1()).to(e1()).with("k1");
    }

    @Bean
    public Declarables es() {
        return new Declarables(
                new DirectExchange("e2", false, true),
                new DirectExchange("e3", false, true));
    }

    @Bean
    public Declarables qs() {
        return new Declarables(
                new Queue("q2", false, false, true),
                new Queue("q3", false, false, true));
    }

    @Bean
    @Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
    public Declarables prototypes() {
        return new Declarables(new Queue(this.prototypeQueueName, false, false, true));
    }

    @Bean
    public Declarables bs() {
        return new Declarables(
                new Binding("q2", DestinationType.QUEUE, "e2", "k2", null),
                new Binding("q3", DestinationType.QUEUE, "e3", "k3", null));
    }

    @Bean
    public Declarables ds() {
        return new Declarables(
                new DirectExchange("e4", false, true),
                new Queue("q4", false, false, true),
                new Binding("q4", DestinationType.QUEUE, "e4", "k4", null));
    }

}
```

|      | 在 2.1 之前的版本中，你可以通过定义 `Collection<Declarable>` 类型的 bean 来声明多个 `Declarable` 实例。这在某些情况下会造成不理想的副作用，因为 admin 必须遍历所有 `Collection<?>` bean。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

2.2 版本为 `Declarables` 添加了 `getDeclarablesByType` 方法；这可以作为一种便利，例如，在声明监听器容器 Bean 时使用。

```java
public SimpleMessageListenerContainer container(ConnectionFactory connectionFactory,
        Declarables mixedDeclarables, MessageListener listener) {

    SimpleMessageListenerContainer container = new SimpleMessageListenerContainer(connectionFactory);
    container.setQueues(mixedDeclarables.getDeclarablesByType(Queue.class).toArray(new Queue[0]));
    container.setMessageListener(listener);
    return container;
}
```

##### 条件式声明

默认情况下，所有 queue、exchange 和 binding 都由 application context 中的所有 `RabbitAdmin` 实例（假设它们具有 `auto-startup="true"`）声明。

从 2.1.9 版开始，`RabbitAdmin` 有一个新的属性 `explicitDeclarationsOnly`（默认为 `false`）；当它被设置为 `true` 时，admin 将只声明被明确配置为由该 admin 声明的 Bean。

|      | 从 1.2 版本开始，你可以有条件地声明这些元素。当一个应用程序连接到多个 broker，需要指定向哪个 broker 声明一个特定的元素时，这一点特别有用。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

代表这些元素的类实现了 `Declarable`，它有两个方法：`shouldDeclare()` 和 `getDeclaringAdmins()`。`RabbitAdmin` 使用这些方法来确定一个特定的实例是否应该实际处理其 `Connection` 上的声明。

这些属性在命名空间中作为属性可用，如下面的例子所示：

```xml
<rabbit:admin id="admin1" connection-factory="CF1" />

<rabbit:admin id="admin2" connection-factory="CF2" />

<rabbit:admin id="admin3" connection-factory="CF3" explicit-declarations-only="true" />

<rabbit:queue id="declaredByAdmin1AndAdmin2Implicitly" />

<rabbit:queue id="declaredByAdmin1AndAdmin2" declared-by="admin1, admin2" />

<rabbit:queue id="declaredByAdmin1Only" declared-by="admin1" />

<rabbit:queue id="notDeclaredByAllExceptAdmin3" auto-declare="false" />

<rabbit:direct-exchange name="direct" declared-by="admin1, admin2">
    <rabbit:bindings>
        <rabbit:binding key="foo" queue="bar"/>
    </rabbit:bindings>
</rabbit:direct-exchange>
```

|      | 默认情况下，`auto-declare` 属性为 `true`，如果未提供 `declared-by`（或为空），那么所有 `RabbitAdmin` 实例都会声明该对象（只要 admin 的 `auto-startup` 属性为 `true`（默认），并且 admin 的 `explicit-declarations-only` 属性为 `false`）。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

同样地，你可以使用基于 Java 的 `@Configuration` 来达到同样的效果。在下面的例子中，组件是由 `admin1` 声明的，但不是由 `admin2` 声明的：

```java
@Bean
public RabbitAdmin admin1() {
    return new RabbitAdmin(cf1());
}

@Bean
public RabbitAdmin admin2() {
    return new RabbitAdmin(cf2());
}

@Bean
public Queue queue() {
    Queue queue = new Queue("foo");
    queue.setAdminsThatShouldDeclare(admin1());
    return queue;
}

@Bean
public Exchange exchange() {
    DirectExchange exchange = new DirectExchange("bar");
    exchange.setAdminsThatShouldDeclare(admin1());
    return exchange;
}

@Bean
public Binding binding() {
    Binding binding = new Binding("foo", DestinationType.QUEUE, exchange().getName(), "foo", null);
    binding.setAdminsThatShouldDeclare(admin1());
    return binding;
}
```

##### 关于 `id` 和 `name` 属性的说明

`<rabbit:queue/>` 和 `<rabbit:exchange/>` 元素上的 `name` 属性反映了 broker 中的实体名称。对于队列，如果省略了 `name`，就会创建一个匿名队列（见 [`AnonymousQueue`](https://springdoc.cn/spring-amqp/#anonymous-queue)）。

在 2.0 之前的版本中，该 `name` 也被注册为 `bean` 名称的别名（类似于 `<bean/>` 元素上的 `name` ）。

这造成了两个问题：

- 它阻止了同名的 queue 和 exchange 的声明。
- 如果别名包含SpEL表达式（`#{…}`），则无法解析。

从 2.0 版开始，如果你用 `id` 和 `name` 属性来声明这些元素之一，那么 `name` 就不再被声明为 bean 名称别名。如果你想用相同的 `name` 声明一个 queue 和 exchange，你必须提供一个 `id`。

如果该元素只有一个 `name` 属性，则没有变化。这个 Bean 仍然可以通过 `name` 被引用—例如，在 binding 声明中。然而，如果 name 中包含SpEL，你仍然不能引用它—你必须提供一个 `id` 用于引用目的。

##### `AnonymousQueue`

一般来说，当你需要一个唯一命名的、独占的（exclusive）、自动删除（auto-delete）的队列时，我们建议你使用 `AnonymousQueue` 而不是 broker 定义的队列名称（使用 `""` 作为 `Queue` 名称会导致 broker 生成队列名称）。

这是因为：

1. 队列实际上是在建立与 broker 的连接时被声明的。这是在 Bean 被创建并装配在一起之后很长时间。使用队列的 Bean 需要知道它的名字。事实上，当应用程序被启动时，broker 可能甚至没有运行。
2. 如果与 broker 的连接由于某种原因而丢失，admin 会以相同的 name 重新声明 `AnonymousQueue`。如果我们使用 broker 声明的队列，队列的 name 就会改变。

你可以控制 `AnonymousQueue` 实例所使用的 queue name 的格式。

默认情况下，队列名称的前缀是 `spring.gen-`，后缀是 `UUID` 的 base64 表示 - 例如：`spring.gen-MRBv9sqISkuCiPfOYfpo4g`。

你可以在构造函数参数中提供一个 `AnonymousQueue.NamingStrategy` 的实现。下面的例子显示了如何做到这一点：

```java
@Bean
public Queue anon1() {
    return new AnonymousQueue();
}

@Bean
public Queue anon2() {
    return new AnonymousQueue(new AnonymousQueue.Base64UrlNamingStrategy("something-"));
}

@Bean
public Queue anon3() {
    return new AnonymousQueue(AnonymousQueue.UUIDNamingStrategy.DEFAULT);
}
```

第一个Bean生成一个以 `spring.gen` 为前缀的队列名称，后面是 `UUID` 的base64表示，例如：`spring.gen-MRBv9sqISkuCiPfOYfpo4g`。第二个Bean生成一个以 `something` 为前缀的队列名称，后面是 `UUID` 的base64表示。第三个Bean只使用 `UUID`（没有base64转换）来生成一个名字—例如：`f20c818a-006b-4416-bf91-643590fedb0e`。

base64 编码使用 RFC 4648 的 "URL和文件名安全字母表"。后面的填充字符（`=`）被删除。

你可以提供你自己的命名策略，据此你可以在队列名称中包括其他信息（如应用程序名称或客户端主机）。

你可以在使用 XML 配置时指定命名策略。对于实现 `AnonymousQueue.NamingStrategy` 的 Bean 引用，`naming-strategy` 属性存在于 `<rabbit:queue>` 元素中。下面的例子显示了如何以各种方式指定命名策略：

```xml
<rabbit:queue id="uuidAnon" />

<rabbit:queue id="springAnon" naming-strategy="uuidNamer" />

<rabbit:queue id="customAnon" naming-strategy="customNamer" />

<bean id="uuidNamer" class="org.springframework.amqp.core.AnonymousQueue.UUIDNamingStrategy" />

<bean id="customNamer" class="org.springframework.amqp.core.AnonymousQueue.Base64UrlNamingStrategy">
    <constructor-arg value="custom.gen-" />
</bean>
```

第一个例子创建的 name 是 `spring.gen-MRBv9sqISkuCiPfOYfpo4g`。第二个例子用UUID的字符串表示来创建 name。第三个例子创建的 name 是 `custom.gen-MRBv9sqISkuCiPfOYfpo4g`。

你也可以提供你自己的 naming strategy bean。

从 2.1 版开始，匿名队列在声明时，参数 `Queue.X_QUEUE_LEADER_LOCATOR` 默认设置为 `client-local`。这确保了 queue 是在应用程序所连接的节点上声明的。你可以在构造实例后调用 `queue.setLeaderLocator(null)` 来恢复到以前的行为。

##### 恢复 Auto-Delete 的声明

通常，`RabbitAdmin` 只恢复在应用程序上下文中被声明为 Bean 的 queue/exchange/binding；如果任何此类声明是自动删除（auto-delete）的，那么如果连接丢失，它们将被 broker 删除。当连接被重新建立时，admin 将重新声明这些实体。通常，通过调用 `admin.declareQueue(…)`、`admin.declareExchange(…)` 和 `admin.declareBinding(…)` 创建的实体将不会被恢复。

从 2.4 版本开始，admin 有一个新的属性 `redeclareManualDeclarations`；当为 `true` 时，admin 将在 application context 中的 Bean之 外恢复这些实体。

如果调用 `deleteQueue(…)`、`deleteExchange(…)` 或 `removeBinding(…)`，将不执行单个声明的恢复。当 queue 和 exchange 被删除时，相关的 binding 将从可恢复的实体中移除。

最后，调用 `resetAllManualDeclarations()` 将阻止恢复任何先前声明的实体。

#### 4.1.12. Broker 事件监听器

当 [Event Exchange Plugin](https://www.rabbitmq.com/event-exchange.html) 插件被启用时，如果你将 `BrokerEventListener` 类型的 bean 添加到应用程序上下文中，它就会将选定的 broker 事件发布为 `BrokerEvent` 实例，这些实例可以被正常的Spring `ApplicationListener` 或 `@EventListener` 方法所消费。事件被 broker 发布到 topic exchange `amq.rabbitmq.event`，每个事件类型都有不同的 routing key。监听器使用 event key，这些 key 被用来将 `AnonymousQueue` 绑定到 exchange，因此监听器只接收选定的事件。因为它是一个 topic exchange，所以可以使用通配符（以及明确请求特定的事件），正如下面的例子所示：

```java
@Bean
public BrokerEventListener eventListener() {
    return new BrokerEventListener(connectionFactory(), "user.deleted", "channel.#", "queue.#");
}
```

你可以通过使用正常的Spring技术，在单个事件监听器中进一步缩小接收到的事件，如下例所示：

```java
@EventListener(condition = "event.eventType == 'queue.created'")
public void listener(BrokerEvent event) {
    ...
}
```

#### 4.1.13. 延迟消息 Exchange

1.6 版引入了对 [延迟消息 Exchange 插件](https://www.rabbitmq.com/blog/2015/04/16/scheduling-messages-with-rabbitmq/) 的支持

|      | 该插件目前被标记为实验性的，但已经有一年多的时间了（在撰写本文时）。如果该插件的变化使其成为必要，我们计划在可行的情况下尽快添加对这种变化的支持。由于这个原因，Spring AMQP 中的这种支持也应该被视为实验性的。该功能已通过 RabbitMQ 3.6.0 和该插件的 0.0.1 版本进行了测试。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

要使用 `RabbitAdmin` 将 exchange 声明为延迟，你可以将 exchange Bean 上的 `delayed` 属性设置为 `true`。`RabbitAdmin` 使用 exchange 类型（`Direct`、`Fanout` 等）来设置 `x-delayed-type` 参数并以 `x-delayed-message` 类型声明 exchange。

当使用 XML 配置 exchange bean 时，`delayed` 属性（默认：`false`）也可用。下面的例子显示了如何使用它：

```xml
<rabbit:topic-exchange name="topic" delayed="true" />
```

要发送一个延迟的消息，你可以通过 `MessageProperties` 设置 `x-delay` 头，如下面的例子所示：

```java
MessageProperties properties = new MessageProperties();
properties.setDelay(15000);
template.send(exchange, routingKey,
        MessageBuilder.withBody("foo".getBytes()).andProperties(properties).build());
rabbitTemplate.convertAndSend(exchange, routingKey, "foo", new MessagePostProcessor() {

    @Override
    public Message postProcessMessage(Message message) throws AmqpException {
        message.getMessageProperties().setDelay(15000);
        return message;
    }

});
```

要检查一条消息是否被延迟，请使用 `MessageProperties` 上的 `getReceivedDelay()` 方法。它是一个独立的属性，以避免无意中传播到由输入消息产生的输出消息。

#### 4.1.14. RabbitMQ REST API

当 management 插件被启用时，RabbitMQ 服务器会公开一个 REST API 以监控和配置 broker。现在 [为该 API 提供了一个 Java Binding](https://github.com/rabbitmq/hop)。`com.rabbitmq.http.client.Client` 是一个标准的、即时的，因此是阻塞式 API。它基于 [Spring Web](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#spring-web) 模块和它的 `RestTemplate` 实现。另一方面，`com.rabbitmq.http.client.ReactorNettyClient` 是一个基于 Reactor Netty 项目的响应式、非阻塞式实现。

hop dependency （`com.rabbitmq:http-client`） 现在也是可选的。

更多信息请见其Javadoc。

#### 4.1.15. 异常处理

使用 RabbitMQ Java 客户端的许多操作都可以抛出检查异常。例如，有很多情况下可能会抛出 `IOException` 实例。`RabbitTemplate`、`SimpleMessageListenerContainer` 和其他 Spring AMQP 组件会捕捉这些异常，并将其转换为 `AmqpException` 层次结构中的一个异常。这些都是在 `org.springframework.amqp` 包中定义的，`AmqpException` 是该层次结构的基础。

当 listener 抛出一个异常时，它被包装成 `ListenerExecutionFailedException`。通常情况下，消息会被拒绝，并由 broker 重新排队。将 `defaultRequeueRejected` 设置为 `false` 会导致消息被丢弃（或被路由到死信 exchange）。正如在 [消息监听器和异步案例](https://springdoc.cn/spring-amqp/#async-listeners) 中所讨论的，监听器可以抛出 `AmqpRejectAndDontRequeueException`（或 `ImmediateRequeueAmqpException`）来有条件地控制这种行为。

然而，有一类错误，监听器不能控制其行为。当遇到无法转换的消息时（例如，一个无效的 `content_encoding` header），在消息到达用户代码之前会抛出一些异常。在 `defaultRequeueRejected` 设置为 `true`（默认）的情况下（或者抛出一个 `ImmediateRequeueAmqpException`），这样的消息会被一次又一次地重新传递。在 1.3.2 版本之前，用户需要写一个自定义的 `ErrorHandler`，正如在 [异常处理](https://springdoc.cn/spring-amqp/#exception-handling) 中所讨论的，以避免这种情况。

从 1.3.2 版本开始，默认的 `ErrorHandler` 现在是一个 `ConditionalRejectingErrorHandler`，它拒绝（并且不重新发送）那些以不可恢复的错误失败的消息。具体来说，它拒绝以下列错误失败的消息：

- `o.s.amqp…MessageConversionException`: 在使用 `MessageConverter` 转换传入的消息 payload 时可以抛出。
- `o.s.messaging…MessageConversionException`: 如果在映射到 `@RabbitListener` 方法时需要额外的转换，可以由转换服务抛出。
- `o.s.messaging…MethodArgumentNotValidException`: 如果在监听器中使用了验证（例如，`@Valid`），并且验证失败，则可以抛出。
- `o.s.messaging…MethodArgumentTypeMismatchException`: 如果入站消息被转换为对目标方法不正确的类型，则可能被抛出。例如，参数被声明为 `Message<Foo>`，但收到的是 `Message<Bar>`。
- `java.lang.NoSuchMethodException`: 在1.6.3版本中添加。
- `java.lang.ClassCastException`: 在1.6.3版本中添加。

你可以用 `FatalExceptionStrategy` 来配置这个 error handler 的实例，这样用户就可以为有条件的消息拒绝提供自己的规则—例如，对 Spring Retry 的 `BinaryExceptionClassifier` 的委托实现（[消息监听器和异步案例](https://springdoc.cn/spring-amqp/#async-listeners)）。此外，`ListenerExecutionFailedException` 现在有一个 `failedMessage` 属性，你可以在决策中使用。如果 `FatalExceptionStrategy.isFatal()` 方法返回 `true`，error handler 会抛出 `AmqpRejectAndDontRequeueException`。默认的 `FatalExceptionStrategy` 在确定一个异常是致命的时候会记录一个警告信息。

从 1.6.3 版本开始，将用户异常添加到致命列表中的一个方便方法是将 `ConditionalRejectingErrorHandler.DefaultExceptionStrategy` 子类化，并重写 `isUserCauseFatal(Throwable cause)` 方法，使致命异常返回 `true`。

处理 DLQ 消息的常见模式是在这些消息上设置一个 `time-to-live`，以及额外的 DLQ 配置，使这些消息过期并被送回主队列进行重试。这种技术的问题是导致致命异常的消息会永远循环。从2.1版开始，`ConditionalRejectingErrorHandler` 检测到消息上的 `x-death` header，导致抛出一个致命的异常。该消息被记录下来并被丢弃。你可以通过将 `ConditionalRejectingErrorHandler` 上的 `discardFatalsWithXDeath` 属性设置为 `false` 来恢复到以前的行为。

|      | 从 2.1.9 版本开始，即使容器的确认模式（acknowledge mode）是 `MANUAL`，带有这些致命异常的消息也会被拒绝，而且默认情况下不会重新获取。这些异常通常发生在监听器被调用之前，所以监听器没有机会确认或拒绝消息，所以它仍然在队列中处于未被确认的状态。要恢复到以前的行为，将 `ConditionalRejectingErrorHandler` 上的 `rejectManual` 属性设置为 `false`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 4.1.16. 事务

Spring Rabbit 框架支持同步和异步用例中的自动事务管理，有许多不同的语义可以声明性地选择，这对于Spring 事务的现有用户来说是熟悉的。这使得许多（如果不是大多数）常见的消息传递模式易于实现。

有两种方法可以向框架发出所需事务语义的信号。在 `RabbitTemplate` 和 `SimpleMessageListenerContainer` 中，有一个标志 `channelTransacted`，如果为 `true`，则告诉框架使用事务性 channel，并以提交或回滚（取决于结果）结束所有操作（发送或接收），并以异常信号表示回滚。另一个信号是用 Spring 的 `PlatformTransactionManager` 实现之一提供一个外部事务，作为正在进行的操作的上下文。如果在框架发送或接收消息时，已经有一个事务在进行中，并且 `channelTransacted` 标志为 `true`，那么消息传递事务的提交或回滚将被推迟到当前事务结束时。如果 `channelTransacted` 标志为 `false`，则没有事务语义适用于消息传递操作（它是自动返回的）。

`channelTransacted` 标志是一个配置时间设置。它在AMQP组件被创建时被声明和处理一次，通常是在应用程序启动时。外部事务原则上更加动态，因为系统会在运行时对当前线程状态做出响应。然而，在实践中，当事务被声明地分层到一个应用程序上时，它往往也是一个配置设置。

对于使用 `RabbitTemplate` 的同步用例，外部事务是由调用者提供的，可以是声明性的，也可以是强制性的（通常是 Spring 事务模型）。下面的示例显示了一种声明式方法（通常是首选，因为它是非侵入性的），其中模板已被配置为 `channelTransacted=true`：

```java
@Transactional
public void doSomething() {
    String incoming = rabbitTemplate.receiveAndConvert();
    // do some more database processing...
    String outgoing = processInDatabaseAndExtractReply(incoming);
    rabbitTemplate.convertAndSend(outgoing);
}
```

在前面的例子中，一个 `String` payload 被接收、转换，并作为标记为 `@Transactional` 的方法内的消息体发送。如果数据库处理因异常而失败，则传入的消息被返回给 broker，而传出的消息不被发送。这适用于在事务性方法链中使用 `RabbitTemplate` 的任何操作（除非，例如，直接操作 `Channel` 以提前提交事务）。

对于使用 `SimpleMessageListenerContainer` 的异步用例，如果需要外部事务，则必须由容器在设置监听器时请求它。为了发出需要外部事务的信号，用户在配置容器时向它提供了 `PlatformTransactionManager` 的实现。下面的例子显示了如何做到这一点：

```java
@Configuration
public class ExampleExternalTransactionAmqpConfiguration {

    @Bean
    public SimpleMessageListenerContainer messageListenerContainer() {
        SimpleMessageListenerContainer container = new SimpleMessageListenerContainer();
        container.setConnectionFactory(rabbitConnectionFactory());
        container.setTransactionManager(transactionManager());
        container.setChannelTransacted(true);
        container.setQueueName("some.queue");
        container.setMessageListener(exampleListener());
        return container;
    }

}
```

在前面的例子中，事务管理器被添加为从另一个Bean定义中注入的依赖关系（未显示），并且 `channelTransacted` 标志也被设置为 `true`。其效果是，如果监听器因异常而失败，事务被回滚，消息也被返回给 broker。重要的是，如果事务提交失败（例如，由于数据库约束错误或连接问题），AMQP事务也会回滚，消息也会返回给 broker。这有时被称为 "最佳努力1阶段提交"，是一种非常强大的可靠消息传递模式。如果在前面的例子中，`channelTransacted` 标志被设置为 `false`（默认值），外部事务仍将为监听器提供，但所有的消息传递操作都将被自动恢复，所以效果是即使在业务操作回滚时也要提交消息传递操作。

##### 条件性回滚

在 1.6.6 版之前，当使用外部事务管理器（如 JDBC）时，向容器的 `transactionAttribute` 添加回滚规则没有效果。异常总是使事务回滚。

另外，当在容器的 advice 链中使用 [transaction advice](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/html/transaction.html#transaction-declarative) 时，条件性回滚不是很有用，因为所有的监听器异常都被包裹在 `ListenerExecutionFailedException` 中。

第一个问题已经被纠正，现在规则被正确应用。此外，现在提供了 `ListenerFailedRuleBasedTransactionAttribute`。它是 `RuleBasedTransactionAttribute` 的一个子类，唯一的区别是它知道 `ListenerExecutionFailedException` 并将这种异常的原因用于规则。这个事务属性可以直接在容器中使用，也可以通过 transaction advice 使用。

下面的例子使用了这个规则：

```java
@Bean
public AbstractMessageListenerContainer container() {
    ...
    container.setTransactionManager(transactionManager);
    RuleBasedTransactionAttribute transactionAttribute =
        new ListenerFailedRuleBasedTransactionAttribute();
    transactionAttribute.setRollbackRules(Collections.singletonList(
        new NoRollbackRuleAttribute(DontRollBackException.class)));
    container.setTransactionAttribute(transactionAttribute);
    ...
}
```

##### 关于回滚收到的信息的说明

AMQP 事务只适用于发送给 broker 的消息和acks。因此，当 Spring 事务发生回滚并收到消息时，Spring AMQP不仅要回滚该事务，还要手动拒绝该消息（有点像 nack，但这不是规范中的叫法）。拒绝消息时采取的行动与事务无关，取决于 `defaultRequeueRejected` 属性（默认：`true`）。关于拒绝失败的消息的更多信息，请参阅 [消息监听器和异步案例](https://springdoc.cn/spring-amqp/#async-listeners)。

有关 RabbitMQ 事务及其限制的更多信息，请参阅 [RabbitMQ Broker 语义](https://www.rabbitmq.com/semantics.html)。

|      | 在 RabbitMQ 2.7.0 之前，这类消息（以及在 channel 关闭或中止时被解包的任何消息）会进入 Rabbit Broker 上的队列后面。自 2.7.0 以来，被拒绝的消息以类似于 JMS 回滚消息的方式进入队列前面。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 以前，事务回滚时的消息请求在本地事务和提供 `TransactionManager` 时是不一致的。在前一种情况下，正常的请求逻辑（`AmqpRejectAndDontRequeueException` 或 `defaultRequeueRejected=false`）适用（见 [消息监听器和异步案例](https://springdoc.cn/spring-amqp/#async-listeners)）。有了事务管理器，消息在回滚时被无条件地重新获取。从2.0版本开始，行为是一致的，正常的请求逻辑在两种情况下都被应用。要恢复到以前的行为，你可以把容器的 `alwaysRequeueWithTxManagerRollback` 属性设置为 `true`。参见 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 使用 `RabbitTransactionManager`

`RabbitTransactionManager` 是在外部事务中执行 Rabbit 操作并与之同步的一种选择。该事务管理器是 [`PlatformTransactionManager`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/transaction/PlatformTransactionManager.html) 接口的实现，应与单个 Rabbit `ConnectionFactory` 一起使用。

|      | 这种策略不能提供XA事务—例如，为了在消息传递和数据库访问之间共享事务。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

应用程序代码需要通过 `ConnectionFactoryUtils.getTransactionalResourceHolder(ConnectionFactory, boolean)` 检索事务性 Rabbit 资源，而不是通过标准的 `Connection.createChannel()` 调用来创建后续 Channel。当使用 Spring AMQP 的 [RabbitTemplate](https://docs.spring.io/spring-amqp/docs/latest_ga/api/org/springframework/amqp/rabbit/core/RabbitTemplate.html) 时，它将自动检测线程绑定的 channel 并自动参与其事务。

使用 Java 配置，你可以通过使用以下 Bean 来设置一个新的 `RabbitTransactionManager`：

```java
@Bean
public RabbitTransactionManager rabbitTransactionManager() {
    return new RabbitTransactionManager(connectionFactory);
}
```

如果你喜欢XML配置，你可以在你的 XML Application Context 文件中声明以下 bean：

```xml
<bean id="rabbitTxManager"
      class="org.springframework.amqp.rabbit.transaction.RabbitTransactionManager">
    <property name="connectionFactory" ref="connectionFactory"/>
</bean>
```

##### 事务同步

将 RabbitMQ 事务与其他一些（例如 DBMS）事务同步提供了 "尽最大努力的第一阶段提交" 语义。RabbitMQ 事务有可能在事务同步完成后的阶段提交失败。这被 `spring-tx` 基础设施记录为一个错误，但不会向调用代码抛出异常。从 2.3.10 版开始，你可以在事务提交后在处理该事务的同一线程上调用 `ConnectionUtils.checkAfterCompletion()` 。如果没有发生异常，它将简单地返回；否则，它将抛出一个 `AfterCompletionFailedException`，它将有一个代表完成的同步状态的属性。

通过调用 `ConnectionFactoryUtils.enableAfterCompletionFailureCapture(true)` 启用该功能；这是一个全局标志，适用于所有线程。

#### 4.1.17. 消息监听器容器配置

有相当多的选项用于配置与事务和服务质量相关的 `SimpleMessageListenerContainer`（SMLC）和 `DirectMessageListenerContainer`（DMLC），其中一些是相互影响的。适用于SMLC、DMLC或 `StreamListenerContainer`（StLC）（见 [使用 RabbitMQ Stream 插件](https://springdoc.cn/spring-amqp/#stream-support)）的属性由相应列中的复选标记表示。参见 [选择容器](https://springdoc.cn/spring-amqp/#choose-container) 的信息，以帮助你决定哪种容器适合你的应用。

下表显示了使用命名空间配置 `<rabbit:listener-container/>` 时的容器属性名称及其对应的属性名称（在括号内）。该元素上的 `type` 属性可以是 `simple`（默认）或 `direct`，以分别指定 `SMLC` 或 `DMLC`。有些属性没有被命名空间公开。这些属性用 `N/A` 来表示。

| 属性                                                         | 说明                                                         | SMLC | DMLC | StLC |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :--- | :--- | :--- |
| [`ackTimeout`](https://springdoc.cn/spring-amqp/#ackTimeout) (N/A) | 当 `messagesPerAck` 被设置时，这个超时被用来作为发送ack的替代。当一个新的消息到达时，未acked消息的数量与 `messagesPerAck` 相比较，而自上次ack以来的时间与这个值相比较。如果任一条件为 `true`，该消息被确认。当没有新消息到达并且有未acked的消息时，这个超时是近似的，因为这个条件只在每个 `monitorInterval` 检查。请参见本表中的 `messagesPerAck` 和 `monitorInterval`。 |      |      |      |
| [`acknowledgeMode`](https://springdoc.cn/spring-amqp/#acknowledgeMode) (acknowledge) | `NONE`: 不发送 ack（与 `channelTransacted=true` 不兼容）。RabbitMQ 称之为 “autoack”，因为 broker 假定所有消息都被acked，而不需要来自消费者的任何操作。`MANUAL`: listener 必须通过调用 `Channel.basicAck()` 来确认所有的消息。`AUTO`: 容器会自动确认消息，除非 `MessageListener` 抛出一个异常。注意 `acknowledgeMode` 是对 `channelTransacted` 的补充—如果 channel 是事务性的，那么除了 ack 之外，broker 还需要一个提交通知。这是默认 mode。另见 `batchSize`。 |      |      |      |
| [`adviceChain`](https://springdoc.cn/spring-amqp/#adviceChain) (advice-chain) | 应用于 listener 执行的 AOP Advice 数组。这可以用来应用额外的切点，比如在 broker 死亡的情况下自动重试。请注意，在AMQP出错后的简单重连是由 `CachingConnectionFactory` 处理的，只要 broker 还活着。 |      |      |      |
| [`afterReceivePostProcessors`](https://springdoc.cn/spring-amqp/#afterReceivePostProcessors) (N/A) | 在调用 listener 之前被调用的 `MessagePostProcessor` 实例的一个数组。后处理程序可以实现 `PriorityOrdered` 或 `Ordered`。该数组被排序，未排序的成员最后被调用。如果一个后处理器返回 `null`，则消息被丢弃（如果合适，则被确认）。 |      |      |      |
| [`alwaysRequeueWithTxManagerRollback`](https://springdoc.cn/spring-amqp/#alwaysRequeueWithTxManagerRollback) (N/A) | 设置为 `true`，当配置了事务管理器时，在回滚时总是重新获取信息。 |      |      |      |
| [`autoDeclare`](https://springdoc.cn/spring-amqp/#autoDeclare) (auto-declare) | 当设置为 `true`（默认）时，如果容器在启动期间检测到它的至少一个队列丢失，也许是因为它是一个 `auto-delete` 或过期的队列，则容器使用 `RabbitAdmin` 来重新声明所有 AMQP 对象（队列、exchange、binding），但如果队列因任何原因丢失，则重新声明继续进行。要禁用这种行为，请将此属性设置为 `false`。注意，如果容器的所有队列都丢失了，那么容器就无法启动。在1.6版本之前，如果 context 中有一个以上的 admin，容器会随机选择一个。如果没有 admin，它将在内部创建一个。在这两种情况下，这可能会导致意外的结果。从 1.6 版开始，为了使 `autoDeclare` 起作用，context 中必须正好有一个 `RabbitAdmin`，或者必须使用 `rabbitAdmin` 属性在容器上配置对特定实例的引用。 |      |      |      |
| [`autoStartup`](https://springdoc.cn/spring-amqp/#autoStartup) (auto-startup) | 标志，表示容器应该在 `ApplicationContext` 启动时启动（作为 `SmartLifecycle` 回调的一部分，它发生在所有 Bean 被初始化之后）。默认为 `true`，但是如果你的 broker 在启动时可能不可用，你可以把它设置为 `false`，然后在你知道 broker 已经准备好时手动调用 `start()`。 |      |      |      |
| [`batchSize`](https://springdoc.cn/spring-amqp/#batchSize) (transaction-size) (batch-size) | 当与 `acknowledgeMode` 设置为 `AUTO` 一起使用时，容器在发送 ack 之前会尝试处理最多这个数量的消息（等待每个消息，直到接收超时设置）。这也是当一个事务性 channel 被提交时。如果 `prefetchCount` 小于 `batchSize`，它将被增加到与 `batchSize` 一致。 |      |      |      |
| [`batchingStrategy`](https://springdoc.cn/spring-amqp/#batchingStrategy) (N/A) | 调配消息时使用的策略。默认为 `SimpleDebatchingStrategy`。见 [批处理](https://springdoc.cn/spring-amqp/#template-batching) 和 [带有批处理功能的 @RabbitListener](https://springdoc.cn/spring-amqp/#receiving-batch)。 |      |      |      |
| [`channelTransacted`](https://springdoc.cn/spring-amqp/#channelTransacted) (channel-transacted) | Boolean 标志，表示在一个事务中所有消息都应该被确认（手动或自动）。 |      |      |      |
| [`concurrency`](https://springdoc.cn/spring-amqp/#concurrency) (N/A) | `m-n` 每个 listener 的并发消费者的范围（min， max）。如果只提供 `n`，则 `n` 是一个固定的消费者数量。见 [并发的 Listener](https://springdoc.cn/spring-amqp/#listener-concurrency)。 |      |      |      |
| [`concurrentConsumers`](https://springdoc.cn/spring-amqp/#concurrentConsumers) (concurrency) | 最初为每个 listener 启动的并发消费者的数量。见 [并发的 Listener](https://springdoc.cn/spring-amqp/#listener-concurrency)。对于 `StLC`，并发性是通过重载的 `superStream` 方法控制的；见 [用单一活动消费者消费超级流](https://springdoc.cn/spring-amqp/#super-stream-consumer)。 |      |      |      |
| [`connectionFactory`](https://springdoc.cn/spring-amqp/#connectionFactory) (connection-factory) | 对 `ConnectionFactory` 的引用。当通过使用 XML 命名空间进行配置时，默认引用的 bean 名称是 `rabbitConnectionFactory`。 |      |      |      |
| [`consecutiveActiveTrigger`](https://springdoc.cn/spring-amqp/#consecutiveActiveTrigger) (min-consecutive-active) | 当考虑启动一个新的消费者时，消费者收到的连续消息的最小数量，没有发生接收超时。也受 'batchSize' 的影响。见 [并发的 Listener](https://springdoc.cn/spring-amqp/#listener-concurrency)。默认：`10`。 |      |      |      |
| [`consecutiveIdleTrigger`](https://springdoc.cn/spring-amqp/#consecutiveIdleTrigger) (min-consecutive-idle) | 消费者在考虑停止消费之前必须经历的最小接收超时数。也受 'batchSize' 的影响。请参阅 [并发的 Listener](https://springdoc.cn/spring-amqp/#listener-concurrency)性。默认：`10`。 |      |      |      |
| [`consumerBatchEnabled`](https://springdoc.cn/spring-amqp/#consumerBatchEnabled) (batch-enabled) | 如果 `MessageListener` 支持，将其设置为 `true`，就可以对离散的消息进行批处理，批处理的大小不超过 `batchSize`；如果在 `receiveTimeout` 中没有新的消息到达，就会交付部分批处理。当此选项为 `false` 时，批处理只支持由生产者创建的批次；见 [批处理](https://springdoc.cn/spring-amqp/#template-batching)。 |      |      |      |
| [`consumerCustomizer`](https://springdoc.cn/spring-amqp/#consumerCustomizer) (N/A) | 一个 `ConsumerCustomizer` Bean，用于修改由容器创建的 stream 消费者。 |      |      |      |
| [`consumerStartTimeout`](https://springdoc.cn/spring-amqp/#consumerStartTimeout) (N/A) | 等待消费者线程启动的时间，单位是毫秒。如果这个时间过了，就会写一个错误日志。一个可能发生这种情况的例子是，如果配置的 `taskExecutor` 没有足够的线程来支持容器中的 `concurrentConsumers`。见 [线程和异步消费者](https://springdoc.cn/spring-amqp/#threading)。默认值：60000（1分钟）。 |      |      |      |
| [`consumerTagStrategy`](https://springdoc.cn/spring-amqp/#consumerTagStrategy) (consumer-tag-strategy) | 设置一个 [ConsumerTagStrategy](https://springdoc.cn/spring-amqp/#consumerTags) 的实现，使每个消费者能够创建一个（唯一的）标签（tag）。 |      |      |      |
| [`consumersPerQueue`](https://springdoc.cn/spring-amqp/#consumersPerQueue) (consumers-per-queue) | 为每个配置的队列创建消费者的数量。见 [并发的 Listener](https://springdoc.cn/spring-amqp/#listener-concurrency)。 |      |      |      |
| [`consumeDelay`](https://springdoc.cn/spring-amqp/#consumeDelay) (N/A) | 当使用 `concurrentConsumers > 1` 的 [RabbitMQ Sharding Plugin](https://github.com/rabbitmq/rabbitmq-sharding) 时，存在一个竞赛条件，它可以阻止消费者在分片上的均匀分布。使用此属性在消费者启动之间添加一个小的延迟，以避免这种竞赛条件。你应该试验一些值，以确定适合你的环境的延迟。 |      |      |      |
| [`debatchingEnabled`](https://springdoc.cn/spring-amqp/#debatchingEnabled) (N/A) | 当为 `true` 时，listener 容器将对成批的消息进行调试，并对成批的每个消息调用 listener。从2.2.7版本开始，如果 listener 是 `BatchMessageListener` 或 `ChannelAwareBatchMessageListener`，生产者创建的批处理将作为 `List<Message>` 被调试。否则，批次中的消息将被逐一呈现。默认为 `true`。参见 [批处理](https://springdoc.cn/spring-amqp/#template-batching) 和 [带有批处理功能的 @RabbitListener](https://springdoc.cn/spring-amqp/#receiving-batch)。 |      |      |      |
| [`declarationRetries`](https://springdoc.cn/spring-amqp/#declarationRetries) (declaration-retries) | 被动队列声明失败时重试的次数。被动队列声明发生在消费者启动时，或者当从多个队列中消费时，在初始化期间并非所有队列都可用。当重试次数用完后，没有一个配置的队列可以被被动地声明（出于任何原因），容器的行为由前面描述的 'missingQueuesFatal' 属性控制。默认情况下： 三次重试（总共四次尝试）。 |      |      |      |
| [`defaultRequeueRejected`](https://springdoc.cn/spring-amqp/#defaultRequeueRejected) (requeue-rejected) | 决定是否应该重新接收因 listener 抛出异常而被拒绝的消息。默认值：`true`。 |      |      |      |
| [`errorHandler`](https://springdoc.cn/spring-amqp/#errorHandler) (error-handler) | 对 `ErrorHandler` 策略的引用，用于处理 `MessageListener` 执行过程中可能出现的任何未捕获的异常。默认： `ConditionalRejectingErrorHandler`。 |      |      |      |
| [`exclusive`](https://springdoc.cn/spring-amqp/#exclusive) (exclusive) | 确定这个容器中的单个消费者是否有对队列的独占访问权。当这一点为 `true` 时，容器的并发量必须是 1。如果另一个消费者有独占访问权，那么容器就会根据 `recovery-interval` 或 `recovery-back-off` 来尝试恢复消费者。当使用命名空间时，这个属性与队列名称一起出现在 `<rabbit:listener/>` 元素上。默认值：`false`。 |      |      |      |
| [`exclusiveConsumerExceptionLogger`](https://springdoc.cn/spring-amqp/#exclusiveConsumerExceptionLogger) (N/A) | 当排他性消费者无法获得对队列的访问时，使用的异常 logger。默认情况下，这被记录在 `WARN` 级别。 |      |      |      |
| [`failedDeclarationRetryInterval`](https://springdoc.cn/spring-amqp/#failedDeclarationRetryInterval) (failed-declaration -retry-interval) | 被动队列声明重试的间隔时间。被动队列声明发生在消费者启动时，或者当从多个队列中消费时，在初始化期间并非所有队列都可用。默认值：5000（5秒）。 |      |      |      |
| [`forceCloseChannel`](https://springdoc.cn/spring-amqp/#forceCloseChannel) (N/A) | 如果消费者在 `shutdownTimeout` 内没有响应 shutdown，如果这是 `true`，channel 将被关闭，导致任何未acked的消息被重新进入队列。从2.0开始默认为 `true`。你可以把它设置为 `false` 来恢复到以前的行为。 |      |      |      |
| [`globalQos`](https://springdoc.cn/spring-amqp/#globalQos) (global-qos) | 当为 `true` 时，`prefetchCount` 全局应用于 channel，而不是应用于 channel 上的每个消费者。参见 [`basicQos.global`](https://www.rabbitmq.com/amqp-0-9-1-reference.html#basic.qos.global) 获取更多信息。 |      |      |      |
| (group)                                                      | 这只有在使用命名空间（namespace）时才可用。当被指定时，一个类型为 `Collection<MessageListenerContainer>` 的bean被注册为这个名字，并且每个 `<listener/>` 元素的容器被添加到集合中。例如，这允许通过迭代集合来启动和停止这组容器。如果多个 `<listener-container/>` 元素有相同的 group 值，集合中的容器就会形成所有如此指定的容器的集合。 |      |      |      |
| [`idleEventInterval`](https://springdoc.cn/spring-amqp/#idleEventInterval) (idle-event-interval) | 见 [检测闲置的异步消费者](https://springdoc.cn/spring-amqp/#idle-containers)。 |      |      |      |
| [`javaLangErrorHandler`](https://springdoc.cn/spring-amqp/#javaLangErrorHandler) (N/A) | 一个 `AbstractMessageListenerContainer.JavaLangErrorHandler` 的实现，当容器线程捕捉到一个 `Error` 时被调用。默认实现调用 `System.exit(99)`；要恢复到以前的行为（什么都不做），请添加一个 no-op handler。 |      |      |      |
| [`maxConcurrentConsumers`](https://springdoc.cn/spring-amqp/#maxConcurrentConsumers) (max-concurrency) | 如果需要，按需启动的最大并发消费者数量。必须大于或等于 'concurrentConsumers'。见 [并发的 Listener](https://springdoc.cn/spring-amqp/#listener-concurrency)。 |      |      |      |
| [`messagesPerAck`](https://springdoc.cn/spring-amqp/#messagesPerAck) (N/A) | 在 acks 之间要接收的信息数量。使用这个属性可以减少发送给 broker 的acks的数量（代价是增加重新交付消息的可能性）。一般来说，你应该只在高容量的监听器容器上设置这个属性。如果设置了这个属性，并且消息被拒绝（抛出的异常），待处理的acks被确认，失败的消息被拒绝。不允许使用事务 channel。如果 `prefetchCount` 小于 `messagesPerAck`，它会被增加到与 `messagesPerAck` 一致。默认情况下：每条消息都要进行检查。也请看本表中的 `ackTimeout`。 |      |      |      |
| [`mismatchedQueuesFatal`](https://springdoc.cn/spring-amqp/#mismatchedQueuesFatal) (mismatched-queues-fatal) | 当容器启动时，如果这个属性为 `true`（默认为 `false`），容器会检查在上下文中声明的所有队列是否与 broker 上已有的队列兼容。如果存在不匹配的属性（如 `auto-delete`）或参数（如 `x-message-ttl`），那么容器（和应用程序上下文）将无法启动，并出现致命的异常。如果在恢复过程中发现问题（例如，在失去连接后），容器就会停止。在 application context 中必须有一个 `RabbitAdmin`（或者通过使用 `rabbitAdmin` 属性在容器上特别配置的一个）。否则，此属性必须为 `false`。如果在初始启动时 broker 不可用，容器就会启动，并在建立连接时检查条件。该检查是针对 context 中的所有队列进行的，而不仅仅是某个监听器被配置为使用的队列。如果你希望将检查限制为仅由容器使用的队列，你应该为该容器配置一个单独的 `RabbitAdmin`，并使用 `rabbitAdmin` 属性提供对它的引用。请参阅 [条件式声明](https://springdoc.cn/spring-amqp/#conditional-declaration) 以了解更多信息。在为被标记为 `@Lazy` 的 bean 中的 `@RabbitListener` 启动容器时，不匹配的队列参数检测被禁用。这是为了避免潜在的死锁，因为死锁会使这类容器的启动延迟60秒。使用 lazy listener Bean 的应用程序应该在获得对 lazy Bean 的引用之前检查队列参数。 |      |      |      |
| [`missingQueuesFatal`](https://springdoc.cn/spring-amqp/#missingQueuesFatal) (missing-queues-fatal) | 当设置为 `true`（默认）时，如果配置的队列中没有一个在 broker 上可用，则被认为是致命的。这将导致 application context 在启动时无法初始化。另外，当容器运行时删除队列时，默认情况下，消费者会进行三次重试以连接到队列（间隔五秒），如果这些尝试失败，则停止容器。这在以前的版本中是无法配置的。当设置为 `false` 时，在进行了三次重试之后，容器会进入恢复模式，就像其他问题一样，比如说 broker 被关闭。容器会根据 `recoveryInterval` 属性尝试进行恢复。在每次恢复尝试中，每个消费者再次尝试四次，以五秒钟的间隔被动地声明队列。这个过程无限期地持续下去。你也可以使用 properties bean 来为所有容器设置全局属性，如下所示：`<util:properties        id="spring.amqp.global.properties">    <prop key="mlc.missing.queues.fatal">        false    </prop> </util:properties>`这个全局属性不应用于任何设置了显式 `missingQueuesFatal` 属性的容器。默认的重试（retry）属性（以五秒钟的间隔进行三次重试）可以通过设置以下属性而被覆盖。在为被标记为 `@Lazy` 的 bean 中的 `@RabbitListener` 启动容器时，缺失队列检测被禁用。这是为了避免潜在的死锁，因为死锁会使这类容器的启动延迟60秒。使用 lazy listener Bean 的应用程序应该在获得对 lazy Bean 的引用之前检查队列。 |      |      |      |
| [`monitorInterval`](https://springdoc.cn/spring-amqp/#monitorInterval) (monitor-interval) | 在DMLC中，一个任务被安排在这个时间间隔内运行，以监测消费者的状态并恢复任何已经失败的消费者。 |      |      |      |
| [`noLocal`](https://springdoc.cn/spring-amqp/#noLocal) (N/A) | 设置为 `true`，以禁止从服务器向在同一 channel 连接上发布的消费者信息传递。 |      |      |      |
| [`phase`](https://springdoc.cn/spring-amqp/#phase) (phase)   | 当 `autoStartup` 为 `true` 时，这个容器应该在哪个生命周期阶段启动和停止。这个值越小，这个容器就越早开始，越晚停止。默认值是 `Integer.MAX_VALUE`，意味着容器尽可能晚地开始，尽可能快地停止。 |      |      |      |
| [`possibleAuthenticationFailureFatal`](https://springdoc.cn/spring-amqp/#possibleAuthenticationFailureFatal) (possible-authentication-failure-fatal) | 当设置为 `true` 时（SMLC 的默认值），如果在连接过程中抛出 `PossibleAuthenticationFailureException`，它将被视为致命的。这将导致应用程序上下文在启动时无法初始化（如果容器被配置为自动启动）。从2.0版本开始。**DirectMessageListenerContainer**当设置为 `false`（默认）时，每个消费者将尝试根据 `monitorInterval` 重新连接。**SimpleMessageListenerContainer**当设置为 `false` 时，在进行了3次重试之后，容器将进入恢复模式，就像其他问题一样，比如说 broker 被关闭。容器将根据 `recoveryInterval` 属性来尝试恢复。在每次恢复尝试中，每个消费者将再次尝试4次启动。这个过程将无限期地持续下去。你也可以使用 properties bean 来为所有容器设置全局属性，如下所示：`<util:properties    id="spring.amqp.global.properties">  <prop    key="mlc.possible.authentication.failure.fatal">     false  </prop> </util:properties>`这个全局属性将不会应用于任何设置了显式 `missingQueuesFatal` 属性的容器。默认的重试（retry）属性（以5秒的间隔重试3次）可以用这个属性之后的属性来覆盖。 |      |      |      |
| [`prefetchCount`](https://springdoc.cn/spring-amqp/#prefetchCount) (prefetch) | 每个消费者可以未被确认的消息的数量。这个值越大，消息的传递速度就越快，但非连续处理的风险也越大。如果 `acknowledgeMode` 是 `NONE`，则忽略不计。如果有必要，这个值会增加，以匹配 `batchSize` 或 `messagePerAck`。从 2.0 开始默认为 250。你可以把它设置为 1 来恢复到以前的行为。在有些情况下，prefetch 值应该很低—例如，对于大的消息，特别是在处理速度很慢的情况下（消息可能会在客户端进程中增加大量的内存），以及如果需要严格的消息排序（在这种情况下，prefetch 应该被设回1）。另外，对于低容量的消息传递和多个消费者（包括单个监听器容器实例内的并发性），你可能希望减少 prefetch，以便在消费者之间获得更均匀的消息分布。另见 `globalQos`。 |      |      |      |
| [`rabbitAdmin`](https://springdoc.cn/spring-amqp/#rabbitAdmin) (admin) | 当一个监听器容器监听至少一个自动删除队列并且在启动期间发现它丢失时，该容器使用 `RabbitAdmin` 来声明该队列和任何相关的 binding 和 exchange。如果这些元素被配置为使用条件式声明（见 [条件式声明](https://springdoc.cn/spring-amqp/#conditional-declaration)），那么容器必须使用被配置为声明这些元素的 admin。在这里指定那个admin。只有在使用具有条件式声明的自动删除队列时，才需要它。如果你不希望自动删除队列在容器启动之前被声明，请在 admin 上将 `auto-startup` 设置为 `false`。默认为声明所有非条件性元素的 `RabbitAdmin`。 |      |      |      |
| [`receiveTimeout`](https://springdoc.cn/spring-amqp/#receiveTimeout) (receive-timeout) | 每个消息的最大等待时间。如果 `acknowledgeMode=NONE`，它的影响很小—容器会旋转并要求得到另一条消息。对于 `batchSize > 1` 的事务性 `Channel` 来说，它的影响最大，因为它可能导致已经消费的消息在超时之前不被确认。当 `consumerBatchEnabled` 为 `true` 时，如果在一个批处理完成之前发生超时，就会交付一个部分批处理。 |      |      |      |
| [`recoveryBackOff`](https://springdoc.cn/spring-amqp/#recoveryBackOff) (recovery-back-off) | 如果消费者因非致命原因而无法启动，则指定尝试启动消费者的间隔时间的 `BackOff`。默认是 `FixedBackOff`，每五秒重试一次，没有限制。与 `recoveryInterval` 相互排斥。 |      |      |      |
| [`recoveryInterval`](https://springdoc.cn/spring-amqp/#recoveryInterval) (recovery-interval) | 确定在消费者因非致命原因无法启动时，尝试启动消费者的间隔时间，以毫秒计。默认值：5000。与 `recoveryBackOff` 相互排斥。 |      |      |      |
| [`retryDeclarationInterval`](https://springdoc.cn/spring-amqp/#retryDeclarationInterval) (missing-queue- retry-interval) | 如果在消费者初始化期间，配置的队列的一个子集可用，消费者就开始从这些队列消费。消费者试图通过使用这个时间间隔来被动地声明缺少的队列。当这个时间间隔过后，再次使用 'declarationRetries' 和 'failedDeclarationRetryInterval'。如果仍然有缺失的队列，消费者在再次尝试之前再次等待这个时间间隔。这个过程无限期地持续下去，直到所有队列都可用。默认值：60000（1分钟）。 |      |      |      |
| [`shutdownTimeout`](https://springdoc.cn/spring-amqp/#shutdownTimeout) (N/A) | 当一个容器关闭时（例如，如果它所包裹的 `ApplicationContext` 被关闭），它将等待飞行中的消息被处理，最多到这个限制。默认为 5 秒。 |      |      |      |
| [`startConsumerMinInterval`](https://springdoc.cn/spring-amqp/#startConsumerMinInterval) (min-start-interval) | 在每个新的消费者按需启动之前必须经过的时间，单位是毫秒。见 [并发的 Listener](https://springdoc.cn/spring-amqp/#listener-concurrency).。默认：10000（10秒）。 |      |      |      |
| [`statefulRetryFatal`](https://springdoc.cn/spring-amqp/#statefulRetryFatal) WithNullMessageId (N/A) | 当使用一个有状态的 retry advice 时，如果收到一个缺失 `messageId` 属性的消息，默认情况下，它被认为对消费者来说是致命的（它被停止）。将此设置为 `false`，以丢弃（或路由到一个死信队列）这样的消息。 |      |      |      |
| [`stopConsumerMinInterval`](https://springdoc.cn/spring-amqp/#stopConsumerMinInterval) (min-stop-interval) | 检测到空闲消费者时，自上一个消费者被停止后，在消费者被停止前必须经过的时间，单位是毫秒。见 [并发的 Listener](https://springdoc.cn/spring-amqp/#listener-concurrency)。默认：60000（一分钟）。 |      |      |      |
| [`streamConverter`](https://springdoc.cn/spring-amqp/#streamConverter) (N/A) | 一个 `StreamMessageConverter`，将本地 Stream 消息转换为 Spring AMQP 消息。 |      |      |      |
| [`taskExecutor`](https://springdoc.cn/spring-amqp/#taskExecutor) (task-executor) | 对 Spring `TaskExecutor`（或标准JDK 1.5+ `Executor`）的引用，用于执行监听器调用程序。默认是一个 `SimpleAsyncTaskExecutor`，使用内部管理的线程。 |      |      |      |
| [`taskScheduler`](https://springdoc.cn/spring-amqp/#taskScheduler) (task-scheduler) | 在DMLC中，调度器用于在 'monitorInterval' 运行监控任务。      |      |      |      |
| [`transactionManager`](https://springdoc.cn/spring-amqp/#transactionManager) (transaction-manager) | listener 操作的外部事务管理器。也是对 `channelTransacted` 的补充—如果 Channel 是事务的，其事务与外部事务同步。 |      |      |      |

#### 4.1.18. 并发的 Listener

##### SimpleMessageListenerContainer

默认情况下，监听器容器启动一个单一的消费者，从队列中接收消息。

在检查上一节的表格时，你可以看到一些控制并发性的属性和特性。最简单的是 `concurrentConsumers`，它创建了并发处理消息的（固定）数量的消费者。

在 1.3.0 版本之前，这是唯一可用的设置，容器必须停止并重新启动才能改变设置。

从1.3.0版本开始，你现在可以动态地调整 `concurrentConsumers` 属性。如果它在容器运行时被改变，消费者会根据需要被添加或删除，以适应新的设置。

此外，还增加了一个名为 `maxConcurrentConsumers` 的新属性，容器会根据工作量动态地调整并发性。这与另外四个属性一起工作：`consecutiveActiveTrigger`、`startConsumerMinInterval`、`consecutiveIdleTrigger` 和 `stopConsumerMinInterval`。在默认设置下，增加消费者的算法工作如下：

如果没有达到 `maxConcurrentConsumers`，并且现有的消费者连续10个周期处于活动状态，并且自上一个消费者被启动以来至少经过了10秒，一个新的消费者被启动。如果一个消费者在 `batchSize` * `receiveTimeout` 毫秒内至少收到一条消息，那么它就被认为是活跃的。

在默认设置下，减少消费者的算法工作如下：

如果有超过 `concurrentConsumers` 的消费者在运行，并且一个消费者检测到连续10次超时（空闲），并且最后一个消费者至少在60秒前被停止，那么一个消费者就会被停止。超时取决于 `receiveTimeout` 和 `batchSize` 属性。如果一个消费者在 `batchSize` * `receiveTimeout` 毫秒内没有收到任何消息，它就被认为是空闲的。因此，在默认的超时（一秒）和 `batchSize` 为 4 的情况下，在40秒的空闲时间后就会考虑停止一个消费者（四个超时对应于一个空闲检测）。

|      | 实际上，只有当整个容器闲置一段时间时，消费者才能被停止。这是因为 broker 在所有活动的消费者之间分享其工作。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

每个消费者使用一个 channel，不管配置的队列数量如何。

从2.0版本开始，`concurrentConsumers` 和 `maxConcurrentConsumers` 属性可以用 `concurrency` 属性设置—例如 `2-4`。

##### 使用 `DirectMessageListenerContainer`

有了这个容器，并发性是基于配置的队列和 `consumersPerQueue`。每个队列的每个消费者使用一个单独的 channel，并发性由 rabbit 客户端库控制。默认情况下，在编写本文时，它使用一个 `DEFAULT_NUM_THREADS = Runtime.getRuntime().availableProcessors() * 2` 线程的池。

你可以配置一个 `taskExecutor` 来提供所需的最大并发性。

#### 4.1.19. 独占的消费者

从1.3版本开始，你可以将监听器容器配置为单一的独占消费者。这可以防止其他容器从队列中消费，直到当前的消费者被取消。这样的容器的并发量（concurrency）必须是 `1`。

当使用独占消费者时，其他容器会尝试根据 `recoveryInterval` 属性从队列中消费，如果尝试失败，会记录一个 `WARN` 消息。

#### 4.1.20. 监听器容器队列

1.3版本为处理一个监听器容器中的多个队列引入了一些改进。

容器最初可以被配置为在零队列上监听。队列可以在运行时被添加和删除。 `SimpleMessageListenerContainer` 回收（取消和重新创建）所有消费者，当任何预先获取的消息被处理后。`DirectMessageListenerContainer` 为每个队列创建/取消单个消费者，而不影响其他队列的消费者。参见 `addQueues`、`addQueueNames`、`removeQueues` 和 `removeQueueNames` 方法的 [Javadoc](https://docs.spring.io/spring-amqp/docs/latest-ga/api/org/springframework/amqp/rabbit/listener/AbstractMessageListenerContainer.html) 。

如果不是所有的队列都是可用的，容器会尝试每60秒被动地声明（并从）缺失的队列中消费。

另外，如果一个消费者收到来自 broker 的取消（例如，如果一个队列被删除），消费者会尝试恢复，恢复后的消费者会继续处理来自任何其他配置的队列的消息。以前，一个队列的取消会取消整个消费者，最终，容器会因为队列的丢失而停止。

如果你想永久地删除一个队列，你应该在删除队列之前或之后更新容器，以避免将来试图从它那里消费。

#### 4.1.21. Resilience: 从 Error 和 Broker 的失败中恢复

Spring AMQP提供的一些关键（也是最受欢迎的）高级功能是在协议错误或 broker 失败的情况下进行恢复和自动重新连接。在本指南中，我们已经看到了所有相关的组件，但在这里把它们集中起来，并单独指出这些功能和恢复方案，应该会有帮助。

主要的重新连接功能是由 `CachingConnectionFactory` 本身启用的。使用 `RabbitAdmin` 的自动声明功能通常也是有益的。此外，如果你关心保证交付，你可能还需要在 `RabbitTemplate` 和 `SimpleMessageListenerContainer` 中使用 `channelTransacted` 标志，并在 `SimpleMessageListenerContainer` 中使用 `AcknowledgeMode.AUTO`（如果你自己进行 acks，则为手动）。

##### Exchange、 Queue 和 Binding 的自动声明

`RabbitAdmin` 组件可以在启动时声明 exchange、queue 和 binding。它通过 `ConnectionListener` 延迟地做这件事。因此，如果 broker 在启动时不存在，这并不重要。在第一次使用 `Connection` 时（例如，通过发送消息），监听器就会启动，并应用 admin 功能。在监听器中做自动声明的另一个好处是，如果 connection 由于任何原因被放弃（例如，broker 死亡、网络故障和其他原因），当 connection 重新建立时，它们会被再次应用。

|      | 以这种方式声明的队列必须有固定的名称—要么明确声明，要么由框架为 `AnonymousQueue` 实例生成。匿名队列是非持久性的，排他性的，并且是自动删除的。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | 只有当 `CachingConnectionFactory` 的缓存模式为 `CHANNEL`（默认）时，才会进行自动声明。存在这个限制是因为独占（exclusive）和自动删除（auto-delete）队列被绑定到连接上。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

从 2.2.2 版开始，`RabbitAdmin` 将检测 `DeclarableCustomizer` 类型的 Bean 并在实际处理声明之前应用该函数。这很有用，例如，在框架内有第一类支持之前设置一个新参数（属性）。

```java
@Bean
public DeclarableCustomizer customizer() {
    return dec -> {
        if (dec instanceof Queue && ((Queue) dec).getName().equals("my.queue")) {
            dec.addArgument("some.new.queue.argument", true);
        }
        return dec;
    };
}
```

在不提供直接访问 `Declarable` bean定义的项目中，它也很有用。

另见 [RabbitMQ 自动连接/拓扑恢复](https://springdoc.cn/spring-amqp/#auto-recovery)。

##### 同步操作中的失败和重试的选项

如果你在使用 `RabbitTemplate` 时在同步序列中失去了与 broker 的连接（例如），Spring AMQP会抛出一个 `AmqpException`（通常，但不总是 `AmqpIOException`）。我们并不试图掩盖有问题的事实，所以你必须能够捕捉并响应异常。如果你怀疑连接丢失（而且不是你的错），最简单的事情就是重新尝试操作。你可以手动操作，或者你可以考虑使用 Spring Retry 来处理重试（强制性的或声明性的）。

Spring Retry 提供了几个AOP拦截器和大量的灵活性来指定重试的参数（尝试次数、异常类型、退避算法等）。Spring AMQP还提供了一些方便的工厂 bean，用于以方便的形式为AMQP用例创建Spring Retry拦截器，有强类型的回调接口，你可以用来实现自定义恢复逻辑。更多细节请参见 Javadoc 以及 `StatefulRetryOperationsInterceptor` 和 `StatelessRetryOperationsInterceptor` 的属性。如果没有事务，或者在重试回调中启动了一个事务，那么无状态重试是合适的。请注意，无状态重试比有状态重试更容易配置和分析，但如果有一个正在进行的事务必须回滚或肯定会回滚，那么它通常不适合。在一个事务的中间放弃连接应该和回滚有同样的效果。因此，对于事务在堆栈中较高位置启动的重新连接，有状态重试通常是最佳选择。有状态重试需要一个机制来唯一地识别一个消息。最简单的方法是让发送者在 `MessageId` 消息属性中放一个唯一的值。所提供的消息转换器提供了一个选项：你可以把 `createMessageIds` 设置为 `true`。否则，你可以在拦截器中注入一个 `MessageKeyGenerator` 实现。key 生成器必须为每个消息返回一个唯一的 key。在2.0版本之前的版本中，提供了一个 `MissingMessageIdAdvice`。它使没有 `messageId` 属性的消息被精确地重试一次（忽略重试设置）。这个 advice 不再提供，因为随着 `spring-retry` 1.2版本的推出，它的功能已经内置于拦截器和消息监听器容器。

|      | 为了向后兼容，默认情况下（在一次重试之后），具有空消息ID的消息对消费者来说是致命的（消费者被停止）。为了复制 `MissingMessageIdAdvice` 提供的功能，你可以把监听器容器上的 `statefulRetryFatalWithNullMessageId` 属性设置为 `false`。有了这个设置，消费者继续运行，消息被拒绝（在一次重试之后）。它被丢弃或被路由到死信队列（如果配置了一个）。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

从1.3版本开始，提供了一个 builder API，以帮助使用Java（在 `@Configuration` 类中）来组装这些拦截器。下面的例子展示了如何做到这一点：

```java
@Bean
public StatefulRetryOperationsInterceptor interceptor() {
    return RetryInterceptorBuilder.stateful()
            .maxAttempts(5)
            .backOffOptions(1000, 2.0, 10000) // initialInterval, multiplier, maxInterval
            .build();
}
```

只有重试功能的一个子集可以通过这种方式配置。更高级的功能需要将 `RetryTemplate` 配置为 Spring Bean。请参阅 [Spring Retry Javadoc](https://docs.spring.io/spring-retry/docs/api/current/)，了解有关可用策略及其配置的完整信息。

##### 批处理监听器的重试

不建议用批处理监听器配置重试，除非该批处理是由生产者创建的，在一条记录中。关于消费者和生产者创建的批处理的信息，见 [批处理](https://springdoc.cn/spring-amqp/#de-batching)。对于消费者创建的批处理，框架不知道批处理中的哪条消息导致了失败，所以在重试用完后恢复是不可能的。对于生产者创建的批处理，由于只有一个消息实际失败，整个消息可以被恢复。应用程序可能希望通知自定义恢复器（recoverer）在批处理中发生故障的位置，也许可以通过设置抛出的异常的索引属性。

一个批处理监听器的 retry recoverer 必须实现 `MessageBatchRecoverer`。

##### 消息监听器和异步案例

如果一个 `MessageListener` 因为业务异常而失败，该异常由消息监听器容器处理，然后继续监听另一个消息。如果失败是由放弃的连接引起的（不是业务异常），为监听器收集消息的消费者必须被取消并重新启动。 `SimpleMessageListenerContainer` 无缝地处理这个问题，它留下一个日志，说明监听器正在被重新启动。事实上，它无休止地循环，试图重新启动消费者。只有当消费者确实表现得非常糟糕时，它才会放弃。一个副作用是，如果容器启动时代理机已经停机，它将继续尝试，直到可以建立连接。

相对于协议错误和放弃的连接，业务异常处理可能需要更多的考虑和一些自定义配置，尤其是在使用事务或容器 acks 时。在 2.8.x 之前，RabbitMQ 没有对死信行为进行定义。因此，在默认情况下，因业务异常而被拒绝或回滚的消息可以无休止地重新交付。为了限制客户端的重传次数，一种选择是在监听器的 advice chain 中设置 `StatefulRetryOperationsInterceptor`。这个拦截器可以有一个恢复回调，实现一个自定义的死信动作—无论什么都适合你的特定环境。

另一个办法是把容器的 `defaultRequeueRejected` 属性设置为 `false`。这将导致所有失败的消息被丢弃。当使用 RabbitMQ 2.8.x 或更高版本时，这也有利于将消息传递给死信 exchange。

或者，你可以抛出一个 `AmqpRejectAndDontRequeueException`。这样做可以防止消息重新进入队列，不管默认 `RequeueRejected` 属性的设置如何。

从 2.1 版开始，引入了一个 `ImmediateRequeueAmqpException`，以执行完全相反的逻辑：无论 `defaultRequeueRejected` 属性的设置如何，消息都将被重新获取。

通常情况下，这两种技术的组合被使用。你可以在 advice chain 中使用一个 `StatefulRetryOperationsInterceptor` 和一个抛出 `AmqpRejectAndDontRequeueException` 的 `MessageRecoverer`。当所有的重试都被用完时， `MessageRecover` 被调用。`RejectAndDontRequeueRecoverer` 正是这样做的。默认的 `MessageRecoverer` 消费错误的消息，并发出一个 `WARN` 消息。

从1.3版开始，提供了一个新的 `RepublishMessageRecoverer`，以允许在重试用尽后发布失败的消息。

当恢复者消耗最后的异常时，消息被ack，如果配置的话，不会被 broker 发送到死信 exchange。

|      | 当消费者端使用 `RepublishMessageRecoverer` 时，收到的消息在 `receivedDeliveryMode` 消息属性中具有 `deliveryMode`。在这种情况下，`deliveryMode` 是 `null`。这意味着 broker 上有一个 `NON_PERSISTENT` 交付模式（delivery mode）。从2.0版本开始，你可以配置 `RepublishMessageRecoverer` 的 `deliveryMode`，如果它是 `null`，就设置到消息中重新发布。默认情况下，它使用 `MessageProperties` 默认值 - `MessageDeliveryMode.PERSISTENT`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

下面的例子显示了如何设置一个 `RepublishMessageRecoverer` 作为 recoverer：

```java
@Bean
RetryOperationsInterceptor interceptor() {
    return RetryInterceptorBuilder.stateless()
            .maxAttempts(5)
            .recoverer(new RepublishMessageRecoverer(amqpTemplate(), "something", "somethingelse"))
            .build();
}
```

`RepublishMessageRecoverer` 在发布消息时，在消息 header 中加入了额外的信息，如异常消息、stack trace、原始 exchange 和 routing key。可以通过创建一个子类并重写 `additionalHeaders()` 来添加额外的 header 信息。`deliveryMode`（或任何其他属性）也可以在 `additionalHeaders()` 中改变，如下面的例子所示：

```java
RepublishMessageRecoverer recoverer = new RepublishMessageRecoverer(amqpTemplate, "error") {

    protected Map<? extends String, ? extends Object> additionalHeaders(Message message, Throwable cause) {
        message.getMessageProperties()
            .setDeliveryMode(message.getMessageProperties().getReceivedDeliveryMode());
        return null;
    }

};
```

从2.0.5版本开始，如果 stack trace 太大，可能会被截断；这是因为所有的 header 信息都必须装在一个帧内。默认情况下，如果 stack trace 会导致少于 20,000字节（'headroom'）可用于其他 header，它将被截断。如果你需要更多或更少的空间给其他 header，可以通过设置 recoverer 的 `frameMaxHeadroom` 属性来调整。从2.1.13、2.2.3版本开始，异常信息被包括在这个计算中，stack trace 的数量将通过以下算法被最大化：

- 如果单独的 stack trace 会超过限制，那么异常消息 header 将被截断为97字节，再加上 `…`，stack trace 也被截断。
- 如果 stack trace 很小，信息将被截断（加上 `…`）以适应可用的字节数（但 stack trace 中的信息本身被截断为97字节，加上 `…`）。

每当发生任何形式的截断，原始的异常将被记录下来，以保留完整的信息。评估是在 header 被增强后进行的，所以诸如异常类型等信息可以在表达式中使用。

从2.4.8版本开始，error exchange 和 routing key 可以作为SpEL表达式提供，`Message` 是评估的根对象。

从2.3.3版本开始，提供了一个新的子类 `RepublishMessageRecovererWithConfirms`；它支持两种风格的发布者确认，并将在返回之前等待确认（如果没有确认或消息被返回，则抛出一个异常）。

如果确认类型是 `CORRELATED`，子类也将检测是否有消息返回，并抛出 `AmqpMessageReturnedException`；如果发布的消息被否定了，它将抛出 `AmqpNackReceivedException`。

如果确认类型是 `SIMPLE`，子类将调用 channel 上的 `waitForConfirmsOrDie` 方法。

请参阅 [发布者消息确认和返回](https://springdoc.cn/spring-amqp/#cf-pub-conf-ret)，了解更多关于确认和返回的信息。

从2.1版开始，增加了一个 `ImmediateRequeueMessageRecoverer`，用来抛出一个 `ImmediateRequeueAmqpException`，它通知一个监听器容器重新排队接收当前失败的消息。

##### 用于 Spring Retry 的异常分类

Spring Retry在确定哪些异常可以调用重试方面有很大的灵活性。默认配置是对所有异常进行重试。鉴于用户异常被包裹在 `ListenerExecutionFailedException` 中，我们需要确保分类器检查异常的原因。默认的分类器（classifier ）只看顶层的异常。

从 Spring Retry 1.0.3 开始，`BinaryExceptionClassifier` 有一个属性叫做 `traverseCauses`（默认：`false`）。当为 `true` 时，它会遍历异常原因，直到找到一个匹配的原因或没有原因。

为了将这个分类器（classifier）用于重试，你可以使用一个用构造函数创建的 `SimpleRetryPolicy`，该构造函数接收最大尝试次数、`Exception` 实例 `Map` 和布尔值（`traverseCauses`），并将该策略注入 `RetryTemplate` 中。

#### 4.1.22. 支持多个 Broker （或集群）

2.3 版在单个应用程序和多个 broker 或 broker 集群之间进行通信时增加了更多的便利。在消费者方面，主要的好处是，基础设施可以自动将自动声明的队列与适当的 broker 联系起来。

这一点最好用一个例子来说明：

```java
@SpringBootApplication(exclude = RabbitAutoConfiguration.class)
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    CachingConnectionFactory cf1() {
        return new CachingConnectionFactory("localhost");
    }

    @Bean
    CachingConnectionFactory cf2() {
        return new CachingConnectionFactory("otherHost");
    }

    @Bean
    CachingConnectionFactory cf3() {
        return new CachingConnectionFactory("thirdHost");
    }

    @Bean
    SimpleRoutingConnectionFactory rcf(CachingConnectionFactory cf1,
            CachingConnectionFactory cf2, CachingConnectionFactory cf3) {

        SimpleRoutingConnectionFactory rcf = new SimpleRoutingConnectionFactory();
        rcf.setDefaultTargetConnectionFactory(cf1);
        rcf.setTargetConnectionFactories(Map.of("one", cf1, "two", cf2, "three", cf3));
        return rcf;
    }

    @Bean("factory1-admin")
    RabbitAdmin admin1(CachingConnectionFactory cf1) {
        return new RabbitAdmin(cf1);
    }

    @Bean("factory2-admin")
    RabbitAdmin admin2(CachingConnectionFactory cf2) {
        return new RabbitAdmin(cf2);
    }

    @Bean("factory3-admin")
    RabbitAdmin admin3(CachingConnectionFactory cf3) {
        return new RabbitAdmin(cf3);
    }

    @Bean
    public RabbitListenerEndpointRegistry rabbitListenerEndpointRegistry() {
        return new RabbitListenerEndpointRegistry();
    }

    @Bean
    public RabbitListenerAnnotationBeanPostProcessor postProcessor(RabbitListenerEndpointRegistry registry) {
        MultiRabbitListenerAnnotationBeanPostProcessor postProcessor
                = new MultiRabbitListenerAnnotationBeanPostProcessor();
        postProcessor.setEndpointRegistry(registry);
        postProcessor.setContainerFactoryBeanName("defaultContainerFactory");
        return postProcessor;
    }

    @Bean
    public SimpleRabbitListenerContainerFactory factory1(CachingConnectionFactory cf1) {
        SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
        factory.setConnectionFactory(cf1);
        return factory;
    }

    @Bean
    public SimpleRabbitListenerContainerFactory factory2(CachingConnectionFactory cf2) {
        SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
        factory.setConnectionFactory(cf2);
        return factory;
    }

    @Bean
    public SimpleRabbitListenerContainerFactory factory3(CachingConnectionFactory cf3) {
        SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
        factory.setConnectionFactory(cf3);
        return factory;
    }

    @Bean
    RabbitTemplate template(SimpleRoutingConnectionFactory rcf) {
        return new RabbitTemplate(rcf);
    }

    @Bean
    ConnectionFactoryContextWrapper wrapper(SimpleRoutingConnectionFactory rcf) {
        return new ConnectionFactoryContextWrapper(rcf);
    }

}

@Component
class Listeners {

    @RabbitListener(queuesToDeclare = @Queue("q1"), containerFactory = "factory1")
    public void listen1(String in) {

    }

    @RabbitListener(queuesToDeclare = @Queue("q2"), containerFactory = "factory2")
    public void listen2(String in) {

    }

    @RabbitListener(queuesToDeclare = @Queue("q3"), containerFactory = "factory3")
    public void listen3(String in) {

    }

}
```

如你所见，我们已经声明了3套基础设施（连接工厂、admin、容器工厂）。正如前面所讨论的，`@RabbitListener` 可以定义使用哪个容器工厂；在这种情况下，它们还使用 `queuesToDeclare`，如果队列不存在，它将导致队列在 broker 上被声明。通过用 `<container-factory-name>-admin` 的约定命名 `RabbitAdmin` Bean，基础设施能够确定哪个 admin 应该声明队列。这也将与 `bindings = @QueueBinding(…)` 一起工作，据此 exchange 和 binding 也将被声明。它不会与队列一起工作，因为它期望队列已经存在。

在生产者一方，提供了一个方便的 `ConnectionFactoryContextWrapper` 类，以使使用 `RoutingConnectionFactory`（见 [路由 Connection Factory](https://springdoc.cn/spring-amqp/#routing-connection-factory)）更加简单。

正如你在上面看到的，一个 `SimpleRoutingConnectionFactory` Bean 被添加到routing key `one`、`two` 和 `three`。还有一个使用该工厂的 `RabbitTemplate`。下面是一个使用该 template 与 wrapper 来路由到其中一个 broker 集群的例子。

```java
@Bean
public ApplicationRunner runner(RabbitTemplate template, ConnectionFactoryContextWrapper wrapper) {
    return args -> {
        wrapper.run("one", () -> template.convertAndSend("q1", "toCluster1"));
        wrapper.run("two", () -> template.convertAndSend("q2", "toCluster2"));
        wrapper.run("three", () -> template.convertAndSend("q3", "toCluster3"));
    };
}
```

#### 4.1.23. 调试

Spring AMQP 提供了广泛的日志记录，特别是在 `DEBUG` 级别。

如果你希望监控应用程序和 broker 之间的 AMQP 协议，你可以使用 `WireShark` 这样的工具，它有一个插件来解码该协议。另外，RabbitMQ Java 客户端附带了一个非常有用的类，称为 `Tracer`。当作为 `main` 程序运行时，在默认情况下，它监听端口 5673 并连接到 localhost 上的端口 5672。你可以运行它并改变你的连接工厂配置以连接到 localhost 的 5673 端口。它在控制台显示解码后的协议。更多信息请参考 `Tracer` Javadoc。

### 4.2. 使用 RabbitMQ Stream 插件

2.4 版引入了对 [RabbitMQ Stream 插件的 Java 客户端](https://github.com/rabbitmq/rabbitmq-stream-java-client) 的初步支持，用于 [RabbitMQ Stream 插件](https://rabbitmq.com/stream.html)。

- `RabbitStreamTemplate`
- `StreamListenerContainer`

#### 4.2.1. 发送消息

`RabbitStreamTemplate` 提供了 `RabbitTemplate`（AMQP）功能的一个子集。

Example 3. RabbitStreamOperations

```java
public interface RabbitStreamOperations extends AutoCloseable {

	ConvertableFuture<Boolean> send(Message message);

	ConvertableFuture<Boolean> convertAndSend(Object message);

	ConvertableFuture<Boolean> convertAndSend(Object message, @Nullable MessagePostProcessor mpp);

	ConvertableFuture<Boolean> send(com.rabbitmq.stream.Message message);

	MessageBuilder messageBuilder();

	MessageConverter messageConverter();

	StreamMessageConverter streamMessageConverter();

	@Override
	void close() throws AmqpException;

}
```

`RabbitStreamTemplate` 实现具有以下构造函数和属性：

Example 4. RabbitStreamTemplate

```java
public RabbitStreamTemplate(Environment environment, String streamName) {
}

public void setMessageConverter(MessageConverter messageConverter) {
}

public void setStreamConverter(StreamMessageConverter streamConverter) {
}

public synchronized void setProducerCustomizer(ProducerCustomizer producerCustomizer) {
}
```

`MessageConverter` 在 `convertAndSend` 方法中被用来将对象转换为 Spring AMQP `Message`。

`StreamMessageConverter` 用于将 Spring AMQP `Message` 转换为原生 stream `Message`。

你也可以直接发送原生 stream `Message`；用 `messageBuilder()` 方法提供对 `Producer` 的 message builder 的访问。

`ProducerCustomizer` 提供了一种机制，可以在生产者被构建之前对其进行自定义。

关于定制 `Environment` 和 `Producer`，请参考 [Java客户端文档](https://rabbitmq.github.io/rabbitmq-stream-java-client/stable/htmlsingle/)。

|      | 从 3.0 版本开始，方法的返回类型是 `CompletableFuture` 而不是 `ListenableFuture`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 4.2.2. 接收消息

异步消息接收是由 `StreamListenerContainer`（以及当使用 `@RabbitListener` 时的 `StreamRabbitListenerContainerFactory`）提供。

listener container 需要一个 `Environment` 以及一个单一的 stream name.。

你可以使用经典的 `MessageListener` 来接收 Spring AMQP `Message`，也可以使用一个新的接口来接收原生 stream `Message`：

```java
public interface StreamMessageListener extends MessageListener {

	void onStreamMessage(Message message, Context context);

}
```

有关支持的属性信息，请参见 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes) 。

类似 template，容器有一个 `ConsumerCustomizer` 属性。

关于定制 `Environment` 和 `Consumer`，请参考 [Java客户端文档](https://rabbitmq.github.io/rabbitmq-stream-java-client/stable/htmlsingle/)。

当使用 `@RabbitListener` 时，配置一个 `StreamRabbitListenerContainerFactory`；此时，大多数 `@RabbitListener` 属性（`concurrency` 等）被忽略。只有 `id`、`queues`、`autoStartup` 和 `containerFactory` 被支持。此外，`queues` 只能包含一个 stream name。

#### 4.2.3. 实例

```java
@Bean
RabbitStreamTemplate streamTemplate(Environment env) {
    RabbitStreamTemplate template = new RabbitStreamTemplate(env, "test.stream.queue1");
    template.setProducerCustomizer((name, builder) -> builder.name("test"));
    return template;
}

@Bean
RabbitListenerContainerFactory<StreamListenerContainer> rabbitListenerContainerFactory(Environment env) {
    return new StreamRabbitListenerContainerFactory(env);
}

@RabbitListener(queues = "test.stream.queue1")
void listen(String in) {
    ...
}

@Bean
RabbitListenerContainerFactory<StreamListenerContainer> nativeFactory(Environment env) {
    StreamRabbitListenerContainerFactory factory = new StreamRabbitListenerContainerFactory(env);
    factory.setNativeListener(true);
    factory.setConsumerCustomizer((id, builder) -> {
        builder.name("myConsumer")
                .offset(OffsetSpecification.first())
                .manualTrackingStrategy();
    });
    return factory;
}

@RabbitListener(id = "test", queues = "test.stream.queue2", containerFactory = "nativeFactory")
void nativeMsg(Message in, Context context) {
    ...
    context.storeOffset();
}
```

2.4.5 版本在 `StreamListenerContainer`（及其工厂）中添加了 `adviceChain` 属性。还提供了一个新的工厂 Bean 来创建一个无状态的重试拦截器，该拦截器有一个可选的 `StreamMessageRecoverer`，在消费原始 stream 消息时使用。

```java
@Bean
public StreamRetryOperationsInterceptorFactoryBean sfb(RetryTemplate retryTemplate) {
    StreamRetryOperationsInterceptorFactoryBean rfb =
            new StreamRetryOperationsInterceptorFactoryBean();
    rfb.setRetryOperations(retryTemplate);
    rfb.setStreamMessageRecoverer((msg, context, throwable) -> {
        ...
    });
    return rfb;
}
```

|      | 这个容器不支持有状态的重试。 |
| ---- | ---------------------------- |
|      |                              |

#### 4.2.4. 超级流（Super Streams）

超级流是一个分区流的抽象概念，通过将一些流队列绑定到一个具有参数 `x-super-stream: true` 的 exchange 来实现。

##### 供应

为了方便起见，超级流可以通过定义一个类型为 `SuperStream` 的 single Bean 来提供。

```java
@Bean
SuperStream superStream() {
    return new SuperStream("my.super.stream", 3);
}
```

`RabbitAdmin` 检测到这个 bean 并将声明 exchange（`my.super.stream`）和 3 个队列（分区） - `my.super-stream-n`，其中 `n` 是 `0`、`1`、`2`，与 routing keys 等于 `n` 的绑定。

如果你也希望通过 AMQP 向 exchange 发布，你可以提供自定义 routing key：

```java
@Bean
SuperStream superStream() {
    return new SuperStream("my.super.stream", 3, (q, i) -> IntStream.range(0, i)
					.mapToObj(j -> "rk-" + j)
					.collect(Collectors.toList()));
}
```

key 的数量必须等于分区的数量。

##### 向超级流生产

你必须向 `RabbitStreamTemplate` 添加一个 `superStreamRoutingFunction`：

```java
@Bean
RabbitStreamTemplate streamTemplate(Environment env) {
    RabbitStreamTemplate template = new RabbitStreamTemplate(env, "stream.queue1");
    template.setSuperStreamRouting(message -> {
        // some logic to return a String for the client's hashing algorithm
    });
    return template;
}
```

你也可以通过AMQP发布，使用 `RabbitTemplate`。

##### 用单一活动消费者消费超级流

在监听器容器上调用 `superStream` 方法，以便在一个超级流上启用一个活动的消费者。

```java
@Bean
@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
StreamListenerContainer container(Environment env, String name) {
    StreamListenerContainer container = new StreamListenerContainer(env);
    container.superStream("ss.sac", "myConsumer", 3); // concurrency = 3
    container.setupMessageListener(msg -> {
        ...
    });
    container.setConsumerCustomizer((id, builder) -> builder.offset(OffsetSpecification.last()));
    return container;
}
```

|      | 此时，当 concurrency 大于 1 时，实际的 concurrency 由 `Environment` 进一步控制；为了实现完全的 concurrency，将环境的 `maxConsumersByConnection` 设置为 `1`。 参见 [配置环境](https://rabbitmq.github.io/rabbitmq-stream-java-client/snapshot/htmlsingle/#configuring-the-environment)。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

### 4.3. 日志子系统 AMQP Appender

该框架为一些流行的日志子系统提供了 logging appender：

- logback (自Spring AMQP 1.4版本起)
- log4j2 (自Spring AMQP 1.6版本起)

appender 是通过使用日志子系统的正常机制来配置的，可用的属性在下面几节中说明。

#### 4.3.1. 常见的属性

以下属性对所有 appender 都是可用的：

| 属性                          | 默认                                      | 说明                                                         |
| :---------------------------- | :---------------------------------------- | :----------------------------------------------------------- |
| ` exchangeName`               | ` logs`                                   | 要发布日志事件的 exchange 的名称。                           |
| ` exchangeType`               | ` topic`                                  | 发布日志事件的 exchange 的类型 - 只有在 appender 声明了 exchange 的情况下才需要。参见 `declareExchange`。 |
| ` routingKeyPattern`          | ` %c.%p`                                  | Logging 子系统的 pattern format，用于生成 routing key。      |
| ` applicationId`              | ``                                        | Application ID — 应用ID - 如果 pattern 包括 `%X{applicationId}`，则添加到 routing key 中。 |
| ` senderPoolSize`             | ` 2`                                      | 用来发布日志事件的线程数。                                   |
| ` maxSenderRetries`           | ` 30`                                     | 如果 broker 不可用或有其他错误，要重试多少次发送消息。重试的延迟时间如下：`N ^ log(N)`，其中 `N` 是重试次数。 |
| ` addresses`                  | ``                                        | 以逗号分隔的 broker 地址列表，格式如下：`host:port[,host:port]*` - 覆盖 `host` 和 `port`。 |
| ` host`                       | ` localhost`                              | 连接到的RabbitMQ主机。                                       |
| ` port`                       | ` 5672`                                   | 连接到的 RabbitMQ 端口。                                     |
| ` virtualHost`                | ` /`                                      | 连接到的 RabbitMQ virtual host。                             |
| ` username`                   | ` guest`                                  | 连接时使用的 RabbitMQ user。                                 |
| ` password`                   | ` guest`                                  | 该 user 的 RabbitMQ password 。                              |
| ` useSsl`                     | ` false`                                  | 是否为 RabbitMQ 连接使用 SSL。请参见 [`RabbitConnectionFactoryBean` 以及配置 SSL](https://springdoc.cn/spring-amqp/#rabbitconnectionfactorybean-configuring-ssl)。 |
| ` verifyHostname`             | ` true`                                   | 为TLS连接启用服务器主机名验证。请参见 [`RabbitConnectionFactoryBean` 以及配置 SSL](https://springdoc.cn/spring-amqp/#rabbitconnectionfactorybean-configuring-ssl)。 |
| ` sslAlgorithm`               | ` null`                                   | 使用的SSL算法。                                              |
| ` sslPropertiesLocation`      | ` null`                                   | SSL properties 文件的位置。                                  |
| ` keyStore`                   | ` null`                                   | keystore 的位置。                                            |
| ` keyStorePassphrase`         | ` null`                                   | keystore 的密码口令。                                        |
| ` keyStoreType`               | ` JKS`                                    | keystore 类型。                                              |
| ` trustStore`                 | ` null`                                   | Truststore 的位置。                                          |
| ` trustStorePassphrase`       | ` null`                                   | Truststore 的密码。                                          |
| ` trustStoreType`             | ` JKS`                                    | truststore 的类型。                                          |
| ` saslConfig`                 | ` null (RabbitMQ client default applies)` | `saslConfig` - 有关有效值，请参阅 `RabbitUtils.stringToSaslConfig` 的 javadoc。 |
| ` contentType`                | ` text/plain`                             | 日志消息的 `content-type` 属性。                             |
| ` contentEncoding`            | ``                                        | 日志消息的 `content-encoding` 属性。                         |
| ` declareExchange`            | ` false`                                  | 是否在这个 appender 启动时声明配置的 exchange。另请参见 `durable` 和 `autoDelete`。 |
| ` durable`                    | ` true`                                   | 当 `declareExchange` 为 `true` 时，durable 标志被设置为这个值。 |
| ` autoDelete`                 | ` false`                                  | 当 `declareExchange` 为 `true` 时，auto-delete 标志被设置为这个值。 |
| ` charset`                    | ` null`                                   | 将 `String` 转换为 `byte[]` 时使用的字符集。默认：null（使用系统默认字符集）。如果当前平台不支持该字符集，我们将退回到使用系统字符集。 |
| ` deliveryMode`               | ` PERSISTENT`                             | `PERSISTENT` 或 `NON_PERSISTENT`，以确定 RabbitMQ 是否应该持久化消息。 |
| ` generateId`                 | ` false`                                  | 用于确定 `messageId` 属性是否被设置为唯一值。                |
| ` clientConnectionProperties` | ` null`                                   | 以逗号分隔的 `key:value` 列表，用于向 RabbitMQ 连接提供自定义客户端属性。 |
| ` addMdcAsHeaders`            | ` true`                                   | 在引入该属性之前，MDC 属性总是被添加到 RabbitMQ 消息 header 中。这可能会导致大型 MDC 的问题，因为 RabbitMQ 对所有 header 都有有限的缓冲区大小，而且这个缓冲区相当小。引入此属性是为了避免在大型 MDC 的情况下出现问题。默认情况下，为了向后兼容，此值设置为 `true`。`false` 则是关闭序列化MDC到 header 中。请注意，`JsonLayout` 默认将MDC添加到消息中。 |

#### 4.3.2. Log4j 2 Appender

下面的例子显示了如何配置一个 Log4j 2 appender：

```xml
<Appenders>
    ...
    <RabbitMQ name="rabbitmq"
        addresses="foo:5672,bar:5672" user="guest" password="guest" virtualHost="/"
        exchange="log4j2" exchangeType="topic" declareExchange="true" durable="true" autoDelete="false"
        applicationId="myAppId" routingKeyPattern="%X{applicationId}.%c.%p"
        contentType="text/plain" contentEncoding="UTF-8" generateId="true" deliveryMode="NON_PERSISTENT"
        charset="UTF-8"
        senderPoolSize="3" maxSenderRetries="5"
        addMdcAsHeaders="false">
    </RabbitMQ>
</Appenders>
```

|      | 从 1.6.10 和 1.7.3 版本开始，默认情况下，log4j2 附加程序将消息发布到调用线程上的 RabbitMQ。这是因为 Log4j 2 默认不创建线程安全的事件。如果 broker 停机，则使用 `maxSenderRetries` 来重试，重试之间没有延迟。如果你希望恢复以前的行为，在不同的线程上发布消息（`senderPoolSize`），你可以将 `async` 属性设置为 `true`。然而，你还需要配置Log4j 2以使用 `DefaultLogEventFactory` 而不是 `ReusableLogEventFactory`。一种方法是设置系统属性 `-Dlog4j2.enable.threadlocals=false`。如果你使用 `ReusableLogEventFactory` 的异步发布，事件很有可能因为交叉对话而被破坏。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 4.3.3. Logback Appender

下面的例子显示了如何配置一个 logback appender：

```xml
<appender name="AMQP" class="org.springframework.amqp.rabbit.logback.AmqpAppender">
    <layout>
        <pattern><![CDATA[ %d %p %t [%c] - <%m>%n ]]></pattern>
    </layout>
    <addresses>foo:5672,bar:5672</addresses>
    <abbreviation>36</abbreviation>
    <includeCallerData>false</includeCallerData>
    <applicationId>myApplication</applicationId>
    <routingKeyPattern>%property{applicationId}.%c.%p</routingKeyPattern>
    <generateId>true</generateId>
    <charset>UTF-8</charset>
    <durable>false</durable>
    <deliveryMode>NON_PERSISTENT</deliveryMode>
    <declareExchange>true</declareExchange>
    <addMdcAsHeaders>false</addMdcAsHeaders>
</appender>
```

从1.7.1版本开始，Logback `AmqpAppender` 提供了一个 `includeCallerData` 选项，默认为 `false`。提取调用者数据可能相当昂贵，因为日志事件必须创建一个 throwable 并检查它以确定调用位置。因此，默认情况下，当事件被添加到事件队列中时，与事件相关的调用者数据不会被提取。你可以通过设置 `includeCallerData` 属性为 `true` 来配置 appender，使其包括调用者数据。

从 2.0.0 版本开始，Logback `AmqpAppender` 支持带有 `encoder` 选项的 [Logback encoders](https://logback.qos.ch/manual/encoders.html)。`encoder` 和 `layout` 选项是相互排斥的。

#### 4.3.4. 自定义信息

默认情况下，AMQP appender 会填充以下 message properties:

- `deliveryMode`
- contentType
- `contentEncoding`，如果配置了的话
- `messageId`，如果 `generateId` 配置了的话
- 日志事件的 `timestamp`
- `appId`，如果配置了applicationId

此外，他们还用以下值填充 header：

- 日志事件的 `categoryName`
- 日志事件的级别（level ）
- `thread`：发生日志事件的线程名称。
- 日志事件调用的 stack trace 的位置
- 所有 MDC properties 的副本（除非 `addMdcAsHeaders` 被设置为 `false`）。

每个 appender 都可以被子类化，让你在发布前修改信息。下面的例子显示了如何定制日志信息：

```java
public class MyEnhancedAppender extends AmqpAppender {

    @Override
    public Message postProcessMessageBeforeSend(Message message, Event event) {
        message.getMessageProperties().setHeader("foo", "bar");
        return message;
    }

}
```

从 2.2.4 开始，log4j2的 `AmqpAppender` 可以使用 `@PluginBuilderFactory` 进行扩展，同时扩展 `AmqpAppender.Builder`。

```java
@Plugin(name = "MyEnhancedAppender", category = "Core", elementType = "appender", printObject = true)
public class MyEnhancedAppender extends AmqpAppender {

	public MyEnhancedAppender(String name, Filter filter, Layout<? extends Serializable> layout,
			boolean ignoreExceptions, AmqpManager manager, BlockingQueue<Event> eventQueue, String foo, String bar) {
		super(name, filter, layout, ignoreExceptions, manager, eventQueue);

	@Override
	public Message postProcessMessageBeforeSend(Message message, Event event) {
			message.getMessageProperties().setHeader("foo", "bar");
		return message;
	}

	@PluginBuilderFactory
	public static Builder newBuilder() {
		return new Builder();
	}

	protected static class Builder extends AmqpAppender.Builder {

		@Override
		protected AmqpAppender buildInstance(String name, Filter filter, Layout<? extends Serializable> layout,
				boolean ignoreExceptions, AmqpManager manager, BlockingQueue<Event> eventQueue) {
			return new MyEnhancedAppender(name, filter, layout, ignoreExceptions, manager, eventQueue);
		}
	}

}
```

#### 4.3.5. 自定义 Client Properties

你可以通过添加字符串属性或更复杂的属性来添加自定义 client properties 。

##### 简单的字符串属性

每个 appender 都支持向 RabbitMQ 连接添加客户端属性。

下面的示例显示了如何为 logback 添加一个自定义客户端属性：

```xml
<appender name="AMQP" ...>
    ...
    <clientConnectionProperties>thing1:thing2,cat:hat</clientConnectionProperties>
    ...
</appender>
```

Example 5. log4j2

```xml
<Appenders>
    ...
    <RabbitMQ name="rabbitmq"
        ...
        clientConnectionProperties="thing1:thing2,cat:hat"
        ...
    </RabbitMQ>
</Appenders>
```

这些属性是一个以逗号分隔的 `key:value` 对的列表。key 和 value 不能包含逗号或冒号。

当查看连接时，这些属性会出现在 RabbitMQ 管理用户界面上。

##### Logback 的高级技术

你可以对 Logback appender 进行子类化。这样做可以让你在连接建立之前修改客户端的连接属性。下面的例子展示了如何做到这一点：

```java
public class MyEnhancedAppender extends AmqpAppender {

    private String thing1;

    @Override
    protected void updateConnectionClientProperties(Map<String, Object> clientProperties) {
        clientProperties.put("thing1", this.thing1);
    }

    public void setThing1(String thing1) {
        this.thing1 = thing1;
    }

}
```

然后你可以在 logback.xml 中添加 `<thing1>thing2</thing1>`。

对于像前面的例子中所示的字符串属性，可以使用前面的技术。子类允许添加更丰富的属性（比如添加一 `Map` 或数字属性）。

#### 4.3.6. 提供一个自定义队列实现

`AmqpAppenders` 使用一个 `BlockingQueue` 来异步发布日志事件到 RabbitMQ。默认情况下，使用一个 `LinkedBlockingQueue`。但是，你可以提供任何类型的自定义 `BlockingQueue` 实现。

下面的例子显示了如何对 Logback 进行操作：

```java
public class MyEnhancedAppender extends AmqpAppender {

    @Override
    protected BlockingQueue<Event> createEventQueue() {
        return new ArrayBlockingQueue();
    }

}
```

Log4j 2 appender 支持使用 [`BlockingQueueFactory`](https://logging.apache.org/log4j/2.x/manual/appenders.html#BlockingQueueFactory)，如下例所示：

```xml
<Appenders>
    ...
    <RabbitMQ name="rabbitmq"
              bufferSize="10" ... >
        <ArrayBlockingQueue/>
    </RabbitMQ>
</Appenders>
```

### 4.4. 示例应用

[Spring AMQP Samples](https://github.com/SpringSource/spring-amqp-samples) 项目包括两个示例应用程序。第一个是一个简单的 “Hello World” 例子，演示了同步和异步的消息接收。它为获得对基本组件的理解提供了一个很好的起点。第二个例子是基于一个股票交易的用例，演示了现实世界应用中常见的交互类型。在本章中，我们将对每个样本进行快速浏览，以便你能专注于最重要的组件。这些样本都是基于 Maven 的，所以你应该能够将它们直接导入任何支持Maven的IDE（如 [SpringSource Tool Suite](https://www.springsource.org/sts)）。

#### 4.4.1. “Hello World” 示例

“Hello World” 示例演示了同步和异步的消息接收。你可以将 `spring-rabbit-helloworld` 样本导入IDE，然后按照下面的讨论进行。

##### 同步示例

在 `src/main/java` 目录中，导航到 `org.springframework.amqp.helloworld` 包。打开 `HelloWorldConfiguration` 类，注意到它在类的层面上包含了 `@Configuration` 注解，并注意到在方法层面上有一些 `@Bean` 注解。这是Spring基于Java的配置的一个例子。你可以在 [这里](https://docs.spring.io/spring/docs/current/spring-framework-reference/html/beans.html#beans-java) 阅读更多关于这个的内容。

下面的列表显示了连接工厂是如何创建的：

```java
@Bean
public CachingConnectionFactory connectionFactory() {
    CachingConnectionFactory connectionFactory =
        new CachingConnectionFactory("localhost");
    connectionFactory.setUsername("guest");
    connectionFactory.setPassword("guest");
    return connectionFactory;
}
```

该配置还包含 `RabbitAdmin` 的一个实例，默认情况下，它寻找任何 exchange、queue 或 binding 类型的 Bean，然后在 broker 上声明它们。事实上，在 `HelloWorldConfiguration` 中生成的 `helloWorldQueue` Bean 就是一个例子，因为它是 `Queue` 的一个实例。

下面的列表显示了 `helloWorldQueue` Bean的定义：

```java
@Bean
public Queue helloWorldQueue() {
    return new Queue(this.helloWorldQueueName);
}
```

回头看看 `rabbitTemplate` Bean的配置，你可以看到它将 `helloWorldQueue` 的名字设置为其 `queue` 属性（用于接收消息）和 `routingKey` 属性（用于发送消息）。

现在我们已经探索了配置，我们可以看一下实际使用这些组件的代码。首先，从同一个包中打开 `Producer` 类。它包含一个 `main()` 方法，Spring `ApplicationContext` 在此被创建。

下面的列表显示了 `main` 方法：

```java
public static void main(String[] args) {
    ApplicationContext context =
        new AnnotationConfigApplicationContext(RabbitConfiguration.class);
    AmqpTemplate amqpTemplate = context.getBean(AmqpTemplate.class);
    amqpTemplate.convertAndSend("Hello World");
    System.out.println("Sent: Hello World");
}
```

在前面的例子中，`AmqpTemplate` Bean 被检索并用于发送一个消息。由于客户机代码应该尽可能地依赖接口，所以类型是 `AmqpTemplate` 而不是 `RabbitTemplate`。尽管在 `HelloWorldConfiguration` 中创建的 bean 是 `RabbitTemplate` 的一个实例，但对接口的依赖意味着此代码更具可移植性（您可以独立于代码来更改配置）。由于 `convertAndSend()` 方法被调用，template 委托给它的 `MessageConverter` 实例。在这种情况下，它使用默认的 `SimpleMessageConverter`，但可以向 `rabbitTemplate` bean 提供一个不同的实现，如 `HelloWorldConfiguration` 中定义的那样。

现在打开 `Consumer` 类。它实际上共享相同的配置基类，这意味着它共享 `rabbitTemplate` Bean。这就是为什么我们用 `routingKey`（用于发送）和 `queue`（用于接收）来配置那个 template。正如我们在 [`AmqpTemplate`](https://springdoc.cn/spring-amqp/#amqp-template) 中所描述的，你可以把 'routingKey' 参数传递给 send 方法，把 'queue' 参数传递给 receive 方法。`Consumer` 的代码基本上是生产者的镜像，调用 `receiveAndConvert()` 而不是 `convertAndSend()`。

下面列出了 `Consumer` 的 main 方法：

```java
public static void main(String[] args) {
    ApplicationContext context =
        new AnnotationConfigApplicationContext(RabbitConfiguration.class);
    AmqpTemplate amqpTemplate = context.getBean(AmqpTemplate.class);
    System.out.println("Received: " + amqpTemplate.receiveAndConvert());
}
```

如果你运行 `Producer`，然后运行 `Consumer`，你应该看到 `Received: Hello World` 出现在控制台输出中。

##### 异步示例

[同步示例](https://springdoc.cn/spring-amqp/#hello-world-sync) 走了一遍同步 Hello World 的例子。本节介绍了一个稍微高级但明显更强大的选项。经过一些修改，Hello World 示例可以提供一个异步接收的例子，也被称为消息驱动的POJO。事实上，有一个子包正是提供了这个功能：`org.springframework.amqp.samples.helloworld.async`。

同样，我们从发送端开始。打开 `ProducerConfiguration` 类，注意它创建了一个 `connectionFactory` 和一个 `rabbitTemplate` Bean。这一次，由于配置是专门用于消息发送端，我们甚至不需要任何队列定义，而且 `RabbitTemplate` 只设置了 'routingKey' 属性。回顾一下，消息被发送到一个 exchange，而不是直接发送到一个队列。AMQP 默认 exchange 是一个没有名称的 direct exchange。所有的队列都被绑定到那个默认的 exchange，并以它们的名字作为 routing key。这就是为什么我们只需要在这里提供 routing key。

下面的列表显示了 `rabbitTemplate` 的定义：

```java
public RabbitTemplate rabbitTemplate() {
    RabbitTemplate template = new RabbitTemplate(connectionFactory());
    template.setRoutingKey(this.helloWorldQueueName);
    return template;
}
```

由于这个例子演示了异步消息接收，生产方被设计为持续发送消息（如果它是一个像同步版本那样的每执行一次的消息模型，就不会那么明显，事实上，它是一个消息驱动的消费者）。负责持续发送消息的组件被定义为 `ProducerConfiguration` 中的一个内部类。它被配置为每三秒运行一次。

下面列出了该组件：

```java
static class ScheduledProducer {

    @Autowired
    private volatile RabbitTemplate rabbitTemplate;

    private final AtomicInteger counter = new AtomicInteger();

    @Scheduled(fixedRate = 3000)
    public void sendMessage() {
        rabbitTemplate.convertAndSend("Hello World " + counter.incrementAndGet());
    }
}
```

你不需要了解所有的细节，因为真正的重点应该是在接收方面（我们接下来会介绍）。然而，如果你还不熟悉 Spring 任务调度支持，你可以在 [这里](https://docs.spring.io/spring/docs/current/spring-framework-reference/html/scheduling.html#scheduling-annotation-support) 了解更多。简而言之， `ProducerConfiguration` 中的 `postProcessor` Bean 将任务注册到调度器中。

现在我们可以转向接收方了。为了强调消息驱动的POJO行为，我们从对消息做出反应的组件开始。这个类被称为 `HelloWorldHandler`，在下面的列表中显示：

```java
public class HelloWorldHandler {

    public void handleMessage(String text) {
        System.out.println("Received: " + text);
    }

}
```

该类是一个 POJO。它没有扩展任何基类，没有实现任何接口，甚至没有包含任何 import。它被Spring AMQP `MessageListenerAdapter` "改编" 为 `MessageListener` 接口。然后你可以在 `SimpleMessageListenerContainer` 上配置该适配器。对于这个例子，容器是在 `ConsumerConfiguration` 类中创建的。你可以看到 POJO 被包裹在适配器中。

下面的列表显示了 `listenerContainer` 是如何定义的：

```java
@Bean
public SimpleMessageListenerContainer listenerContainer() {
    SimpleMessageListenerContainer container = new SimpleMessageListenerContainer();
    container.setConnectionFactory(connectionFactory());
    container.setQueueName(this.helloWorldQueueName);
    container.setMessageListener(new MessageListenerAdapter(new HelloWorldHandler()));
    return container;
}
```

`SimpleMessageListenerContainer` 是一个 Spring 生命周期组件，默认情况下会自动启动。如果你看一下 `Consumer` 类，你可以看到它的 `main()` 方法只不过是由一个创建 `ApplicationContext` 的单行 bootstrap 组成。生产者的 `main()` 方法也是一个单行 bootstrap，因为其方法被 `@Scheduled` 注解的组件也会自动启动。你可以以任何顺序启动 `Producer` 和 `Consumer`，你应该看到每三秒就有消息被发送和接收。

#### 4.4.2. 股票交易

股票交易样本展示了比 [Hello World 样本](https://springdoc.cn/spring-amqp/#hello-world-sample) 更高级的消息传递场景。然而，配置是非常相似的，只是涉及的内容更多一点。由于我们详细介绍了Hello World的配置，在这里，我们着重介绍这个例子的不同之处。有一个服务器将市场数据（股票报价）推送到一个 topic exchange。然后，客户可以通过绑定一个带有 routing pattern（例如，`app.stock.quotes.nasdaq.*`）的队列来订阅市场数据。这个演示的另一个主要特征是 request-reply "股票交易" 互动，由客户端发起，由服务器处理。这涉及到一个私有的 `replyTo` 队列，由客户在 order request 消息本身中发送。

服务器的核心配置在 `org.springframework.amqp.rabbit.stocks.config.server` 包中的 `RabbitServerConfiguration` 类中。它继承了 `AbstractStockAppRabbitConfiguration`。这就是定义服务器和客户端通用资源的地方，包括市场数据 topic exchange（其名称为 'app.stock.marketdata'）和服务器为股票交易提供的队列（其名称为 'app.stock.request'）。在该通用配置文件中，你还看到在 `RabbitTemplate` 上配置了一个 `Jackson2JsonMessageConverter`。

服务器特定的配置包括两件事。首先，它配置 `RabbitTemplate` 上的市场数据 exchange，这样它就不需要在每次调用发送 `Message` 时提供该 exchange 名称。它在基配置类中定义的一个抽象回调方法中完成此工作。下面的列表显示了该方法。

```java
public void configureRabbitTemplate(RabbitTemplate rabbitTemplate) {
    rabbitTemplate.setExchange(MARKET_DATA_EXCHANGE_NAME);
}
```

第二，声明股票请求队列（stock request queue）。在这种情况下，它不需要任何显式绑定，因为它被绑定到默认的无名 exchange，用自己的名字作为 routing key。如前所述，AMQP规范定义了这种行为。下面的列表显示了 `stockRequestQueue` Bean的定义：

```java
@Bean
public Queue stockRequestQueue() {
    return new Queue(STOCK_REQUEST_QUEUE_NAME);
}
```

现在你已经看到了服务器的AMQP资源的配置，导航到 `src/test/java` 目录下的 `org.springframework.amqp.rabbit.stock` 包。在那里，你可以看到实际的 `Server` 类，它提供了一个 `main()` 方法。它根据 `server-bootstrap.xml` 配置文件创建一个 `ApplicationContext`。在那里，你可以看到发布虚假市场数据的计划任务。该配置依赖于Spring的 `task` 命名空间支持。bootstrap 配置文件还导入了其他一些文件。最有趣的是 `server-messaging.xml`，它直接位于 `src/main/resources` 下。在那里，你可以看到负责处理股票交易请求的 `messageListenerContainer` bean。最后，看看 `server-handlers.xml` 中定义的 `serverHandler` Bean（也在 'src/main/resources' 中）。该 Bean 是 `ServerHandler` 类的一个实例，是一个消息驱动的POJO的好例子，它也可以发送回复消息。请注意，它本身并没有与框架或任何AMQP概念相耦合。它接受一个 `TradeRequest` 并返回一个 `TradeResponse`。下面的列表显示了 `handleMessage` 方法的定义：

```java
public TradeResponse handleMessage(TradeRequest tradeRequest) { ...
}
```

现在我们已经看到了服务器的最重要的配置和代码，我们可以转向客户端了。最好的起点可能是 `org.springframework.amqp.rabbit.stocks.config.client` 包中的 `RabbitClientConfiguration`。请注意，它声明了两个队列，但没有提供明确的名称。下面的列表显示了这两个队列的 bean 定义：

```java
@Bean
public Queue marketDataQueue() {
    return amqpAdmin().declareQueue();
}

@Bean
public Queue traderJoeQueue() {
    return amqpAdmin().declareQueue();
}
```

这些都是私人队列，唯一的名字是自动生成的。第一个生成的队列被客户端用来绑定到服务器所暴露的市场数据 exchange。回顾一下，在AMQP中，消费者与队列互动，而生产者与 exchange 互动。队列与交易所的 “binding” 是告诉 broker 从一个给定的 exchange 向队列传递（或 route）消息。由于市场数据 exchange 是一个 topic exchange，绑定可以用 routing pattern 表示。`RabbitClientConfiguration` 用一个 `Binding` 对象来做，该对象是用 `BindingBuilder` fluent API 生成的。下面的列表显示了 `Binding`：

```java
@Value("${stocks.quote.pattern}")
private String marketDataRoutingKey;

@Bean
public Binding marketDataBinding() {
    return BindingBuilder.bind(
        marketDataQueue()).to(marketDataExchange()).with(marketDataRoutingKey);
}
```

请注意，实际值已经外化在一个属性文件中（`src/main/resources` 下的 `client.properties`），而且我们使用 Spring 的 `@Value` 注解来注入该值。这通常是个好主意。否则，这个值就会被硬编码在一个类中，不重新编译就无法修改。在这种情况下，运行多个版本的客户端，同时对用于绑定的 routing pattern 进行修改，就会容易得多。我们现在可以试试。

首先运行 `org.springframework.amqp.rabbit.stocks.Server`，然后运行 `org.springframework.amqp.rabbit.stocks.Client`。你应该看到纳斯达克股票的虚拟报价，因为当前与 client.properties 中的 'stocks.quote.pattern' key 相关的值是 `app.stock.quotes.nasdaq.*`。现在，在保持现有 `Server` 和 `Client` 运行的情况下，将该属性值改为 `app.stock.quotes.nyse.*` 并启动第二个 `Client` 实例。你应该看到，第一个 `Client` 仍然接收纳斯达克的报价，而第二个 `Client` 则接收纽约证券交易所的报价。你可以改变 pattern 来获得所有的股票，甚至是单个的股票。

我们探讨的最后一个功能是从客户的角度来探讨 request-reply 的互动。回顾一下，我们已经看到了接受 `TradeRequest` 对象并返回 `TradeResponse` 对象的 `ServerHandler`。客户端上的相应代码是 `org.springframework.amqp.rabbit.stocks.gateway` 包中的 `RabbitStockServiceGateway`。它委托给 `RabbitTemplate`，以便发送消息。下面的列表显示了 `send` 方法：

```java
public void send(TradeRequest tradeRequest) {
    getRabbitTemplate().convertAndSend(tradeRequest, new MessagePostProcessor() {
        public Message postProcessMessage(Message message) throws AmqpException {
            message.getMessageProperties().setReplyTo(new Address(defaultReplyToQueue));
            try {
                message.getMessageProperties().setCorrelationId(
                    UUID.randomUUID().toString().getBytes("UTF-8"));
            }
            catch (UnsupportedEncodingException e) {
                throw new AmqpException(e);
            }
            return message;
        }
    });
}
```

注意，在发送消息之前，它设置了 `replyTo` 地址。它提供了由 `traderJoeQueue` Bean定义生成的队列（如前所示）。下面的列表显示了 `StockServiceGateway` 类本身的 `@Bean` 定义：

```java
@Bean
public StockServiceGateway stockServiceGateway() {
    RabbitStockServiceGateway gateway = new RabbitStockServiceGateway();
    gateway.setRabbitTemplate(rabbitTemplate());
    gateway.setDefaultReplyToQueue(traderJoeQueue());
    return gateway;
}
```

如果你不再运行服务器和客户端，现在就启动它们。尝试发送一个格式为 "100 TCKR" 的请求。在模拟 "处理" 请求的短暂人工延迟之后，你应该看到客户端上出现一条确认信息。

#### 4.4.3. 从非Spring应用程序接收JSON

Spring应用程序在发送JSON时，将 `*TypeId*` header 设置为全路径的类名，以协助接收应用程序将JSON转换为Java对象。

`spring-rabbit-json` 示例探讨了几种技术，以转换来自非Spring应用程序的JSON。

也请参见 [Jackson2JsonMessageConverter](https://springdoc.cn/spring-amqp/#json-message-converter) 以及 [DefaultClassMapper 的Javadoc](https://docs.spring.io/spring-amqp/docs/current/api/index.html?org/springframework/amqp/support/converter/DefaultClassMapper.html)。

### 4.5. 测试支持

为异步应用程序编写集成必然比测试简单的应用程序更复杂。当 `@RabbitListener` 注解这样的抽象概念出现时，情况就更复杂了。问题是如何验证在发送消息后，监听器是否按预期收到了该消息。

该框架本身有许多单元和集成测试。有些使用模拟（mocks），而有些则使用与实时 RabbitMQ broker 的集成测试。你可以参考这些测试，了解一些测试场景的想法。

Spring AMQP 1.6 版本引入了 `spring-rabbit-test` jar，它为测试一些更复杂的场景提供了支持。预计这个项目会随着时间的推移而扩大，但我们需要社区的反馈，为帮助测试所需的功能提出建议。请使用 [JIRA](https://jira.spring.io/browse/AMQP) 或 [GitHub Issues](https://github.com/spring-projects/spring-amqp/issues) 来提供这种反馈。

#### 4.5.1. @SpringRabbitTest

使用此注解可将基础设施 Bean 添加到 Spring 测试 `ApplicationContext` 中。当使用 `@SpringBootTest` 时，这就没有必要了，因为 Spring Boot 的自动配置会添加这些Bean。

已注册的 bean 有：

- `CachingConnectionFactory` (`autoConnectionFactory`)。 如果 `@RabbitEnabled` 存在，它的连接工厂被使用。
- `RabbitTemplate` (`autoRabbitTemplate`)
- `RabbitAdmin` (`autoRabbitAdmin`)
- `RabbitListenerContainerFactory` (`autoContainerFactory`)

此外，与 `@EnableRabbit` 相关的bean（支持 `@RabbitListener`）也被添加。

Example 6. Junit5 示例

```java
@SpringJunitConfig
@SpringRabbitTest
public class MyRabbitTests {

	@Autowired
	private RabbitTemplate template;

	@Autowired
	private RabbitAdmin admin;

	@Autowired
	private RabbitListenerEndpointRegistry registry;

	@Test
	void test() {
        ...
	}

	@Configuration
	public static class Config {

        ...

	}

}
```

在JUnit4中，用 `@RunWith(SpringRunnner.class)` 替换 `@SpringJunitConfig`。

#### 4.5.2. Mockito `Answer<?>` 实现

目前有两个 `Answer<?>` 的实现来帮助测试。

第一个，`LatchCountDownAndCallRealMethodAnswer`，提供了一个返回 `null` 的 `Answer<Void>`，并倒数一个 latch。下面的例子展示了如何使用 `LatchCountDownAndCallRealMethodAnswer`：

```java
LatchCountDownAndCallRealMethodAnswer answer = this.harness.getLatchAnswerFor("myListener", 2);
doAnswer(answer)
    .when(listener).foo(anyString(), anyString());

...

assertThat(answer.await(10)).isTrue();
```

第二个，`LambdaAnswer<T>` 提供了一种机制，可以选择性地调用真正的方法，并提供了一个机会，根据 `InvocationOnMock` 和结果（如果有的话）返回一个自定义的结果。

考虑以下POJO：

```java
public class Thing {

    public String thing(String thing) {
        return thing.toUpperCase();
    }

}
```

以下是测试 `Thing` POJO 的类：

```java
Thing thing = spy(new Thing());

doAnswer(new LambdaAnswer<String>(true, (i, r) -> r + r))
    .when(thing).thing(anyString());
assertEquals("THINGTHING", thing.thing("thing"));

doAnswer(new LambdaAnswer<String>(true, (i, r) -> r + i.getArguments()[0]))
    .when(thing).thing(anyString());
assertEquals("THINGthing", thing.thing("thing"));

doAnswer(new LambdaAnswer<String>(false, (i, r) ->
    "" + i.getArguments()[0] + i.getArguments()[0])).when(thing).thing(anyString());
assertEquals("thingthing", thing.thing("thing"));
```

从 2.2.3 版本开始，answer 可以捕获被测试方法抛出的任何异常。使用 `answer.getExceptions()` 来获取它们的引用。

当与 [`@RabbitListenerTest` 和 `RabbitListenerTestHarness`](https://springdoc.cn/spring-amqp/#test-harness) 结合使用时，使用 `harness.getLambdaAnswerFor("listenerId", true, …)` 来为监听器获得一个正确构建的 answer。

#### 4.5.3. `@RabbitListenerTest` 和 `RabbitListenerTestHarness`

用 `@RabbitListenerTest` 注解你的 `@Configuration` 类之一会导致框架用一个名为 `RabbitListenerTestHarness` 的子类替换标准 `RabbitListenerAnnotationBeanPostProcessor`（它还通过 `@EnableRabbit` 启用 `@RabbitListener` 检测）。

`RabbitListenerTestHarness` 以两种方式增强了监听器。首先，它将监听器包裹在一个 `Mockito Spy` 中，以实现正常的 `Mockito` stub 和验证操作。它还可以向监听器添加一个 `Advice`，使其能够访问参数、结果和任何抛出的异常。你可以通过 `@RabbitListenerTest` 的属性来控制其中的哪一个（或两个）被启用。后者是为了访问关于调用的低级数据而提供的。它还支持阻塞测试线程，直到异步监听器被调用。

|      | `final` `@RabbitListener` 方法不能 spied 视或 advised。另外，只有带有 `id` 属性的监听器可以被 spied 或 advised。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

请考虑一些例子。

下面的例子使用了spy：

```java
@Configuration
@RabbitListenerTest
public class Config {

    @Bean
    public Listener listener() {
        return new Listener();
    }

    ...

}

public class Listener {

    @RabbitListener(id="foo", queues="#{queue1.name}")
    public String foo(String foo) {
        return foo.toUpperCase();
    }

    @RabbitListener(id="bar", queues="#{queue2.name}")
    public void foo(@Payload String foo, @Header("amqp_receivedRoutingKey") String rk) {
        ...
    }

}

public class MyTests {

    @Autowired
    private RabbitListenerTestHarness harness; 

    @Test
    public void testTwoWay() throws Exception {
        assertEquals("FOO", this.rabbitTemplate.convertSendAndReceive(this.queue1.getName(), "foo"));

        Listener listener = this.harness.getSpy("foo"); 
        assertNotNull(listener);
        verify(listener).foo("foo");
    }

    @Test
    public void testOneWay() throws Exception {
        Listener listener = this.harness.getSpy("bar");
        assertNotNull(listener);

        LatchCountDownAndCallRealMethodAnswer answer = this.harness.getLatchAnswerFor("bar", 2); 
        doAnswer(answer).when(listener).foo(anyString(), anyString()); 

        this.rabbitTemplate.convertAndSend(this.queue2.getName(), "bar");
        this.rabbitTemplate.convertAndSend(this.queue2.getName(), "baz");

        assertTrue(answer.await(10));
        verify(listener).foo("bar", this.queue2.getName());
        verify(listener).foo("baz", this.queue2.getName());
    }

}
```

|      | 将 harness 注入测试用例中，这样我们就可以获得对 spy 的访问。 |
| ---- | ------------------------------------------------------------ |
|      | 获取对 spy 的引用，以便我们可以验证它是否如预期那样被调用。由于这是一个发送和接收操作，所以不需要暂停测试线程，因为它已经在 `RabbitTemplate` 中暂停，等待回复。 |
|      | 在这种情况下，我们只使用一个发送操作，所以我们需要一个 latch 来等待对容器线程上的监听器的异步调用。我们使用 [Answer](https://springdoc.cn/spring-amqp/#mockito-answer) 的一个实现来帮助解决这个问题。重要的是：由于监听器被监视的方式，使用 `harness.getLatchAnswerFor()` 来为 spy 获得一个正确配置的 answer 是很重要的。 |
|      | 配置spy来调用 `Answer`。                                     |

下面的例子使用了捕获 advice：

```java
@Configuration
@ComponentScan
@RabbitListenerTest(spy = false, capture = true)
public class Config {

}

@Service
public class Listener {

    private boolean failed;

    @RabbitListener(id="foo", queues="#{queue1.name}")
    public String foo(String foo) {
        return foo.toUpperCase();
    }

    @RabbitListener(id="bar", queues="#{queue2.name}")
    public void foo(@Payload String foo, @Header("amqp_receivedRoutingKey") String rk) {
        if (!failed && foo.equals("ex")) {
            failed = true;
            throw new RuntimeException(foo);
        }
        failed = false;
    }

}

public class MyTests {

    @Autowired
    private RabbitListenerTestHarness harness; 

    @Test
    public void testTwoWay() throws Exception {
        assertEquals("FOO", this.rabbitTemplate.convertSendAndReceive(this.queue1.getName(), "foo"));

        InvocationData invocationData =
            this.harness.getNextInvocationDataFor("foo", 0, TimeUnit.SECONDS); 
        assertThat(invocationData.getArguments()[0], equalTo("foo"));     
        assertThat((String) invocationData.getResult(), equalTo("FOO"));
    }

    @Test
    public void testOneWay() throws Exception {
        this.rabbitTemplate.convertAndSend(this.queue2.getName(), "bar");
        this.rabbitTemplate.convertAndSend(this.queue2.getName(), "baz");
        this.rabbitTemplate.convertAndSend(this.queue2.getName(), "ex");

        InvocationData invocationData =
            this.harness.getNextInvocationDataFor("bar", 10, TimeUnit.SECONDS); 
        Object[] args = invocationData.getArguments();
        assertThat((String) args[0], equalTo("bar"));
        assertThat((String) args[1], equalTo(queue2.getName()));

        invocationData = this.harness.getNextInvocationDataFor("bar", 10, TimeUnit.SECONDS);
        args = invocationData.getArguments();
        assertThat((String) args[0], equalTo("baz"));

        invocationData = this.harness.getNextInvocationDataFor("bar", 10, TimeUnit.SECONDS);
        args = invocationData.getArguments();
        assertThat((String) args[0], equalTo("ex"));
        assertEquals("ex", invocationData.getThrowable().getMessage()); 
    }

}
```

|      | 将 harness 注入测试用例中，这样我们就可以获得对 spy 的访问。 |
| ---- | ------------------------------------------------------------ |
|      | 使用 `harness.getNextInvocationDataFor()` 检索调用数据 - 在这种情况下，因为它是一个请求/回复场景，所以不需要等待任何时间，因为测试线程被暂停在 `RabbitTemplate` 中等待结果。 |
|      | 然后，我们可以验证参数和结果是否符合预期。                   |
|      | 这一次我们需要一些时间来等待数据，因为这是一个在容器线程上的异步操作，我们需要暂停测试线程。 |
|      | 当监听器抛出一个异常时，它在调用数据的 `throwable` 属性中可用。 |

|      | 当使用自定义的 `Answer<?>` 与 harness 时，为了正常操作，这种 answer 应该子类 `ForwardsInvocation`，并从 harness 中获取实际的监听器（而不是 spy）（`getDelegate("myListener")`），并调用 `super.answer(invocation)`。参见所提供的 [Mockito `Answer` 实现](https://springdoc.cn/spring-amqp/#mockito-answer) 源代码中的例子。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

#### 4.5.4. 使用 `TestRabbitTemplate`

`TestRabbitTemplate` 被提供来执行一些基本的集成测试，而不需要一个 broker。当你把它作为 `@Bean` 添加到你的测试用例中时，它会发现上下文中所有的监听器容器，不管是以 `@Bean` 或 `<bean/>` 的形式声明，还是使用 `@RabbitListener` 注解。它目前只支持通过队列名称进行路由。template 从容器中提取消息监听器，并在测试线程中直接调用它。对于返回回复的监听器，支持 Request-reply 的消息传递（`sendAndReceive` 方法）。

下面的测试案例使用了该 template：

```java
@RunWith(SpringRunner.class)
public class TestRabbitTemplateTests {

    @Autowired
    private TestRabbitTemplate template;

    @Autowired
    private Config config;

    @Test
    public void testSimpleSends() {
        this.template.convertAndSend("foo", "hello1");
        assertThat(this.config.fooIn, equalTo("foo:hello1"));
        this.template.convertAndSend("bar", "hello2");
        assertThat(this.config.barIn, equalTo("bar:hello2"));
        assertThat(this.config.smlc1In, equalTo("smlc1:"));
        this.template.convertAndSend("foo", "hello3");
        assertThat(this.config.fooIn, equalTo("foo:hello1"));
        this.template.convertAndSend("bar", "hello4");
        assertThat(this.config.barIn, equalTo("bar:hello2"));
        assertThat(this.config.smlc1In, equalTo("smlc1:hello3hello4"));

        this.template.setBroadcast(true);
        this.template.convertAndSend("foo", "hello5");
        assertThat(this.config.fooIn, equalTo("foo:hello1foo:hello5"));
        this.template.convertAndSend("bar", "hello6");
        assertThat(this.config.barIn, equalTo("bar:hello2bar:hello6"));
        assertThat(this.config.smlc1In, equalTo("smlc1:hello3hello4hello5hello6"));
    }

    @Test
    public void testSendAndReceive() {
        assertThat(this.template.convertSendAndReceive("baz", "hello"), equalTo("baz:hello"));
    }
    @Configuration
    @EnableRabbit
    public static class Config {

        public String fooIn = "";

        public String barIn = "";

        public String smlc1In = "smlc1:";

        @Bean
        public TestRabbitTemplate template() throws IOException {
            return new TestRabbitTemplate(connectionFactory());
        }

        @Bean
        public ConnectionFactory connectionFactory() throws IOException {
            ConnectionFactory factory = mock(ConnectionFactory.class);
            Connection connection = mock(Connection.class);
            Channel channel = mock(Channel.class);
            willReturn(connection).given(factory).createConnection();
            willReturn(channel).given(connection).createChannel(anyBoolean());
            given(channel.isOpen()).willReturn(true);
            return factory;
        }

        @Bean
        public SimpleRabbitListenerContainerFactory rabbitListenerContainerFactory() throws IOException {
            SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
            factory.setConnectionFactory(connectionFactory());
            return factory;
        }

        @RabbitListener(queues = "foo")
        public void foo(String in) {
            this.fooIn += "foo:" + in;
        }

        @RabbitListener(queues = "bar")
        public void bar(String in) {
            this.barIn += "bar:" + in;
        }

        @RabbitListener(queues = "baz")
        public String baz(String in) {
            return "baz:" + in;
        }

        @Bean
        public SimpleMessageListenerContainer smlc1() throws IOException {
            SimpleMessageListenerContainer container = new SimpleMessageListenerContainer(connectionFactory());
            container.setQueueNames("foo", "bar");
            container.setMessageListener(new MessageListenerAdapter(new Object() {

                public void handleMessage(String in) {
                    smlc1In += in;
                }

            }));
            return container;
        }

    }

}
```

#### 4.5.5. JUnit4 `@Rules`

Spring AMQP 1.7 及以后的版本提供了一个额外的jar，叫做 `spring-rabbit-junit`。这个jar包含了几个实用的 `@Rule` 实例，在运行 JUnit4 测试时使用。参见 JUnit5 测试的 [JUnit5 Conditions](https://springdoc.cn/spring-amqp/#junit5-conditions)。

##### 使用 `BrokerRunning`

`BrokerRunning` 提供了一种机制，当 broker 没有运行时（默认情况下是在 `localhost`），让测试成功。

它还有一些实用的方法来初始化和清空队列以及删除队列和exchange。

下面的例子显示了它的用法：

```java
@ClassRule
public static BrokerRunning brokerRunning = BrokerRunning.isRunningWithEmptyQueues("foo", "bar");

@AfterClass
public static void tearDown() {
    brokerRunning.removeTestQueues("some.other.queue.too") // removes foo, bar as well
}
```

有几个 `isRunning…` 静态方法，比如 `isBrokerAndManagementRunning()`，它验证了 broker 是否启用了管理插件。

###### 配置 Rule

有些时候，如果没有 broker，你希望测试失败，比如每晚的CI构建。要在运行时禁用该规则，可将环境变量 `RABBITMQ_SERVER_REQUIRED` 设置为 `true`。

你可以用 setter 或环境变量来覆盖 broker 的属性，如 hostname：

下面的例子显示了如何用 setter 来覆盖属性：

```java
@ClassRule
public static BrokerRunning brokerRunning = BrokerRunning.isRunningWithEmptyQueues("foo", "bar");

static {
    brokerRunning.setHostName("10.0.0.1")
}

@AfterClass
public static void tearDown() {
    brokerRunning.removeTestQueues("some.other.queue.too") // removes foo, bar as well
}
```

你也可以通过设置以下环境变量来覆盖属性：

```java
public static final String BROKER_ADMIN_URI = "RABBITMQ_TEST_ADMIN_URI";
public static final String BROKER_HOSTNAME = "RABBITMQ_TEST_HOSTNAME";
public static final String BROKER_PORT = "RABBITMQ_TEST_PORT";
public static final String BROKER_USER = "RABBITMQ_TEST_USER";
public static final String BROKER_PW = "RABBITMQ_TEST_PASSWORD";
public static final String BROKER_ADMIN_USER = "RABBITMQ_TEST_ADMIN_USER";
public static final String BROKER_ADMIN_PW = "RABBITMQ_TEST_ADMIN_PASSWORD";
```

这些环境变量覆盖了默认设置（amqp 为 `localhost:5672`，management REST API 为 `localhost:15672/api/`）。

改变 host name 会影响 `amqp` 和 `management` REST API的连接（除非明确设置了 admin URI）。

`BrokerRunning` 还提供了一个名为 `setEnvironmentVariableOverrides` 的 `static` 方法，让你可以传入一个包含这些变量的 map。它们覆盖了系统环境变量。如果你希望在多个测试套件的测试中使用不同的配置，这可能是有用的。重要提示：该方法必须在调用任何创建规则实例的 `isRunning()` 静态方法之前被调用。变量值将应用于所有在此调用后创建的实例。调用 `clearEnvironmentVariableOverrides()` 来重置规则以使用默认值（包括任何实际环境变量）。

在你的测试案例中，你可以在创建连接工厂时使用 `brokerRunning`；`getConnectionFactory()` 返回该规则的 RabbitMQ `ConnectionFactory`。下面的示例显示了如何做到这一点：

```java
@Bean
public CachingConnectionFactory rabbitConnectionFactory() {
    return new CachingConnectionFactory(brokerRunning.getConnectionFactory());
}
```

##### 使用 `LongRunningIntegrationTest`

`LongRunningIntegrationTest` 是一个禁止长时间运行测试的 rule。你可能想在开发者系统上使用这个 rule，但要确保该 rule 在诸如夜间CI构建中被禁用。

下面的例子显示了它的用法：

```java
@Rule
public LongRunningIntegrationTest longTests = new LongRunningIntegrationTest();
```

要在运行时禁用该规则，请将一个名为 `RUN_LONG_INTEGRATION_TESTS` 的环境变量设置为 `true`。

#### 4.5.6. JUnit5 Conditions

2.0.2版本引入了对JUnit5的支持。

##### 使用 `@RabbitAvailable` 注解

这个类级注解类似于 [JUnit4 `@Rules`](https://springdoc.cn/spring-amqp/#junit-rules) 中讨论的 `BrokerRunning` `@Rule`。它由 `RabbitAvailableCondition` 处理。

该注解有三个属性：

- `queues`: 一个 queue 数组，在每次测试前声明（和清除），在所有测试完成后删除。
- `management`: 如果你的测试也需要在 broker 上安装 management 插件，则将此设置为true。
- `purgeAfterEach`: （从2.2版开始）当为 `true` 时（默认），`queues` 将在测试之间被清除。

它被用来检查 broker 是否可用，如果没有，就跳过测试。正如 [配置 Rule](https://springdoc.cn/spring-amqp/#brokerRunning-configure) 中所讨论的，环境变量 `RABBITMQ_SERVER_REQUIRED`，如果为 `true`，会导致测试在没有 broker 时快速失败。你可以通过使用环境变量来配置这个条件，如 [配置 Rule](https://springdoc.cn/spring-amqp/#brokerRunning-configure) 中所讨论的。

此外，`RabbitAvailableCondition` 支持参数化测试构造函数和方法的参数解析。支持两种参数类型：

- `BrokerRunningSupport`: 实例（在2.2之前，这是一个JUnit 4 `BrokerRunning` 实例）。
- `ConnectionFactory`: `BrokerRunningSupport` 实例的 RabbitMQ 连接工厂。

下面的例子显示了这两种情况：

```java
@RabbitAvailable(queues = "rabbitAvailableTests.queue")
public class RabbitAvailableCTORInjectionTests {

    private final ConnectionFactory connectionFactory;

    public RabbitAvailableCTORInjectionTests(BrokerRunningSupport brokerRunning) {
        this.connectionFactory = brokerRunning.getConnectionFactory();
    }

    @Test
    public void test(ConnectionFactory cf) throws Exception {
        assertSame(cf, this.connectionFactory);
        Connection conn = this.connectionFactory.newConnection();
        Channel channel = conn.createChannel();
        DeclareOk declareOk = channel.queueDeclarePassive("rabbitAvailableTests.queue");
        assertEquals(0, declareOk.getConsumerCount());
        channel.close();
        conn.close();
    }

}
```

前面的测试是在框架本身中进行的，它验证了参数注入和条件（condition）正确创建了队列。

一个实际的用户测试可能如下：

```java
@RabbitAvailable(queues = "rabbitAvailableTests.queue")
public class RabbitAvailableCTORInjectionTests {

    private final CachingConnectionFactory connectionFactory;

    public RabbitAvailableCTORInjectionTests(BrokerRunningSupport brokerRunning) {
        this.connectionFactory =
            new CachingConnectionFactory(brokerRunning.getConnectionFactory());
    }

    @Test
    public void test() throws Exception {
        RabbitTemplate template = new RabbitTemplate(this.connectionFactory);
        ...
    }
}
```

当你在测试类中使用 Spring 注解 application context 时，你可以通过一个名为 `RabbitAvailableCondition.getBrokerRunning()` 的静态方法获得对条件（condition）的连接工厂的引用。

|      | 从 2.2 版本开始，`getBrokerRunning()` 返回一个 `BrokerRunningSupport` 对象；以前，返回的是JUnit 4 `BrokerRunnning` 实例。这个新类具有与 `BrokerRunning` 相同的API。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

下面的测试来自于该框架并演示了用法：

```java
@RabbitAvailable(queues = {
        RabbitTemplateMPPIntegrationTests.QUEUE,
        RabbitTemplateMPPIntegrationTests.REPLIES })
@SpringJUnitConfig
@DirtiesContext(classMode = ClassMode.AFTER_EACH_TEST_METHOD)
public class RabbitTemplateMPPIntegrationTests {

    public static final String QUEUE = "mpp.tests";

    public static final String REPLIES = "mpp.tests.replies";

    @Autowired
    private RabbitTemplate template;

    @Autowired
    private Config config;

    @Test
    public void test() {

        ...

    }

    @Configuration
    @EnableRabbit
    public static class Config {

        @Bean
        public CachingConnectionFactory cf() {
            return new CachingConnectionFactory(RabbitAvailableCondition
                    .getBrokerRunning()
                    .getConnectionFactory());
        }

        @Bean
        public RabbitTemplate template() {

            ...

        }

        @Bean
        public SimpleRabbitListenerContainerFactory
                            rabbitListenerContainerFactory() {

            ...

        }

        @RabbitListener(queues = QUEUE)
        public byte[] foo(byte[] in) {
            return in;
        }

    }

}
```

##### 使用 `@LongRunning` 注解

与 `LongRunningIntegrationTest` JUnit4 `@Rule` 类似，这个注解导致测试被跳过，除非环境变量（或系统属性）被设置为 `true`。下面的例子展示了如何使用它：

```java
@RabbitAvailable(queues = SimpleMessageListenerContainerLongTests.QUEUE)
@LongRunning
public class SimpleMessageListenerContainerLongTests {

    public static final String QUEUE = "SimpleMessageListenerContainerLongTests.queue";

...

}
```

默认情况下，该变量为 `RUN_LONG_INTEGRATION_TESTS`，但你可以在注解的 `value` 属性中指定变量名称。

## 5. Spring Integration - 参考

这一部分的参考文档快速介绍了 Spring Integration 项目中的 AMQP 支持。

### 5.1. Spring Integration AMQP 支持

这一章简要介绍了 Spring Integration 和 Spring AMQP 项目之间的关系。

#### 5.1.1. 简介

[Spring Integration](https://www.springsource.org/spring-integration) 项目包括建立在Spring AMQP项目之上的 AMQP Channel Adapter 和 Gateways。这些适配器是在Spring Integration项目中开发和发布的。在Spring Integration中，“Channel Adapters” 是单向的，而 “Gateways” 是双向的（request-reply）。我们提供了一个 inbound-channel-adapter，一个 outbound-channel-adapter，一个 inbound-gateway，和一个 outbound-gateway。

由于 AMQP adapter 是 Spring Integration 版本的一部分，所以文档可作为Spring Integration 发布的一部分。我们在这里提供了主要功能的快速概述。更多细节请参见 [《Spring Integration参考指南》](https://docs.spring.io/spring-integration/reference/htmlsingle/)。

#### 5.1.2. Inbound Channel Adapter

为了从队列中接收AMQP消息，你可以配置一个 `<inbound-channel-adapter>`。下面的例子显示了如何配置一个 inbound channel adapter：

```xml
<amqp:inbound-channel-adapter channel="fromAMQP"
                              queue-names="some.queue"
                              connection-factory="rabbitConnectionFactory"/>
```

#### 5.1.3. Outbound Channel Adapter

要发送AMQP消息到一个 exchange，你可以配置一个 `<outbound-channel-adapter>`。除了 exchange 名称之外，你还可以选择提供一个 "routing-key"。下面的例子显示了如何定义一个 outbound channel adapter：

```xml
<amqp:outbound-channel-adapter channel="toAMQP"
                               exchange-name="some.exchange"
                               routing-key="foo"
                               amqp-template="rabbitTemplate"/>
```

#### 5.1.4. Inbound Gateway

为了从队列中接收 AMQP 消息并响应其回复地址，你可以配置一个 `<inbound-gateway>`。下面的例子显示了如何定义一个 inbound gateway：

```xml
<amqp:inbound-gateway request-channel="fromAMQP"
                      reply-channel="toAMQP"
                      queue-names="some.queue"
                      connection-factory="rabbitConnectionFactory"/>
```

#### 5.1.5. Outbound Gateway

为了向一个 exchange 发送AMQP消息并从远程客户端接收响应，你可以配置一个 `<outbound-gateway>`。除了 exchange 名称之外，你还可以选择提供一个 "routing-key"。下面的例子显示了如何定义一个 outbound gateway：

```xml
<amqp:outbound-gateway request-channel="toAMQP"
                       reply-channel="fromAMQP"
                       exchange-name="some.exchange"
                       routing-key="foo"
                       amqp-template="rabbitTemplate"/>
```

## 6. 其他资源

除了这个参考文档之外，还有一些其他的资源可以帮助你了解 AMQP。

### 6.1. 延伸阅读

对于那些不熟悉AMQP的人来说，该 [规范](https://www.amqp.org/resources/download) 实际上是相当可读的。当然，它是权威的信息来源，对于熟悉规范的人来说，Spring AMQP 代码应该很容易理解。我们目前对 RabbitMQ 支持的实现是基于他们的 2.8.x 版本，它正式支持 AMQP 0.8 和 0.9.1。我们建议阅读 0.9.1 文档。

在 RabbitMQ [入门](https://www.rabbitmq.com/how.html) 页面上有许多精彩的文章、演示和博客。由于这是目前唯一支持 Spring AMQP 的实现，我们也推荐将其作为所有 broker 相关问题的一般起点。

## Appendix A: Micrometer Observation 文档

### A.1. Observability - Metrics

Below you can find a list of all metrics declared by this project.

#### A.1.1. Listener Observation

> Observation for Rabbit listeners.

**Metric name** `spring.rabbit.listener` (defined by convention class `RabbitListenerObservation$DefaultRabbitListenerObservationConvention`). **Type** `timer`.

**Metric name** `spring.rabbit.listener.active` (defined by convention class `RabbitListenerObservation$DefaultRabbitListenerObservationConvention`). **Type** `long task timer`.

|      | KeyValues that are added after starting the Observation might be missing from the *.active metrics. |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | Micrometer internally uses `nanoseconds` for the baseunit. However, each backend determines the actual baseunit. (i.e. Prometheus uses seconds) |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

Name of the enclosing class `RabbitListenerObservation`.

|      | All tags must be prefixed with `spring.rabbit.listener` prefix! |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

| Name                                     | Description  |
| ---------------------------------------- | ------------ |
| `spring.rabbit.listener.id` *(required)* | Listener id. |

#### A.1.2. Template Observation

> Observation for RabbitTemplates.

**Metric name** `spring.rabbit.template` (defined by convention class `RabbitTemplateObservation$DefaultRabbitTemplateObservationConvention`). **Type** `timer`.

**Metric name** `spring.rabbit.template.active` (defined by convention class `RabbitTemplateObservation$DefaultRabbitTemplateObservationConvention`). **Type** `long task timer`.

|      | KeyValues that are added after starting the Observation might be missing from the *.active metrics. |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

|      | Micrometer internally uses `nanoseconds` for the baseunit. However, each backend determines the actual baseunit. (i.e. Prometheus uses seconds) |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

Name of the enclosing class `RabbitTemplateObservation`.

|      | All tags must be prefixed with `spring.rabbit.template` prefix! |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

| Name                                       | Description                |
| ------------------------------------------ | -------------------------- |
| `spring.rabbit.template.name` *(required)* | Bean name of the template. |

### A.2. Observability - Spans

Below you can find a list of all spans declared by this project.

#### A.2.1. Listener Observation Span

> Observation for Rabbit listeners.

**Span name** `spring.rabbit.listener` (defined by convention class `RabbitListenerObservation$DefaultRabbitListenerObservationConvention`).

Name of the enclosing class `RabbitListenerObservation`.

|      | All tags must be prefixed with `spring.rabbit.listener` prefix! |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

| Name                                     | Description  |
| ---------------------------------------- | ------------ |
| `spring.rabbit.listener.id` *(required)* | Listener id. |

#### A.2.2. Template Observation Span

> Observation for RabbitTemplates.

**Span name** `spring.rabbit.template` (defined by convention class `RabbitTemplateObservation$DefaultRabbitTemplateObservationConvention`).

Name of the enclosing class `RabbitTemplateObservation`.

|      | All tags must be prefixed with `spring.rabbit.template` prefix! |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

| Name                                       | Description                |
| ------------------------------------------ | -------------------------- |
| `spring.rabbit.template.name` *(required)* | Bean name of the template. |

### A.3. Observability - Conventions

Below you can find a list of all `GlobalObservationConvention` and `ObservationConvention` declared by this project.

| ObservationConvention Class Name                             | Applicable ObservationContext Class Name |
| ------------------------------------------------------------ | ---------------------------------------- |
| `RabbitListenerObservation`                                  | `RabbitMessageReceiverContext`           |
| `RabbitListenerObservation.DefaultRabbitListenerObservationConvention` | `RabbitMessageReceiverContext`           |
| `RabbitListenerObservationConvention`                        | `RabbitMessageReceiverContext`           |
| `RabbitTemplateObservation`                                  | `RabbitMessageSenderContext`             |
| `RabbitTemplateObservation.DefaultRabbitTemplateObservationConvention` | `RabbitMessageSenderContext`             |
| `RabbitTemplateObservationConvention`                        | `RabbitMessageSenderContext`             |

## Appendix B: 原生镜像

提供 [Spring AOT](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aot) native hints，以协助为使用Spring AMQP的Spring应用程序开发原生镜像（native image）。

一些例子可以在 [`spring-aot-smoke-tests` GitHub仓库](https://github.com/spring-projects/spring-aot-smoke-tests/tree/main/integration) 中看到。

## Appendix C: 更改历史

本节介绍了随着版本变化而做出的改变。

### C.1. 当前版本

见 [最新动态](https://springdoc.cn/spring-amqp/#whats-new)。

### C.2. 之前的发布

#### C.2.1. 2.4中自2.3版以来的变化

本节描述了2.3版和2.4版之间的变化。关于以前版本的变化，请参见 [更改历史](https://springdoc.cn/spring-amqp/#change-history)。

##### `@RabbitListener` 的变化

`MessageProperties` 现在可用于参数匹配。更多信息见 [注解端点的方法签名](https://springdoc.cn/spring-amqp/#async-annotation-driven-enable-signature)。

##### `RabbitAdmin` 的变化

一个新的属性 `recoverManualDeclarations` 允许恢复手动声明的 queue/exchange/binding。更多信息见 [恢复 Auto-Delete 的声明](https://springdoc.cn/spring-amqp/#declarable-recovery)。

##### 远程支持

使 用Spring 框架的 RMI 支持的远程支持已被废弃，并将在3.0中删除。参见 Spring Remoting with AMQP 获取更多信息。

#### C.2.2. Message Converter 的变化

`Jackson2JsonMessageConverter` 现在可以从 `contentEncoding` header 确定字符集。更多信息见 [Jackson2JsonMessageConverter](https://springdoc.cn/spring-amqp/#json-message-converter)。

#### C.2.3. Stream 支持的变化

`RabbitStreamOperations` 和 `RabbitStreamTemplate` 已被弃用，而分别支持 `RabbitStreamOperations2` 和 `RabbitStreamTemplate2`；它们返回 `CompletableFuture` 而不是 `ListenableFuture`。请参阅 [使用 RabbitMQ Stream 插件](https://springdoc.cn/spring-amqp/#stream-support) 以了解更多信息。

#### C.2.4. 2.3中自2.2以来的变化

本节描述了2.2版和2.3版之间的变化。有关以前版本的变化，请参见 [更改历史](https://springdoc.cn/spring-amqp/#change-history)。

##### Connection Factory 的变化

现在提供了两个额外的连接工厂。更多信息见 [选择一个 Connection Factory](https://springdoc.cn/spring-amqp/#choosing-factory)。

##### `@RabbitListener` 的变化

你现在可以指定一个 reply content type。更多信息见 [回复内容类型（ContentType）](https://springdoc.cn/spring-amqp/#reply-content-type)。

##### Message Converter 的变化

如果 `ObjectMapper` 配置了自定义 deserializer，`Jackson2JMessageConverter` 现在可以反序列化抽象类（包括接口）。更多信息见 [反序列化抽象类](https://springdoc.cn/spring-amqp/#jackson-abstract)。

##### 测试的变化

提供了一个新的注解 `@SpringRabbitTest`，用于在不使用 `SpringBootTest` 时自动配置一些基础设施Bean。更多信息见 [@SpringRabbitTest](https://springdoc.cn/spring-amqp/#spring-rabbit-test)。

##### RabbitTemplate 的变化

template 的 `ReturnCallback` 已经被重构为 `ReturnsCallback`，以便在 lambda 表达式中更简单地使用。更多信息见 [相关的 Publisher（发布者）确认和返回](https://springdoc.cn/spring-amqp/#template-confirms)。

当使用 return 和相关的确认时，`CorrelationData` 现在需要一个唯一的 `id` 属性。更多信息见 [相关的 Publisher（发布者）确认和返回](https://springdoc.cn/spring-amqp/#template-confirms)。

当使用 direct reply-to 时，你现在可以配置 template，使服务器不需要与 reply 一起返回相关的数据。参见 [RabbitMQ Direct reply-to](https://springdoc.cn/spring-amqp/#direct-reply-to) 获取更多信息。

##### Listener Container 的变化

一个新的监听器容器属性 `consumeDelay` 现在可用；它在使用 [RabbitMQ Sharding Plugin](https://github.com/rabbitmq/rabbitmq-sharding) 时很有用。

默认的 `JavaLangErrorHandler` 现在调用 `System.exit(99)`。要恢复到以前的行为（什么都不做），请添加一个 no-op handler。

容器现在支持 `globalQos` 属性，以便在全局范围内为 channel 而不是为 channel 上的每个消费者应用 `prefetchCount`。

更多信息见 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes)。

##### MessagePostProcessor 的变化

压缩的 `MessagePostProcessor` 现在使用逗号来分隔多个内容编码（multiple content encoding），而不是冒号。解压缩器可以处理这两种格式，但是，如果你用这个版本产生的消息被早于2.2.12的版本所消耗，你应该将压缩器配置为使用旧的分隔符。更多信息请参见 [修改消息 - 压缩和其他](https://springdoc.cn/spring-amqp/#post-processing) 中的 IMPORTANT 说明。

##### 多 Broker 支持的改进

更多信息见 [支持多个 Broker （或集群）](https://springdoc.cn/spring-amqp/#multi-rabbit)。

##### RepublishMessageRecoverer 的变化

没有提供支持发布者确认的该 recoverer 的新子类。更多信息见 [消息监听器和异步案例](https://springdoc.cn/spring-amqp/#async-listeners)。

#### C.2.5. 2.2中自2.1以来的变化

本节介绍了2.1版和2.2版之间的变化。

##### Package 的变化

以下类/接口已从 `org.springframework.amqp.rabbit.core.support` 移至 `org.springframework.amqp.rabbit.batch`：

- `BatchingStrategy`
- `MessageBatch`
- `SimpleBatchingStrategy`

此外，`ListenerExecutionFailedException` 已经从 `org.springframework.amqp.rabbit.listener.exception` 移到了 `org.springframework.amqp.rabbit.support`。

##### 依赖的变化

JUnit (4) 现在是一个可选的依赖，不再作为一个传递赖关系出现。

`spring-rabbit-junit` 模块现在是 `spring-rabbit-test` 模块的一个 **compile** 依赖项，以获得更好的目标应用开发体验，当我们只用一个 `spring-rabbit-test` 就可以获得AMQP组件的全部测试工具。

##### "Breaking" 的变化

JUnit (5) `RabbitAvailableCondition.getBrokerRunning()` 现在返回一个 `BrokerRunningSupport` 实例而不是 `BrokerRunning`，后者依赖于 JUnit 4。它具有相同的 API，所以只需改变任何引用的类名就可以了。更多信息请参见 [JUnit5 Conditions](https://springdoc.cn/spring-amqp/#junit5-conditions)。

##### ListenerContainer 的变化

现在，即使确认模式是手动的，带有致命异常的消息也会被拒绝，并且不会被重新接收。更多信息请参见 [异常处理](https://springdoc.cn/spring-amqp/#exception-handling)。

现在可以使用 Micrometer `Timer` 来监测 listener 的性能。更多信息请参见 [监测监听器的性能](https://springdoc.cn/spring-amqp/#micrometer)。

##### @RabbitListener 的变化

你现在可以在每个监听器上配置一个 `executor`，覆盖工厂配置，以更容易地标识与监听器相关的线程。你现在可以用注解的 `ackMode` 属性覆盖容器工厂的 `acknowledgeMode` 属性。更多信息请参见 [覆盖容器工厂属性](https://springdoc.cn/spring-amqp/#listener-property-overrides)。

当使用 [批处理](https://springdoc.cn/spring-amqp/#receiving-batch) 时，`@RabbitListener` 方法现在可以在一次调用中接收完整的一批消息，而不是一次一次地获取它们。

当逐一接收批处理的消息时，最后一条消息的 `isLastInBatch` 消息属性设置为 `true`。

此外，收到的批处理信息现在包含 `amqp_batchSize` header。

Listener 也可以消费在 `SimpleMessageListenerContainer` 中创建的批处理消息，即使该批处理消息不是由生产者创建的。更多信息见 [选择容器](https://springdoc.cn/spring-amqp/#choose-container)。

`Jackson2JsonMessageConverter` 现在支持 Spring 数据投射接口。更多信息见 [使用 Spring Data 投影接口](https://springdoc.cn/spring-amqp/#data-projection)。

`Jackson2JsonMessageConverter` 现在假定内容是 JSON，如果没有 `contentType` 属性，或它是默认的（`application/octet-string`）。参见 [从 `Message` 转换](https://springdoc.cn/spring-amqp/#Jackson2JsonMessageConverter-from-message) 以了解更多信息。

同样，`Jackson2XmlMessageConverter` 现在假定内容是XML，如果没有 `contentType` 属性，或者是默认的（`application/octet-string`）。更多信息见 [`Jackson2XmlMessageConverter`](https://springdoc.cn/spring-amqp/#jackson2xml)。

当 `@RabbitListener` 方法返回一个结果时，Bean 和 `Method` 现在可以在回复消息属性中使用。这允许配置 `beforeSendReplyMessagePostProcessor`，例如，在回复中设置一个 header，以表明在服务器上调用了哪个方法。更多信息请参见 [回复管理](https://springdoc.cn/spring-amqp/#async-annotation-driven-reply) 。

你现在可以配置一个 `ReplyPostProcessor`，在回复信息发送前对其进行修改。更多信息请参见 [回复管理](https://springdoc.cn/spring-amqp/#async-annotation-driven-reply)。

##### AMQP Logging Appenders 的变化

Log4J 和 Logback `AmqpAppender` 现在支持 `verifyHostname` SSL 选项。

另外，这些 appender 现在可以被配置为不把 MDC 条目作为 header 添加。 `addMdcAsHeaders` 布尔选项被引入来配置这种行为。

appender 现在支持 `SaslConfig` 属性。

更多信息见 [日志子系统 AMQP Appender](https://springdoc.cn/spring-amqp/#logging)。

##### MessageListenerAdapter 的变化

`MessageListenerAdapter` 现在提供了一个新的 `buildListenerArguments(Object, Channel, Message)` 方法来建立一个参数数组，以传递给目标监听器，旧的方法已被废弃。更多信息请参见 [`MessageListenerAdapter`](https://springdoc.cn/spring-amqp/#message-listener-adapter) 。

##### Exchange/Queue Declaration 的变化

用于创建供 `RabbitAdmin` 声明的 `Exchange` 和 `Queue` 对象的 `ExchangeBuilder` 和 `QueueBuilder` fluent API 现在支持 "已知" 参数。请参阅 [Queue 和 Exchange 的 Builder API](https://springdoc.cn/spring-amqp/#builder-api) 以了解更多信息。

`RabbitAdmin` 有一个新的属性 `explicitDeclarationsOnly`。请参阅 [条件式声明](https://springdoc.cn/spring-amqp/#conditional-declaration) 以了解更多信息。

##### Connection Factory 的变化

`CachingConnectionFactory` 有一个新属性 `shuffleAddresses`。当提供 broker 节点地址列表时，该列表将在创建连接前被“洗牌打乱”，以便尝试连接的顺序是随机的。更多信息见 [连接到群集](https://springdoc.cn/spring-amqp/#cluster)。

当使用 Publisher 确认和返回时，回调现在被调用到连接工厂的 `executor` 上。这避免了在回调中执行 rabbit 操作时，`amqp-clients` 库可能出现的死锁。更多信息请参见 [相关的 Publisher（发布者）确认和返回](https://springdoc.cn/spring-amqp/#template-confirms)。

另外，现在用 `ConfirmType` 枚举来指定发布者的确认类型，而不是用两个相互排斥的 setter 方法。

`RabbitConnectionFactoryBean` 现在在启用 SSL 时默认使用 TLS 1.2。请参阅 [`RabbitConnectionFactoryBean` 以及配置 SSL](https://springdoc.cn/spring-amqp/#rabbitconnectionfactorybean-configuring-ssl) 了解更多信息。

##### 新的 MessagePostProcessor 类

增加了 `DeflaterPostProcessor` 和 `InflaterPostProcessor` 类，当消息 content-encoding 被设置为 `deflate` 时，分别支持压缩和解压。

##### 其他变化

`Declarables` 对象（用于声明多个 queue、exchange、binding）现在对每种类型都有一个过滤的 getter。更多信息见 [声明 Exchange、Queue 和 Binding 的集合](https://springdoc.cn/spring-amqp/#collection-declaration)。

现在你可以在 `RabbitAdmin` 处理其声明之前自定义每个 `Declarable` Bean。请参阅 [Exchange、 Queue 和 Binding 的自动声明](https://springdoc.cn/spring-amqp/#automatic-declaration) 以了解更多信息。

`singleActiveConsumer()` 已被添加到 `QueueBuilder` 中，用于设置 `x-single-active-consumer` 队列参数。更多信息见 [Queue 和 Exchange 的 Builder API](https://springdoc.cn/spring-amqp/#builder-api)。

具有 `Class<?>` 类型值的出站 header 现在使用 `getName()` 而不是 `toString()` 进行映射。更多信息见 [Message Properties 转换器](https://springdoc.cn/spring-amqp/#message-properties-converters)。

现在支持恢复生产者创建的失败批处理。更多信息见 [批处理监听器的重试](https://springdoc.cn/spring-amqp/#batch-retry) 。

#### C.2.6. 2.1中自2.0以来的改变

##### AMQP Client 库

Spring AMQP 现在使用 RabbitMQ 团队提供的 5.4.x 版本的 `amqp-client` 库。该客户端默认配置了自动恢复功能。参见 [RabbitMQ 自动连接/拓扑恢复](https://springdoc.cn/spring-amqp/#auto-recovery)。

|      | 从 4.0 版本开始，客户端默认启用自动恢复功能。虽然与此功能兼容，但Spring AMQP有自己的恢复机制，一般不需要客户端恢复功能。我们建议禁用 `amqp-client` 自动恢复功能，以避免 在broker 可用但连接尚未恢复时得到 `AutoRecoverConnectionNotCurrentlyOpenException` 实例。从 1.7.1 版开始，Spring AMQP 禁用它，除非你明确创建你自己的 RabbitMQ 连接工厂并将其提供给 `CachingConnectionFactory`。由 `RabbitConnectionFactoryBean` 创建的 RabbitMQ `ConnectionFactory` 实例也默认禁用该选项。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### Package 的变化

某些类已经转移到不同的包中。大多数是内部类，不影响用户应用程序。两个例外是 `ChannelAwareMessageListener` 和 `RabbitListenerErrorHandler`。这些接口现在在 `org.springframework.amqp.rabbit.listener.api` 中。

##### 发布确认的变化

当有未完成的确认时，为发布者确认启用的 channel 不会返回到缓存中。更多信息见 [相关的 Publisher（发布者）确认和返回](https://springdoc.cn/spring-amqp/#template-confirms)。

##### Listener Container Factory 的改进

你现在可以使用监听器容器工厂来创建任何监听器容器，而不仅仅是那些用于 `@RabbitListener` 注解或 `@RabbitListenerEndpointRegistry` 的容器。请参阅 [使用容器工厂](https://springdoc.cn/spring-amqp/#using-container-factories) 了解更多信息。

`ChannelAwareMessageListener` 现在继承自 `MessageListener`。

##### Broker Event Listener

引入 `BrokerEventListener` 来发布选定的 broker 事件作为 `ApplicationEvent` 实例。更多信息见 [Broker 事件监听器](https://springdoc.cn/spring-amqp/#broker-events)。

##### RabbitAdmin 的变化

`RabbitAdmin` 会发现 `Declarables` 类型的 Bean（它是 `Declarable` - `Queue`、`Exchange` 和 `Binding` 对象的容器）并在 broker 上声明所包含的对象。不鼓励用户使用声明 `<Collection<Queue>>`（和其他）的旧机制，而应使用 `Declarables` Bean。默认情况下，旧机制被禁用。更多信息请参见 [声明 Exchange、Queue 和 Binding 的集合](https://springdoc.cn/spring-amqp/#collection-declaration)。

`AnonymousQueue` 实例现在在声明时默认将 `x-queue-master-locator` 设置为 `client-local`，以确保队列在应用程序所连接的节点上创建。更多信息见 [配置 Broker](https://springdoc.cn/spring-amqp/#broker-configuration)。

##### RabbitTemplate 的变化

你现在可以使用 `noLocalReplyConsumer` 选项配置 `RabbitTemplate`，以控制 `sendAndReceive()` 操作中的回复消费者的 `noLocal` 标志。请参阅 [请求/回复消息](https://springdoc.cn/spring-amqp/#request-reply) 了解更多信息。

用于发布者确认的 `CorrelationData` 现在有一个 `ListenableFuture`，你可以用它来获取确认，而不是使用回调。当返回和确认被启用时，相关数据（如果提供的话）被填充到返回的消息中。更多信息请参见 [相关的 Publisher（发布者）确认和返回](https://springdoc.cn/spring-amqp/#template-confirms)。

现在提供了一个叫做 `replyTimedOut` 的方法来通知子类一个回复已经超时了，从而允许任何状态的清理。更多信息请参见 [回复超时](https://springdoc.cn/spring-amqp/#reply-timeout) 。

现在，当使用带有 `DirectReplyToMessageListenerContainer`（默认）的 `request/reply` 时，你可以指定一个 `ErrorHandler`，以便在回复交付时发生异常（例如，延迟回复）时被调用。请参阅 `RabbitTemplate` 上的 `setReplyErrorHandler`。（也是自 2.0.11 起）。

##### 消息转换

我们引入了一个新的 `Jackson2XmlMessageConverter` 来支持将消息从XML格式反序列化/序列化为XML格式。更多信息见 [`Jackson2XmlMessageConverter`](https://springdoc.cn/spring-amqp/#jackson2xml) 。

##### Management REST API

`RabbitManagementTemplate` 现在已被弃用，而是直接使用 `com.rabbitmq.http.client.Client`（或 `com.rabbitmq.http.client.ReactorNettyClient`）。更多信息请参见 [RabbitMQ REST API](https://springdoc.cn/spring-amqp/#management-rest-api) 。

##### `@RabbitListener` 的变化

现在可以用 `RetryTemplate` 来配置监听器容器工厂，也可以选择在发送回复时使用 `RecoveryCallback`。更多信息见 [启用 Listener Endpoint 注解](https://springdoc.cn/spring-amqp/#async-annotation-driven-enable)。

##### `@RabbitListener` 异步返回

`@RabbitListener` 方法现在可以返回 `ListenableFuture<?>` 或 `Mono<?>`。更多信息见 [异步的 `@RabbitListener` 返回类型](https://springdoc.cn/spring-amqp/#async-returns) 。

##### Connection Factory Bean 的变化

默认情况下，`RabbitConnectionFactoryBean` 现在调用 `enableHostnameVerification()`。要恢复到以前的行为，请将 `enableHostnameVerification` 属性设置为 `false`。

##### Connection Factory 的变化

`CachingConnectionFactory` 现在无条件地禁用底层 RabbitMQ `ConnectionFactory` 的自动恢复功能，即使在构造函数中提供了预配置的实例。虽然已经采取措施使 Spring AMQP 与自动恢复兼容，但仍出现了某些问题的角落。Spring AMQP从1.0.0开始就有自己的恢复机制，不需要使用客户端提供的恢复。虽然在构建 `CachingConnectionFactory` 后仍有可能启用该功能（使用 `cachingConnectionFactory.getRabbitConnectionFactory().setAutomaticRecoveryEnabled()` ），但我们强烈建议你不要这样做。如果你在直接使用客户端工厂时需要自动恢复连接（而不是使用 Spring AMQP 组件），我们建议你使用单独的 RabbitMQ `ConnectionFactory`。

##### Listener Container 的变化

默认的 `ConditionalRejectingErrorHandler` 现在可以完全丢弃导致致命错误的信息，如果有 `x-death` header 的话。更多信息请参见 [异常处理](https://springdoc.cn/spring-amqp/#exception-handling) 。

##### 即时请求

引入了一个新的 `ImmediateRequeueAmqpException` 来通知监听器容器该消息必须被重新入队列。为了使用这个功能，增加了一个新的 `ImmediateRequeueMessageRecoverer` 实现。

更多信息见 [消息监听器和异步案例](https://springdoc.cn/spring-amqp/#async-listeners) 。

#### C.2.7. 2.0中自1.7以来的变化

##### 使用 `CachingConnectionFactory`

从 2.0.2 版开始，你可以将 `RabbitTemplate` 配置为使用与监听器容器所使用的不同的连接。这一变化可避免在生产者因任何原因被阻塞时出现死锁的消费者。请参阅 [使用一个单独的连接](https://springdoc.cn/spring-amqp/#separate-connection) 以了解更多信息。

##### AMQP Client 库

Spring AMQP 现在使用 RabbitMQ 团队提供的新的 5.0.x 版本的 `amqp-client` 库。该客户端默认配置了自动恢复功能。参见 [RabbitMQ 自动连接/拓扑恢复](https://springdoc.cn/spring-amqp/#auto-recovery)。

|      | 从 4.0 版本开始，客户端默认启用自动恢复功能。虽然与此功能兼容，但Spring AMQP有自己的恢复机制，一般不需要客户端的恢复功能。我们建议你禁用 `amqp-client` 自动恢复功能，以避免在经纪商可用但连接尚未恢复时得到 `AutoRecoverConnectionNotCurrentlyOpenException` 实例。从 1.7.1 版开始，Spring AMQP 禁用它，除非你明确创建你自己的 RabbitMQ 连接工厂并将其提供给 `CachingConnectionFactory`。由 `RabbitConnectionFactoryBean` 创建的 RabbitMQ `ConnectionFactory` 实例也默认禁用该选项。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### 一般性变化

`ExchangeBuilder` 现在默认建立了 durable exchange。在 `@QeueueBinding` 中使用的 `@Exchange` 注解也默认声明了 durable exchange。在 `@RabbitListener` 中使用的 `@Queue` 注解，如果是命名的，则默认声明持久的队列，如果是匿名的，则声明非持久的。更多信息请参见 [Queue 和 Exchange 的 Builder API](https://springdoc.cn/spring-amqp/#builder-api) 和 [注解驱动的 Listener 端点](https://springdoc.cn/spring-amqp/#async-annotation-driven)。

##### 被删除的类

`UniquelyNameQueue` 不再被提供。创建一个具有唯一名称的 durable，非 auto-delete 队列是不寻常的。这个类已经被删除。如果你需要它的功能，请使用 `new Queue(UUID.randomUUID().toString())`。

##### 新的 Listener Container

`DirectMessageListenerContainer` 已经与现有的 `SimpleMessageListenerContainer` 一起被添加。请参阅 [选择容器](https://springdoc.cn/spring-amqp/#choose-container) 和 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes)，了解关于选择使用哪种容器以及如何配置它们的信息。

##### Log4j Appender

由于log4j的寿命结束，这个appender已经不再可用。有关可用的 log appender 的信息，请参见 [日志子系统 AMQP Appender](https://springdoc.cn/spring-amqp/#logging)。

##### `RabbitTemplate` 的变化

|      | 以前，如果一个非事务性的 `RabbitTemplate` 在一个事务性的监听器容器线程上运行，它就会参与一个现有的事务。这是一个严重的错误。然而，用户可能已经依赖这种行为。从 1.6.2 版开始，你必须在模板上设置 `channelTransacted` 布尔值，以便它参与容器事务。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

`RabbitTemplate` 现在使用 `DirectReplyToMessageListenerContainer`（默认），而不是为每个请求创建一个新的消费者。请参阅 [RabbitMQ Direct reply-to](https://springdoc.cn/spring-amqp/#direct-reply-to) 以了解更多信息。

`AsyncRabbitTemplate` 现在支持 direct reply-to。更多信息见 [Async Rabbit Template](https://springdoc.cn/spring-amqp/#async-template) 。

`RabbitTemplate` 和 `AsyncRabbitTemplate` 现在有 `receiveAndConvert` 和 `convertSendAndReceiveAsType` 方法，它们接受一个 `ParameterizedTypeReference<T>` 参数，让调用者指定将结果转换为何种类型。这对于复杂的类型或类型信息没有在消息 header 中传达的情况下特别有用。它需要一个 `SmartMessageConverter`，比如 `Jackson2JsonMessageConverter`。参见 [接收消息](https://springdoc.cn/spring-amqp/#receiving-messages)、[请求/回复消息](https://springdoc.cn/spring-amqp/#request-reply)、[Async Rabbit Template](https://springdoc.cn/spring-amqp/#async-template) 和 [使用 `RabbitTemplate` 从 `Message` 转换](https://springdoc.cn/spring-amqp/#json-complex) 获取更多信息。

你现在可以使用 `RabbitTemplate` 在一个专用 channel 上执行多个操作。请参阅 [Scope 操作](https://springdoc.cn/spring-amqp/#scoped-operations) 以了解更多信息。

##### Listener Adapter

一个方便的 `FunctionalInterface` 可用于在 `MessageListenerAdapter` 中使用 lambda。更多信息见 [`MessageListenerAdapter`](https://springdoc.cn/spring-amqp/#message-listener-adapter)。

##### Listener Container 的变化

###### Prefetch 默认值

prefetch 的默认值曾经是 1，这可能导致有效消费者的利用不足。现在的默认预取值是250，这应该使消费者在大多数常见情况下保持忙碌，从而提高吞吐量。

|      | 在有些情况下，prefetch 值应该很低—例如，对于大的消息，特别是在处理速度很慢的情况下（消息可能会在客户端进程中增加大量的内存），以及如果需要严格的消息排序（在这种情况下，prefetch 值应该被设回1）。另外，对于低容量的消息传递和多个消费者（包括单个监听器容器实例内的并发性），你可能希望减少 prefetch，以便在消费者之间获得更均匀的消息分布。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

有关 prefetch 的更多背景，请参阅这篇关于 [RabbitMQ 中消费者利用率](https://www.rabbitmq.com/blog/2014/04/14/finding-bottlenecks-with-rabbitmq-3-3/) 的文章和这篇关于 [队列理论](https://www.rabbitmq.com/blog/2012/05/11/some-queuing-theory-throughput-latency-and-bandwidth/) 的文章。

###### Message 数量

以前，`MessageProperties.getMessageCount()` 对于由容器发出的消息返回 `0`。该属性仅在你使用 `basicGet`（例如，从 `RabbitTemplate.receive()` 方法）时适用，并且现在对容器消息初始化为 `null`。

###### 事务回滚行为

现在，无论是否配置了事务管理器，事务回滚时的消息重新排队（入队列）都是一致的。更多信息见 [关于回滚收到的信息的说明](https://springdoc.cn/spring-amqp/#transaction-rollback)。

###### 关机的行为

如果容器线程在 `shutdownTimeout` 内没有对关闭做出反应，channel 就会被默认强制关闭。更多信息见 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes)。

###### 接收信息后的后处理程序

如果 `afterReceiveMessagePostProcessors` 属性中的 `MessagePostProcessor` 返回 `null`，那么该消息将被丢弃（如果合适的话将被确认）。

##### Connection Factory 的变化

连接和 channel listener 接口现在提供了一种机制来获取关于异常的信息。更多信息请参见 [连接和 Channel 监听器](https://springdoc.cn/spring-amqp/#connection-channel-listeners) 和 [异步发布 - 如何检测成功和失败](https://springdoc.cn/spring-amqp/#publishing-is-async)。

现在提供了一个新的 `ConnectionNameStrategy`，以便从 `AbstractConnectionFactory` 填充目标 RabbitMQ 连接的特定应用程序标识。请参阅 [连接和资源管理](https://springdoc.cn/spring-amqp/#connections) 以了解更多信息。

##### Retry 的变化

不再提供 `MissingMessageIdAdvice`。它的功能现在是内置的。更多信息见 [同步操作中的失败和重试的选项](https://springdoc.cn/spring-amqp/#retry)。

##### 匿名队列的命名

默认情况下， `AnonymousQueues` 现在使用默认的 `Base64UrlNamingStrategy` 来命名，而不是简单的 `UUID` 字符串。更多信息见 [`AnonymousQueue`](https://springdoc.cn/spring-amqp/#anonymous-queue)。

##### `@RabbitListener` 的变化

你现在可以在 `@RabbitListener` 注解中提供简单的队列声明（仅与默认 exchange 绑定）。更多信息请参见 [注解驱动的 Listener 端点](https://springdoc.cn/spring-amqp/#async-annotation-driven)。

你现在可以配置 `@RabbitListener` 注解，以便将任何异常返回给发送者。你还可以配置 `RabbitListenerErrorHandler` 来处理异常。请参阅 [处理异常](https://springdoc.cn/spring-amqp/#annotation-error-handling) 了解更多信息。

当你使用 `@QueueBinding` 注解时，你现在可以将一个队列与多个 routing key 绑定。另外 `@QueueBinding.exchange()` 现在支持自定义 exchange 类型，并默认声明 durable exchange。

你现在可以在注解层设置监听器容器的 `concurrency`，而不是为不同的 concurrency 设置配置不同的容器工厂。

现在你可以在注解层设置监听器容器的 `autoStartup` 属性，覆盖容器工厂的默认设置。

你现在可以在 `RabbitListener` 容器工厂中设置接收后和发送（回复）前的 `MessagePostProcessor` 实例。

更多信息见 [注解驱动的 Listener 端点](https://springdoc.cn/spring-amqp/#async-annotation-driven)。

从 2.0.3 版本开始，类级 `@RabbitListener` 上的 `@RabbitHandler` 注解之一可以被指定为默认。更多信息请参见 [多方法监听器](https://springdoc.cn/spring-amqp/#annotation-method-selection)。

##### 容器条件性回滚

当使用外部事务管理器（如JDBC）时，当你为容器提供事务属性时，现在支持基于规则的回滚。当你使用事务 advice 时，它现在也更加灵活。更多信息请参见 [条件性回滚](https://springdoc.cn/spring-amqp/#conditional-rollback)。

##### 移除对Jackson 1.x的支持

在以前的版本中被废弃，Jackson `1.x` 的 converter 和相关组件现在已经被删除。你可以使用基于Jackson 2.x的类似组件。更多信息见 [Jackson2JsonMessageConverter](https://springdoc.cn/spring-amqp/#json-message-converter)。

##### JSON Message Converter

当一个入站的 JSON 消息的 `*TypeId*` 被设置为 `Hashtable` 时，默认转换类型现在是 `LinkedHashMap`。以前，它是 `Hashtable`。要恢复到 `Hashtable`，你可以在 `DefaultClassMapper` 上使用 `setDefaultMapType`。

##### XML 解析器

在解析 `Queue` 和 `Exchange` XML 组件时，如果存在 `id` 属性，解析器不再将 `name` 属性值注册为 bean 别名。参见 [关于 `id` 和 `name` 属性的说明](https://springdoc.cn/spring-amqp/#note-id-name) 了解更多信息。

##### 阻塞的连接

你现在可以将 `com.rabbitmq.client.BlockedListener` 注入 `org.springframework.amqp.rabbit.connection.Connection` 对象。同时，当连接被 Broker 阻塞或解除阻塞时，`ConnectionFactory` 会发出 `ConnectionBlockedEvent` 和 `ConnectionUnblockedEvent` 事件。

更多信息见 [连接和资源管理](https://springdoc.cn/spring-amqp/#connections) 。

#### C.2.8. 1.7中自1.6以来的变化

##### AMQP Client 库

Spring AMQP 现在使用 RabbitMQ 团队提供的新 4.0.x 版本的 amqp-client 库。该客户端默认配置了自动恢复功能。参见 [RabbitMQ 自动连接/拓扑恢复](https://springdoc.cn/spring-amqp/#auto-recovery)。

|      | 4.0.x客户端默认启用自动恢复功能。虽然与此功能兼容，但Spring AMQP有自己的恢复机制，一般不需要客户端恢复功能。我们建议禁用 `amqp-client` 自动恢复功能，以避免在 broker 可用但连接尚未恢复时得到 `AutoRecoverConnectionNotCurrentlyOpenException` 实例。从 1.7.1 版开始，Spring AMQP 禁用它，除非你明确创建你自己的 RabbitMQ 连接工厂并将其提供给 `CachingConnectionFactory`。由 `RabbitConnectionFactoryBean` 创建的 RabbitMQ `ConnectionFactory` 实例也默认禁用该选项。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

##### Log4j 2 升级

Log4j 2的最低版本（用于 `AmqpAppender`）现在是2.7。该框架已不再与以前的版本兼容。更多信息见 [日志子系统 AMQP Appender](https://springdoc.cn/spring-amqp/#logging)。

##### Logback Appender

这个appender默认情况下不再捕获调用者数据（方法，行号）。你可以通过设置 `includeCallerData` 配置选项重新启用它。关于可用的日志附加程序的信息，请参见 [日志子系统 AMQP Appender](https://springdoc.cn/spring-amqp/#logging) 。

##### Spring Retry 升级

现在Spring Retry的最低版本是 `1.2`。该框架已不再与以前的版本兼容。

###### 停机行为

你现在可以将 `forceCloseChannel` 设置为 `true`，这样，如果容器线程在 `shutdownTimeout` 内没有响应关闭，channel 就会被强制关闭，导致任何未 acked 消息被重新排队（入队列）。更多信息请参见 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes)。

##### FasterXML Jackson 升级

现在Jackson的最低版本是 `2.8`。该框架已不再与以前的版本兼容。

##### JUnit `@Rules`

以前框架内部使用的 Rule 现在已经在一个单独的jar中提供了，叫做 `spring-rabbit-junit`。更多信息见 [JUnit4 `@Rules`](https://springdoc.cn/spring-amqp/#junit-rules) 。

##### 容器条件性回滚

当你使用外部事务管理器（比如JDBC）时，当你为容器提供事务属性时，现在支持基于规则的回滚。当你使用一个事务 advice 时，它现在也更加灵活。

##### 连接命名策略

现在提供了一个新的 `ConnectionNameStrategy`，以便从 `AbstractConnectionFactory` 填充目标 RabbitMQ 连接的特定应用程序标识。请参阅 [连接和资源管理](https://springdoc.cn/spring-amqp/#connections) 以了解更多信息。

##### Listener Container 的变化

###### 事务回滚行为

现在你可以将事务回滚时的消息重新排队（入队列）配置为一致的，不管是否配置了事务管理器。更多信息见 [关于回滚收到的信息的说明](https://springdoc.cn/spring-amqp/#transaction-rollback)。

#### C.2.9. 之前的发布

请看 [之前的发布](https://springdoc.cn/spring-amqp/#previous-whats-new)，了解以前版本的变化。

#### C.2.10. 1.6中自1.5以来的变化

##### 测试的支持

现在提供了一个新的测试支持库。更多信息见 [测试支持](https://springdoc.cn/spring-amqp/#testing)。

##### Builder

为配置 `Queue` 和 `Exchange` 对象提供 fluent API 的 Builder 现在可用。更多信息见 [Queue 和 Exchange 的 Builder API](https://springdoc.cn/spring-amqp/#builder-api)。

##### 命名空间的变化

###### Connection Factory

现在你可以在连接工厂Bean声明中添加一个 `thread-factory` --例如，命名由 `amqp-client` 库创建的线程。更多信息见 [连接和资源管理](https://springdoc.cn/spring-amqp/#connections) 。

当你使用 `CacheMode.CONNECTION` 时，你现在可以限制允许的总连接数。更多信息见 [连接和资源管理](https://springdoc.cn/spring-amqp/#connections) 。

###### Queue 的定义

你现在可以为匿名队列提供一个命名策略。更多信息见 [`AnonymousQueue`](https://springdoc.cn/spring-amqp/#anonymous-queue) 。

##### Listener Container 的变化

###### 闲置 Message Listener 检测

你现在可以配置监听器容器在空闲时发布 `ApplicationEvent` 实例。更多信息见 [检测闲置的异步消费者](https://springdoc.cn/spring-amqp/#idle-containers)。

###### 不匹配的队列检测

默认情况下，当监听器容器启动时，如果检测到属性或参数不匹配的队列，容器会记录异常但继续监听。容器现在有一个叫做 `mismatchedQueuesFatal` 的属性，如果在启动期间检测到这个问题，它可以阻止容器（和上下文）启动。如果后来检测到该问题，例如从连接失败中恢复后，它也会停止容器。更多信息请参见 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes) 。

###### Listener Container 日志

现在，监听器容器向内部的 `SimpleAsyncTaskExecutor` 提供它的 `beanName` 作为 `threadNamePrefix`。这对日志分析很有用。

###### 默认的错误处理程序

默认的错误处理程序（`ConditionalRejectingErrorHandler`）现在认为不可恢复的 `@RabbitListener` 异常是致命的。更多信息请参见 [异常处理](https://springdoc.cn/spring-amqp/#exception-handling) 。

##### `AutoDeclare` 和 `RabbitAdmin` 实例

请参阅 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes) (`autoDeclare`) 以了解该选项在应用程序上下文中使用 `RabbitAdmin` 实例方面的一些语义变化。

##### `AmqpTemplate`: 接收与超时

已为 `AmqpTemplate` 及其 `RabbitTemplate` 实现引入了许多新的带超时的 `receive()` 方法。参见 [消费者轮询](https://springdoc.cn/spring-amqp/#polling-consumer) 以了解更多信息。

##### 使用 `AsyncRabbitTemplate`

一个新的 `AsyncRabbitTemplate` 已经被引入。这个 template 提供了许多发送和接收方法，其中的返回值是一个 `ListenableFuture`，以后可以用来同步或异步地获得结果。更多信息见 [Async Rabbit Template](https://springdoc.cn/spring-amqp/#async-template)。

##### `RabbitTemplate` 的变化

1.4.1引入了在 broker 支持的情况下使用 [direct reply-to](https://www.rabbitmq.com/direct-reply-to.html) 的能力。它比为每个回复使用一个临时队列更有效。这个版本允许你通过设置 `useTemporaryReplyQueues` 属性为 `true` 来覆盖这个默认行为并使用临时队列。更多信息见 [RabbitMQ Direct reply-to](https://springdoc.cn/spring-amqp/#direct-reply-to) 。

`RabbitTemplate` 现在支持 `user-id-expression` （使用 Java 配置时为 `userIdExpression`）。请参阅 [验证的用户 ID RabbitMQ 文档](https://www.rabbitmq.com/validated-user-id.html) 和 [验证的 User Id](https://springdoc.cn/spring-amqp/#template-user-id) 以了解更多信息。

##### Message Properties

###### 使用 `CorrelationId`

`correlationId` 消息属性现在可以是一个 `String`。请参阅 [Message Properties 转换器](https://springdoc.cn/spring-amqp/#message-properties-converters) 了解更多信息。

###### 长字符串 Header

以前，`DefaultMessagePropertiesConverter` 将长于长字符串限制（默认为 1024）的header "转换" 为 `DataInputStream`（实际上，它引用了 `LongString` 实例的 `DataInputStream`）。在输出时，这个 header 没有被转换（除了转换为一个字符串—例如，通过在流上调用 `toString()`，`java.io.DataInputStream@1d057a39`）。

在这个版本中，长的 `LongString` 实例现在被默认为 `LongString` 实例。你可以通过使用 `getBytes[]`、`toString()` 或 `getStream(`) 方法访问其内容。一个大的传入的 `LongString` 现在也会在输出时正确 "转换"。

更多信息见 [Message Properties 转换器](https://springdoc.cn/spring-amqp/#message-properties-converters)。

###### Inbound Delivery Mode

`deliveryMode` 属性不再被映射到 `MessageProperties.deliveryMode`。这一变化避免了在同一 `MessageProperties` 对象被用于发送出站消息时的意外传播。相反，入站的 `deliveryMode` header 被映射到 `MessageProperties.receivedDeliveryMode`。

更多信息见 [Message Properties 转换器](https://springdoc.cn/spring-amqp/#message-properties-converters)。

当使用注解的端点时，该 header 在名为 `AmqpHeaders.RECEIVED_DELIVERY_MODE` 的 header 中提供。

更多信息见 [注解端点的方法签名](https://springdoc.cn/spring-amqp/#async-annotation-driven-enable-signature)。

###### Inbound User ID

`user_id` 属性不再被映射到 `MessageProperties.userId`。这一变化避免了在同一 `MessageProperties` 对象被用于发送出站消息时的意外传播。相反，入站的 `userId` header 被映射到 `MessageProperties.receivedUserId`。

更多信息见 [Message Properties 转换器](https://springdoc.cn/spring-amqp/#message-properties-converters)。

当你使用注解的端点时，在名为 `AmqpHeaders.RECEIVED_USER_ID` 的 header 中提供 header。

更多详细信息见 [注解端点的方法签名](https://springdoc.cn/spring-amqp/#async-annotation-driven-enable-signature)。

##### `RabbitAdmin` 的变化

###### 声明失败

以前，`ignoreDeclarationFailures` 标志只对 channel 上的 `IOException` 生效（比如错误匹配的参数）。现在它对任何异常（如 `TimeoutException`）都有效。此外，现在每当声明失败时都会发布一个 `DeclarationExceptionEvent`。`RabbitAdmin` 的最后一次声明事件也可作为 `lastDeclarationExceptionEvent` 的属性。请参阅 [配置 Broker](https://springdoc.cn/spring-amqp/#broker-configuration) 了解更多信息。

##### `@RabbitListener` 的变化

###### 每个 Bean 的多个容器

当你使用 Java 8 或更高版本时，你现在可以向 `@Bean` 类或其方法添加多个 `@RabbitListener` 注解。当使用Java 7或更早版本时，你可以使用 `@RabbitListeners` 容器注解来提供同样的功能。更多信息请参见 [`@Repeatable` `@RabbitListener`](https://springdoc.cn/spring-amqp/#repeatable-rabbit-listener)。

###### `@SendTo` SpEL 表达式

没有 `replyTo` 属性的路由回复的 `@SendTo` 现在可以是针对请求/回复评估的 SpEL 表达式。更多信息见 [回复管理](https://springdoc.cn/spring-amqp/#async-annotation-driven-reply)。

###### `@QueueBinding` 改进

现在你可以在 `@QueueBinding` 注解中为 queue、exchange 和 binding 指定参数。现在 `@QueueBinding` 支持 Header exchange。更多信息见 [注解驱动的 Listener 端点](https://springdoc.cn/spring-amqp/#async-annotation-driven)。

##### Delayed Message Exchange

Spring AMQP 现在对 RabbitMQ 延迟消息 Exchange 插件有了一流的支持。请参阅 [延迟消息 Exchange](https://springdoc.cn/spring-amqp/#delayed-message-exchange) 了解更多信息。

##### Exchange Internal 标识

任何 `Exchange` 定义现在都可以标记为 `internal`，并且 `RabbitAdmin` 在声明 `Exchange` 时将该值传递给 broker。请参阅 [配置 Broker](https://springdoc.cn/spring-amqp/#broker-configuration) 以了解更多信息。

##### `CachingConnectionFactory` 的变化

###### `CachingConnectionFactory` 缓存统计

`CachingConnectionFactory` 现在在运行时和通过JMX提供缓存属性。更多信息见 [运行时缓存属性](https://springdoc.cn/spring-amqp/#runtime-cache-properties)。

###### 访问底层 RabbitMQ 连接工厂

增加了一个新的 getter 来提供对底层工厂的访问。你可以使用这个 getter，例如，添加自定义连接属性。更多信息见 [添加自定义的客户端连接属性](https://springdoc.cn/spring-amqp/#custom-client-props)。

###### Channel 缓存

默认的 channel 缓存大小已经从1增加到25。更多信息见 [连接和资源管理](https://springdoc.cn/spring-amqp/#connections)。

此外，`SimpleMessageListenerContainer` 不再调整缓存大小，使其至少与 `concurrentConsumers` 一样大—这是多余的，因为容器消费者 channel 从未被缓存过。

##### 使用 `RabbitConnectionFactoryBean`

工厂Bean现在暴露了一个属性，用于将客户端连接属性添加到由结果工厂建立的连接中。

##### Java 反序列化

当你使用Java反序列化时，你现在可以配置一个允许类的 "允许列表"（“allowed list”）。如果你接受来自不受信任的来源的带有序列化的java对象的消息，你应该考虑创建一个允许列表。更多信息请参见 [Java 反序列化](https://springdoc.cn/spring-amqp/#java-deserialization)。

##### JSON `MessageConverter`

对 JSON 消息 converter 的改进现在允许消费那些在消息 header 中没有类型信息的消息。更多信息请参见 [注解方法的消息转换](https://springdoc.cn/spring-amqp/#async-annotation-conversion) 和 [Jackson2JsonMessageConverter](https://springdoc.cn/spring-amqp/#json-message-converter)。

##### Logging Appender

###### Log4j 2

增加了一个log4j 2 appender，现在可以用 `addresses` 属性来配置appender，以连接到一个 broker 集群。

###### 客户端连接属性

你现在可以向 RabbitMQ 连接添加自定义客户端连接属性。

更多详细信息见 [日志子系统 AMQP Appender](https://springdoc.cn/spring-amqp/#logging) 。

#### C.2.11. 1.5中自1.4以来的变化

##### `spring-erlang` 不再被支持

`spring-erlang` jar 已不再包含在发行版中。请使用 [RabbitMQ REST API](https://springdoc.cn/spring-amqp/#management-rest-api) 代替。

##### `CachingConnectionFactory` 的变化

###### `CachingConnectionFactory` 中的空 Addresses 属性

以前，如果连接工厂配置了一个 host 和 port，但也为 `addresses` 提供了一个空的字符串，那么主机和端口会被忽略。现在，一个空的 `addresses` 字符串被视为与 `null` 相同，并且使用 host 和 port。

###### URI 构造函数

`CachingConnectionFactory` 有一个额外的构造函数，有一个 `URI` 参数，用于配置 broker 连接。

###### Connection Reset

增加了一个名为 `resetConnection()` 的新方法，让用户重置连接（或连接）。你可以使用这个方法，例如，在故障转移到辅助 broker 后重新连接到主 broker。这确实会影响进程中的操作。现有的 `destroy()` 方法做得完全一样，但新的方法有一个不太令人生畏的名字。

##### 控制容器队列声明行为的属性

当监听器容器消费者启动时，他们试图被动地声明队列，以确保它们在 broker 上是可用的。以前，如果这些声明失败（例如，因为队列不存在）或当 HA（高可用） 队列被移动时，重试逻辑被固定为以五秒钟的间隔进行三次重试。如果队列仍然不存在，行为由 `missingQueuesFatal` 属性控制（默认：`true`）。另外，对于配置为从多个队列监听的容器，如果只有一个子集的队列可用，消费者会以60秒的固定间隔重试缺少的队列。

`declarationRetries`、`failedDeclarationRetryInterval` 和 `retryDeclarationInterval` 属性现在是可配置的。更多信息请参见 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes)。

##### Class Package 的变化

`RabbitGatewaySupport` 类已从 `o.s.amqp.rabbit.core.support` 移到 `o.s.amqp.rabbit.core`。

##### `DefaultMessagePropertiesConverter` 的变化

你现在可以配置 `DefaultMessagePropertiesConverter` 来确定被转换为 `String` 而不是 `DataInputStream` 的 `LongString` 的最大长度。该转换器有一个替代的构造函数，将该值作为一个限制。以前，这个限制被硬编码为 `1024` 字节。(在1.4.4中也可用)。

##### `@RabbitListener` 改进

###### 为 `@RabbitListener` 提供的 `@QueueBinding`

`bindings` 属性已被添加到 `@RabbitListener` 注解中，作为与 `queues` 属性的互斥，以允许指定 `queues`、其 `exchange` 和 `binding`，以便由 Broker 上的 `RabbitAdmin` 声明。

###### `@SendTo` 中的 SpEL

`@RabbitListener` 的默认回复地址（`@SendTo`）现在可以是一个SpEL表达式。

###### 通过配置文件指定多个队列

现在你可以使用 SpEL 和属性占位符的组合来为一个 listener 指定多个队列。

更多信息见 [注解驱动的 Listener 端点](https://springdoc.cn/spring-amqp/#async-annotation-driven)。

##### Exchange， Queue 和 Binding 的自动声明

现在你可以声明定义这些实体集合的 Bean，`RabbitAdmin` 会将这些内容添加到它在建立连接时声明的实体列表中。请参阅 [声明 Exchange、Queue 和 Binding 的集合](https://springdoc.cn/spring-amqp/#collection-declaration) 以了解更多信息。

##### `RabbitTemplate` 的变化

###### 添加了 `reply-address`

`reply-address` 属性已被添加到 `<rabbit-template>` 组件中，作为一种替代的 `reply-queue`。参见 [请求/回复消息](https://springdoc.cn/spring-amqp/#request-reply) 以了解更多信息。（在 1.4.4 中也可作为 `RabbitTemplate` 上的一个 setter 使用）。

###### 阻塞 `receive` 方法

`RabbitTemplate` 现在支持在 `receive` 和 `convertAndReceive` 方法中进行阻塞。请参阅 [消费者轮询](https://springdoc.cn/spring-amqp/#polling-consumer) 了解更多信息。

###### 必须使用 `sendAndReceive` 方法

当使用 `sendAndReceive` 和 `convertSendAndReceive` 方法时，`mandatory` 标志被设置，如果请求消息不能被传递，调用线程会抛出 `AmqpMessageReturnedException`。更多信息见 [回复超时](https://springdoc.cn/spring-amqp/#reply-timeout)。

###### 不正确的 Reply Listener 配置

当使用命名的回复队列时，该框架会尝试验证 Reply Listener 容器的正确配置。

更多信息见 [回复 Listener 容器](https://springdoc.cn/spring-amqp/#reply-listener)。

##### 添加了 `RabbitManagementTemplate`

`RabbitManagementTemplate` 已被引入，以通过使用其 [management plugin](https://www.rabbitmq.com/management.html) 提供的 REST API 来监控和配置 RabbitMQ Broker。请参阅 [RabbitMQ REST API](https://springdoc.cn/spring-amqp/#management-rest-api) 了解更多信息。

##### Listener Container Bean Name (XML)

|      | `<listener-container/>` 元素上的 `id` 属性已被删除。从这个版本开始，`<listener/>` 子元素上的 `id` 被单独用来命名为每个监听器元素创建的监听器容器bean。正常的 Spring Bean 名称覆盖将被应用。如果后来的 `<listener/>` 被解析出与现有Bean相同的 `id`，那么新的定义将覆盖现有的定义。以前，Bean 的名字是由 `<listener-container/>` 和 `<listener/>` 元素的 `id` 属性组成。当迁移到这个版本时，如果你的 `<listener-container/>` 元素上有 `id` 属性，请删除它们并在子 `<listener/>` 元素上设置 `id`。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

然而，为了支持将容器作为一个组（group）来启动和停止，我们增加了一个新的 `group` 属性。当这个属性被定义时，由这个元素创建的容器被添加到一个这个名字的Bean中，其类型为 `Collection<SimpleMessageListenerContainer>`。你可以遍历这个组来启动和停止容器。

##### 类级别的 `@RabbitListener`

`@RabbitListener` 注解现在可以在类的层面上应用。与新的 `@RabbitHandler` 方法注解一起，这可以让你根据 payload 类型来选择处理方法。更多信息请参见 [多方法监听器](https://springdoc.cn/spring-amqp/#annotation-method-selection)。

##### `SimpleMessageListenerContainer`: BackOff 支持

`SimpleMessageListenerContainer` 现在可以提供一个 `BackOff` 实例用于 `consumer` 启动恢复。更多信息见 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes)。

##### Channel Close 日志

引入了一个控制 channel 关闭的日志级别的机制。参见 [记录 Channel 关闭事件](https://springdoc.cn/spring-amqp/#channel-close-logging)。

##### Application Event

`SimpleMessageListenerContainer` 现在会在消费者失败时发出 Application Event。更多信息见 [消费者事件](https://springdoc.cn/spring-amqp/#consumer-events) 。

##### Consumer Tag 配置

以前，异步消费者的消费者标签（Consumer Tag）是由 broker 生成的。在这个版本中，现在可以向监听器容器提供一个命名策略。参见 [消费者标签（Tag）](https://springdoc.cn/spring-amqp/#consumerTags)。

##### 使用 `MessageListenerAdapter`

`MessageListenerAdapter` 现在支持队列名称（或消费者标签）到方法名称的映射，以确定根据接收消息的队列来调用哪个委托方法。

##### 添加了 `LocalizedQueueConnectionFactory`

`LocalizedQueueConnectionFactory` 是一个新的连接工厂，可以连接到镜像队列实际所在的集群中的节点。

见 [Queue 亲和性和 `LocalizedQueueConnectionFactory`](https://springdoc.cn/spring-amqp/#queue-affinity)。

##### 匿名队列的命名

从1.5.3版本开始，你现在可以控制 `AnonymousQueue` 名称的生成方式。参见 [`AnonymousQueue`](https://springdoc.cn/spring-amqp/#anonymous-queue) 获取更多信息。

#### C.2.12. 1.4中自1.3以来的变化

##### `@RabbitListener` 注解

POJO监听器可以用 `@RabbitListener` 注解，通过 `@EnableRabbit` 或 `<rabbit:annotation-driven />` 启用。该功能需要 Spring Framework 4.1。更多信息请参见 [注解驱动的 Listener 端点](https://springdoc.cn/spring-amqp/#async-annotation-driven) 。

##### 添加了 `RabbitMessagingTemplate`

一个新的 `RabbitMessagingTemplate` 让你通过使用 `spring-messaging` `Message` 实例与 RabbitMQ 进行交互。在内部，它使用 `RabbitTemplate`，你可以像平常一样配置它。此功能需要 Spring Framework 4.1。请参阅 [Messaging 整合](https://springdoc.cn/spring-amqp/#template-messaging) 了解更多信息。

##### Listener Container `missingQueuesFatal` 属性

1.3.5在 `SimpleMessageListenerContainer` 上引入了 `missingQueuesFatal` 属性。现在这在 listener container 命名空间元素上可用。参见 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes)。

##### RabbitTemplate `ConfirmCallback` 接口

这个接口上的 `confirm` 方法有一个额外的参数，叫做 `cause`。如果有的话，这个参数包含了否定确认（nack）的原因。参见 [相关的 Publisher（发布者）确认和返回](https://springdoc.cn/spring-amqp/#template-confirms)。

##### 添加了 `RabbitConnectionFactoryBean`

`RabbitConnectionFactoryBean` 创建了由 `CachingConnectionFactory` 使用的底层 RabbitMQ `ConnectionFactory`。这样就可以使用 Spring 的依赖注入配置 SSL 选项。参见 [配置底层客户端连接工厂（Connection Factory）](https://springdoc.cn/spring-amqp/#connection-factory)。

##### 使用 `CachingConnectionFactory`

`CachingConnectionFactory` 现在允许将 `connectionTimeout` 设置为一个属性或命名空间中的一个属性。它在底层 RabbitMQ `ConnectionFactory` 上设置该属性。参见 [配置底层客户端连接工厂（Connection Factory）](https://springdoc.cn/spring-amqp/#connection-factory)。

##### Log Appender

引入了 Logback `org.springframework.amqp.rabbit.logback.AmqpAppender`。它提供与 `org.springframework.amqp.rabbit.log4j.AmqpAppender` 类似的选项。欲了解更多信息，请参见这些类的JavaDoc。

Log4j `AmqpAppender` 现在支持 `deliveryMode` 属性（`PERSISTENT` 或 `NON_PERSISTENT`，默认：`PERSISTENT`）。以前，所有的log4j消息都是 `PERSISTENT`。

Appender 还支持在发送前修改 `Message` --例如，允许添加自定义 header 信息。子类应该覆写 `postProcessMessageBeforeSend()`。

##### Listener Queue

listener container 现在默认会在启动期间重新声明任何缺失的队列。在 `<rabbit:listener-container>` 中添加了一个新的 `auto-declare` 属性以防止这些重新声明。参见 [`auto-delete` 队列](https://springdoc.cn/spring-amqp/#lc-auto-delete)。

##### `RabbitTemplate`: `mandatory` 和 `connectionFactorySelector` 表达式

`mandatoryExpression`、`sendConnectionFactorySelectorExpression` 和 `receiveConnectionFactorySelectorExpression` SpEL Expression 属性已被添加到 `RabbitTemplate`。`mandatoryExpression` 用于在使用 `ReturnCallback` 时针对每个请求消息评估一个 `mandatory` 布尔值。请参见 [相关的 Publisher（发布者）确认和返回](https://springdoc.cn/spring-amqp/#template-confirms)。 `sendConnectionFactorySelectorExpression` 和 `receiveConnectionFactorySelectorExpression` 在提供 `AbstractRoutingConnectionFactory` 时使用，以便在每个 AMQP 协议交互操作的运行时确定目标 `ConnectionFactory` 的 `lookupKey`。参见 [路由 Connection Factory](https://springdoc.cn/spring-amqp/#routing-connection-factory)。

##### Listener 和 Routing Connection Factory

你可以用路由连接工厂来配置 `SimpleMessageListenerContainer`，以实现基于队列名称的连接选择。参见 [路由 Connection Factory](https://springdoc.cn/spring-amqp/#routing-connection-factory)。

##### `RabbitTemplate`: `RecoveryCallback` 选项

增加了 `recoveryCallback` 属性，以便在 `retryTemplate.execute()` 中使用。参见 [添加重试能力](https://springdoc.cn/spring-amqp/#template-retry)。

##### `MessageConversionException` 的变化

这个异常现在是 `AmqpException` 的一个子类。考虑一下下面的代码：

```java
try {
    template.convertAndSend("thing1", "thing2", "cat");
}
catch (AmqpException e) {
	...
}
catch (MessageConversionException e) {
	...
}
```

第二个 catch 块已经无法到达，需要移到所有 `AmqpException` catch 块的上方。

##### RabbitMQ 3.4 兼容性

Spring AMQP 现在与 RabbitMQ 3.4 兼容，包括 direct reply-to。请参阅 [兼容性](https://springdoc.cn/spring-amqp/#compatibility) 和 [RabbitMQ Direct reply-to](https://springdoc.cn/spring-amqp/#direct-reply-to) 获取更多信息。

##### 添加了 `ContentTypeDelegatingMessageConverter`

引入了 `ContentTypeDelegatingMessageConverter`，根据 `MessageProperties` 中的 `contentType` 属性，选择要使用的 `MessageConverter`。更多信息见 [消息转换器](https://springdoc.cn/spring-amqp/#message-converters)。

#### C.2.13. 1.3中自1.2以来的变化

##### Listener 并发

listener container 现在支持根据工作量动态扩展消费者的数量，或者你可以在不停止容器的情况下以编程方式改变并发性。参见 [并发的 Listener](https://springdoc.cn/spring-amqp/#listener-concurrency)。

##### Listener Queue

监听器容器现在允许在运行时修改它所监听的队列。此外，如果至少有一个配置的队列可供使用，该容器就会启动。参见 [监听器容器队列](https://springdoc.cn/spring-amqp/#listener-queues)。

这个监听器容器现在会在启动时重新声明任何自动删除队列。参见 [`auto-delete` 队列](https://springdoc.cn/spring-amqp/#lc-auto-delete)。

##### Consumer 优先级

listener container 现在支持消费者参数，让 `x-priority` 参数被设置。参见 [消费者优先级](https://springdoc.cn/spring-amqp/#consumer-priority)。

##### 独占 Consumer

你现在可以将 `SimpleMessageListenerContainer` 配置为单一的 `exclusive` 消费者，防止其他消费者监听队列。参见 [独占的消费者](https://springdoc.cn/spring-amqp/#exclusive-consumer)。

##### Rabbit Admin

你现在可以让 broker 生成队列名称，而不考虑 `durable`、`autoDelete` 和 `exclusive` 设置。参见 [配置 Broker](https://springdoc.cn/spring-amqp/#broker-configuration)。

##### Direct Exchange Binding

以前，从 `direct-exchange` 配置的 `binding` 元素中省略 `key` 属性会导致 queue 或 exchange 被绑定为一个空字符串作为 routing key。现在，它将与所提供的 `Queue` 或 `Exchange` 的名称绑定。如果你想用一个空字符串的 routing key 进行绑定，你需要指定 `key=""`。

##### `AmqpTemplate` 的变化

`AmqpTemplate` 现在提供了几个同步的 `receiveAndReply` 方法。这些是由 `RabbitTemplate` 实现的。有关详细信息，请参见 [接收消息](https://springdoc.cn/spring-amqp/#receiving-messages)。

`RabbitTemplate` 现在支持配置 `RetryTemplate`，以便在 broker 不可用时尝试重试（带有可选的 back-off 策略）。有关详细信息，请参阅 [添加重试能力](https://springdoc.cn/spring-amqp/#template-retry)。

##### 缓存 Connection Factory

你现在可以配置缓存连接工厂来缓存 `Connection` 实例和它们的 `Channel` 实例，而不是使用单一的连接并只缓存 `Channel` 实例。参见 [连接和资源管理](https://springdoc.cn/spring-amqp/#connections)。

##### Binding 参数

`<exchange>` 的 `<binding>` 现在支持 `<binding-arguments>` 子元素的解析。你现在可以用一个 `key/value` 属性对来配置 `<headers-exchange>` 的 `<binding>`（在一个头上匹配）或用一个 `<binding-arguments>` 子元素（允许在多个 header 上匹配）。这些选项是相互排斥的。参见 [Headers Exchange](https://springdoc.cn/spring-amqp/#headers-exchange).。

##### 路由 Connection Factory

一个新的 `SimpleRoutingConnectionFactory` 已经被引入。它允许配置 `ConnectionFactories` 的映射，以确定在运行时使用的目标 `ConnectionFactory`。参见 [路由 Connection Factory](https://springdoc.cn/spring-amqp/#routing-connection-factory)。

##### `MessageBuilder` 和 `MessagePropertiesBuilder`

现在提供了用于构建消息或消息属性的 “Fluent API” 。参见 [Message Builder API](https://springdoc.cn/spring-amqp/#message-builder)。

##### `RetryInterceptorBuilder` 的变化

现在提供了一个用于构建监听器容器重试拦截器的 “Fluent API” 。参见 [同步操作中的失败和重试的选项](https://springdoc.cn/spring-amqp/#retry)。

##### 添加了 `RepublishMessageRecoverer`

提供这个新的 `MessageRecoverer` 是为了允许在重试用尽时将失败的消息发布到另一个队列（包括 header 中的堆栈跟踪信息）。参见 [消息监听器和异步案例](https://springdoc.cn/spring-amqp/#async-listeners)。

##### 默认的错误处理程序（从1.3.2开始）

一个默认的 `ConditionalRejectingErrorHandler` 已经被添加到监听器容器中。这个错误处理程序检测到致命的消息转换问题，并指示容器拒绝该消息，以防止 broker 不断地重新传递不可转换的消息。参见 [异常处理](https://springdoc.cn/spring-amqp/#exception-handling)。

##### Listener Container `missingQueuesFatal` 属性 （自1.3.5起）

`SimpleMessageListenerContainer` 现在有一个名为 `missingQueuesFatal` 的属性（默认：`true`）。以前，丢失队列总是致命的。参见 [消息监听器容器配置](https://springdoc.cn/spring-amqp/#containerAttributes)。

#### C.2.14. 1.2中自1.1以来的变化

##### RabbitMQ 版本

Spring AMQP 现在默认使用 RabbitMQ 3.1.x（但保留了与早期版本的兼容性）。对于 RabbitMQ 3.1.x 不再支持的功能（federated exchange 和 `RabbitTemplate` 上的 `immediate` 属性），已经添加了某些弃用。

##### Rabbit Admin

`RabbitAdmin` 现在提供了一个选项，以便在声明失败时让 exchange、queue 和 binding 声明继续进行。以前，所有声明都在失败时停止。通过设置 `ignore-declaration-exceptions`，此类异常将被记录下来（在 `WARN` 级别），但进一步的声明将继续进行。一个可能有用的例子是，当一个队列声明失败时，因为一个稍微不同的 `ttl` 设置，通常会阻止其他声明的进行。

`RabbitAdmin` 现在提供了一个名为 `getQueueProperties()` 的额外方法。你可以使用它来确定 broker 上是否存在队列（对于不存在的队列返回 `null`）。此外，它还返回队列中的当前消息数以及当前消费者数。

##### Rabbit Template

以前，当 `…sendAndReceive()` 方法与固定的回复队列一起使用时，两个自定义 header 信息被用于相关数据以及保留和恢复回复队列信息。在这个版本中，标准的消息属性（`correlationId`）被默认使用，尽管你可以指定一个自定义的属性来代替使用。此外，嵌套的 `replyTo` 信息现在被保留在 template 内部，而不是使用一个自定义 header。

`immediate` 属性已被弃用。在使用 RabbitMQ 3.0.x 或更高版本时，你不得设置此属性。

##### JSON Message Converter

现在提供了一个 Jackson 2.x `MessageConverter`，以及使用Jackson 1.x的现有转换器。

##### 自动声明 Queue 和其他项目

以前，在声明 queue、exchange 和 binding 时，你无法定义哪个连接工厂被用于声明。每个 `RabbitAdmin` 通过使用其连接来声明所有组件。

从这个版本开始，你现在可以将声明限制在特定的 `RabbitAdmin` 实例中。请参阅 [条件式声明](https://springdoc.cn/spring-amqp/#conditional-declaration)。

##### AMQP 远程

现在提供了使用 Spring 远程技术的设施，使用AMQP作为RPC调用的传输。更多信息见 [AMQP 和 Spring Remoting](https://springdoc.cn/spring-amqp/#remoting)。

##### 请求心跳

一些用户要求在 Spring AMQP `CachingConnectionFactory` 上公开底层客户端连接工厂的 `requestedHeartBeats` 属性。这一点现在可以实现了。以前，有必要将 AMQP 客户端工厂配置为一个单独的Bean，并在 `CachingConnectionFactory` 中提供对它的引用。

#### C.2.15. 1.1中自1.0以来的变化

##### 一般

Spring-AMQP现在是用Gradle构建的。

增加了对发布者确认和返回的支持。

增加了对HA队列和代理故障转移的支持。

增加了对死信 exchange 和死信队列的支持。

##### AMQP Log4j Appender

增加一个选项，支持在记录的消息中添加一个消息ID。

增加一个选项，允许在将 `String` 转换为 `byte[]` 时指定使用 `Charset` 名称。

Version 3.0.4-SNAPSHOT
Last updated 2023-06-05 21:26:23 +0800

[主页](https://springdoc.cn/docs/)