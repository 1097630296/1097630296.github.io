---
layout: post
title: Git操作手册|命令速查表 
category: PHP
tag: [php]
date : 2015-02-20
excerpt: Git操作手册|命令速查表
---


******

<!-- more -->

这篇文章主要介绍Git分布式版本管理与集中式管理的一些差异，总结下Git常用命令作为日后的速查表，最后介绍Git进阶的一些案例。
本文分为以下几个部分：
1. Git与SVN差异
2. Git常用命令
3. Git进阶指南

##Git与SVN差异

Git的第一个版本是Linux之父Linus Torvalds亲手操刀设计和实现的,Git 基于 DAG 结构 (Directed Acyclic Graph)，其运行起来相当的快,它已经是现在的主流。

Git 和 SVN 思想最大的差别有四个：

* 去中心化
* 直接记录快照，而非差异
* 不一样的分支概念
* 三个文件状态

**去中心化**

Git是一个DVCS（分布式版本管理系统），在技术层面上并不存在一个像中心仓库这样的东西 ， 所有的数据都在本地，不存在谁是中心

![](/images/images/git.gif)

图中每个开发者拉取(pull)并推送(push)到origin。但除了这种集中式的推送拉取关系，每个开发者也可能会从其他的开发者处拉取代码的变更，从技术上讲，这意味着Alice定义了一个名为bob的Git的remote，它指向了Bob的软件仓库。反之亦然。

**直接记录快照，而非差异**

Git每一个版本都是直接记录快照，而非文件的差异。 下面两个对比图在网上是广为流传大家应该熟悉：

SVN：

![](/images/images/svn.png)

Git:

![](/images/images/gitgit.png)

Git使用SHA-1算法计算数据的校验和，通过文件的内容或目录计算出SHA-1哈希值，作为指纹字符串，每个Version 都是一个快照。

**不一样的分支概念**

Git的分支本质是一个指向提交快照的指针，是从某个提交快照往回看的历史。当创建/切换分支的时候，只是变换了指针指向而已.而SVN创建一个分支， 是的的确确的复制了一份文件。

**三个文件状态**

在Git中文件有三种状态：

* 已提交（committed）：该文件被安全地保存在了本地数据库
* 已修改（modified）：修改了某个文件，但还没有保存
* 已暂存（staged）：把已修改的文件放下下次保存的清单中


##Git常用命令

###创建

复制一个已创建的仓库:

	$ git clone ssh://user@domain.com/repo.git

创建一个新的本地仓库:

	$ git init

###本地修改

显示工作路径下已修改的文件：

	$ git status

显示与上次提交版本文件的不同：

	$ git diff

把当前所有修改添加到下次提交中：

	$ git add

把对某个文件的修改添加到下次提交中：

	$ git add -p <file>
	
提交本地的所有修改：

	$ git commit -a

提交之前已标记的变化：

	$ git commit

附加消息提交：

	$ git commit -m 'message here'

提交，并将提交时间设置为之前的某个日期:

	git commit --date="`date --date='n day ago'`" -am "Commit Message"

###修改上次提交

请勿修改已发布的提交记录!

	$ git commit --amend

把当前分支中未提交的修改移动到其他分支

	git stash
	git checkout branch2
	git stash pop

###搜索

从当前目录的所有文件中查找文本内容：

	$ git grep "Hello"

在某一版本中搜索文本：

	$ git grep "Hello" v2.5

###提交历史

从最新提交开始，显示所有的提交记录（显示hash， 作者信息，提交的标题和时间）：

	$ git log

显示所有提交（仅显示提交的hash和message）：

	$ git log --oneline

显示某个用户的所有提交：

	$ git log --author="username"

显示某个文件的所有修改：

	$ git log -p <file>

谁，在什么时间，修改了文件的什么内容：

	$ git blame <file>

###分支与标签

列出所有的分支：

	$ git branch

切换分支：

	$ git checkout <branch>

创建并切换到新分支:

	$ git checkout -b <branch>

