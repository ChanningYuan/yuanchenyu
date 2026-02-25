# CLAUDE.md — 项目说明（给 Claude Code）

## 项目概述

这是 **原晨瑜** 的个人博客，基于 **Hugo** 静态网站生成器 + **PaperMod** 主题构建。

- **域名**: https://yuanchenyu.com
- **作者**: 原晨瑜（产品经理）
- **主要内容方向**: 产品思考、行业分析、职场经验、读书笔记
- **默认语言**: 中文（zh）
- **部署方式**: GitHub Actions 自动部署到 GitHub Pages（push 到 `master` 分支触发）

---

## 技术栈

| 项目 | 说明 |
|------|------|
| 静态框架 | Hugo（>= 0.146.0，通过 `hugo-bin` npm 包安装） |
| 主题 | PaperMod |
| 包管理 | npm（仅用于安装 Hugo binary） |
| 配置格式 | YAML |
| 内容格式 | Markdown + YAML Front Matter |

---

## 目录结构

```
yuanchenyu/
├── config.yml              # 主配置文件（站点标题、菜单、主题参数等）
├── content/
│   ├── about.md            # 关于页面
│   ├── archives.md         # 归档页（自动聚合）
│   ├── search.md           # 搜索页
│   ├── categories/_index.md
│   ├── tags/_index.md
│   └── posts/              # 博客文章目录（重点！）
│       └── *.md
├── layouts/                # Hugo 模板（自定义页面结构）
│   ├── _default/           # 基础模板（single、list、archives 等）
│   └── partials/           # 可复用组件（header、footer、toc 等）
├── assets/css/             # 样式文件
├── i18n/                   # 多语言翻译文件
├── .github/workflows/      # GitHub Actions CI/CD
└── package.json            # npm 配置（仅含 hugo-bin 依赖）
```

---

## 本地开发

```bash
# 安装依赖（首次）
npm install

# 启动本地开发服务器（含草稿，热更新）
npx hugo server -D

# 访问 http://localhost:1313
```

---

## 常见任务

### 1. 新增博客文章

在 `content/posts/` 目录下创建 `.md` 文件，文件名使用英文或拼音（用连字符分隔），例如 `product-thinking-2024.md`。

**Front Matter 模板（普通文章）**:

```yaml
---
title: "文章标题"
date: 2024-01-01T10:00:00+08:00
draft: false
tags: ["标签1", "标签2"]
categories: ["分类"]
summary: "文章摘要，显示在列表页。"
cover:
  image: "封面图片URL（可选）"
  alt: "图片描述"
showToc: true
---

正文内容（Markdown）...
```

**Front Matter 模板（外链文章，如知乎专栏）**:

```yaml
---
title: "文章标题"
date: 2024-01-01T10:00:00+08:00
draft: false
tags: ["标签"]
categories: ["分类"]
externalUrl: "https://外部链接"
summary: "摘要"
hideSummary: false
---
```

### 2. 修改关于页面

编辑 `content/about.md`。

### 3. 修改网站配置

编辑 `config.yml`：
- 站点标题/描述：`params.title` / `params.description`
- 首页副标题：`params.profileMode.subtitle`
- 社交链接：`params.socialIcons`
- 导航菜单：`menu.main`

### 4. 修改首页 Profile

编辑 `config.yml` 中的 `params.profileMode` 部分：
- `title`: 首页大标题
- `subtitle`: 首页副标题
- `imageUrl`: 头像图片（相对于 `assets/` 或外链）
- `buttons`: 快捷按钮

---

## Front Matter 字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| `title` | string | 文章标题 |
| `date` | datetime | 发布时间（`2024-01-01T10:00:00+08:00` 格式） |
| `draft` | bool | `true`=草稿不发布，`false`=正式发布 |
| `tags` | array | 标签列表 |
| `categories` | array | 分类列表 |
| `summary` | string | 摘要（显示在列表页） |
| `externalUrl` | string | 外部链接（点击后跳转，不显示正文） |
| `cover.image` | string | 封面图 URL |
| `cover.alt` | string | 封面图 alt 文字 |
| `showToc` | bool | 是否显示目录 |
| `TocOpen` | bool | 目录默认展开 |
| `hideSummary` | bool | 是否隐藏摘要 |
| `disableShare` | bool | 是否隐藏分享按钮 |
| `weight` | int | 排序权重（数字越小越靠前） |
| `series` | array | 系列名称（连载文章） |

---

## 部署

推送到 `master` 分支后，GitHub Actions 自动构建并部署到 GitHub Pages。

```bash
git add .
git commit -m "feat: 新增文章《xxx》"
git push origin master
```

---

## 注意事项

- 文章文件名用英文/拼音 + 连字符，避免中文路径
- 时区使用 `+08:00`（北京时间）
- `draft: true` 的文章不会部署到生产环境
- 外链文章使用 `externalUrl` 字段，正文可以为空
- 图片优先使用外部 CDN 链接，避免增加仓库体积
