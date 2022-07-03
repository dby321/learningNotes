# Spring-Security

## 1 基本概念

### 1.1 什么是认证

认证：就是判断一个用户的身份是否合法的过程，常用的用户身份认证方式有：用户名密码登录、二维码登录、手机短信登录、指纹认证等方式

### 1.2 什么是会话

会话：为了避免用户的每次操作都进行认证可将用户的信息保存在会话中，会话就是系统为了保持当前用户的登录状态所提供的机制，常见的有基于session方式、基于token方式等

基于session的认证方式的交互流程是：用户认证成功后，在服务端生成用户相关的数据保存在session（当前会话）中，发给客户端的session_id存放到cookie中，这样用户客户端请求时带上session_id就可以验证服务器是否存在session数据，以此完成用户的合法校验，当用户退出系统或session过期销毁时，客户端的session_id也就无效了

![1623815517413](.\images\1623815517413.png)

基于token的认证方式的交互流程是：用户认证成功后，服务端生成一个token发给客户端，客户端可以放到cookie或localstorage等存储中，每次请求时带上token，服务端收到token通过验证后即可确认用户身份。

![1623815612887](.\images\1623815612887.png)

### 1.3 什么是授权

授权：用户认证通过根据用户的权限来控制用户访问资源的过程，用于资源的访问权限则正常访问，没有权限则拒绝访问

授权的数据模型：授权可简单理解为who对what进行how操作

who，即主体（subject），一般是指用户，也可以是程序，需要访问系统中的资源

what，即资源（Resource），如系统菜单、页面、按钮、代码方法、系统商品信息、系统订单信息等。系统菜单、页面、按钮、代码方法都属于系统功能资源，对于web系统每个功能资源通常对应一个URL；系统商品信息、系统订单信息属于实体资源（数据资源），实体资源由资源类型和资源实例组成，比如商品信息为资源类型，商品编号0001的商品为资源实例。

How，即权限（Permission），规定了用户对资源的操作许可，权限离开资源没有意义，如用户查询权限、用户添加权限、某个代码方法的调用权限、编号0001的用户的修改权限等

![1623816655424](.\images\1623816655424.png)

### 1.4 RBAC

RBAC基于角色的访问控制

![1623817060258](.\images\1623817060258.png)

RBAC基于资源的访问控制

![1623816952151](.\images\1623816952151.png)

## 2 基于Session的认证方式

### 2.1 认证流程

![1623817116971](.\images\1623817116971.png)

基于session的认证机制有sevlet规范定制，sevlet容器已实现，用户通过HttpSession的操作方法即可实现，如下是HttpSession的相关操作API

```java
//获取Session对象
request.getSession()
request.getSession(boolean create)
//获取SessionId
getId()
//获取当前session对象的创建时间
getCreationTime()
//获取最后一次访问该session对象的时间
getLastAccessedTime()
//设置Session最大时效
setMaxInactiveInterval()
//获取Session最大时效
getMaxInactiveInterval()
//判断当前Session对象是不是新建的
/**
如果客户端请求消息中返回了一个与Servlet程序当前获得的HttpSession对象的会话标识号相同的会话标识号，则认为这个HttpSession对象不是新建的。
*/
isNew()
//销毁当前Session对象
invalidate()
//属性相关
setAttribute()
getAttribute()
removeAttribute()
getAttributeNames()
```

### 2.2 创建工程

```java
@Configuration
@ComponentScan(basePackages = "com.binyu",excludeFilters ={@ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class)} )
public class ApplicationConfig {
    // 在此配置除了Controller的其他bean，比如数据库连接池、事务管理器、业务bean

}
```

```java
@Configuration
@EnableWebMvc
@ComponentScan(basePackages = "com.binyu",includeFilters = {@ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class)})
public class WebConfig implements WebMvcConfigurer {
    //视图解析器
    @Bean
    public InternalResourceViewResolver viewResolver(){
        InternalResourceViewResolver viewResolver=new InternalResourceViewResolver (  );
        viewResolver.setPrefix ( "/WEB-INF/view/" );
        viewResolver.setSuffix ( ".jsp" );
        return viewResolver;
    }
}
```

```java
public class SpringApplicationInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
    // spring容器,相当于加载applicationContext.xml
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{ApplicationConfig.class};
    }
    // servletcontext，相当于加载springmvc.xml
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{ WebConfig.class };
    }
    // 加载url-mapping
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```



![1623822289733](.\images\1623822289733.png)

```html
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<html>
<head>
    <title>用户登录</title>
</head>
<body>
<form action="login" method="post">
    用户名:<input type="text" name="username"/><br>
    密码:<input type="password" name="password"/>
    <input type="submit" value="登录">
</form>
</body>
</html>
```

> jsp页面如果使用HTML5的头会出中文乱码问题

> form action="login" 而不是 form action="/login"



### 2.3 实现认证功能

在WebConfig下添加

```java
  @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("login");
    }
```

认证接口+实现

