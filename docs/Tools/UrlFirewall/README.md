# UrlFirewall

**Github**
> [Github:https://github.com/stulzq/UrlFirewall](https://github.com/stulzq/UrlFirewall)

**介绍**
*&emsp;&emsp;UrlFirewall 是一款http请求过滤中间件，可以和网关（Ocelot）搭配，实现屏蔽外网访问内部接口，只让内部接口之间相互通讯，而不暴露到外部。它支持黑名单模式和白名单模式，支持自定义http请求响应代码。具有良好的扩展性，可自己实现验证逻辑，从数据库或者Redis缓存等介质实现对规则的检索。*

**使用**
> [ASP.NET Core 使用UrlFirewall对请求进行过滤 — 晓晨Master](https://www.cnblogs.com/stulzq/p/8987632.html)