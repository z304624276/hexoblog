---
title: git代理设置
date: 2026-04-30 11:48:55
categories: [杂谈]
tags: [git]
cover:  https://qncdn.178981.xyz/test/l9ZMR1ie.jpeg
---

## git代理设置

### 设置http代理

```
git config --global http.proxy http://127.0.0.1:10809
git config --global https.proxy https://127.0.0.1:10809
```

### 设置socks5代理

```
git config --global http.proxy socks5://127.0.0.1:10809
git config --global https.proxy socks5://127.0.0.1:10809
```


### 设置cmd代理
```
set http_proxy=http://127.0.0.1:10809
set https_proxy=https://127.0.0.1:10809

```

### 端口解释

10809为代理端口，根据实际情况修改比如我的是10809，v2ray的端口大部分是10809

### 取消代理

```
git config --global --unset http.proxy
```
