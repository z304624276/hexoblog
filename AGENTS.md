# AGENTS.md

## 项目概览

这是一个基于 **Hexo 7.3.0** 的个人博客项目，名为"啊超的博客"。项目使用 Hexo 静态站点生成器构建，当前启用的是 **hexo-theme-stellar** 主题（版本 1.33.1）。Stellar 是一个功能强大的综合型 Hexo 主题，支持博客系统、知识库系统、专栏系统和笔记系统。

### 主要技术栈

- **Hexo** 7.3.0 - 静态站点生成器
- **Node.js** - 运行环境（建议使用 LTS 版本）
- **渲染器**：
  - hexo-renderer-ejs - EJS 模板渲染
  - hexo-renderer-marked - Markdown 渲染
  - hexo-renderer-pug - Pug 模板渲染
  - hexo-renderer-stylus - Stylus 样式渲染
- **主题**：hexo-theme-stellar 1.33.1（当前启用），hexo-theme-butterfly 5.5.4（已安装）
- **其他已安装主题**：hexo-theme-landscape、hexo-theme-solitude

### 项目结构

```
D:\Bun\blog\
├── _config.yml              # Hexo 主配置文件
├── _config.landscape.yml    # Landscape 主题配置
├── package.json             # 项目依赖和脚本
├── db.json                  # Hexo 数据库
├── scaffolds/               # 文章模板
│   ├── post.md             # 博客文章模板
│   ├── page.md             # 页面模板
│   └── draft.md            # 草稿模板
├── source/                  # 源文件目录
│   └── _posts/             # 博客文章目录
│       └── 我的第一篇文章.md
├── themes/                  # 主题目录
│   ├── hexo-theme-stellar/  # Stellar 主题
│   └── hexo-theme-butterfly/ # Butterfly 主题
└── .github/                 # GitHub 配置
    └── dependabot.yml       # Dependabot 自动更新配置
```

## 构建和运行

### 可用命令

在 `package.json` 中定义了以下核心命令：

```bash
# 清理生成的文件和缓存
npm run clean
# 或
hexo clean

# 启动本地开发服务器（默认端口 4000）
npm run server
# 或
hexo server

# 生成静态站点
npm run build
# 或
hexo generate

# 部署站点
npm run deploy
# 或
hexo deploy
```

### 开发工作流

1. **创建新文章**：
   ```bash
   hexo new post "文章标题"
   ```
   文章将在 `source/_posts/` 目录下创建，使用 `scaffolds/post.md` 模板

2. **创建新页面**：
   ```bash
   hexo new page "页面名称"
   ```
   页面使用 `scaffolds/page.md` 模板

3. **本地预览**：
   ```bash
   hexo server
   ```
   访问 `http://localhost:4000` 预览

4. **生成静态文件**：
   ```bash
   hexo generate
   ```
   生成的静态文件在 `public/` 目录

5. **清理缓存**：
   ```bash
   hexo clean
   ```

## 开发约定

### 文章格式

所有文章使用 Markdown 格式，必须包含 front-matter 元数据：

```markdown
---
title: 文章标题
date: 2026-02-09 10:00:00
tags: [标签1, 标签2]
categories: 分类
---
```

### 目录约定

- `source/_posts/` - 存放博客文章
- `source/` - 其他静态资源和自定义页面
- `themes/hexo-theme-stellar/` - 主题文件（如需自定义主题，请复制配置到主配置文件）

### 主题配置

Stellar 主题的配置文件位于 `themes/hexo-theme-stellar/_config.yml`。主题支持：

- 博客文章（layout: post）
- 文档/知识库（layout: wiki）
- 专栏文章（layout: topic）
- 笔记系统（layout: note）
- 自定义页面（layout: page）

### 语言设置

项目当前设置为中文：
```yaml
language: zh-CN
```

### 当前主题配置要点

- 当前启用主题：`hexo-theme-stellar`
- 站点标题：啊超的博客
- 作者：阿超
- URL 设置为 `http://example.com`（需根据实际部署地址修改）
- 文章默认每页显示 10 篇

## 注意事项

1. **主题切换**：如需切换主题，在 `_config.yml` 中修改 `theme` 字段
2. **配置文件**：Stellar 主题配置在 `themes/hexo-theme-stellar/_config.yml`，详细文档见 https://xaoxuu.com/wiki/stellar/
3. **部署配置**：`_config.yml` 中的 `deploy` 部分当前为空，需要根据部署方式（如 GitHub Pages、Netlify 等）进行配置
4. **图片资源**：如需在文章中使用图片，建议放在 `source/images/` 目录或使用图床服务

## 常见问题

### 端口占用
如果 4000 端口被占用，可以使用：
```bash
hexo server -p 4001
```

### 依赖安装
如果需要重新安装依赖：
```bash
npm install
```

### 主题更新
```bash
npm update hexo-theme-stellar
```

## 参考资源

- Hexo 官方文档：https://hexo.io/docs/
- Stellar 主题文档：https://xaoxuu.com/wiki/stellar/
- Stellar GitHub：https://github.com/xaoxuu/hexo-theme-stellar