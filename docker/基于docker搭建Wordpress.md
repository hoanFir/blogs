
### 一、准备

1. 购买服务器 centos，如阿里云
2. 下载 SecureCRT 或 putty 或 ssh(mac)：登录远程服务器


### 二、开始 / 安装docker

docker 学习链接：

1. http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html

2. https://help.aliyun.com/knowledge_detail/40559.html?spm=a2c4g.11186623.2.12.2e507c2bUqFwCR

使用 yum 安装 yum-utils device-mapper-persistent-data lvm2源

```

sudo yum install -y yum-utils \
device-mapper-persistent-data \
lvm2

```

添加库

```

sudo yum-config-manager \
–add-repo \
https://download.docker.com/linux/centos/docker-ce.repo

```

安装 docker-ce docker-ce-cli containerd.io ，该步骤有可能安装出错，可试着通过命令yum clean all, yum makecache 解决 / 也可根据具体错误提示谷歌解决

```
sudo yum install docker-ce docker-ce-cli containerd.io

```


查看docker安装是否成功

```
docker version

```


### 三、修改权限和启动docker服务

由于 docker 需要用户具有 sudo 权限 为了避免每次都要输入 sudo ，建议将用户加入docker用户组

```
sudo usermod -aG docker $USER

```

启动docker服务
```
service docker start
```

查看所有镜像、容器实例

```
docker image ls
docker container ls
```


### 四、部署wordpress

获取 wordpress 和数据库镜像并生成容器实例

```

docker container run \
-d \
–rm \
–name wordpressdb \
–env MYSQL_ROOT_PASSWORD=123456 \
–env MYSQL_DATABASE=wordpress \
mysql:5.7
docker container run \
-d \
-p 80:80 \
–rm \
–name wordpress \
–env WORDPRESS_DB_PASSWORD=123456 \
–link wordpressdb:mysql \
–volume “$PWD/wordpress”:/var/www/html \
wordpress

```

到这一步基本实现可以在本地浏览器输入服务器 ip 进行访问。在这里我只列出具体步骤，相关原理自行根据教程理解 如：

docker 中国官方镜像加速
docker 镜像和容器实例的区别
docker 常用命令和参数理解
…
