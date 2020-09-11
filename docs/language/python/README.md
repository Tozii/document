# python基础语法

**数字类型**

int (整数), 如 1, 只有一种整数类型 int，表示为长整型，没有 python2 中的 Long。<br/>
bool (布尔), 如 True。<br/>
float (浮点数), 如 1.23、3E-2<br/>
complex (复数), 如 1 + 2j、 1.1 + 2.2j<br/>

**标准数据类型**

Number（数字）<br/>
String（字符串）<br/>
List（列表）<br/>
Tuple（元组）<br/>
Set（集合）<br/>
Dictionary（字典）<br/>

Python中的字符串用单引号 ' 或双引号 " 括起来，同时使用反斜杠 \ 转义特殊字符。<br/>
列表是写在方括号 [] 之间、用逗号分隔开的元素列表。<br/>
加号 + 是列表连接运算符，星号 * 是重复操作。如下实例：<br/>
```python
#!/usr/bin/python3

list = [ 'abcd', 786 , 2.23, 'runoob', 70.2 ]
tinylist = [123, 'runoob']

print (list)            # 输出完整列表
print (list[0])         # 输出列表第一个元素
print (list[1:3])       # 从第二个开始输出到第三个元素
print (list[2:])        # 输出从第三个元素开始的所有元素
print (tinylist * 2)    # 输出两次列表
print (list + tinylist) # 连接列表
```

元组（tuple）与列表类似，不同之处在于元组的元素不能修改。元组写在小括号 () 里，元素之间用逗号隔开。
```python
#!/usr/bin/python3

tuple = ( 'abcd', 786 , 2.23, 'runoob', 70.2  )
tinytuple = (123, 'runoob')

print (tuple)             # 输出完整元组
print (tuple[0])          # 输出元组的第一个元素
print (tuple[1:3])        # 输出从第二个元素开始到第三个元素
print (tuple[2:])         # 输出从第三个元素开始的所有元素
print (tinytuple * 2)     # 输出两次元组
print (tuple + tinytuple) # 连接元组
```

集合（set）是由一个或数个形态各异的大小整体组成的，构成集合的事物或对象称作元素或是成员。<br/>
基本功能是进行成员关系测试和删除重复元素。<br/>
可以使用大括号 { } 或者 set() 函数创建集合，注意：创建一个空集合必须用 set() 而不是 { }，因为 { } 是用来创建一个空字典。<br/>
创建格式：
```python
#!/usr/bin/python3

sites = {'Google', 'Taobao', 'Runoob', 'Facebook', 'Zhihu', 'Baidu'}
print(sites)   # 输出集合，重复的元素被自动去掉

# 成员测试
if 'Runoob' in sites :
    print('Runoob 在集合中')
else :
    print('Runoob 不在集合中')

# set可以进行集合运算
a = set('abracadabra')
b = set('alacazam')

print(a)
print(a - b)     # a 和 b 的差集
print(a | b)     # a 和 b 的并集
print(a & b)     # a 和 b 的交集
print(a ^ b)     # a 和 b 中不同时存在的元素
```

字典（dictionary）是Python中另一个非常有用的内置数据类型。<br/>
列表是有序的对象集合，字典是无序的对象集合。两者之间的区别在于：字典当中的元素是通过键来存取的，而不是通过偏移存取。<br/>
字典是一种映射类型，字典用 { } 标识，它是一个无序的 键(key) : 值(value) 的集合。<br/>
键(key)必须使用不可变类型。<br/>
在同一个字典中，键(key)必须是唯一的。<br/>
```python
#!/usr/bin/python3

dict = {}
dict['one'] = "1 - 菜鸟教程"
dict[2]     = "2 - 菜鸟工具"

tinydict = {'name': 'runoob','code':1, 'site': 'www.runoob.com'}


print (dict['one'])       # 输出键为 'one' 的值
print (dict[2])           # 输出键为 2 的值
print (tinydict)          # 输出完整的字典
print (tinydict.keys())   # 输出所有键
print (tinydict.values()) # 输出所有值
```

Python 中的变量不需要声明。每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。
```python
#!/usr/bin/python3

counter = 100          # 整型变量
miles   = 1000.0       # 浮点型变量
name    = "runoob"     # 字符串

print (counter)
print (miles)
print (name)
```

**条件控制**

1、每个条件后面要使用冒号 :，表示接下来是满足条件后要执行的语句块。<br/>
2、使用缩进来划分语句块，相同缩进数的语句在一起组成一个语句块。<br/>
3、在Python中没有switch – case语句。<br/>
```python
#!/usr/bin/python3
 
age = int(input("请输入你家狗狗的年龄: "))
print("")
if age <= 0:
    print("你是在逗我吧!")
elif age == 1:
    print("相当于 14 岁的人。")
elif age == 2:
    print("相当于 22 岁的人。")
elif age > 2:
    human = 22 + (age -2)*5
    print("对应人类年龄: ", human)
 
### 退出提示
input("点击 enter 键退出")
```

while语句
```python
#!/usr/bin/python3 
 
# 该实例演示了数字猜谜游戏
number = 7
guess = -1
print("数字猜谜游戏!")
while guess != number:
    guess = int(input("请输入你猜的数字："))
 
    if guess == number:
        print("恭喜，你猜对了！")
    elif guess < number:
        print("猜的数字小了...")
    elif guess > number:
        print("猜的数字大了...")
```

