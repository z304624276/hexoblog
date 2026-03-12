---
title: cloudflare部署D1
date: 2026-02-26 22:14:43
categories: [前端]
tags: [cloudflare, D1]
cover:  https://img.datebase.de5.net/file/test/1772076279682_18018512951168384.webp
---

## cloudflare部署D1

最近做点小工具，需要后端，但是不想买服务器了，就想着用Cloudflare的D1来部署，顺便学习一下。

Cloudflare D1 是 Cloudflare 提供的免费 SQLite 数据库服务，配合 Workers 可以快速构建无服务器后端应用。本文将介绍如何从零开始使用 Workers + D1 创建一个简单的 API 服务。

## 前置准备

1. 注册 [Cloudflare 账号](https://dash.cloudflare.com/)
2. 安装 [Node.js](https://nodejs.org/)
3. 安装 Wrangler CLI：

```bash
npm install -g wrangler
```

4. 登录 Cloudflare：

```bash
wrangler login
```

## 创建项目

### 1. 初始化 Workers 项目

```bash
wrangler init my-d1-app
```

按照提示选择：
- What would you like to start with? → "Hello World" Worker
- Do you want to deploy? → No

### 2. 创建 D1 数据库

```bash
wrangler d1 create my-database
```

复制返回的 `database_id`，然后更新 `wrangler.toml` 文件：

```toml
name = "my-d1-app"
main = "src/index.js"
compatibility_date = "2024-01-01"

[[d1_databases]]
binding = "DB"
database_name = "my-database"
database_id = "你的database_id"
```

## 设计数据库

创建一个简单的用户表作为示例：

```sql
-- 创建 migrations 目录
mkdir migrations

-- 创建迁移文件 0001_init.sql
```

`migrations/0001_init.sql` 内容：

```sql
CREATE TABLE IF NOT EXISTS users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  email TEXT UNIQUE NOT NULL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

执行迁移：

```bash
wrangler d1 execute my-database --local --file=./migrations/0001_init.sql
```

## 编写 Workers 代码

### 基础 API 结构

`src/index.js`：

```javascript
export default {
  async fetch(request, env, ctx) {
    const url = new URL(request.url);
    const path = url.pathname;
    const method = request.method;

    // CORS 处理
    if (method === 'OPTIONS') {
      return new Response(null, {
        status: 204,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE',
          'Access-Control-Allow-Headers': 'Content-Type',
        },
      });
    }

    // 路由处理
    if (path === '/api/users' && method === 'GET') {
      return getUsers(env.DB);
    } else if (path === '/api/users' && method === 'POST') {
      return createUser(request, env.DB);
    } else if (path.startsWith('/api/users/') && method === 'GET') {
      const id = path.split('/').pop();
      return getUserById(id, env.DB);
    } else if (path.startsWith('/api/users/') && method === 'DELETE') {
      const id = path.split('/').pop();
      return deleteUser(id, env.DB);
    }

    return new Response('Not Found', { status: 404 });
  },
};

// 获取所有用户
async function getUsers(db) {
  try {
    const { results } = await db.prepare('SELECT * FROM users ORDER BY created_at DESC').all();
    return jsonResponse(results);
  } catch (error) {
    return jsonResponse({ error: error.message }, 500);
  }
}

// 根据 ID 获取用户
async function getUserById(id, db) {
  try {
    const { results } = await db.prepare('SELECT * FROM users WHERE id = ?').bind(id).all();
    if (results.length === 0) {
      return jsonResponse({ error: 'User not found' }, 404);
    }
    return jsonResponse(results[0]);
  } catch (error) {
    return jsonResponse({ error: error.message }, 500);
  }
}

// 创建用户
async function createUser(request, db) {
  try {
    const { name, email } = await request.json();
    
    if (!name || !email) {
      return jsonResponse({ error: 'Name and email are required' }, 400);
    }

    const result = await db.prepare('INSERT INTO users (name, email) VALUES (?, ?)')
      .bind(name, email)
      .run();

    const { results } = await db.prepare('SELECT * FROM users WHERE id = ?')
      .bind(result.meta.last_row_id)
      .all();

    return jsonResponse(results[0], 201);
  } catch (error) {
    if (error.message.includes('UNIQUE')) {
      return jsonResponse({ error: 'Email already exists' }, 409);
    }
    return jsonResponse({ error: error.message }, 500);
  }
}

// 删除用户
async function deleteUser(id, db) {
  try {
    const result = await db.prepare('DELETE FROM users WHERE id = ?')
      .bind(id)
      .run();

    if (result.meta.changes === 0) {
      return jsonResponse({ error: 'User not found' }, 404);
    }

    return jsonResponse({ message: 'User deleted successfully' });
  } catch (error) {
    return jsonResponse({ error: error.message }, 500);
  }
}

// JSON 响应辅助函数
function jsonResponse(data, status = 200) {
  return new Response(JSON.stringify(data), {
    status,
    headers: {
      'Content-Type': 'application/json',
      'Access-Control-Allow-Origin': '*',
    },
  });
}
```

## 本地开发

启动本地开发服务器：

```bash
wrangler dev
```

服务将在 `http://localhost:8787` 运行。

### 测试 API

```bash
# 创建用户
curl -X POST http://localhost:8787/api/users \
  -H "Content-Type: application/json" \
  -d '{"name":"张三","email":"zhangsan@example.com"}'

# 获取所有用户
curl http://localhost:8787/api/users

# 获取单个用户
curl http://localhost:8787/api/users/1

# 删除用户
curl -X DELETE http://localhost:8787/api/users/1
```

## 部署到生产环境

### 1. 执行生产环境迁移

```bash
wrangler d1 execute my-database --remote --file=./migrations/0001_init.sql
```

### 2. 部署 Workers

```bash
wrangler deploy
```

部署成功后，你会获得一个 Workers URL，如 `https://my-d1-app.你的账户名.workers.dev`

## 进阶功能

### 添加分页功能

```javascript
async function getUsers(db) {
  const { searchParams } = new URL(request.url);
  const page = parseInt(searchParams.get('page')) || 1;
  const limit = parseInt(searchParams.get('limit')) || 10;
  const offset = (page - 1) * limit;

  const { results } = await db.prepare(
    'SELECT * FROM users ORDER BY created_at DESC LIMIT ? OFFSET ?'
  ).bind(limit, offset).all();

  const { count } = await db.prepare('SELECT COUNT(*) as count FROM users').first();

  return jsonResponse({
    data: results,
    pagination: {
      page,
      limit,
      total: count,
      totalPages: Math.ceil(count / limit),
    },
  });
}
```

### 添加搜索功能

```javascript
async function searchUsers(request, db) {
  const { searchParams } = new URL(request.url);
  const query = searchParams.get('q');

  if (!query) {
    return jsonResponse({ error: 'Search query is required' }, 400);
  }

  const { results } = await db.prepare(
    'SELECT * FROM users WHERE name LIKE ? OR email LIKE ?'
  ).bind(`%${query}%`, `%${query}%`).all();

  return jsonResponse(results);
}
```

## 注意事项

1. **免费额度**：D1 免费版每天有 5,000,000 次读取操作和 100,000 次写入操作
2. **本地与远程**：使用 `--local` 参数在本地开发，使用 `--remote` 参数在生产环境操作
3. **数据库备份**：定期导出数据备份：
   ```bash
   wrangler d1 export my-database --remote --output=backup.sql
   ```
4. **安全性**：生产环境记得添加认证和输入验证

## 总结

Cloudflare Workers + D1 提供了一个简单、免费且强大的后端解决方案。通过本文的示例，你可以快速构建一个 RESTful API 服务。这种架构特别适合小型项目、个人博客后端或 API 原型开发。

## 参考资源

- [Cloudflare D1 文档](https://developers.cloudflare.com/d1/)
- [Cloudflare Workers 文档](https://developers.cloudflare.com/workers/)
- [Wrangler CLI 文档](https://developers.cloudflare.com/workers/wrangler/)

