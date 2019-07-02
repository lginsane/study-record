# GIT
> 一个开源的版本控制系统，主要用于合并文件、查看记录。（有后悔药的地方）

## 下载和配置
* [官方下载地址](https://git-scm.com/downloads)
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