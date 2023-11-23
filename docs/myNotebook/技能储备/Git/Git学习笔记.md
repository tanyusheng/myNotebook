> 关于Git学习推荐一个网站:[学习Git分支](https://learngitbranching.js.org/?locale=zh_CN)

## Git 基础

### 初始化

查看Git版本`git -v`

设置Git用户名：`git config --global user.name "xiaoyu"`

设置Git邮箱：`git config --global user.email"xiaoxiaoyu@outlook.com"`

查看Git配置信息：`git congfig --list`

### Git仓库

把本地文件夹转化为Git仓库：`git init`

Git 使用时的三个区域：

* 工作区：实际开发时操作的文件夹
* 暂存区：暂存改动过的文件
* 版本库：提交并保存暂存区的内容，产生一个本部快照，位于.git/objects文件夹中

查看暂存区的文件内容可以使用命令`git ls-files`命令；

将暂存区的文件恢复至工作区可以使用命令`git restore .`

如果不想让某个文件在暂存区里被跟踪了，可以使用命令`git rm --cached 文件标识名`

查看版本库的代码版本可以使用命令`git log --oneline`,如果要查看所有版本记录则使用命令`git reflog --oneline`

### Git回退版本

Git可以将版本库里的快照进行回退，有三种模式，`soft、hard、mixed`,

```shell
git reset --soft 版本号 #工作区、暂存区工作文件均保留
git reset --hard 版本号 #工作区、暂存区工作文件均被恢复
git reset --mixed 版本号 # 工作区文件保留、暂存区文件被恢复，与git reset等价
```

回退到上一个版本可以使用`HEAD^`

### Git 删除文件

在工作区删除了某个文件，暂存区依然存在，如果要真正删除则需要再执行一次`git add .`或者使用`git rm --cached 文件标识名`的方式将文件在暂存区中删除掉；

### Git忽略文件

项目根目录创建一个`.gitignore`文件可以让Git忽略跟踪指定的文件；

文件夹名字可以直接写

## Git分支

### 切分支

初始版本C0,变成新的版本C1，使用git commit命令；
创建一个新的分支：`git branch 分支名`

切换到新的分支：`git checkout 分支名`，切分支会直接影响工作区和暂存区的文件内容；

创建并切换到新的分支：`git branch -b 分支名`

查看当前存在哪些分支：`git branch`

### 合并分支

将其他分支合并到当前分支：`git merge 其他分支名`；

如果要合并分支，先切换到主分支上来，通过merge操作将其它分支合并到主分支上。

合并后原分支如果需要被删除则可以使用命令`git branch -d 分支名`

合并分支有两种方法：`git merge`和`git rebase`
相对引用：使用哈希值指定提交记录很不方便，git引入了相对引用，使用`^`向上移动一个提交记录；
使用`~<num>`向上移动多个提交记录，如`~3`;
`main^`相当于`main`的父节点，切换到main的父节点可以使用命令`git checkout main^`
强制修改分支位置：`git branch main HEAD~3`,该命令可以将main分支强制指向HEAD的第3级父提交；

### 合并冲突

如果不同分支对同一个文件的同一部分做了不同的修改，那么合并分支时就会产生冲突；需要手动解决冲突后再提交一次； 

### 撤销变更

主要有两种方法来撤销变更：`git reset`和`git revert`
`git reset HEAD~1`将分支回退到上一个版本，该方法对远程分支无效；
`git revert HEAD`虽然是撤销修改记录，但其实是在原有修改的基础上再进行修改，与最近的一次修改抵消从而达到上一次的状态。也就是说，revert之后就可以把更改推送到远程仓库与别人分享了。
### Cherry-pick
`git cheery-pick 哈希值C1 哈希值C2 哈希值C3`直接将C1、C2、C3的修改拿到当前分支下；
如果不知道提交的哈希值，就需要采用交互式rebase的方式来完成，使用参数`--interactive`的rebase命令，简写为`-i`
`git rebase -i HEAD~4`
cherry-pick可以将提交树上的任何地方的提交记录取过来追加到HEAD上，只要不是HEAD上游的提交就没问题。
### 标签
建立一个标签V1指向C1
`git tag v1 c1`
### 远程分支
在使用`git clone`命令后，本地仓库会多一个远程分支。远程分支反映了远程仓库在你上次和他通信时的状态。远程分支有一个特殊属性，在切换到远程分支时，会自动进入分离HEAD状态。Git这么做是出于不能直接在这些分支上进行操作的原因，你必须在别的地方完成你的工作。
远程分支的命名规范是`<remote name>/<branch name>`,大部分开发人员将远程仓库命名为`origin`。
如果切换到远程分支，Git会变成分离HEAD状态，当添加新的提交时，远程分支也不会更新。这是因为远程分支只有在远程仓库中相应的分值更新了以后才会更新。
### 远程仓库
注册第三方托管平台网站，新建仓库得到远程仓库Git地址，然后在本地仓库添加远程仓库地址

```shell
git remote add 远程仓库名 远程仓库地址
```

本地仓库推送版本记录到远程仓库

```shell
git push -u 远程仓库名 本地和远程分支名
# 常规写法
git push -u origin main
# 完整写法
git push --set-upstream origin main:main
```

一般要求在git push之前，先将远程仓库的内容拉到本地，预防远程仓库已经有了部分更新；

如果远程仓库与本地有不同的文件内容，强行的合并则可以使用

```shell
git pull --rebase origin main
```

从远程仓库获取数据`git fetch`;
`git fetch`做了什么：

* 从远程仓库下载本地仓库中缺失的提交记录；
* 跟新远程分支指针。
`git fetch`不会改变本地仓库的状态，也不会跟新`main`分支，不会修改磁盘上的文件。
`git pull`是指从远程仓库获取最新的提交并合并到本地分支中，相当于执行了`git fetch`和`git merge`两个命令。

### git stash

* 存（入栈）

`git stash`

`git stash save '注释'`

* 取（出栈）

`git stash pop`

`git stash apply`（不出栈，类似于peek）

* 清除

`git stash drop`

`git stash clear`

* 查看

 `git stash list`

`git stash show + 栈索引`