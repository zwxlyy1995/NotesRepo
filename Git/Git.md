# Git 自学笔记

***

- [Git 自学笔记](#git-自学笔记)
  - [说明](#说明)
  - [1.常识](#1常识)
    - [1.1 创建版本库](#11-创建版本库)
    - [1.2 文件添加](#12-文件添加)
  - [2.基本操作](#2基本操作)
    - [2.1 版本回退](#21-版本回退)
    - [2.3 管理修改](#23-管理修改)
    - [2.4 撤销修改](#24-撤销修改)
    - [2.5 删除文件](#25-删除文件)
  - [3.远程仓库](#3远程仓库)
    - [3.1 添加远程仓库](#31-添加远程仓库)
    - [3.2 从远程库克隆](#32-从远程库克隆)
  - [4.分支管理](#4分支管理)
    - [4.1 创建与合并分支](#41-创建与合并分支)
    - [4.2 解决冲突](#42-解决冲突)
    - [4.3 分支管理策略](#43-分支管理策略)
    - [4.4 Bug分支](#44-bug分支)
      - [4.4.1 工作区的保存与取出](#441-工作区的保存与取出)
      - [4.4.2 对于bug的处理](#442-对于bug的处理)
    - [4.5 多人协作](#45-多人协作)
  - [5.标签](#5标签)
    - [5.1 创建标签](#51-创建标签)
    - [5.2 操作标签](#52-操作标签)
  - [6.Github & Gitee](#6github--gitee)
  - [7.自定义Git](#7自定义git)
    - [7.1 忽略文件](#71-忽略文件)
    - [7.2 配置别名](#72-配置别名)

***

## 说明

本文为记录学习git所作，非原创，内容不完全，待完善。

***

## 1.常识

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

### 1.1 创建版本库

1. 创建一个空目录
```
$ mkdir 文件夹名称
$ cd 文件夹名称
$ pwd
```
2. 初始化
```
$ git init
```
产生的`.git`，跟踪版本库，不能删除。

### 1.2 文件添加

* 所有的版本控制系统只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等；而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。
* Microsoft的Word格式是二进制格式，因此，版本控制系统是没法跟踪Word文件的改动的
* 不要使用Windows自带的记事本编辑任何文本文件。原因是Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”。
  
1. 使用`git add <File>`添加文件，可以多次添加，添加多个文件。
2. 使用`git commit -m <Message>`提交。

* **Git Add**
  1. `git add`不加参数默认为将修改操作的文件和未跟踪新添加的文件添加到git系统的暂存区，注意不包括删除
  2. `git add -u`-u 表示将已跟踪文件中的修改和删除的文件添加到暂存区，不包括新增加的文件，注意这些被删除的文件被加入到暂存区再被提交并推送到服务器的版本库之后这个文件就会从git系统中消失了。
  3. `git add -A`-A 表示将所有的已跟踪的文件的修改与删除和新增的未跟踪的文件都添加到暂存区。

* **Git Commit**
  1. `git commit -m "..."`
  2. `git commit -am "..."`-am等同于-a -m ；-a参数可以将所有已跟踪文件中的执行修改或删除操作的文件都提交到本地仓库，即使它们没有经过git add添加到暂存区，注意: 新加的文件（即没有被git系统管理的文件）是不能被提交到本地仓库的。

* **Git Push**
  1. `git push origin master`远程分支被省略。
  2. `git push origin : refs/for/master`本地分支被省略，等同于推送一个本地空分支到远程分支，相当于`git push origin –delete master`
  3. `git push origin`如果当前分支与远程分支存在追踪关系，则本地分支和远程分支都可以省略，将当前分支推送到origin主机的对应分支。
  4. `git push`如果当前分支只有一个远程分支，那么主机名都可以省略，形如 `git push`，可以使用`git branch -r` ，查看远程的分支名。
  5. `refs/for` 的意义在于我们提交代码到服务器之后是需要经过code review 之后才能进行merge的，而`refs/heads` 不需要。

***

## 2.基本操作

`git status`命令可以让我们时刻掌握仓库当前的状态。
`git diff <File>`命令可以查看文件具体的改动。

### 2.1 版本回退

* `git log`可以查看从最近到最远的commit日志。
* `git reset --hard 版本号`，在git中使用`HEAD`代表当前版本，`HEAD^`代表上个版本，`HEAD^^`代表上上个版本，以此类推；或者直接写commit id。
* `git reflog`可以提供每一次命令的记录，从里面查找因为错误回退版本丢失的commit版本号。

###2.2工作区和版本库

* **Working Directory** 工作区就是工作项目的目录。
* **Repository** 版本库就是工作区中的.git目录。
  
  版本最重要的是被称为`stage/index`的暂存区；Git自动创建的第一个分支`master/main`；以及指向`master/main`的指针`HEAD`。
  `git add`本质上就是把文件修改添加到暂存区
  `git commit`本质上就是把暂存区的内容提交到当前分支。

### 2.3 管理修改

每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。

### 2.4 撤销修改

`git checkout -- <File>`回到最近一次`git add`或`git commit`时的状态：
* 一种是文件修改后还没有使用`git add`添加到暂存区，使用此命令回到和版本库一摸一样的状态；
* 一种是文件修改了，并且`git add`添加到暂存区，但是又做出了修改，使用此命令回到添加暂存区后大状态。
* 命令中的`--`很重要，没有则意味着切换到其他分支。

`git reset HEAD <File>`可以撤销(**unstage**)暂存区的修改，`HEAD`表示当前版本。

### 2.5 删除文件

* `git rm <File>`在版本库中删除文件，然后使用`git commit`。
* 对于误删文件，使用`git checkout -- <File>`找回，前提是该文件被添加到版本库。

***

## 3.远程仓库

* 创建SSH KEY
```
$ ssh-keygen -t rsa -C "邮箱"
```
* 向Github里面添加`id_rsa.pub`的公钥，保管好`id_rsa`中的私钥，不能泄露。

### 3.1 添加远程仓库

* 关联远程库
```
$ git remote add origin git@github.com:用户名/仓库名.git
```
 远程库名字叫做`origin`，这是Git默认做法，也可以改成其他名字。

* 建立追踪关系
 1. 手动建立`$ git branch --set-upstream-to=origin/远程分支 本地分支`
 2. **push**时建立`git push -u origin 本地分支`
 3. 新建分支时建立`git checkout -b 本地分支 origin/远程分支`
   
查看追踪关系：
```
$ git branch -vv
```

* 推送前要拉取(**Pull**)远程库内的内容
  现在Github默认分支`master`变成了`main`
```
$ git pull origin master [--allow-unrelated-histories]
$ git pull origin main [--allow-unrelated-histories]
```

* 推送(**Push**)本地库所有内容到远程库上
```
$ git push [-u] origin 本地分支:远程分支
$ git push -u origin master:master
$ git push -u origin master:main
```

### 3.2 从远程库克隆 

* 克隆(**Clone**)代码
```
$ git clone git@github.com:用户名/仓库名.git
```

***

## 4.分支管理

### 4.1 创建与合并分支

* `HEAD`指针严格来说指向`master`，而`master`指向提交，所以`HEAD`指向当前提交。每次提交`master`都会向前移动一步，这样随着提交的进行`master`分支越来越长。
* 当创建一个新的分支，例如`dev`，`dev`指向和`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上。
* Git创建分支很快，只是增加了一个`dev`指针，改变了`HEAD`的指向，工作区文件并未发生变化。之后的操作和提交都是针对`dev`分支了，每次commit之后`dev`指针都会移动一步，而`master`指针不变。
* 加入`dev`分支的都完成了，要将`dev`合并到`master`上，则直接将`master`指向`dev`当前的提交就完成了合并。
* 合并之后可以删除`dev`分支，本质上就是删除`dev`指针。

创建并切换分支
```
$ git checkout -b 分支名
```
相当于以下两条命令
```
$ git branch 分支名
$ git checkout 分支名
```
由于切换分支用的是`git checkout 分支名`，而撤销文件修改的命令是`git checkout -- <File>`，两种功能使用同一个命令，令人迷惑，所以最新的Git提供了`git switch`命令。
```
$ git switch 分支名
```
创建并切换
```
$ git switch -c 分支名
```
切换到`master`分支
```
$ git switch master
```

可以使用`git branch`查看当前分支，当前分支前有一个`*`号。

合并分支
```
$ git merge 分支名
```
删除分支
```
$ git branch -d 分支名
```
强制删除分支
```
$ git branch -D 分支名
```

### 4.2 解决冲突

Git使用`<<<<<<<`，`=======`，`>>>>>>>`标记不同分支的内容，对于冲突，修改后保存。然后使用`git add`和`git commit`提交修改版本。

合并只对当前分支产生作用，合并后，被合并分支保持不变。

`git log --graph --pretty=online --abbrev-commit`可以可视化分支合并情况。

### 4.3 分支管理策略

通常Git合并使用`fast forward`模式，但是这种模式下，删除分支后会丢失分支信息。可以强制禁用`fast forward`，Git在合并时就会产生一个新的commit，从分支历史可以看出分支信息。

```
$ git merge --no-ff -m "..." 被合并分支名称
```
因为会产生一次commit 所以要有-m参数写入commit描述

### 4.4 Bug分支

#### 4.4.1 工作区的保存与取出
* `git stash`可以将工作区现场保存下来。`git stash list`可以查看保存的现场。
* `git stash apply`可以恢复之前保存的现场，但是stash内容并未删除，需要使用`git stash drop`来删除。恢复指定的stash：
```
$ git stash apply 具体stash名称
```
* `git stash pop`可以恢复同时删除现场。

#### 4.4.2 对于bug的处理
假设有两个分支`master`和`dev`，都存在相同的一个bug
1. 可以先将`dev`分支使用`git stash`保存现场
2. 切换至`master`分支，创建一个临时分支`bug`，进入`bug`分支并修复bug，回到`master`分支，合并`bug`分支并commit。
3. 切换至`dev`分支，使用`git stash apply`和`git stash drop`或者`git stash pop`还原现场；将`master`分支中修复bug的commit复制给`dev`分支，这样`dev`分支的bug也被修复了，可以通过`git cherry-pick <commit_id>`来将修复bug的commit应用到`dev`分支上。

### 4.5 多人协作

查看远程库信息
```
$ git remote
```
查看更详细信息
```
$ git remote -v
```
推送分支
```
$ git push origin 本地分支名
```
抓取分支
```
$ git checkout -b 本地分支 远程分支
```
  取远程的分支分化为本地的一个新分支

设置本地分支和远程分支的链接
```
$ git branch --set-upstream-to=origin/远程分支 本地分支
```

多人协作的一般流程：
1. 试图使用`git push origin 本地分支[:远程分支]`推送自己的改动。
2. 如果推送失败，是因为远程分支比本地分支版本新，需要先`git pull`试图合并
3. 如果提示`no tracking information`，则在本地分支和远程分支建立链接关系，`git branch --set-upstream-to=origin/远程分支 本地分支`
4. 如果有冲突，先解决冲突，并在本地commit
5. 没有冲突或者冲突解决后，再用`git push origin 本地分支[:远程分支]`推送成功。


`git rebase`操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。

***

## 5.标签

### 5.1 创建标签

1. 首先切换至需要打标签的分支。
2. 使用`git tag <tagname>`打上标签。
3. 可以使用`git tag`查看所有标签。
4. 默认标签是打在最新的commit上的，可以通过`git tag <tagname> commit_id`给历史commit打标签，还可以使用`git tag -a <tagname> -m "blablabla..."`指定标签信息。
5. 可以使用`git show <tagname>`查看标签信息

注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

### 5.2 操作标签

删除标签
```
$ git tag -d <tagname>
```
创建的标签只会储存在本地，不会自动推送到远程，所以本地打错了可以安全删除。
如果要推送某个标签到远程：
```
$ git push origin <tagname>
```
或者一次性推送所有尚未推送的标签：
```
$ git push origin --tags
```
删除远程标签分两步：
1. `git tag -d <tagname>`先删除本地标签
2. 然后删除远程：`git push origin :refs/tags/<tagname>`

***

## 6.Github & Gitee

&#x1F644;

***

## 7.自定义Git

### 7.1 忽略文件

`.gitignore`文件记录要忽视的文件和规则

可以用`git check-ignore`命令检查被忽视文件受什么规则限制。
例如：
```
git check-ignore -v App.class
```
得到：
```
.gitignore:3:*.class App.class
```
说明是`.gitignore`的第3行的规则忽略了该文件

`.gitignore`常用规则：
```
# 排除所有.开头的隐藏文件:
.*
# 排除所有.class文件:
*.class

# 不排除.gitignore和App.class:
!.gitignore
!App.class
```

### 7.2 配置别名

例如：准备将`git status`简化成`git st`
```
$ git config --global alias.st status
```

又如：
```
$ git config --global alias.unstage 'reset HEAD'
```
将`unstage`配置成了`reset HEAD`，使得以下两句话等价：
```
git unstage test.py
git reset HEAD test.py
```

配置Git时，加上`--global`则针对当前用户起作用，不加只对当前仓库起作用。
配置文件放在`.git/config`

***

本文主要参考自：
1. https://www.liaoxuefeng.com/wiki/896043488029600
2. https://www.runoob.com/git/git-tutorial.html
<p align="right">zwxupc@163.com 2020.10.10</p>