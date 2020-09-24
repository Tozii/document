# TypeScript

## 介绍
---
TypeScript 是 JavaScript 的一个超集，支持 ECMAScript 6 标准。  
TypeScript 由微软开发的自由和开源的**面向对象**编程语言。首次发布于2012年10月，是一门**静态类型**语言（node.js首次发布2009年5月，2011年7月，node在微软的支持下发布Windows版本）
TypeScript 设计目标是开发大型应用，它可以编译成纯 JavaScript，编译出来的 JavaScript 可以运行在任何浏览器上。

#### 语言特性
TypeScript 是一种给 JavaScript 添加特性的语言扩展。增加的功能包括：
- 类型批注和编译时类型检查
- 类型推断
- 类型擦除
- 接口
- 枚举
- Mixin
- 泛型编程
- 名字空间
- 元组
- Await

以下功能是从 ECMA 2015 反向移植而来：
- 类
- 模块
- lambda 函数的箭头语法
- 可选参数以及默认参数

## 基础类型
---
- 任意类型	any	声明为 any 的变量可以赋予任意类型的值。
- 数字类型	number 双精度 64 位浮点值。它可以用来表示整数和分数。

    let binaryLiteral: number = 0b1010; // 二进制
    let octalLiteral: number = 0o744;    // 八进制
    let decLiteral: number = 6;    // 十进制
    let hexLiteral: number = 0xf00d;    // 十六进制

- 字符串类型 string	一个字符系列，使用单引号（'）或双引号（"）来表示字符串类型。反引号（`）来定义多行文本和内嵌表达式。

    let name: string = "Runoob";
    let years: number = 5;
    let words: string = `您好，今年是 ${ name } 发布 ${ years + 1} 周年`;

- 布尔类型	boolean	表示逻辑值：true 和 false。

    let flag: boolean = true;

- 数组类型	无	声明变量为数组。

    // 在元素类型后面加上[]
    let arr: number[] = [1, 2];
    // 或者使用数组泛型
    let arr: Array<number> = [1, 2];

- 元组	无	元组类型用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同。

    let x: [string, number];
    x = ['Runoob', 1];    // 运行正常
    x = [1, 'Runoob'];    // 报错
    console.log(x[0]);    // 输出 Runoob

- 枚举	enum 枚举类型用于定义数值集合。

    enum Color {Red, Green, Blue};
    let c: Color = Color.Blue;
    console.log(c);    // 输出 2

- void	void 用于标识方法返回值的类型，表示该方法没有返回值。

    function hello(): void {
        alert("Hello Runoob");
    }
    
- null	null	表示对象值缺失。
- undefined	undefined	用于初始化变量为一个未定义的值
- never	never	never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。

#### 变量声明

`var [变量名] : [类型] = 值;`

声明变量并初始值，但不设置类型，该变量可以是任意类型：`var [变量名] = 值;`

示例：
```ts
var uname:string = "Runoob";
var score1:number = 50;
var score2:number = 42.50
var sum = score1 + score2
console.log("名字: "+uname)
console.log("第一个科目成绩: "+score1)
console.log("第二个科目成绩: "+score2)
console.log("总成绩: "+sum)
```

#### 变量作用域
```ts
var global_num = 12          // 全局变量
class Numbers { 
   num_val = 13;             // 实例变量
   static sval = 10;         // 静态变量
   
   storeNum():void { 
      var local_num = 14;    // 局部变量
   } 
} 
console.log("全局变量为: "+global_num)  
console.log(Numbers.sval)   // 静态变量
var obj = new Numbers(); 
console.log("实例变量: "+obj.num_val)
```

## 条件语句、循环
---

#### if...else if....else 语句
```ts
var num:number = 2 
if(num > 0) { 
    console.log(num+" 是正数") 
} else if(num < 0) { 
    console.log(num+" 是负数") 
} else { 
    console.log(num+" 不是正数也不是负数") 
}
```

#### switch…case 语句
```ts
var grade:string = "A"; 
switch(grade) { 
    case "A": { 
        console.log("优"); 
        break; 
    } 
    case "B": { 
        console.log("良"); 
        break; 
    } 
    case "C": {
        console.log("及格"); 
        break;    
    } 
    case "D": { 
        console.log("不及格"); 
        break; 
    }  
    default: { 
        console.log("非法输入"); 
        break;              
    } 
}
```

#### 循环