基于当前分支创建新分支：

	$ git branch <new-branch>

基于远程分支创建新的可追溯的分支：

	$ git branch --track <new-branch> <remote-branch>

删除本地分支:

	$ git branch -d <branch>

给当前版本打标签：

	$ git tag <tag-name>

###更新与发布

列出当前配置的远程端：

	$ git remote -v

显示远程端的信息：

	$ git remote show <remote>

添加新的远程端：

	$ git remote add <remote> <url>

下载远程端版本，但不合并到HEAD中：

	$ git fetch <remote>

下载远程端版本，并自动与HEAD版本合并：

	$ git remote pull <remote> <url>

将远程端版本合并到本地版本中：

	$ git pull origin master

将本地版本发布到远程端：

	$ git push remote <remote> <branch>

删除远程端分支：

	$ git push <remote> :<branch> (since Git v1.5.0)
	或
	git push <remote> --delete <branch> (since Git v1.7.0)

发布标签:

	$ git push --tags

###合并与重置

将分支合并到当前HEAD中：

	$ git merge <branch>

将当前HEAD版本重置到分支中:
请勿重置已发布的提交!

	$ git rebase <branch>
	
退出重置:

	$ git rebase --abort

解决冲突后继续重置：

	$ git rebase --continue

使用配置好的merge tool 解决冲突：

	$ git mergetool

在编辑器中手动解决冲突后，标记文件为已解决冲突

	$ git add <resolved-file>
	$ git rm <resolved-file>

###撤销

放弃工作目录下的所有修改：

	$ git reset --hard HEAD

移除缓存区的所有文件（i.e. 撤销上次git add）:

	$ git reset HEAD

放弃某个文件的所有本地修改：

	$ git checkout HEAD <file>

重置一个提交（通过创建一个截然不同的新提交）

	$ git revert <commit>

将HEAD重置到指定的版本，并抛弃该版本之后的所有修改：

	$ git reset --hard <commit>

将HEAD重置到上一次提交的版本，并将之后的修改标记为未添加到缓存区的修改：

	$ git reset <commit>

将HEAD重置到上一次提交的版本，并保留未提交的本地修改：

	$ git reset --keep <commit>


##Git进阶指南


###问：如何修改 origin 仓库信息？

####1、添加 origin 仓库信息

	git remote add origin <git仓库地址>

####2、查看 origin 仓库信息

	# 以下三种方式均可
	git config get --remote.origin.url
	git remote -v
	git remote show origin
 
####3、删除 origin 仓库信息

	 git remote rm origin

###问：如何配置 git ssh keys ？

在本地生成 ssh 私钥 / 公钥 文件
将「公钥」添加到 git 服务（github、gitlab、coding.net 等）网站后台
测试 git ssh 连接是否成功
接下来以添加 github ssh keys 为例，请注意替换 github 文件名。

注：如果对密钥机制不熟悉，建议不要指定 -f 参数，直接使用默认的 id_rsa 文件名。

	# 运行以下命令，一直回车，文件名可随意指定
	ssh-keygen -t rsa -b 4096 -C "kaiye@macbook" -f ~/.ssh/github

	# 如果不是默认密钥 id_rsa ，则需要以下命令注册密钥文件，-K 参数将密钥存入 Mac Keychain
	ssh-add -K ~/.ssh/github

	# 将 pub 公钥的内容粘贴到线上网站的后台
	cat ~/.ssh/github.pub

	# 测试 git ssh 是否连接成功
	ssh -T git@github.com

###问：如何撤销修改？

  修改包含四种情况，需单独区分。

####1、新建的文件和目录，且从未提交至版本库

  此类文件的状态为 Untracked files ，撤销方法如下：

	git clean -fd .
 
其中，. 表示当前目录及所有子目录中的文件，也可以直接指定对应的文件路径，以下其他情况类似。

####2、提交过版本库，但未提交至暂存区的文件（未执行 git add）

  此类文件的状态为` Changes not staged for commit`，撤销方法：

	 git checkout .

