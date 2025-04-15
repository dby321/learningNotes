

## 1. 前端登录页面$route是什么？

### 问题描述

> ```js
>  watch: {
>     $route: {
>       handler: function (route) {
>         this.redirect = route.query && route.query.redirect;
>       },
>       immediate: true,
>     },
>   },
> ```
>
> 请问这是在干嘛？

你提到的代码片段是 Vue.js 中用于路由导航守卫（navigation guards）的一个配置项，具体来说是 Vue Router 的一个 `watch` 监听器。这个监听器用于在路由发生变化时执行某些操作。下面是对这段代码的详细解释：

### 代码解析



```js
 watch: {
    $route: {
      handler: function (route) {
        this.redirect = route.query && route.query.redirect;
      },
      immediate: true,
    },
  },
```

#### 1. `this.$route`

- 在 Vue Router 中，`$route` 是一个全局属性，它包含了当前路由的信息，如路径、参数、查询参数等。

#### 2. `handler` 函数

- `handler` 是一个回调函数，当 `$route` 发生变化时会被调用。
- 这个函数接收一个参数 `route`，它代表当前的路由对象。
- 在函数内部，检查 `route.query` 是否存在，并且是否包含 `redirect` 参数。如果存在，则将其值赋给 `this.redirect`。

#### 3. `immediate: true`

- `immediate: true` 表示在初次绑定时立即触发一次该监听器，而不仅仅是在路由变化时触发。
- 这意味着当组件首次加载并且 `$route` 已经有值时，`handler` 函数会立即执行一次。

### 示例场景

假设你有一个登录页面，用户在未登录的情况下访问某个需要认证的页面时，会被重定向到登录页面，并且原目标页面的 URL 作为查询参数传递给登录页面。这样，在用户成功登录后，你可以将用户重定向回原来的页面。

例如：

- 用户尝试访问 `/dashboard`，但由于未登录被重定向到 `/login?redirect=/dashboard`。
- 登录页面通过监听 `$route` 来获取 `redirect` 参数，并在用户成功登录后使用该参数进行重定向。

### 完整示例

以下是一个完整的示例，展示了如何在 Vue 组件中使用上述代码片段来处理重定向逻辑：



```html
<template>
  <div>
    <!-- 登录表单 -->
    <form @submit.prevent="handleLogin">
      <input v-model="username" placeholder="Username" />
      <input v-model="password" type="password" placeholder="Password" />
      <button type="submit">Login</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      password: '',
      redirect: null,
    };
  },
  watch: {
    $route: {
      handler: function (route) {
        this.redirect = route.query && route.query.redirect;
      },
      immediate: true,
    },
  },
  methods: {
    handleLogin() {
      // 模拟登录逻辑
      if (this.username === 'admin' && this.password === 'password') {
        // 登录成功
        if (this.redirect) {
          // 如果存在 redirect 参数，重定向到该路径
          this.$router.push(this.redirect);
        } else {
          // 否则重定向到默认路径
          this.$router.push('/');
        }
      } else {
        alert('Invalid credentials');
      }
    },
  },
};
</script>
```

### 解释

1. **模板部分**：包含一个简单的登录表单，用户可以输入用户名和密码并提交。
2. **数据部分**：定义了 `username`, `password`, 和 `redirect` 数据属性。
3. **监听器部分**：使用 `watch` 监听 `$route` 的变化，当 `$route` 变化时，提取 `redirect` 查询参数并存储在 `redirect` 数据属性中。
4. **方法部分**：定义了一个 `handleLogin` 方法，模拟登录逻辑。如果登录成功且存在 `redirect` 参数，则重定向到指定路径；否则重定向到默认路径。

通过这种方式，你可以轻松地处理用户登录后的重定向逻辑，确保用户体验流畅。

## 2. 远程调用后为什么date时间不带时区了？



理解你的需求后，这里提供一个相对简单的方法来确保在使用 Feign 进行远程调用时 `Date` 类型数据的时区信息不丢失。我们可以通过配置 Jackson 来自动处理日期和时间的序列化和反序列化，并确保时区信息被正确传递。

### 简单方法：配置 Jackson 的 `ObjectMapper`

你可以通过配置 Jackson 的 `ObjectMapper` 杆来统一处理日期时间格式和时区信息。以下是简化版的步骤：

#### 1. 配置全局的 `ObjectMapper`

在 Spring Boot 应用中，你可以通过配置类来设置全局的 `ObjectMapper`，使其默认使用 ISO 8601 格式并包含时区信息。





