## Git引入（一）

### 1. 理解 HEAD 版本指针

`Git` 有个叫 `HEAD` 的版本指针，当其指向某个版本，就会切换到哪个，所以Git切版本比SVN快

> 如：刚刚 add 的文件，会有一个提示 git reset HEAD <file>，就可以重置修改

> 如：通过 checkout 创建分支并切换，git checkout -b test，就是把HEAD版本指针切到分支，可以在分支上任意修改，若搞坏了只要把版本指针 checkout master，再删掉分支即可



### 2. 引入 Git 原因

> 熟悉编程的知道，我们在软件开发中，源代码其实是最重要的，那么对源代码的管理变得异常重要

- 为了防止代码的丢失，肯定本地机器与远程服务器都要存放一份，而且还需要有一套机制让本地可以跟远程同步；
- 经常是一个团队做同一个项目，都要对一份代码做更改，这个时候需要大家互不影响，又需要各自可以同步别人的代码；

- 我们开发的时候免不了有bug，有时候刚发布的功能就出现了严重的bug，这个时候需要紧急对代码进行还原；

- 随着我们版本迭代的功能越来越多，但是我们需要清楚的知道历史每一个版本的代码更改记录，甚至知道每个人历史提交代码的情况；

- ...

  以上这些都是`版本控制系统`能解决的问题。

  所以说，版本控制是一种记录若干文件内容变化，以便将来检查特定版本修订情况的系统，对于软件开发领域来，版本控制是最重要的一环，而 Git 毫无疑问是当下最流行、最好用的版本控制系统。



## git基础使用（二）

### 1. 创建本地仓库

```cmd
mkdir test
cd test
git init
# 本地仓库已建好，但还是一个空的仓库（empty Git repository）
# 当前目录下多了一个 .git 的目录，这个目录是 Git 来跟踪管理版本库的
# 千万不要手动修改 .git 目录里面的文件，不然改乱了，就把Git仓库给破坏了。
# 由于 .git 目录默认是隐藏的，用 ls -a 命令就可以看见
```



### 2. 将修改更新到本地仓库

```cmd
cd test
vim test.js
git status
# 此时没有任何修改，工作目录clean

git add test.js
# 暂存 test.js 的修改
git add ./ 
# 暂存全部修改
git status
# 会提示当前有暂存修改需要提交到仓库里

git commit -m "commit something"
# 提交到本地仓库
git status
# 会发现当前没有需要提交的修改，工作目录clean
```



### 3. add前、后和 commit 的撤销

```cmd
git checkout <file>...
## 撤销add前的文件修改

git reset HEAD <file>...
git reset HEAD
## 撤销add后的文件修改

git reset commit_id
## 撤销commit之后的文件修改
```



Tips：git checkout 撤销作用

git checkout 不仅可以用作切换branch、切换tag、切换到某次commit，甚至还有撤销功能。比如我们在一个分支开发一个小功能，写完一般需求更改，并且是大变化，之前写的代码完全用不了，这时候就需要撤销刚刚所写代码内容。注意，checkout 命令只能撤销还没有add进暂存区的文件

场景：假设在一个分支开发一个小功能，写完一般需求更改，并且是大变化，之前写的代码完全用不了，这时候就需要撤销刚刚所写代码内容



### 4. 本地ssh访问远程仓库

>当我们需要跟远程 Github 仓库进行clone、同步等操作，就需要通过 ssh 方式来访问远程仓库



1、先查看本地是否已经有 ssh 文件(用于使用ssh协议传输，该方式每次访问传输无须口令)

```cmd
cd ~/.ssh
# id_rsa，id_rsa.pub，known_hosts
# id_rsa.pub 将该公钥加到github的Setting里的ssh keys里
```



2. 假如本地还没有 ssh 文件

需要生成 rsa 密钥，自行谷歌。



3. 另外，为什么要使用ssh，不直接使用HTTPS

