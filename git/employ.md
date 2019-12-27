# GIT
> 一个开源的版本控制系统，主要用于合并文件、查看记录。（有后悔药的地方）

## 下载和配置
* [官方下载地址](https://git-scm.com/downloads)
* [官方使用文档](https://git-scm.com/book/zh/v2)
```配置名称、邮箱
$ git config --global user.name "你的名称"
$ git config --global user.email 你的邮箱
```

## 初始化和帮助
```初始化
$ git init
```
```帮助
$ git help
```

## 流程步骤
`git add .`（所有文件）或者`git add 文件路径`(单个文件)将文件添加到暂存区 <br/> 
`git commit -m "说明"`将文件从暂存区提交到本地仓库，`"说明"`为本次提交的文件说明 <br/> 
`git pull 远程仓库 分支名`拉取远程仓库文件，防止冲突 <br/>
`git push 远程仓库 分支名`将本地仓库文件上传到远程仓库
```
$ git add .
$ git commit -m "说明"
$ git pull 远程仓库 分支名
$ git push 远程仓库 分支名
```

## 克隆远程仓库和添加远程仓库
`origin`为本地给远程仓库创建的快捷名称，可以随便命名。添加过远程仓库后可以直接使用`git pull origin 分支名`和`git push origin 分支名`来拉取和上传文件
```克隆远程仓库
$ git clone 远程仓库
```
```添加远程仓库
$ git remote add origin 远程仓库
```

## 分支操作
`git checkout 分支名`切换分支<br/>
`git branch 分支名`创建分支<br/>
`git branch -d 分支名`删除分支<br/>
`git merge 分支名`将指定分支合并到当前分支<br/>
`git cherry-pick 版本号`将当前仓库的指定commit合并到当前分支
* 注：当合并分支出现冲突时，请手动修改冲突
```（简写）创建并切换分支
$ git checkout -b 分支名
```

## 版本回退(后悔药)
`git log`查看提交日志<br/>
`git reset --hard HEAD^`回退到上一个版本<br/>
`git reset --hard 版本号`回退固定版本<br/>
`git status`查看提交状态<br/>
`git status -s`查看简洁的提交状态<br/>
`git diff`查看暂存区与工作区的区别<br/>
`git checkout -- <file>`撤销文件，`<file>`为文件名