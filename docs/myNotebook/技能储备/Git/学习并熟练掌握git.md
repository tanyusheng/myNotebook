关于Git学习推荐一个网站:[学习Git分支](https://learngitbranching.js.org/?locale=zh_CN)

初始版本C0,变成新的版本C1，使用git commit命令；
创建一个新的分支：`git branch 分支名`
切换到新的分支：`git checkout 分支名`
创建并切换到新的分支：`git branch -b 分支名`
将其他分支合并到当前分支：`git merge 其他分支名`
合并分支有两种方法：`git merge`和`git rebase`
相对引用：使用哈希值指定提交记录很不方便，git引入了相对引用，使用`^`向上移动一个提交记录；
使用`~<num>`向上移动多个提交记录，如`~3`;
`main^`相当于`main`的父节点，切换到main的父节点可以使用命令`git checkout main^`
强制修改分支位置：`git branch main HEAD~3`,该命令可以将main分支强制指向HEAD的第3级父提交；

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
从远程仓库获取数据`git fetch`;
`git fetch`做了什么：
* 从远程仓库下载本地仓库中缺失的提交记录；
* 跟新远程分支指针。
`git fetch`不会改变本地仓库的状态，也不会跟新`main`分支，不会修改磁盘上的文件。
`git pull`是指从远程仓库获取最新的提交并合并到本地分支中，相当于执行了`git fetch`和`git merge`两个命令。