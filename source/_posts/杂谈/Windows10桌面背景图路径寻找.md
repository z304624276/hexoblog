---
title: Windows10桌面背景图路径寻找
date: 2026-02-26 14:51:47
categories: [杂谈]
tags: [windows10]
cover:
---

## Windows10桌面背景图路径寻找

最近在换桌面壁纸，但是个性化里只显示最近几个壁纸，怕之前的壁纸被顶掉找不到了，所以记录一下路径。

寻找路径方法

按下 Win+R 打开运行，输入 regedit（或者直接搜索注册表编辑器），找到下面的路径

```
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Wallpapers
```
双击BackgroundHistoryPath0，就可以看到壁纸的路径了。