for语句
```python
#!/usr/bin/python3
 
sites = ["Baidu", "Google","Runoob","Taobao"]
for site in sites:
    if site == "Runoob":
        print("菜鸟教程!")
        break
    print("循环数据 " + site)
else:
    print("没有循环数据!")
print("完成循环!")
```

**函数**

函数代码块以 def 关键词开头，后接函数标识符名称和圆括号 ()。<br/>
任何传入参数和自变量必须放在圆括号中间，圆括号之间可以用于定义参数。<br/>
函数的第一行语句可以选择性地使用文档字符串—用于存放函数说明。<br/>
函数内容以冒号起始，并且缩进。<br/>
return [表达式] 结束函数，选择性地返回一个值给调用方。不带表达式的return相当于返回 None。<br/>

> python 使用修饰器 `@staticmethod` 定义静态方法

[python装饰器用法](https://www.jb51.net/article/168276.htm)

```python
class Dog(object):
  def __init__(self,name):
    self.name=name
  def talk(self):
    print("%s is talking"%self.name)
  @staticmethod
  def eat(self,food):##
    print("%s is eating %s"%(self.name,food))
 
  @staticmethod
  def bulk(): ##如果不涉及实例变量的内容，可以不传self
    print("wang wang!")
 
d=Dog("haha")
d.talk()
Dog.eat(d,"baozi")
d.eat(d,"mianbao")
d.bulk()
Dog.bulk()
```

```python
#!/usr/bin/python3
 
# 计算面积函数
def area(width, height):
    return width * height
 
def print_welcome(name):
    print("Welcome", name)
 
print_welcome("Runoob")
w = 4
h = 5
print("width =", w, " height =", h, " area =", area(w, h))
```

加了星号 * 的参数会以元组(tuple)的形式导入，存放所有未命名的变量参数。
```python
#!/usr/bin/python3
  
# 可写函数说明
def printinfo( arg1, *vartuple ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   print (vartuple)
 
# 调用printinfo 函数
printinfo( 70, 60, 50 )
```

两个星号 ** 的参数会以字典的形式导入。
```python
#!/usr/bin/python3
  
# 可写函数说明
def printinfo( arg1, **vardict ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   print (vardict)
 
# 调用printinfo 函数
printinfo(1, a=2,b=3)
```

匿名函数
```python
#!/usr/bin/python3
 
# 可写函数说明
sum = lambda arg1, arg2: arg1 + arg2
 
# 调用sum函数
print ("相加后的值为 : ", sum( 10, 20 ))
print ("相加后的值为 : ", sum( 20, 20 ))
```

**类定义**

类的方法与普通的函数只有一个特别的区别——它们必须有一个额外的第一个参数名称, 按照惯例它的名称是 self。

> python 实例化类和其它大多数语言不通不需要关键字 **new**
```python
#!/usr/bin/python3
 
class MyClass:
    """一个简单的类实例"""
    i = 12345
    def f(self):
        return 'hello world'
 
# 实例化类
x = MyClass()
 
# 访问类的属性和方法
print("MyClass 类的属性 i 为：", x.i)
print("MyClass 类的方法 f 输出为：", x.f())
```
类有一个名为 `__init__()` 的特殊方法（构造方法），该方法在类实例化时会自动调用，像下面这样：
```python
def __init__(self):
    self.data = []
```

当然， `__init__()` 方法可以有参数，参数通过 `__init__()` 传递到类的实例化操作上。例如:
```python
#!/usr/bin/python3
 
class Complex:
    def __init__(self, realpart, imagpart):
        self.r = realpart
        self.i = imagpart
x = Complex(3.0, -4.5)
print(x.r, x.i)   # 输出结果：3.0 -4.5
```

类属性与方法:<br/>
__private_attrs：两个下划线开头，声明该属性为私有，不能在类的外部被使用或直接访问。在类内部的方法中使用时 self.__private_attrs。<br/>
__private_method：两个下划线开头，声明该方法为私有方法，只能在类的内部调用 ，不能在类的外部调用。self.__private_methods。<br/>

> python可以多继承 如：`class Complex(Base1,Base2):` 需要注意圆括号中父类的顺序，若是父类中有相同的方法名，而在子类使用时未指定，python从左至右搜索 即方法在子类中未找到时，从左到右查找父类中是否包含方法。

**模块**

模块是一个包含所有你定义的函数和变量的文件，其后缀名是.py。模块可以被别的程序引入，以使用该模块中的函数等功能,使用 `import` 关键字引入模块

```python
#!/usr/bin/python3
# 文件名: using_sys.py
 
import sys
 
print('命令行参数如下:')
for i in sys.argv:
   print(i)
 
print('\n\nPython 路径为：', sys.path, '\n')
```

定义support.py 文件代码
```python
#!/usr/bin/python3
# Filename: support.py
 
def print_func( par ):
    print ("Hello : ", par)
    return
```

test.py 引入 support 模块：
```python
#!/usr/bin/python3
# Filename: test.py
 
# 导入模块
import support
 
# 现在可以调用模块里包含的函数了
support.print_func("Runoob")
```