####3、已提交至暂存区的文件

  此类文件的状态为 Changes to be committed，撤销方法：

	git reset .
 
执行之后文件将会回到以上的 1 或者 2 状态，可继续按以上步骤执行撤销，若 git reset 同时加上 --hard 参数，将会把修改过的文件也还原成版本库中的版本。

####4、已提交至版本库（执行了 git commit）

  每次提交都会生成一个 hash 版本号，通过以下命令可查阅版本号并将其回滚：

	git log
	git reset <版本号>
 
如果需要「回滚至上一次提交」，可直接使用以下命令：

	git reset head~1
 
执行之后，再按照 1 或者 2 状态进行处理即可，如果回滚之后的代码同时需要提交至 origin 仓库（即回滚 origin 线上仓库的代码），需要使用 -f 强制提交参数，且当前用户需要具备「强制提交的权限」。

####5、如果回滚了之后又不想回滚了怎么办？

  如果是以上的情况 1 或者 2，只能歇屁了，因为修改没入过版本库，无法回滚。

  如果是情况 4，回滚之后通过 git log 将看不到回滚之前的版本号，但可通过 git reflog 命令（所有使用过的版本号）找到回滚之前的版本号，然后 git reset <版本号> 。

###问：遇到冲突了怎么解决？

  两个分支进行合并时（通常是 git pull 时），可能会遇到冲突，同时被修改的文件会进入 Unmerged 状态，需要解决冲突。

####1、最快的办法

  大部分时候，「最快解决冲突」的办法是：使用当前 HEAD 的版本（ours），或使用合并进来的分支版本（theirs）。

	# 使用当前分支 HEAD 版本，通常是冲突源文件的 <<<<<<< 标记部分，======= 的上方
	git checkout --ours <文件名>

	 # 使用合并分支版本，通常是源冲突文件的 >>>>>>> 标记部分
	 git checkout --theirs <文件名>

	# 标记为解决状态加入暂存区
	git add <文件名>

####2、最通用的办法

  用编辑器打开冲突的源文件进行修改，可能会发生遗留，且体验不好，通常需要借助 git mergetool 命令。

  在 Mac 系统下，运行 git mergetool <文件名> 可以开启配置的第三方工具进行 merge，默认的是 FileMerge 应用程序，还可以配置成 Meld 或 kdiff3，体验更佳。

####3、最好的习惯

  有三个好的习惯，可以减少代码的冲突：
 在开始修改代码前先 git pull 一下；
  将业务代码进行划分，尽量不要多个人在同一时间段修改同一文件；
  通过Gitflow 工作流也可以提升 git流程效率，减少发生冲突的可能性。
  
####4、最复杂的情况

  如果你的项目周期比较长，还应该养成「定期 rebase 的习惯」，git pull --rebase 可以让分支的代码和 origin 仓库的代码保持兼容，同时还不会破坏线上代码的可靠性。

  它的大概原理是，先将 origin 仓库的代码按 origin 的时间流在本地分支中提交，再将本地分支的修改记录追加到 origin 分支上。如果发生冲突，则可以即时的发现问题并解决，否则到项目上线时再解决冲突，可能会发生额外的风险。

  rebase 大概的操作步骤如下：

	# 将当前分支的版本追加到从远程 pull 回来的节点之后
	git pull --rebase

	# 若发生冲突，则按以上其他方法进行解决，解决后继续
	git rebase --continue

	# 直到所有冲突得以解决，待项目最后上线前再执行
	git push origin

	# 若多次提交修改了同一文件，可能需要直接跳过后续提交，按提示操作即可
	git rebase --skip