for 循环
```ts
var num:number = 5; 
var i:number; 
var factorial = 1; 
 
for(i = num;i>=1;i--) {
   factorial *= i;
}
console.log(factorial)
```
for...in 循环
```ts
var j:any; 
var n:any = "a b c" 
 
for(j in n) {
    console.log(n[j])  
}
```
TypeScript forEach 循环
```ts
let list = [4, 5, 6];
list.forEach((val, idx, array) => {
    // val: 当前值
    // idx：当前index
    // array: Array
});
```
while 循环
```ts
var num:number = 5; 
var factorial:number = 1; 
 
while(num >=1) { 
    factorial = factorial * num; 
    num--; 
} 
console.log("5 的阶乘为："+factorial);
```

## 函数
---
无参数：
```ts
// 函数定义
function greet():string { // 返回一个字符串
    return "Hello World" 
} 
 
function caller() { 
    var msg = greet() // 调用 greet() 函数 
    console.log(msg) 
} 
 
// 调用函数
caller()
```

带参数：
```ts
function add(x: number, y: number): number {
    return x + y;
}
console.log(add(1,2))
```

可选参数:
```ts
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}
 
let result1 = buildName("Bob");  // 正确
let result2 = buildName("Bob", "Adams", "Sr.");  // 错误，参数太多了
let result3 = buildName("Bob", "Adams");  // 正确
```

默认参数:
```ts
function calculate_discount(price:number,rate:number = 0.50) { 
    var discount = price * rate; 
    console.log("计算结果: ",discount); 
} 
calculate_discount(1000) 
calculate_discount(1000,0.30)
```

剩余参数:
有一种情况，我们不知道要向函数传入多少个参数，这时候我们就可以使用剩余参数来定义。   
剩余参数语法允许我们将一个不确定数量的参数作为一个数组传入。   
函数的最后一个命名参数 restOfName 以 ... 为前缀，它将成为一个由剩余参数组成的数组，索引值从0（包括）到 restOfName.length（不包括）。
```ts
function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}
  
let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```
```ts
function addNumbers(...nums:number[]) {  
    var i;   
    var sum:number = 0; 
    
    for(i = 0;i<nums.length;i++) { 
       sum = sum + nums[i]; 
    } 
    console.log("和为：",sum) 
 } 
 addNumbers(1,2,3) 
 addNumbers(10,10,10,10,10)
```

#### TypeScript 联合类型
联合类型（Union Types）可以通过管道(|)将变量设置多种类型，赋值时可以根据设置的类型来赋值。   
注意：只能赋值指定的类型，如果赋值其它类型就会报错。
```ts
var val:string|number 
val = 12 
console.log("数字为 "+ val) 
val = "Runoob" 
console.log("字符串为 " + val)
```

## 接口&类

接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要由具体的类去实现，然后第三方就可以通过这组抽象方法调用，让具体的类执行具体的方法

#### 示例

以下实例中，我们定义了一个接口 IPerson，接着定义了一个变量 customer，它的类型是 IPerson。   
customer 实现了接口 IPerson 的属性和方法。
```ts
interface IPerson { 
    firstName:string, 
    lastName:string, 
    sayHi: ()=>string 
} 
 
var customer:IPerson = { 
    firstName:"Tom",
    lastName:"Hanks", 
    sayHi: ():string =>{return "Hi there"} 
} 
 
console.log("Customer 对象 ") 
console.log(customer.firstName) 
console.log(customer.lastName) 
console.log(customer.sayHi())  
 
var employee:IPerson = { 
    firstName:"Jim",
    lastName:"Blakes", 
    sayHi: ():string =>{return "Hello!!!"} 
} 
 
console.log("Employee  对象 ") 
console.log(employee.firstName) 
console.log(employee.lastName)
```
#### 单继承实例
```ts
interface Person { 
   age:number 
} 
 
interface Musician extends Person { 
   instrument:string 
} 
 
var drummer = <Musician>{}; 
drummer.age = 27 
drummer.instrument = "Drums" 
console.log("年龄:  "+drummer.age)
console.log("喜欢的乐器:  "+drummer.instrument)
```
#### 多继承实例
```ts
interface IParent1 { 
    v1:number 
} 
 
interface IParent2 { 
    v2:number 
} 
 
interface Child extends IParent1, IParent2 { } 
var Iobj:Child = { v1:12, v2:23} 
console.log("value 1: "+Iobj.v1+" value 2: "+Iobj.v2)
```

