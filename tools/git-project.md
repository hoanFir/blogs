clone 并创建本地和远程分支

```cmd
git clone http://gitlab.shandianshua.com/html/longmao-tags-ui.git
git log

# A 如果远程分支有想要的分支
git checkout dev_hong origin/dev_hong
# 把远程分支迁到本地
git checkout dev_hong -b origin/dev_hong
# 把远程分支迁到本地，并切换到该分支

# B 如果远程没有想要的分支，同时创建本地分支和远程分支
git checkout -b dev_hong
# 本地库版本指针HEAD->dev_hong
git push origin dev_hong
# 在远程创建dev_hong分支
# 注意，如果只是修改部分或短期添加功能，在本地分支上修改完合并到本地master就行，尽量不在远程添加分支

# C 如果直接基于master分支修改再提交一个分支
git push origin master:dev_xxx
```


查看并设置当前项目仓库配置

```cmd
git config --local --list 

git config user.email "hongzhenpeng@longmaosoft.com"
git config user.name "hongzhenpeng"
```



删除分支

```cmd
git branch -d dev_hong
# 删除本地分支
# 注意 如果分支的代码还没有合并到master，执行git branch -d是无法删除的，git会智能提示分支还有未合并的代码

git branch -D dev_hong 
# 强制删除分支

git push origin :dev_hong
git push origin --delete dev_hong
# 删除远程分支
```



查看本地和远程分支信息

```cmd
git brach
# 查看本地分支列表

git branch -r
# 查看远程分支列表
```



基于远程某个dev分支进行修改

```cmd
git clone ...
git checkout -b dev origin/dev

# 没有其他人协作开发
git add .
git commit -m ""
git push origin dev

# 有其他人写协作开发并有更新
git stash
git stash pop
git pull
git add .
git commit -m ""
git push origin dev
```



update and reset

```cmd
git status

git add <file>
git reset HEAD <file> 
# 撤销某个文件的暂存修改

git add .
git reset HEAD
# 撤销所有文件的暂存修改

git commit -m ""
git reset commit_id
# 撤销提交，commit_id可以通过git log查看
```



git log
```cmd
git log 
# 查看当前分支历史

git log --graph --all
# 查看所有分支历史

git log --graph master
git log --graph dev_hong
# 查看某一个分支历史
```



将远程某个dev分支合并到master再提交

```cmd
git checkout -b dev_zhp origin/dev_zhp
git checkout master
git merge --no-ff dev_zhp
git push origin master
```



合并本地分支到master

```cmd
# 合并本地分支到master
git checkout master
git merge dev_hong

# 假如出现冲突 conflicts
# 需要手动解决冲突

# 把dev_hong分支合并到master之后
# A 删除dev_hong分支，将本地master分支push到远程
```