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

## 2. 简单实战

### 简单集成

#### 引入依赖

```xml
<dependency>
  <groupId>org.springframework.kafka</groupId>
  <artifactId>spring-kafka</artifactId>
  <version>2.2.6.RELEASE</version>
</dependency>
```

#### 添加配置

```
spring.kafka.producer.bootstrap-servers=192.168.31.16:9092
```

#### 测试发送和接收

```java
/**
 * @author: kl @kailing.pub
 * @date: 2019/5/30
 */
@SpringBootApplication
@RestController
public class Application {

	private final Logger logger = LoggerFactory.getLogger(Application.class);

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}

	@Autowired
	private KafkaTemplate<Object, Object> template;

	@GetMapping("/send/{input}")
	public void sendFoo(@PathVariable String input) {
		this.template.send("topic_input", input);
	}
	@KafkaListener(id = "webGroup", topics = "topic_input")
	public void listen(String input) {
		logger.info("input value: {}" , input);
	}
}
```

启动应用后，在浏览器中输入：http://localhost:8080/send/kl。就可以在控制台看到有日志输出了：input value: "kl"。基础的使用就这么简单。发送消息时注入一个 KafkaTemplate，接收消息时添加一个 @KafkaListener 注解即可。



#### Spring-kafka-test 嵌入式 Kafka Server

不过上面的代码能够启动成功，前提是你已经有了 Kafka Server 的服务环境，我们知道 Kafka 是由 Scala + Zookeeper 构建的，可以从官网下载部署包在本地部署。但是，我想告诉你，为了简化开发环节验证 Kafka 相关功能，Spring-Kafka-Test 已经封装了 Kafka-test 提供了注解式的一键开启 Kafka Server 的功能，使用起来也是超级简单。本文后面的所有测试用例的 Kafka 都是使用这种嵌入式服务提供的。



#### 引入依赖

```xml
<dependency>
   <groupId>org.springframework.kafka</groupId>
   <artifactId>spring-kafka-test</artifactId>
   <version>2.2.6.RELEASE</version>
   <scope>test</scope>
</dependency>
```



#### 启动服务

下面使用 Junit 测试用例，直接启动一个 Kafka Server 服务，包含四个 Broker 节点。

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = ApplicationTests.class)
@EmbeddedKafka(count = 4,ports = {9092,9093,9094,9095})
public class ApplicationTests {
	@Test
	public void contextLoads()throws IOException {
		System.in.read();
	}
}
```

如上：只需要一个注解 @EmbeddedKafka 即可，就可以启动一个功能完整的 Kafka 服务，是不是很酷。默认只写注解不加参数的情况下，是创建一个随机端口的 Broker，在启动的日志中会输出具体的端口以及默认的一些配置项。不过这些我们在 Kafka 安装包配置文件中的配置项，在注解参数中都可以配置，下面详解下 @EmbeddedKafka 注解中的可设置参数 ：

- value：broker 节点数量
- count：同 value 作用一样，也是配置的 broker 的节点数量
- controlledShutdown：控制关闭开关，主要用来在 Broker 意外关闭时减少此 Broker 上 Partition 的不可用时间

   Kafka 是多 Broker 架构的高可用服务，一个 Topic 对应多个 partition，一个 Partition 可以有多个副本 Replication，这些 Replication 副本保存在多个 Broker，用于高可用。但是，虽然存在多个分区副本集，当前工作副本集却只有一个，默认就是首次分配的副本集【首选副本】为 Leader，负责写入和读取数据。当我们升级 Broker 或者更新 Broker 配置时需要重启服务，这个时候需要将 partition 转移到可用的 Broker。下面涉及到三种情况

1.   直接关闭 Broker：当 Broker 关闭时，Broker 集群会重新进行选主操作，选出一个新的 Broker 来作为 Partition Leader，选举时此 Broker 上的 Partition 会短时不可用
2.  开启 controlledShutdown：当 Broker 关闭时，Broker 本身会先尝试将 Leader 角色转移到其他可用的 Broker 上
3.  使用命令行工具：使用 bin/kafka-preferred-replica-election.sh，手动触发 PartitionLeader 角色转移

- ports：端口列表，是一个数组。对应了 count 参数，有几个 Broker，就要对应几个端口号
- brokerProperties：Broker 参数设置，是一个数组结构，支持如下方式进行 Broker 参数设置：

```java
@EmbeddedKafka(brokerProperties = {"log.index.interval.bytes = 4096","num.io.threads = 8"})
```

- okerPropertiesLocation：Broker 参数文件设置

 功能同上面的 brokerProperties，只是 Kafka Broker 的可设置参数达 182 个之多，都像上面这样配置肯定不是最优方案，所以提供了加载本地配置文件的功能，如：

```java
@EmbeddedKafka(brokerPropertiesLocation = "classpath:application.properties")
```



### 创建新的 Topic

默认情况下，如果在使用 KafkaTemplate 发送消息时，Topic 不存在，会创建一个新的 Topic，默认的分区数和副本数为如下 Broker 参数来设定

```
num.partitions = 1 #默认Topic分区数
num.replica.fetchers = 1 #默认副本数
```



#### 程序启动时创建 Topic

```java
/**
 * @author: kl @kailing.pub
 * @date: 2019/5/31
 */
