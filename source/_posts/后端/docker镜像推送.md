---
title: docker镜像推送
date: 2026-02-12 23:41:30
categories: [后端, Docker]
tags: [docker]
cover: https://img.178981.xyz/file/test/1772033179266_8c724dd10e24a011f912964c9faff04c.png
---

# docker镜像推送

docker笔记

## 1. 登录

```bash
docker login -u 用户名 -p 密码
```

## 2. 给镜像打标签

```bash
docker tag 镜像名称 用户名/仓库名:tag标签
```

## 3. 推送

```bash
docker push 用户名/仓库名:tag标签
```

## 4. 拉取

```bash
docker pull 用户名/仓库名:tag
```

## 总结
注意镜像打包以后，需要打标签才能推送，标签是用户名/仓库名:tag，tag可以自定义，不写默认是latest