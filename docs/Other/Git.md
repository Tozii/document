# Git基本操作

2020-12-24 更新

参考：https://www.cnblogs.com/tinywan/p/12344267.html

Git社区发布了Git的新版本2.23。在该版本中，有一个特性非常引人瞩目，就是新版本的Git引入了两个新命令 git switch 和 git restore，用以替代现在的 git checkout。换言之，git checkout 将逐渐退出历史舞台

Git社区决定这样做，是因为目前 git checkout 命令承载了太多的功能，这让新手们感到困惑。git checkout 的核心功能包括两个方面，一个是分支的管理，一个是文件的恢复。这两个核心功能，未来将由 git switch 和 git restore 分别负责。

---

参考：[Git教程 — 廖雪峰](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

退出git当前操作：`q`

创建SSH key：`ssh-keygen -t rsa -C "your_email@youremail.com"`

初始化仓库：`git init`

克隆远程仓库代码：`git clone [address]` 例：`git clone git@github.com:Tozii/document.git`

查看远程仓库信息列表：`git remote -v`

添加远程仓库：`git remote add [remotename] [address]`例：`git remote add origin git@github.com:Tozii/document.git`

删除远程仓库：`git remote rm [remotename]`例：`git remote rm origin`

拉取远程仓库最新代码：`git pull`或`git pull [origin] [master]`

拉取远程分支并合并到本地当前分支上（强制）：`git pull --rebase origin master`

将两个不相干的git库强制合并：`git pull origin master --allow-unrelated-histories`

添加忽略配置文件(git bash)：`touch .gitignore`

检测当前文件状态 : `git status`

创建文件夹(git bash)：`mkdir filename`

添加忽略配置文件(cmd)：`type nul> .gitignore`

创建文件夹(cmd)：`mkdir filename` 或 `md filename`

撤销全部未提交更改（使用暂存区文件替换工作区文件）：`git checkout .`   
当执行 git checkout HEAD . 或者 git checkout HEAD <file> 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

添加本地所有更改到暂存区：`git add -A`

添加除删除外更改到暂存区： `git add .`

添加删除...文件到暂存区：`git add -u`

提交代码到本地仓库：`git commit -m "更改信息"`

推送到远程仓库：`git push [remotename][branchname]`例：`git push origin master`

版本差异比较(暂存区和工作区的差异)：`git diff head -- [filename]`
例子：假设仓库里已提交的有五个版本，依次提交的是A、B、C、D、E 。
执行 `git diff <filename>` 命令，命令行窗口不会输出文件的改动信息。

执行 `git diff HEAD -- <filename>`  命令同样也不会输出文件的改动信息。因为当前工作区未做改动。

执行 `git diff HEAD^ -- <filename>`  命令则可以查看最近两次提交版本的区别（版本E和版本D的差别——增加数字“5”）

执行 `git diff HEAD^^ -- <filename>` 命令则可以查看最近一次提交和最近一次提交的上上个版本的区别（版本E和版本C的差别——增加数字“4”和“5”）

执行 `git diff HEAD^^^ -- <filename>`  命令则可以查看版本E和版本B的差别——增加数字“3”，“4”和“5”。

执行 `git diff HEAD~4 -- <filename>`  命令则可以查看版本E和版本A的差别——增加数字“2”，“3”，“4”和“5”。等同于`HEAD^^^^`

修改最后一次提交备注信息（未push到远程仓库）：`git commit --amend` 执行命令后进入vim编辑器，此时处于不可编辑状态，按下字母`c`，进入编辑状态修改信息，
修改完成后按`Esc`退出编辑状态，然后连按两次大写`Z`保存并退出

查看当前项目所有分支：`git branch` 带星号的为当前分支

切换分支：`git checkout [branchname]`

创建分支并切换到新分支：`git checkout -b [branchname]`

创建分支：`git branch [branchname]`

合并指定分支到当前分支：`git merge [branchname]`

删除本地分支：`git branch -d [branchname]`

删除远程仓库分支：`git push origin :[branchname]`

查看历史：`git log` 退出使用英文 `q`

回退上一个版本(撤销暂存区修改)：`git reset --hard HEAD^`   
当执行 git reset HEAD 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。

查看操作记录：`git reflog`

查看某次操作变更：`git show [提交编码]` 例子: `git show 8aaafd0`

查看指定文件git使用`cat index.html`windows使用`type index.html`

更新远程仓库地址：`git remote set-url [remotename] [address]` 例 `git remote set-url origin https://gitee.com/Tozii/Simple.Crypto.git`

修改仓库名：`git remote rename origin oschina`

关联远程分支：`git push --set-upstream origin master` 这样以后就可以使用`git push` `git pull`，默认推送和拉取`origin`远程仓库的`master`分支 

### 在未获取远程更新情况下，推送本地分支更改到远程分支报错（non-fast-forward）
1. `git fetch [originname] [branchname]` 获取远程指定分支修改到本地仓库
2. `git merge [originname] [branchname]` 合并远程分支

完成上面步骤后，解决本地`[branchname]`分支冲突，然后提交就可以了
