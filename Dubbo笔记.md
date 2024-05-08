# Dubbo笔记

## 参考资料

[Dubbo中文文档](https://cn.dubbo.apache.org/zh-cn/overview/home/)

[JavaGuide-Dubbo](https://javaguide.cn/distributed-system/rpc/dubbo.html)

## 问题

> Dubbo协议和Tri协议是RPC协议
>
> 跨语言调用是怎么回事

## Dubbo配置

[CSDN-Dubbo（配置篇）：常用配置总结（配置文件详解）](https://blog.csdn.net/Soinice/article/details/84800122)



[Dubbo官网-启动时检查](https://cn.dubbo.apache.org/zh-cn/overview/mannual/java-sdk/advanced-features-and-usage/service/preflight-check/)

[Dubbo官网-服务多版本【用于灰度发布】](https://cn.dubbo.apache.org/zh-cn/overview/mannual/java-sdk/advanced-features-and-usage/service/multi-versions/)

[Dubbo官网-本地存根【消费者远程调用调用stub,用作一些参数校验等,stub在API层】](https://cn.dubbo.apache.org/zh-cn/overview/mannual/java-sdk/advanced-features-and-usage/service/local-stub/)

![/user-guide/images/stub.jpg](https://cn.dubbo.apache.org/imgs/user/stub.jpg)

[Dubbo官网-负载均衡](https://cn.dubbo.apache.org/zh-cn/overview/mannual/java-sdk/advanced-features-and-usage/performance/loadbalance/)

[Dubbo官网-服务降级](https://cn.dubbo.apache.org/zh-cn/overview/mannual/java-sdk/advanced-features-and-usage/service/service-downgrade/)

[Dubbo官网-集群容错](https://cn.dubbo.apache.org/zh-cn/overview/mannual/java-sdk/advanced-features-and-usage/service/fault-tolerent-strategy/)

## Dubbo与SpringBoot整合

[Dubbo官网-基于SpringBootStarter开发微服务应用](https://cn.dubbo.apache.org/zh-cn/overview/mannual/java-sdk/quick-start/spring-boot/)

## Dubbo与Zookeeper的高可用

> zookeeper宕机不影响dubbo消费者的调用，因为消费者有本地缓存，相当于直连，但是没有负载均衡了





