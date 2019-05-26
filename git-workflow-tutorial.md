
# 參考

[深入理解学习Git工作流（git-workflow-tutorial）](https://segmentfault.com/a/1190000002918123)

參考後紀錄

# 目錄

- [參考](#參考)
- [目錄](#目錄)
- [集中式工作流](#集中式工作流)
    - [示例](#示例) 
        - [有人先初始化好中央仓库](#有人先初始化好中央仓库)
        - [所有人克隆中央仓库](#所有人克隆中央仓库)
        - [小明发布功能](#小明发布功能)
        - [小红试着发布功能 Git拒绝操作](#小红试着发布功能-git拒绝操作)
        - [小红解决合并冲突](#小红解决合并冲突)
        - [小红成功发布功能](#小红成功发布功能)
- [功能分支工作流](#功能分支工作流)
    - [工作方式](#工作方式)
    - [示例](#示例-1)
        - [小红开始开发一个新功能](#小红开始开发一个新功能)
        - [小红要去吃个午饭](#小红要去吃个午饭)
        - [小红完成功能开发](#小红完成功能开发)
        - [小黑收到Pull Request](#小黑收到pull-request)
        - [小红再做修改](#小红再做修改)
        - [小红发布她的功能](#小红发布她的功能)
        - [与此同时，小明在做和小红一样的事](#与此同时小明在做和小红一样的事)
- [Gitflow工作流](#gitflow工作流)
    - [历史分支 master develop](#历史分支-master-develop)
    - [功能分支 feature](#功能分支-feature)
    - [发布分支 release](#发布分支-release)
    - [维护分支 hotfix](#维护分支-hotfix)
    - [示例 (历史分支)](#示例-历史分支)
        - [小红和小明开始开发新功能 (功能分支)](#小红和小明开始开发新功能-功能分支)
        - [小红完成功能开发](#小红完成功能开发)
        - [小红开始准备发布(发布分支)](#小红开始准备发布发布分支)
        - [小红完成发布](#小红完成发布)
        - [最终用户发现Bug (维护分支)](#最终用户发现bug-维护分支)
- [Forking工作流](#forking工作流)

# 集中式工作流

中央仓库代表了正式项目，所以提交历史应该被尊重且是稳定不变的。如果开发者本地的提交历史和中央仓库有分歧，Git会拒绝push提交否则会覆盖已经在中央库的正式提交。

在开发者提交自己功能修改到中央库前，需要先fetch在中央库的新增提交，rebase自己提交到中央库提交历史之上。
这样做的意思是在说，『我要把自己的修改加到别人已经完成的修改上。』最终的结果是一个完美的线性历史，就像以前的SVN的工作流中一样

## 示例

### 有人先初始化好中央仓库

### 所有人克隆中央仓库

    git clone

小红、小明开发功能

### 小明发布功能

一旦小明完成了他的功能开发，会发布他的本地提交到中央仓库中，这样其它团队成员可以看到他的修改。他可以用下面的git push命令：

    git push origin master

### 小红试着发布功能 Git拒绝操作

这避免了小红覆写正式的提交。她要先pull小明的更新到她的本地仓库合并上她的本地修改后，再重试。

小红在小明的提交之上rebase

    git pull --rebase origin master

### 小红解决合并冲突

小红可以简单的运行git status命令来查看哪里有问题

    git status

接着小红编辑这些文件。修改完成后，用老套路暂存这些文件，并让git rebase完成剩下的事：

    git add <some-file> 
    git rebase --continue

如果你碰到了冲突，但发现搞不定，不要惊慌。只要执行下面这条命令，就可以回到你执行git pull --rebase命令前的样子：

    git rebase --abort

### 小红成功发布功能

    git push origin master

# 功能分支工作流

功能分支工作流以集中式工作流为基础，不同的是为各个新功能分配一个专门的分支来开发。这样可以在把新功能集成到正式项目前，用Pull Requests的方式讨论变更。

## 工作方式

## 示例

### 小红开始开发一个新功能

在开始开发功能前，小红需要一个独立的分支。使用下面的命令新建並轉至一个分支：

    git checkout -b marys-feature master

-b选项表示如果分支还不存在则新建分支。

这个新分支上，小红按老套路编辑、暂存和提交修改，按需要提交以实现功能：

    git status
    git add <some-file>
    git commit

### 小红要去吃个午饭

早上小红为新功能添加一些提交。
去吃午饭前，push功能分支到中央仓库是很好的做法，这样可以方便地备份，如果和其它开发协作，也让他们可以看到小红的提交。

    git push -u origin marys-feature

-u选项设置本地分支去跟踪远程对应的分支。

设置好跟踪的分支后，小红就可以使用git push命令省去指定推送分支的参数

### 小红完成功能开发

首先，她要确认中央仓库中已经有她最近的提交：

    git push

在她的Git GUI客户端中发起*Pull Reques*t，请求合并*marys-feature*到*master*，团队成员会自动收到通知

### 小黑收到Pull Request

小黑收到了*Pull Request*后会查看*marys-feature*的修改。决定在合并到正式项目前是否要做些修改，且通过*Pull Request*和小红来回地讨论。

### 小红再做修改

要再做修改，小红用和功能第一个迭代完全一样的过程。编辑、暂存、提交并*push*更新到中央仓库。小红这些活动都会显示在*Pull Request*上，小黑可以断续做评注。

如果小黑有需要，也可以把*marys-feature*分支拉到本地，自己来修改，他加的提交也会一样显示在*Pull Request*上。

### 小红发布她的功能

一旦小黑可以的接受*Pull Reques*t，就可以合并功能到稳定项目代码中（可以由小黑或是小红来做这个操作）：

    git checkout master
    git pull
    git pull origin marys-feature
    git push

无论谁来做合并，首先要检出*master*分支并确认是它是最新的。然后执行*git pull origin marys-feature*合并*marys-feature*分支到和已经和远程一致的本地*master*分支。
你可以使用简单*git merge marys-feature*命令，但前面的命令可以保证总是最新的新功能分支。
最后更新的*master*分支要重新*push*回到*origin*

但如果你偏爱线性的提交历史，可以在执行合并时*rebase*新功能到*master*分支的顶部，这样生成一个快进（*fast-forward*）的合并。

一些GUI客户端可以只要点一下『接受』按钮执行好上面的命令来自动化*Pull Request*接受过程。
如果你的不能这样，至少在功能合并到*master*分支后能自动关闭*Pull Request*。

### 与此同时，小明在做和小红一样的事

当小红和小黑在*marys-feature*上工作并讨论她的*Pull Request*的时候，小明在自己的功能分支上做完全一样的事。

通过隔离功能到独立的分支上，每个人都可以自主的工作，当然必要的时候在开发者之间分享变更还是比较繁琐的。

到了这里，但愿你发现了功能分支可以很直接地在 集中式工作流 的仅有的*master*分支上完成多功能的开发。
另外，功能分支还使用了*Pull Request*，使得可以在你的版本控制GUI客户端中讨论某个提交。

功能分支工作流是开发项目异常灵活的方式。问题是，有时候太灵活了。对于大型团队，常常需要给不同分支分配一个更具体的角色。

# Gitflow工作流

Gitflow工作流是管理功能开发、发布准备和维护的常用模式。

## 历史分支 master develop

相对使用仅有的一个*master*分支，*Gitflow*工作流使用2个分支来记录项目的历史。        
* *master*分支存储了正式发布的历史，通常有分配一个版本号

* *develop*分支作为功能的集成分支。

## 功能分支 feature

每个新功能位于一个自己的分支，这样可以push到中央仓库以备份和协作。
但功能分支不是从*master*分支上拉出新分支，而是使用develop分支作为父分支。当新功能完成时，合并回*develop*分支。
新功能提交应该从不直接与*master*分支交互

注意，从各种含义和目的上来看，*develop*分支就是功能分支工作流的用法

## 发布分支 release

一旦*develop*分支上有了做一次发布（或者说快到了既定的发布日）的足够功能，就从develop分支上fork一个发布分支。
新建的分支用于开始发布循环，所以从这个时间点开始之后新的功能不能再加到这个分支上——
这个分支只应该做Bug修复、文档生成和其它面向发布任务。
一旦对外发布的工作都完成了，发布分支合并到master分支并分配一个版本号打好Tag。
另外，这些从新建发布分支以来的做的修改要合并回develop分支。

使用一个用于发布准备的专门分支，使得一个团队可以在完善当前的发布版本的同时，另一个团队可以继续开发下个版本的功能。
这也打造定义良好的开发阶段（比如，可以很轻松地说，『这周我们要做准备发布版本4.0』，并且在仓库的目录结构中可以实际看到）

## 维护分支 hotfix

维护分支或说是热修复（hotfix）分支用于生成快速给产品发布版本（production releases）打补丁，这是唯一可以直接从master分支fork出来的分支。
修复完成，修改应该马上合并回master分支和develop分支（当前的发布分支），master分支应该用新的版本号打好Tag。

为Bug修复使用专门分支，让团队可以处理掉问题而不用打断其它工作或是等待下一个发布循环。
你可以把维护分支想成是一个直接在master分支上处理的临时发布

## 示例 (历史分支)

第一步为master分支配套一个develop分支。简单来做可以本地创建一个空的develop分支，push到服务器上：

    git branch develop
    git push -u origin develop

以后这个分支将会包含了项目的全部历史，而master分支将只包含了部分历史。其它开发者这时应该克隆中央仓库，建好develop分支的跟踪分支：

    git clone ssh://user@host/path/to/repo.git
    git checkout -b develop origin/develop

### 小红和小明开始开发新功能 (功能分支)

这个示例中，小红和小明开始各自的功能开发。他们需要为各自的功能创建相应的分支。新分支不是基于master分支，而是应该基于develop分支：

    git checkout -b some-feature develop

他们用老套路添加提交到各自功能分支上：编辑、暂存、提交：

    git status
    git add <some-file>
    git commit

### 小红完成功能开发

添加了提交后，小红觉得她的功能OK了。如果团队使用Pull Requests，这时候可以发起一个用于合并到develop分支。
否则她可以直接合并到她本地的develop分支后push到中央仓库：

    git pull origin develop
    git checkout develop
    git merge some-feature
    git push
    git branch -d some-feature

第一条命令在合并功能前确保develop分支是最新的。注意，功能决不应该直接合并到master分支。
冲突解决方法和集中式工作流一样。

### 小红开始准备发布(发布分支)

这个时候小明正在实现他的功能，小红开始准备她的第一个项目正式发布。
像功能开发一样，她用一个新的分支来做发布准备。这一步也确定了发布的版本号：

    git checkout -b release-0.1 develop

#### 小红完成发布

一旦准备好了对外发布，小红合并修改到master分支和develop分支上，删除发布分支。合并回develop分支很重要，因为在发布分支中已经提交的更新需要在后面的新功能中也要是可用的。
另外，如果小红的团队要求Code Review，这是一个发起Pull Request的理想时机。

    git checkout master
    git merge release-0.1
    git push
    git checkout develop
    git merge release-0.1
    git push
    git branch -d release-0.1

发布分支是作为功能开发（develop分支）和对外发布（master分支）间的缓冲。只要有合并到master分支，就应该打好Tag以方便跟踪。

    git tag -a 0.1 -m "Initial public release" master
    git push --tags

### 最终用户发现Bug (维护分支)

对外发布后，小红回去和小明一起做下个发布的新功能开发，直到有最终用户开了一个Ticket抱怨当前版本的一个Bug。
为了处理Bug，小红（或小明）从master分支上拉出了一个维护分支，提交修改以解决问题，然后直接合并回master分支：

    git checkout -b issue-#001 master
    # Fix the bug
    git checkout master
    git merge issue-#001
    git push

就像发布分支，维护分支中新加这些重要修改需要包含到develop分支中，所以小红要执行一个合并操作。然后就可以安全地删除这个分支了：

    git checkout develop
    git merge issue-#001
    git push
    git branch -d issue-#001

# Forking工作流

Forking工作流和前面讨论的几种工作流有根本的不同，这种工作流不是使用单个服务端仓库作为『中央』代码基线，而让各个开发者都有一个服务端仓库。这意味着各个代码贡献者有2个Git仓库而不是1个：一个本地私有的，另一个服务端公开的。

未完...