```java
/**
 * Created by Administrator.
 */
public interface AuthenticationService {
    /**
     * 用户认证
     * @param authenticationRequest 用户认证请求，账号和密码
     * @return 认证成功的用户信息
     */
    UserDto authentication(AuthenticationRequest authenticationRequest);
}
```

```java
@Data
@AllArgsConstructor
public class UserDto {
    public static final String SESSION_USER_KEY = "_user";
    //用户身份信息
    private String id;
    private String username;
    private String password;
    private String fullname;
    private String mobile;
    /**
     * 用户权限
     */
    private Set<String> authorities;
}
```

```java
/**
 * @author Administrator
 * @version 1.0
 **/
@Service
public class AuthenticationServiceImpl implements  AuthenticationService{
    /**
     * 用户认证，校验用户身份信息是否合法
     *
     * @param authenticationRequest 用户认证请求，账号和密码
     * @return 认证成功的用户信息
     */
    @Override
    public UserDto authentication(AuthenticationRequest authenticationRequest) {
        //校验参数是否为空
        if(authenticationRequest == null
            || StringUtils.isEmpty(authenticationRequest.getUsername())
            || StringUtils.isEmpty(authenticationRequest.getPassword())){
            throw new RuntimeException("账号和密码为空");
        }
        //根据账号去查询数据库,这里测试程序采用模拟方法
        UserDto user = getUserDto(authenticationRequest.getUsername());
        //判断用户是否为空
        if(user == null){
            throw new RuntimeException("查询不到该用户");
        }
        //校验密码
        if(!authenticationRequest.getPassword().equals(user.getPassword())){
            throw new RuntimeException("账号或密码错误");
        }
        //认证通过，返回用户身份信息
        return user;
    }
    //根据账号查询用户信息
    private UserDto getUserDto(String userName){
        return userMap.get(userName);
    }
    //用户信息
    private Map<String,UserDto> userMap = new HashMap<>();
    {
        Set<String> authorities1 = new HashSet<>();
        authorities1.add("p1");//这个p1我们人为让它和/r/r1对应
        Set<String> authorities2 = new HashSet<>();
        authorities2.add("p2");//这个p2我们人为让它和/r/r2对应
        userMap.put("zhangsan",new UserDto("1010","zhangsan","123","张三","133443",authorities1));
        userMap.put("lisi",new UserDto("1011","lisi","456","李四","144553",authorities2));
    }
}

```

```java
/**
 * @author Administrator
 * @version 1.0
 **/
@Data
public class AuthenticationRequest {
    //认证请求参数，账号、密码。。
    /**
     * 用户名
     */
    private String username;

    /**
     * 密码
     */
    private String password;

}
```



### 2.4 实现会话功能

```java
public class UserDetails {
    public static final String SESSION_USER_KEY="_user";
```

登录功能

```java
 @PostMapping(value = "/login",produces = "text/plain;charset=UTF-8")
    public String login(AuthenticationRequest authenticationRequest, HttpSession session){
        UserDetails userDetails = authenticationService.authentication ( authenticationRequest );
        //存入session
        session.setAttribute ( UserDetails.SESSION_USER_KEY,userDetails );
        return userDetails.getUsername ()+"登录成功";
    }
```

```java
 @GetMapping(value = "/r/r1",produces = "text/plain;charset=UTF-8")
    public String r1(HttpSession session){
        String fullname;
        Object attribute = session.getAttribute ( UserDetails.SESSION_USER_KEY );
        if(attribute==null){
            fullname="匿名";
        }else{
            UserDetails userDetails = (UserDetails) attribute;
            fullname=userDetails.getFullname ();
        }
        return fullname+"访问资源r1";
    }
```

退出功能

```java
   @GetMapping(value = "/logout",produces = {"text/plain;charset=UTF-8"})
    public String logout(HttpSession session){
        session.invalidate();
        return "退出成功";
    }
```



### 2.5 实现授权功能

```JAVA
@Component
public class SimpleAuthenticationInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 校验用户请求的url是否在用户的权限范围内
        // 取出用户身份信息
        Object object = request.getSession ().getAttribute ( UserDetails.SESSION_USER_KEY );
        if(object==null){
            // 没有认证，提示登录
            writeContent(response,"请登录");
        }
        UserDetails userDetails=(UserDetails)object;
        String requestURI = request.getRequestURI ();
        if(userDetails.getAuthorities ().contains ( "p1" )&&requestURI.contains ( "/r/r1" )){
            return true;
        }
        if(userDetails.getAuthorities ().contains ( "p2" )&&requestURI.contains ( "/r/r2" )){
            return true;
        }
        writeContent ( response,"没有权限，拒绝访问" );
        return false;
    }

    private void writeContent(HttpServletResponse response, String msg) throws IOException {
    response.setContentType ( "text/html;charset=UTF-8" );
        PrintWriter writer = response.getWriter ();
        writer.print ( msg );
        writer.close ();
    }

}		
```

添加拦截器

```java
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(simpleAuthenticationInterceptor).addPathPatterns("/r/**");
    }
```



## 3 Spring Security快速上手

### 3.1 Spring Security介绍

