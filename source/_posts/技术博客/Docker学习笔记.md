---
title: Docker学习笔记
tags: Docker
categories: 编程技能
date: 2023.7.2 21:17:00
---



# Docker学习笔记



## Dokcer安装

### 卸载原来的Docker

```bash
yum remove docker \
                   docker-client \
                   docker-client-latest \
                   docker-common \
                   docker-latest \
                   docker-latest-logrotate \
                   docker-logrotate \
                   docker-engine
```

### 下载Docker

```
yum install -y docker-ce
```

### 关闭防火墙

```
#关闭
systemctl stop firewalld
#禁止开机启动防火墙 
systemct1 disable firewalld
```

### 启动docker

```
systemctl start docker	#启动docker服务
systemctl stop docker	#停止docker服务
systemctl restart docker	#重启docker服务
```

### 配置阿里云仓库加速

```shell
mkdir -p /etc/docker

tee /etc/docker/daemon.json <<-'EOF'
{
	"registry-mirrors": ["https://kskdqwg1.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload
systemctl restart docker
```



## 常用命令

### 镜像

```dockerfile
# 查看当前所有镜像
doeker images
# 拉取镜像(下载镜像)
docker pull
# 推送镜像
docker push
# 保存镜像
docker save
# 加载镜像
docker load
# 删除镜像
docker rmi
```
### 容器
```
# 创建容器
docker run 
# 暂停容器
docker pause
# 启动容器
docker unpause
# 关闭容器
docker stop 
# 开启容器
docker start
# 查看所有容器的状态
docker ps
# 查看容器日志
docker logs
# 进入容器
docker exec
# 删除容器
docker rm 
```

**特殊的语句**

```
# 创建一个名为web的在nginx镜像为基础的容器
docker run --name web -p 8888:80 -d nginx 
# 进入容器并且可以进行基础的shell语句进行输入输出
docker exec -it web bash
```

### 数据卷

```
# 创建数据卷
docker volume create
# 查看数据卷的信息
docker volume inspect
# 列出所有的volume
docker volume ls
# 删除未使用的volume
docker volume prune
# 删除一个或多个指定的volume
docker volume rm
```

**样例**

```
# 创建基于nginx镜像名为web的容器
docker run --name web -p 8888:80 -d -v html:/usr/share/nginx/html nginx
```

> 注意： docker在创建容器的时候会自动创建数据卷。

### Dockerfile

![image-20230703010824707](https://s2.loli.net/2023/07/03/pFz7x4cM65wje9S.png)

```
# FROM 表示，继承自哪个 image，这里继承的是官方的 node image，那么该 image 文件生成的容器实例是可以运行 node 命令的。image 版本为 8.4
FROM node:8.4

# COPY 表示，把主机的 . 目录里的文件复制到 image 文件的 /app 目录里（当然，要先忽略掉 .dockerignore 里的文件）
COPY . /app

# WORKDIR 表示，接下来的工作目录是 /app
WORKDIR /app

# RUN 表示，先通过 npm install 安装所有的依赖后，再打包进 image 文件。
RUN ["npm", "install"]

# EXPOSE 表示把容器的 3000 端口暴露出来，以便和容器外部交互（本文后面会有例子）
EXPOSE 3000

# CMD 命令，表示在容器生成后执行的命令，这是和 RUN 的区别，一个在 image ，一个在 容器
CMD node 01.js
```



```
# 可以基于jdk8的环境来构建镜像
FROM java:8-alpine

ENTRYPOINT 8090
```

```
docker build -t jdk:1.0
```

### DockerCompose

## 概念

### 为什么要有docker



### 镜像

是一个只读的静态模板。它保存着容器需要的环境和应用的执行代码，可以把镜像看作容器的代码，当代码运行起来后就成了容器。镜像采用分层机制，每一层镜像都是只读的，但是可以将写数据的层通过联合文件系统附加到原有的镜像上。

### 容器

是一个运行时环境，它是一个镜像的运行状态，相对于静态的镜像而言。容器是镜像执行的动态表现，用户可以在容器中运行所想要的程序和服务，容器也不在乎你在什么样的环境下运行它。

### 数据卷



## 参考资料

[1]: https://www.bilibili.com/video/BV1s54y1n7Ev/?spm_id_from=333.337.search-card.all.click&amp;vd_source=0433279976fad48ef00e33a4e0bdab91	"B站 Docker 10分钟快速入门Docker 10分钟快速入门"
[2]: https://www.bilibili.com/video/BV1LQ4y127n4?p=43&amp;vd_source=0433279976fad48ef00e33a4e0bdab91	"B站 黑马程序员SpringCloud+RabbitMQ+Docker+Redis+搜索+分布式，系统详解springcloud微服务技术栈课程"

[3]: https://zhuanlan.zhihu.com/p/365455200	"最新、最全、最详细的 Docker 学习笔记总结（2021最新版）"

