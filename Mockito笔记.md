# SpringBoot Mockito使用指南

在Spring Boot应用中，Mock是一种常用的测试手段，用于模拟对象的行为，以便在不依赖外部系统或服务的情况下进行单元测试。本指南将介绍如何在Spring Boot项目中使用Mock进行测试。我发现在知识库文档中尚缺少相关文档，故就此补充，深入的内容需要自行查阅相关文档，本人在此仅做抛砖引玉。

[mockito中文文档](https://gitee.com/wnboy/mockito-doc-zh )
[Mockito官方网站](https://site.mockito.org/ )
[思否-Spring单元测试教程（JUnit5+Mockito）](https://segmentfault.com/a/1190000040803747 )
[Baeldung-Mockito教程](https://www.baeldung-cn.com/mockito-series )

## 1. 测试概述

如果项目组有测试团队，最常接触的概念有：功能测试、回归测试、冒烟测试等，但这些都是由测试人员发起的。
开发人员写的常常是“单元测试”，但其实可以细分成 单元测试 和 集成测试 两个。划分的原因拿常见的 Spring IoC 举例。Spring 不同Bean之间相互依赖，例如某API业务逻辑中会依赖不同模块的 Service，Service 方法中又可能依赖不同的 Dao 层方法，甚至还会通过 RPC、HTTP 调用外部服务方法。这给我们写测试用例带来了难度，本来只想测试某个方法的功能，却要考虑一连串的依赖关系。

### 1.1. 单元测试

单元测试：是指对软件中的最小可测试单元进行检查和验证。
通常任何软件都会划分为不同的模块和组件。单独测试一个组件时，我们叫做单元测试。单元测试用于验证相关的一小段代码是否正常工作。单元测试不是用于发现应用程序范围内的 bug，或者回归测试的 bug，而是分别检测每个代码片段。
单元测试不验证应用程序代码是否和外部依赖正常工作。它聚焦与单个组件并且 Mock 所有和它交互的依赖。例如，方法中调用发短信的服务，以及和数据库的交互，我们只需要 Mock 假执行即可，毕竟测试的焦点在当前方法上。
单元测试的特点：
-不依赖任何模块。
-基于代码的测试，不需要在 ApplicationContext 中运行。
-方法执行快，500ms以内（也和不启动 Spring 有关）。
-同一单元测试可重复执行N次，并每次运行结果相同。

### 1.2. 集成测试

集成测试：在单元测试的基础上，将所有模块按照设计要求组装成为子系统或系统，进行集成测试。
集成测试主要用于发现用户端到端请求时不同模块交互产生的问题。集成测试范围可以是整个应用程序，也可以是一个单独的模块，取决于要测试什么。
在集成测试中，我们应该聚焦于从控制器层到持久层的完整请求。应用程序应该运行嵌入服务（例如：Tomcat）以创建应用程序上下文和所有 bean。这些 bean 有的可能会被 Mock 覆盖。
集成测试的特点：
-集成测试的目的是测试不同的模块一共工作能否达到预期。
-应用程序应该在 ApplicationContext 中运行。Spring boot 提供 @SpringBootTest 注解创建运行上下文。

- 使用 @TestConfiguration 等配置测试环境。

  

## 2. Mock起步

### 2.1 Mock依赖引入

在Spring Boot项目中，通常会使用spring-boot-starter-test依赖，它包含了JUnit、Spring Test、Mockito等测试相关的库,为了方便模拟web层测试，我们还需要引入spring-boot-starter-web。
示例：

```xml
<!-- 在pom.xml中添加依赖 -->
<dependencies>
 <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-test</artifactId>
     <scope>test</scope>
     <version>2.7.10</version>    
 </dependency>
</dependencies>
```



### 2.2 使用Mockito模拟对象

Mockito是一个流行的Java模拟框架，它可以用来模拟对象、验证方法调用等。

#### 2.2.1 模拟对象

```java
// 模拟一个UserService对象
UserService mockUserService = Mockito.mock(UserService.class);
```

#### 2.2.2 验证方法调用
示例：下面是测试通过和不通过的代码

```java
@Test
void testFindUserById() {
    // 调用控制器的方法userService.findUserById(1)
    User user = mockUserService.findUserById(1);
    // 断言结果
    assertNotNull(user);
    assertEquals("John", user.getFirstName());
    assertEquals("Doe", user.getLastName());
    verify(mockUserService).findUserById(1);
}
```





```java
@Test
void testFindUserById() {
    // 调用控制器的方法userService.findUserById(1)
    User user = mockUserService.findUserById(1);
  	// 断言结果
    assertNotNull(user);
    assertEquals("Johx", user.getFirstName());
    assertEquals("Doe", user.getLastName());
    verify(mockUserService).findUserById(1);
}
```

### 2.3 使用SpringTest模拟Bean
Spring Test提供了@MockBean和@Mock注解，用于在Spring应用上下文中模拟Bean。
#### 2.3.1 使用@MockBean模拟Bean
@MockBean注解用于在Spring应用上下文中添加一个模拟的Bean，它将在测试开始时自动添加到Spring应用上下文中。
示例：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class MyServiceTest {
    @MockBean
    private UserService userService;

    @Autowired
    private MyService myService;

    @Test
    void testFindUserById() {
        // 调用控制器的方法，这里假设你的控制器会调用 userService.findUserById(1)
        User user = userService.findUserById(1);

        // 断言结果
        assertNotNull(user);
        assertEquals("John", user.getFirstName());
        assertEquals("Doe", user.getLastName());
        verify(userService).findUserById(1);
    }
}
```

#### 2.3.2 使用@Mock模拟Bean
@Mock注解用于创建一个模拟对象，但它不会自动添加到Spring应用上下文中。需要手动将其添加到Spring应用上下文中。
示例：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class MyServiceTest {

    @Mock
	private UserService userService;
	@Test
    void testFindUserById() {
    // 调用控制器的方法 userService.findUserById(1)
          User user = userService.findUserById(1);
    // 断言结果
       assertNotNull(user);
       assertEquals("John", user.getFirstName());
       assertEquals("Doe", user.getLastName());
       verify(userService).findUserById(1);
    }
   
}
```

### 2.4 使用TestRestTemplate进行Web层测试
TestRestTemplate是Spring提供的一个用于测试Web层的工具类，它可以发送HTTP请求并验证响应。
示例：

```java
@SpringBootTest(classes = App.class)
public class MyControllerTest {

    @Autowired
    private RestTemplate restTemplate;

    @Test
    public void testSomeControllerMethod() {
        // 发送GET请求
        ResponseEntity<String> response = restTemplate.getForEntity("/some/path", String.class);

        // 验证响应状态码
        assertEquals(HttpStatus.OK, response.getStatusCode());
    }
}
```



## 3. 要注意的问题
Mockito 是一个流行的 Java 测试框架，用于模拟依赖关系和编写单元测试。在使用 Mockito 的过程中，开发人员可能会遇到一些常见的问题和错误。以下是一些使用 Mockito 时的坑点总结 ：

1. 滥用 @Mock 和 @InjectMocks 注释：
   使用 @Mock 注解为特定类创建模拟对象，而 @InjectMocks 注解用于将模拟对象注入到被测试的类中。需要注意的是，@InjectMocks 只能与类一起使用，不能与接口一起使用 。

2. 不重置 Mock 对象：
   Mockito 允许创建可在多个测试中重用的 Mock 对象。如果在测试之间未重置 Mock 对象，则可能会导致意外行为和不可靠的测试。可以使用 Mockito.reset() 方法重置所有 Mock 对象 。

3. 对 Mock 对象使用错误的范围：
   Mockito 默认创建范围为类级别的 Mock 对象，这意味着同一个 Mock 对象将用于类中的所有测试方法。如果模拟对象需要为每个测试方法具有不同的状态或行为，则应使用方法级别的范围创建。可以使用 Spring Boot 提供的 @MockBean 注解来创建具有正确范围的 Mock 对象 。
4. 未使用正确的注解：
   使用 @MockBean 和 @RunWith 注解来配置单元测试的 Spring 测试框架。通过使用这些注解，可以确保应用程序上下文被加载并且依赖项被正确地注入 。
5. 未使用正确的测试配置：
   使用正确的配置以确保正确加载应用程序上下文并按预期注入依赖项。可以使用 @ContextConfiguration 注解来指定测试的配置 。
6. 未使用正确的方法来创建 Mock 对象：
   使用正确的方法来创建 Mock 对象，以确保依赖项的行为是可控的并且测试是可靠的。可以使用 Mockito.mock() 方法来创建 Mock 对象 。
7. 未使用正确的方法来存根 Mock 对象：
   使用 Mockito.when() 方法来指定 Mock 对象的行为，确保在测试中控制依赖项 。
8. 未使用正确的方法来验证 Mock 对象的交互：
   Mockito 提供了几种验证 Mock 对象交互的方法，例如 Mockito.verify()、Mockito.verifyZeroInteractions() 和 Mockito.verifyNoMoreInteractions()。使用正确的方法来实现所需的行为非常重要 。
9. 未使用正确的方法来验证 Mock 对象的交互顺序：
   Mockito 提供了一个名为 Mockito.inOrder() 的方法，可用于验证与模拟对象交互的顺序。在验证交互顺序时使用此方法非常重要 。
   通过避免这些坑点，开发人员可以确保他们的测试更加可靠和有效。
