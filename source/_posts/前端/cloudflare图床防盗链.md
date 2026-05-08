---
title: cloudflare图床防盗链
date: 2026-03-16 17:27:31
categories: [前端,cloudflare]
tags: [cloudflare, R2,图床]
cover:  https://qncdn.178981.xyz/test/fq6LUKRR.jpeg
---

## 前言

最近开通了cloudflare，并且开通了R2对象存储服务，用来做图床。防止图片被人盗用，所以需要设置防盗链。

### R2对象存储

R2是Cloudflare推出的一款对象存储服务，可以用来存储图片、视频等文件，并且有CDN加速功能，非常适合用来做图床。

R2的官方文档：https://developers.cloudflare.com/r2/

### 防盗链

通过设置域名的安全规则，可以设置防盗链，防止图片被别人盗用。


1.创建一个“跳过”规则 (Skip Rule) - 优先级最高
表达式：(http.host eq "img.yourdomain.com") and (http.referer contains "your-gallery.com" or http.referer eq "")
操作：Skip (跳过)
解释：如果请求是发往 img.yourdomain.com，并且来源是 your-gallery.com 或者是直接访问（空 Referer），则跳过后续所有规则，直接允许请求通过。
2.保留一个通用的“阻止”规则 (Block Rule) - 优先级较低
表达式：http.host eq "img.yourdomain.com"
操作：Block (阻止)
解释：如果请求是发往 img.yourdomain.com，但不满足上面“跳过”规则的条件（即，来源是除了 your-gallery.com 之外的任何其他网站），则阻止该请求。
最终逻辑：
浏览器从 your-gallery.com 访问 img.yourdomain.com 的图片？-> 匹配第一条“跳过”规则 -> 允许。
用户直接在浏览器地址栏访问 img.yourdomain.com/some-pic.jpg？-> 匹配第一条“跳过”规则 -> 允许。
evil-website.com 在其页面上 <img src="https://img.yourdomain.com/some-pic.jpg">？-> 不匹配第一条“跳过”规则 -> 执行第二条“阻止”规则 -> 阻止。
这样设置后，用户访问能正常，同时有效防止其他网站的盗链。

### 域名关闭图片访问

用上面的方法，会导致后端面无法进入，所以需要重新绑定一个域名，并关闭图片访问，用来做后端管理。

方法一：使用 Page Rules (推荐，配置简单)
Page Rules 是一个强大且直观的工具，可以针对特定 URL 模式应用行为。
登录 Cloudflare 控制面板。
进入您的 B 域名 admin.gallery.com。
导航到 “规则” > “页面规则”。
创建新规则。
设置 URL 模式：
输入 admin.gallery.com/*。这个 * 通配符表示匹配该域名下的所有路径。
或者，如果您知道图片通常存放在特定目录，比如 /images/，可以更精确地设置为 admin.gallery.com/images/*。
设置设置：
点击“添加设置”。
选择 “始终在线” 并设置为 “关闭”。这个设置本身不阻止访问，但我们可以结合其他设置。
最关键的是：选择 “缓存级别” 并设置为 “绕过”。这可以防止静态资源被缓存。
但最好的方式是：选择 “转发 URL”。然后设置一个动作：
状态代码：选择 301 (永久重定向) 或 302 (临时重定向)。
转发 URL：指向一个无效地址或您自己的错误页面，例如 https://www.google.com 或一个自定义的 https://admin.gallery.com/access-denied.html。
或者，最直接的方法是选择 “最小 TLS 版本” 并设置一个极高的版本，但这可能导致奇怪的错误。
最标准的做法是：使用 “自定义响应” (Custom Response)。如果您的 Cloudflare 计划支持此功能（付费计划）：
选择 “自定义响应”。
状态代码：403 (Forbidden)。
内容：Access to images is forbidden on this domain. (或其他您想显示的内容)。
保存并部署。

方法二：使用 Transform Rules (更灵活，需要 Worker)
如果您发现 Page Rules 的选项不够用，或者需要更复杂的逻辑，可以使用 Transform Rules。
导航到 “规则” > “转换规则” > “HTTP 响应标头”。
创建规则。
设置条件：匹配的请求 -> http.host -> 包含 -> admin.gallery.com。
设置操作：添加一个 HTTP 响应标头，例如 X-Robots-Tag，值为 noindex, nofollow。但这只是告诉搜索引擎不要索引，对直接访问无效。
实际上，Transform Rules 更多用于修改请求/响应头，而不是直接阻止或重定向。要实现阻止，最常用的方法还是 Page Rules 中的 “自定义响应” 或 “转发 URL”。
最佳实践总结：
A域名 img.public.com：使用您已经配置好的 防火墙规则 (Firewall Rules) 通过 Referer 头来实现防盗链。
B域名 admin.gallery.com：使用 页面规则 (Page Rules) 中的 “自定义响应” (Custom Response) 或 “转发 URL” (Forwarding URL) 功能。当有人试图访问 admin.gallery.com 下的任何资源时，直接返回 403 Forbidden 错误或重定向到其他地方。


### 总结
图床防盗链的方法有很多，这里只介绍一种，通过 Cloudflare 的页面规则来实现。防止被人刷R2存储桶，但是不能防止用户直接访问图片。