类描述了所创建的对象共同的属性和方法。
```ts
class Car { 
   // 字段
   engine:string; 
   
   // 构造函数
   constructor(engine:string) { 
      this.engine = engine 
   }  
   
   // 方法
   disp():void { 
      console.log("函数中显示发动机型号  :   "+this.engine) 
   } 
} 
 
// 创建一个对象
var obj = new Car("XXSY1")
 
// 访问字段
console.log("读取发动机型号 :  "+obj.engine)  
 
// 访问方法
obj.disp()
```
#### 类的继承
- TypeScript 支持继承类，即我们可以在创建类的时候继承一个已存在的类，这个已存在的类称为父类，继承它的类称为子类。
- 类继承使用关键字 extends，子类除了不能继承父类的私有成员(方法和属性)和构造函数，其他的都可以继承。
- TypeScript 一次只能继承一个类，不支持继承多个类，但 TypeScript 支持多重继承（A 继承 B，B 继承 C）。
```ts
class Shape { 
   Area:number 
   
   constructor(a:number) { 
      this.Area = a 
   } 
} 
 
class Circle extends Shape { 
   disp():void { 
      console.log("圆的面积:  "+this.Area) 
   } 
}
  
var obj = new Circle(223); 
obj.disp()
```
#### 继承类的方法重写
```ts
class PrinterClass { 
   doPrint():void {
      console.log("父类的 doPrint() 方法。") 
   } 
} 
 
class StringPrinter extends PrinterClass { 
   doPrint():void { 
      super.doPrint() // 调用父类的函数
      console.log("子类的 doPrint()方法。")
   } 
}
```
> static 关键字用于定义类的数据成员（属性和方法）为静态的，静态成员可以直接通过类名调用

#### 访问控制修饰符

TypeScript 中，可以使用访问控制符来保护对类、变量、方法和构造方法的访问。TypeScript 支持 3 种不同的访问权限。
- public（默认） : 公有，可以在任何地方被访问。
- protected : 受保护，可以被其自身以及其子类和父类访问。
- private : 私有，只能被其定义所在的类访问。

#### 类和接口

类可以实现接口，使用关键字 implements，并将 interest 字段作为类的属性使用。
```ts
interface ILoan { 
   interest:number 
} 
 
class AgriLoan implements ILoan { 
   interest:number 
   rebate:number 
   
   constructor(interest:number,rebate:number) { 
      this.interest = interest 
      this.rebate = rebate 
   } 
} 
 
var obj = new AgriLoan(10,1) 
console.log("利润为 : "+obj.interest+"，抽成为 : "+obj.rebate )
```

## TypeScript 命名空间

命名空间一个最明确的目的就是解决重名问题。

TypeScript 中命名空间使用 namespace 来定义，语法格式如下：
```ts
namespace SomeNameSpaceName { 
   export interface ISomeInterfaceName {      }  
   export class SomeClassName {      }  
}
```
以上定义了一个命名空间 SomeNameSpaceName，如果我们需要在外部可以调用 SomeNameSpaceName 中的类和接口，则需要在类和接口添加 export 关键字。

要在另外一个命名空间调用语法格式为：
```ts
SomeNameSpaceName.SomeClassName;
```
如果一个命名空间在一个单独的 TypeScript 文件中，则应使用三斜杠 /// 引用它，语法格式如下：
```ts
/// <reference path = "SomeFileName.ts" />
```
以下实例演示了命名空间的使用，定义在不同文件中：
```ts
//IShape.ts 文件代码：
namespace Drawing { 
    export interface IShape { 
        draw(); 
    }
}

//Circle.ts 文件代码：
/// <reference path = "IShape.ts" /> 
namespace Drawing { 
    export class Circle implements IShape { 
        public draw() { 
            console.log("Circle is drawn"); 
        }  
    }
}

//Triangle.ts 文件代码：
/// <reference path = "IShape.ts" /> 
namespace Drawing { 
    export class Triangle implements IShape { 
        public draw() { 
            console.log("Triangle is drawn"); 
        } 
    } 
}

//TestShape.ts 文件代码：
/// <reference path = "IShape.ts" />   
/// <reference path = "Circle.ts" /> 
/// <reference path = "Triangle.ts" />  
function drawAllShapes(shape:Drawing.IShape) { 
    shape.draw(); 
} 
drawAllShapes(new Drawing.Circle());
drawAllShapes(new Drawing.Triangle());
```

