---
title: "Docker"
date: 2023-01-11T10:55:27+08:00
categories:
  - Note
  - daily
tags:
  - Docker
---


- Docker概述
- Docker安装
- Docker命令
   - 镜像
   - 容器
   - 操作
   - ……
- Docker镜像
- 容器数据卷
- DockerFile
- Docker网络原理
- Docker Compose
- Docker Swarm
- CI/CD Jenkins
> Docker 概述

## Docker 为什么会出现

代码+环境 一起发布
集装箱 隔离
![logo.png](docker-logo.png)
## Docker 的历史

dotCloud 开源后 -> Docker


传统模式（虚拟技术）
![traditional.png](traditional.png)

Docker
![docker-mod.png](docker-mod.png)

虚拟技术和Docker的不同

- 传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件
- 容器内的应用直接运行在 宿主机的内核上，容器是自己没有内核的，也没有虚拟硬件
- 每个容器内都是互相隔离的，互不影响

> Docker 的安装

## Docker 的基本组成

![image.png](composition.png)
镜像（image）：
 Docker镜像就好比一个模版来创建容器服务
容器（container）：
服务运行的载体  
可以 启动， 停止，删除等
仓库（repository）：
存放镜像的地方
仓库分为共有仓库和私有仓库

# 常用命令
```shell
docker run 镜像名

docker images  -- 查看所有的镜像
❯ docker images
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    feb5d9fea6a5   3 weeks ago   13.3kB


```
![image.png](image.png)

> Docker 是怎么工作的

Docker 是一个 Client-Server 结构的系统，Docker的守护进程运行在主机上，通过Socket从客户端访问

DockerServer接收到Docker-Client的指令，就会执行这个指令


> Docker 的常用命令

