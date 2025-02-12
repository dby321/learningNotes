  [CSDN-Swagger3.0介绍及springboot整合Swagger3.0](https://blog.csdn.net/qq\_43521797/article/details/115835771)

Swagger
----------------------------------------------------------------------

学习目标：

*   了解Swagger的作用和概念
*   了解前后端分离
*   在SpringBoot中集成Swagger

### 一、Swagger出现的原因

**前后端分离**  
Vue+SpringBoot  
后端时代：前端只用管理静态页面;html==>后端。模板引擎 JSP=>后端是主力

前后端分离时代：

*   后端：后端控制层，服务层，数据访问层 【后端团队】
*   前端：前端控制层，视图层 【前端团队】
    *   伪造后端数据，json。已经存在了，不需要后端，前端工程依|旧能够跑起来·前端后如何交互?===>API
*   前后端相对独立，松耦合;
*   前后端甚至可以部署在不同的服务器上;

产生一个问题：

*   前后端集成联调，前端人员和后端人员无法做到，即使协商，尽早解决”，最终导致问题集中爆发;

解决方案

*   首先指定schema\[计划的提纲\]，实时更新最新API，降低集成的风险;
*   早些年：指定word计划文档;  
    \-前后端分离：

前后端分离:

*   前端测试后端接口:postman（测试工具）
*   后端提供接口，需要实时更新最新的消息及改动!

### 二、Swagger介绍

*   号称世界上最流行的Api框架；
*   Restful Api 文档在线自动生成工具=>Api文档与API定义同步更新
*   直接运行，可以在在线测试API 接口
*   支持多种语言：（java，Php…）

官网：[https://swagger.io/](https://swagger.io/)

#### 1、springfox-swagger 2

SpringBoot项目整合swagger2需要用到两个依赖：springfox-swagger2和springfox-swagger-ui，用于自动生成swagger文档。

*   springfox-swagger2：这个组件的功能用于帮助我们自动生成描述API的json文件
*   springfox-swagger-ui：就是将描述API的json文件解析出来，用一种更友好的方式呈现出来。

#### 2、SpringFox 3.0.0 发布

**此版本的亮点：**

*   Spring5，Webflux支持（仅支持请求映射，尚不支持功能端点）。
*   Spring Integration支持。
*   SpringBoot支持springfox Boot starter依赖性（零配置、自动配置支持）。
*   支持OpenApi 3.0.3。
*   零依赖。几乎只需要spring-plugin，swagger-core ，现有的swagger2注释将继续工作并丰富openapi3.0规范。

**兼容性说明：**

*   需要Java 8
*   需要Spring5.x（未在早期版本中测试）
*   需要SpringBoot 2.2+（未在早期版本中测试）

注意：`Swagger2.0`和`Swagger3.0`有很多区别，下面使用的是`Swagger3.0`，有区别的地方会简单说明

### 三、SpringBoot集成Swagger

#### 1、创建springboot WEB项目

项目设置  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7ea491b38134c71557758c8c9492183c.png)  
选择web模块  
![在这里插入图片描述](images/Swagger笔记/24b488a2fe1f947b4c72554a6fa82114-1739343807652-73.png)

#### 2、导入相关依赖

swagger3.0以下版本导入这两个就可以了

```
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>3.0.0</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>3.0.0</version>
</dependency>

```

swagger3.0以上还需要导入，不然无法进入后台界面

```
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-boot-starter -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-boot-starter</artifactId>
            <version>3.0.0</version>
        </dependency>

```

#### 3、编写万能的Hello工程

创建Controller层写一个HelloController  
写一个测试路径hello进行测试，项目是否可以跑起来。  
![在这里插入图片描述](images/Swagger笔记/e5088ccbadf0e17a56de5d17614e9483-1739343807652-75.png)  
测试地址：http://localhost:8080/  
出现下面页面就成功了！  
![在这里插入图片描述](images/Swagger笔记/6661f46ec3c2bc4bdae3f561b5898b7b-1739343807652-77.png)  
继续学习吧！！！

#### 4、配置Swagger

创建config层编写Swagger配置  
![在这里插入图片描述](images/Swagger笔记/54f6e65e88fc0c1ec8ec4a6c638b16fc-1739343807652-79.png)

```
package com.zhao.swagger.config;

import org.springframework.context.annotation.Configuration;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2 //开启Swagger2
public class SwaggerConfig {
}


```

#### 5、测试运行

运行程序进行测试  
**注意：版本不同`测试路径`不同**

*   **Swagger3.0**——>测试地址：`http://localhost:8080/swagger-ui/index.html`
*   **Swagger2.0**——>测试地址：`http://localhost:8080/swagger-ui.html`

显示的页面一共分四个部分如下图：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f755175d0ccbe15a8b3ba72e3620d502.png)

