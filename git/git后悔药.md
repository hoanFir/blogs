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