@Configuration
public class KafkaConfig {
	@Bean
	public KafkaAdmin admin(KafkaProperties properties){
		KafkaAdmin admin = new KafkaAdmin(properties.buildAdminProperties());
		admin.setFatalIfBrokerNotAvailable(true);
		return admin;
	}
	@Bean
	public NewTopic topic2() {
		return new NewTopic("topic-kl", 1, (short) 1);
	}
}
```

如果 Kafka Broker 支持（1.0.0 或更高版本），则如果发现现有 Topic 的 Partition 数少于设置的 Partition 数，则会新增新的 Partition 分区。关于 KafkaAdmin 有几个常用的用法如下：

setFatalIfBrokerNotAvailable (true)：默认这个值是 False 的，在 Broker 不可用时，不影响 Spring 上下文的初始化。如果你觉得 Broker 不可用影响正常业务需要显示的将这个值设置为 True

setAutoCreate (false) : 默认值为 True，也就是 Kafka 实例化后会自动创建已经实例化的 NewTopic 对象

initialize ()：当 setAutoCreate 为 false 时，需要我们程序显示的调用 admin 的 initialize () 方法来初始化 NewTopic 对象



#### 代码逻辑中创建

有时候我们在程序启动时并不知道某个 Topic 需要多少 Partition 数合适，但是又不能一股脑的直接使用 Broker 的默认设置，这个时候就需要使用 Kafka-Client 自带的 AdminClient 来进行处理。上面的 Spring 封装的 KafkaAdmin 也是使用的 AdminClient 来处理的。如：

```java
	@Autowired
	private KafkaProperties properties;
	@Test
	public void testCreateToipc(){
		AdminClient client = AdminClient.create(properties.buildAdminProperties());
		if(client !=null){
			try {
				Collection<NewTopic> newTopics = new ArrayList<>(1);
				newTopics.add(new NewTopic("topic-kl",1,(short) 1));
				client.createTopics(newTopics);
			}catch (Throwable e){
				e.printStackTrace();
			}finally {
				client.close();
			}
		}
	}
```



#### ps: 其他的方式创建 Topic

上面的这些创建 Topic 方式前提是你的 spring boot 版本到 2.x 以上了，因为 spring-kafka2.x 版本只支持 spring boot2.x 的版本。在 1.x 的版本中还没有这些 api。下面补充一种在程序中通过 Kafka_2.10 创建 Topic 的方式



##### 引入依赖

```xml
        <dependency>
            <groupId>org.apache.kafka</groupId>
            <artifactId>kafka_2.10</artifactId>
            <version>0.8.2.2</version>
        </dependency>
```



##### api 方式创建

```java
	@Test
	public void testCreateTopic()throws Exception{
		ZkClient zkClient =new ZkClient("192.168.31.16:2181", 3000, 3000, ZKStringSerializer$.MODULE$)
		String topicName = "topic-kl";
		int partitions = 1;
		int replication = 1;
		AdminUtils.createTopic(zkClient,topicName,partitions,replication,new Properties());
	}
