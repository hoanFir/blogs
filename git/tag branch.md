
## 列出 tag

```

$ git tag
v1.0
v2.0


$ git tag -l "v1.8.5*"
v1.8.5
v1.8.5.1
v1.8.5.2
v1.8.5.3

```


### 创建 tag

git 支持两种 tag：lightweight tag, annotated tag.

- lightweight tag

只是一个引用，指向特定 commit。

```

$ git tag v1.0-lw

v1.0
v1.0-lw



$ git show v1.0-lw

commit xxx
Author: xxx
Date: xxx

  changed the version number
  
```

- annotated tag

存储在 git 数据库中，是一个包含完整信息的对象。其中包含创建 tag 的人的信息、commit 的信息和日期信息。

```

$ git tag -a v1.0 -m "my version 1.0"

-a create
-m info



$ git show v1.0

tag v1.4
Tagger: xxx
Date: xxx
my version 1.0

commit xxx
Author: xxx
Date: xxx

  changed the version number

```



## 主要作用

使用 git 过程中，频繁 `commit` 带来的结果是一长串密密麻麻的提交记录。一旦项目出现问题，需要检查某个节点的代码问题，就会有点头疼。虽然有 `commit message`，但还是有存在查找困难和描述不清的问题。

tag 提供一个语义化的形式，代替 commit 不清晰的描述，方便项目日后维护过程中的回溯。


