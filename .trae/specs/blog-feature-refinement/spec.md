# 博客功能细化规范 (Blog Feature Refinement Spec)

## 为什么 (Why)
当前博客设置较为基础。为了达成“专业级”个人博客的目标，我们需要根据 `hugo-PaperMod` 的能力详细规划具体的功能需求和配置。本规范旨在细化需求，确保博客功能完整且体验丰富。

## 变更内容 (What Changes)
- 详细配置 `hugo-PaperMod` 的各项特性。
- 设置高级内容管理功能（如封面图、目录、代码高亮等）。
- 集成第三方服务（评论系统、统计分析）。

## 影响 (Impact)
- **涉及规范**: 无（新规范）。
- **涉及代码**: `config.yml` 配置文件，`content/` 内容目录结构，`layouts/`（如果需要定制）。

## 新增需求 (ADDED Requirements)

### 需求：站点标识与导航 (Site Identity & Navigation)
系统应提供鲜明的品牌标识和直观的导航。
- **站点标题**: 可配置的网站标题。
- **Favicon**: 支持自定义图标 (apple-touch-icon, 32x32, 16x16)。
- **菜单**:
    -   **首页 (Home)**: 链接到个人主页。
    -   **归档 (Archives)**: 按时间线展示所有文章。
    -   **分类 (Categories)**: 按分类展示文章。
    -   **标签 (Tags)**: 标签云或列表。
    -   **搜索 (Search)**: 客户端搜索页面。
    -   **关于 (About)**: 静态关于页面（可选）。

### 需求：个人主页模式 (Profile Mode / Landing Page)
首页应作为“个人资料模式”的落地页。
- **布局**: 居中的个人资料卡片。
- **元素**:
    -   **头像**: 圆形图片（建议 120x120px）。
    -   **标题**: 用户名或站点名称。
    -   **副标题**: 简短介绍或标语（支持 Markdown）。
    -   **社交图标**: GitHub, Twitter, LinkedIn, Email 等 SVG 图标链接。
    -   **操作按钮**: 主要按钮指向“博客文章”、“项目展示”等。

### 需求：内容展示 (Content Display / Posts)
博客文章应支持丰富的格式和元数据。
- **Front Matter**: 标准化字段：`title`（标题）, `date`（日期）, `draft`（草稿）, `tags`（标签）, `categories`（分类）, `cover`（封面图配置）。
- **封面图**: 响应式封面图，支持标题说明。
- **目录 (ToC)**:
    -   自动生成（H1-H6）。
    -   支持折叠/展开。
    -   侧边悬浮或内嵌显示。
- **代码块**:
    -   语法高亮 (Chroma)。
    -   行号显示（可切换）。
    -   一键复制按钮。
- **元数据**:
    -   阅读时间预估。
    -   字数统计。
    -   发布日期（及最后修改日期）。
    -   作者名称。
- **导航**:
    -   面包屑导航 (Home > Posts > Title)。
    -   上一篇/下一篇链接。
    -   相关文章推荐。
- **分享**:
    -   文章底部社交分享按钮（Twitter, LinkedIn, Facebook, WhatsApp, Telegram）。

### 需求：内容发现 (Content Discovery)
用户应能轻松找到内容。
- **搜索**:
    -   基于 Fuse.js 的客户端搜索。
    -   索引内容：标题、正文、摘要、链接。
    -   搜索结果关键词高亮。
- **归档**:
    -   专用页面，按年份分组展示所有文章。
- **分类法 (Taxonomies)**:
    -   专用页面展示所有分类和标签及其文章数量。

### 需求：用户交互与主题 (User Interaction & Theme)
- **主题切换**:
    -   支持 浅色 (Light)、深色 (Dark) 和跟随系统 (Auto) 模式。
    -   本地存储用户偏好。
- **评论**:
    -   集成评论系统（推荐 **Giscus** 基于 GitHub Discussions，或 **Utterances**）。
    -   评论懒加载。

### 需求：SEO 与性能 (SEO & Performance)
- **元数据**:
    -   OpenGraph 标签（社交媒体预览）。
    -   Twitter Card 元数据。
    -   Canonical URL 规范链接。
- **订阅源**:
    -   RSS 2.0 订阅源生成。
    -   Sitemap.xml 站点地图生成。
- **统计分析**:
    -   支持 Google Analytics (v4)。