## TypeScript 模块
---
TypeScript 模块的设计理念是可以更换的组织代码。   
模块是在其自身的作用域里执行，并不是在全局作用域，这意味着定义在模块里面的变量、函数和类等在模块外部是不可见的，除非明确地使用 export 导出它们。类似地，我们必须通过 import 导入其他模块导出的变量、函数、类等。

两个模块之间的关系是通过在文件级别上使用 import 和 export 建立的。   
模块使用模块加载器去导入其它的模块。 在运行时，模块加载器的作用是在执行此模块代码前去查找并执行这个模块的所有依赖。 大家最熟知的JavaScript模块加载器是服务于 Node.js 的 CommonJS 和服务于 Web 应用的 Require.js。   
此外还有有 SystemJs 和 Webpack。   
模块导出使用关键字 export 关键字，语法格式如下：
```ts
// 文件名 : SomeInterface.ts 
export interface SomeInterface { 
   // 代码部分
}
```
要在另外一个文件使用该模块就需要使用 import 关键字来导入:
```ts
import someInterfaceRef = require("./SomeInterface");
```

#### 示例
```ts
//IShape.ts 文件代码：
/// <reference path = "IShape.ts" /> 
export interface IShape { 
   draw(); 
}

//Circle.ts 文件代码：
import shape = require("./IShape"); 
export class Circle implements shape.IShape { 
   public draw() { 
      console.log("Cirlce is drawn (external module)"); 
   } 
}

//Triangle.ts 文件代码：
import shape = require("./IShape"); 
export class Triangle implements shape.IShape { 
   public draw() { 
      console.log("Triangle is drawn (external module)"); 
   } 
}

//TestShape.ts 文件代码：
import shape = require("./IShape"); 
import circle = require("./Circle"); 
import triangle = require("./Triangle");  
 
function drawAllShapes(shapeToDraw: shape.IShape) {
   shapeToDraw.draw(); 
} 
 
drawAllShapes(new circle.Circle()); 
drawAllShapes(new triangle.Triangle());
```

## TypeScript 声明文件
---

TypeScript 作为 JavaScript 的超集，在开发过程中不可避免要引用其他第三方的 JavaScript 的库。虽然通过直接引用可以调用库的类和方法，但是却无法使用TypeScript 诸如类型检查等特性功能。为了解决这个问题，需要将这些库里的函数和方法体去掉后只保留导出类型声明，而产生了一个描述 JavaScript 库和模块信息的声明文件。通过引用这个声明文件，就可以借用 TypeScript 的各种特性来使用库文件了。

假如我们想使用第三方库，比如 jQuery，我们通常这样获取一个 id 是 foo 的元素：
```js
$('#foo');
// 或
jQuery('#foo');
```
但是在 TypeScript 中，我们并不知道 $ 或 jQuery 是什么东西：   
这时，我们需要使用 declare 关键字来定义它的类型，帮助 TypeScript 判断我们传入的参数类型对不对：
```ts
declare var jQuery: (selector: string) => any;

jQuery('#foo');
```

#### 声明文件

声明文件以 .d.ts 为后缀，例如：
```ts
runoob.d.ts
```
声明文件或模块的语法格式如下：
```ts
declare module Module_Name {
}
```
TypeScript 引入声明文件语法格式：
```ts
/// <reference path = " runoob.d.ts" />
```

示例：
以下定义一个第三方库来演示：
```ts
//CalcThirdPartyJsLib.js 文件代码：
var Runoob;  
(function(Runoob) {
    var Calc = (function () { 
        function Calc() { 
        } 
    })
    Calc.prototype.doSum = function (limit) {
        var sum = 0; 
 
        for (var i = 0; i <= limit; i++) { 
            sum = sum + i; 
        }
        return sum; 
    }
    Runoob.Calc = Calc; 
    return Calc; 
})(Runoob || (Runoob = {})); 
var test = new Runoob.Calc();
```
如果我们想在 TypeScript 中引用上面的代码，则需要设置声明文件 Calc.d.ts，代码如下：
```ts
//Calc.d.ts 文件代码：
declare module Runoob { 
   export class Calc { 
      doSum(limit:number) : number; 
   }
}
```
声明文件不包含实现，它只是类型声明，把声明文件加入到 TypeScript 中：
```ts
/// <reference path = "Calc.d.ts" /> 
var obj = new Runoob.Calc(); 
// obj.doSum("Hello"); // 编译错误
console.log(obj.doSum(10));
```