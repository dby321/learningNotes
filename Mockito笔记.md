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

  

## 2. Mock实战

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

### 2.2 开启Mockito注解

#### 2.2.1. *MockitoJUnitRunner*

我们的第一个选择是**使用\*MockitoJUnitRunner\*注解JUnit测试**：

```java
@ExtendWith(MockitoExtension.class)
public class MockitoAnnotationUnitTest {
    ...
}
```

#### 2.2.2. MockitoAnnotations.openMocks（）

或者，我们可以通过调用*MockitoAnnotations.openMocks（）***以编程方式启用Mockito注解**：

```java
@BeforeEach
public void init() {
    MockitoAnnotations.openMocks(this);
}
```

####  2.2.3. mockitoJUnit.rule（）

最后，**我们可以使用\*MockitoJUnit.rule（）\***：

```java
public class MockitoAnnotationsInitWithMockitoJUnitRuleUnitTest {

    @Rule
    public MockitoRule initRule = MockitoJUnit.rule();

    ...
}
```

在这种情况下，我们必须记住将我们的规则*公开*。

### 2.3 @Mock

Mockito中使用最广泛的注解是*@Mock*。我们可以使用*@Mock*创建和注入模拟实例，而无需手动调用*Mockito.mock*。

在下面的例子中，我们将手动创建一个模拟*的ArrayList*，而不使用*@Mock*注解：

```java
@Test
public void whenNotUseMockAnnotation_thenCorrect() {
    List mockList = Mockito.mock(ArrayList.class);
    
    mockList.add("one");
    Mockito.verify(mockList).add("one");
    assertEquals(0, mockList.size());

    Mockito.when(mockList.size()).thenReturn(100);
    assertEquals(100, mockList.size());
}
```

现在我们将做同样的事情，但我们将使用*@Mock*注解注入mock：

```java
@Mock
List<String> mockedList;

@Test
public void whenUseMockAnnotation_thenMockIsInjected() {
    mockedList.add("one");
    Mockito.verify(mockedList).add("one");
    assertEquals(0, mockedList.size());

    Mockito.when(mockedList.size()).thenReturn(100);
    assertEquals(100, mockedList.size());
}
```

### 2.4 @DoNotMock

*@DoNotMock*注解用于指示在测试期间不应模拟特定类型。它通常应用于类或接口级别，以指导测试框架和开发人员使用替代的测试解决方案，例如使用真实的实例或标准的伪实例。

下面是如何使用*@DoNotMock*注解的示例：

```java
import org.mockito.exceptions.misusing.DoNotMock;

@DoNotMock(reason = "Use a real instance instead")
public abstract class NotToMock {
    // Class implementation
}
```

例如，在NotToMock类的上下文中，*@DoNotMock*注解与指定的reason属性一起应用，例如“改用真实的实例”。这个注解明确地传达*了NotToMock*类在测试期间不应该被模仿的意图，而是建议使用实际的实例。

带有reason属性的*@DoNotMock*注解清楚地指导测试框架和开发人员考虑替代的测试方法，而不是诉诸mocking。

### 2.5 @Spy

现在让我们看看如何使用*@Spy*注解来监视现有实例。

在下面的例子中，我们创建了一个不使用*@Spy*注解*的List*的spy：

```java
@Test
public void whenNotUseSpyAnnotation_thenCorrect() {
    List<String> spyList = Mockito.spy(new ArrayList<String>());
    
    spyList.add("one");
    spyList.add("two");

    Mockito.verify(spyList).add("one");
    Mockito.verify(spyList).add("two");

    assertEquals(2, spyList.size());

    Mockito.doReturn(100).when(spyList).size();
    assertEquals(100, spyList.size());
}
```

现在我们将做同样的事情，监视列表，但我们将使用*@Spy*注解：

```java
@Spy
List<String> spiedList = new ArrayList<String>();

@Test
public void whenUseSpyAnnotation_thenSpyIsInjectedCorrectly() {
    spiedList.add("one");
    spiedList.add("two");

    Mockito.verify(spiedList).add("one");
    Mockito.verify(spiedList).add("two");

    assertEquals(2, spiedList.size());

    Mockito.doReturn(100).when(spiedList).size();
    assertEquals(100, spiedList.size());
}
```

请注意，像以前一样，我们在这里与间谍进行交互，以确保它的行为正确。在这个例子中，我们：

- 使用了**真实**的方法*spiedList.add（）*向*spiedList*添加元素。
- 使用*Mockito.doReturn（**）*对spiedList.size（）**方法进行存根处理**，使其返回*100*而不是*2*。

### 2.6 @Captor

接下来，我们看看如何使用*@Captor*注解创建*ArgumentCaptor*实例。

在下面的示例中，我们将创建一个*ArgumentCaptor*而不使用*@Captor*注解：

```java
@Test
public void whenNotUseCaptorAnnotation_thenCorrect() {
    List mockList = Mockito.mock(List.class);
    ArgumentCaptor<String> arg = ArgumentCaptor.forClass(String.class);

    mockList.add("one");
    Mockito.verify(mockList).add(arg.capture());

    assertEquals("one", arg.getValue());
}
```

