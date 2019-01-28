# Gitea安装部署
**Gitea 是一个开源社区驱动的 Gogs 克隆, 是一个轻量级的代码托管解决方案，后端采用 Go 编写，采用 MIT 许可证.** <br/>

官网：https://gitea.io/zh-cn/

!> 安装前需要git，如果使用ssh，则还要安装OpenSSH

使用ssh操作目前遇到一个问题，就是在本地电脑上使用`ssh-keygen -t rsa -C "youremail"`生成的密钥添加到仓库中一直无法使用，最后使用安装gitea，并启动内部ssh服务功能生成的密钥才成功。
具体操作：
1. 在安装部署gitea服务端的目中`gitea\data\ssh`文件夹下（根据实际安装目录）下会生成一对密钥，然后拷贝到本地操作电脑的`C:\Users\Tozii\.ssh`文件夹下
2. 将拷贝下来的公钥添加到gitea账户ssh中

**配置邮件服务：密码参数PASSWD使用授权码，HOST：smtp.qq.com:465**