### 四、配置Swagger

Swagger的bean实例Docket;

#### 1、创建配置类配置Swagger

**注意：版本不同`注解`不同**

*   **Swagger3.0**——>注解：`@EnableOpenApi`
*   **Swagger2.0**——>注解：`@EnableSwagger2`  
    ![在这里插入图片描述](images/Swagger笔记/93c39d424242696c031b5978aa4d0af4-1739343807652-83.png)

```
package com.zhao.swagger.config;

        import org.springframework.context.annotation.Bean;
        import org.springframework.context.annotation.Configuration;
        import springfox.documentation.oas.annotations.EnableOpenApi;
        import springfox.documentation.spi.DocumentationType;
        import springfox.documentation.spring.web.plugins.Docket;

@Configuration
@EnableOpenApi //开启Swagger3.0
public class SwaggerConfig {

    //配置swagger的Docket的bean实例
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.OAS_30);
    }
}


```

#### 2、分析源码找参数`DocumentationType.OAS_30`

![在这里插入图片描述](images/Swagger笔记/b683a28dca1a9b9b830661a087d75555-1739343807652-85.png)

这里需要的是一个`DocumentationType`类型的参数  
![在这里插入图片描述](images/Swagger笔记/3c0f033c81fcc2ac65a3b09594b767ed-1739343807652-89.png)

我们看一下`Docket`源码，找到`DocumentationType`去看他的源码  
![在这里插入图片描述](images/Swagger笔记/e431c6c2e9713c4d706148be5c45dfc9-1739343807652-87.png)  
这里找到了我们需要的参数，选择我们使用的即可  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2a5808e8f0ce569f7344334b96b47381.png)

#### 3、配置Swagger信息ApiInfo

我们去看`ApiInfo`的源码获取我们需要的信息  
![在这里插入图片描述](images/Swagger笔记/f7cc7d8f6c93ff7cbe0c565e84854dc9-1739343807652-93.png)  
配置代码如下（他们对应的信息后面有解释）

```
 //配置Swagger信息apiInfo
    private ApiInfo apiInfo() {
        //作者信息
        Contact contact= new Contact(
                "Mr.zhao",
                "https://blog.csdn.net/qq_43521797?spm=1011.2124.3001.5343",
                "1433304356@qq.com");
        return new ApiInfo(
                "Mr.zhao API文档",
                "再小灰尘也能在阳光下起舞",
                "v1.0",
                "https://blog.csdn.net/qq_43521797?spm=1011.2124.3001.5343",
                contact,
                "Apache 2.0",
                "http://www.apache.org/licenses/LICENSE-2.0",
                new ArrayList<>());
    }

```

配置的信息分析如图：  
![在这里插入图片描述](images/Swagger笔记/0384f81f2c970318107bc0cebb030460-1739343807652-95.png)

### 五、配置扫描接口及开关

分析一下接口部分显示信息  
![在这里插入图片描述](images/Swagger笔记/d34eafaa8b49437042a16009a94ca1f3-1739343807652-97.png)  
信息来自于`springfox-swagger-ui-3.0.0.jar`  
![在这里插入图片描述](images/Swagger笔记/438c2f51692ea57c7c8b49fd2dea24a6-1739343807652-99.png)

