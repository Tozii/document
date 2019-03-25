# Consul

[^_^]:![Consul](https://tozii.github.io/Asset/document/images/consul.jpg)

**Github**
> [Github：https://github.com/hashicorp/consul](https://github.com/hashicorp/consul)

**介绍**

&emsp;&emsp;Consul是一种服务网格解决方案，提供具有服务发现，配置和分段功能的全功能控制平面。这些功能中的每一个都可以根据需要单独使用，也可以一起使用以构建全服务网格。Consul需要数据平面并支持代理和本机集成模型。Consul附带一个简单的内置代理，因此一切都可以开箱即用，但也支持第三方代理集成，如Envoy。<br/>
**Consul的主要特点是：**
- 服务发现：Consul的客户可以注册服务，例如 api或mysql，并且其他客户可以使用Consul来发现给定服务的提供者。使用DNS或HTTP，应用程序可以轻松找到它们所依赖的服务。<br/>
- 运行状况检查：Consul客户端可以提供任意数量的运行状况检查，这些检查与给定服务（“是Web服务器返回200 OK”）或本地节点（“内存利用率低于90％”）相关联。运营商可以使用此信息来监控群集运行状况，服务发现组件使用此信息将流量路由远离不健康的主机。<br/>
- KV存储：应用程序可以将Consul的分层键/值存储用于任何目的，包括动态配置，功能标记，协调，领导者选举等。简单的HTTP API使其易于使用。<br/>
- 安全服务通信：Consul可以为服务生成和分发TLS证书，以建立相互的TLS连接。 意图 可用于定义允许哪些服务进行通信。可以使用可以实时更改的意图轻松管理服务分段，而不是使用复杂的网络拓扑和静态防火墙规则。<br/>
- 多数据中心：Consul支持多个数据中心。这意味着Consul的用户不必担心构建额外的抽象层以扩展到多个区域<br/>

**使用**
> [.NET Core微服务实施之Consul服务发现与治理 — dotnetgeek](https://www.cnblogs.com/waynechan/p/9354909.html)