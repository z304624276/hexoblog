---
title: 记录一下Node项目
date: 2026-05-17 13:27:14
categories: [后端]
tags: [node]
cover: https://qncdn.178981.xyz/test/20260625003036428.png?imageslim
---

## 项目情况：

在ipv6的服务器下，docker部署的node项目，好像无法访问外网，不太清楚什么原因。
所以不用docker来部署了，直接在服务器部署node项目。

## 项目准备

首先是安装node,npm，这里推荐使用nvm进行管理
```
# 下载并安装 nvm：
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
# 代替重启 shell
\. "$HOME/.nvm/nvm.sh"
# 下载并安装 Node.js：
nvm install 24
# 验证 Node.js 版本：
node -v # Should print "v24.15.0".
# 验证 npm 版本：
npm -v # Should print "11.12.1".
# 设置镜像源
npm config set registry https://registry.npmmirror.com

```

接下来是安装pm2
```
# 安装 PM2
npm install -g pm2
```

## 项目启动

进入项目目录，执行npm install 安装依赖

## 配置pm2启动文件

创建pm2.config.js

```
module.exports = {
  apps: [{
    // 应用名称（PM2中显示的进程名）
    name: 'cloudflare-imgbed',
    
    // 启动脚本路径（入口文件）
    script: 'deploy/server/index.js',
    
    // Node.js 运行时参数
    // --import：预加载 ES 模块，这里加载 register.mjs（可能用于注册模块别名或配置）
    node_args: '--import ./deploy/server/register.mjs',
    
    // 工作目录（应用运行的根目录）
    cwd: '/www/wwwroot/CloudFlare-ImgBed',
    
    // 启动实例数量（1表示单进程）
    instances: 1,
    
    // 进程崩溃后是否自动重启
    autorestart: true,
    
    // 是否监听文件变化自动重启（false表示不监听）
    watch: false,
    
    // 环境变量配置
    env: {
      NODE_ENV: 'production'  // 设置为生产环境
    }
  }]
}
```

或者使用命令行创建
```
pm2 start index.js --name cloudflare-imgbed --node-args="--import ./deploy/server/register.mjs" --env production
```
或者通过 npm 脚本启动
```
pm2 start npm --name cloudflare-imgbed -- run start:docker
```

## 启动项目

```
pm2 start pm2.config.js
```

## 停止项目

```
pm2 stop cloudflare-imgbed
```

## 重启项目

```
pm2 restart cloudflare-imgbed
```

## 查看项目状态

```
pm2 status
```

## 查看项目日志

```
pm2 logs cloudflare-imgbed
```

## 删除项目

```
pm2 delete cloudflare-imgbed
```

## 开放端口

```
ufw allow 28787
```

## 查看端口

```
 ufw status
```
## 关闭端口

```
ufw delete allow 28787
```