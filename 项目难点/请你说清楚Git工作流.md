# Git 工作流

Git仓库的三个组成部分：**工作区**（Working Directory）、**暂存区**（Stage）、**历史记录区**（History）

* 工作区：在Git管理的正常目录都算是工作区，我们平时编辑工作都是在工作区完成。
* 暂存区：临时区域。里面存放将要提交的文件快照。
* 历史记录区：git commit 后的记录区。

![git工作分区](https://user-gold-cdn.xitu.io/2019/6/28/16b9e685a80e7072?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## Git 常见工作流

Git 主要的工作流有

* 集中式工作流（比较像SVN）
* 功能分支工作流
* Gitflow工作流（主要介绍这个）
* 分叉工作流

### Gitflow 分支工作流

有五种分支类型

* master 主分支，对外发布的版本
* dev 开发分支，日常开发，最新的版本
* feature 功能分支
* release 预发分支
* hotfix 补丁分支

一旦开发完成，将被合并进dev分支或者master并删除

![gitflow](../images/gitflow.png)

## Git 常用命令

> [来自于这位老哥](https://juejin.im/post/5c35b1f3e51d4503834d39b4)

* master： 默认开发分支
* origin：默认远程版本库
* Index/Stage：暂存区
* Workspace：工作区
* Repository：仓库区（或本地仓库）
* Remote：远程仓库

### merge操作

`git merge <name>` 省略了主体是当前的head

merge 前要保证暂存区内有待commit的内容，工作区有没提交的内容没有推送，不是必须的，但是保持干净吧。

merge会创建一个新的commit对象

### rebase 操作

### 新建仓库

```shell
git init  #在当前目录新建一个Git代码库
git init [project-name]  #当前目录新建一个目录，将其初始化为Git代码库
git clone [url]  #下载一个项目和它的整个代码历史
```

### 配置

```shell
git config --list  # 显示当前的Git配置
git config -e [--global]  # 显示Git配置文件
git config [--global] user.name "[name]"  # 设置提交代码时的用户名
git config [--global] user.email "[email address]"  # 设置提交代码时的用户邮箱
```

### 添加/删除/修改文件

```shell
git add [file1] [file2] ...  # 添加指定文件到暂存区
git add [dir]  # 添加指定目录到暂存区，包括子目录
git add .  # 添加当前目录的所有文件到暂存区
git add -p  # 添加每个变化前，都会要求确认，对于同一个文件的多处变化，可以实现分次提交
git rm [file1] [file2] ...  # 删除工作区文件，并且将这次删除放入暂存区
git rm --cached [file]  # 停止追踪指定文件，但该文件会保留在工作区
git mv [file-originname] [file-newname]  # 改名文件，并且将这个改名放入暂存区
```

### 代码提交

```shell
git commit -m [message]  # 提交暂存区到本地仓库
git commit [file1] [file2] ... -m [message]  # 提交暂存区指定文件到本地仓库
git commit -a  # 提交工作区自上次commit之后的变化，直接到本地仓库
git commit -v  # 提交时显示所有diff信息
git commit --amend -m [message]  # 使用一次新的commit，替代上一次提交，如果代码没有任何变化，则用来改写上一次commit的提交信息
git commit --amend [file1] [file2] ...  # 重做上一次commit，并包括指定文件的新变化
```

### 分支操作

```shell
git branch  # 显示所有本地分支
git branch -r  # 列出所有远程分支
git branch -a  # 列出所有本地分支和远程分支
git branch [branch-name]  # 新建一个分支，但head依然停留在当前分支
git branch --track [branch] [remote-branch]  # 新建一个分支，与指定的远程分支建立追踪关系
git branch -d [branch-name]  # 删除指定分支
git checkout -b [branch-name] # 新建一个分支，并切换head到该分支
git checkout [branch-name]  # 切换head到指定分支，并更新工作区
git checkout -  # 切换到上一个分支
git branch --set-upstream [branch] [remote-branch] # 建立追踪关系，在现有分支与指定的远程分支之间

git merge [branch]  # 合并指定分支到当前分支
git rebase <branch>  # 衍合指定分支到当前分支
git cherry-pick [commit]  # 选择一个commit，合并进当前分支
```

### 标签

```shell
git tag  # 列出所有本地标签
git tag <tagname>  # 基于最新提交创建标签
git tag -d <tagname>  # 删除标签
git push origin :refs/tags/[tagName]  # 删除远程tag
git show [tag]  # 查看tag信息
git push [remote] [tag]  # 提交指定tag
git push [remote] --tags  # 提交所有tag
git checkout -b [branch] [tag] # 新建一个分支，指向某个tag
```

### 查看信息

```shell
git status  # 查看当前工作区状态(与暂存区对比，增加删除或修改)
git log  # 显示当前分支的版本历史
git log --stat  # 显示commit历史，以及每次commit发生变更的文件
git log -S [keyword]  # 根据关键字搜索提交历史
git log [tag] HEAD --pretty=format:%s  # 显示某个commit之后的变动，每个commit占据一行。我记得--pretty=online也行
git log -p [file]  # 显示指定文件相关的每一个diff
git log -5 --pretty --oneline  # 显示过去5次提交
git shortlog -sn  # 显示所有提交过的用户，按提交次数排序
git blame [file]  # 显示指定文件是什么人在什么时间修改过，这个blame很生动形象
git diff  # 显示暂存区和工作区的差异
git diff --cached [file]  # 显示暂存区和上一个commit的差异
git diff HEAD  # 显示工作区与当前分支最新commit之间的差异
git diff [first-branch]...[second-branch]  # 显示两次提交之间的差异
git diff --shortstat "@{0 day agp}"  # 显示今天你写了多少航代码
git show [commit]  # 显示某次提交的元数据和内容变化
git show --name-only [commit]  # 显示某次提交发生变化的文件
git show [commit]:[filename]  # 显示某次提交时，某个文件的内容
git reflog  # 显示当前分支的最近几次提交
```

### 远程操作

```shell
git fetch [remote]  # 下载远程仓库的所有变动，注意这个时候是不会修改本地文件的
git pull [remote] [branch]  # 拉取远程仓库的变化，并与本地分支合并
git remote -v  # 显示所有远程仓库
git remote show [remote]  # 显示某个远程仓库的信息
git remote add [shortname] [url]  # 增加一个新的远程仓库，并命名
git push [remote] [branch]  # 上传本地指定分支到远程仓库
git push [remote] --force  # 强行推送当前分支到远程仓库，即使有冲突
git push [remote] --all  # 推送所有分支到远程仓库
git push <remote> :<branch/tag-name>  # 删除远程分支或标签
git push --tags  # 上传所有标签
```

### 比较

```js
/*工作区与暂存区之间的差别，即还没有添加到暂存区的修改，这里比较的是修改内容*/
git diff
/*暂存区与上一次提交的差别*/
git diff --cached
/*比较两次commit之间的差别*/
git diff <commit id1> <commit id2>
/*比较两个分支之间的差别*/
git diff <branch1> <branch2>
```

### 撤销

```shell
git reset --hrad HEAD  # 撤销工作目录中所有未提交文件的修改内容
git checkout HEAD <file>  # 撤销指定的未提交文件的修改内容
git revert <commit>  # 撤销指定的提交
git log --before="1 days"   # 退回到之前1天的版本
git checkout [file]  # 恢复暂存区的指定文件到工作区
git checkout [commit] [file]  # 恢复某个commit的指定文件到暂存区和工作区
git checkout .  # 恢复暂存区的所有文件到工作区
git reset [file]  # 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
git reset --hard  # 重置暂存区与工作区，与上一次commit保持一致
git reset [commit]  # 重置当前分支的指针未指定commit，同时重置暂存区，但工作区不变
git reset --hard [commit]  # 重置当前分支的HEAD未指定commit，同时重置暂存区和工作区，与指定commit一致
git reset --keep [commit]  # 重置当前HEAD未指定commit，但保持暂存区和工作区不变
```

## Git 面试常问

### 脑子抽了想要撤销之前的操作

情况一：文件 file 还在工作区，想要丢弃对这个文件所做的修改

`git checkout -- file`

情况二：文件 file 放入了暂存区，想要取消这个 add 操作

`git reset HEAD <file>`

情况三：文件 file 已经 commit，想要修改 commit

如果只是修改 commit message，直接用 `git commit --amend` 修改

如果需要新 add 文件 或者修改文件，先在工作区修改，之后 add 进入缓存区，最后通过 `git commit --amend` 与上一个错误的 commit 合并

### 怎么解决冲突

一般都是在 merge 的时候出错，别人已经更新了 dev 分支，而你合代码的时候有可能出现冲突。

解决冲突 -> add -> commit(merge)

是怎么发下冲突的？vscode 帮你做了什么？下面几条命令

```JS
git reset commit id  //取消暂存文件，将 HEAD 的指针指向 commit id，修改了暂存区域和历史记录库，工作区保持原样
git stash //把自己修改的代码隐藏
git pull //把远程仓库的代码拉下来
git stash pop //把自己隐藏的修改的代码释放出来，让 git 自动合并。接着找 <<<<<<<, 哪里冲突哪里改
```

### git add 和git stage有什么区别

git add 和git stage，其实这两个命令是同一个意思，

是因为要跟 svn add 区分，svn add 和 git add 的功能是完全不一样的，svn add 是将某个文件加入版本控制，而 git add 则是把某个文件加入暂存区，

### git fetch和git pull的区别

* git pull = fetch + merge
* 使用git fetch是取回所有的最新的远程分支更新，不会对本地执行merge操作，本地内容不会有变动；
* git pull会更新你本地代码变成服务器上对应分支的最新版本代码；

### git merge 和 git rebase 的区别

git rebase 和 git merge 都可以用于合并分支，从 feature 分支上，取得新的提交 commits, 然后运用到 master 分支上（当然运用到其他分支上也行）。但是路子不同，merge 是合并，rebase 是变基

git rebase 的 commit id 修改了

### git reset、git revert 和 git checkout 有什么区别

* 共同点：用来撤销代码仓库中的某些更改。
* 不同点

从commit层面来讲

* git reset可以将一个分支的末端指向前一个commit。然后再下次git执行垃圾回收的时候，会把这个commit之后的commit都扔掉。
* git reset还支持三种标记。用来标记reset指令的影响范围。
  * --mixed：会影响到暂存区和历史记录区。也是默认选项。
  * --soft：只影响历史记录区，缓存区和工作目录都不会被改变
  * --hard：影响工作区，暂存区和历史记录区。

> 注意，因为git reset是直接删除commit记录，从而会影响其他开发人员的分支，所以不要在公共分支做这个操作。

* git checkout 可以将HEAD移到一个新的分支，并更新工作目录。以为可能会覆盖本地的修改，所以执行这个指令之前，你需要 stash 或者commit暂存区和工作区的更改。
* git revert和git reset的目的是一样的，但是做法不一样，它会创建新的commit的方式来撤销commit，这样能保留之前的 commit 历史，比较安全。另外，同样因为可能会覆盖本地的修改，所以执行这个指令之前，你需要 stash 或者 commit 暂存区和工作区的更改。

然后从**文件**的层面来说：

* git reset 只是把文件从**历史记录区拿到暂存区**，不影响工作区的内容，而且不支持 --mixed、--soft 和 --hard。
* git checkout 则是把文件从**历史记录拿到工作**区，不影响暂存区的内容。
* git revert不支持文件层面的操作。

说明：

`git reset commit id` 就是 `git reset -mixed commit id`，移动 HEAD，更新索引，即更新暂存区。移动 HEAD 分支的指向，使索引看起来像 HEAD。效果上看，就是取消了 commit id 以后的，add 和 commit .

`git reset --soft commit id`，就是移动 HEAD。移动 HEAD 分支的指向，本质上是撤销了上一次 git commit 命令。

当你在运行 git commit 时，Git 会创建一个新的提交，并移动 HEAD 所指向的分支来使其指向该提交。
当你将它 reset 回 HEAD~（HEAD 的父结点）时，其实就是把该分支移动回原来的位置，而不会改变索引和工作目录。

`git reset --hard commit id`, 移动 HEAD，更新索引，更新工作目录。三件事情，全做了。前两件事情，已经说了。更新工作目录，让工作目录看起来像索引。从效果上看，就是撤销一切修改，本地文件状态同 commit id 的那时候。

## Git 权限管理

Git 中的五种角色：

* Owner     Git 系统管理员
* Master    Git 项目管理员
* Developer    Git 项目开发人员
* Reporter     Git 项目测试人员
* Guest     访客
