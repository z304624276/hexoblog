---
title: pocketbase镜像打包方法
date: 2026-02-10 01:22:05
categories: [后端, Sqlite]
tags: [pocketbase]
cover: https://img.tanlugeek.dpdns.org/i/2026/02/13/698f4566e7ec0.webp
---

# pocketbase镜像打包方法
记录一下pocketbase镜像打包方法，免得忘记了
## 首先创建文件Dockerfile，内容如下
```Dockerfile
FROM alpine:latest

ARG PB_VERSION=0.36.2

RUN apk add --no-cache \
    unzip \
    ca-certificates \
    # this is needed only if you want to use scp to copy later your pb_data locally
    openssh

# download and unzip PocketBase
ADD https://github.com/pocketbase/pocketbase/releases/download/v${PB_VERSION}/pocketbase_${PB_VERSION}_linux_amd64.zip /tmp/pb.zip
RUN unzip /tmp/pb.zip -d /pb/

# uncomment to copy the local pb_migrations dir into the container
# COPY ./pb_migrations /pb/pb_migrations

# uncomment to copy the local pb_hooks dir into the container
# COPY ./pb_hooks /pb/pb_hooks

EXPOSE 8080

# start PocketBase
CMD ["/pb/pocketbase", "serve", "--http=0.0.0.0:8080"]

```

## 打包方法：
不写版本号的方法
```
docker build -t my-pocketbase-image .
```
写版本号的方法
```
docker build -t pocketbase:${PB_VERSION} .
```
用pocketbase:版本号，需要先执行

```
export PB_VERSION=0.36.2
```

## 需要注意的地方

PB_VERSION是版本号，需要什么版本就填写什么版本号，如果不知道版本号，可以到github上或者官网查看，官网地址是https://pocketbase.io/

http=0.0.0.0:8080的端口号是docker容器的端口号

## 结束
镜像制作完成后创建容器即可