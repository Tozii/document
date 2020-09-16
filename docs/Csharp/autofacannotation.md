# Autofac.Annotation

[开源项目地址](https://github.com/yuzd/Autofac.Annotation)

## 介绍

Autofac.Annotation是一个基于autofac开源项目，借鉴Spring注解方式实现对象注入的一个包。

Autofac是一个.net平台IOC容器，注入方式使用代码配置注入

## 使用

```csharp
var builder = new ContainerBuilder();

// 注册autofac打标签模式
builder.RegisterModule(new AutofacAnnotationModule(typeof(AnotationTest).Assembly));
//如果需要开启支持循环注入
//builder.RegisterModule(new AutofacAnnotationModule(typeof(AnotationTest).Assembly).SetAllowCircularDependencies(true));
// var container = builder.Build();
// var serviceB = container.Resolve<B>();
```

AutofacAnnotationModule有两种构造方法:
- 可以传一个Assebly列表 （这种方式会注册传入的Assebly里面打了标签的类）
- 可以传一个AsseblyName列表 (这种方式是先会根据AsseblyName查找Assebly 然后在注册)

## 特性说明

### Component标签

说明：只能打在class上面(且不能是抽象class) 把某个类注册到autofac容器 例如：

1.无构造方法的方式 等同于 builder.RegisterType();
```csharp
//把class A 注册到容器
[Component]
public class A
{
	public string Name { get; set; }
}
//如果 A有父类或者实现了接口 也会自动注册(排除非public的因为autofac不能注册私有类或接口)
public interface IB
{

}	
public class ParentB:IB
{
	public string Name1 { get; set; }
}

//把class B 注册到容器 并且把 B作为ParentB注册到容器 并且把B作为IB注册到容器
[Component]
public class B:ParentB
{
	public string Name { get; set; }
}	
```

2.指定Scope [需要指定AutofacScope属性 如果不指定为则默认为AutofacScope.InstancePerDependency]
```csharp
[Component(AutofacScope = AutofacScope.SingleInstance)]
public class A
{
    public string Name { get; set; }
}
```

3.指定类型注册 等同于 builder.RegisterType().As()
```csharp
public class B
{

}
	
[Component(typeof(B))]
public class A6:B
{

}
```

4.指定名字注册 等同于 builder.RegisterType().Keyed("a4")
```csharp
[Component("a4")]
public class A4
{
    public string School { get; set; } = "测试2";
}
```

5.其他属性说明
- OrderIndex 注册顺序 【顺序值越大越早注册到容器，但是一个类型多次注册那么装配的时候会拿OrderIndex最小的值(因为autofac的规则会覆盖)】
- InjectProperties 是否默认装配属性 【默认为true】
- InjectPropertyType 属性自动装配的类型
    - Autowired 【默认值】代表打了Autowired标签的才会自动装配
    - ALL 代表会装配所有 等同于 builder.RegisterType().PropertiesAutowired()
- AutoActivate 【默认为false】 如果为true代表autofac build完成后会自动创建 具体请参考 autofac官方文档
- Ownership 【默认为空】 具体请参考 autofac官方文档
- Interceptor 【默认为空】指定拦截器的Type
- InterceptorType 拦截器类型 拦截器必须实现 Castle.DynamicProxy的 IInterceptor 接口， 有以下两种
    - Interface 【默认值】代表是接口型
    - Class 代表是class类型 这种的话是需要将要拦截的方法标virtual
- InterceptorKey 如果同一个类型的拦截器有多个 可以指定Key
- InitMethod 当实例被创建后执行的方法名称 类似Spring的init-method 可以是有参数(只能1个参数类型是IComponentContext)和无参数的方法
- DestroyMetnod 当实例被Release时执行的方法 类似Spring的destroy-method 必须是无参数的方法
```csharp
    [Component(InitMethod = "start",DestroyMetnod = "destroy")]
    public class A30
    {
        [Value("aaaaa")]
        public string Test { get; set; }

        public A29 a29;

        void start(IComponentContext context)
        {
            this.Test = "bbbb";
            a29 = context.Resolve<A29>();
        }

        void destroy()
        {
            this.Test = null;
            a29.Test = null;
        }
    }
```

```csharp
    public class B
    {

    }
	
    [Component(typeof(B),"a5")]
    public class A5:B
    {
        public string School { get; set; } = "测试a5";
        public override string GetSchool()
        {
            return this.School;
        }
    }
```

### Autowired 自动装配

可以打在Field Property 构造方法的Parameter上面 其中Field 和 Property 支持在父类
```csharp
    [Component]
    public class A16
    {
	public A16([Autowired]A21 a21)
        {
            Name = name;
            A21 = a21;
        }
		
        [Autowired("A13")]
        public B b1;


        [Autowired]
        public B B { get; set; }
		
	//Required默认为true 如果装载错误会抛异常出来。如果指定为false则不抛异常
	[Autowired("adadada",Required = false)]
        public B b1;
    }
```

### Value 和 PropertySource

PropertySource类似Spring里面的PropertySource 可以指定数据源 支持 xml json格式 支持内嵌资源

_Value标签_
- ${xxx} 的格式代表是 从配置文件读取 xxx的值
- #{} 的格式代表是 执行 SPEL表达式 , #{}内部可以嵌入 ${}
- 更多关于SPEL表达式请参考 [Spring.EL](https://github.com/yuzd/Spring.EL)

1.json格式的文件
```json
{
  "a9": "aaaaaaaaa",
  "list": "1, 2, 3, 4 ",
  "dic": "#{'name': 'name1','school': 'school1'}",
  "parent": {
    "name" : "yuzd"
  },
  "testInitField": 1,
  "testInitProperty": 1,
}
```
```csharp
    [Component]
    [PropertySource("/file/appsettings1.json")]
    public class A10
    {
        public A10([Value("${a10}")]string school,[Value("${list}")]List<int> list,[Value("#{${dic}}")]Dictionary<string,string> dic)
        {
            this.School = school;
            this.list = list;
            this.dic = dic;

        }
        public string School { get; set; }
        public List<int> list { get; set; } 
        public Dictionary<string,string> dic { get; set; } 
		
		[Value("${testInitField}")]
        public int test;
		
		[Value("${testInitProperty}")]
        public int test2 { get; set; }
		
		[Value("${parent:name}")]//json文件如果嵌套对象可以冒号隔开
        public string ParentName { get; set; }

		//可以直接指定值
		[Value("2")]
		public int test3 { get; set; }
	}
```

2.xml格式的文件
```xml
<?xml version="1.0" encoding="utf-8" ?>
<autofac>
  <a11>aaaaaaaaa1</a11>
  <list>1,2,3</list>
  <dic>#{'name': 'name1'}</dic>
</autofac>
```
```csharp
    [Component]
    [PropertySource("/file/appsettings1.xml")]
    public class A11
    {
        public A11([Value("${a11}")]string school,[Value("${list}")]List<int> list,[Value("#{${dic}}")]Dictionary<string,string> dic)
        {
            this.School = school;
            this.list = list;
            this.dic = dic;

        }
        public string School { get; set; }
        public List<int> list { get; set; } 
        public Dictionary<string,string> dic { get; set; } 
    }
```
3.不指定PropertySource的话会默认从工程目录的 appsettings.json获取值

### AutoConfiguration标签 和 Bean标签
```csharp
    [AutoConfiguration]
    public class TestConfiguration
    {
        //Bean标签只能搭配AutoConfiguration标签使用，在其他地方没有效
	    //并且是单例注册
        [Bean]
        private ITestModel4 getTest5()
        {
            return new TestModel4
            {
                Name = "getTest5"
            };
        }
    }
```
在容器build完成后执行： 扫描指定的程序集，发现如果有打了AutoConfiguration标签的class，就会去识别有Bean标签的方法，并执行方法将方法返回实例注册为方法返回类型到容器！ 一个程序集可以有多个AutoConfiguration标签的class会每个都加载。

AutoConfiguration标签的其他属性：
- OrderIndex 可以通过OrderIndex设置优先级，越大的越先加载。
- Key 也可以通过Key属性设置

搭配如下代码可以设置过滤你想要加载的，比如你想要加载Key = “test” 的所有 AutoConfiguration标签class
`builder.RegisterModule(new AutofacAnnotationModule(typeof(AnotationTest).Assembly).SetAutofacConfigurationKey("test"));`

Bean标签的其他属性：
- Key 也可以通过Key属性设置 比如有多个方法返回的类型相同 可以设置Key来区分

### Aspect拦截器
```csharp
    [Component] //注册到容器
    [Aspect]//开启AOP拦截器
    public class TestModel
    {

        [TestHelloBefor]//参考下面的类 这是一个前置拦截器
        public virtual void Say()
        {
            Console.WriteLine("say");
        }

        [TestHelloAfter]//参考下面的类，这是一个后置拦截器
        public virtual void SayAfter()
        {
            Console.WriteLine("SayAfter");
        }

        [TestHelloArround]//参考下面的类，这是一个环绕拦截器
        public virtual void SayArround()
        {
            Console.WriteLine("SayArround");
        }
    }
    
    //定义一个前置拦截器
    public class TestHelloBefor : AspectBeforeAttribute
    {
        public override Task Before(IInvocation invocation)
        {
            Console.WriteLine("TestHelloBefor");
            return Task.CompletedTask;
        }
    }
    
    //定义一个后置拦截器
    public class TestHelloAfter : AspectAfterAttribute
    {

        public override Task After(IInvocation invocation, Exception exp)
        {
            if(exp!=null) Console.WriteLine(exp.Message);
            Console.WriteLine("TestHelloAfter");
            return Task.CompletedTask;
        }
    }

    //定义一个环绕拦截器
    public class TestHelloArround : AspectAroundAttribute
    {

        public override Task After(IInvocation invocation, Exception exp)
        {
            if (exp != null) Console.WriteLine(exp.Message);
            Console.WriteLine("TestHelloArround");
            return Task.CompletedTask;
        }

        public override Task Before(IInvocation invocation)
        {
            Console.WriteLine("TestHelloArround.Before");
            return Task.CompletedTask;
        }
    }
    
    //测试代码
     public void Test_Type_08()
     {
            var builder = new ContainerBuilder();

            // autofac打标签模式
            builder.RegisterModule(new AutofacAnnotationModule(typeof(TestModel).Assembly));

            var container = builder.Build();

            var a12 = container.Resolve<TestModel>();

            a12.Say();
            a12.SayAfter();
            a12.SayArround();

    }
```