使用 https 除了速度慢，还有个最大的麻烦是每次访问传输都必须输入口令，不过也不是说我们只选择用ssh，因为在某些只开放 http 端口的公司内部就无法使用 ssh 协议而只能用 https



### 5. 查看配置信息 git config

在上一小节里我们在本地生成rsa密钥时，需要配置`用户名`和`邮箱`，指定 Git 提交用户的信息，`git config --global user.name "name"` 和`git config --global user.email "mail"`，它们是全局配置的，可以通过`git config --global --list`查看。



Git 配置有system级别、global级别/用户级别、local级别/当前仓库级别。这三级设置底层配置会覆盖顶层配置。

1. git config --system --list 查看系统配置
2. git config --global --list 查看当前用户配置
3. git config --local --list 查看当前仓库配置



Tips：

有些时候需要提交到不同的远程库，比如私人的 Github 和公司的 GitLab，这就不能使用配置global级的用户信息了，而需要在每个项目当前目录下配置特定的用户信息：`git config user.email ""`和`git config user.name ""`



### 6. 本地仓库关联远程仓库

```cmd
git remote add origin git@github.com:autoliuweijie/DeepLearning.git
```



### 7. 从远程仓库 clone 项目到本地

```cmd
git clone git@github.com:autoliuweijie/DeepLearning.git
```



### 8. 从本地仓库提交到远程仓库

```
git push origin master 
# 是把本地代码推到远程master分支
```



Tips：**提交到远程仓库是需要权限的**

1. 假如远程仓库是自己创建的

通过第4小节在个人远程仓库配置 ssh 就可以进行推送了

1. 假如远程仓库是项目负责任创建的

除了在团队远程仓库配置 ssh，**还需要项目负责人赋予权限才能参与项目源码的修改**



### 9. 从远程仓库拉取到本地仓库

```cmd
git pull origin master
# 如果别人提交代码到远程仓库，此时本地需要把远程仓库的最新代码拉取下来，保证两端代码的同步
```





### 10. 查看关联的远程仓库名字

```cmd
git config --local --list
#通过查看当前仓库配置

git remote get-url --all
#query all URLS

git remote get-url --push
#query push URLs rather than fetch URLs
```



### 11. 删除关联的远程仓库

```cmd
git remote remove origin
```



### 12. git log / git lg

1. git log 

查看项目master分支的版本记录



2. git lg

git lg 是基于`git log`的命令基础上进行命令修饰，使得展示可视化。在全局配置信息可以查看

```cmd
git config --global --list
# alias.lg=log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
```





## git其他概念（三）

### 1. fork

`fork` 其实就是从别人的项目源码仓库，创建一个新分支，新的项目前缀是你的用户名，此时你可以进行任何更改，也可以将这个 fork 后的库与本地某一个库进行关联同步开发；

当你更新之后，假如你觉得自己更新后的代码更佳，可以 `pull request`，让对方知道，你改进了他的项目，假如对方觉得你的更新的确帮他解决了问题，他可以选择接收并入主分支（否则并不会影响他的源主分支和代码）



### 2. git alias

操作命令使用缩写输入

```cmd
git	config	--global	alias.co	checkout		
git	config	--global	alias.ci	commit 
git	config	--global	alias.st	status 
git	config	--global	alias.br	branch
git	config	--global	alias.psm	'push	origin	master' 
git	config	--global	alias.plm	'pull	origin	master'
```

```
# 视图化git log
git	config	--global	alias.lg	"log	--graph	--pretty=format:'%Cred%h%Creset	-%C(yellow)% d%Creset	%s	%Cgreen(%cr)	%C(bold	blue)<%an>%Creset'	--abbrev-commit	--date=relative"
```



### 3. git diff

在提交之前确认代码**文件**做过哪些改动。



#### 比较当前文件和暂存区文件的diff

直接输入`git diff`只能比较当前文件和暂存区（还未执行git add的文件）文件的差异。



#### 比较两次commit提交之间的diff

```cmd
git diff $id1 $id2
```



