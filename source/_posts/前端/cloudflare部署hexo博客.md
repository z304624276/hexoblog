---
title: cloudflare部署hexo博客
date: 2026-02-19 11:06:41
categories: [前端]
tags: [hexo]
cover:  https://img.178981.xyz/file/test/FMAJURmQ.jpeg
---

# cloudflare部署hexo博客

## 1. 前言

最近在用hexo搭建博客，把项目放到github中，用cloudflare的pages来部署，netlify也一样
Vercel可以直接选hexo框架，cloudflare和netlify需要自己配置

## 2. 项目部署方法

在本地电脑创建项目，并安装hexo主题

```bash
hexo init myblog
cd myblog
npm install
npm install hexo-theme-next 
# 安装主题可以自己去找
hexo s  # 启动服务
```

## 3.通过git上传到github
创建github仓库，并把项目上传到github
```bash
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/yourname/yourname.github.io.git
git push -u origin master
```

## 4. 配置cloudflare

### 4.1 创建worker

进入cloudflare，点击workers，创建worker，选择pages，选择github，选择项目，点击create

### 4.2 配置pages的构建配置


构建命令填入
```
npx hexo generate
```
构建输出路径填入
```
public
```

### 等待部署完成
部署完成之后，就可以通过cloudflare的域名访问了
更新博客只需要在本地添加新的md文件，然后提交到github，然后等待cloudflare更新即可
