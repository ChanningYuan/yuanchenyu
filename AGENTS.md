# AGENTS.md — AI Agent 操作指南

本文档为 AI Agent（Claude 等）提供操作本博客仓库的结构化指南。

---

## 项目身份

- **博客名称**: 原晨瑜的个人博客
- **站点地址**: https://yuanchenyu.com
- **作者**: 原晨瑜（产品经理）
- **内容定位**: 产品思考 · 行业分析 · 职场经验 · 读书笔记
- **框架**: Hugo + PaperMod 主题
- **语言**: 中文（简体）为主

---

## 仓库结构速查

```
content/posts/        ← 所有博客文章（Markdown）
content/about.md      ← 关于页面
config.yml            ← 全局配置（标题、菜单、主题参数）
layouts/              ← Hugo 模板（一般不需修改）
assets/css/           ← 样式文件（一般不需修改）
.github/workflows/    ← CI/CD（push master 自动部署）
```

---

## 文章操作规范

### 创建新文章

文件路径: `content/posts/<英文文件名>.md`

文件名规则:
- 使用英文或拼音
- 单词间用连字符 `-` 分隔
- 全部小写
- 示例: `product-thinking-2025.md`、`feishu-table-tips.md`

Front Matter 模板:

```yaml
---
title: "文章标题（中文）"
date: 2025-01-01T10:00:00+08:00
draft: false
tags: ["标签1", "标签2"]
categories: ["分类"]
summary: "一句话摘要，会显示在文章列表页。"
---

文章正文...
```

外链文章（如知乎、公众号等）Front Matter 模板:

```yaml
---
title: "文章标题"
date: 2025-01-01T10:00:00+08:00
draft: false
tags: ["标签"]
categories: ["分类"]
externalUrl: "https://目标URL"
summary: "摘要说明"
hideSummary: false
---
```

### 修改现有文章

直接编辑对应的 `.md` 文件。如果是草稿，将 `draft: true` 改为 `draft: false` 发布。

### 删除文章

删除对应的 `.md` 文件即可。

---

## 配置操作规范

### 修改站点基本信息

编辑 `config.yml`:

```yaml
params:
  title: "站点标题"
  description: "站点描述"
  author: "作者名"
```

### 修改首页 Profile

```yaml
params:
  profileMode:
    enabled: true
    title: "首页大标题"
    subtitle: "首页副标题"
    imageUrl: "头像URL（可选）"
    buttons:
      - name: 按钮文字
        url: 目标路径
```

### 修改导航菜单

```yaml
menu:
  main:
    - identifier: 唯一标识
      name: 菜单文字
      url: /路径/
      weight: 数字（越小越靠前）
```

### 添加社交链接

```yaml
params:
  socialIcons:
    - name: github    # 支持: github, twitter, linkedin, email, rss, youtube 等
      url: "https://..."
    - name: email
      url: "mailto:xxx@xxx.com"
```

---

## 构建与部署

| 操作 | 命令 |
|------|------|
| 安装依赖 | `npm install` |
| 本地预览（含草稿） | `npx hugo server -D` |
| 本地预览（仅正式） | `npx hugo server` |
| 生产构建 | `hugo --minify` |
| 部署 | `git push origin master`（自动触发 GitHub Actions） |

---

## Git 提交规范

```bash
# 新增文章
git commit -m "feat: 新增文章《文章标题》"

# 修改文章
git commit -m "fix: 修正《文章标题》中的描述"

# 更新关于页面
git commit -m "update: 更新关于页面"

# 修改配置
git commit -m "config: 更新首页副标题"

# 调整样式
git commit -m "style: 调整导航菜单顺序"
```

---

## 常用分类与标签参考

**现有分类（categories）**:
- 多维表格
- 时间管理
- Examples（示例，可移除）

**现有标签（tags）**:
- 产品使用技巧
- demo、markdown、features（示例，可移除）

**推荐新增分类方向**:
- 产品思考
- 行业分析
- 职场经验
- 读书笔记

---

## 注意事项

1. **不要修改 `layouts/` 和 `assets/` 下的文件**，除非明确需要调整样式或模板
2. **图片使用外链**，不要将图片文件提交到仓库（避免仓库体积过大）
3. **时区统一使用 `+08:00`**（北京时间）
4. **草稿文章**设置 `draft: true`，不会出现在生产站点
5. **外链文章**正文留空即可，通过 `externalUrl` 跳转
6. **文件名**全部使用英文/拼音，避免中文路径导致的潜在问题