是一个能够基于Spring的企业应用系统提供声明式的安全访问控制解决方案的安全框架

 正如你可能知道的关于安全方面的两个主要区域是“**认证**”和“**授权**”（或者访问控 制），一般来说，Web 应用的安全性包括用户认证（Authentication）和用户授权 （Authorization）两个部分，这两点也是 Spring Security 重要核心功能。 

（1）**用户认证**指的是：验证某个用户是否为系统中的合法主体，也就是说用户能否访问 该系统。用户认证一般要求用户提供用户名和密码。系统通过校验用户名和密码来完成认 证过程。通俗点说就是系统认为用户是否能登录 

（2）**用户授权**指的是验证某个用户是否有权限执行某个操作。在一个系统中，不同用户 所具有的权限是不同的。比如对一个文件来说，有的用户只能进行读取，而有的用户可以 进行修改。一般来说，系统会为不同的用户分配不同的角色，而每个角色则对应一系列的 权限。通俗点讲就是系统判断用户是否有权限去做某些事情 

### 3.2 Spring Security和Shiro对比

 **Spring Security 特点**： 

⚫ 和 Spring 无缝整合。

 ⚫ 全面的权限控制。

 ⚫ 专门为 Web 开发而设计。

​	 ◼旧版本不能脱离 Web 环境使用。

​	 ◼新版本对整个框架进行了分层抽取，分成了核心模块和 Web 模块。单独 引入核心模块就可以脱离 Web 环境。

 ⚫ 重量级。 



 **Shiro 特点**： 

⚫ 轻量级。Shiro 主张的理念是把复杂的事情变简单。针对对性能有更高要求 的互联网应用有更好表现。 

⚫ 通用性。 

​	◼好处：不局限于 Web 环境，可以脱离 Web 环境使用。

​	◼缺陷：在 Web 环境下一些特定的需求需要手动编写代码定制。

 

 因此，一般来说，常见的安全管理技术栈的组合是这样的： 

• SSM + Shiro

• Spring Boot/Spring Cloud + Spring Security  

### 3.3 Spring Security基本原理

 Spring Security 本质是一个过滤器链： 从启动是可以获取到过滤器链：

```org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter org.springframework.security.web.context.SecurityContextPersistenceFilter  org.springframework.security.web.header.HeaderWriterFilter org.springframework.security.web.csrf.CsrfFilter org.springframework.security.web.authentication.logout.LogoutFilter  org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter  org.springframework.security.web.authentication.ui.DefaultLoginPageGeneratingFilter  org.springframework.security.web.authentication.ui.DefaultLogoutPageGeneratingFilter org.springframework.security.web.savedrequest.RequestCacheAwareFilter org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter org.springframework.security.web.authentication.AnonymousAuthenticationFilter  org.springframework.security.web.session.SessionManagementFilter  org.springframework.security.web.access.ExceptionTranslationFilter  org.springframework.security.web.access.intercept.FilterSecurityInterceptor```

 代码底层流程：

重点看三个过滤器： 

`FilterSecurityInterceptor`：是一个方法级的权限过滤器, 基本位于过滤链的最底部 

 `ExceptionTranslationFilter`：是个异常过滤器，用来处理在认证授权过程中抛出的异常 

`UsernamePasswordAuthenticationFilter` ：对/login 的 POST 请求做拦截，校验表单中用户 名，密码。  

![02-Spring Security基本原理](images/02-SpringSecurity基本原理.png)

### 3.4 UserDetailsService接口讲解

 如果需要自定义逻辑时，只需要实现 UserDetailsService 接口即可。接口定义如下 :

![1638157113323](images/1638157113323.png)

 返回值 UserDetails  这个类是系统默认的用户“主体 ”   

```java
// 表示获取登录用户所有权限
Collection<? extends GrantedAuthority> getAuthorities();
// 表示获取密码
String getPassword();
// 表示获取用户名
String getUsername();
// 表示判断账户是否过期
boolean isAccountNonExpired();
// 表示判断账户是否被锁定
boolean isAccountNonLocked();
// 表示凭证{密码}是否过期
boolean isCredentialsNonExpired();
// 表示当前用户是否可用
boolean isEnabled();
```

以下是UserDetails的实现类

![1638157181426](images/1638157181426.png)

![1638157203695](images/1638157203695.png)




### 3.5 PasswordEncoder接口讲解

```java
// 表示把参数按照特定的解析规则进行解析
String encode(CharSequence rawPassword);
// 表示验证从存储中获取的编码密码与编码后提交的原始密码是否匹配。如果密码匹
配，则返回 true；如果不匹配，则返回 false。第一个参数表示需要被解析的密码。第二个
参数表示存储的密码。
boolean matches(CharSequence rawPassword, String encodedPassword);
// 表示如果解析的密码能够再次进行解析且达到更安全的结果则返回 true，否则返回
false。默认返回 false。
default boolean upgradeEncoding(String encodedPassword) {
return false;
}

```

 接口实现类  

