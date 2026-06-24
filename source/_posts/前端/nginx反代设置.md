---
title: nginx反代设置
date: 2026-06-23 21:28:17
categories: [前端]
tags: [nginx]
cover: https://qncdn.178981.xyz/test/20260625005536381.png?imageslim
---

## nginx反代设置

部署项目出错，搞半天发现是反代的问题
简单记录一下

```
location /api {
        proxy_pass http://127.0.0.1:8787;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_cache_bypass $http_upgrade;
    }
```

上面这段代码，执行请求到 域名/api 就会转发到 127.0.0.1:8787，
我希望是转发到反代的项目首页，但是实际请求到的地址是 http://127.0.0.1:8787/api
需要后端有这个路由，否则会报404


如果后端路由是 /，想让 /api 映射到后端的 /
```
location /api/ {
        proxy_pass http://127.0.0.1:8787/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_cache_bypass $http_upgrade;
    }
```

修改成这样，http://域名/api/test 会转发到 http://127.0.0.1:18787/test