```

注意下 ZkClient 最后一个构造入参，是一个序列化反序列化的接口实现，博主测试如果不填的话，创建的 Topic 在 ZK 上的数据是有问题的，默认的 Kafka 实现也很简单，就是做了字符串 UTF-8 编码处理。ZKStringSerializer$ 是 Kafka 中已经实现好的一个接口实例，是一个 Scala 的伴生对象，在 Java 中直接调用点 MODULE$ 就可以得到一个实例



##### 命令方式创建

```java
	@Test
	public void testCreateTopic(){
		String [] options= new String[]{
				"--create",
				"--zookeeper","192.168.31.16:2181",
				"--replication-factor", "3",
				"--partitions", "3",
				"--topic", "topic-kl"
		};
		TopicCommand.main(options);
	}
```



### 消息发送之 KafkaTemplate 探秘



#### 异步获取

```java
		template.send("","").addCallback(new ListenableFutureCallback<SendResult<Object, Object>>() {
			@Override
			public void onFailure(Throwable throwable) {
				......
			}

			@Override
			public void onSuccess(SendResult<Object, Object> objectObjectSendResult) {
				....
			}
		});
```



#### 同步获取

```java
		ListenableFuture<SendResult<Object,Object>> future = template.send("topic-kl","kl");
		try {
			SendResult<Object,Object> result = future.get();
		}catch (Throwable e){
			e.printStackTrace();
		}
```



#### kafka 事务消息

默认情况下，Spring-kafka 自动生成的 KafkaTemplate 实例，是不具有事务消息发送能力的。需要使用如下配置激活事务特性。事务激活后，所有的消息发送只能在发生事务的方法内执行了，不然就会抛一个没有事务交易的异常

```java
spring.kafka.producer.transaction-id-prefix=kafka_tx.
```

当发送消息有事务要求时，比如，当所有消息发送成功才算成功，如下面的例子：假设第一条消费发送后，在发第二条消息前出现了异常，那么第一条已经发送的消息也会回滚。而且正常情况下，假设在消息一发送后休眠一段时间，在发送第二条消息，消费端也只有在事务方法执行完成后才会接收到消息

```java
	@GetMapping("/send/{input}")
	public void sendFoo(@PathVariable String input) {
		template.executeInTransaction(t ->{
			t.send("topic_input","kl");
			if("error".equals(input)){
				throw new RuntimeException("failed");
			}
			t.send("topic_input","ckl");
			return true;
		});
	}
```

当事务特性激活时，同样，在方法上面加 @Transactional 注解也会生效

```java
	@GetMapping("/send/{input}")
	@Transactional(rollbackFor = RuntimeException.class)
	public void sendFoo(@PathVariable String input) {
		template.send("topic_input", "kl");
		if ("error".equals(input)) {
			throw new RuntimeException("failed");
		}
		template.send("topic_input", "ckl");
	}
```

Spring-Kafka 的事务消息是基于 Kafka 提供的事务消息功能的。而 Kafka Broker 默认的配置针对的三个或以上 Broker 高可用服务而设置的。这边在测试的时候为了简单方便，使用了嵌入式服务新建了一个单 Broker 的 Kafka 服务，出现了一些问题：如

1、事务日志副本集大于 Broker 数量，会抛如下异常：

```
Number of alive brokers '1' does not meet the required replication factor '3' 
for the transactions state topic (configured via 'transaction.state.log.replication.factor').
This error can be ignored if the cluster is starting up and not all brokers are up yet.
```

默认 Broker 的配置 transaction.state.log.replication.factor=3，单节点只能调整为 1

2、副本数小于副本同步队列数目，会抛如下异常

```java
Number of insync replicas for partition __transaction_state-13 is [1], below required minimum [2]
```

默认 Broker 的配置 transaction.state.log.min.isr=2，单节点只能调整为 1



#### ReplyingKafkaTemplate 获得消息回复

ReplyingKafkaTemplate 是 KafkaTemplate 的一个子类，除了继承父类的方法，新增了一个方法 sendAndReceive，实现了消息发送 \ 回复语义

```
RequestReplyFuture<K, V, R> sendAndReceive(ProducerRecord<K, V> record);
```

也就是我发送一条消息，能够拿到消费者给我返回的结果。就像传统的 RPC 交互那样。当消息的发送者需要知道消息消费者的具体的消费情况，非常适合这个 api。如，一条消息中发送一批数据，需要知道消费者成功处理了哪些数据。下面代码演示了怎么集成以及使用 ReplyingKafkaTemplate

```java
/**
 * @author: kl @kailing.pub
 * @date: 2019/5/30
 */