![1638157412584](images/1638157412584.png)

 BCryptPasswordEncoder 是 Spring Security 官方推荐的密码解析器，平时多使用这个解析 器。 BCryptPasswordEncoder 是对 bcrypt 强散列方法的具体实现。是基于 Hash 算法实现的单 向加密。可以通过 strength 控制加密强度，默认 10. 

### 3.6 创建工程

创建maven工程security-spring-security

````java
@Configuration
@EnableWebMvc
@ComponentScan(basePackages = "com.binyu.security.springmvc", includeFilters = {@ComponentScan.Filter(type = FilterType.ANNOTATION, value = Controller.class)})
public class WebConfig implements WebMvcConfigurer {
    //视图解析器
    @Bean
    public InternalResourceViewResolver viewResolver() {
        InternalResourceViewResolver viewResolver = new InternalResourceViewResolver ();
        viewResolver.setPrefix ( "/WEB-INF/view/" );
        viewResolver.setSuffix ( ".jsp" );
        return viewResolver;
    }

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController ( "/" ).setViewName ( "redirect:/login" );
    }


}
````



```xml
 <dependencies>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-web</artifactId>
            <version>5.1.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>5.1.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.1.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.18</version>
        </dependency>

    </dependencies>
```

### 3.7 认证

```java
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    // 定义用户信息服务（查询用户信息）
    @Bean
    @Override
    public UserDetailsService userDetailsService(){
        InMemoryUserDetailsManager inMemoryUserDetailsManager=new InMemoryUserDetailsManager (  );
        inMemoryUserDetailsManager.createUser ( User.withUsername ( "zhangsan" ).password ( "123" ).authorities ( "p1" ).build () );
        inMemoryUserDetailsManager.createUser ( User.withUsername ( "lisi" ).password ( "456" ).authorities ( "p2" ).build () );
        return inMemoryUserDetailsManager;
    }
    // 密码编码器
    @Bean
    public PasswordEncoder passwordEncoder(){
        return  NoOpPasswordEncoder.getInstance ();
    }
    // 安全拦截机制

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests ()
                .antMatchers ( "/r/**" ).authenticated ()// 所有/r/**的请求必须认证通过
                .anyRequest (  ).permitAll ()// 除了/r/**,其他请求可以访问
                .and ()
                .formLogin ()//允许表单登录
                .successForwardUrl ( "/login-success" );//自定义登录成功地址
    }
}
```

````java
public class SpringSecurityApplicationInitializer extends AbstractSecurityWebApplicationInitializer {
   public  SpringSecurityApplicationInitializer(){

   }
}
````

````java
public class SpringApplicationInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
    // spring容器,相当于加载applicationContext.xml
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{ApplicationConfig.class, WebSecurityConfig.class};
    }
  
}
````

### 3.8 授权

```java
http.authorizeRequests ()
                .antMatchers ( "/r/r1" ).hasAuthority ( "p1" )
                .antMatchers ( "/r/r2" ).hasAuthority ( "p2" )
```

## 4 Spring Security应用详解

### 4.1 集成Spring Boot

WebConfig和WebSecurityConfig注解都改为只有@Configuration

application.yml

```yml
server:
  port: 8080
  servlet:
    context-path: /security-springboot
spring:
  application:
    name: security-springboot
  mvc:
    view:
      prefix: /WEB-INF/views/
      suffix: .jsp
```

### 4.2 工作原理

![1623907278117](.\images\1623907278117.png)

![1623907358960](.\images\1623907358960.png)

SecurityContextPersistenceFilter：这个Filter是拦截过程的入口和出口（第一个也是最后一个拦截器）

UsernamePasswordAuthenticationFilter：用于处理来自表单提交的认证，该表单必须提供对应的用户名和密码，其内部还有登录成功或失败后进行处理得当AuthenticationSuccessHandler和AuthenticationFailureHandler，这些都可以根据需求做相关改变

FilterSecurityInterceptor：用于保护web资源的，使用AccessDecisionManager对当前用户进行授权访问

ExceptionTranslationFilter：能够捕获Filterchain的异常

#### 4.2.1 认证流程

![1623907725957](.\images\1623907725957.png)

UserDetailsService

```java
@Service
public class SpringDataUserDetailsService implements UserDetailsService {
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        //连接数据库根据账号查询用户信息
        UserDetails userDetails = User.withUsername ( "zhangsan" ).password ( "123" ).authorities ( "p1" ).build ();
        return userDetails;
    }
}
```

PasswordEncoder

```java
 @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder (  );
    }
```

```java
@Service
public class SpringDataUserDetailsService implements UserDetailsService {
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        //连接数据库根据账号查询用户信息
        UserDetails userDetails = User.withUsername ( "zhangsan" ).password ( "$2a$10$ssLgQ.z6P0u0xR868yyG2.sOeZnTc4wblSBlaFu5nIC1YYCfrSnDC" ).authorities ( "p1" ).build ();
        return userDetails;
    }
}
```



#### 4.2.2 授权流程

![1623912765530](.\images\1623912765530.png)

授权投票决策

