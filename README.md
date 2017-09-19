---
title: tool-git
date: 2017-09-19 17:30:19
tags: Git
categories: 学习
---

## 一、Git与常用的版本控制工具CVS,SVN等不同，它采用了分布式版本库的方式，不必服务器端软件支持。
#### GIT不仅仅是个版本控制系统，它也是个内容管理系统(CMS),工作管理系统等。
*Git 与 SVN 区别点：*  
1. GIT是分布式的，SVN不是。  
2. GIT把内容按元数据方式存储，而SVN是按文件：所有的资源控制系统都是把文件的元信息隐藏在一个类似.svn,.cvs等的文件夹里。  
3. GIT分支和SVN的分支不同：分支在SVN中一点不特别，就是版本库中的另外的一个目录。  
4. GIT没有一个全局的版本号，而SVN有：目前为止这是跟SVN相比GIT缺少的最大的一个特征。  
5. GIT的内容完整性要优于SVN：GIT的内容存储使用的是SHA-1哈希算法。这能确保代码内容的完整性，确保在遇到磁盘故障和网络问题时降低对版本库的破坏。  

#### 集中式和分布式的区别  
**集中式(svn,cvs) －－ 集中式版本控制系统**，工作前都需要把最新代码从中央库 Pull 下来。工作完成然后再 Push 到中央库中，意味着必须联网。受网络限制，并且不安全
**分布式** －－ 分布式版本控制系统根本没有“中央服务器”，每一台终端都是一个中央服务器。代码保存在本地，十分灵活。 
***
## 二、工作区、暂存区和版本库
+ 工作区：就是你在电脑里能看到的目录。
+ 版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
+ 暂存区：Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
***
## 三、创建仓库和常用git命令
- git init：初始化一个git仓库，Git的很多命令都需要在Git的仓库中运行。可以使用一个已经存在的目录作为Git仓库。在执行命令完成后，Git仓库会生成一个.git目录，该目录包含了资源的所有元数据，其他的项目目录保持不变（Git只在仓库的根目录生成.git目录）。  
- git init newrepo：初始化后，会在newrepo目录下会出现一个名为.git的目录，所有Git需要的数据和资源都存放在这个目录中。
- 如果当前目录下有几个文件想要纳入版本控制，需要先用 git add 命令告诉 Git 开始对这些文件进行跟踪，然后提交：  
    - `git add *.c        //将以.c结尾的文件提交到仓库`  
    - `git add README     //将README文件提交到仓库`  
    - `git commit -m '初始化项目版本'`
    -  `git clone <repo> <directory>    //将远程Git仓库克隆到指定的目录 repo:Git仓库 directory:本地目录`   
    - **默认情况下，Git 会按照你提供的 URL 所指示的项目的名称创建你的本地项目目录，通常就是该 URL 最后一个 / 之后的项目名称。如果你想要一个不一样的名字，你可以在该命令后加上你想要的名称。**  

`mkdir html                  //可以创建一个html的文件夹`  
`git init                //通过git init命令把这个目录变成Git可以管理的仓库。`  
`git pull               //将远程库上面的内容拉到本地库`  
`git add .           //将当前文件夹所有文件添加到缓存`  
`git commit -m "初始化项目版本"         //将缓存区内容添加到仓库中`  
`git push            //将本地库的内容推送到远程仓库`
`git status        //查看当前目录的状态`
`git diff           //查看修改内容`

***
## 四、配置用户名和邮箱地址(git是分布式的，所以每个机器都必须自报家门)  
`$ git config --global user.name "name"`  
`$ git config --global user.email "test@hhh.com"`  
`--global表示这台机器上所有Git仓库都会使用这个配置,当然也可以对某个仓库指定不同的用户名和Email地址`

***
## 五、git reset --hard HEAD^ 可以回退到上一版本，`git checkout -- <file>`撤销修改  
`git log --pretty=oneline       //可以查看历史纪录的版本号`  
`git reset --hard commit_id(十六进制的版本号)       //可以指定会到某一版本`  
Git的版本回退速度非常快，因为Git在内部有个指向当前版本的HEAD指针  
Git提供了一个命令`git reflog`用来记录你的每一次命令  
`git checkout -- <file>`可以丢弃工作区的修改，手动把文件恢复到上一个版本的状态。  
`git checkout -- readme.txt`意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：这里有两种情况：  
1. 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态;
2. 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。  
**Tip: git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令**

***
## 删除文件：  
- 命令`git rm file`用于从版本库中删除一个文件。git checkout -- file能用版本库里的版本替换工作区的版本，无论工作区的文件是修改还是删除，只要版本库有这个文件，都可以“一键还原”。 

***
## 远程仓库  
1. 第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：`ssh-keygen -t rsa -C "youremail@example.com"`，邮箱换成自己的邮箱。如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。  
2. 第2步：登陆GitHub，打开“Account settings”，“SSH and GPG Keys”页面：然后，点“New SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容  
**为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。**
当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。github上面的东西是公开看得到的，要想别人看不到，自己搭一个git服务器，公司内部开发必备。  

