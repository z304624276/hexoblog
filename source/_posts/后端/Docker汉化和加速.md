---
title: Docker Desktop汉化和加速
date: 2026-03-18 22:03:12
categories: [后端,docker]
tags:   [docker]
cover:  https://img.178981.xyz/file/test/960Btz0s.jpeg
---


## Docker Desktop汉化和加速

今天Docker Desktop不小心删除了，所以重新装了，又要重新汉化和设置加速，免得忘记了，记录一下

### Docker Desktop汉化

1. 下载汉化包

   [下载地址](https://github.com/asxez/DDCS)

按照文档安装依赖，然后执行汉化命令即可，执行汉化期间不要启动Docker Desktop

### Docker Desktop加速

在Docker Desktop设置中找到Docker Engine，添加如下配置

```json
{
  "registry-mirrors": [
    "https://docker.1ms.run/",
    "https://docker.xuanyuan.me"
  ]
}
```

重启Docker Desktop即可

### 总结

Docker Desktop的汉化和加速，可以提升使用体验，建议都设置一下，这样使用起来会更顺手