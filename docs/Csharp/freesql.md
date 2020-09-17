# FreeSql

[开源地址](https://github.com/dotnetcore/FreeSql)   
[官方文档](https://github.com/dotnetcore/FreeSql/wiki/%E5%85%A5%E9%97%A8)

## 介绍

FreeSql 是功能强大的对象关系映射技术(O/RM)，支持 .NETCore 2.1+ 或 .NETFramework 4.0+ 或 Xamarin。
- 支持 CodeFirst 迁移，哪怕使用 Access 数据库也支持；
- 支持 DbFirst 从数据库导入实体类，安装实体类生成工具；
- 支持 深入的类型映射，比如pgsql的数组类型；
- 支持 丰富的表达式函数，以及灵活的自定义解析；
- 支持 导航属性一对多、多对多贪婪加载，以及延时加载；
- 支持 读写分离、分表分库、过滤器、乐观锁、悲观锁；
- 支持 MySql/SqlServer/PostgreSQL/Oracle/Sqlite/Firebird/达梦/人大金仓/神舟通用/Access；

## 使用
---
### 模型
```csharp
using FreeSql.DataAnnotations;
using System;

public class Blog 
{
    [Column(IsIdentity = true, IsPrimary = true)]
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

### 声明
```csharp
static IFreeSql fsql = new FreeSql.FreeSqlBuilder()
    .UseConnectionString(FreeSql.DataType.Sqlite, @"Data Source=db1.db")
    .UseAutoSyncStructure(true) //自动同步实体结构到数据库
    .Build(); //请务必定义成 Singleton 单例模式
```

### 迁移
程序运行中FreeSql会检查`AutoSyncStructure`参数，以此条件判断是否对比实体与数据库结构之间的变化，达到自动迁移的目的。(生产环境谨慎)

### 查询
```csharp
var blogs = fsql.Select<Blog>()
    .Where(b => b.Rating > 3)
    .OrderBy(b => b.Url)
    .Skip(100)
    .Limit(10) //第100行-110行的记录
    .ToList();
```

### 插入
```csharp
var blog = new Blog { Url = "http://sample.com" };
blog.BlogId = (int)fsql.Insert<Blog>()
    .AppendData(blog)
    .ExecuteIdentity();
```

### 更新
```csharp
fsql.Update<Blog>()
    .Set(b => b.Url, "http://sample2222.com")
    .Where(b => b.Url == "http://sample.com")
    .ExecuteAffrows();
```

### 删除
```csharp
fsql.Delete<Blog>()
    .Where(b => b.Url == "http://sample.com")
    .ExecuteAffrows();
```

## 说明
---
### FreeSqlBuilder
| 方法 | 返回值 | 说明 |
| -----| ---- | :---- |
| UseConnectionString | this | 	设置连接串 |
| UseSlave	| this	| 设置从数据库，支持多个| 
| UseConnectionFactory	| this	| 设置自定义数据库连接对象（放弃内置对象连接池技术）| 
| UseAutoSyncStructure	| this	| 【开发环境必备】自动同步实体结构到数据库，程序运行中检查实体创建或修改表结构| 
| UseNoneCommandParameter	| this	| 不使用命令参数化执行，针对 Insert/Update，也可临时使用 IInsert/IUpdate.NoneParameter()| 
| UseGenerateCommandParameterWithLambda	this	| 生成命令参数化执行，针对 lambda 表达式解析| 
| UseLazyLoading	| this	| 开启延时加载功能| 
| UseMonitorCommand	| this	| 监视全局 SQL 执行前后| 
| UseNameConvert	| this	| 自动转换名称 Entity -> Db| 
| UseExitAutoDisposePool	| this	| 监听 AppDomain.CurrentDomain.ProcessExit/Console.CancelKeyPress 事件自动释放连接池 (默认true)| 
| Build<T>	| IFreeSql<T>	| 创建一个 IFreeSql 对象，注意：单例设计，不要重复创建| 

### ConnectionStrings
| DataType	| ConnectionString| 
| -----| ---- |
| DataType.MySql	| Data Source=127.0.0.1;Port=3306;User ID=root;Password=root; Initial Catalog=cccddd;Charset=utf8; SslMode=none;Min pool size=1| 
| DataType.PostgreSQL	| Host=192.168.164.10;Port=5432;Username=postgres;Password=123456; Database=tedb;Pooling=true;Minimum Pool Size=1| 
| DataType.SqlServer	| Data Source=.;Integrated Security=True;Initial Catalog=freesqlTest;Pooling=true;Min Pool Size=1| 
| DataType.Oracle	| user id=user1;password=123456; data source=//127.0.0.1:1521/XE;Pooling=true;Min Pool Size=1| 
| DataType.Sqlite	| Data Source=|DataDirectory|\document.db; Attachs=xxxtb.db; Pooling=true;Min Pool Size=1| 