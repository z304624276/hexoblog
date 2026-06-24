---
title: nginx均衡负载
date: 2026-05-10 23:44:16
categories: [前端,nginx]
tags: [nginx]
cover: https://qncdn.178981.xyz/test/20260625005618668.png?imageslim
---

## nginx均衡负载

1. 宝塔配置nginx均衡负载

### 在 server 块上方添加
```
upstream backend_servers {
    # ip_hash;  # 如果需要会话保持，取消注释
    # least_conn;  # 最少连接数策略，取消注释
    server 192.168.1.10:8080 weight=3;  # 权重为3
    server 192.168.1.11:8080;           # 默认权重为1
    server 192.168.1.12:8080 max_fails=3 fail_timeout=30s;  # 健康检查
}

server
{
    listen 80;
    listen 443 ssl;
    
    # ... 其余配置保持不变

    location / {
    proxy_pass http://backend_servers;
    }
}
```
### 总结
需要注意的地方：

upstream块在 server块外部​ 
upstream和 server是同级关系​ 
location中使用 proxy_pass http://backend_servers;