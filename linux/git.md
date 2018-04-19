## git 的日常使用

1. 设置git diff比较的文件
使用`--diff-filter = M`限制diff输出只有内容修改。
可以使用`git attributes` 来限制要比较的文件。创建一个.gitattributes文件，添加如下内容：
    ```
    build.css diff=nodiff
    build.js  diff=nodiff 
    ```
当我们运行git diff命令的时候，git就会读取.gitattributes的配置，然后发现遇到build.css和build.js文件的时候，需要执行nodiff指令。这里文件路径的配置和.gitignore是一样的，支持目录，文件，通配符之类的，可以轻松实现忽略一批文件或者整个目录。

## 配置
```
git config --list                              来查看我们的一些配置信息
git config --global --user.name=name           配置用户名
git config --global --user.email=name@xxx.xxx  配置用户邮箱
git config --global core.editor "vim"          配置编辑器
```

## 仓库
```
git init                          初始化仓库
git add -A                        添加所有文件到暂存区
git commit -a -m "msg"            -a 表示将所有修改或删除的文件添加到暂存区
git commit --amend                可以用来修改（自动打开vim让你修改）最近一次提交的注释信息，但是一旦你更新到远程仓库这个命令就不管用了
```

## 分支
```
git branch <-a>  <name>             查看当前分支，-a表示查看包括远程的所有分支，name存在则创建一个新的分支
git branch -D <-r> [name]           删除一个分支，-r代表远程的分支
git branch -u origin/master foo     设置远程分支与本地的foo分支对应
git branch -f master HEAD~3         强制移动分支，也可以理解为将HEAD~3命名为master即使是master已经存在
git checkout <-b> [name]            切换分支，-b 表示分支不存在的时候建立一个新的分支
git checkout HEAD~3^2               ~表示往上回溯几代，^用于在多个父提交中选择一个父提交
```

## 查看仓库信息
```
git log --all --decorate --graph --oneline   –decorate 会显示出tag信息，–graph 可以图形化地表示出分支合并历史， –oneline 每条log只显示一行
git status                                   用于显示工作区的当前状态
git show [commit]                            用来查看某次提交的具体情况
git tag version [commit]                       为 commit打上tag
```

## 分支合并
```
git merge buxfig            就是把bugfix的内容包含到当前的分支，相当当前分支作为默认参数放在后面，即  git merge bugfix master
git rebase C1 C2            就是拿出C2一系列新的的提交记录，”复制”它们，然后把它在C1后面新建一个提交放下来（HEAD->C1->C2），C2省略时默认为当前分支
git rebase -i dev           交互式合并
git cherry-pick C1 C2 C3    有目的的选择一些提交，并复制到你当前的位置 HEAD 下面。按照顺序选择性的合并一条分支。
```

## 回退分支
```
git reset            往回移动分支，只是撤销了提交而已,但是最近一次提交的修改仍然存在（干净的暂存区被污染了），对别人的远端分支是无效的
git revert           新建一个提交用来撤销最近一次提交的,最近一次提交的修改已经不存在（干净的工作区依旧干净）
```

## 远程分支
```
git fetch           从远端仓库获取数据
git pull            就是 fetch 和 merge 的简写
git pull –rebase    就是 fetch 和 rebase 的简写
```