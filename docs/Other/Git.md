# Git基本操作

退出git当前操作：`q`

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

版本差异比较：`git diff head -- [filename]`
例子：假设仓库里已提交的有五个版本，依次提交的是A、B、C、D、E 。
执行 `git diff <filename>` 命令，命令行窗口不会输出文件的改动信息。

执行 `git diff HEAD -- <filename>`  命令同样也不会输出文件的改动信息。因为当前工作区未做改动。

执行 `git diff HEAD^ -- <filename>`  命令则可以查看最近两次提交版本的区别（版本E和版本D的差别——增加数字“5”）

执行 `git diff HEAD^^ -- <filename>` 命令则可以查看最近一次提交和最近一次提交的上上个版本的区别（版本E和版本C的差别——增加数字“4”和“5”）

执行 `git diff HEAD^^^ -- <filename>`  命令则可以查看版本E和版本B的差别——增加数字“3”，“4”和“5”。

执行 `git diff HEAD~4 -- <filename>`  命令则可以查看版本E和版本A的差别——增加数字“2”，“3”，“4”和“5”。等同于HEAD^^^^

修改最后一次提交备注信息（未push到远程仓库）：`git commit --amend` 执行命令后进入vim编辑器，此时处于不可编辑状态，按下字母`c`，进入编辑状态修改信息，
修改完成后按`Esc`退出编辑状态，然后连按两次大写`Z`保存并退出