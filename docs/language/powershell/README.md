# powershell基础语法

**变量** powershell的变量无需预定义，可直接使用。当使用一个变量时，该变量被自动声明
变量名前加$ `$name="powershell"`

**函数**
无参定义
```powershell
function sayHello{
    $name="powershell"
    write-output $name
}
```

有参定义
方式一
```powershell
function sayHello([string]$name){
    write-output $name
}

sayHello -name "powershell"
sayHello "powershell"
```
方式二
```powershell
function sayHello{
    param(
        [string]$name
    )
    write-output $name
}
```

**Get类（部分）**
`Get-Process` 获取所有进程
`Get-History` 获取在当前会话中输入的命令列表
`Get-Job` 获取在当前会话中运行的Windows PowerShell后台作业
`Get-FormatData` 获取当前会话中的格式数据
`Get-Event` 获取事件队列中的事件
`Get-Alias` 获取当前会话的别名
`Get-Date` 获取当前日期和时间
`Get-Member` 获取对象的属性和方法
`Get-Random` 从集合中获取随机数或随机选择对象
`Get-Content` 获取指定位置项的内容
`Get-Service` 获取本地或远程计算机上的服务

**Write类**
`Write-Host` 将自定义输出内容写入主机。类似.net的write()功能
`Write-Progress` 在Windows Powershell命令窗口内显示进度栏
`Write-Debug` 将调试消息写入控制台
`Write-Verbose` 将文本写入详细消息流
`Write-Warning` 写入警告消息
`Write-Error` 将对象写入错误流
`Write-Output` 将指定对象发送到管道中的下一个命令；如果该命令是管道中的最后一个命令，则在控制台上显示这些对象
`Write-EventLog` 将事件写入事件日志

**Set类**
`Set-Alias` 在当前powershell会话中为cmdlet或其他命令元素创建或更改别名 如：`Set-Alias aaa Get-Command`
`Set-PSDebug` 打开和关闭脚本调式功能，设置跟踪级别并切换strict模式
`Set-StrictMode` 建立和强制执行表达式、脚本和脚本块中的编码规则
`Set-Date` 将计算机上的系统时间更改为指定的时间
`Set-Variable` 设置变量的值，如果该变量还不存在，则创建该变量
`Set-PSBreakpoint` 在行、命令或者变量上设置断点
`Set-Location` 将当前工作位置设置为指定位置
`Set-Item` 将项的值更改为命令中指定的值
`Set-Service` 启动、停止和挂起服务并更改服务的属性
`Set-Content` 在项中写入内容或用新内容替换其中的内容

**流程控制**
foreach的用法
```powershell
$var=1..6 #定义数组

foreach($i in $var){
    $n++
    Write-Host "$i"
}
```

if用法
```powershell
$var="powershell"

if($var -eq "powershell"){
    write-host "等于"
}else{
    write-host "不等于"
}
```


逻辑运算符
！不等于  -not 非  -and 且 -or 或

比较运算符
-eq 等于  -ne 不等于  -gt 大于  -ge 大于等于  -lt 小于  -le  小于等于

调用运算符
`::` 用于静态方法调用 如：`[DateTime]::Now #返回当前时间`

其他运算符
,数组构造函数   ..范围运算符