###问：如何在不提交修改的前提下，执行 pull / merge 等操作？

  有些修改没有完全完成之前，可能不需要提交到版本库，圡方法是将修改的文件 copy 到 git 仓库之外的目录临时存放，pull / merge 操作完成之后，再 copy 回来。

  这样的做法一个是效率不高，另外一个可能会遗漏潜在的冲突。此类需求最好是通过 git stash 命令来完成，它可以将当前工作状态（WIP，work in progress）临时存放在 stash 队列中，待操作完成后再从 stash 队列中重新应用这些修改。

  以下是 git stash 常用命令：

	# 查看 stash 队列中已暂存了多少 WIP
	git stash list

	# 恢复上一次的 WIP 状态，并从队列中移除
	git stash pop

	# 添加当前 WIP，注意：未提交到版本库的文件会自动忽略，只要不运行 git clean -fd . 就不会丢失
	git stash

	# 恢复指定编号的 WIP，同时从队列中移除
	git stash pop stash@{num}

	# 恢复指定编号的 WIP，但不从队列中移除
	git stash apply stash@{num}

###问：如何在 git log 中查看修改的文件列表？

  默认的 git log 会显示较全的信息，且不包含文件列表。使用 --name-status 可以看到修改的文件列表，使用 --oneline 可以将参数简化成一行。

	git log --name-status --oneline
	
  每次手动加上参数很麻烦，可以通过自定义快捷命令的方式来简化操作：

	git config --global alias.ls 'log --name-status --oneline --graph'

  运行以上配置后，可通过 git ls 命令来实现「自定义 git log」效果，通过该方法也可以创建 git st 、 git ci 等一系列命令，以便沿用 svn 命令行习惯。

	git config --global alias.st 'status --porcelain'

  更多 git log 参数，可通过 git help log 查看手册。

  如果是看上一次提交的版本日志，直接运行 git show 即可。

  此外，如果你的 Mac 安装了zsh（参考《全新Mac安装指南（编程篇），那么可以直接使用 gst、glog 等一系列快捷命令，详情见此列表：Plugin:git 。

###问：git submodule update 时出错怎么解决？

  例如，在执行 git submodule update 时有以下错误信息：

>fatal: reference is not a tree: f869da471c5d8a185cd110bbe4842d6757b002f5
Unable to checkout 'f869da471c5d8a185cd110bbe4842d6757b002f5' in submodule path 'source/i18n-php-server'

  在此例中，发生以上错误是因为 i18n-php-server 子仓库在某电脑 A 的「本地」commit 了新的版本 「f869da471c5d8a185cd110bbe4842d6757b002f5」，且该次 commit 未 push origin。但其父级仓库 i18n-www 中引用了该子仓库的版本号，且将引用记录 push origin，导致其他客户机无法 update 。

  解决方法，在电脑 A 上将 i18n-php-server 版本库 push origin 后，在其他客户机上执行 git submodule update 。或者用以上提到的 git reset 方法，将子仓库的引用版本号还原成 origin 上存在的最新版本号。

###其他问题

  设置本地分支与远程分支保持同步，在第一次 git push 的时候带上 -u 参数即可

	git push origin master -u 

  支持中文目录与文件名的显示（git 默认将非 ASCII 编码的目录与文件名以八进制编码展示）

	git config core.quotepath off

  常用的打 tag 操作，更多请查看《Git 基础 - 打标签》

	# 列出所有本地 tag
	git tag   

	# 本地新增一个 tag，推送至 origin 服务器
	git tag -a v1.0.0 -m 'tag description'
	git push origin v1.0.0

	# 删除本地与 origin tag
	git tag -d v1.0.0
	git push origin --delete v1.0.0

  使用 git GUI 客户端（如，SoureTree、Github Desktop）能极大的提升分支管理效率。分支合并操作通常只有两种情况：从 origin merge 到本地，使用 git pull 即可；从另外一个本地分支 merge 到当前分支，使用 git merge <分支名>，以下是常用命令：

	# 新建分支 branch1，并切换过去
	git checkout -b branch1

	# 查看所有本地与远程分支
	git branch -a

	# 修改完成后，切换回 master 分支，将 branch1 分支合并进来
	git checkout master
	git merge branch1

	# 删除已完成合并的分支 branch1
	git branch -d branch1

###参考资料

1. Pro Git 简体中文版
2. Git权威指南
3. 命令行man手册
