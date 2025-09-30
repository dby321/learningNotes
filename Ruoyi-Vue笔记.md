# Ruoyi-Vue笔记

## Bat入门到精通~

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
| ------------------------ | ------------------------------------------------ |
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