@SpringBootApplication
@RestController
public class Application {
	private final Logger logger = LoggerFactory.getLogger(Application.class);
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
	@Bean
	public ConcurrentMessageListenerContainer<String, String> repliesContainer(ConcurrentKafkaListenerContainerFactory<String, String> containerFactory) {
		ConcurrentMessageListenerContainer<String, String> repliesContainer = containerFactory.createContainer("replies");
		repliesContainer.getContainerProperties().setGroupId("repliesGroup");
		repliesContainer.setAutoStartup(false);
		return repliesContainer;
	}

	@Bean
	public ReplyingKafkaTemplate<String, String, String> replyingTemplate(ProducerFactory<String, String> pf, ConcurrentMessageListenerContainer<String, String> repliesContainer) {
		return new ReplyingKafkaTemplate(pf, repliesContainer);
	}

	@Bean
	public KafkaTemplate kafkaTemplate(ProducerFactory<String, String> pf) {
		return new KafkaTemplate(pf);
	}

	@Autowired
	private ReplyingKafkaTemplate template;

	@GetMapping("/send/{input}")
	@Transactional(rollbackFor = RuntimeException.class)
	public void sendFoo(@PathVariable String input) throws Exception {
		ProducerRecord<String, String> record = new ProducerRecord<>("topic-kl", input);
		RequestReplyFuture<String, String, String> replyFuture = template.sendAndReceive(record);
		ConsumerRecord<String, String> consumerRecord = replyFuture.get();
		System.err.println("Return value: " + consumerRecord.value());
	}

	@KafkaListener(id = "webGroup", topics = "topic-kl")
	@SendTo
	public String listen(String input) {
		logger.info("input value: {}", input);
		return "successful";
	}
}
```



### Spring-kafka 消息消费用法探秘



#### @KafkaListener 的使用

前面在简单集成中已经演示过了 @KafkaListener 接收消息的能力，但是 @KafkaListener 的功能不止如此，其他的比较常见的，使用场景比较多的功能点如下：

- 显示的指定消费哪些 Topic 和分区的消息，
- 设置每个 Topic 以及分区初始化的偏移量，
- 设置消费线程并发度
- 设置消息异常处理器

```java
	@KafkaListener(id = "webGroup", topicPartitions = {
			@TopicPartition(topic = "topic1", partitions = {"0", "1"}),
					@TopicPartition(topic = "topic2", partitions = "0",
							partitionOffsets = @PartitionOffset(partition = "1", initialOffset = "100"))
			},concurrency = "6",errorHandler = "myErrorHandler")
	public String listen(String input) {
		logger.info("input value: {}", input);
		return "successful";
	}
```

其他的注解参数都很好理解，errorHandler 需要说明下，设置这个参数需要实现一个接口 KafkaListenerErrorHandler。而且注解里的配置，是你自定义实现实例在 spring 上下文中的 Name。比如，上面配置为 errorHandler = "myErrorHandler"。则在 spring 上线中应该存在这样一个实例：

```java
/**
 * @author: kl @kailing.pub
 * @date: 2019/5/31
 */
