# 私有化部署 DeepSeek - Java 技术栈

## 1. 环境准备

### 1.1 安装 Ollama

- 官方文档：[Ollama 官网](https://ollama.com/)
- 中文教程：[菜鸟教程 - Ollama 教程](https://www.runoob.com/ollama/ollama-tutorial.html)

![Ollama 安装界面](images/私有化部署deepseek-Java技术栈/image-20250208112332029.png)

## 2. 本地部署 DeepSeek R1

1. 打开命令行窗口（cmd）
2. 执行以下命令：
   ```bash
   ollama run deepseek-r1:1.5b
   ```
   > 注意：下载速度可能较慢，建议使用浏览器迅雷嗅探插件（需要科学上网）

## 3. 项目配置

### 3.1 拉取项目

```bash
git clone https://github.com/dby321/Java-Ai.git
```

> 如果觉得项目有用，欢迎给个 ⭐ Star 支持！

### 3.2 修改配置文件

1. 打开项目中的 `application.yml` 文件
2. 如果是本地部署，无需修改任何配置

> 重要提示：本项目基于 Spring Boot 3 开发，需要 Java 17 及以上版本

![项目结构](images/私有化部署deepseek-Java技术栈/image-20250208112809970.png)

## 4. 启动项目

1. 进入项目根目录
2. 执行启动命令：
   ```bash
   ./mvnw spring-boot:run
   ```

## 5. 接口测试

使用 Postman 进行接口测试：

![Postman 测试示例](images/私有化部署deepseek-Java技术栈/image-20250208113012303.png)
