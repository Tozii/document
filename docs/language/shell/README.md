# shell script基础语法

**变量**
```shell
your_name="qinjx"
echo $your_name
echo ${your_name}
```

**数组**

Shell 数组用括号来表示，元素用"空格"符号分割开，语法格式如下：
`array_name=(value1 value2 ... valuen)`

读取数组

`${array_name[index]}`

获取数组长度(使用@ 或 * 可以获取数组中的所有元素)

`${#array_name[*]}`

**逻辑运算符**

&& 逻辑and    || 逻辑or

read 命令从标准输入中读取一行,并把输入行的每个字段的值指定给 shell 变量
```shell
#!/bin/sh
read name 
echo "$name It is a test"
```

**文件测试运算符**

|操作符	|说明	|举例 |
| --- | --- | --- |
|-b file|	检测文件是否是块设备文件，如果是，则返回 true。|	[ -b $file ] 返回 false。|
|-c file|	检测文件是否是字符设备文件，如果是，则返回 true。|	[ -c $file ] 返回 false。|
|-d file|	检测文件是否是目录，如果是，则返回 true。	|[ -d $file ] 返回 false。|
|-f file|	检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。	|[ -f $file ] 返回 true。|
|-g file|	检测文件是否设置了 SGID 位，如果是，则返回 true。	|[ -g $file ] 返回 false。|
|-k file|	检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。	|[ -k $file ] 返回 false。|
|-p file|	检测文件是否是有名管道，如果是，则返回 true。	|[ -p $file ] 返回 false。|
|-u file|	检测文件是否设置了 SUID 位，如果是，则返回 true。	|[ -u $file ] 返回 false。|
|-r file|	检测文件是否可读，如果是，则返回 true。	|[ -r $file ] 返回 true。|
|-w file|	检测文件是否可写，如果是，则返回 true。	|[ -w $file ] 返回 true。|
|-x file|	检测文件是否可执行，如果是，则返回 true。	|[ -x $file ] 返回 true。|
|-s file|	检测文件是否为空（文件大小是否大于0），不为空返回 true。	|[ -s $file ] 返回 true。|
|-e file|	检测文件（包括目录）是否存在，如果是，则返回 true。|
```shell
file="/var/www/runoob/test.sh"
if [ -r $file ]
then
   echo "文件可读"
else
   echo "文件不可读"
fi
if [ -w $file ]
then
   echo "文件可写"
else
   echo "文件不可写"
fi
if [ -x $file ]
then
   echo "文件可执行"
else
   echo "文件不可执行"
fi
```

**流程控制**

if 语句语法格式
```shell
a=10
b=20
if [ $a == $b ]
then
   echo "a 等于 b"
elif [ $a -gt $b ]
then
   echo "a 大于 b"
elif [ $a -lt $b ]
then
   echo "a 小于 b"
else
   echo "没有符合的条件"
fi
```

for语句
```shell
for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done
```

case语句
```shell
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```
case工作方式如上所示。取值后面必须为单词in，每一模式必须以右括号结束。取值可以为变量或常数。匹配发现取值符合某一模式后，其间所有命令开始执行直至 ;;。

**函数**

可以带function fun() 定义，也可以直接fun() 定义,不带任何参数。
```shell
demoFun(){
    echo "这是我的第一个 shell 函数!"
}
```
在Shell中，调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数... 带参数的函数示例：
```shell
funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```

`$?` 获取上一个函数的返回值（紧跟函数后面执行）