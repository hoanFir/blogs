
## git submodule

当某个项目含有子模块项目，可以查看 `.gitmodules` 文件里的子模块配置信息，如

```

/.gitmodules

[submodule "src/pages/subproject"]
	path = src/pages/subproject
	url = http://git.xxx.com/xxx/xxx.git

```

子模块项目一般称为**子仓**，主项目一般称为**主仓**，虽然子仓是作为主仓的一个子文件夹，但只要开发者不在子仓的目录里，git 就不会跟踪它的内容。而且在主仓进行commit时，不会把子仓作为子文件夹来追踪，而是记为一个项目记录，该记录包含子仓最新的**commit id**。

总之，不管如何修改主仓和子仓代码，只要保证主仓的子模块记录能准确指向你想要的子仓commit记录即可。比如，在本地子仓做了修改后，可以发现主仓下对子仓的commit指向也有更新，这时就要把这个更新同步到主仓远端。

## 开发

```shell

# 在主项目初始化本地子模块配置文件
git submodule init

# 更新子模块所有代码
git submodule update

# 注意，也可以使用 git clone - - recursive xxx 来自动clone主项目并初始化、更新每一个子模块项目

# 有必要的话，执行如下明亮，这样就能通过 git status 查看子模块项目的更改摘要
git config status.submodulesummary 1

# 进入产生的子模块项目的目录
cd my-submodule

# 执行分支命令，可以看到没有指向master，而是指向一个“游离的HEAD”，这意味着子模块项目还没有本地工作分支来跟踪改动
git branch

# 切换到指定分支，跟踪子模块项目改动
git checkout develop

# 在修改了子模块项目代码之后，将代码push到远程，还需要再回到主项目，把子模块项目最新的commit id记录也同步到主项目远程仓库

```

## 注意事项

在主仓git pull之后，如果要更新子仓的代码，还需要git submodule update。