AffirmativeBased

![1623913000878](.\images\1623913000878.png)

ConsensusBased

<img src=".\images\1623913017814.png" alt="1623913017814"  />

UnanimousBased

![1623913077641](.\images\1623913077641.png)

### 4.3 自定义认证

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {


    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController ( "/" ).setViewName ( "redirect:/login-view" );
        registry.addViewController ( "/login-view" ).setViewName ( "login" );
    }


}
```

```java
@Configuration
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    // 定义用户信息服务（查询用户信息）
//    @Bean
//    @Override
//    public UserDetailsService userDetailsService(){
//        InMemoryUserDetailsManager inMemoryUserDetailsManager=new InMemoryUserDetailsManager (  );
//        inMemoryUserDetailsManager.createUser ( User.withUsername ( "zhangsan" ).password ( "123" ).authorities ( "p1" ).build () );
//        inMemoryUserDetailsManager.createUser ( User.withUsername ( "lisi" ).password ( "456" ).authorities ( "p2" ).build () );
//        return inMemoryUserDetailsManager;
//    }
    // 密码编码器
//    @Bean
//    public PasswordEncoder passwordEncoder(){
//        return  NoOpPasswordEncoder.getInstance ();
//    }
    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder (  );
    }
    // 安全拦截机制

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf ().disable ()
                .authorizeRequests ()
                .antMatchers ( "/r/r1" ).hasAuthority ( "p1" )
                .antMatchers ( "/r/r2" ).hasAuthority ( "p2" )
                .antMatchers ( "/r/**" ).authenticated ()// 所有/r/**的请求必须认证通过
                .anyRequest (  ).permitAll ()// 除了/r/**,其他请求可以访问
                .and ()
                .formLogin ()//允许表单登录
                .loginPage ( "/login-view" )
                .loginProcessingUrl ( "/login" )
                .successForwardUrl ( "/login-success" );//自定义登录成功地址
    }
}
```

```html
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<html>
<head>
    <title>用户登录</title>
</head>
<body>
<form action="login" method="post">
    用户名:<input type="text" name="username"/><br>
    密码:<input type="password" name="password"/>
    <input type="submit" value="登录">
</form>
</body>
</html>
```

spring security为了防止CSRF（跨站请求伪造），限制了除了get以外的大多数方法

解决方法有：

```java
 http.csrf ().disable ()
```

```html
<form action="login" method="post">
    <input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}">
   </form>
```

### 4.4 会话

```java
private String getUsername(){
        String username;
        Authentication authentication = SecurityContextHolder.getContext ().getAuthentication ();
        Object principal = authentication.getPrincipal ();
        if(principal==null){
            username="匿名";
        }
        if(principal instanceof UserDetails){
            UserDetails userDetails=(UserDetails)principal;
            username=userDetails.getUsername ();
        }else{
            username=principal.toString ();
        }
        return username;
    }
```

![1623915821154](.\images\1623915821154.png)

![1623915901889](.\images\1623915901889.png)

![1623915973341](.\images\1623915973341.png)

### 4.5 退出

```java
http.logout().logoutUrl("/logout").logoutSuccessUrl("/login-view").addLogoutHandler(logoutHandler);
```

![1623916274933](.\images\1623916274933.png)

### 4.6 授权

修改数据库完成5个表，在dao中写查询语句查询某个用户的权限，在UserDetailsService中使用

#### 4.6.1 WEB授权

注意：规则的顺序是重要的，更具体的规则应该先写

```JAVA
.antMatchers("/admin/**").hasRole("ADMIN")
.antMatchers("/admin/login").permitAll()
```

#### 4.6.2 方法授权

如果

使用了这个，基于WEB的授权就可以注释掉了

```java
.antMatchers ( "/r/r1" ).hasAuthority ( "p1" )
 .antMatchers ( "/r/r2" ).hasAuthority ( "p2" )
```

@Secured
判断是否具有角色，另外需要注意的是这里匹配的字符串需要添加前缀“ROLE_“。
使用注解先要开启注解功能！

`@EnableGlobalMethodSecurity(securedEnabled=true)`

```java
// 测试注解：
@RequestMapping("testSecured") 
@ResponseBody
@Secured({"ROLE_normal","ROLE_admin"}) 
public String helloUser() { return "hello,user"; } 
```

**@PreAuthorize**
先开启注解功能： @EnableGlobalMethodSecurity(prePostEnabled = true) @PreAuthorize：注解适合进入方法前的权限验证， @PreAuthorize可以将登录用户的roles/permissions参数传到方法中。

```java
@RequestMapping("/preAuthorize") 
@ResponseBody 
//@PreAuthorize("hasRole('ROLE_管理员')") @PreAuthorize("hasAnyAuthority('menu:system')") 
public String preAuthorize(){ System.out.println("preAuthorize"); return "preAuthorize"; }
```

@PostAuthorize
先开启注解功能： @EnableGlobalMethodSecurity(prePostEnabled = true) @PostAuthorize 注解使用并不多，在方法执行后再进行权限验证，适合验证带有返回值的权限.

```java
@RequestMapping("/testPostAuthorize") 
@ResponseBody @PostAuthorize("hasAnyAuthority('menu:system')") 
public String preAuthorize(){
    System.out.println("test--PostAuthorize"); 
    return "PostAuthorize"; }