## 帮助命令
```shell
docker version            -- 版本信息
docker info               -- docker的系统信息，包括镜像和容器的数量
docker 命令 --help        --# 万能命令  查询帮助
```
帮助文档地址： [https://docs.docker.com/engine/reference/run/](https://docs.docker.com/engine/reference/run/)

## 镜像命令
```shell
docker images              -- 查看所有本地主机上的镜像

❯ docker images
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    feb5d9fea6a5   3 weeks ago   13.3kB

# 解释
REPOSITORY  镜像的仓库源
TAG         镜像的标签
IMAGE ID    镜像的id
CREATED     镜像的创建时间
SIZE        镜像的大小

#可选项
-a， --all    # 列出所有镜像
-q， --quiet  # 只显示镜像的id
```
```shell
docker search             -- 搜索镜像

# 可选项 
-f  
```
![image.png](docker-search.png)

```shell
docker pull   -- 下载

docker pull mysql 【版本】
```
```shell
docker rmi   -- 删除镜像
docker rmi -f id           #删除容器
docker rmi -f id id id     #删除多个容器
docker rmi -f $(docker images -aq)  #删除全部的容器
```

## 容器的命令
下载镜像并启动
```shell
docker pull centos:7   --下载centos7

docker run -it centos:7 /bin/bash    -- 运行
# 参数说明
--name="Name"     容器名字 tomcat01 tomcat02 用来区分容器
-d                后台方式运行
-it               使用交互方式运行，进入容器查看内容
-p                制定容器的端口  -p 8080:8080
		-p  ip:主机端口:容器端口
    -p  主机端口:容器端口（常用）
    -p  容器
-P                随机制定端口


查看命令
docker ps       --查看当前运行的镜像
docker ps -a    --查看镜像运行历史
docker ps -n=?  --显示最近创建的容器 n为最近几个就写几个
docker ps -q    --只显示容器的编号

退出命令
exit   								 # 直接停止并退出
ctrl + p + q           # 不停止退出

删除命令
docker rm 容器id                 --删除制定容器
docker rm -f $(docker ps -aq)    --删除全部容器
docker pa -a -q｜xargs docker rm   --删除全部容器


启动和停止的操作
docker start 容器id
docker restart 容器id
docker stop  容器id
docker kill  容器id
```
## 常用的其他命令
```shell
后台启动容器
docker run -d 镜像名




```
![image.png](docker-ps.png)
注意： docker run -d centos 后 使用docker ps 发现容器直接停止了
原因： -d是容器后台运行，必须要有前台进程，docker 发现并没有前台进程，所以就自动停止了
例如： nginx 容器启动后，发现自己没有提供服务，就会停止

```shell
docker logs            -- 查看日志
例如：
docker logs -tf --tail 10 容器id     --查看id的10条日志
```
```shell
docker top 容器ID                 --查看容器进程
docker inspect 容器ID             --查看容器信息
```
### 进入当前运行的容器
```shell
docker exec -it 容器ID /bin/bash       --进入容器（进入容器后开一个新的终端）
docker attach 容器ID                   --进入容器（进入正在执行命令的终端，不会打开新的终端）

exec打开的容器，exit后不会关闭，docker ps可以查到，attach打开的终端会关闭，docker ps查不到

```
### 从容器内拷贝文件到主机上
```shell
docker cp 容器id:容器内路径 目的的主机路径      -- 复制文件
```
![image.png](docker-cp.png)
拷贝是一个手动过程，可以使用数据卷的技术进行自动同步

```shell
docker run -it --rm 镜像名          -- 用完即删除

docker stats                        -- 查看cpu的状态
```
## Commit 镜像
```shell
docker commit    提交容器成为一个新的版本
#命令和git commit 类似

docker commit -m='描述信息' -a='作者' 容器id 目标镜像名:[TAG]

# 镜像会被暂时 commit 到本地
```

# 容器数据卷
## 什么是容器数据卷
主要是做数据和容器的分离，做数据的持久化，这样就可以共享及保存数据

### 使用数据卷
> 方式一： 直接通过命令来挂载 -v

```shell
# 例子
docker run -it -v 主机目录:容器目录 镜像名 /bin/bash
```
启动后可以通过`docker inspect 容器ID`来查看挂载信息
![image.png](docker-inspect.png)

实战
```shell
# 获取镜像
docker pull mysql:5.7

# 运行容器， 需要做数据挂载  # 安装启动mysql ，需要配置密码
# 官方测试： docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

# 参数说明
-d 后台运行
-p 端口映射
-v 卷挂载
-e 环境配置
--name 容器名字
docker run -d -p 3310:3306 -v ~/Code/temp/docker-demo/mysql/conf:/etc/mysql/conf.d -v ~/Code/temp/docker-demo/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql-demo-02 mysql:5.7

```
### 具名挂载和匿名挂载
```shell
docker volume --help              --查看volume的参数

docker run -d -P --name nginx-demo -v etc/nginx nginx   -- 通过匿名方式启一个nginx

docker volume ls
```
![image.png](docker-volume.png)
这里发现，这个就是匿名挂载，我们在`-v`的时候只写了容器内部命令，没有写容器外部命令

```shell
通过 `-v 卷名:容器内路径` 这种方式挂载的就是具名挂载

可以通过
docker volume inspect 卷名      来显示详细的挂载信息
```
如何区分是匿名挂载还是具名挂载

- -v 容器内路径                              #匿名挂载
- -v 卷名(不以`/`开头):容器内路径      #具名挂载
- -v /宿主机路径:容器内路径             #制定路径挂载

### 拓展知识
```shell
# 通过 -v 容器内路径:ro :rw 改变文件读写权限
ro  readonly  # 只读
rw  readwrite # 读写

例如：
docker run -d -P --name nginx-demo -v etc/nginx:ro nginx
docker run -d -P --name nginx-demo -v etc/nginx:rw nginx

ro只能通过宿主机来操作 容器内部不可操作

```

### 初识Dockerfile
Dockerfile 就是用来构建 docker 镜像的构建文件
```shell
创建一个dockerfile文件 名字可以随机
# 文件中的内容  指令（大写）   参数
FROM centos

VOLUME ["volume01","volume02"]

CMD echo "--------end-----------"
CMD /bin/bash
```
## 数据卷容器

一个容器启动的时候使用 --volume-from 命令来指定挂载的继承
```shell
# 示例
docker run -it --name docker02 --volumes-from docker01 centos

# 其中docker02中的挂载点和docker01的挂载点是共用的   
```
## DockerFile
dockerfile 是用来构建docker镜像的文件

构建步骤

1. 编写一个dockerfile文件
2. docker build 构建成为一个镜像
3. docker run 运行镜像
4. docker push 发布镜像（DockerHub，阿里云镜像仓库）
#### 基础知识：

1. 每个保留关键字（指令）都必须是大写字母
2. 执行从上到下的顺序执行
3. #表示注释
4. 每一个指令都会创建提交一个新的镜像层，并提交


dockerfile是面向开发的

### DockerFile的指令
```shell
FROM                             #基础镜像    例如：centos  ubuntu
MAINTAINER                       #镜像是谁写的 姓名+邮箱
RUN                              #镜像构建的时候需要运行的命令
ADD                              #步骤，比如需要有tomcat，ADD tomcat ADD命令可以自动解压
WORKDIR                          #镜像的工作目录
VOLUME                           #挂载卷
EXPOSE                           #暴露端口（指定对外接口）
CMD                              # 制定这个容器启动的时候要运行的命令 ，会被覆盖
ENTERYPOINT                      # 指定容器启动时运行的命令 ， 可以追加命令
ONBUILD                          #当构建一个被继承.DockerFile 这个时候就会运行ONBUILD的指令，触发指令
COPY                             # 类似ADD，将我们的文件拷贝到镜像中
ENV                              # 构建的时候设置环境变量
```
Docker Hub中99%的镜像都是从这个基础镜像过来的 `FROM scratch`

示例：
文件名： mydockerfile-centos
```shell
FROM centos
MAINTAINER nanpangyou<xxxx@mail.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "-----------end-------------"
CMD /bin/bash
```
使用命令`docker build -f mydockerfile-centos -t mycentos:0.1 .` 在当前目录`.`使用文件（-f）mydockerfile-centos 来构建生成镜像（-t即tag）mycentos:0.1版本的镜像
![image.png](docker-build.png)

docker history 镜像ID   可以查看镜像历史

### Docker 流程

![image.png](docker-flow.png)