```java
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.datatype.jsr310.JavaTimeModule;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class JacksonConfig {

    @Bean
    public ObjectMapper objectMapper() {
        ObjectMapper objectMapper = new ObjectMapper();
        JavaTimeModule timeModule = new JavaTimeModule();

        // 设置 ISO 8601 格式并包含时区信息
        objectMapper.setDateFormat(new CustomDateFormat());

        // 注册 JavaTimeModule 以支持 java.time 包中的类
        objectMapper.registerModule(timeModule);

        return objectMapper;
    }
}
```

#### 2. 自定义 `DateFormat`

创建一个自定义的 `DateFormat` 来确保日期时间包含时区信息。



```java
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.TimeZone;

public class CustomDateFormat extends DateFormat {

    private static final long serialVersionUID = 1L;

    private final SimpleDateFormat dateFormat;

    public CustomDateFormat() {
        dateFormat = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSSZ");
        dateFormat.setTimeZone(TimeZone.getTimeZone("UTC")); // 设置为 UTC 或其他统一的时区
    }

    @Override
    public StringBuffer format(Date date, StringBuffer toAppendTo, java.text.FieldPosition fieldPosition) {
        return dateFormat.format(date, toAppendTo, fieldPosition);
    }

    @Override
    public Date parse(String source) {
        try {
            return dateFormat.parse(source);
        } catch (Exception e) {
            throw new RuntimeException("Failed to parse date string: " + source, e);
        }
    }
}
```

#### 3. 使用 `@JsonFormat` 注解（可选）

如果你不想全局修改日期格式，可以在需要的地方使用 `@JsonFormat` 注解来指定日期格式和时区。

java

深色版本



```java
import com.fasterxml.jackson.annotation.JsonFormat;
import java.util.Date;

public class TimeResponse {

    @JsonFormat(shape=JsonFormat.Shape.STRING,pattern = "yyyy-MM-dd HH:mm:ss",timezone = "GMT+8")// 可以解决问题
    //@JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd'T'HH:mm:ss.SSSZ", timezone = "UTC")// 可以解决问题
    //@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss") //不能解决问题
    private Date time;

    public Date getTime() {
        return time;
    }

    public void setTime(Date time) {
        this.time = time;
    }
}
```

### 完整示例

#### 控制器示例



```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.Date;

@RestController
public class TimeController {

    @GetMapping("/api/time")
    public TimeResponse getCurrentTime() {
        TimeResponse response = new TimeResponse();
        response.setTime(new Date());
        return response;
    }
}
```

#### Feign 客户端接口



```java
import feign.Headers;
import feign.RequestLine;
import org.springframework.cloud.openfeign.FeignClient;

@FeignClient(name = "timeService", url = "http://localhost:8080")
public interface TimeServiceClient {

    @RequestLine("GET /api/time")
    @Headers("Content-Type: application/json")
    TimeResponse getCurrentTime();
}
```

#### 时间响应类



```java
import com.fasterxml.jackson.annotation.JsonFormat;
import java.util.Date;

public class TimeResponse {

    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd'T'HH:mm:ss.SSSZ", timezone = "UTC")
    private Date time;

    public Date getTime() {
        return time;
    }

    public void setTime(Date time) {
        this.time = time;
    }
}
```

### 总结

通过上述简单的配置，你可以确保在使用 Feign 进行远程调用时 `Date` 类型的数据能够正确地保留时区信息：

1. **配置全局的 `ObjectMapper`**：通过配置类设置全局的 `ObjectMapper`，使其默认使用 ISO 8601 格式并包含时区信息。
2. **自定义 `DateFormat`**：创建一个自定义的 `DateFormat` 来确保日期时间包含时区信息。
3. **使用 `@JsonFormat` 注解**：如果不需要全局修改，可以在特定字段上使用注解来指定格式和时区。

这种方法相对简单，只需要几行代码即可实现，同时确保了时间和时区信息的一致性和准确性。



## 3. nacos找不到配置文件怎么办？

1. 查看配置文件nacos地址和账号密码是否配置正确
2. 查看配置文件的**命名空间**
3. 检查nacos命名空间下配置文件名字是否正确
4. 配置正确的命名空间，上传项目nacos配置压缩包



## 4. Mysql Too Many Connections怎么办？

1. 修改my.ini

```
#加大连接列表数量

max_connections = 2000

# 调整失效连接清理时长 缩短

wait_timeout=7200

interactive_timeout=7200
```

## 5. 实体类名字重复怎么办？

```
nested exception is org.apache.ibatis.type.TypeException: The alias 'SmsLoginLog' is already mapped to the value 'com.ivu.system.api.domain.SmsLoginLog'.
	
```

1. 删除这个实体类，不让项目下有重复名字的实体类

