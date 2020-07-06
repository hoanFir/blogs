🐾 git-in-work

🕘 2019.10.14 由 hoanfirst 编辑


**以下总结在工作中常用的一些git版本管理的场景和命令**


- *git clone工程并切换到指定分支*

```bash

# 默认 `HEAD->master`
git clone http://gitlab.company.com/company-project.git

# after clone...

# 场景一：如果远程分支中有想要的分支（一般是在开发完之后代码修复和完善阶段）

# A)拉到本地分支列表
git checkout dev_hong origin/dev_hong
# 查看当前分支信息
git branch -l

# B)拉到本地并新建和切换到本地同名分支
git checkout -b dev_hong origin/dev_hong
# 查看当前分支信息
git branch -l

# 场景二：如果远程没有想要的分支，在本地创建新分支并push（一般是面向新需求创建新分支进行开发）

# 基于当前分支创建新分支
git checkout -b dev_hong
# 在远程创建同名分支
git push origin dev_hong

# 场景三：直接在本地master分支上修改完提交（一般在工程下载处理完后即删的情况下）

# 在修改完代码后
git add .
git commit -m"description"
# 将更新内容提交到远程dev_hong分支
git push origin master:dev_hong

```


- *查看并设置当前项目仓库配置*

```bash

git config --local --list 

git config user.email "hongzhenpeng@company.com"
git config user.name "hongzhenpeng"

```


- *git log查看版本历史*

```bash

git log 
# 查看当前分支历史

git log --graph --all
# 查看所有分支历史

git log --graph master
git log --graph dev_hong
# 查看指定分支历史

```


 - *查看本地和远程分支信息*

```bash

git brach
# 查看本地分支列表

git branch -r
# 查看远程分支列表

```


- *删除分支*

```bash

# 删除本地分支
git branch -d dev_hong

# 注意 如果分支的代码有更新且没有合并到master，执行git branch -d无法删除，git会智能提示分支还有未合并的代码
# 可以强制删除
git branch -D dev_hong 
# 强制删除分支

# 删除远程分支
git push origin :dev_hong
git push origin --delete dev_hong

```

- *更新代码并push*

```bash

# 查看本地工作区和暂存区的区别
git status

# 将更新从工作区提交到暂存区
git add .

# 将更新从暂存区提交到本地仓库
git commit -m"description"

# 将远程最新代码拉取到本地
git pull origin dev

# 如果出现冲突手动解决

# 从本地仓库同步到远程仓库
git push origin dev_hong

```


- *git stash*

```bash

git stash save "save description"
git stash list
git stash show # 显示第一个stash的内容
git stash show stash@{1} # 显示第二个stash的内容
git stash pop
git stash pop stas@{1}
git stash clear

# 场景一：

# 有时候我们在开发过程中，想中途拉取团队其他成员已经提交的最新代码
# 此时执行git pull origin dev会报错，提示需要先将本地更新提交到暂存区才能拉取
# 但是，commit会生成版本记录，这是没必要的。此时就可以使用git stash

# 开发过程中...
git stash
git stash pop
git pull
# 继续开发...

git add .
git commit -m "description"
git push origin dev

```


- *reset*

```bash

# 撤销某个文件的暂存修改
git add <file>
git reset HEAD <file> 

# 撤销所有文件的暂存修改
git add .
git reset HEAD

# 撤销从暂存区到本地仓库的提交，指定commit_id回退到指定commit，commit_id可以通过git log查看
git log
git reset commit_id
git reset HEAD~1

git reset commit_id === git reset --mixed commit_id
git reset --soft commit_id
git reset --hard commit_id

```


- *合并分支*

```bash

# 将远程某个dev分支合并到master再提交
git clone ...
git checkout -b dev_hong origin/dev_hong
git checkout master
git merge dev_hong/git merge --no-ff dev_hong
# 处理冲突
git branch -d dev_hong #删除本地分支
git push origin :dev_hong #删除远程分支
git push origin master

# 合并本地分支到master并删除
git checkout master
git merge dev_hong
#处理冲突
git branch -d dev_hong

```



- *打tag*


```bash

git tag

git tag -l "v1.8.5*"

# 创建附注标签 / annotated tag
git tag -a v1.4 -m "my version 1.4"

git show v1.4



# 创建轻量标签 / lightweight tag

git tag v1.4-lw
# 运行 git show，不会看到额外的标签信息，只会显示出提交信息


```
