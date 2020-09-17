# Dubbo

## 介绍

[Dubbo官网](http://dubbo.apache.org/zh-cn/index.html)   
[开源地址](https://github.com/apache/dubbo)

Apache Dubbo |ˈdʌbəʊ| 是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：
- 面向接口的远程方法调用
- 智能容错和负载均衡
- 以及服务自动注册和发现。   
Dubbo 是由阿里开源，后来捐给了 Apache 

Dubbo支持多种注册中心：
- multicast
- zookeeper
- redis
- simple
- ...

开源版本大都使用zookeeper作为注册中心

## 系统架构
---

![dubbo](http://dubbo.apache.org/img/architecture.png)

### Dubbo 系统分为5个部分：
- 远程服务运行容器(Container)
- 远程服务提供方(Provider)
- 注册中心(Register)
- 远程服务调用者(Consumer)
- 监控中心(Monitor)

### Dubbo 服务调用过程：
- 服务提供方(Provider)所在的应用在容器中启动运行（这个容器可以说是该应用部署的tomcat）
- 服务提供方（Provider）将自己要发布的服务注册到注册中心（Registry）
- 服务调用方（Consumer）启动后向注册中心订阅它想要调用的服务
- 注册中心（Registry）存储着Provider注册的远程服务，并将其所在管理的服务列表通知给服务调用方（Consumer），且注册中心和提供方和调用方之间均保持长连接，可以获取Provider发布的服务的变化情况，并将最新的服务列表推送给Consumer
- Consumer根据从注册中心获得的服务列表，根据软负载均衡算法选择一个服务提供者（Provider）进行远程服务调用，如果调用失败则选择另一台进行调用。（Consumer会缓存服务列表，即使注册中心宕机也不妨碍进行远程服务调用）
- 监控中心（Monitor）对服务的发布和订阅进行监控和统计服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心（Monitor）；Monitor可以选择zookeeper、redis或者multiast注册中心等

