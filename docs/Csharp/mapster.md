# Mapster

[开源地址](https://github.com/MapsterMapper/Mapster)

[文章来源-【5min+】 对象映射只有AutoMapper？试试Mapster  -作者：句幽](https://www.cnblogs.com/uoyo/p/12342290.html)

## 介绍

Mapster是.net下一个对象映射工具，作用同AutoMapper一样

## 使用

写一个小例子，对比两者在开发中的区别：
```csharp
public class MyEntity
{
    public string Name { get; set; }

    public int No { get; set; }
}

public class MyDto
{
    public string Name { get; set; }

    public int No { get; set; }
}
```

### AutoMapper
```csharp
var configuration = new MapperConfiguration(cfg =>
{
    cfg.CreateMap<MyEntity, MyDto>();
});

var mapper = configuration.CreateMapper();

var r = mapper.Map<MyDto>(new MyEntity() { Name = "xxx", No = 111 });
```
这是9.0的版本，如果您用过以前的版本可能会有点差异，比如老版本会使用Initialize方法来配置。但是思路都是一样的，也就是说，咱们需要先配置对象与对象之间的相互关系，然后创建一个Mapper，在.NET core中咱们一般会在Configura配置好之后，将mapper注册为一个单例，以后使用的话通过依赖注入就可以使用了。

### Mapster
```csharp
var entity = new MyEntity() { Name = "xxx", No = 111 };
var r = entity.Adapt<MyDto>();
```

是的，没有看错，只有一句代码。（虽然我写了两句😜）。 当我们安装了Mapster之后，object对象就会拥有一个 Adapt() 的扩展方法。只需要调用该方法就可以直接完成转换。对于简单的关系，我们根本都不需要进行配置。

那么对于复杂的映射呢？ Mapster 提供了一个 TypeAdapterConfig<T,T> 的静态泛型类型来进行配置，所以我们可以在任何地方书写配置:
```csharp
TypeAdapterConfig<MyEntity, MyDto>
    .NewConfig()
    .Map(s => s.Name, d => d.Name + "_Mapster");
```

## 小试牛刀
```csharp
public class MyDto
{
    public string Name { get; set; }

    public int No { get; set; }

    public string UoyoName { get; set; }
}

public class MyEntity
{
    public string Name { get; set; }

    public int No { get; set; }

    public ChildEntity UoyoCSharp { get; set; }
}

public class ChildEntity
{
    public string Name { get; set; }
}
```

在MyEntity里面拥有一个ChildEntity的类型。 “什么？您问我为什么不好好命名，比如ChildEntity就命名为Child呀，为什么要命名成读不懂的东西。” 因为……您命名规范了，根本都不用写配置，Mapster会自动完成映射。 所以基于这个不规则的命名，我们只需要进行下面的配置就行了：
```csharp
TypeAdapterConfig<MyEntity, MyDto>
           .NewConfig()
           .Map(s => s.UoyoName, d => d.UoyoCSharp.Name);
```

更加官方提供的性能测试对比，性能优于AutoMapper