```

@PostFilter ：权限验证之后对数据进行过滤 留下用户名是admin1的数据
表达式中的 filterObject 引用的是方法返回值List中的某一个元素

```java
@RequestMapping("getAll") 
@PreAuthorize("hasRole('ROLE_管理员')") @PostFilter("filterObject.username == 'admin1'") @ResponseBody public List<UserInfo> getAllUser(){ ArrayList<UserInfo> list = new ArrayList<>();                              list.add(new UserInfo(1l,"admin1","6666"));                                             list.add(new UserInfo(2l,"admin2","888"));                                               return list; }
```

@PreFilter: 进入控制器之前对数据进行过滤

```java
@RequestMapping("getTestPreFilter") @PreAuthorize("hasRole('ROLE_管理员')") 
@PreFilter(value = "filterObject.id%2==0")
@ResponseBody public List<UserInfo> getTestPreFilter(@RequestBody List<UserInfo> list){ list.forEach(t-> { System.out.println(t.getId()+"\t"+t.getUsername()); });
 return list; }
```

### 4.7 基于数据库的记住我

 创建表 

```sql
CREATE TABLE `persistent_logins` (
 `username` varchar(64) NOT NULL,
 `series` varchar(64) NOT NULL,
 `token` varchar(64) NOT NULL,
 `last_used` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE 
CURRENT_TIMESTAMP,
 PRIMARY KEY (`series`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

 添加数据库的配置文件  

```yml
spring:
	datasource:
		driver-class-name: com.mysql.jdbc.Driver
		url: jdbc:mysql://192.168.200.128:3306/test
		username: root
		password: root

```

 编写配置类 

```java
@Configuration
public class BrowserSecurityConfig {
@Autowired
private DataSource dataSource;
@Bean
public PersistentTokenRepository persistentTokenRepository(){
 JdbcTokenRepositoryImpl jdbcTokenRepository = new 
JdbcTokenRepositoryImpl();
// 赋值数据源
jdbcTokenRepository.setDataSource(dataSource);
// 自动创建表,第一次执行会创建，以后要执行就要删除掉！
jdbcTokenRepository.setCreateTableOnStartup(true);
return jdbcTokenRepository;
 }
}

```

修改安全配置类

```java
@Autowired
private UsersServiceImpl usersService;
@Autowired
private PersistentTokenRepository tokenRepository;
// 开启记住我功能
http.rememberMe()
 .tokenRepository(tokenRepository)
 .userDetailsService(usersService);

```

 页面添加记住我复选框  

```html
记住我：<input type="checkbox"name="remember-me"title="记住密码"/><br/>
```

设置有效期

![1638158614029](images/1638158614029.png)

### 4.8 CSRF

 跨站请求伪造（英语：Cross-site request forgery），也被称为 one-click  attack 或者 session riding，通常缩写为 CSRF 或者 XSRF， 是一种挟制用户在当前已 登录的 Web 应用程序上执行非本意的操作的攻击方法。跟跨网站脚本（XSS）相比，XSS 利用的是用户对指定网站的信任，CSRF 利用的是网站对用户网页浏览器的信任。 跨站请求攻击，简单地说，是攻击者通过一些技术手段欺骗用户的浏览器去访问一个 自己曾经认证过的网站并运行一些操作（如发邮件，发消息，甚至财产操作如转账和购买 商品）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去运行。 这利用了 web 中用户身份验证的一个漏洞：简单的身份验证只能保证请求发自某个用户的 浏览器，却不能保证请求本身是用户自愿发出的。 从 Spring Security 4.0 开始，默认情况下会启用 CSRF 保护，以防止 CSRF 攻击应用 程序，Spring Security CSRF 会针对 PATCH，POST，PUT 和 DELETE 方法进行防护。 



 在登录页面添加一个隐藏域：

```HTML
<input 
type="hidden"th:if="${_csrf}!=null"th:value="${_csrf.token}"name="_csrf
"/>
```

关闭安全配置的类中的 csrf

```java
 // http.csrf().disable();  
```





### 4.9 Spring Security原理总结

 SpringSecurity 采用的是责任链的设计模式，它有一条很长的过滤器链。现在对这条过滤 器链的 15 个过滤器进行说明: 

（1） WebAsyncManagerIntegrationFilter：将 Security 上下文与 Spring Web 中用于 处理异步请求映射的 WebAsyncManager 进行集成。 

（2） SecurityContextPersistenceFilter：在每次请求处理之前将该请求相关的安全上 下文信息加载到 SecurityContextHolder 中，然后在该次请求处理完成之后，将 SecurityContextHolder 中关于这次请求的信息存储到一个“仓储”中，然后将 SecurityContextHolder 中的信息清除，例如在 Session 中维护一个用户的安全信 息就是这个过滤器处理的。 

（3） HeaderWriterFilter：用于将头信息加入响应中 

 （4） CsrfFilter：用于处理跨站请求伪造。 

（5）LogoutFilter：用于处理退出登录。 

（6）UsernamePasswordAuthenticationFilter：用于处理基于表单的登录请求，从表单中 获取用户名和密码。默认情况下处理来自 /login 的请求。从表单中获取用户名和密码 时，默认使用的表单 name 值为 username 和 password，这两个值可以通过设置这个 过滤器的 usernameParameter 和 passwordParameter 两个参数的值进行修改。 

（7）DefaultLoginPageGeneratingFilter：如果没有配置登录页面，那系统初始化时就会 配置这个过滤器，并且用于在需要进行登录时生成一个登录表单页面。 

（8）BasicAuthenticationFilter：检测和处理 http basic 认证。 

（9）RequestCacheAwareFilter：用来处理请求的缓存。 

（10）SecurityContextHolderAwareRequestFilter：主要是包装请求对象 request。 

（11）AnonymousAuthenticationFilter：检测 SecurityContextHolder 中是否存在 Authentication 对象，如果不存在为其提供一个匿名 Authentication。 

（12）SessionManagementFilter：管理 session 的过滤器 

（13）ExceptionTranslationFilter：处理 AccessDeniedException 和 AuthenticationException 异常。 

（14）FilterSecurityInterceptor：可以看做过滤器链的出口。

 （15）RememberMeAuthenticationFilter：当用户没有登录而直接访问资源时, 从 cookie  里找出用户的信息, 如果 Spring Security 能够识别出用户提供的 remember me cookie,  用户将不必填写用户名和密码, 而是直接登录进入系统，该过滤器默认不开启。  



![1638159068477](images/1638159068477.png)

### 4.10 Spring Security页面端标签控制权限

[CSDN-Spring Security页面端标签控制权限](https://blog.csdn.net/qq_39182939/article/details/104588193)

### 附录 常见HttpSecurity API

openidLogin()

用于基于 OpenId 的验证

headers()

将安全标头添加到响应

cors()

配置跨域资源共享（ CORS ）

sessionManagement()

允许配置会话管理

portMapper()

允许配置一个PortMapper(HttpSecurity#(getSharedObject(class)))，其他提供SecurityConfigurer的对象使用 PortMapper 从 HTTP 重定向到 HTTPS 或者从 HTTPS 重定向到 HTTP。默认情况下，Spring Security使用一个PortMapperImpl映射 HTTP 端口8080到 HTTPS 端口8443，HTTP 端口80到 HTTPS 端口443

jee()

配置基于容器的预认证。 在这种情况下，认证由Servlet容器管理

x509()

配置基于x509的认证

rememberMe

允许配置“记住我”的验证

authorizeRequests()

允许基于使用HttpServletRequest限制访问

requestCache()

允许配置请求缓存

exceptionHandling()

允许配置错误处理

securityContext()

在HttpServletRequests之间的SecurityContextHolder上设置SecurityContext的管理。 当使用WebSecurityConfigurerAdapter时，这将自动应用

servletApi()

将HttpServletRequest方法与在其上找到的值集成到SecurityContext中。 当使用WebSecurityConfigurerAdapter时，这将自动应用

csrf()

添加 CSRF 支持，使用WebSecurityConfigurerAdapter时，默认启用

logout()

添加退出登录支持。当使用WebSecurityConfigurerAdapter时，这将自动应用。默认情况是，访问URL”/ logout”，使HTTP Session无效来清除用户，清除已配置的任何#rememberMe()身份验证，清除SecurityContextHolder，然后重定向到”/login?success”

anonymous()

允许配置匿名用户的表示方法。 当与WebSecurityConfigurerAdapter结合使用时，这将自动应用。 默认情况下，匿名用户将使用org.springframework.security.authentication.AnonymousAuthenticationToken表示，并包含角色 “ROLE_ANONYMOUS”

formLogin()

指定支持基于表单的身份验证。如果未指定FormLoginConfigurer#loginPage(String)，则将生成默认登录页面

oauth2Login()

根据外部OAuth 2.0或OpenID Connect 1.0提供程序配置身份验证

requiresChannel()

配置通道安全。为了使该配置有用，必须提供至少一个到所需信道的映射

httpBasic()

配置 Http Basic 验证

addFilterAt()

在指定的Filter类的位置添加过滤器

## 5 分布式系统认证方案

### 5.1 什么是分布式

 简单的说，微服务是架构设计方式，分布式是系统部署方式，两者概念不同 

 **微服务**就是很小的服务，小到一个服务只对应一个单一的功能，只做一件事。这个服务可以单独部署运行，服务之间可以通过RPC来相互交互，每个微服务都是由独立的小团队开发，测试，部署，上线，负责它的整个生命周期。 

**分布式服务**顾名思义服务是分散部署在不同的机器上的，一个服务可能负责几个功能，是一种面向SOA架构的，服务之间也是通过rpc来交互或者是webservice来交互的。逻辑架构设计完后就该做物理架构设计，系统应用部署在超过一台服务器或虚拟机上，且各分开部署的部分彼此通过各种通讯协议交互信息，就可算作分布式部署，生产环境下的微服务肯定是分布式部署的，分布式部署的应用不一定是微服务架构的，比如集群部署，它是把相同应用复制到不同服务器上，但是逻辑功能上还是单体应用。

### 5.2 分布式认证需求

如果每个服务都实现一套认证授权逻辑会非常冗余，由于分布式系统共享性的特点，所以：

**统一认证授权**：提供独立的认证服务，无论是不同类型的用户还是不同种类的客户端（web端，H5，APP）均采用一直的认证、权限、会话机制。要实现统一的认证方式必须可拓展，支持各种认证需求，比如：用户名密码认证、短信验证码、二维码、人脸识别等认证方式，并可以灵活的切换

**应用接入认证**：应提供拓展和开发能力，提供安全的系统对接机制，并可开发部分API给接入第三方使用，一方应用（内部系统服务）和三方应用均采用统一机制接入

### 5.3 分布式认证方式

1. 基于session的认证方式：

![1624077284947](.\images\1624077284947.png)

**Session复制**：多台应用服务器之间同步session，使session保持一致，对外透明

**Session黏贴**：当用户访问集群中某台服务器后，强制指定后续所有请求均落在此机器上

**Session集中存储**：将session存入分布式缓存中，所有服务器应用实例统一从分布式缓存中存取session

总体来说，基于session认证的认证方式，可以更好的在服务端对会话进行控制，且安全性较高。但是，session机制方式基于cookie，在复杂多样的移动客户端上不能有效使用，并且无法跨域，另外随着系统的拓展需提高session的复制、黏贴和存储的容错性。

2. 基于token的认证方式

基于token的认证方式，服务端不用存储认证数据，易维护拓展性强，客户端可以把token存在任意地方，并且可以实现web和APP统一认证机制。其缺点也很明显，token由于自包含信息，因此一般数据量较大，而且每次请求都需要传递，因此比较占带宽。另外，token的签名验签操作也会给CPU带来额外的处理负担。

![1624077849425](.\images\1624077849425.png)

决定采用基于token的认证方式，优点是：

1. 适合统一认证的机制
2. token认证方式对第三方应用接入更适合
3. 一般服务端无需存储会话信息，减轻服务端压力

![1624078075806](.\images\1624078075806.png)

## 6 OAuth2.0

### 6.1 介绍

 OAuth是一个开放标准，允许用户授权第三方应用（例如黑马程序员网站）访问他们存储在另外服务提供者（例如微信登录）上的信息，而不需要将用户名和密码提供给第三方应用或分享他们数据的所有内容

![1624078583689](.\images\1624078583689.png)

授权服务器对两种OAuth2.0的角色进行认证授权，分别是资源拥有者、客户端

![1638003776408](images/1638003776408.png)

### 6.2 Spring Cloud Security OAuth2.0

[OAuth 2 开发人员指南](https://projects.spring.io/spring-security-oauth/docs/oauth2.html)

[CSDN-四种授权模式](https://blog.csdn.net/zyt807/article/details/103386120)

![1624082980449](.\images\1624082980449.png)

授权服务器总结：

![1640767024171](images/1640767024171.png)

使用token获取资源：

```
$ curl  -H "Authorization:bearer 284a5718-0a80-4eab-9d04-1bda3b6ceb62" -X GET  http://localhost:8080/user/get
{"error":"invalid_token","error_description":"Invalid access token: 284a5718-0a80-4eab-9d04-1bda3b6ceb62"}
或者
$ curl -X GET  http://localhost:8080/user/get?access_token=0f22ff90-1834-4654-9576-8737ec84d61e
```

#### 授权码方式

第一步：http://localhost:8201/oauth/authorize?client_id=c1&redirect_uri=http://www.baidu.com&response_type=code

其中 

- client_id对应数据库oauth_client_details中的client_id
- redirect_url对应数据库oauth_client_details的web_server_redirect_url
- response_type代表授权方式

![1640785591975](images/1640785591975.png)

之后跳转到http://www.baidu.com?code=HAsA2q获取到授权码

第二步： http://localhost:8201/oauth/token?grant_type=authorization_code&client_id=c1&client_secret=123123&redirect_uri=http://www.baidu.com&code=VQjV5g **注意**：必须要发POST请求

#### 密码模式

发请求 http://localhost:8201/oauth/token?client_id=c1&client_secret=123123&grant_type=password&username=dby321&password=123 

**注意**：必须要发POST请求

#### 简化模式

发请求http://localhost:8201/oauth/authorize?response_type=token&client_id=c1&redirect_uri=http://www.baidu.com

**注意**：可以发GET请求

然后会要求登录，登录后进行授权

#### 客户端模式

发请求 http://localhost:8201/oauth/token?client_id=c1&client_secret=123123&grant_type=client_credentials 

**注意**：必须要发POST请求

此时principal存的是客户端id