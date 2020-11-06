## git命令
![常用命令图](https://upload-images.jianshu.io/upload_images/18087435-bf2a996ef50a21b0.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1000/format/webp)

### reset
常用参数：
```
// 暂存区和HEAD的提交保持一致
git reset HEAD // 回滚未推送到远程的commit，并且清空暂存区的内容到工作区
git reset HEAD~2 // 回滚未推到远程的commit最新的两个到工作区，并且清空暂存区的内容到工作区
```
```
git reset --hard HEAD // 工作区、暂存区和HEAD保持一致 HEAD可以是提交sha
git reset --soft HEAD  // 回滚历史，并且将回滚的更改放到暂存区
```

详细描述：
```
 --soft 回退后a分支修改的代码被保留并标记为add的状态（git status 是绿色的状态）。<br/>
 --mixed 重置索引，但不重置工作树，更改后的文件标记为未提交（add）的状态。默认操作。。<br/>
 --hard 重置索引和工作树，并且a分支修改的所有文件和中间的提交，没提交的代码都被丢弃了。<br/>
 --merge 和--hard类似，只不过如果在执行reset命令之前你有改动一些文件并且未提交，merge会保留你的这些修改，hard则不会。【注：如果你的这些修改add过或commit过，merge和hard都将删除你的提交】<br/>
 --keep 和--hard类似，执行reset之前改动文件如果是a分支修改了的，会提示你修改了相同的文件，不能合并。如果不是a分支修改的文件，会移除缓存区。git status还是可以看到保持了这些修改。
```

### rebase （合并衍合变基）
```rebase``` 即变基，顾名思义，就是改变基准点，以 commit 为基准点可以随意修改 ```commit```
 历史，以分支为基准点可以合并分支，同时整理 ```commit```。
#### rebase合并分支
```
git rebase dev
```
历史记录回滚到最后一个公共父节点，如果当前节点和目标节点中间还有节点，则会复制中间所有节点内容，会把当前节点指向dev最新的节点。

一般我们从develop拉一个分支出来，但是develop更新的时候，新的分支又要和develop保持一致，这个时候，不要又从develop直接合过来，我们可以用rebase,直接将当前分支移到develop最新节点上，这个看下面参考链接2。

（参考链接：https://juejin.im/post/6844903729209016327#heading-10
https://juejin.im/post/6844903593015787534#heading-9）

>注意： 多人开发，已经push到远端的代码，请不要使用rebase合并提交，因为rebase会改变提交线，极易造成冲突的产生。
#### 拉取最新代码
```rebase``` 也可以在本地分支push到服务器仓库前进行```git rebase origin (branch)```保证拉取最新代码。（有时候要慎用，情况及解决方案见底部备注1）


#### rebase合并commit
```
git rebase -i HEAD~3
```
>git rebase命令的i参数表示互动（interactive），这时git会打开一个互动界面，进行下一步操作。

常见合并commit命令：
- pick：正常选中
- reword：选中，并且修改提交信息；
- edit：选中，rebase时会暂停，允许你修改这个commit（参考这里）
- squash：选中，会将当前commit与上一个commit合并
- fixup：与squash相同，但不会保存当前commit的提交信息
- exec：执行其他shell命令

### revert （撤销）
新建一个commit，用来撤销指定commit
后者的所有变化都将被前者抵消，并且应用到当前分支

- 对于单一 parent 的 commit，直接使用 git revert commit_id;
- 对于具有多个 parent 的 commit，需要结合 -m 属性：git revert **commit_id** -m **parent_id**;
- 对于从 branch 合并到 master 的 merge commit，master 的 parent_id 是1，branch 的 parent_id 是2, 反之亦然;（1,2的目的是决定哪个分支会被保留下来，如果当前分支是develop，合并过来的分支是feature，那么参数为1，则会保留当前的develop分支，如果是2，则会保留feature分支）

这些都是丢弃某次提交的操作
但是，有时候我们想把丢弃的提交在找回来，我们就可以用git revert把之前的revert丢弃掉，就相当于找回之前的提交。

### stash （开发暂存）
暂时将未提交的变化移除，稍后再移入
```
git stash // 当前修改代码存入栈中，你的栈里将充满了未提交的代码

git stash save "work in progress for foo feature" // 增加stash日志信息
git stash list // 将当前的Git栈信息打印出来，你只需要将找到对应的版本号
git stash apply stash@{1} // 将你指定版本号为stash@{1}的工作取出来
git stash pop // 将最后一次stash工作区提取出来
git stash clear // 来将栈清空
```
## 常见问题
##### 1. git rebase后出现(xxx|REBASE-i)的解决办法
```
git rebase --abort // 代码回退  回到git rebase之前的状态
```

##### 2. git在提交代码时需要先和远程代码进行同步

```
git fetch origin
$ git rebase origin/master
```

##### 3. --no-ff作用
// 对Develop分支进行合并
```
git merge --no-ff develop
```
作用，提供清晰合并记录，加上这个就是合并会以新的提交记录出现，不加则是一条提交记录线，不够清晰（http://www.ruanyifeng.com/blog/2012/07/git.html）
##### 4. 撤销git rebase
使用git reflog + git reset --hard
```
git reflog // 获取更改列表
git reset --hard HEAD@{23}
```
## 一些规范
##### 禁止反向拉取 develop 分支
使用rebase让分支移到最新代码上
##### 不经过 Pull Request 的合并
Pull Request 可以进行简单的codeReview
##### 重复使用已经合并的分支
移除已完成的分支，让提交记录更清晰
##### 没有意义的 Commit Message
commit 没有任何意义
## 备注

### 备注1 git rebase origin/master 可能遇到的坑

git fetch origin dev && git rebase origin/dev这种操作在大部分情况下是没问题的，但这里至少有两个坑。第一个坑比较常见，有些仓库里remote.origin.fetch由于某些原因没有配置，或者配置的值是错的，或者配置的值不够通用，导致git fetch origin dev并不会更新origin/dev，因而rebase的时候origin/dev指向的commit并不是预期的commit。另一个问题比较少见，那就是在远程仓库里可能存在好几个不同的dev，譬如dev分支，dev tag，甚至其它叫dev的ref。那这里仅仅写dev的话，可能拉取到的并不是refs/heads/dev，而是譬如refs/comments/dev，那也会导致origin/dev不会更新。

针对第一个坑，当你的操作依赖origin/dev这种remote tracking branch时，务必确定remote.origin.fetch的值是对的。克隆生成的仓库里，它的值会被自动配置为
> +refs/heads/*:refs/remotes/origin/*。git init创建的仓库里这个需要手动添加git config remote.origin.fetch +refs/heads/*:refs/remotes/origin/*

另一个比较方便的做法是不要用origin/dev，改用FETCH_HEAD。FETCH_HEAD里面存储的是拉取ref当前指向的commit，即使remote.origin.fetch没有配置也可以按预期工作。当一次拉取多个ref时，FETCH_HEAD里存的是排序以后排第一位的ref的commit，所以一次拉取多个的话，还是用origin/dev这样的更可靠。

FETCH_HEAD无法应付第二个坑。针对第二个坑，在fetch或者pull时，使用ref的全名可破。譬如拉分支dev，就写refs/heads/dev，拉tag dev，就写refs/tags/dev等等。这样就可以避免歧义了。日常提交代码时很少遇到这种情况，可以还是写dev比较省事，因为出问题了大概率当场就会发现。但在自动化脚本里，譬如构建脚本里，尽量用全称避免潜在的问题，否则默默跑了一段时间才发现问题就坑大了。

参考链接：

Git分支管理策略：
http://www.ruanyifeng.com/blog/2012/07/git.html

Git 工作流程：
http://www.ruanyifeng.com/blog/2015/12/git-workflow.html

简介我的 Git Work Flow：
https://juejin.im/post/6844903593015787534