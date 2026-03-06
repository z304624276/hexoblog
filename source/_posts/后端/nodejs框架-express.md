---
title: nodejs框架-express
date: 2026-02-12 00:23:51
categories: [后端, Node]
tags: [express]
cover: https://img.datebase.de5.net/file/test/aThPzEPm.webp
---

### Nodejs修改要重启的问题

对Node后台需要的一些插件做的笔记，免得忘记找不到了  

首先解决Node每次修改都要启动的问题  

使用插件Nodemon

```Node
Npm install -g nodemon
```
安装完成后Node命令替换成nodemon

### Nodejs框架-express安装

这是全局安装express-generator，安装完成后就可以使用express命令来创建项目了

```Node
npm install -g express-generator
```

创建项目

```Node
express -e myapp
cd myapp
npm install
nodemon dev
```
这样就可以启动项目了，并且每次修改代码都会自动重启

### express框架接入mongodb数据库

首先安装插件Mongoose

```Node
Pnpm install mongoose
```
然后在app.js中引入Mongoose，并连接数据库

```Node
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/myapp', { useNewUrlParser: true, useUnifiedTopology: true });
```
这样就可以连接到本地的myapp数据库了

### express框架接入mysql数据库

首先安装插件mysql

```Node
npm install mysql
```
然后在app.js中引入mysql，并连接数据库

```Node
const mysql = require('mysql');
const connection = mysql.createConnection({
    
})

```

### express框架接入redis数据库

首先安装插件redis

```Node
npm install redis
```
然后在app.js中引入redis，并连接数据库

```Node
const redis = require('redis');
const client = redis.createClient();
client.on('error', function (err) {
  console.log('Error ' + err);
});
```

### express框架接入jwt

首先安装插件jsonwebtoken

```Node
npm install jsonwebtoken
```
然后在app.js中引入jsonwebtoken，并生成token

```Node
const jwt = require('jsonwebtoken');


const token = jwt.sign({ foo: 'bar' }, 'shhhhh');
```
这样就可以生成一个token了

### express框架接入bcrypt

首先安装插件bcrypt

```Node
npm install bcrypt
```
然后在app.js中引入bcrypt，并生成hash

```Node
const bcrypt = require('bcrypt');
const saltRounds = 10;
const myPlaintextPassword = 's0/\/\P4$$w0rD'; // 密码

bcrypt.hash(myPlaintextPassword, saltRounds, function (err, hash) {
  // Store hash in your password DB.
});
```

这样就可以生成一个hash了

### 结束
框架使用笔记完毕，目前用到的就这么多，后续会更新详细的用法


