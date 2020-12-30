
- 当某个项目含有子模块项目，可以查看 `.gitmodules` 文件里的子模块配置信息

```

./.gitmodules

[submodule "src/pages/subproject"]
	path = src/pages/subproject
	url = http://git.xxx.com/xxx/xxx.git

```

子模块项目（子仓），虽然是作为当前主项目（主仓）的一个子目录，但只要开发者不在子模块的目录里，git 就不会跟踪它的内容，而且在主项目进行commit时不会把子模块记录成子文件夹来追踪，而是记为一项目录记录，该记录包含子模块项目的**commit id**。

- 在主项目和子模块项目进行开发

1）在主项目执行 `git submodule init` 用来初始化本地配置文件，以及运行 `git submodule update` 抓取所有数据。注意，也可以使用 `git clone - - recursive xxx` 来自动clone主项目并初始化、更新每一个子模块项目。有必要可以继续执行 `git config status.submodulesummary 1` 后，这样就能通过 git status 查看子模块项目的更改摘要

2）进入产生的子模块项目的目录，执行`git branch`，可以看到没有指向master，而是指向一个“游离/分离的HEAD”，这意味着子仓还没有本地工作分支跟踪改动。需要`git checkout develop`切换到指定分支

3）修改了子模块项目代码

6）在子模块项目将代码push到远程后，需要再回到主项目，执行commit和push，更新主项目的子仓commit id记录

注意，在主项目执行git pull，会拉取主项目的代码更改，也会拉取子模块项目的记录（不是子模块项目）的更改。这意味着只 pull 不会更新子模块项目，还需要运行git submodule update 进行更新
