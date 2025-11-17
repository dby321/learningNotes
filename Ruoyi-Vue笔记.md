# Ruoyi-Vue笔记

## Bat入门到精通

### 命令详解

- `::` 是一种 **写注释** 的方式。
- `echo` 相当于打印print
- `exit /b 1` 以错误码1退出
- `%0` 表示批处理文件自身路径。
- `%~d0` 提取该批处理文件所在的盘符（如 `C:`）
- `%~dp0`是组合用法：
  - `%~d0`：盘符（如 C:）
  - `%~p0`：路径（不包含盘符，如 `\path\to\script\`）
  - 合起来就是批处理文件的完整所在目录路径。
- `call mvn clean` 执行命令
- `set JAVA_OPTS=-Xms256m -Xmx1024m -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=512m ` 设置变量
- `java -jar %JAVA_OPTS% ruoyi-admin.jar` 执行java进程
- `-Dmaven.test.skip=true` maven配置 跳过测试检查

### clean.bat

```bat
@echo off
echo.
echo [信息] 正在清理工程target生成路径...
echo.

:: 跳转到脚本所在目录
%~d0
cd %~dp0

:: 回到项目根目录并执行清理
cd .. 
if exist "pom.xml" (
    call mvn clean
) else (
    echo [错误] 未找到 pom.xml，可能不在正确的项目路径！
    exit /b 1
)

echo.
echo [完成] 清理完毕。
pause
```

### package.bat

```bat
@echo off
echo.
echo [信息] 打包Web工程，生成war/jar包文件。
echo.

%~d0
cd %~dp0

cd ..
call mvn clean package -Dmaven.test.skip=true

pause
```

### run.bat

```bat
@echo off
echo.
echo [信息] 使用Jar命令运行Web工程。
echo.

cd %~dp0
cd ../ruoyi-admin/target

set JAVA_OPTS=-Xms256m -Xmx1024m -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=512m

java -jar %JAVA_OPTS% ruoyi-admin.jar

