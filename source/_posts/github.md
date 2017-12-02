title: Github学习心得
date: 2015-09-23 17:36:44
layout: post
tags:
- Github
categories:
- Tech
- Github
---

# Github学习心得

不得不说，Git是一个强大的分布式版本控制系统。

对比起集中式分布系统来说，分布式版本控制系统仅仅是传输了对文件的修改，而非文件本身，所以使用Git的时候你会发现上传下载的速度都很快。

集中式的特点是仅仅只有一台主机作为库，所有程序员都要从这台主机上上传下载文件，所以当使用互联网而带宽不足，文件太大时，上传下载耗费的时间就很多了。而分布式版本控制系统的最大优势在于每个程序员直接都可以传输文件，这就是本地库的使用。有了本地库，两台电脑就可以互相传输文件，最后只要再合并后上传到总库即可，这无疑大大提高了工作效率。


### 安装Git

- Linux下打开终端输入 sudo apt-get install git

- Windows下打开http://msysgit.github.io/，从里面下载msysgit并安装即可

- MacOS的话用Xcode，点击菜单栏的Preference，找到Download并选择command line tools，点击instal即可

- 最后输入本地Git作者信息，终端输入
`$ git config --global user.name "YourName"`
`$ git config --global user.email "YourEmail@example.com"`


### 新建本地版本库

- 在本地的一个工作目录下新建一个本地库
`$ git init`

- 这时在文件夹中就会出现.git的隐藏文件夹，输入ll便可查看


### 添加文件

- 这里本地版本库分暂存区和分支，而工作区就是本地文件夹。我们首先将工作区（当前文件夹）里面的文件添加到暂存区里面
`$ git add FileName`

- 再添加到分支里面
`$ git commit -m "added message"`

- 这里添加到分支还可以添加一个信息，就是简单介绍这个提交修改了什么


### 分支

- 分支是Git里面一个强大的机制，分支是发展是按时间轴进行的，可以将分支理解为一个链表。这个链表的头部HEAD指向最后一次提交（包括分支名和提交号）

- 如果创建一个分支，就是从当前分支新建一个节点创建
`$ git branch`

- 这条命令用于查看当前工作区的所有分支，*标记的是当前分支

- 下面是一些分支的命令

- 创建并切换分支
`$ git checkout -b BranchName'

- 切换分支
`$ git checkout BranchName`


### 查看git状态

- 我们可以通过一系列的查看命令查看当前的工作区/暂存区/分支表的状态
`$ git status`

- 查看具体修改了什么内容
`$ git diff`

- 检查两个文件的不同
`$ git diff HEAD -- FileName`

- 查看库所有修改内容（包括其他人）
`$ git log`

- 单行查看（简便）
`$ git log --pretty=oneline`

- 图形查看（分支明显）
`$ git log --pretty=oneline --abbrev-commit`

### 回退分支表的版本

- 回到CommitID的状态，CommitID可以在log中获取
`$ git reset --hard CommitID`

- 回退到上一版本
`$ git reset --hard HEAD^`

- 回退到上100个版本
`$ git reset --hard HEAD~100`

### 新建远程库，克隆远程库，上传与拉下

- 远程库的新建有SSH和http两种协议，如果是SSH协议来创建远程库的话，要在终端输入ssh -keygen -t rsa -C "youremail@example.com"来产生一个密钥存储本机信息，然后在主文件夹中打开.ssh隐藏文件夹，找到id_rsa.pub（公钥，没有.pub的是私钥）。将公钥复制到GitHub的SSH Key。最后在GitHub上新建一个公共的仓库即可。
`$ git remote add RemoteName git@github.com:GithubName/ReporityName.git `

- 克隆就是将远程库的所有东西下载到本地文件夹中，简单来说就是工作的开始
`$ git clone git@github.com:GithubName/ReporityName.git`

- 上传用push，拉下用pull。每次你修改完成后要push到另外一个库，就要用push。每次你想从别人的版本中更新到自己的本地，就要用pull
`$ git push RemoteName BranchName:RemoteBranchName`
`$ git pull RemoteName BranchName`

### 合并分支，解决冲突

- 在多分支工作就要合并，合并是Git一个重要的功能，通过merge合并。

- 将BranchName分支的内容合并到当前分支
`$ git merge BranchName`

- 采用非FastForward方法合并
$ git merge --no-ff -m "merge message" BranchName`


### 管理标签

- 标签是个挺方便查看项目的东西，我们可以新建标签和修改标签等

- 查看标签
`$ git tag`

- 新建标签
`$ git tag TagName

- 删除标签
`$ git tag -d TagName`


### 忽略文件

- 如果在当前文件夹下有些无关的文件，而用status 查看时又会显示未跟踪，该怎么办呢。我们只需要在当前文件夹下新建一个.gitignore文件，将需要隐藏的文件或文件夹添加到这个文件中即可。


### Reference

- http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000