#### 1、设置扫描包及过滤

> Docket下的select()

```
  //配置swagger的Docket的bean实例
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.OAS_30)
                .apiInfo(apiInfo())
                .select()
                //RequestHandlerSelectors配置要扫播接口的方式
                //basePackage指定要扫描的包
                .apis(RequestHandlerSelectors.basePackage("com.zhao.swagger.controller"))
                //paths()过滤什么路径
                .paths(PathSelectors.ant("/zhao/**"))
                .build();
    }

```

##### 1）RequestHandlerSelectors扫描接口的方式

RequestHandlerSelectors，扫描接口的方式。我们去`RequestHandlerSelectors`的源码看看其他配置方法

*   basePackage指定要扫描的包
*   any();扫描全部
*   none;不扫描
*   withClassAnnotation 扫描类上得注解
*   withMethodAnnotation 扫描方法上得注解  
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/782462d8d1f255508b3cd63c5c80c0c1.png)  
    ![在这里插入图片描述](images/Swagger笔记/41f4467132272fc573bd457c36ba96ca-1739343807652-103.png)

##### 2）PathSelectors过滤方式

和上面一样我们去源码中看他的方法有哪些

*   ant()：过滤路径
*   any()：全部过滤
*   none()：不过滤
*   regex()：正则表达式

我们使用第一种过滤路径测试  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/22e7360ef477f5c36afc64abcf6dc12e.png)

过滤后什么都没有了  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/8ec9978fe77518baf3feb7907d61783f.png)

#### 2、设置Swagger启动与关闭

我们在Docket的源码中科院看到`enabled=true`这个属性就是控制启动  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/89f6f77cba5f2c2c1e495c4f83965dcf.png)  
我们去改变一下他的值 改成false关闭一下试试  
注意：下面红框的是一个整体不能在吗中间添加其他东西。  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/0827f954414c6a1de259c15ecf5a76c9.png)

我们在`.apiInfo(apiInfo())`后面进行添加`.enable(false)`

```
 @Bean
    public Docket docket(){
        return new Docket(DocumentationType.OAS_30)
                .apiInfo(apiInfo())
                .enable(false)//关闭Swagger
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.zhao.swagger.controller"))
                .build();
    }

```

测试地址：http://localhost:8080/swagger-ui/index.html#/  
关闭后的效果  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/52609b3f7a7445b9ff29613aa20b5058.png)

#### 3、设置Swagger在对应的项目环境启动

我们在项目中只在**开发环境**开启Swagger，**生产环境**就要关闭Swagger。  
下面我们进行设置一下

##### 1）创建测试环境

*   application-dev：开发环境
*   application-pro：生产环境  
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/8aebfe4f80bd7d613d35f62020860e19.png)  
    application.properties内代码：`spring.profiles.active=dev`  
    application-dev.properties内代码：`server.port=8081`  
    application-pro.properties内代码：`server.port=8082`

##### 2）编写配置类获取项目环境

`Profiles.of()`设置要显示的swagger的环境  
通过`environment.acceptsProfiles`判断是否处在自己设定的环境中 获取`boolean`值`flag`  
赋值给`enable（flag）`

```
    //配置swagger的Docket的bean实例
    @Bean
    public Docket docket(Environment environment){

        //设置要显示的swagger的环境
        Profiles profiles =Profiles.of("dev");
        //通过environment.acceptsProfiles判断是否处在自己设定的环境中
        boolean flag = environment.acceptsProfiles(profiles);

        return new Docket(DocumentationType.OAS_30)
                .apiInfo(apiInfo())
                .enable(flag)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.zhao.swagger.controller"))
                .build();
    }


```

##### 3)测试

我们先测试dev环境  
application.properties中切换环境为dev环境： `spring.profiles.active=dev`

> dev环境===》测试地址：http://localhost:8081/swagger-ui/index.html#/  
> ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/fbb944354e79cd81f5befa4dcbf79250.png)

