---
title: nat小鸡使用记录
date: 2026-02-25 00:36:06
categories: [杂谈]
tags: [nat]
cover:
---

# nat小鸡使用记录以免忘记
小鸡有很多种使用面板，这里介绍两种面板，适合小鸡使用

## 1.ArgoSBX小钢炮脚本

网址： https://yonggekkk.github.io/argosbx/

主要介绍：

Hysteria2 需要使用UDP端口

其他都用tcp端口

重点介绍一下Vmess-ws端口

套CF后，可以使用80系端口和443端口

生成一键脚本命令后，复制Vmess到工具中

cf托管好域名，在SSL/TLS概述中，设置为灵活模式

规则概述中

Origin Rules中，添加规则，

自定义筛选表达式
主机名 等于 解析的域名

目标端口

重写到Vmess-ws的端口号


保存后在工具的编辑中设置编辑服务器

host设置为解析的域名

端口用80系端口，地址填CF优选的ip

然后就可以使用了

443端口同理，只是端口只能用443

CF优选网址： https://api.uouin.com/cloudflare.html

### 主要是给自己记录，怕忘记了，看不懂的，请勿使用

## 2.x-ui
安装命令
```
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```
访问：ip+外部端口

设置两个tcp端口

一个用来做面板的端口，一个做ws的端口

登陆面板后添加入站，只需要设置端口和传输（ws）

然后复制链接就行了

套CF

解析域名

设置回源规则 
Origin Rules中，添加规则，

自定义筛选表达式
主机名 等于 解析的域名

目标端口

重写到Vmess-ws的端口号

节点开启tls 
TLS源服务器创建证书

公钥文件路径：/root/cert/xxxx.crt
密钥文件路径：/root/cert/xxxx.key

申请cf的15年证书

SSL/TLS—概述—SSL/TLS 加密更改为完全或者完全（严格）

边缘证书 始终使用 HTTPS打开

网络 WebSockets打开

测试节点

## 其他脚本

一、Hysteria 2 一键安装脚本
```
wget -N --no-check-certificate https://raw.githubusercontent.com/flame1ce/hysteria2-install/main/hysteria2-install-main/hy2/hysteria.sh && bash hysteria.sh
```
二、安装X-UI面板
```
bash <(curl -Ls https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install.sh)
```
## 总结

1.套CF只需要设置回源规则，两种模式都一样，不同的是，SSL/TLS是用完全模式还是用灵活模式，灵活模式下，只需要设置回源规则，完全模式下，需要设置回源规则和SSL/TLS，SSL/TLS证书仅对 Cloudflare 与源服务器之间的加密有效。




