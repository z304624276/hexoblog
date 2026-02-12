---
title: supabase自部署方法
date: 2026-02-12 00:41:30
categories: [后端, Postgresql]
tags: [supabase]
cover:
---

## supabase自部署方法

写一篇supabase自部署方法，方便以后使用

## 前言

Supabase 是一个开源的、完全托管的数据库即服务（DBaaS），它提供了一整套数据库解决方案，包括数据库管理、数据迁移、数据备份、数据恢复等。Supabase 还提供了一整套 API，包括 RESTful API、GraphQL API、实时数据订阅等，方便开发者快速构建应用程序。Supabase 还提供了一整套数据安全解决方案，包括数据加密、数据访问控制、数据审计等，保证数据的安全性和可靠性。
比较遗憾的是，supabase目前自部署仅支持一个项目，并且对服务器的配置要求较高，CPU和内存至少为2核4G，磁盘至少为20G

## 部署方法

```
git clone --depth 1 https://github.com/supabase/supabase
#首先拉取项目

mkdir supabase-project
#创建一个项目文件夹

cp -rf supabase/docker/* supabase-project
#将supabase项目中的docker文件夹复制到项目文件夹中

cp supabase/docker/.env.example supabase-project/.env
#将supabase项目中的.env.example复制到项目文件夹中并且配置文件里的参数

cd supabase-project
#进入项目文件夹

docker compose pull
#启动项目
```

## 需要修改的配置文件

主要有几个地方

```
POSTGRES_PASSWORD=your-super-secret-and-long-postgres-password  #设置数据库密码
JWT_SECRET= #JWT_SECRET 是JWT加密的密钥，需要自己生成
ANON_KEY= #ANON_KEY  是匿名用户的密钥，需要自己生成
SERVICE_ROLE_KEY= #SERVICE_ROLE_KEY  是服务角色的密钥，需要自己生成
DASHBOARD_USERNAME #DASHBOARD_USERNAME  是仪表盘的用户名，需要自己生成
DASHBOARD_PASSWORD  #DASHBOARD_PASSWORD  是仪表盘的密码，需要自己生成
```
查看文档  

https://supabase.com/docs/guides/self-hosting/docker#generate-and-configure-api-keys

## 访问项目
部署完成后访问项目
```
http://服务器地址:8000端口
#访问项目
```

## 总结