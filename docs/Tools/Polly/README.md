# Polly

<!--![Polly](https://tozii.github.io/Asset/document/images/polly.png)-->

**Github**
> [Github：https://github.com/App-vNext/Polly](https://github.com/App-vNext/Polly)

**介绍**

Polly是一个被.NET基金会认可的弹性和瞬态故障处理库，允许我们以非常顺畅和线程安全的方式来执诸如行重试，断路，超时，故障恢复等策略，其主要功能如下：
1. 重试（Retry）
2. 断路器（Circuit-Breaker）
3. 超时检测（Timeout）
4. 缓存（Cache）
5. 降级（Fallback）
Polly的策略主要由“故障”和“动作”两个部分组成，“故障”可以包括异常、超时等情况，“动作”则包括Fallback（降级）、重试（Retry）、熔断（Circuit-Breaker）等。策略则用来执行业务代码，当业务代码出现了“故障”中的情况时就开始执行“动作”。

**使用**
> [.NET Core微服务之基于Polly+AspectCore实现熔断与降级机制 — 周旭龙](https://www.cnblogs.com/edisonchou/p/9159644.html)

> [使用.NetCore 控制台演示 熔断 降级（polly）— 乐途](https://www.cnblogs.com/szlblog/p/9300845.html)