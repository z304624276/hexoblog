---
title: node版本管理工具
date: 2026-05-10 11:26:14
categories: [后端, Node]
tags: [Node]
cover: 
---

## nvm

nvm 是一个 Node.js 版本管理工具，可以方便地安装、切换和管理多个 Node.js 版本。

### 安装

在 Windows 上安装 nvm，可以下载 nvm-windows 安装包并按照提示进行安装。

### 使用

安装 Node.js 版本：

```bash
nvm install <version>
```

例如，安装 Node.js 14.17.0 版本：

```bash
nvm install 14.17.0
```

切换 Node.js 版本：

```bash
nvm use <version>
```

例如，切换到 Node.js 14.17.0 版本：

```bash
nvm use 14.17.0
```

列出已安装的 Node.js 版本：

```bash
nvm ls
```

列出可用的 Node.js 版本：

```bash
nvm ls-remote
```

卸载 Node.js 版本：

```bash
nvm uninstall <version>
```

例如，卸载 Node.js 14.17.0 版本：

```bash
nvm uninstall 14.17.0
```

### 总结
nvm 是一个 Node.js 版本管理工具，可以方便地安装、切换和管理多个 Node.js 版本。