@Service("myErrorHandler")
public class MyKafkaListenerErrorHandler implements KafkaListenerErrorHandler {
	Logger logger =LoggerFactory.getLogger(getClass());
	@Override
	public Object handleError(Message<?> message, ListenerExecutionFailedException exception) {
		logger.info(message.getPayload().toString());
		return null;
	}
	@Override
	public Object handleError(Message<?> message, ListenerExecutionFailedException exception, Consumer<?, ?> consumer) {
		logger.info(message.getPayload().toString());
		return null;
	}
}
```



#### 手动 Ack 模式

手动 ACK 模式，由业务逻辑控制提交偏移量。比如程序在消费时，有这种语义，特别异常情况下不确认 ack，也就是不提交偏移量，那么你只能使用手动 Ack 模式来做了。开启手动首先需要关闭自动提交，然后设置下 consumer 的消费模式

```
spring.kafka.consumer.enable-auto-commit=false
spring.kafka.listener.ack-mode=manual
```

上面的设置好后，在消费时，只需要在 @KafkaListener 监听方法的入参加入 Acknowledgment 即可，执行到 ack.acknowledge () 代表提交了偏移量

```java
	@KafkaListener(id = "webGroup", topics = "topic-kl")
	public String listen(String input, Acknowledgment ack) {
		logger.info("input value: {}", input);
		if ("kl".equals(input)) {
			ack.acknowledge();
		}
		return "successful";
	}
```



#### @KafkaListener 注解监听器生命周期

@KafkaListener 注解的监听器的生命周期是可以控制的，默认情况下，@KafkaListener 的参数 autoStartup = "true"。也就是自动启动消费，但是也可以同过 KafkaListenerEndpointRegistry 来干预他的生命周期。KafkaListenerEndpointRegistry 有三个动作方法分别如：start (),pause (),resume ()/ 启动，停止，继续。如下代码详细演示了这种功能。

```java
/**
 * @author: kl @kailing.pub
 * @date: 2019/5/30
 */
@SpringBootApplication
@RestController
public class Application {
	private final Logger logger = LoggerFactory.getLogger(Application.class);

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}

	@Autowired
	private KafkaTemplate template;

	@GetMapping("/send/{input}")
	@Transactional(rollbackFor = RuntimeException.class)
	public void sendFoo(@PathVariable String input) throws Exception {
		ProducerRecord<String, String> record = new ProducerRecord<>("topic-kl", input);
		template.send(record);
	}

	@Autowired
	private KafkaListenerEndpointRegistry registry;

	@GetMapping("/stop/{listenerID}")
	public void stop(@PathVariable String listenerID){
		registry.getListenerContainer(listenerID).pause();
	}
	@GetMapping("/resume/{listenerID}")
	public void resume(@PathVariable String listenerID){
		registry.getListenerContainer(listenerID).resume();
	}
	@GetMapping("/start/{listenerID}")
	public void start(@PathVariable String listenerID){
		registry.getListenerContainer(listenerID).start();
	}
	@KafkaListener(id = "webGroup", topics = "topic-kl",autoStartup = "false")
	public String listen(String input) {
		logger.info("input value: {}", input);
		return "successful";
	}
}
```

在上面的代码中，listenerID 就是 @KafkaListener 中的 id 值 “webGroup”。项目启动好后，分别执行如下 url，就可以看到效果了。

先发送一条消息：http://localhost:8081/send/ckl。因为 autoStartup = "false"，所以并不会看到有消息进入监听器。

接着启动监听器：http://localhost:8081/start/webGroup。可以看到有一条消息进来了。

暂停和继续消费的效果使用类似方法就可以测试出来了。



#### SendTo 消息转发

前面的消息发送响应应用里面已经见过 @SendTo, 其实除了做发送响应语义外，@SendTo 注解还可以带一个参数，指定转发的 Topic 队列。常见的场景如，一个消息需要做多重加工，不同的加工耗费的 cup 等资源不一致，那么就可以通过跨不同 Topic 和部署在不同主机上的 consumer 来解决了。如：

```java
	@KafkaListener(id = "webGroup", topics = "topic-kl")
	@SendTo("topic-ckl")
	public String listen(String input) {
		logger.info("input value: {}", input);
		return input + "hello!";
	}

	@KafkaListener(id = "webGroup1", topics = "topic-ckl")
	public void listen2(String input) {
		logger.info("input value: {}", input);
	}
