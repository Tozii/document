# Ocelot

![Ocelot](https://tozii.github.io/Asset/document/images/ocelot.png)

**Github**
> [Github：https://github.com/ThreeMammals/Ocelot](https://github.com/ThreeMammals/Ocelot)

**介绍**

*&emsp;&emsp;Ocelot是一个.NET API网关。该项目针对的是使用.NET运行面向微服务/面向服务的体系结构的人，这些体系结构需要统一的入口点。但是，它可以与任何说HTTP并在ASP.NET Core支持的任何平台上运行的东西一起使用。<br/>
&emsp;&emsp;特别是我希望与IdentityServer引用和承载令牌轻松集成。<br/>
&emsp;&emsp;我们无法在当前的工作场所找到这个，而无需编写我们自己的Javascript中间件来处理<br/>IdentityServer引用令牌。我们宁愿使用已存在的IdentityServer代码来执行此操作。<br/>
&emsp;&emsp;Ocelot是一组按特定顺序排列的中间件。<br/>
&emsp;&emsp;Ocelot将HttpRequest对象操作到其配置指定的状态，直到它到达请求构建器中间件，在该中间件中，它创建一个HttpRequestMessage对象，该对象用于向下游服务发出请求。发出请求的中间件是Ocelot管道中的最后一件事。它不会调用下一个中间件。当请求返回Ocelot管道时，将检索下游服务的响应。有一个中间件将HttpResponseMessage映射到HttpResponse对象并返回给客户端。基本上它还带有许多其他功能*

**使用**
> [.NET Core开源API网关 – Ocelot中文文档 —— 腾飞（Jesse）](https://www.cnblogs.com/jesse2013/p/net-core-apigateway-ocelot-docs.html)

!> Ocelot v13版本的负载均衡的算法参数由LoadBalancer 键值对变成LoadBalancerOptions对象
```
"LoadBalancer": "RoundRobin"

"LoadBalancerOptions": {
    "Type": "RoundRobin"
},
```
