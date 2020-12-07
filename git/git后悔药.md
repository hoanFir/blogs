
# 一、常用


- commit之后发现暂存区还有漏掉的文件，又不想生成两条commit记录

```bash

git commit -m"first commit"
git add .
git commit --amend

```

- merge之后想要撤销

```bash

merge时有冲突，回退到merge之前
git merge -- abort

```


# 二、回滚

git需要回滚的场景大概分为四大类：工作区、暂存区、本地仓库、远程仓库。

## 1. 工作区

### 1.1 撤销文件变更

```bash

# 撤销部分文件的工作区修改
git checkout -- <file> <file>
git restore <file> <file>【Git 2.23版本以上支持】

# 撤销所有文件的工作区修改（已受版本控制的文件，即不包括新增文件）
git checkout -- .
git restore .【Git 2.23版本以上支持】

# 撤销所有文件的工作区修改（包括新增文件）
git clean -df
  -d 删除未跟踪目录以及目录下的文件
  -f 如果git config下的clean.requireForce不为false，那么clean操作需要-f来强制执行

```

### 1.2 将文件回滚到指定版本

```bash

git checkout <commit-id> -- <file> <file>

```

## 2. 暂存区

### 2.1 撤销文件变更

```bash

# 撤销部分文件的暂存修改
git add .
git reset -- <file> <file>
git restore --staged <file> <file> 【Git 2.23版本以上支持】

# 撤销所有文件的暂存修改
git add .
git reset -- .
git restore --staged .【Git 2.23版本以上支持】

```

## 3. 本地仓库

**适用于：提交到本地仓库且未推送到远程仓库。**

### 3.1 回滚指定版本

```bash

# 撤销从暂存区到本地仓库的提交，且变更会恢复到工作区
# 指定commit_id回退，commit_id可以通过git log查看
git log --oneline
git reset <commit_id>

# 本地提交了bad commits，想要将版本回退到指定版本
git log --oneline
git reset --hard <commit_id>
  --hard 彻底回退到某个版本，工作区也会变为回退版本的内容。所以强烈建议在reset --hard之前，先把本地未提交的修改stash

# 本地的变更不需要了，需要将本地分支更新同步远程仓库的最新，可以直接回滚到远程分之的版本即可
git reset --hard origin/分支

```

## 4. 远程仓库

建议：回滚远程仓库的版本，都应立即联系仓库管理员锁库（设置保护分支），减少在错误的版本后提交的变更。

### 4.1 撤销某次提交

```bash

# 回滚普通commit（非merge commit）
# revert会撤销指定commit提交，但是通过生成一个新的commit来实现
# 比如，提交了a，b，c三次，b是bad commit，执行下行命令后，commit记录变为a，b，c，d，b'
git revert <commit-id>

```

