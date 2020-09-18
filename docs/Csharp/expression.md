# Expression 表达式

## 表达式和表达式树
---
表达式可以是一个参数（如参数X），一个常数（如常数5），一个加运算（如x+5）等等，可以把几个小的表达式组装在一起成为大的表达式，例如：（x+5）-（++y）。对于这样一个表达式可以用一棵树来表示，如下：
```
                        -
                        |
         一 一 一 一 一 一一 一 一 一 一 一
         |                             |
         +                             ++
    一 一一 一                      一 一一 一
    |       |                          |
    x       5                          y
```
这就是表达式树，表达式树本身也是一个表达式（大的表达式）。一个表达式也是一颗表达式树，可以说它是一棵小的表达式树。可以把表达式树和表达式认为是一个东西，C#中都是用Expression类表示。

## 表达书树结构
---
对于一颗表达书树，其叶子节点都是参数或者常数，非叶子节点都是运算符或者控制符。

C#中的Expression类就是表达式类，其中表达式的节点共有85种，全都定义在枚举类型`ExpressionType`中

所有有关表达式操作类型都是继承自`Expression`

`LambdaExpression`类型成员`NodeType`记录了表达式树根节点类型，如：（x+5）-（++y）  NodeType为`ExpressionType.Subtract`（减）

### 表达式树创建

1.使用Lambda表达式创建:`Expression<TDelegate>`类型，该类型继承自`LambdaExpression`,用于将强类型 lambda 表达式表示为数据结构
如：`Expression<Func<int,int,bool>> fun = (x,y) => x<y`

这种方法创建除的表达式根节点类型为`ExpressionType.Lambda`

由于`Expression<TDelegate>`类型继承自`LambdaExpression`，`LambdaExpression`成员属性`Body`获取lambda表达式主体：
```csharp
// 摘要:
//     获取 lambda 表达式的主体。
//
// 返回结果:
//     一个表示 lambda 表达式主体的 System.Linq.Expressions.Expression。
public Expression Body { get; }
```
`fun.Body`的节点类型为`LessThan`

`Expression<TDelegate>`类型的`Compile`方法(将一个Expression表达式转换成委托)：
```csharp
//
// 摘要:
//     将表达式树描述的 lambda 表达式编译为可执行代码，并生成表示该 lambda 表达式的委托。
//
// 返回结果:
//     一个 TDelegate 类型的委托，它表示由 System.Linq.Expressions.Expression<TDelegate> 描述的已编译的
//     lambda 表达式。
public TDelegate Compile();
```

2.组装法
```csharp
ParameterExpression p1=Expression.Parameter(typeof(int),"x");
ParameterExpression p2 = Expression.Parameter(typeof(int),"y");
BinaryExpression bexpr = Expression.GreaterThan(p1, p2);

`BinaryExpression`类型表示包含二元运算符的表达式。
```
`Expression`类型部分静态方法：
```csharp
//
// 摘要:
//     创建一个表示“大于”数值比较的 System.Linq.Expressions.BinaryExpression。
//
// 参数:
//   left:
//     要将 System.Linq.Expressions.BinaryExpression.Left 属性设置为与其相等的 System.Linq.Expressions.Expression。
//
//   right:
//     要将 System.Linq.Expressions.BinaryExpression.Right 属性设置为与其相等的 System.Linq.Expressions.Expression。
[TargetedPatchingOptOut("Performance critical to inline this type of method across NGen image boundaries")]
public static BinaryExpression GreaterThan(Expression left, Expression right);
```
```csharp
//
// 摘要:
//     创建一个 System.Linq.Expressions.ParameterExpression 节点，该节点可用于标识表达式树中的参数或变量。
//
// 参数:
//   type:
//     参数或变量的类型。
//
//   name:
//     仅用于调试或打印目的的参数或变量的名称。
public static ParameterExpression Parameter(Type type, string name);
```
由以上参考可知该组装法最终生成而已表达式 `x > y`

### 表达式的运行

大多数表达式都不能运行，只有`LambdaExpression` 和 `Expression<TDelegate>` 表达式可以运行，因为这两种表达式相当于一个函数，且都拥有一个成员函数`Compile`，可把表达式转换成委托，如：
```csharp
Expression<Fun<int,int>> fun = x => x+6;
var f=fun.Compile();
f(5);
```

其它类型表达式如果想要运行，可以通过`Expression`类的静态函数`Lambda<TDelegate>`转换成相应的`Expression<TDelegate>`表达式，然后调用`Compile`方法:
```csharp
ParameterExpression p1=Expression.Parameter(typeof(int),"x");
ParameterExpression p2 = Expression.Parameter(typeof(int),"y");
BinaryExpression bexpr = Expression.GreaterThan(p1, p2);

Expression<Func<int,int,bool>> exp =Expression.Lambda<Func<int,int,bool>>(bexpr,p1,p2);

Console.WriteLine(exp.Compile().Invoke(2, 3)); //输出False
```

## 使用Expression实现Lambda表达式拼接
```csharp
public static class PredicateBuilder
{
    public static Expression<Func<T, bool>> True<T>()
    {
        return f => true;
    }

    public static Expression<Func<T, bool>> False<T>()
    {
        return f => false;
    }

    public static Expression<Func<T, bool>> Or<T>(this Expression<Func<T, bool>> ex1, Expression<Func<T, bool>> ex2)
    {
        var invoked = Expression.Invoke(ex2, ex1.Parameters.Cast<Expression>());
        return Expression.Lambda<Func<T, bool>>(Expression.Or(ex1.Body, invoked), ex1.Parameters);
    }

    public static Expression<Func<T, bool>> And<T>(this Expression<Func<T, bool>> ex1, Expression<Func<T, bool>> ex2)
    {
        var invoked = Expression.Invoke(ex2, ex1.Parameters.Cast<Expression>());
        return Expression.Lambda<Func<T, bool>>(Expression.And(ex1.Body, invoked), ex1.Parameters);
    }
}

//使用
Expression<Func<Type, bool>> builder=a=>true;
builder = builder.And(a => a.Name == parame.Name);
builder = builder.And(a => a.Age == parame.Age);
var list = students.AsQueryable().Where(builder).ToList();
```