#### 比较两个分支branch之间的diff

```cmd
git diff branch1..branch2
```



#### 比较暂存区和版本库之间的diff

```cmd
git diff --staged
```



## git合作开发及其冲突（三）

### 分支操作

~~~~cmd
新建分支
语法 : git branch branchName
例子 :git branch shanshan

查看分支 / 查看远端分支
语法 : git branch / git branch -a
例子 :git branch / git branch -a

切换分支
语法 :git checkout branchName 
例子 :git checkout shanshan

新建一个分支并且自动切换到该分支
git checkout -b test

将分支合并到主分支master，才进行发布
git checkout master
git merge test了（不出意外，因为有可能有冲突合并而失败）

删除分支
git branch -d test

有时候会删除失败，比如如果分支test的代码还没有合并到master，你执行git branch -d test是删除不了的
此时，可以强制删除分支
git branch -D test

远程
 (将本地分支的内容发布到远端)
语法 :git push origin branchName 
例子 :git push origin shanshan
# 假如出现错误：git error: src refspec dev does not match any
# 原因：git push时选取的本地分支进行推送，如果推送的分支在本地分支中不存在，就会产生报错src refspec dev does not match any，所以需要在本地创建一个相同分支

拉取远端分支的内容到本地 :git pull origin branchName
例子 :git pull origin shanshan

删除远端分支git push origin :
语法 :git push origin :branchName
例子 :git push origin :shanshan
~~~~



> git checkout 不仅可以用作切换分支，还可以用来切换tag或者切换到某次commit，甚至还有撤销作用



### 分支代码控制

**所以一般，我们需要在github上创建分支、git clone到本地、创建并转换分支、pull远程分支内容、add/commit/push origin branchname上传代码**



### git merge和git rebase

#### git merge

当我们在一个分支开发完一个功能之后，此时需要合并到主分支master上（我们知道还未合并就删除分支会无法删除，需要强制删除）

```cmd
git checkout master
git merge branchname
```



#### git rebase

其实`git rebase`也是合并的意思

区别：

https://blog.csdn.net/liuxiaoheng1992/article/details/79108233

理解成有两个书架，你需要把两个书架的书整理到一起 去，第一种做法是**merge**，比较粗鲁暴力，就直接腾出一块地方把另一个书架的书全部放进 去，虽然暴力，但是这种做法你可以知道哪些书是来自另一个书架的；第二种做法就是 rebase，他会把两个书架的书先进行比较，按照购书的时间来给他重新排序，然后重新放置好，这样做的好处就是合并之后的书架看起来很有逻辑，但是你很难清晰的知道哪些书来自哪个书架的。
只能说各有好处的，不同的团队根据不同的需要以及不同的习惯来选择就好。



### 合并的冲突及解决





## git tag控制（四）

### tag

客户端开发的时候经常有版本的概念，比如v1.0、v1.1之类的，不同的版本肯定对应不同的代码，所以我一般要给我们的代码加上**标签**，这样假设v1.1版本出了一个新bug，但是又不晓得v1.0是不是有这个bug，有了标签就可以顺利切换到v1.0的代码，重新打个包测试。



```cmd
# 新建标签
git tag v1.0
# 在当前代码状态下新建了一个 v1.0的标签
```

```cmd
# 查看历史tag记录
git tag
```



### git checkout切换tag

> git checkout 不仅可以用作切换分支，还可以用来切换tag或者切换到某次commit，甚至还有撤销作用

```cmd
git	checkout	v1.0 git	checkout	ffd9f2dd68f1eb21d36cee50dbdd504e95d9c8f7	
# 后面的一长串是commit_id，是每次com mit的SHA1值，可以根据	git	log	看到。
```



## git版本回滚（五）

#### 版本回退

首先，在本地回退版本，使用如下命令： 
`git reset --hard HEAD^` 
^的个数表示回退几个版本，^^表示回到上上个版本。

然后，强制push: 
`git push origin HEAD --force`