这次测试pro环境  
application.properties中切换环境为pro环境： `spring.profiles.active=pro`

> pro环境===》测试地址：http://localhost:8082/swagger-ui/index.html#/  
> ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/83a903bafc03a28635a97ea0634bc5f6.png)

### 六、分组

#### 1、设置分组

我继续看Docket的源码我们可以看到分组的默认值为default  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/8b658fd4e977e3306a5b92ec5bf1e291.png)  
下面我们进行修改,指需要一行代码

```
 .groupName("Mr.zhao")

```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/10e190f6cc9f9df10cb56e6da808f521.png)  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/831185ab870866b877f3ec0075e96244.png)

#### 2、多个分组

多个分组就是多人协作开发时每个人分组  
其实就是多个**Docket实例**  
下面我们测试一下  
我们在复制几个**Docket实例**进行测试

```
@Bean
    public Docket docket1(Environment environment){
        return new Docket(DocumentationType.OAS_30).groupName("A");
    }
    @Bean
    public Docket docket2(Environment environment){
        return new Docket(DocumentationType.OAS_30).groupName("B");
    }
    @Bean
    public Docket docket3(Environment environment){
        return new Docket(DocumentationType.OAS_30).groupName("C");
    }

```

运行一下就可以看到多个分组了，选择一个分组信息就会发生变化，以及测试路径。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/849772458f40903cbc352e7ffa03fe91.png)

### 七、接口注释及实体类注释

#### 1、接口注释

接口常用的三个注解

*   `@Api(tags = "helloController层")`：放在控制层类上，描述控制层
*   `@ApiOperation(value = "hello2方法",notes = "方法描述")`：放在方法控制层上，描述控制层方法
*   `@ApiParam("用户名")`：参数描述

```
package com.zhao.swagger.controller;

import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import io.swagger.annotations.ApiParam;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

@Api(tags = "helloController层")
@RestController
public class HelloController {

    @ApiOperation("hello方法")
    @PostMapping("/hello")
    public String hello(){
        return "hello Swagger";
    }
    @ApiOperation(value = "hello2方法",notes = "方法描述")
    @GetMapping("/hello2")
    public String hello2(@ApiParam("用户名") String username){
        return username;
    }
}


```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a74b371f7d75d1ca81252fcd6400215a.png)

#### 2、实体类注释

我们先创建pojo编写一个User进行测试

```
package com.zhao.swagger.pojo;

public class User {
    private String username;
    private String password;
}


```

我们直接启动项目看看可以扫描我们的实体类吗？答案是不能  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/aa6322f3abdbdec7af5ea8841fb4833c.png)  
方法一（不是用注解，也可以扫描到实体类）  
实体类不使用注释也可以被扫描，只要请求返回类型是实体类即可。我们测试一下  
我写一个方法

```
@ApiOperation(value = "user方法",notes = "测试不使用注解，把实体类作为返回值可以扫描到实体类吗")
    @GetMapping("/user")
    public User user(){
        return new User();
    }

```

实体有了但是这样的没有属性，这是因为没有给属性添加get set方法，  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/8773ac4bcc8cb014b67b3772d7362b54.png)  
给是实体类添加get set 方法,再次测试

```
package com.zhao.swagger.pojo;

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;

public class User {
    private String username;
    private String password;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}

```

这次就完美了  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/3afd725204407385eaa1d443dcd57208.png)

方法二（注解）

*   `@ApiModel("用户实体类")`：用于实体类上，描述实体类
*   `@ApiModelProperty(value = "用户名",hidden = false)`：用于属性上描述实体类属性，hidden功能是，是否隐藏该属性

```
package com.zhao.swagger.pojo;

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;

@ApiModel("用户实体类")
public class User {
    @ApiModelProperty(value = "用户名",hidden = false)//hidden是否隐藏
    private String username;
    @ApiModelProperty("密码")
    private String password;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}


```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/fe54bdb25baa96a13361e3dd2c35d608.png)

### 八、简单的测试

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5956253e7c126f659be2d7f3603336d4.png)

 

  


