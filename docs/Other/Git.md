# Git基本操作

创建SSH key：`ssh-keygen -t rsa -C "your_email@youremail.com"`

初始化仓库：`git init`

克隆远程仓库代码：`git clone [address]` 例：`git clone git@github.com:Tozii/document.git`

查看远程仓库信息列表：`git remote -v`

添加远程仓库：`git remote add [remotename] [address]`例：`git remote add origin git@github.com:Tozii/document.git `

删除远程仓库：`git remote rm [remotename]`例：`git remote rm origin`

拉取远程仓库最新代码：`git pull`或`git pull [origin] [master]`

撤销全部未提交更改：`git checkout .`

添加本地所有更改到暂存区：`git add -A`

添加除删除外更改到暂存区： `git add .`

添加删除...文件到暂存区：`git add -u`

提交代码到本地仓库：`git commit -m "更改信息"`

推送到远程仓库：`git push [remotename][branchname]  `例：`git push origin master`







