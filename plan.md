# Hugo PaperMod 个人博客说明手册（0-1 搭建与后续维护）

## 1. 这是什么项目？
`hugo-PaperMod` 是一个 Hugo 主题（Theme），配合 Hugo 这个静态网站生成器，可以把 Markdown 文章生成成一个可部署的静态博客站点。

你现在这个目录里既包含主题源码（`layouts/`, `assets/` 等），也已经放入了一个最小可运行的"站点配置 + 内容"（`config.yml`, `content/`）。

## 2. 能做到什么程度？
PaperMod 适合做专业级个人博客，常见能力包括：
- 主页三种模式：Regular / Home-Info / Profile
- 文章体验：目录 ToC、代码高亮、代码复制按钮、阅读时间、面包屑导航、上一篇下一篇
- 内容组织：归档（Archives）、标签/分类聚合页、RSS
- 搜索：内置 Fuse.js 客户端搜索（无需后端）
- SEO：OpenGraph / Twitter Cards 等
- 多语言：i18n 支持

## 3. 当前站点的目录结构（你现在就能改的）
本仓库目前已经具备以下站点内容结构：

```
hugo-PaperMod/
├── config.yml            # 站点配置文件
├── content/              # 内容目录
│   ├── about.md          # 关于页
│   ├── archives.md       # 归档页
│   ├── search.md         # 搜索页
│   ├── categories/       # 分类聚合（Hugo 自动管理）
│   ├── tags/             # 标签聚合（Hugo 自动管理）
│   └── posts/
│       ├── demo-post.md  # 演示文章（可删除）
│       └── second-post.md# 演示文章（可删除）
├── layouts/              # 主题布局文件（原项目自带）
├── assets/               # 主题资源（原项目自带）
└── ...
```

## 4. 博客内容方向
- **主要**：产品思考（产品设计、需求分析、用户研究、方法论）
- **其次**：行业分析、职场经验、读书笔记

## 5. 我现在应该怎么体验（本地预览）
### 5.1 安装 Hugo
由于环境限制（没有 brew），我们使用 npm 安装 Hugo 二进制文件：

```bash
npm install
```

这会根据 `package.json` 安装 `hugo-bin`。

### 5.2 启动本地服务
在仓库根目录运行：

```bash
npx hugo server -D
```

然后打开：
- `http://localhost:1313/`

你将看到 Profile 模式主页、菜单入口（搜索 / 归档 / 标签等）和演示文章。

## 6. baseURL 要不要改成本地地址？
本地体验时 **不强制要改**。一般来说：
- `hugo server` 会在本地起服务，页面访问走 `http://localhost:1313/`。
- `baseURL` 主要影响：生成出来的"绝对链接"、canonical URL、RSS/站点地图里的一些链接。

推荐做法：
- 本地开发：可以先保持 `baseURL: "https://example.com"` 作为占位符，不影响大多数预览。
- 准备上线：把 `baseURL` 改成你真实的域名（例如 `https://yourdomain.com/`）。

## 7. 配置怎么改（核心在 config.yml）
站点大部分能力都由 [config.yml](file:///Users/yuanchenyu/ai_coding/hugo-PaperMod/config.yml) 控制，重点区域一般包括：
- `baseURL`：上线时改成真实域名
- `title` / `params.description`：站点标题、描述
- `params.profileMode`：Profile 主页内容（头像、标题、副标题、按钮）
- `params.socialIcons`：社交链接（目前已注释，填入真实链接后取消注释）
- `menu.main`：顶部菜单
- `outputs.home`：搜索需要开启 `JSON`

## 8. 内容怎么加（写文章 / 写页面）
### 8.1 写文章（posts）
路径：`content/posts/` 下新增 Markdown 文件即可。

你可以参考演示文章：
- [demo-post.md](file:///Users/yuanchenyu/ai_coding/hugo-PaperMod/content/posts/demo-post.md)

常用写法：
- 在文件顶部写 Front Matter（标题、时间、标签、分类、封面图、是否草稿）
- 正文用 Markdown 编写（标题会用于 ToC）

文章 Front Matter 示例（产品类）：
```yaml
---
title: "你的文章标题"
date: 2024-01-01T10:00:00+08:00
draft: false
tags: ["产品设计", "用户研究"]
categories: ["产品思考"]
showToc: true
---
```

### 8.2 写独立页面（比如 Search / Archives）
路径：`content/` 下新增 `xxx.md`，并在 Front Matter 里指定 layout。

当前已创建：
- [search.md](file:///Users/yuanchenyu/ai_coding/hugo-PaperMod/content/search.md)：`layout: "search"`
- [archives.md](file:///Users/yuanchenyu/ai_coding/hugo-PaperMod/content/archives.md)：`layout: "archives"`

## 9. 如何部署上线（静态站点托管）
PaperMod + Hugo 输出的是静态文件（默认在 `public/`），适合这些方式：
- GitHub Pages（最常见）：构建后把 `public/` 部署到 Pages
- Vercel / Netlify / Cloudflare Pages：直接接 Git 仓库，自动构建与发布
- 云服务器（VPS）：用 Nginx/Apache 直接托管 `public/` 静态文件
- 对象存储 + CDN：例如 OSS/S3 + CDN

上线前需要做的一件关键事：把 `config.yml` 的 `baseURL` 改为你的线上地址（域名或 Pages 地址）。

## 10. 后续你可能最常改的地方
- 改主页文案与头像：`config.yml -> params.profileMode`（头像 `imageUrl` 待填入真实链接）
- 改菜单：`config.yml -> menu.main`
- 新增/修改社交链接：`config.yml -> params.socialIcons`（取消注释并填入真实链接）
- 写新文章：`content/posts/*.md`
- 新增独立页面：`content/*.md`（指定 layout）