cd bin
pause
```

## Vue.config.js

### publicPath

```java
publicPath: process.env.NODE_ENV === "production" ? "/admin/" : "/"
publicPath: process.env.NODE_ENV === "production" ? "/" : "/",
```

| 配置                     | 适用场景                                         |
|  |  |
| `publicPath: '/'`        | 部署在域名根目录                                 |
| `publicPath: './'`       | 本地预览、子目录部署、GitHub Pages（✔ 推荐通用） |
| `publicPath: '/子目录/'` | 明确部署在某个子路径下                           |

### mode:hash还是history

```java
export default new Router({
  mode: 'history', // 去掉url中的#
  scrollBehavior: () => ({y: 0}),
  routes: constantRoutes
})
```

## nginx配置详解

### 🌐 全局配置部分

```nginx
worker_processes  1;
```
- **含义**：启动 1 个 Nginx 工作进程（worker process）。
- **说明**：通常设置为 CPU 核心数，单核环境或测试环境下设为 1 是合理的。

```nginx
events {
    worker_connections  1024;
}
```
- **含义**：每个工作进程最多可以同时处理 1024 个连接。
- **说明**：这是 Nginx 的事件模型配置，决定了并发处理能力。

### 📦 HTTP 模块开始（Web 服务主模块）

```nginx
http {
    include       mime.types;                    # 包含 MIME 类型映射表，让 Nginx 正确识别返回内容类型
    default_type  application/octet-stream;      # 默认响应类型
    sendfile        on;                          # 启用高效文件传输模式（适用于静态资源）
    keepalive_timeout  65;                       # 客户端连接保持时间（秒）
```

这些是常见的性能优化与基础设置：
- `include mime.types`：确保 `.html`, `.css`, `.js` 等文件能被正确识别并返回正确的 `Content-Type`。
- `sendfile on`：提升静态文件传输效率。
- `keepalive_timeout`：允许客户端复用 TCP 连接，提高性能。

### 🏗️ Server 块（虚拟主机配置）

```nginx
server {
    listen       80;                      # 监听 80 端口（HTTP 默认端口）
    server_name  localhost;              # 绑定域名 localhost
    charset utf-8;                       # 设置默认字符集为 UTF-8，避免中文乱码
```

这部分定义了一个网站的服务入口：
- 当用户访问 `http://localhost` 时，由这个 server 块处理请求。
- 使用 UTF-8 编码支持中文等多语言字符。

### 📍 Location 路由规则

#### ✅ 静态资源根路径 `/`

```nginx
location / {
    root   /home/ruoyi/projects/ruoyi-ui;     # 文件根目录
    try_files $uri $uri/ /index.html;         # 支持前端路由（SPA）
    index  index.html index.htm;
}
```

- `root /home/ruoyi/projects/ruoyi-ui;`：指定网页文件存放路径。
- `try_files $uri $uri/ /index.html;`：
  - 尝试按原路径找文件（如 `/about` → `/about` 文件或目录）；
  - 如果找不到，则返回 `/index.html`。
  - **用途**：支持基于 HTML5 History 的单页应用（SPA）路由（比如 Vue、React 前端项目）。
- `index`：默认首页文件。

> 👉 这意味着你部署的是一个前端项目（可能是 RuoYi-Vue 或类似管理系统前端）。

#### 🔁 反向代理 `/prod-api/` 请求到后端

```nginx
location /prod-api/ {
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header REMOTE-HOST $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_pass http://localhost:8080/;
}
```

- 所有以 `/prod-api/` 开头的请求都会被转发到本地 `8080` 端口的应用（通常是 Java Spring Boot 后端）。
- 示例：
  - 请求 `http://localhost/prod-api/user/list`
  - 实际转发到：`http://localhost:8080/user/list`

##### 头部设置说明：
| 指令                               | 作用                         |
| - | - |
| `proxy_set_header Host $http_host` | 保留原始 Host 头             |
| `X-Real-IP` / `X-Forwarded-For`    | 让后端获取真实客户端 IP 地址 |
| `REMOTE-HOST`                      | 可选，记录来源主机           |

> ⚠️ 注意：URL 末尾是否有 `/` 很关键。这里 `proxy_pass` 结尾有 `/`，所以 `/prod-api/foo` → `/foo`。

#### 📘 API 文档代理（SpringDoc OpenAPI）

```nginx
location ~ ^/v3/api-docs/(.*) {
    proxy_pass http://localhost:8080/v3/api-docs/$1;
}
```

- 使用正则匹配 `/v3/api-docs/...` 路径。
- 将请求代理到后端的 Swagger/OpenAPI 接口文档服务（Spring Boot + SpringDoc）。
- `$1` 表示捕获括号中的内容，实现动态路径转发。

> 💡 例如：访问 `/v3/api-docs/app` → 转发到 `http://localhost:8080/v3/api-docs/app`

这样可以在不暴露后端端口的情况下查看接口文档。

### ❌ 错误页面处理

```nginx
error_page   500 502 503 504  /50x.html;
location = /50x.html {
    root   html;
}
```

- 当发生 5xx 服务器错误时，显示 Nginx 自带的 `50x.html` 页面。
- 页面位于 Nginx 安装目录下的 `html/50x.html`。

### ✅ 总结：这个配置的作用

这是一个典型的 **前后端分离架构** 中 Nginx 的部署配置：

| 功能               | 说明                                                         |
| -- | -- |
| 🖼️ 前端静态资源服务 | 提供 `/home/ruoyi/projects/ruoyi-ui` 下的前端页面（Vue/React） |
| 🔙 后端 API 代理    | 将 `/prod-api/**` 请求转发给运行在 `8080` 端口的 Spring Boot 应用 |
| 📚 API 文档代理     | 支持通过 Nginx 访问 Swagger UI 的数据接口 `/v3/api-docs`     |
| 🧱 单页应用支持     | `try_files` 实现前端路由刷新不 404                           |
| 🛡️ 安全与日志       | 传递客户端真实 IP 给后端                                     |

### 🔄 典型应用场景（如 RuoYi 框架）

假设你在使用 [RuoYi](https://gitee.com/y_project/RuoYi-Vue) 这类前后端分离项目：

- 前端：`ruoyi-ui` （端口 80）
- 后端：`ruoyi-admin` （端口 8080）
- 访问方式：
  - 浏览器访问 `http://localhost` → 加载前端页面
  - 前端发起请求 `/prod-api/login` → 被 Nginx 转发到 `http://localhost:8080/login`
  - 查看文档 `http://localhost/v3/api-docs/system` → 显示后端接口信息

✅ **建议改进点（生产环境）**：

1. 使用 `worker_processes auto;` 更好地利用多核 CPU。
2. 添加 HTTPS (`listen 443 ssl`)。
3. 设置 `access_log off;` 对静态资源减少日志输出。
4. 增加缓存控制（如对 JS/CSS 设置 long cache）。
5. 限制敏感路径访问（如禁止访问 `.git`、`.env` 等）。

# Ruoyi-Vue后端笔记

## OpenFeign

[ Ruoyi-OpenFeign 支持](https://doc.ruoyi.vip/ruoyi-cloud/cloud/sentinel.html#openfeign-%E6%94%AF%E6%8C%81)

> 依赖和@EnableRyFeignClients通常已经加上了

```java
@FeignClient(contextId = "remoteUserService", value = ServiceNameConstants.SYSTEM_SERVICE, fallbackFactory = RemoteUserFallbackFactory.class)
public interface RemoteUserService
{
    /**
     * 通过用户名查询用户信息
     *
     * @param username 用户名
     * @return 结果
     */
    @GetMapping(value = "/user/info/{username}")
    public R<LoginUser> getUserInfo(@PathVariable("username") String username);
}
```

```java
@Component
public class RemoteUserFallbackFactory implements FallbackFactory<RemoteUserService>
{
    private static final Logger log = LoggerFactory.getLogger(RemoteUserFallbackFactory.class);

    @Override
    public RemoteUserService create(Throwable throwable)
    {
        log.error("用户服务调用失败:{}", throwable.getMessage());
        return new RemoteUserService()
        {
            @Override
            public R<LoginUser> getUserInfo(String username)
            {
                return R.fail("获取用户失败:" + throwable.getMessage());
            }
        };
    }
}
```

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class TestUserController
{
    @Autowired
    private RemoteUserService remoteUserService;

    /**
     * 获取当前用户信息
     */
    @GetMapping("/user/{username}")
    public Object info(@PathVariable("username") String username)
    {
        return remoteUserService.getUserInfo(username);
    }
}
```





### 规范的降级处理

```java
@Override
public R<Boolean> remoteXxx() {
    return R.fail(false);
}
@Override
public R<String> afterXxx(SysDept sysDept) {
    return R.fail("服务调用失败");
}
@Override
public AjaxResult sendMsg(WxTemplateMsgPost msg) {
    return AjaxResult.error("服务调用失败");
}
```

### R和AjaxResult

让我来解释一下项目中 AjaxResult 和 R 的区别：

#### AjaxResult

AjaxResult 通常用于 Web 层的 AJAX 请求响应，具有以下特点：

```java
// 典型的 AjaxResult 结构
public class AjaxResult {
    private int code;          // 状态码 (200成功, 500错误等)
    private String msg;        // 消息描述
    private Object data;       // 返回数据
    
    // 常用方法
    public static AjaxResult success() { ... }
    public static AjaxResult error(String msg) { ... }
    public static AjaxResult success(Object data) { ... }
}
```


**使用场景**：
- Controller 层直接返回给前端页面
- RESTful API 接口响应
- AJAX 异步请求响应

**示例响应**：
```json
{
  "code": 200,
  "msg": "操作成功",
  "data": {
    "userId": 1,
    "userName": "张三"
  }
}
```


#### R

R 通常是一个更通用的统一响应类，具有以下特点：

```java
// 典型的 R 结构
public class R<T> {
    private int code;          // 业务状态码
    private String msg;        // 业务消息
    private T data;            // 泛型数据
    
    // 常用方法
    public static <T> R<T> ok(T data) { ... }
    public static <T> R<T> fail(String msg) { ... }
    public static <T> R<T> fail(T data) { ... }
}
```


**使用场景**：
- 微服务间调用的统一响应格式
- 业务层返回结果的封装
- 更广泛的系统内部调用场景

**示例响应**：
```json
{
  "code": 200,
  "msg": "success",
  "data": true
}
```


#### 主要区别

| 特性         | AjaxResult          | R                    |
| ------------ | ------------------- | -------------------- |
| **用途**     | 主要用于Web前端响应 | 更通用的业务响应封装 |
| **数据类型** | Object类型          | 泛型T，类型安全      |
| **适用范围** | Web层               | 各层均可使用         |
| **设计目标** | AJAX请求响应        | 统一的业务结果封装   |

#### 实际应用建议

在项目中应该根据使用场景选择：

1. **Controller层对外接口**：推荐使用 AjaxResult
2. **微服务间调用**：推荐使用 R
3. **业务逻辑层返回**：可以使用 R
4. **Feign客户端接口**：通常使用 R

这样可以保持系统的清晰分层和响应格式的一致性。

## 日志

这段代码是使用SLF4J框架创建一个日志记录器(Logger)的语句。让我详细解释一下：

```java
private static final Logger log = LoggerFactory.getLogger(RemoteAuthFallBackFactory.class);
```


这行代码的作用是：

1. **Logger**: 这是SLF4J日志框架中的核心接口，用于记录应用程序的日志信息。

2. **LoggerFactory.getLogger()**: 这是SLF4J提供的工厂方法，用于获取Logger实例。

3. **RemoteAuthFallBackFactory.class**: 传入当前类的Class对象，这样创建的Logger会与这个特定的类关联。在日志输出时，可以清楚地知道是哪个类产生的日志。

4. **private static final**: 
   - `private`: 保证这个日志记录器只能在当前类内部使用
   - `static`: 保证在类加载时只创建一个Logger实例，所有该类的实例共享同一个Logger
   - `final`: 保证这个引用不会被修改

这种写法是Java中使用SLF4J日志框架的标准模式，可以高效地记录日志，并且便于日志管理与追踪。在后续代码中，可以通过`log.error()`、`log.info()`、`log.debug()`等方法来记录不同级别的日志信息。

基于代码中的日志记录方式，生成的日志通常会像这样：

```
2025-11-11 14:30:25.123 ERROR 12345 --- [nio-8080-exec-1] c.i.a.f.RemoteAuthFallBackFactory : 远程服务调用失败:Connection refused
```


让我解释一下日志的各个部分：

1. **时间戳**: `2025-11-11 14:30:25.123` - 日志记录的时间
2. **日志级别**: ERROR - 表示这是一个错误级别的日志
3. **进程ID**: `12345` - 应用程序的进程ID
4. **线程信息**: `[nio-8080-exec-1]` - 产生日志的线程名称
5. **日志记录器名称**: `c.i.a.f.RemoteAuthFallBackFactory` - 简化的类名（通常是包名首字母缩写+类名）
6. **日志内容**: `远程服务调用失败:Connection refused` - 具体的日志信息，其中"Connection refused"是throwable.getMessage()的值

在我们之前脱敏后的代码中，当日志被记录时会显示"远程服务调用失败"，并附带具体的异常信息（如连接被拒绝、超时等）。这样的日志格式有助于开发者快速定位和诊断问题。

### SLF4J日志门面框架

SLF4J（Simple Logging Facade for Java）是一个日志门面框架，它本身不提供具体的日志实现，而是提供了统一的日志接口。以下是几种常见的SLF4J实现：

1. **Logback**（推荐）
   - 由SLF4J的作者开发，是SLF4J的原生实现
   - 性能优秀，功能强大
   - 配置简单，支持XML配置

2. **Log4j 2**
   - Apache的Log4j的下一代产品
   - 性能比Log4j 1.x大幅提升
   - 支持异步日志，功能丰富

3. **Java Util Logging**（JUL）
   - Java标准库自带的日志框架
   - 无需引入额外依赖
   - 功能相对简单

4. **Log4j 1.x**
   - 经典的日志框架，但已不再积极维护
   - 由于存在一些安全问题，不建议在新项目中使用

5. **Simple Logger**
   - SLF4J提供的简单实现
   - 主要用于测试或简单场景

在实际项目中，Logback和Log4j 2是最常用的选择。从您项目中的`logback.xml`文件可以看出，您的项目使用的是Logback作为SLF4J的具体实现。

### Logback.xml详情

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration scan="true" scanPeriod="60 seconds" debug="false">
```


这是配置文件的根节点，开启了自动扫描功能，每60秒检查一次配置文件是否有更新，debug设为false表示不输出logback内部调试信息。

```xml
    <!-- 日志存放路径 -->
	<property name="log.path" value="logs/ivu-village" />
   <!-- 日志输出格式 -->
	<property name="log.pattern" value="%d{HH:mm:ss.SSS} [%thread] %-5level %logger{20} - [%method,%line] - %msg%n" />
```


这里定义了两个属性：
1. `log.path`: 设置日志文件存储路径为 `logs/ivu-village`
2. `log.pattern`: 定义日志输出格式，包含时间、线程名、日志级别、日志记录器名、方法名、行号和消息内容
   1. `%d{HH:mm:ss.SSS}` - 日期时间格式
      输出格式：14:30:25.123
      显示小时:分钟:秒.毫秒
   2. `[%thread]` - 线程名
      输出格式：[http-nio-8080-exec-1]
      显示产生日志的线程名称，用方括号包围
   3. `%-5level` - 日志级别
      输出格式：INFO（注意后面有空格补齐到5位）
      显示日志级别（TRACE, DEBUG, INFO, WARN, ERROR）表示左对齐，5 表示最小宽度为5个字符
   4. `%logger{20}` - 日志记录器名称
      输出格式：c.i.m.s.SomeService
      显示 Logger 的名称，{20} 表示最大宽度为20个字符
      超过长度时会进行缩写，如 com.ivu.module.service.SomeService 会缩写为 c.i.m.s.SomeService
   5. `[%method,%line]` - 方法名和行号
       输出格式：[someMethod,123]
       显示产生日志的方法名和代码行号，用方括号包围
   6. `%msg` - 日志消息内容
       输出格式：用户登录成功
       显示实际的日志内容
   7. `%n` - 换行符
       输出格式：换行
       表示一行日志结束，换到下一行




```xml
    <!-- 控制台输出 -->
	<appender name="console" class="ch.qos.logback.core.ConsoleAppender">
		<encoder>
			<pattern>${log.pattern}</pattern>
		</encoder>
	</appender>
```


定义了一个名为 `console` 的控制台输出器，使用之前定义的日志格式将日志输出到控制台。

```xml
    <!-- 系统日志输出 -->
	<appender name="file_info" class="ch.qos.logback.core.rolling.RollingFileAppender">
	    <file>${log.path}/info.log</file>
        <!-- 循环政策：基于时间创建日志文件 -->
		<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- 日志文件名格式 -->
			<fileNamePattern>${log.path}/info.%d{yyyy-MM-dd}.log</fileNamePattern>
			<!-- 日志最大的历史 180天 -->
			<maxHistory>180</maxHistory>
		</rollingPolicy>
		<encoder>
			<pattern>${log.pattern}</pattern>
		</encoder>
		<filter class="ch.qos.logback.classic.filter.LevelFilter">
            <!-- 过滤的级别 -->
            <level>INFO</level>
            <!-- 匹配时的操作：接收（记录） -->
            <onMatch>ACCEPT</onMatch>
            <!-- 不匹配时的操作：拒绝（不记录） -->
            <onMismatch>DENY</onMismatch>
        </filter>
	</appender>
```


定义了一个名为 `file_info` 的文件输出器：
- 使用滚动策略，每天生成一个新的日志文件
- 文件命名格式为 `info.日期.log`
- 保留最近180天的日志文件
- 只记录INFO级别的日志

```xml
    <appender name="file_error" class="ch.qos.logback.core.rolling.RollingFileAppender">
	    <file>${log.path}/error.log</file>
        <!-- 循环政策：基于时间创建日志文件 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- 日志文件名格式 -->
            <fileNamePattern>${log.path}/error.%d{yyyy-MM-dd}.log</fileNamePattern>
			<!-- 日志最大的历史 60天 -->
			<maxHistory>180</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>${log.pattern}</pattern>
        </encoder>
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <!-- 过滤的级别 -->
            <level>ERROR</level>
			<!-- 匹配时的操作：接收（记录） -->
            <onMatch>ACCEPT</onMatch>
			<!-- 不匹配时的操作：拒绝（不记录） -->
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>
```


定义了一个名为 `file_error` 的文件输出器，专门记录ERROR级别日志，配置与 `file_info` 类似。

```xml
    <!-- 系统模块日志级别控制  -->
	<logger name="com.ivu" level="warn" />
	<!-- Spring日志级别控制  -->
	<logger name="org.springframework" level="warn" />
```


设置特定包的日志级别：
- `com.ivu` 包下的日志级别为 WARN
- `org.springframework` 包下的日志级别为 WARN

```xml
	<root level="warn">
		<appender-ref ref="console" />
	</root>
	
	<!--系统操作日志-->
    <root level="warn">
        <appender-ref ref="file_info" />
        <appender-ref ref="file_error" />
    </root>
```


配置根日志记录器，日志级别为 WARN，并关联前面定义的三个输出器（console、file_info 和 file_error）。

总的来说，这个配置实现了以下功能：
1. 在控制台和文件中输出日志
2. 将 INFO 和 ERROR 级别日志分别存储到不同的文件中
3. 按天滚动日志文件，保留180天的历史记录
4. 设置应用相关包的日志级别为 WARN，减少不必要的日志输出

### throwable.printStackTrace()优化写法

#### 优化前

```java
try {
    // 一些操作
} catch (Exception e) {
    e.printStackTrace();  // 直接输出到控制台
}
```

输出

```java
java.lang.NullPointerException
    at com.example.MyClass.someMethod(MyClass.java:25)
    at com.example.MyClass.main(MyClass.java:10)
```

#### 优化后

> 直接日志打印`e`包含完整堆栈信息，**推荐使用**
>
> `e.getMessage()`只包含异常消息部分，不建议使用

```java
try {
    // 一些操作
} catch (Exception e) {
    log.error("业务处理异常:", e);  // 通过日志系统记录
}

```

输出

```java
2025-11-11 14:30:25.123 ERROR [http-nio-8080-exec-1] c.e.MyClass - 业务处理异常: 
java.lang.NullPointerException
    at com.example.MyClass.someMethod(MyClass.java:25)
    at com.example.MyClass.main(MyClass.java:10)

```

## Maven

### mvn dependency:tree

#### 1. 显示完整依赖树
```bash
mvn dependency:tree
```


#### 2. 显示特定 artifactId 的依赖
```bash
mvn dependency:tree "-Dincludes=*:log4j-api"
mvn dependency:tree "-Dincludes=*:log4j-core"
```


#### 3. 显示多个依赖
```bash
mvn dependency:tree "-Dincludes=org.apache.logging.log4j, ch.qos.logback"
```


#### 4. 排除特定依赖
```bash
mvn dependency:tree "-Dexcludes=org.apache.logging.log4j:log4j-core"
```


#### 5. 输出到文件
```bash
mvn dependency:tree > dependency-tree.txt
```


#### 6. 显示所有传递依赖
```bash
mvn dependency:tree -Dverbose
```


#### 7. 查找冲突依赖
```bash
mvn dependency:tree -Dverbose -Dincludes=org.apache.logging.log4j
```

### mvn dependency:tree实战

```bash
mvn dependency:tree "-Dincludes=org.apache.logging.log4j"
```

运行命令得知

```cmd
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ ivu-common-core ---
[INFO] com.ivu:ivu-common-core:jar:2.5.0
[INFO] \- com.github.pagehelper:pagehelper-spring-boot-starter:jar:1.3.0:compile
[INFO]    \- org.springframework.boot:spring-boot-starter:jar:2.3.7.RELEASE:compile
[INFO]       \- org.springframework.boot:spring-boot-starter-logging:jar:2.3.7.RELEASE:compile
[INFO]          \- org.apache.logging.log4j:log4j-to-slf4j:jar:2.13.3:compile
[INFO]             \- org.apache.logging.log4j:log4j-api:jar:2.13.3:compile
```

#### 排除2.13.3的log4j-api的原因

这是一个很好的问题。从你的配置来看，项目排除了 Spring Boot 默认提供的 Log4j 2.13.3 版本，然后显式引入了 Log4j 2.15.0 版本，主要原因可能是：

##### 安全漏洞修复

Log4j 2.15.0 版本主要是为了修复一个严重的安全漏洞：

**CVE-2021-44228 (Log4Shell)** - 这是在 2021 年底发现的一个严重远程代码执行漏洞，影响 Log4j 2.x 版本。2.15.0 版本是第一个修复此漏洞的版本。

##### 配置分析

在你的 pom.xml 中：

```xml
<!-- 排除 Spring Boot Actuator 中的 Log4j 2.13.3 -->
<exclusion>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
</exclusion>

<!-- 显式引入 Log4j 2.15.0 -->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.15.0</version>
</dependency>
```


这种方式确保项目使用的是修复了安全漏洞的 Log4j 版本，而不是 Spring Boot 默认提供的较旧版本。

不过需要注意的是，现在已经有更新的 Log4j 版本（2.17.0+）进一步修复了后续发现的相关漏洞，如果需要的话可以考虑升级到更安全的版本。

#### 整个项目并没有配置log4j的xml啊，是不是说明log4j没有生效

你说得非常对！这是一个关键的观察点。

如果项目中没有 `log4j2.xml` 配置文件，而且你之前看到的配置文件是 `logback.xml`，这说明：

##### 实际情况分析

1. **Log4j 没有真正生效**：虽然引入了 Log4j 的依赖，但由于缺少配置文件，Log4j 可能没有被实际使用。

2. **实际使用的仍是 Logback**：
   - 项目中有 `logback.xml` 配置文件
   - Spring Boot 默认使用 Logback
   - SLF4J 的实际绑定可能是 Logback

3. **依赖引入但未使用**：引入 Log4j 2.15.0 依赖可能是为了：
   - 某些类路径上的兼容性需求
   - 未来计划切换到 Log4j
   - 解决特定的依赖冲突问题

##### 如何验证实际使用的日志框架

你可以通过以下方式确认实际使用的是哪个日志框架：

1. **在代码中添加调试信息**：
```java
Logger logger = LoggerFactory.getLogger(YourClass.class);
logger.info("Actual logging implementation: {}", logger.getClass().getName());
```


2. **查看应用启动日志**：Spring Boot 启动时通常会打印日志系统的初始化信息

3. **检查类路径**：看实际加载的是哪个日志实现类

所以你的判断很可能是正确的 - 虽然引入了 Log4j 依赖，但实际工作的还是 Logback，因为有相应的配置文件和默认绑定。

## Java基础

### equals()和hashcode()

使用`Objects.equals()`和`Objects.hash()`避免了空指针异常

```java
@Override
public boolean equals(Object o) {
    // 1. 检查是否为同一个对象引用
    if (this == o) return true;
    
    // 2. 检查对象是否为null或者是否属于相同的类
    if (o == null || getClass() != o.getClass()) return false;
    
    // 3. 类型转换
    CoreDemoVo that = (CoreDemoVo) o;
    
    // 4. 比较关键字段
    return Objects.equals(name, that.name) &&
            Objects.equals(school, that.school);
}
@Override
public int hashCode() {
    return Objects.hash(name, school);
}
```

### serialVersionUID

在你的项目中，这个类可能用于：

- 微服务间通过网络传输数据
- 将对象存储到Redis等缓存中
- 消息队列中传递对象

- 会话管理中存储对象状态

添加serialVersionUID确保了这些场景下对象能够正确地序列化和反序列化。

## FastJson

### @JSONField()

`@JSONField` 是阿里巴巴 Fastjson 库提供的注解，用于控制 Java 对象与 JSON 数据之间的序列化和反序列化过程。

#### 主要作用

```java
@JSONField(name = "errcode")
private int errcode;
```


这个注解的作用是：

1. **字段映射**：将 Java 字段名与 JSON 属性名进行映射
2. **序列化控制**：控制字段是否参与序列化
3. **反序列化控制**：控制字段是否参与反序列化
4. **格式化**：对日期等特殊类型进行格式化处理

#### 使用场景示例

在你的代码中：
```java
@JSONField(name = "errcode")    // JSON中的属性名为"errcode"
private int errcode;            // Java中的字段名为"errcode"

@JSONField(name = "errmsg")     // JSON中的属性名为"errmsg"  
private String errmsg;          // Java中的字段名为"errmsg"

@JSONField(name = "phone_info") // JSON中的属性名为"phone_info"
private PhoneInfoDTO phoneInfo; // Java中的字段名为"phoneInfo"
```


这样当处理微信接口返回的 JSON 数据时：
```json
{
  "errcode": 0,
  "errmsg": "success",
  "phone_info": {
    "phoneNumber": "13800138000"
  }
}
```

#### 其他常用属性

```java
@JSONField(name = "create_time", format = "yyyy-MM-dd HH:mm:ss")
private Date createTime;

@JSONField(serialize = false)  // 不参与序列化
private String password;

@JSONField(deserialize = false) // 不参与反序列化
private String internalData;
```


这是处理第三方 API 返回数据时常用的技巧，可以很好地解决 Java 命名规范（驼峰命名）与 JSON 命名规范（下划线命名）不一致的问题。

## SPI

SPI（Service Provider Interface）是一种服务发现机制，允许第三方为应用程序提供服务实现。

### SPI 的基本概念

SPI 是 Java 提供的一种解耦机制，通过在 JAR 包的 `META-INF/services/` 目录下创建配置文件，来声明接口的具体实现类。

### 工作原理

#### 1. 定义接口
```java
// 定义服务接口
public interface DatabaseDriver {
    String getConnection(String url);
    void executeQuery(String sql);
}
```


#### 2. 创建实现类
```java
// MySQL 实现
package com.mysql.driver;
public class MySQLDriver implements DatabaseDriver {
    @Override
    public String getConnection(String url) {
        return "MySQL connection to " + url;
    }
    
    @Override
    public void executeQuery(String sql) {
        System.out.println("Executing on MySQL: " + sql);
    }
}

// Oracle 实现
package com.oracle.driver;
public class OracleDriver implements DatabaseDriver {
    @Override
    public String getConnection(String url) {
        return "Oracle connection to " + url;
    }
    
    @Override
    public void executeQuery(String sql) {
        System.out.println("Executing on Oracle: " + sql);
    }
}
```


#### 3. 创建配置文件
```
# 文件路径: META-INF/services/com.example.DatabaseDriver
com.mysql.driver.MySQLDriver
# 或者
com.oracle.driver.OracleDriver
```


#### 4. 使用 ServiceLoader 加载
```java
public class DatabaseClient {
    public static void main(String[] args) {
        // 使用 SPI 机制加载服务
        ServiceLoader<DatabaseDriver> loaders = ServiceLoader.load(DatabaseDriver.class);
        
        for (DatabaseDriver driver : loaders) {
            String connection = driver.getConnection("localhost:3306/mydb");
            System.out.println(connection);
            driver.executeQuery("SELECT * FROM users");
        }
    }
}
```


### Java 中的原生 SPI 应用

Java 标准库中大量使用了 SPI 机制：

1. **JDBC 驱动加载**
   ```
   # META-INF/services/java.sql.Driver
   com.mysql.cj.jdbc.Driver
   ```


2. **日志框架**
   ```
   # META-INF/services/org.slf4j.spi.SLF4JServiceProvider
   org.slf4j.simple.SimpleServiceProvider
   ```


3. **XML 解析器**
   ```
   # META-INF/services/javax.xml.parsers.DocumentBuilderFactory
   com.sun.org.apache.xerces.internal.jaxp.DocumentBuilderFactoryImpl
   ```


### SPI 的优势

#### 1. 解耦合
```java
// 应用代码不需要知道具体的实现类
ServiceLoader<PaymentService> payments = ServiceLoader.load(PaymentService.class);
```


#### 2. 可扩展性
```java
// 第三方可以提供自己的实现，无需修改原有代码
// 只需要在 JAR 中添加配置文件和实现类
```


#### 3. 插件化架构
```java
// 支持运行时动态加载不同的实现
public interface Plugin {
    void execute();
}

// 不同的插件实现可以独立开发和部署
```


### SPI 的局限性

#### 1. 实例化问题
```java
// SPI 创建的实例不受 Spring 等容器管理
ServiceLoader<MyService> loader = ServiceLoader.load(MyService.class);
MyService service = loader.iterator().next(); // 没有依赖注入
```


#### 2. 异常处理
```java
// 如果配置文件中的类不存在，会抛出异常
try {
    ServiceLoader<MyService> loader = ServiceLoader.load(MyService.class);
    // 需要手动处理类加载异常
} catch (ServiceConfigurationError e) {
    // 处理配置错误
}
```


### 现代框架中的 SPI

#### Spring Boot SPI
```properties
# spring.factories - Spring Boot 扩展的 SPI 机制
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```


#### Dubbo SPI
```java
// Dubbo 的扩展机制
@SPI("mysql")
public interface DatabaseService {
    @Adaptive
    void saveData(String data);
}
```


### 最佳实践

#### 1. 配置文件命名规范
```
# 使用接口的全限定名作为文件名
META-INF/services/com.example.service.PaymentService
```


#### 2. 异常处理
```java
ServiceLoader<MyService> loader = ServiceLoader.load(MyService.class);
Iterator<MyService> iterator = loader.iterator();

while (iterator.hasNext()) {
    try {
        MyService service = iterator.next();
        // 使用服务
    } catch (ServiceConfigurationError e) {
        // 记录错误，继续处理下一个实现
        logger.warn("Failed to load service implementation", e);
    }
}
```


#### 3. 延迟加载
```java
// ServiceLoader 是延迟加载的
ServiceLoader<MyService> loader = ServiceLoader.load(MyService.class);
// 只有在遍历时才会实例化具体实现
```


SPI 是一种强大的解耦机制，为应用程序提供了良好的扩展性和插件化能力，是现代 Java 框架广泛采用的设计模式。

## 注解



### @Desensitization

- `@JacksonAnnotationsInside`这是Jackson库提供的注解，用于标记这是一个组合注解，它会将其他Jackson注解组合在一起。
  - 没有 `@JacksonAnnotationsInside` 的情况下，Jackson 只会直接检查字段上的注解。但有了这个注解后，Jackson 会"深入"到 @Desensitization 注解内部，查找它包含的 Jackson 注解（这里是 @JsonSerialize），并应用相应的处理逻辑。

- `@JsonSerialize(using = DesensitizedFilter.class)`这是关键部分，指定使用 DesensitizedFilter 类来进行序列化处理。当带有此注解的字段被序列化为JSON时，会通过 DesensitizedFilter 进行处理

```java
@Target({ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@JacksonAnnotationsInside
@JsonSerialize(using = DesensitizedFilter.class)
public @interface Desensitization {

    DesensitizedType type() default DesensitizedType.NAME;
}

```



```java
/**
 * @Description:脱敏filter
 * @Author：licy
 * @Date：2023/5/10
 */
public class DesensitizedFilter extends JsonSerializer<String> implements ContextualSerializer {

    private DesensitizedType type;

    public DesensitizedFilter() {
    }

    public DesensitizedFilter(DesensitizedType type) {
        this.type = type;
    }

    @Override
    public void serialize(String value, JsonGenerator jsonGenerator, SerializerProvider serializerProvider) throws IOException {
        if (ObjectUtils.isEmpty(value)) {
            return;
        }
        String val;
        switch (type) {
            case NAME:
                val = DesensitizedUtil.chineseName(value);
                break;
            case ID_CARD:
                val = DesensitizedUtil.idCardNum(value,5,2);
                break;
            case MOBILE:
                val = DesensitizedUtil.mobilePhone(value);
                break;
            case EMAIL:
                val = DesensitizedUtil.email(value);
                break;
            case PASSWORD:
                val = DesensitizedUtil.password(value);
                break;
            case RSA:
                try {
                    val = SysRsaUtils.encryptByPublicKey(value);
                } catch (Exception e) {
                    throw new RuntimeException(e);
                }
                break;
            default:
                val = value;
                break;
        }
        jsonGenerator.writeString(val);
    }

    @Override
    public JsonSerializer<?> createContextual(SerializerProvider prov, BeanProperty property) throws JsonMappingException {
        if (ObjectUtils.isEmpty(property)) {
            return prov.findNullValueSerializer(property);
        }
        if (!Objects.equals(property.getType().getRawClass(), String.class)) {
            return prov.findNullValueSerializer(property);
        }
        Desensitization annotation = property.getAnnotation(Desensitization.class);
        if (ObjectUtils.isEmpty(annotation)) {
            return prov.findNullValueSerializer(property);
        }
        return new DesensitizedFilter(annotation.type());
    }
}

```





```java
package com.ivu.common.core.utils;

import cn.hutool.core.util.StrUtil;

public class DesensitizedUtil {
    public DesensitizedUtil() {
    }

    public static String chineseName(String fullName) {
        return StrUtil.isBlank(fullName) ? "" : StrUtil.hide(fullName, 1, fullName.length());
    }

    public static String idCardNum(String idCardNum, int front, int end) {
        if (StrUtil.isBlank(idCardNum)) {
            return "";
        } else if (front + end > idCardNum.length()) {
            return "";
        } else {
            return front >= 0 && end >= 0 ? StrUtil.hide(idCardNum, front, idCardNum.length() - end) : "";
        }
    }

    public static String fixedPhone(String num) {
        return StrUtil.isBlank(num) ? "" : StrUtil.hide(num, 4, num.length() - 2);
    }

    public static String mobilePhone(String num) {
        return StrUtil.isBlank(num) ? "" : StrUtil.hide(num, 3, num.length() - 4);
    }

    public static String address(String address, int sensitiveSize) {
        if (StrUtil.isBlank(address)) {
            return "";
        } else {
            int length = address.length();
            return StrUtil.hide(address, length - sensitiveSize, length);
        }
    }

    public static String email(String email) {
        if (StrUtil.isBlank(email)) {
            return "";
        } else {
            int index = StrUtil.indexOf(email, '@');
            return index <= 1 ? email : StrUtil.hide(email, 3, index);
        }
    }

    public static String password(String password) {
        return StrUtil.isBlank(password) ? "" : StrUtil.repeat('*', password.length());
    }

    public static String carLicense(String carLicense) {
        if (StrUtil.isBlank(carLicense)) {
            return "";
        } else {
            if (carLicense.length() == 7) {
                carLicense = StrUtil.hide(carLicense, 3, 6);
            } else if (carLicense.length() == 8) {
                carLicense = StrUtil.hide(carLicense, 3, 7);
            }

            return carLicense;
        }
    }

    public static String bankCard(String bankCardNo) {
        if (StrUtil.isBlank(bankCardNo)) {
            return bankCardNo;
        } else {
            bankCardNo = StrUtil.trim(bankCardNo);
            if (bankCardNo.length() < 9) {
                return bankCardNo;
            } else {
                int length = bankCardNo.length();
                int midLength = length - 8;
                StringBuilder buf = new StringBuilder();
                buf.append(bankCardNo, 0, 4);

                for(int i = 0; i < midLength; ++i) {
                    if (i % 4 == 0) {
                        buf.append(' ');
                    }

                    buf.append('*');
                }

                buf.append(' ').append(bankCardNo, length - 4, length);
                return buf.toString();
            }
        }
    }

}

```

#### 具体脱敏情况

##### 1. 姓名脱敏 (NAME)

```java
public static String chineseName(String fullName) {
    return StrUtil.isBlank(fullName) ? "" : StrUtil.hide(fullName, 1, fullName.length());
}
```


使用 `StrUtil.hide(fullName, 1, fullName.length())` 方法，保留第一个字符，其余用 `*` 替换。

**示例结果：**
- "张三" → "张*"
- "李小明" → "李**"
- "王小花花" → "王***"

##### 2. 身份证号脱敏 (ID_CARD)

```java
public static String idCardNum(String idCardNum, int front, int end) {
    // ...
    return front >= 0 && end >= 0 ? StrUtil.hide(idCardNum, front, idCardNum.length() - end) : "";
}
```

**示例结果**

- "110101199001011234" → "11010***********34"
- "440106199001012345" → "44010***********45"

##### 3. 手机号脱敏 (MOBILE)

```java
public static String mobilePhone(String num) {
    return StrUtil.isBlank(num) ? "" : StrUtil.hide(num, 3, num.length() - 4);
}
```


保留前3位和后4位。

**示例结果：**
- "13812345678" → "138****5678"
- "15987654321" → "159****4321"

##### 4. 邮箱脱敏 (EMAIL)

```java
public static String email(String email) {
    if (StrUtil.isBlank(email)) {
        return "";
    } else {
        int index = StrUtil.indexOf(email, '@');
        return index <= 1 ? email : StrUtil.hide(email, 3, index);
    }
}
```


保留前3位和@及之后的部分。

**示例结果：**
- "example@qq.com" → "exa***@qq.com"
- "test@example.com" → "tes***@example.com"
- "ab@163.com" → "ab@163.com" (用户名太短则不脱敏)

##### 5. 密码脱敏 (PASSWORD)

```java
public static String password(String password) {
    return StrUtil.isBlank(password) ? "" : StrUtil.repeat('*', password.length());
}
```


用 `*` 替换所有字符。

**示例结果：**
- "123456" → "******"
- "password123" → "***********"

##### 6. RSA加密 (RSA)

```java
public static String encryptByPublicKey(String text) throws Exception {
    // ...
    X509EncodedKeySpec x509EncodedKeySpec2 = new X509EncodedKeySpec(Base64.decodeBase64(Constants.Keys.PUBLIC_KEY));
    KeyFactory keyFactory = KeyFactory.getInstance("RSA");
    PublicKey publicKey = keyFactory.generatePublic(x509EncodedKeySpec2);
    Cipher cipher = Cipher.getInstance("RSA");
    cipher.init(Cipher.ENCRYPT_MODE, publicKey);
    byte[] result = cipher.doFinal(text.getBytes());
    return Base64.encodeBase64String(result);
}
```


使用系统公钥进行RSA加密，结果是Base64编码的密文。

**示例结果：**
- "hello" → "加密后的Base64字符串，长度较长"

##### 总结

不同类型的脱敏处理方式和效果：

1. **姓名** - 保留姓氏，名字用*替换
2. **身份证** - 保留前5位和后2位，中间用*替换
3. **手机号** - 保留前3位和后4位，中间4位用*替换
4. **邮箱** - 保留前3位和@及之后部分，中间用*替换
5. **密码** - 全部用*替换
6. **RSA加密** - 使用公钥加密，输出Base64编码的密文

这些脱敏方式在保护用户隐私的同时，还能保留部分信息供业务使用，比如通过手机号前三位可以知道运营商，通过邮箱后缀可以知道邮箱服务商等。



### @Excel

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import java.math.BigDecimal;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface Excel
{
    /**
     * 导出时在excel中排序
     */
    public int sort() default Integer.MAX_VALUE;

    /**
     * 导出到Excel中的名字.
     */
    public String name() default "";

    /**
     * 日期格式, 如: yyyy-MM-dd
     */
    public String dateFormat() default "";

    /**
     * 读取内容转表达式 (如: 0=男,1=女,2=未知)
     */
    public String readConverterExp() default "";

    /**
     * 分隔符，读取字符串组内容
     */
    public String separator() default ",";

    /**
     * BigDecimal 精度 默认:-1(默认不开启BigDecimal格式化)
     */
    public int scale() default -1;

    /**
     * BigDecimal 舍入规则 默认:BigDecimal.ROUND_HALF_EVEN
     */
    public int roundingMode() default BigDecimal.ROUND_HALF_EVEN;

    /**
     * 导出类型（0数字 1字符串）
     */
    public ColumnType cellType() default ColumnType.STRING;

    /**
     * 导出时在excel中每个列的高度 单位为字符
     */
    public double height() default 14;

    /**
     * 导出时在excel中每个列的宽 单位为字符
     */
    public double width() default 16;

    /**
     * 文字后缀,如% 90 变成90%
     */
    public String suffix() default "";

    /**
     * 当值为空时,字段的默认值
     */
    public String defaultValue() default "";

    /**
     * 提示信息
     */
    public String prompt() default "";

    /**
     * 设置只能选择不能输入的列内容.
     */
    public String[] combo() default {};

    /**
     * 是否导出数据,应对需求:有时我们需要导出一份模板,这是标题需要但内容需要用户手工填写.
     */
    public boolean isExport() default true;

    /**
     * 另一个类中的属性名称,支持多级获取,以小数点隔开
     */
    public String targetAttr() default "";

    /**
     * 是否自动统计数据,在最后追加一行统计数据总和
     */
    public boolean isStatistics() default false;

    /**
     * 导出字段对齐方式（0：默认；1：靠左；2：居中；3：靠右）
     */
    Align align() default Align.AUTO;

    public enum Align
    {
        AUTO(0), LEFT(1), CENTER(2), RIGHT(3);
        private final int value;

        Align(int value)
        {
            this.value = value;
        }

        public int value()
        {
            return this.value;
        }
    }

    /**
     * 字段类型（0：导出导入；1：仅导出；2：仅导入）
     */
    Type type() default Type.ALL;

    public enum Type
    {
        ALL(0), EXPORT(1), IMPORT(2);
        private final int value;

        Type(int value)
        {
            this.value = value;
        }

        public int value()
        {
            return this.value;
        }
    }

    public enum ColumnType
    {
        NUMERIC(0), STRING(1), IMAGE(2);
        private final int value;

        ColumnType(int value)
        {
            this.value = value;
        }

        public int value()
        {
            return this.value;
        }
    }

```

#### 实际案例

```java
@Excel(name = "党员姓名",prompt = "必填项",type = Excel.Type.ALL)
@Excel(name = "性别", readConverterExp= "0=男,1=女,2=未知",combo = "男,女,未知",type = Excel.Type.ALL)
@Excel(name = "入党日期",width = 30,dateFormat = "yyyy-MM-dd",prompt = "必填项",type = Excel.Type.EXPORT)
@Excel(name = "转正日期",width = 30,dateFormat = "yyyy-MM-dd",prompt = "必填项",type = Excel.Type.EXPORT)
```

### @Excels

```java
@Excels({
        @Excel(name = "部门名称", targetAttr = "deptName", type = Type.EXPORT),
        @Excel(name = "部门负责人", targetAttr = "leader", type = Type.EXPORT)
    })
    private SysDept dept;
```

当导出 Excel 时：

- 系统会通过 dept 对象获取 deptName 属性值，填入"部门名称"列

- 系统会通过 dept 对象获取 leader 属性值，填入"部门负责人"列

targetAttr 的作用

- targetAttr 支持多级属性获取，例如：
- targetAttr = "deptName"：获取一级属性
- targetAttr = "dept.deptName"：获取二级属性
- targetAttr = "company.dept.deptName"：获取三级属性

### @JsonFormat()

```java
@JsonFormat(pattern = "yyyy-MM-dd", timezone = "GMT+8")
@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
```

### @Master

@Master 是一个方便的注解别名，当开发者需要明确指定某部分代码应使用主数据库时，可以直接使用 @Master 而不必每次都写 @DS("master")。这种设计通常出现在读写分离的场景中，主库用于写操作，而从库用于读操作。

```java
import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import com.baomidou.dynamic.datasource.annotation.DS;
/**
 * 主库数据源
 */
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
@DS("master")
public @interface Master
{

}
```

### @Slave


```java
import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import com.baomidou.dynamic.datasource.annotation.DS;
/**
 * 从库数据源
 * 
 */
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
@DS("slave")
public @interface Slave
{

}
```

### @Log

```java
/**
 * 自定义操作日志记录注解
 * 
 *
 */
@Target({ ElementType.PARAMETER, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Log
{
    /**
     * 模块
     */
    public String title() default "";

    /**
     * 功能
     */
    public BusinessType businessType() default BusinessType.OTHER;

    /**
     * 操作人类别
     */
    public OperatorType operatorType() default OperatorType.MANAGE;

    /**
     * 是否保存请求的参数
     */
    public boolean isSaveRequestData() default true;
}
```

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;
import com.ivu.system.api.RemoteLogService;
import com.ivu.system.api.domain.SysOperLog;
/**
 * 异步调用日志服务
 * 
 */
@Service
public class AsyncLogService
{
    @Autowired
    private RemoteLogService remoteLogService;

    /**
     * 保存系统日志记录
     */
    @Async
    public void saveSysLog(SysOperLog sysOperLog)
    {
        remoteLogService.saveLog(sysOperLog);
    }
}

```

```java
import java.lang.reflect.Method;
import java.util.Collection;
import java.util.Iterator;
import java.util.Map;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.aspectj.lang.reflect.MethodSignature;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpMethod;
import org.springframework.stereotype.Component;
import org.springframework.web.multipart.MultipartFile;
import com.alibaba.fastjson.JSON;
import com.ivu.common.core.utils.SecurityUtils;
import com.ivu.common.core.utils.ServletUtils;
import com.ivu.common.core.utils.StringUtils;
import com.ivu.common.core.utils.ip.IpUtils;
import com.ivu.common.log.annotation.Log;
import com.ivu.common.log.enums.BusinessStatus;
import com.ivu.common.log.service.AsyncLogService;
import com.ivu.system.api.domain.SysOperLog;

/**
 * 操作日志记录处理
 *
 * @author ivu
 */
@Aspect
@Component
public class LogAspect
{
    private static final Logger log = LoggerFactory.getLogger(LogAspect.class);

    @Autowired
    private AsyncLogService asyncLogService;

    // 配置织入点
    @Pointcut("@annotation(com.ivu.common.log.annotation.Log)")
    public void logPointCut()
    {
    }

    /**
     * 处理完请求后执行
     *
     * @param joinPoint 切点
     */
    @AfterReturning(pointcut = "logPointCut()", returning = "jsonResult")
    public void doAfterReturning(JoinPoint joinPoint, Object jsonResult)
    {
        handleLog(joinPoint, null, jsonResult);
    }

    /**
     * 拦截异常操作
     *
     * @param joinPoint 切点
     * @param e 异常
     */
    @AfterThrowing(value = "logPointCut()", throwing = "e")
    public void doAfterThrowing(JoinPoint joinPoint, Exception e)
    {
        handleLog(joinPoint, e, null);
    }

    protected void handleLog(final JoinPoint joinPoint, final Exception e, Object jsonResult)
    {
        try
        {
            // 获得注解
            Log controllerLog = getAnnotationLog(joinPoint);
            if (controllerLog == null)
            {
                return;
            }

            // *========数据库日志=========*//
            SysOperLog operLog = new SysOperLog();
            operLog.setStatus(BusinessStatus.SUCCESS.ordinal());
            // 请求的地址
            String ip = IpUtils.getIpAddr(ServletUtils.getRequest());
            operLog.setOperIp(ip);
            // 返回参数
            operLog.setJsonResult(JSON.toJSONString(jsonResult));

            operLog.setOperUrl(ServletUtils.getRequest().getRequestURI());
            String username =null;
            try {
                 username = SecurityUtils.getUsername();
            }catch (Exception ee){
                //do nothing
            }
            if (StringUtils.isNotBlank(username))
            {
                operLog.setOperName(username);
            }

            if (e != null)
            {
                operLog.setStatus(BusinessStatus.FAIL.ordinal());
                operLog.setErrorMsg(StringUtils.substring(e.getMessage(), 0, 2000));
            }
            // 设置方法名称
            String className = joinPoint.getTarget().getClass().getName();
            String methodName = joinPoint.getSignature().getName();
            operLog.setMethod(className + "." + methodName + "()");
            // 设置请求方式
            operLog.setRequestMethod(ServletUtils.getRequest().getMethod());
            // 处理设置注解上的参数
            getControllerMethodDescription(joinPoint, controllerLog, operLog);
            // 保存数据库
            asyncLogService.saveSysLog(operLog);
        }
        catch (Exception exp)
        {
            // 记录本地异常日志
            log.error("==前置通知异常==");
            log.error("异常信息:{}", exp.getMessage());
            exp.printStackTrace();
        }
    }

    /**
     * 获取注解中对方法的描述信息 用于Controller层注解
     *
     * @param log 日志
     * @param operLog 操作日志
     * @throws Exception
     */
    public void getControllerMethodDescription(JoinPoint joinPoint, Log log, SysOperLog operLog) throws Exception
    {
        // 设置action动作
        operLog.setBusinessType(log.businessType().ordinal());
        // 设置标题
        operLog.setTitle(log.title());
        // 设置操作人类别
        operLog.setOperatorType(log.operatorType().ordinal());
        // 是否需要保存request，参数和值
        if (log.isSaveRequestData())
        {
            // 获取参数的信息，传入到数据库中。
            setRequestValue(joinPoint, operLog);
        }
    }

    /**
     * 获取请求的参数，放到log中
     *
     * @param operLog 操作日志
     * @throws Exception 异常
     */
    private void setRequestValue(JoinPoint joinPoint, SysOperLog operLog) throws Exception
    {
        String requestMethod = operLog.getRequestMethod();
        if (HttpMethod.PUT.name().equals(requestMethod) || HttpMethod.POST.name().equals(requestMethod))
        {
            String params = argsArrayToString(joinPoint.getArgs());
            operLog.setOperParam(StringUtils.substring(params, 0, 2000));
        }
    }

    /**
     * 是否存在注解，如果存在就获取
     */
    private Log getAnnotationLog(JoinPoint joinPoint) throws Exception
    {
        Signature signature = joinPoint.getSignature();
        MethodSignature methodSignature = (MethodSignature) signature;
        Method method = methodSignature.getMethod();

        if (method != null)
        {
            return method.getAnnotation(Log.class);
        }
        return null;
    }

    /**
     * 参数拼装
     */
    private String argsArrayToString(Object[] paramsArray)
    {
        String params = "";
        if (paramsArray != null && paramsArray.length > 0)
        {
            for (int i = 0; i < paramsArray.length; i++)
            {
                if (!isFilterObject(paramsArray[i]))
                {
                    try
                    {
                        Object jsonObj = JSON.toJSON(paramsArray[i]);
                        params += jsonObj.toString() + " ";
                    }
                    catch (Exception e)
                    {
                    }
                }
            }
        }
        return params.trim();
    }

    /**
     * 判断是否需要过滤的对象。
     *
     * @param o 对象信息。
     * @return 如果是需要过滤的对象，则返回true；否则返回false。
     */
    @SuppressWarnings("rawtypes")
    public boolean isFilterObject(final Object o)
    {
        Class<?> clazz = o.getClass();
        if (clazz.isArray())
        {
            return clazz.getComponentType().isAssignableFrom(MultipartFile.class);
        }
        else if (Collection.class.isAssignableFrom(clazz))
        {
            Collection collection = (Collection) o;
            for (Iterator iter = collection.iterator(); iter.hasNext();)
            {
                return iter.next() instanceof MultipartFile;
            }
        }
        else if (Map.class.isAssignableFrom(clazz))
        {
            Map map = (Map) o;
            for (Iterator iter = map.entrySet().iterator(); iter.hasNext();)
            {
                Map.Entry entry = (Map.Entry) iter.next();
                return entry.getValue() instanceof MultipartFile;
            }
        }
        return o instanceof MultipartFile || o instanceof HttpServletRequest || o instanceof HttpServletResponse;
    }
}

```

### @PreAuthorize

```java
/**
 * 权限注解
 */
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
public @interface PreAuthorize
{
    /**
     * 验证用户是否具备某权限
     */
    public String hasPermi() default "";

    /**
     * 验证用户是否不具备某权限，与 hasPermi逻辑相反
     */
    public String lacksPermi() default "";

    /**
     * 验证用户是否具有以下任意一个权限
     */
    public String[] hasAnyPermi() default {};

    /**
     * 判断用户是否拥有某个角色
     */
    public String hasRole() default "";

    /**
     * 验证用户是否不具备某角色，与 isRole逻辑相反
     */
    public String lacksRole() default "";

    /**
     * 验证用户是否具有以下任意一个角色
     */
    public String[] hasAnyRoles() default {};
}
```

```java
import cn.hutool.core.lang.Console;
import com.ivu.common.core.constant.Constants;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.reflect.MethodSignature;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.util.CollectionUtils;
import org.springframework.util.PatternMatchUtils;
import org.springframework.util.StringUtils;
import com.ivu.common.core.exception.PreAuthorizeException;
import com.ivu.common.security.annotation.PreAuthorize;
import com.ivu.common.security.service.TokenService;
import com.ivu.common.security.utils.TaskEnvironmentUtil;
import com.ivu.system.api.model.LoginUser;

/**
 * 自定义权限实现
 *
 */
@Aspect
@Component
public class PreAuthorizeAspect {
    @Autowired
    private TokenService tokenService;

    private static final Logger logger = LoggerFactory.getLogger(PreAuthorizeAspect.class);

    /**
     * 所有权限标识
     */
    private static final String ALL_PERMISSION = "*:*:*";

    /**
     * 管理员角色权限标识
     */
    private static final String SUPER_ADMIN = "admin";

    /**
     * 数组为0时
     */
    private static final Integer ARRAY_EMPTY = 0;

    @Around("@annotation(com.ivu.common.security.annotation.PreAuthorize)")
    public Object around(ProceedingJoinPoint point) throws Throwable {
        // 检查是否是定时任务环境
        if (isTaskEnvironment()) {
            // 定时任务直接执行，跳过权限检查
            return point.proceed();
        }



        LoginUser loginUser = tokenService.getLoginUser();
        //当登录类型不是微信登录时才做权限判断
        if (loginUser.getUserLoginType() != Constants.LoginUserType.WECHAT) {
            Signature signature = point.getSignature();
            MethodSignature methodSignature = (MethodSignature) signature;
            Method method = methodSignature.getMethod();
            PreAuthorize annotation = method.getAnnotation(PreAuthorize.class);
            if (annotation == null) {
                return point.proceed();
            }

            if (!StringUtils.isEmpty(annotation.hasPermi())) {
                if (hasPermi(annotation.hasPermi())) {
                    return point.proceed();
                }
                throw new PreAuthorizeException();
            } else if (!StringUtils.isEmpty(annotation.lacksPermi())) {
                if (lacksPermi(annotation.lacksPermi())) {
                    return point.proceed();
                }
                throw new PreAuthorizeException();
            } else if (ARRAY_EMPTY < annotation.hasAnyPermi().length) {
                if (hasAnyPermi(annotation.hasAnyPermi())) {
                    return point.proceed();
                }
                throw new PreAuthorizeException();
            } else if (!StringUtils.isEmpty(annotation.hasRole())) {
                if (hasRole(annotation.hasRole())) {
                    return point.proceed();
                }
                throw new PreAuthorizeException();
            } else if (!StringUtils.isEmpty(annotation.lacksRole())) {
                if (lacksRole(annotation.lacksRole())) {
                    return point.proceed();
                }
                throw new PreAuthorizeException();
            } else if (ARRAY_EMPTY < annotation.hasAnyRoles().length) {
                if (hasAnyRoles(annotation.hasAnyRoles())) {
                    return point.proceed();
                }
                throw new PreAuthorizeException();
            }
        }
        return point.proceed();
    }

    /**
     * 验证用户是否具备某权限
     *
     * @param permission 权限字符串
     * @return 用户是否具备某权限
     */
    public boolean hasPermi(String permission) {
        LoginUser userInfo = tokenService.getLoginUser();
        if (StringUtils.isEmpty(userInfo) || CollectionUtils.isEmpty(userInfo.getPermissions())) {
            return false;
        }
        return hasPermissions(userInfo.getPermissions(), permission);
    }

    /**
     * 验证用户是否不具备某权限，与 hasPermi逻辑相反
     *
     * @param permission 权限字符串
     * @return 用户是否不具备某权限
     */
    public boolean lacksPermi(String permission) {
        return hasPermi(permission) != true;
    }

    /**
     * 验证用户是否具有以下任意一个权限
     *
     * @param permissions 权限列表
     * @return 用户是否具有以下任意一个权限
     */
    public boolean hasAnyPermi(String[] permissions) {
        LoginUser userInfo = tokenService.getLoginUser();
        if (StringUtils.isEmpty(userInfo) || CollectionUtils.isEmpty(userInfo.getPermissions())) {
            return false;
        }
        Collection<String> authorities = userInfo.getPermissions();
        for (String permission : permissions) {
            if (permission != null && hasPermissions(authorities, permission)) {
                return true;
            }
        }
        return false;
    }

    /**
     * 判断用户是否拥有某个角色
     *
     * @param role 角色字符串
     * @return 用户是否具备某角色
     */
    public boolean hasRole(String role) {
        LoginUser userInfo = tokenService.getLoginUser();
        if (StringUtils.isEmpty(userInfo) || CollectionUtils.isEmpty(userInfo.getRoles())) {
            return false;
        }
        for (String roleKey : userInfo.getRoles()) {
            if (SUPER_ADMIN.equals(roleKey) || roleKey.equals(role)) {
                return true;
            }
        }
        return false;
    }

    /**
     * 验证用户是否不具备某角色，与 isRole逻辑相反。
     *
     * @param role 角色名称
     * @return 用户是否不具备某角色
     */
    public boolean lacksRole(String role) {
        return hasRole(role) != true;
    }

    /**
     * 验证用户是否具有以下任意一个角色
     *
     * @param roles 角色列表
     * @return 用户是否具有以下任意一个角色
     */
    public boolean hasAnyRoles(String[] roles) {
        LoginUser userInfo = tokenService.getLoginUser();
        if (StringUtils.isEmpty(userInfo) || CollectionUtils.isEmpty(userInfo.getRoles())) {
            return false;
        }
        for (String role : roles) {
            if (hasRole(role)) {
                return true;
            }
        }
        return false;
    }

    /**
     * 判断是否包含权限
     *
     * @param authorities 权限列表
     * @param permission  权限字符串
     * @return 用户是否具备某权限
     */
    private boolean hasPermissions(Collection<String> authorities, String permission) {
        return authorities.stream().filter(StringUtils::hasText)
                .anyMatch(x -> ALL_PERMISSION.contains(x) || PatternMatchUtils.simpleMatch(permission, x));
    }

    /**
     * 检查是否是定时任务执行环境
     */
    private boolean isTaskEnvironment() {
        // 1. 首先检查ThreadLocal标记

        logger.debug("PreAuthorizeAspect，线程ID: {}", Thread.currentThread().getId());

        if (TaskEnvironmentUtil.isTaskEnvironment()) {
            return true;
        }

        // 2. 然后检查调用栈中是否包含定时任务相关类
        StackTraceElement[] stackTrace = Thread.currentThread().getStackTrace();
        for (StackTraceElement element : stackTrace) {
            String className = element.getClassName();
            // 扩展检测范围，包括更多定时任务相关的类和包
            if (className.contains("com.ivu.job") ||
                className.contains("QuartzJobExecution") ||
                className.contains("QuartzDisallowConcurrentExecution") ||
                className.contains("AbstractQuartzJob") ||
                className.contains("JobInvokeUtil") ||
                className.contains("FuncLogTask")) {
                return true;
            }
        }
        return false;
    }
}
```

### @ResponseDataEncrypt

```java
import java.lang.annotation.*;  
  
/**  
 * 响应数据加密注解，主要针对敏感信息  
 *  
 * @author lc  
 */
@Target(ElementType.METHOD)  
@Retention(RetentionPolicy.RUNTIME)  
@Documented  
public @interface ResponseDataEncrypt  
{  
  
}
```

```java
  
import cn.hutool.core.util.URLUtil;  
import cn.hutool.json.JSONUtil;  
import com.alibaba.fastjson.JSONObject;  
import com.alibaba.fastjson.serializer.SerializerFeature;  
import com.ivu.common.core.constant.Constants;  
import com.ivu.common.core.utils.RSAUtils;  
import com.ivu.common.core.utils.StringUtils;  
import com.ivu.common.core.web.domain.AjaxResult;  
import com.ivu.common.core.web.page.TableDataInfo;  
import org.aspectj.lang.JoinPoint;  
import org.aspectj.lang.ProceedingJoinPoint;  
import org.aspectj.lang.annotation.AfterReturning;  
import org.aspectj.lang.annotation.Around;  
import org.aspectj.lang.annotation.Aspect;  
import org.aspectj.lang.annotation.Pointcut;  
import org.slf4j.Logger;  
import org.slf4j.LoggerFactory;  
import org.springframework.stereotype.Component;  
  
import java.util.ArrayList;  
import java.util.Collections;  
import java.util.List;  
  
  
/**  
 * 用于向第三方推送AI消息时加密  
 * 数据过滤处理  
 *   
 */
@Aspect
@Component
public class ResponseDataEncryptAspect
{

    private static final Logger log = LoggerFactory.getLogger(ResponseDataEncryptAspect.class);

    @Around("@annotation(com.ivu.common.security.annotation.ResponseDataEncrypt)")
    public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
        Object jsonResult = joinPoint.proceed();
        if (jsonResult instanceof AjaxResult) {
            AjaxResult ajaxResult = (AjaxResult) jsonResult;
            Object dataObj = ajaxResult.get(AjaxResult.DATA_TAG);
            if (dataObj != null && StringUtils.isNotEmpty(dataObj.toString())) {
                String json = JSONObject.toJSONString(dataObj, SerializerFeature.WriteMapNullValue);
                String encode = URLUtil.encode(json);
                return ajaxResult.put(AjaxResult.DATA_TAG,System.currentTimeMillis()  + encode);
            }
        }else if (jsonResult instanceof TableDataInfo){
                TableDataInfo dataInfo = (TableDataInfo) jsonResult;
            List<?> rows = dataInfo.getRows();
            if (rows != null && rows.size() != 0) {
                String json = JSONObject.toJSONString(rows, SerializerFeature.WriteMapNullValue);
                String encode = URLUtil.encode(json);
                dataInfo.setRows(Collections.singletonList(System.currentTimeMillis()  + encode));
                return dataInfo;
            }
        }

        return jsonResult;

    }

}
```

## 工具类

### R

```java
import java.io.Serializable;
import com.ivu.common.core.constant.Constants;
public class R<T> implements Serializable
{
    private static final long serialVersionUID = 1L;

    /** 成功 */
    public static final int SUCCESS = Constants.SUCCESS;

    /** 失败 */
    public static final int FAIL = Constants.FAIL;

    private int code;

    private String msg;

    private T data;

    public static <T> R<T> ok()
    {
        return restResult(null, SUCCESS, null);
    }

    public static <T> R<T> ok(T data)
    {
        return restResult(data, SUCCESS, null);
    }

    public static <T> R<T> ok(T data, String msg)
    {
        return restResult(data, SUCCESS, msg);
    }

    public static <T> R<T> fail()
    {
        return restResult(null, FAIL, null);
    }

    public static <T> R<T> fail(String msg)
    {
        return restResult(null, FAIL, msg);
    }

    public static <T> R<T> fail(T data)
    {
        return restResult(data, FAIL, null);
    }

    public static <T> R<T> fail(T data, String msg)
    {
        return restResult(data, FAIL, msg);
    }

    public static <T> R<T> fail(int code, String msg)
    {
        return restResult(null, code, msg);
    }

    private static <T> R<T> restResult(T data, int code, String msg)
    {
        R<T> apiResult = new R<>();
        apiResult.setCode(code);
        apiResult.setData(data);
        apiResult.setMsg(msg);
        return apiResult;
    }

    public int getCode()
    {
        return code;
    }

    public void setCode(int code)
    {
        this.code = code;
    }

    public String getMsg()
    {
        return msg;
    }

    public void setMsg(String msg)
    {
        this.msg = msg;
    }

    public T getData()
    {
        return data;
    }

    public void setData(T data)
    {
        this.data = data;
    }
}
```

### BeanUtils

这是比较好的正则表达式案例和反射案例

```java
import java.lang.reflect.Method;
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
/**
 * Bean 工具类
 * 
 * @author ivu
 */
public class BeanUtils extends org.springframework.beans.BeanUtils
{
    /** Bean方法名中属性名开始的下标 */
    private static final int BEAN_METHOD_PROP_INDEX = 3;

    /** * 匹配getter方法的正则表达式 */
    private static final Pattern GET_PATTERN = Pattern.compile("get(\\p{javaUpperCase}\\w*)");

    /** * 匹配setter方法的正则表达式 */
    private static final Pattern SET_PATTERN = Pattern.compile("set(\\p{javaUpperCase}\\w*)");

    /**
     * Bean属性复制工具方法。
     * 
     * @param dest 目标对象
     * @param src 源对象
     */
    public static void copyBeanProp(Object dest, Object src)
    {
        try
        {
            copyProperties(src, dest);
        }
        catch (Exception e)
        {
            e.printStackTrace();
        }
    }

    /**
     * 获取对象的setter方法。
     * 
     * @param obj 对象
     * @return 对象的setter方法列表
     */
    public static List<Method> getSetterMethods(Object obj)
    {
        // setter方法列表
        List<Method> setterMethods = new ArrayList<Method>();

        // 获取所有方法
        Method[] methods = obj.getClass().getMethods();

        // 查找setter方法

        for (Method method : methods)
        {
            Matcher m = SET_PATTERN.matcher(method.getName());
            if (m.matches() && (method.getParameterTypes().length == 1))
            {
                setterMethods.add(method);
            }
        }
        // 返回setter方法列表
        return setterMethods;
    }

    /**
     * 获取对象的getter方法。
     * 
     * @param obj 对象
     * @return 对象的getter方法列表
     */

    public static List<Method> getGetterMethods(Object obj)
    {
        // getter方法列表
        List<Method> getterMethods = new ArrayList<Method>();
        // 获取所有方法
        Method[] methods = obj.getClass().getMethods();
        // 查找getter方法
        for (Method method : methods)
        {
            Matcher m = GET_PATTERN.matcher(method.getName());
            if (m.matches() && (method.getParameterTypes().length == 0))
            {
                getterMethods.add(method);
            }
        }
        // 返回getter方法列表
        return getterMethods;
    }

    /**
     * 检查Bean方法名中的属性名是否相等。<br>
     * 如getName()和setName()属性名一样，getName()和setAge()属性名不一样。
     * 
     * @param m1 方法名1
     * @param m2 方法名2
     * @return 属性名一样返回true，否则返回false
     */

    public static boolean isMethodPropEquals(String m1, String m2)
    {
        return m1.substring(BEAN_METHOD_PROP_INDEX).equals(m2.substring(BEAN_METHOD_PROP_INDEX));
    }
}

```

### HttpHelper

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.Charset;
import javax.servlet.ServletRequest;
public class HttpHelper {
    /**
     * 获取请求Body
     *
     * @param request
     * @return
     */
    public static String getBodyString(ServletRequest request) {
        StringBuilder sb = new StringBuilder();
        InputStream inputStream = null;
        BufferedReader reader = null;
        try {
            inputStream = request.getInputStream();
            reader = new BufferedReader(new InputStreamReader(inputStream, Charset.forName("UTF-8")));
            String line = "";
            while ((line = reader.readLine()) != null) {
                sb.append(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (inputStream != null) {
                try {
                    inputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (reader != null) {
                try {
                    reader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return sb.toString();
    }
}

```



### File工具类

> 看不懂，IO和输入输出学的不好

## 异常

### BaseException

代码中` throw new BaseException("未找到该用户");`这样使用比较普遍

```java
public class BaseException extends RuntimeException
{
    private static final long serialVersionUID = 1L;

    /**
     * 所属模块
     */
    private String module;

    /**
     * 错误码
     */
    private String code;

    /**
     * 错误码对应的参数
     */
    private Object[] args;

    /**
     * 错误消息
     */
    private String defaultMessage;

    public BaseException(String module, String code, Object[] args, String defaultMessage)
    {
        this.module = module;
        this.code = code;
        this.args = args;
        this.defaultMessage = defaultMessage;
    }

    public BaseException(String module, String code, Object[] args)
    {
        this(module, code, args, null);
    }

    public BaseException(String module, String defaultMessage)
    {
        this(module, null, null, defaultMessage);
    }

    public BaseException(String code, Object[] args)
    {
        this(null, code, args, null);
    }

    public BaseException(String defaultMessage)
    {
        this(null, null, null, defaultMessage);
    }

    public String getModule()
    {
        return module;
    }

    public String getCode()
    {
        return code;
    }

    public Object[] getArgs()
    {
        return args;
    }

    public String getDefaultMessage()
    {
        return defaultMessage;
    }
}
```

### CheckedException

从类名来看，"CheckedException" 可能是为了与Java标准的检查型异常（checked exceptions）区分开来，但实际上它是一个非检查型异常。这种设计可能有以下考虑：

- 统一异常处理：在整个系统中提供统一的异常类型，便于全局异常处理
- 业务异常封装：用于封装业务逻辑中的异常情况，而不是系统级异常
- 避免强制处理：作为运行时异常，不需要在每个方法签名中声明 throws，减少代码冗余

```java
public class CheckedException extends RuntimeException
{
    private static final long serialVersionUID = 1L;

    public CheckedException(String message)
    {
        super(message);
    }

    public CheckedException(Throwable cause)
    {
        super(cause);
    }

    public CheckedException(String message, Throwable cause)
    {
        super(message, cause);
    }

    public CheckedException(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace)
    {
        super(message, cause, enableSuppression, writableStackTrace);
    }
}
```

### TaskException

可以学习内部枚举的写法，从0开始

```java
public class TaskException extends Exception
{
    private static final long serialVersionUID = 1L;

    private Code code;

    public TaskException(String msg, Code code)
    {
        this(msg, code, null);
    }

    public TaskException(String msg, Code code, Exception nestedEx)
    {
        super(msg, nestedEx);
        this.code = code;
    }

    public Code getCode()
    {
        return code;
    }

    public enum Code
    {
        TASK_EXISTS, NO_TASK_EXISTS, TASK_ALREADY_STARTED, UNKNOWN, CONFIG_ERROR, TASK_NODE_NOT_AVAILABLE
    }
}
```

```java
Code.TASK_EXISTS.ordinal()  // 返回 0
Code.NO_TASK_EXISTS.ordinal()  // 返回 1
Code.TASK_NODE_NOT_AVAILABLE.ordinal()  // 返回 5
```

### InvalidExtensionException

可以学习内部静态类的写法

````java
public class InvalidExtensionException extends FileUploadException
{
    private static final long serialVersionUID = 1L;

    private String[] allowedExtension;
    private String extension;
    private String filename;

    public InvalidExtensionException(String[] allowedExtension, String extension, String filename)
    {
        super("filename : [" + filename + "], extension : [" + extension + "], allowed extension : [" + Arrays.toString(allowedExtension) + "]");
        this.allowedExtension = allowedExtension;
        this.extension = extension;
        this.filename = filename;
    }

    public String[] getAllowedExtension()
    {
        return allowedExtension;
    }

    public String getExtension()
    {
        return extension;
    }

    public String getFilename()
    {
        return filename;
    }

    public static class InvalidImageExtensionException extends InvalidExtensionException
    {
        private static final long serialVersionUID = 1L;

        public InvalidImageExtensionException(String[] allowedExtension, String extension, String filename)
        {
            super(allowedExtension, extension, filename);
        }
    }

    public static class InvalidFlashExtensionException extends InvalidExtensionException
    {
        private static final long serialVersionUID = 1L;

        public InvalidFlashExtensionException(String[] allowedExtension, String extension, String filename)
        {
            super(allowedExtension, extension, filename);
        }
    }

    public static class InvalidMediaExtensionException extends InvalidExtensionException
    {
        private static final long serialVersionUID = 1L;

        public InvalidMediaExtensionException(String[] allowedExtension, String extension, String filename)
        {
            super(allowedExtension, extension, filename);
        }
    }
    
    public static class InvalidVideoExtensionException extends InvalidExtensionException
    {
        private static final long serialVersionUID = 1L;

        public InvalidVideoExtensionException(String[] allowedExtension, String extension, String filename)
        {
            super(allowedExtension, extension, filename);
        }
    }
}

````

## 生成签名sign与验证

生成签名

```java
Date d = new Date();
SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMddHHmmss");
String timestamp = sdf.format(d); //时间参数
String tempSign = config.getMessageAccount() + config.getMessagePwd() + timestamp;
String sign = DigestUtils.md5DigestAsHex(tempSign.getBytes());//数字签名
```

验证签名

```java
public boolean verifySignature(String account, String timestamp, String sign) {
    // 1. 根据账户名获取对应的密码（通常从数据库中查询）
    String password = getPasswordByAccount(account);
    
    // 2. 按照相同规则拼接字符串
    String tempSign = account + password + timestamp;
    
    // 3. 使用相同算法生成签名
    String expectedSign = DigestUtils.md5DigestAsHex(tempSign.getBytes());
    
    // 4. 比较签名是否一致
    return expectedSign.equals(sign);
}
```

## AOP

### AllowOnlyAspect

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * 定义 限定请求ip注解
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface AllowIpOnly {
}

```



```java

import com.ivu.common.core.exception.CustomException;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.stereotype.Component;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

import javax.annotation.PostConstruct;
import javax.servlet.http.HttpServletRequest;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;
@Aspect
@Component
public class AllowIpOnlyAspect {
    //从nacos里动态读取ip列表
    @Value("${allowed.ips}")
    private String allowedIpsConfig;

    private List<String> allowedIps;

    @PostConstruct
    public void init() {
        if (allowedIpsConfig != null && !allowedIpsConfig.isEmpty()) {
            allowedIps = Arrays.asList(allowedIpsConfig.split(","));
        } else {
            allowedIps = Collections.emptyList();
        }
    }
    @Pointcut("@annotation(com.ivu.common.datascope.annotation.AllowIpOnly)")
    public void allowIpPointCut() {
    }

    @Before("allowIpPointCut()")
    public void doBefore(JoinPoint point) {
        restrictIpAccess(point);
    }
    public void restrictIpAccess(JoinPoint joinPoint) {
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        if (attributes == null) {
            throw new CustomException("请求上下文为空", 455);
        }
        HttpServletRequest request = attributes.getRequest();

        String clientIp = getClientIpAddress(request);
        if (allowedIps != null  && !allowedIps.contains(clientIp)) {
            throw new CustomException("无权访问此接口", 455);
        }
    }

    /**
     * 获取客户端真实IP（处理代理的情况）
     * 第一个 IP：原始客户端 IP（最远端）
     * 最后一个 IP：离服务器最近的一级代理 IP
     * @param request
     * @return
     */
    private String getClientIpAddress(HttpServletRequest request) {
        String ip = request.getHeader("X-Forwarded-For");
        System.out.println("请求ip:"+ip);

        if (ip == null || ip.isEmpty() || "unknown".equalsIgnoreCase(ip)) {
            //当 X-Forwarded-For 请求头为空、为空字符串或值为 "unknown" 时，
            // 使用 request.getRemoteAddr() 获取客户端的直接 IP 地址。
            ip = request.getRemoteAddr();
        } else {
            // 取最后一个IP（即最靠近客户端的一级代理）
            String[] ips = ip.split(",");
            for (int i = 0; i <= ips.length - 1; i++) {
                String candidate = ips[i].trim();
                if (!candidate.isEmpty() && !"unknown".equalsIgnoreCase(candidate)) {
                    ip = candidate;
                    break;
                }
            }
        }
        // 特殊处理本地地址
        ip = "0:0:0:0:0:0:0:1".equals(ip) ? "127.0.0.1" : ip;
        System.out.println("请求ip2:"+ip);
        return ip;
    }

    /**
     * 获取客户端真实IP地址
     * @param request HttpServletRequest对象
     * @return 真实IP地址
     */
    public static String getClientIp(HttpServletRequest request) {
        // 1. 优先从代理头获取
        String[] headers = {
                "X-Forwarded-For",   // 标准代理头
                "Proxy-Client-IP",    // Apache代理
                "WL-Proxy-Client-IP",// WebLogic代理
                "HTTP_X_FORWARDED_FOR",
                "HTTP_CLIENT_IP"
        };

        String ip = null;
        for (String header : headers) {
            ip = request.getHeader(header);
            if (isValidIp(ip)) {
                break;
            }
        }

        // 2. 如果代理头未获取到有效IP，使用remoteAddr
        if (!isValidIp(ip)) {
            ip = request.getRemoteAddr();
        }

        // 3. 处理多级代理情况
        if (ip != null && ip.contains(",")) {
            // 取最后一个IP（即最靠近客户端的一级代理）
            String[] ips = ip.split(",");
            for (int i = ips.length - 1; i >= 0; i--) {
                String candidate = ips[i].trim();
                if (!candidate.isEmpty() && !"unknown".equalsIgnoreCase(candidate)) {
                    ip = candidate;
                    break;
                }
            }
        }

        // 特殊处理本地地址
        return "0:0:0:0:0:0:0:1".equals(ip) ? "127.0.0.1" : ip;
    }
    private static boolean isValidIp(String ip) {
        return ip != null && ip.length() != 0
                && !"unknown".equalsIgnoreCase(ip)
                && !ip.startsWith("0.");
    }

}
```

### DataAuthAspect

```java
import java.lang.annotation.*;

/**
 * 数据权限过滤注解
 *
 * @author ivu
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface DataAuth {
    /**
     * 查询参数的名字
     */
    public String pName() default "";
}
```

```java
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.ivu.common.core.domain.R;
import com.ivu.common.core.exception.CustomException;
import com.ivu.common.core.utils.StringUtils;
import com.ivu.common.datascope.annotation.DataAuth;
import com.ivu.common.redis.service.RedisService;
import com.ivu.common.security.service.TokenService;
import com.ivu.system.api.RemoteDeptService;
import com.ivu.system.api.RemoteWxDeptService;
import com.ivu.system.api.model.LoginUser;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.aspectj.lang.reflect.MethodSignature;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

import javax.servlet.http.HttpServletRequest;
import java.io.BufferedReader;
import java.io.IOException;
import java.lang.reflect.Method;
import java.util.Map;
import java.util.Objects;
import java.util.concurrent.TimeUnit;

import static com.ivu.common.core.constant.Constants.REDIS_KEY_DEPTID;

/**
 * 数据过滤处理
 *
 */
@Aspect
@Component
public class DataAuthAspect {
    private static final Logger log = LoggerFactory.getLogger(DataAuthAspect.class);
    private static final int MAX_BODY_SIZE = 10 * 1024;

    @Autowired
    private TokenService tokenService;
    @Autowired
    private RemoteWxDeptService remoteWxDeptService;
    @Autowired
    private RemoteDeptService deptService;
    @Autowired
    private RedisService redisService;
    @Autowired
    private ObjectMapper objectMapper;

    // 配置织入点
    @Pointcut("@annotation(com.ivu.common.datascope.annotation.DataAuth)")
    public void dataAuthPointCut() {
    }

    @Before("dataAuthPointCut()")
    public void doBefore(JoinPoint point) {
        handleDataAuth(point);
    }

    protected void handleDataAuth(final JoinPoint joinPoint) {
        // 获得注解.对于注解方法才处理
        DataAuth controllerDataAuth = getAnnotationLog(joinPoint);
        if (controllerDataAuth == null) {
            return;
        }
        //获取需要鉴别的参数名称
        String pName = controllerDataAuth.pName();
        if (StringUtils.isEmpty(pName)) {
            //设置默认参数名称
            pName = "deptId";
        }
        // 1.获取当前的request
        HttpServletRequest request = ((ServletRequestAttributes) RequestContextHolder.getRequestAttributes()).getRequest();
        // 2.获取登录用户信息
        LoginUser loginUser = tokenService.getLoginUser();
        //3.未登录接口处理
        if (null == loginUser) {
            throw new CustomException("未登录，无权限访问该区域数据", 455);
        }
        //获取登录的 deptId 是Long
        Long d = loginUser.getSysUser().getDeptId();
        if (null != d) {
            //当前登录用户部门id
            String userDeptId = Objects.toString(d, "");
            //传入参数deptId
            String paramValue = "";
            //4. 参数，则尝试从 request.getParameterMap() 中查找（GET 或 POST 表单） body
            Map<String, String[]> parameterMap = request.getParameterMap();
            if (parameterMap.containsKey(pName)) {
                paramValue = parameterMap.get(pName)[0];
            }
            //5. 尝试从 request.getReader() 中获取 body
            if (StringUtils.isBlank(paramValue)) {
                // 获取 body 内容
                String body = getBody(request);
                paramValue = getDeptIdFromJsonBody(body, pName);
            }
            //6. 尝试从 path 中获取 deptId
            if (StringUtils.isBlank(paramValue)) {
                // 获取路径参数
                String pathInfo = request.getRequestURI();
                pathInfo = pathInfo.substring(pathInfo.lastIndexOf("/") + 1);
                paramValue = pathInfo;
            }
            // 传入deptId是否在 当前登录用户的部门范围中判断
            if (StringUtils.isNotBlank(paramValue)) {
                //先从缓存获取 部门范围信息  ， "deptId:"
                String ancestors = redisService.getCacheObject(REDIS_KEY_DEPTID + paramValue);
                if (StringUtils.isBlank(ancestors)) {
                    // 从数据库中查询 部门范围信息
                    R<String> ancestorR = deptService.selectDeptAncestorsById(paramValue);
                    ancestors = ancestorR.getData();
                    if (StringUtils.isNotBlank(ancestors)) {
                        // 缓存部门范围信息
                        redisService.setCacheObject(REDIS_KEY_DEPTID + paramValue, ancestors, 1L, TimeUnit.DAYS);
                    } else {
                        //数据库中查询不到deptId 非法
                        log.warn("远程调用错误，无法获取deptId：{} 的父级列表", paramValue);
                    }
                } else {
                    //刷新缓存时间
                    redisService.expire(REDIS_KEY_DEPTID + paramValue, 1L, TimeUnit.DAYS);
                }
                //传入deptId不在 当前登录用户的
                if (StringUtils.isNotBlank(ancestors) && !ancestors.contains(userDeptId)) {
                    // 拒绝访问，直接返回 455 状态码，并输出 JSON 提示信息
                    throw new CustomException("无权限访问该区域数据", 455);
                }
            }

        } else {
            throw new CustomException("未登录，无权限访问该区域数据", 455);
        }
    }

    /**
     * 获取body内容
     *
     * @param request
     * @return
     */
    public String getBody(HttpServletRequest request) {
        StringBuilder sb = new StringBuilder();
        try (BufferedReader reader = request.getReader()) {
            String line;
            int totalSize = 0;
            while ((line = reader.readLine()) != null) {
                totalSize += line.length();
                if (totalSize > MAX_BODY_SIZE) {
                    log.warn("请求体过大，已超过限制");
                    return "";
                }
                sb.append(line);
            }
        } catch (IOException e) {
            log.warn("读取请求体失败", e);
        }
        return sb.toString();
    }

    /**
     * 获取deptId
     *
     * @param jsonBody
     * @return
     */
    public String getDeptIdFromJsonBody(String jsonBody, String pName) {
        if (StringUtils.isBlank(jsonBody)) {
            return null;
        }
        try {
            JsonNode jsonNode = objectMapper.readTree(jsonBody);
            if (jsonNode.has(pName)) {
                return jsonNode.get(pName).asText();
            }
        } catch (JsonProcessingException e) {
            log.warn("解析JSON请求体失败", e);
        }
        return null;
    }

    /**
     * 是否存在注解，如果存在就获取
     */
    private DataAuth getAnnotationLog(JoinPoint joinPoint) {
        Signature signature = joinPoint.getSignature();
        MethodSignature methodSignature = (MethodSignature) signature;
        Method method = methodSignature.getMethod();

        if (method != null) {
            return method.getAnnotation(DataAuth.class);
        }
        return null;
    }
}

```

## Kafka配置

```java
/**
 * spring kafka 工具类
 * 
 **/
@Component
public class KafkaService
{
    @Autowired
    private KafkaTemplate<String, Object> kafkaTemplate;

    public void send(String topic, Object obj) {
        //发送消息
        ListenableFuture<SendResult<String, Object>> future = kafkaTemplate.send(StringUtils.isEmpty(topic) ? TOPIC_DEFAULT : topic, obj);
        future.addCallback(new ListenableFutureCallback<SendResult<String, Object>>() {
            @Override
            public void onFailure(Throwable throwable) {
                //发送失败的处理
                System.out.println("消息发送失败");
                throw new KafkaException(throwable.getMessage());
            }

            @Override
            public void onSuccess(SendResult<String, Object> stringObjectSendResult) {
                System.out.println("消息发送成功");
            }
        });
    }
}

```

## 验证码

WebFlux是Spring 5引入的响应式编程框架，具有以下优势：

- 非阻塞I/O：验证码生成涉及图片处理和Redis操作，使用非阻塞方式可以提高并发处理能力

- 资源利用率高：相比传统Servlet，在高并发场景下能更好地利用系统资源

- 响应速度快：对于简单的验证码生成这种I/O密集型操作，响应式处理更加高效

  

传统Servlet处理方式的局限：

- 每个请求占用一个线程

- 在高并发下需要大量线程资源

- 线程切换开销大

  

而WebFlux的事件驱动模型：

- 少量线程处理大量请求
- 通过事件循环机制提高效率
- 更好地应对突发流量

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.MediaType;
import org.springframework.web.reactive.function.server.RequestPredicates;
import org.springframework.web.reactive.function.server.RouterFunction;
import org.springframework.web.reactive.function.server.RouterFunctions;
import com.ivu.gateway.handler.ValidateCodeHandler;

/**
 * 路由配置信息
 * 
 */
@Configuration
public class RouterFunctionConfiguration
{
    @Autowired
    private ValidateCodeHandler validateCodeHandler;

    @Bean
    public RouterFunction<ServerResponse> routerFunction()
    {
        return RouterFunctions.route(
                RequestPredicates.GET("/code").and(RequestPredicates.accept(MediaType.TEXT_PLAIN)),
                validateCodeHandler);
    }
}
```



```java
/**
 * 验证码获取 handler
 * 
 */
@Component
public class ValidateCodeHandler implements HandlerFunction<ServerResponse>
{
    @Autowired
    private ValidateCodeService validateCodeService;

    @Override
    public Mono<ServerResponse> handle(ServerRequest serverRequest)
    {
        try
        {
            AjaxResult ajax = validateCodeService.createCapcha();
            return ServerResponse.status(HttpStatus.OK)
                .contentType(MediaType.APPLICATION_JSON)
                .body(BodyInserters.fromValue(ajax));
        }
        catch (CaptchaException | IOException e)
        {
            // 记录异常日志
            log.error("生成验证码失败", e);
            // 返回友好的错误信息
            AjaxResult errorResult = AjaxResult.error("验证码生成失败，请稍后重试");
            return ServerResponse.status(HttpStatus.INTERNAL_SERVER_ERROR)
                .contentType(MediaType.APPLICATION_JSON)
                .body(BodyInserters.fromValue(errorResult));
        }
    }
}
```

