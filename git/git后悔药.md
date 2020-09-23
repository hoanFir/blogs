

- *amend*

```bash

# 场景

# commit之后发现暂存区还有漏掉的文件，又不想生成两条commit记录

git commit -m"first commit"
git add .
git commit --amend

```

- *checkout*

```bash

# 撤销文件的工作区修改
git status
git checkout -- <file>

```



- *reset*

```bash

# 撤销某个文件的暂存修改
git add .
git status
git reset HEAD <file>

# 撤销所有文件的暂存修改
git add .
git status
git reset HEAD

# 撤销从暂存区到本地仓库的提交
# 指定commit_id回退，commit_id可以通过git log查看
git log
git reset commit_id

```

- *revert*


