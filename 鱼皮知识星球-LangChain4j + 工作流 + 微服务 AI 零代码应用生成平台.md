# 鱼皮知识星球-**LangChain4j + 工作流 + 微服务 AI 零代码应用生成平台**

## 项目总览

- 使用 Cursor+ClaudeCode等主流的AI开发工具

- 区别烂大街的 CRUD 项目

## 前后端项目准备

- 配置 lombok 依赖版本 1.8.36

- 下载依赖失败，使用 maven 强制刷新

- 改成 application.yml

- 安装 TraeAI,不要安装特别多 AI 插件容易冲突

- Maven 依赖采用随用随装原则

  - hutool 工具库

    - 有的公司不用，怕有安全问题，那么公司一般会提供自己的工具类
    - hutool 已经比原来稳定多了
    - 自己开发项目可以使用，快速上线

  - 请求调试 **Swagger+knife4j 接口文档 个人开发者推荐**

    - SpringBoot3 只支持 OpenAPI3规范，引入 maven 包别引入错了

      - ```yml
        server:
          port: 8123
          servlet:
            context-path: /api
        
        # springdoc-openapi项目配置
        springdoc:
          swagger-ui:
            path: /swagger-ui.html
            tags-sorter: alpha
            operations-sorter: alpha
          api-docs:
            path: /v3/api-docs
          group-configs:
            - group: 'default'
              paths-to-match: '/**'
              packages-to-scan: cn.waterplan.yuaicodemother.controller
        # knife4j的增强配置，不需要增强可以不配
        knife4j:
          enable: true
          setting:
            language: zh_cn
        ```

      - http://localhost:8123/api/doc.html knife4j地址

      - http://localhost:8123/api/v3/api-docs openAPI3 地址

      - http://localhost:8123/api/v3/api-docs swagger-ui 地址

      - springdoc配置要选择要扫描的controller类路径

  - 幽灵注释会在codegeex中出现，可以选择关闭

  - 整合后端通用基础代码

    - 自定义异常
      - 区分业务异常和系统内部异常
      - 错误码可以用五位数，和http状态码保持一致，前端可以根据错误码通用处理
    - 解决RestControllerAdvice和OpenAPI3下注解不生效的问题
      - 加一个@Hidden,鱼皮是下来最简单好用的方法
    - 全局异常处理
    - 全局跨域配置
    - 通用请求类包装

  - 开启**Lombok注解支持**