```



#### 消息重试和死信队列的应用

除了上面谈到的通过手动 Ack 模式来控制消息偏移量外，其实 Spring-kafka 内部还封装了可重试消费消息的语义，也就是可以设置为当消费数据出现异常时，重试这个消息。而且可以设置重试达到多少次后，让消息进入预定好的 Topic。也就是死信队列里。下面代码演示了这种效果：

```java
	@Autowired
	private KafkaTemplate template;

	@Bean
	public ConcurrentKafkaListenerContainerFactory<?, ?> kafkaListenerContainerFactory(
			ConcurrentKafkaListenerContainerFactoryConfigurer configurer,
			ConsumerFactory<Object, Object> kafkaConsumerFactory,
			KafkaTemplate<Object, Object> template) {
		ConcurrentKafkaListenerContainerFactory<Object, Object> factory = new ConcurrentKafkaListenerContainerFactory<>();
		configurer.configure(factory, kafkaConsumerFactory);
		//最大重试三次
		factory.setErrorHandler(new SeekToCurrentErrorHandler(new DeadLetterPublishingRecoverer(template), 3));
		return factory;
	}

	@GetMapping("/send/{input}")
	public void sendFoo(@PathVariable String input) {
		template.send("topic-kl", input);
	}

	@KafkaListener(id = "webGroup", topics = "topic-kl")
	public String listen(String input) {
		logger.info("input value: {}", input);
		throw new RuntimeException("dlt");
	}

	@KafkaListener(id = "dltGroup", topics = "topic-kl.DLT")
	public void dltListen(String input) {
		logger.info("Received from DLT: " + input);
	}
```

上面应用，在 topic-kl 监听到消息会，会触发运行时异常，然后监听器会尝试三次调用，当到达最大的重试次数后。消息就会被丢掉重试死信队列里面去。死信队列的 Topic 的规则是，业务 Topic 名字 +“.DLT”。如上面业务 Topic 的 name 为 “topic-kl”，那么对应的死信队列的 Topic 就是 “topic-kl.DLT”

## 3. Kafka集群docker部署

1. 运行zookeeper

```cmd
docker run -d --name zookeeper -p 2181:2181 -t wurstmeister/zookeeper
```

2. 分别创建3个Kafka节点，并注册到ZK上,不同Kafka节点只需要更改端口号即可。

   - Kafka0：

   ```cobol
   docker run -d --name kafka0 -p 9092:9092 -e KAFKA_BROKER_ID=0 -e KAFKA_ZOOKEEPER_CONNECT=192.168.31.16:2181 -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://192.168.31.16:9092 -e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 -t wurstmeister/kafka
   ```

   - Kafka1：

   ```cobol
   docker run -d --name kafka1 -p 9093:9093 -e KAFKA_BROKER_ID=1 -e KAFKA_ZOOKEEPER_CONNECT=192.168.31.16:2181 -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://192.168.31.16:9093 -e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9093 -t wurstmeister/kafka
   ```

   - Kafka2：

   ```cobol
   docker run -d --name kafka2 -p 9094:9094 -e KAFKA_BROKER_ID=2 -e KAFKA_ZOOKEEPER_CONNECT=192.168.31.16:2181 -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://192.168.31.16:9094 -e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9094 -t wurstmeister/kafka
   ```

3. 进入kafka执行命令

```
docker exec -it kafka1 /bin/bash
```

创建主题

```
kafka-topics.sh --create --bootstrap-server 192.168.31.16:9092 --topic test
```

查看主题

```
kafka-topics.sh --describe  --zookeeper 192.168.31.16:2181 --topic test
```

## 4. Offset Explorer3 (图形化Kafka操作KafkaTool)

[KafkaTool官网](https://www.kafkatool.com/)

## 5. Kafka性能测试

[CSDN-Kafka性能测试](https://blog.csdn.net/weixin_42641909/article/details/89320999)

## 6. Java操作Kafka

