# Owin

[官方文档](https://docs.microsoft.com/zh-cn/aspnet/aspnet/overview/owin-and-katana/an-overview-of-project-katana)

[Owin规范文档](http://owin.org/)

Owin接口源码:
```csharp
public interface IAppBuilder
{
	IDictionary<string, object> Properties
	{
		get;
	}

	IAppBuilder Use(object middleware, params object[] args);

	object Build(Type returnType);

	IAppBuilder New();
}

```

## 介绍
---
Owin英文全称是 Open Web Interface for .NET，大体意思是.net开放Web接口。

它的主要作用是为了.net web应用程序和web服务器之间的解耦。比如我们在.net framework框架上开发的.net web应用程序必须依赖于iis服务器才可以运行，这样就限制了.net web程序的可移植性，和复杂场景的应用性（如：在电脑配置不足以安装iis服务器的情况没法部署.net web应用，因为iis服务器属于比较重量型的，运行所占内存比较大，而我们的需求仅仅只是想要运行一个简单的web程序而已）。

Katana 项目是微软依据Owin接口规范实现的一系列组件，我们借助这些组件可以实现将.net web应用程序运行于iis服务器或者自定义主机（控制台应用程序和Windows服务等等）。

> OWIN 是社区拥有的规范，而不是实现。 Katana 项目是由 Microsoft 开发的一组开源 OWIN 组件

[Katana 项目开放源码地址](https://github.com/aspnet/AspNetKatana/)

## 使用
---
[官方入门文档](https://docs.microsoft.com/zh-cn/aspnet/aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana)

### 在 IIS 中承载 OWIN

首先，创建一个新的 ASP.NET Web 应用程序项目。 （使用 ASP.NET 空白 Web 应用程序项目类型。选择“空”模版 `Empty` 模版）

添加 NuGet 包 `Microsoft.Owin.Host.SystemWeb`

添加 Startup 类，在解决方案资源管理器中，右键单击项目并选择 "添加"，然后选择 "新建项"。 在 "添加新项" 对话框中，选择 " Owin Startup class"。

将以下代码添加到 Startup1.Configuration 方法中：
```csharp
public void Configuration(IAppBuilder app)
{
    // New code:
    app.Run(context =>
    {
        context.Response.ContentType = "text/plain";
        return context.Response.WriteAsync("Hello, world.");
    });
}
```
> Visual Studio 2013 中提供了 OWIN Startup 类模板。 如果使用的是 Visual Studio 2012，只需添加一个名为 Startup1的新空类，并粘贴以下代码：
```csharp
using System;
using System.Threading.Tasks;
using Microsoft.Owin;
using Owin;

[assembly: OwinStartup(typeof(OwinApp.Startup1))]

namespace OwinApp
{
    public class Startup1
    {
        public void Configuration(IAppBuilder app)
        {
          app.Run(context =>
          {
              context.Response.ContentType = "text/plain";
              return context.Response.WriteAsync("Hello, world.");
          });
        }
    }
}
```
然后按 F5 运行就可以看到浏览器中输出： Hello，world

### 控制台应用程序中的自承载 OWIN

在自定义过程中，可以轻松地将此应用程序从承载 IIS 托管到自承载。 借助 IIS 托管，IIS 既充当 HTTP 服务器，又充当承载服务的进程。 使用自承载，你的应用程序将创建进程，并使用HttpListener类作为 HTTP 服务器。

在 Visual Studio 中创建新的控制台应用程序。

然后添加 NuGet 包 `Microsoft.Owin.SelfHost`

将在IIS 中承载 OWIN部分中的 Startup 类添加到本项目。 不需要修改此类。

按如下所示实现应用程序的 Main 方法:
```csharp
class Program
{
    static void Main(string[] args)
    {
        using (Microsoft.Owin.Hosting.WebApp.Start<Startup1>("http://localhost:9000"))
        {
            Console.WriteLine("Press [enter] to quit...");
            Console.ReadLine();
        }
    }
}
```

运行控制台应用程序时，服务器开始侦听 http://localhost:9000。 如果在 web 浏览器中导航到此地址，将显示 "Hello world" 页面。

## 添加 OWIN 诊断
---
Owin 包包含捕获未经处理的异常的中间件，并显示包含错误详细信息的 HTML 页面。 此页的工作方式非常类似于 ASP.NET 错误页。

添加 NuGet 包 `Microsoft.Owin.Diagnostics`

> `Microsoft.Owin.SelfHost` 组件4.1.1版本会依赖 `Microsoft.Owin.Diagnostics` 这个组件，根据项目实际情况看看当前项目有没有依赖`Microsoft.Owin.Diagnostics`这个组件而决定要不要下载

更改 Startup.Configuration 方法中的代码，如下所示：
```csharp
public void Configuration(IAppBuilder app)
{
    // 新代码:将错误页面中间件添加到管道中。
    app.UseErrorPage();

    app.Run(context =>
    {
        // 新代码:为此URI路径抛出异常。
        if (context.Request.Path.Equals(new PathString("/fail")))
        {
            throw new Exception("Random exception");
        }

        context.Response.ContentType = "text/plain";
        return context.Response.WriteAsync("Hello, world.");
    });
}
```

然后运行。 使用 CTRL + F5 运行应用程序而不进行调试，以便 Visual Studio 不会在异常上中断。 应用程序的行为与以前相同，直到你导航到 http://localhost/fail，此时应用程序会引发异常。 错误页中间件将捕获异常，并显示包含有关错误的信息的 HTML 页面。 您可以单击选项卡以查看堆栈、查询字符串、cookie、请求标头和 OWIN 环境变量。

## Owin中间件
---
### 自定义中间件

当服务器接受来自客户端的请求时，它负责通过 OWIN 组件（由开发人员的启动代码指定）的管道传递它。 这些管道组件称为中间件。

为了简化中间件组件的开发和组合，Katana 支持用于中间件组件的少量约定和帮助器类型。 最常见的是 OwinMiddleware 类。 使用此类生成的自定义中间件组件如下所示：
```csharp
public class LoggerMiddleware : OwinMiddleware
{
    private readonly ILog _logger;
 
    public LoggerMiddleware(OwinMiddleware next, ILog logger) : base(next)
    {
        _logger = logger;
    }
 
    public override async Task Invoke(IOwinContext context)
    {
        _logger.LogInfo("Middleware begin");
        await this.Next.Invoke(context);
        _logger.LogInfo("Middleware end");
    }
}
```

可以在应用程序启动代码中轻松地将中间件类添加到 OWIN 管道，如下所示：
```csharp
public class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            app.UseErrorPage();

            app.Use<LoggerMiddleware>(new TraceLogger());

            app.Run(context =>
            {
                // 新代码:为此URI路径抛出异常。
                if (context.Request.Path.Equals(new PathString("/fail")))
                {
                    throw new Exception("Random exception");
                }

                context.Response.ContentType = "text/plain";
                return context.Response.WriteAsync("Hello, world.");
            });
        }
    }
```

### OWIN 中间件在 IIS 集成管道中的执行方式

对于 OWIN 控制台应用程序，使用启动配置构建的应用程序管道由使用 IAppBuilder.Use 方法添加组件的顺序设置

在 IIS 集成管道中，请求管道由已订阅预定义管道事件集的HttpModules （如BeginRequest、 AuthenticateRequest、 AuthorizeRequest等）组成。

如果我们使用IIS作为Owin程序托管服务的话，默认情况下，Katana 运行时将每个 OWIN 中间件组件映射到PreExecuteRequestHandler ，这对应于 IIS 管道事件PreRequestHandlerExecute。

如果我们想将指定中间件映射到某个IIS管道事件，则可以使用**阶段标记** `PipelineStage` 枚举类型：
```csharp
public enum PipelineStage
{
    Authenticate = 0,
    PostAuthenticate = 1,
    Authorize = 2,
    PostAuthorize = 3,
    ResolveCache = 4,
    PostResolveCache = 5,
    MapHandler = 6,
    PostMapHandler = 7,
    AcquireState = 8,
    PostAcquireState = 9,
    PreHandlerExecute = 10
}
```

使用方式：
```csharp
public void Configuration(IAppBuilder app)
{
    app.Use((context, next) =>
    {
        PrintCurrentIntegratedPipelineStage(context, "Middleware 1");
        return next.Invoke();
    });
    app.Use((context, next) =>
    {
        PrintCurrentIntegratedPipelineStage(context, "2nd MW");
        return next.Invoke();
    });
    app.UseStageMarker(PipelineStage.Authenticate); //阶段标记
    app.Run(context =>
    {
        PrintCurrentIntegratedPipelineStage(context, "3rd MW");
        return context.Response.WriteAsync("Hello world");
    });
    app.UseStageMarker(PipelineStage.ResolveCache); //阶段标记
}
```