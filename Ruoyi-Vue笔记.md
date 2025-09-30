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
