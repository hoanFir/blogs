🐾 git-in-work

🕘 2019.10.14 由 hoanfirst 编辑


**以下总结在工作中常用的一些git版本管理的场景和命令**

- *配置user config*

```bash

git config user.name  "Administrator"
git config user.email "root@example.com"

```

- *查看并设置git或当前仓库配置*

```bash

# 全局配置
git config --list

# 当前仓库配置
git config --local --list

```



- *关联远程仓库*

```bash

cd new-project
git init
git remote add origin url
git add .
git commit -m "init"
git push -u origin master

```

- *查看当前远程分支仓库地址*

```bash

git remote –v 

```

- *http与ssh互转*

```bash

git remote set-url origin https://xxxxxxx.git
git remote set-url origin git@git.xxxxx.git

```

- *clone工程并切换到指定分支*

```bash

git clone http://gitlab.company.com/project.git

# A：基于远程指定分支创建本地新分支
git checkout -b feature/xxx origin/feature/xxx

# B：本地基于develop创建新分支并推送到远端仓库
git checkout develop
git checkout -b feature/xxx
git push origin feature/xxx

# C：在远程创建指定新分支
git push origin master:feature/xxx

```

- *git log查看版本历史*

```bash

# 查看当前分支历史
git log 

# 查看所有分支历史
git log --graph --all

# 查看指定分支历史
git log --graph master
git log --graph feature/xxx

```

> 在vs code可以安装git graph插件更方便


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
git branch -d feature/xxx

# 注意 如果分支的代码有更新且没有进行merge，git会智能提示无法删除
# 强制删除
git branch -D feature/xxx

# 删除远程分支
git push origin :feature/xxx
git push origin --delete feature/xxx

```

- *更新代码并push*

```bash

# 更新代码

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
git push origin feature/xxx

```


- *git stash*

```bash

# 场景

# 有时候我们在开发过程中，想中途拉取团队其他成员已经提交到远程仓库的最新代码
# 此时执行git pull origin dev会报错，提示需要先将本地更新提交到暂存区才能拉取
# 但是，commit会生成版本记录，有时候是没必要的
# 这时，可以使用git stash先暂存本地更新

git stash
git pull
git stash pop

# 继续开发...

git add .
git commit -m "description"
git push origin dev


# 其他相关命令

git stash save "save description"
git stash list
git stash show # 默认显示第一个stash的内容
git stash show stash@{1} # 显示第二个stash的内容
git stash pop
git stash pop stas@{1}
git stash clear

```


- *合并分支*

```bash

# A：将远端某个dev分支合并到master再提交

git clone ...
git checkout -b feature/xxx origin/feature/xxx
git checkout master
git merge feature/xxx 或者 git merge --no-ff feature/xxx
git push origin master
# 处理冲突后，别忘了删除开发完的分支
#删除本地分支
git branch -d feature/xxx
#删除远程分支
git push origin :feature/xxx


# B：合并本地分支到master并删除

git checkout master
git merge feature/xxx
git push origin master
#处理冲突后，别忘了删除开发完的分支
git branch -d feature/xxx

# C：变基rebase合并
git checkout feature/xxx
git rebase master

```


- *打tag*

```bash

# A：创建附注标签 / annotated tag
git tag -a v1.4 -m "my version 1.4"
git show v1.4
git push origin v1.4


# B：创建轻量标签 / lightweight tag

git tag v1.4-lw
# 运行 git show，不会看到额外的标签信息，只会显示出提交信息
git show v1.4-lw
git push origin v1.4-lw

```