#### 添加远程库  
- 先有本地库，后有远程库的时候，如何关联远程库  
    - 在本地仓库运行命令`git remote add origin git@github.com:williamChann/learngit.git`可将本地库与github上面的远程仓库相关联。添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。  
    - 把本地库的所有内容推送到远程库上:`git push -u origin master`。由于远程库是空的，第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。  
    - 此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改。

#### 假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆｀git clone git地址｀

***  
## 分支管理  
#### 创建和合并分支  
1. 创建`dev`分支，然后切换到`dev`分支: `git checkout -b dev`(git checkout命令加上-b参数表示创建并切换 = `git branch dev` `git checkout dev`);
2. 用`git branch`命令查看当前分支:当前分支前面会标一个`*`号;
3. 可以在`dev`分支上面做修改提交，但是切回到`master`分支的时候，修改的内容会不见的，因为刚才的提交是在`dev`分支上，`master`的提交点没有变，这时候就可以合并两个分支了。
**总结**
- 查看分支:`git branch`
- 创建分支:`git branch <name>`
- 切换分支:`git checkout <name>`
- 创建和切换分支:`git checkout -b <name>`
- 合并某分支到当前分支:`git merge <name>`
- 删除分支:`git branch -d <name>`

#### 解决冲突
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
`git log --graph`和`git log --graph --pretty=oneline --abbrev-commit`都可以查看分支的合并情况

#### 分支管理策略
- 首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
- 平时干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；
- 团队开发时每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。  
而合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并。例如`git merge --no-ff -m "merge with no-ff" dev`，因为本次合并要创建一个新的commit，所以加上`-m`参数，把描述写进去。而fast forward合并就看不出来曾经做过合并。
	
#### Bug分支
- 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
- 当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场。
- 还有一种恢复方法是是用`git stash apply`恢复，但是恢复后，stash内容并不删除，需要用`git stash drop`来删除；
- [参考BUG分支](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137602359178794d966923e5c4134bc8bf98dfb03aea3000)

#### Feature分支
- 在实际的开发中，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。
- 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

#### 多人协作
**推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：**
-  `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于是否和小伙伴合作在上面开发。

***
## 标签管理
- 发布一个版本时，通常会先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本；
- 标签也是版本库的一个快照，但其实它就是指向某个commit的指针；
- 打上标签能够更加容易找到发布的版本。

#### 创建标签
- 默认情况：
    1. 切换到需要打标签的分支上：`git checkout <branch.name>`；
    2. 默认为`HEAD`，即对最新提交的commit打上标签：`git tag <name>`，如:`git tag v1.0`。
- 给历史提交的commit打标签：
    1. 找到历史提交的commit id：`git log --pretty==oneline --abbrev-commit`；
    2. 敲入命令，如`git tag v0.9 4ad987f`。
- 创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：`git tag -a v0.1 -m "version 0.1 released" aa07b47`,可以指定标签信息
- 用`git tag`查看标签时，标签不是按时间顺序列出，而是按字母排序的
- `git tag -s <tagname> -m "blablabla..."`可以用PGP签名标签；此用法必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错,如果报错，请参考GnuPG帮助文档配置Key。

#### 操作标签
- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

***
## 使用Github
- 在GitHub上，可以任意Fork开源仓库；
- 自己拥有Fork后的仓库的读写权限；
- 可以推送pull request给官方仓库来贡献代码。

***
## 码云：中国版的GitHub
- Git仓库，还集成了代码质量检测、项目演示等功能。对于团队协作开发，码云还提供了项目管理、代码托管、文档管理的服务，5人以下小团队免费。
- 在码云也要上传自己的SSH公钥；
- 一个本地库能既关联GitHub，又关联码云，git给远程库起的默认名称是origin，需要用不同的名称来标识不同的远程库。如GitHub可以用github名称、码云用gitee名称来标识远程库；
- 关联之后第一次推送要用`git push -u <branchName> master`命令。
- 如果要推送到GitHub，使用命令：`git push github master`。如果要推送到码云，使用命令：`git push gitee master`。

***
## 自定义Git
#### 忽略特殊文件
- 忽略文件的原则是：
    1. 忽略操作系统自动生成的文件，比如缩略图等；
    2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
    3. 忽略自己的带有敏感信息的配置文件，比如存放口令的配置文件。
**小结**
- 忽略某些文件时，需要编写.gitignore；
- .gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！
- [参考忽略特殊文件](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013758404317281e54b6f5375640abbb11e67be4cd49e0000)

#### 配置别名
- `git config --global alias.st status`中`--global`是全局参数，这命令在这台电脑的所有Git仓库下都有用，以后`git st`就能代表`git status`了，如此类推；
- `cat .git/config`可以查看Git的配置文件，别名在[alias]后面，可以通过`vi .git/config`命令修改文件来配置；
- 在当前用的户主目录下可以用`cat .gitconfig`命令查看本机的Git配置文件，可以通过`vi .gitconfig`命令修改文件来配置，如果改错了，可以删掉文件重新通过命令配置。

#### 搭建Git服务器
- 搭建Git服务器非常简单，通常10分钟即可完成；
- 要方便管理公钥，用Gitosis；
- 要像SVN那样变态地控制权限，用Gitolite。
**具体可按照[参考地址](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000)操作**





