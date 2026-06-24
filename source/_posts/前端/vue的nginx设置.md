---
title: vue的nginx设置
date: 2026-06-13 11:42:00
categories: [前端]
tags: [nginx]
cover: https://qncdn.178981.xyz/test/20260625005740689.png?imageslim 
---

# vue的nginx设置

## 1. vue项目打包

```bash
npm run build
```

## 2. 打包后文件

在项目根目录下会生成一个`dist`文件夹，这个文件夹就是打包后的文件

## 3. 配置nginx

```bash
# 设置反代
location /api/ {
    proxy_pass https://xxx.com/;
    proxy_set_header Host xxx.com;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    
    # 注意：不需要 rewrite，proxy_pass 末尾带 / 会自动拼接
}
# 先处理图片等静态资源（更具体的路径放前面）


# 最后再处理前端路由
location / {
    try_files $uri $uri/ /index.html;
}

```