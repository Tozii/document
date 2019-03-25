# NuGet基本操作

执行打包命令：`nuget spec` 会生成 project.nuspec 文件，该文件为NuGet描述文件，可以自行修改描述信息

生成上传包文件：`nuget pack project.csproj` 会生成 project.1.0.0.nupkg 文件，该文件则是要上传到NuGet包管理服务器的文件

上传包：`nuget push project.1.0.0.nupkg -Source https://www.nuget.org/api/v2/package {apikey}` 上传包到包管理服务站点，`-Source` 要上传到的服务站点，可以是自己搭建的NuGet站点 `{apikey}` 则是注册NuGet网站生成的apikey

设置全局apikey：`nuget setApiKey {apikey} -Source https://www.nuget.org/api/v2/package`

**在iis搭建nuget server时遇到405 method not allow**

参考：https://www.cnblogs.com/czcz1024/p/3490338.html

https://zhidao.baidu.com/question/1304591714345278219.html

产生这个错误是由于IIS安装了WebDAV模块
```
<configuration>
  <system.webServer>
    <validation validateIntegratedModeConfiguration="false" />
    <modules runAllManagedModulesForAllRequests="true">
      <remove name="WebDAVModule" />
    </modules>
```
nuget搭建密钥配置

如果`<add key="requireApiKey" value="true" />` 为true,则必须配置`<add key="apiKey" value="nugetapikey" />`
上传包时使用apiKey才能上传