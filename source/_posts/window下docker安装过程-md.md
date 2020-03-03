---
title: window下docker安装过程
date: 2020-03-03 11:47:39
tags:
  - docker
categories:
  - docker  
---

### 下载
进入[官网](https://docker.com) 进行下载对应安装包，如下载缓慢需要代理翻墙。Windows下需在控制面板中开启Hyper。

安装完毕后命令行下执行 
```bash
docker --version
```
正常显示版本信息，说明安装成功。

### 修改国内镜像
Docker for Windows 下可直接点击图标setting，在docker engine中修改registry-mirrors的配置。

### 运行hello world
在命令行下执行
```bash
docker run hello-world
```
如下图显示，说明运行成功
![](https://dzh213.oss-cn-beijing.aliyuncs.com/blog/docker%20hello%20world.png)

同时，也可以查看当前存在的镜像
```bash
docker images
```
![](https://dzh213.oss-cn-beijing.aliyuncs.com/blog/docker%20images.png)
### 启动docker容器
* 启动一个交互式容器
```bash
docker run -i -t ubuntu:15.10 /bin/bash
```
这条命令会先拉取一个ubuntu 15.10版本的镜像，然后在容器内启动该镜像，并进行bash交互。
* 后台运行容器
```bash
docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
```
此时，控制台会返回该容器的唯一id，并在后台启动
可以通过命令 docker images 查看所有在运行的容器。

* 停止容器：
```bash
docker stop <容器id | 容器name>
```
### docker运行nginx
* 查看NGINX镜像
```bash
docker search nginx
```
![](https://dzh213.oss-cn-beijing.aliyuncs.com/blog/docker%20search%20nginx.png)
* 拉取nginx镜像
```bash
docker pull nginx:latest
```
![](https://dzh213.oss-cn-beijing.aliyuncs.com/blog/docker%20pull%20nginx.png)
* 后台运行docker内的nginx
```bash
docker run --name nginx-test -p 9000:80 -d nginx
```

>--name nginx-test：容器名称。
-p 9000:80： 端口进行映射，将本地 8080 端口映射到容器内部的 80 端口。
-d nginx： 设置容器在在后台一直运行。
>

* 本地浏览器访问
```bash
localhost:9000
```

### docker常用命令

>下载镜像：
`docker pull` <镜像名:tag>    如：下载centos镜像
`docker pull centos`
`docker pull sameersbn/redmine:latest`
查看已下载镜像
`docker images`
删除容器
`docker rm <容器名 or ID>`
查看容器日志
`docker logs -f <容器名 or ID>`
查看正在运行的容器
`docker ps`
查看所有的容器，包括已经停止的。
`docker ps -a`
删除所有容器
`docker rm $(docker ps -a -q)`
停止、启动、杀死指定容器
`docker start <容器名 or ID>` # 启动容器
`docker stop <容器名 or ID>` # 启动容器
`docker kill <容器名 or ID>` # 杀死容器
后台运行 `docker run -d <Other Parameters>`
`docker run -d -p 127.0.0.1:33301:22 centos6-ssh`
暴露端口： 一共有三种形式进行端口映射
`docker -p ip:hostPort:containerPort` # 映射指定地址的主机端口到容器端口
例如：docker -p 127.0.0.1:3306:3306 映射本机3306端口到容器的3306端口
`docker -p ip::containerPort` # 映射指定地址的任意可用端口到容器端口
例如：docker -p 127.0.0.1::3306 映射本机的随机可用端口到容器3306端口
`docer -p hostPort:containerPort` # 映射本机的指定端口到容器的指定端口
例如：docker -p 3306:3306 # 映射本机的3306端口到容器的3306端口
映射数据卷
`docker -v /home/data:/opt/data` # 这里/home/data 指的是宿主机的目录地址，后者则是容器的目录地址
>

***参考***
1. [https://www.runoob.com/docker/](https://www.runoob.com/docker/)
2. [https://www.jianshu.com/p/2dae7b13ce2f](https://www.jianshu.com/p/2dae7b13ce2f)