现在让我们**使用\*@Captor\***达到相同的目的，创建一个*ArgumentCaptor*实例：

```java
@Mock
List mockedList;

@Captor 
ArgumentCaptor argCaptor;

@Test
public void whenUseCaptorAnnotation_thenTheSame() {
    mockedList.add("one");
    Mockito.verify(mockedList).add(argCaptor.capture());

    assertEquals("one", argCaptor.getValue());
}
```

请注意，当我们删除配置逻辑时，测试如何变得更简单且更具可读性。

### 2.7 @InjectMocks注解

现在我们讨论如何使用*@InjectMocks*注解将模拟字段自动注入到被测试对象中。

在下面的示例中，我们将使用*@InjectMocks*将模拟*wordMap*注入到*MyDictionary* *dic*中：

```java
@Mock
Map<String, String> wordMap;

@InjectMocks
MyDictionary dic = new MyDictionary();

@Test
public void whenUseInjectMocksAnnotation_thenCorrect() {
    Mockito.when(wordMap.get("aWord")).thenReturn("aMeaning");

    assertEquals("aMeaning", dic.getMeaning("aWord"));
}
```

这是*MyDictionary*类：

```java
public class MyDictionary {
    Map<String, String> wordMap;

    public MyDictionary() {
        wordMap = new HashMap<String, String>();
    }
    public void add(final String word, final String meaning) {
        wordMap.put(word, meaning);
    }
    public String getMeaning(final String word) {
        return wordMap.get(word);
    }
}
```



### 2.8 向 Spy 中注入 Mock

与上面的测试类似，我们可能想将模拟注入到间谍中：

```java
@Mock
Map<String, String> wordMap;

@Spy
MyDictionary spyDic = new MyDictionary();
```

**但是，Mockito 不支持将模拟注入到间谍中，**并且以下测试会导致异常：

```java
@Test 
public void whenUseInjectMocksAnnotation_thenCorrect() { 
    Mockito.when(wordMap.get("aWord")).thenReturn("aMeaning"); 

    assertEquals("aMeaning", spyDic.getMeaning("aWord")); 
}
```

如果我们想将模拟与间谍一起使用，我们可以通过构造函数手动注入模拟：

```java
MyDictionary(Map<String, String> wordMap) {
    this.wordMap = wordMap;
}
```

我们现在可以手动创建间谍，而不是使用注解：

```java
@Mock
Map<String, String> wordMap; 

MyDictionary spyDic;

@@BeforeEach
public void init() {
    MockitoAnnotations.openMocks(this);
    spyDic = Mockito.spy(new MyDictionary(wordMap));
}
```

现在测试将通过。

### 2.9. 使用注解时遇到 NPE

当我们尝试实际使用*@Mock*或*@Spy*注解的实例时，通常会**遇到\*NullPointerException\*** ：

```java
public class MockitoAnnotationsUninitializedUnitTest {

    @Mock
    List<String> mockedList;

    @Test(expected = NullPointerException.class)
    public void whenMockitoAnnotationsUninitialized_thenNPEThrown() {
        Mockito.when(mockedList.size()).thenReturn(1);
    }
}
```

大多数时候，发生这种情况只是因为我们忘记正确启用 Mockito 注解。

所以我们必须记住，每次我们想要使用 Mockito 注解时，我们都必须采取额外的步骤并初始化它们，正如我们[之前](https://www.baeldung.com/mockito-annotations#enable-annotations)已经解释过的那样。

### 2.10. 注意事项

最后，这里是关于 Mockito 注解的一些注意事项：

- Mockito 的注解最大限度地减少了重复的模拟创建代码。
- 它们使测试更具可读性。
- *@InjectMocks*对于注入*@Spy*和*@Mock*实例是必需的。

## 3. Web项目中使用Mockito

### 3.1 使用Mockito模拟对象

#### 3.1.1 模拟对象

```java
// 模拟一个UserService对象
UserService mockUserService = Mockito.mock(UserService.class);
```

#### 3.1.2 验证方法调用
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

### 3.2 使用SpringTest模拟Bean
Spring Test提供了@MockBean和@Mock注解，用于在Spring应用上下文中模拟Bean。
#### 3.2.1 使用@MockBean模拟Bean
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

#### 3.3.2 使用@Mock模拟Bean
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

### 3.3 使用RestTemplate进行Web层测试
RestTemplate是Spring提供的一个用于测试Web层的工具类，它可以发送HTTP请求并验证响应。
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



## 4. 要注意的问题
Mockito 是一个流行的 Java 测试框架，用于模拟依赖关系和编写单元测试。在使用 Mockito 的过程中，开发人员可能会遇到一些常见的问题和错误。以下是一些使用 Mockito 时的坑点总结 ：

1. 滥用 @Mock 和 @InjectMocks 注解：
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
