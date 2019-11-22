了解docker、image 镜像、container容器实例的关系。



启动docker服务

```bash
service docker start
```



## docker使用

### 重启docker服务

```bash
service docker restart
```



### docker列出镜像

```bash
docker image ls
```



### docker删除镜像

```bash
docker image rm [imageName]
```



### docker拉取镜像（通过中国官方镜像加速）

```bash
docker image pull registry.docker-cn.com/library/hello-world
或者直接
docker image pull hello-world（library可以省略）
```

注意，上面两条命令下载的hello-world镜像的名称是不同的，通过docker image ls可以查看



### 容器实例

docker container run 命令会从 image 文件生成一个正在运行的容器实例



### 运行镜像生成容器实例：hello-world

```bash
docker container run hello-world
```

注意，`docker container run`命令具有自动抓取 image 文件的功能。如果发现本地没有指定的 image 文件，就会从仓库自动抓取。因此，前面的`docker image pull`命令并不是必需的步骤



### 运行镜像生成容器实例并进入：ubuntu

```bash
docker container run -it ubuntu bash
```

上面的命令可以让我们在命令行中体验Ubuntu系统

-it 参数：进入容器

注意，ubuntu 容器实例提供的是服务，所以不会自动终止（hello-world容器实例会自动终止）



### 进入正在运行的容器实例

```bash
docker container run ubuntu
docker container ls
docker container exec -it [containerID] /bin/bash
```



### 终止容器实例

```bash
docker container ls
docker container ls --all（列出所有容器实例，包括终止运行的容器）
docker container kill [containID]
docker container stop [containID]
```

kill 和 stop的不同在于前者会强行立即终止，不做收尾清理工作。

注意，终止运行的容器实例依然会占据硬盘空间



### 启动终止的容器实例

```bash
docker container start [containerID]
```



### 删除容器实例

```bash
docker container rm [containerID]
```



### 拷贝容器实例中文件

```bash
docker container cp [containID]:[/path/to/file]
```



### 容器在后台运行时

```bash
docker container inspect wordpress
```



## 搭建wordpress

```bash
 docker container run \
  -d \
  --rm \
  --name wordpressdb \
  --env MYSQL_ROOT_PASSWORD=123456 \
  --env MYSQL_DATABASE=wordpress \
  mysql:5.7
```



```bash
docker container run \
  -d \
  -p 80:80 \
  --rm \
  --name wordpress \
  --env WORDPRESS_DB_PASSWORD=123456 \
  --link wordpressdb:mysql \
  --volume "$PWD/wordpress":/var/www/html \
  wordpress
```



直接通过服务器ip即可访问，开始搭建wordpress



若要修改访问地址：

```bash
 docker container stop wordpress（自动删除wordpress容器实例）
 docker container run \
  -d \
  -p 127.0.0.1:8080:80 \
  --rm \
  --name wordpress \
  --env WORDPRESS_DB_PASSWORD=123456 \
  --link wordpressdb:mysql \
  --volume "$PWD/wordpress":/var/www/html \
  wordpress
```

