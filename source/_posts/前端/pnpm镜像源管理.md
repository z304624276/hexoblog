---
title: pnpm镜像源管理
date: 2026-05-10 11:49:53
categories: [前端,pnpm]
tags:    [pnpm]
cover: https://qncdn.178981.xyz/test/20260625005647501.png?imageslim
---

## pnpm镜像源操作指南

### npm安装
```bash
pnpm install -g pnpm
```
### windows单独安装
用PowerShell执行以下命令：
```bash
iwr https://get.pnpm.io/install.ps1 -useb | iex
```
不用npm安装的好处是，nvm切换node版本后，pnpm不需要重新再次安装。

### 查看当前镜像源

```bash
pnpm config get registry
```

### 设置镜像源

```bash
pnpm config set registry https://registry.npm.taobao.org/
```

### 删除镜像源

```bash
pnpm config delete registry
```

### 恢复默认镜像源

```bash
pnpm config set registry https://registry.npmjs.org/
```



### 镜像源列表
淘宝镜像源	    npm config set registry https://registry.npmmirror.com
阿里云镜像源	npm config set registry https://mirrors.aliyun.com/npm/
腾讯云镜像源	npm config set registry https://mirrors.cloud.tencent.com/npm/
华为云镜像源	npm config set registry https://mirrors.huaweicloud.com/repository/npm/
npm/
清华大学镜像源	npm config set registry https://mirrors.tuna.tsinghua.edu.cn/npm/
北京外国语大学镜像源	npm config set registry https://mirrors.bfsu.edu.cn/npm/
南京大学镜像源	npm config set registry https://mirrors.nju.edu.cn/npm/

以上都是AI推荐的镜像源，其他镜像源自行搜索。
