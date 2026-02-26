# CLAUDE.md — AI Assistant Guide for yuanchenyu

This file provides essential context for AI assistants (Claude, Copilot, etc.) working on this repository.

---

## Project Overview

**yuanchenyu** is a personal Chinese-language blog/portfolio website built with [Hugo](https://gohugo.io/) using the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme. The site belongs to 原晨瑜 (Yuan Chenyu), a product manager who publishes articles on product thinking, industry analysis, workplace experiences, and reading notes.

- **Live site**: https://yuanchenyu.com
- **Theme**: Hugo PaperMod v1.0 (MIT)
- **Language of content**: Simplified Chinese (zh)
- **Minimum Hugo version**: v0.146.0

---

## Repository Structure

```
yuanchenyu/
├── .github/
│   ├── workflows/
│   │   ├── deploy.yml          # Production deployment (push to master → GitHub Pages)
│   │   └── gh-pages.yml        # PaperMod demo site deployment
│   └── PULL_REQUEST_TEMPLATE.md
├── .trae/                      # Internal project specs & planning documents
├── assets/
│   ├── css/
│   │   ├── common/             # Component styles (header, footer, post, search…)
│   │   ├── core/               # Core styles (theme-vars, reset, media queries)
│   │   ├── includes/           # Syntax highlighting (Chroma), scrollbars
│   │   └── extended/           # Blank file for user customizations
│   ├── js/
│   │   ├── fuse.basic.min.js   # Fuse.js fuzzy search library
│   │   ├── fastsearch.js       # Custom client-side search implementation
│   │   └── license.js          # License display script
│   └── images/
│       └── avatar.png          # Profile avatar (120×120)
├── content/
│   ├── about.md                # /about page
│   ├── archives.md             # /archives page (Hugo list layout)
│   ├── search.md               # /search page (Fuse.js powered)
│   ├── posts/                  # Blog articles (add new articles here)
│   │   ├── claude-mobile-coding.md
│   │   ├── zhihu-mental-accounting.md  # External Zhihu link post
│   │   └── zhihu-pm-thinking.md        # External Zhihu link post
│   ├── categories/_index.md
│   └── tags/_index.md
├── i18n/                       # 46-language translation strings (do not edit for zh content)
├── layouts/
│   ├── _default/               # Base Hugo templates (baseof, single, list, archives…)
│   ├── partials/               # Reusable template components
│   └── shortcodes/             # Custom Hugo shortcodes
├── config.yml                  # Main site configuration (baseURL, menu, params…)
├── go.mod                      # Go module file (Hugo theme module)
├── package.json                # NPM: installs hugo-bin for local dev
├── package-lock.json
├── plan.md                     # Project setup & usage guide (human-readable)
├── theme.toml                  # Theme metadata
└── CLAUDE.md                   # This file
```

---

## Development Commands

### Setup
```bash
npm install        # Installs hugo-bin (the Hugo binary) via Node
```

### Local Development
```bash
npx hugo server -D          # Start dev server at http://localhost:1313/ (includes drafts)
npx hugo server             # Start dev server (published posts only)
```

### Production Build
```bash
npx hugo --minify           # Build to public/ with CSS/HTML/JS minification
```

### No dedicated test/lint commands are configured.
Hugo validates template syntax on every build. A failed build is the primary lint signal.

---

## Deployment

Deployment is fully automated via GitHub Actions:

1. **Trigger**: Push to `master` branch
2. **Workflow**: `.github/workflows/deploy.yml`
3. **Build**: `hugo --minify` → outputs to `public/`
4. **Deploy**: `actions-gh-pages@v4` pushes `public/` to GitHub Pages
5. **CNAME**: `yuanchenyu.com`

> Do not push broken Hugo templates or invalid front matter to `master` — it will break the live site.

---

## Content Conventions

### Adding a New Blog Post

Create a new `.md` file under `content/posts/`:

```yaml
---
title: "文章标题"
date: 2026-02-26T12:00:00+08:00
draft: false
tags: ["标签1", "标签2"]
categories: ["产品"]
showToc: true
TocOpen: true
summary: "一句话描述文章内容，用于列表页展示。"
---

正文内容...
```

**Naming convention**: Use kebab-case English filenames (e.g., `my-new-article.md`), not Chinese filenames.

### Linking to External Articles (e.g., Zhihu)

```yaml
---
title: "知乎文章标题"
date: 2026-02-26T00:00:00+08:00
externalUrl: "https://zhuanlan.zhihu.com/p/..."
summary: "文章摘要"
tags: ["产品", "思考"]
categories: ["产品"]
draft: false
---
```

### Front Matter Reference

| Field | Type | Description |
|---|---|---|
| `title` | string | Article title (Chinese OK) |
| `date` | datetime | Publication date (timezone `+08:00`) |
| `draft` | bool | `true` = hidden from production build |
| `tags` | list | Tag taxonomy values |
| `categories` | list | Category taxonomy values |
| `summary` | string | Excerpt shown in list views |
| `showToc` | bool | Show table of contents |
| `TocOpen` | bool | TOC expanded by default |
| `cover.image` | string | Path to cover image |
| `externalUrl` | string | Redirects post to external URL |

---

## Site Configuration (`config.yml`)

Key sections to be aware of:

| Section | Purpose |
|---|---|
| `baseURL` | Must be `https://yuanchenyu.com/` for production |
| `params.profileMode` | Controls homepage: title, subtitle, avatar, buttons |
| `params.socialIcons` | GitHub icon link in profile |
| `menu.main` | Navigation items (Home, Archives, Categories, Tags, Search, About) |
| `params.fuseOpts` | Fuse.js search sensitivity (threshold: 0.4) |
| `markup.goldmark` | Allows unsafe HTML in Markdown |
| `markup.highlight` | Chroma syntax highlighting (monokai style) |
| `outputs` | HTML + RSS + JSON (JSON required for search) |

**Do not change** `baseURL`, `outputs`, or `taxonomies` without understanding downstream effects on search, RSS, and sitemap.

---

## Template & Layout Conventions

### Template Hierarchy (Hugo standard)
1. `layouts/_default/baseof.html` — Root HTML shell
2. `layouts/_default/single.html` — Individual post pages
3. `layouts/_default/list.html` — List/archive pages
4. `layouts/partials/` — Reusable components (included via `{{ partial "name.html" . }}`)

### Key Partials
| Partial | Purpose |
|---|---|
| `head.html` | `<head>` with SEO meta, CSS, JS |
| `header.html` | Navigation bar |
| `footer.html` | Footer with theme credit |
| `index_profile.html` | Homepage profile card |
| `social_icons.html` | Social icon rendering |
| `post_meta.html` | Date, reading time, word count |
| `toc.html` | Table of contents |
| `extend_head.html` | Hook for adding custom CSS/JS to `<head>` |
| `extend_footer.html` | Hook for adding custom scripts before `</body>` |

### Custom Shortcodes
| Shortcode | Usage |
|---|---|
| `{{< collapse >}}` | Collapsible/accordion content |
| `{{< figure >}}` | Image with caption |
| `{{< rawhtml >}}` | Embed raw HTML |
| `{{< inTextImg >}}` | Inline image within text |
| `{{< rtl >}}` | Right-to-left text block |

---

## CSS Conventions

CSS is organized into four layers under `assets/css/`:

| Layer | Directory | Purpose |
|---|---|---|
| Core | `core/` | CSS variables (`theme-vars.css`), reset, media queries |
| Common | `common/` | Component styles (one file per component) |
| Includes | `includes/` | Third-party styles (Chroma, scrollbars) |
| Extended | `extended/blank.css` | Add custom overrides here — do not edit `core/` or `common/` |

**Dark/light mode** is implemented via CSS variables and a `data-theme` attribute on `<html>`. Theme preference is stored in `localStorage`.

---

## Search Implementation

Client-side search powered by [Fuse.js](https://fusejs.io/):

1. Hugo generates a `public/index.json` during build (requires `JSON` in `outputs`)
2. `assets/js/fastsearch.js` fetches and indexes the JSON on the `/search` page
3. Search fields indexed: `title`, `permalink`, `summary`, `content`
4. Sensitivity controlled by `fuseOpts` in `config.yml`

---

## Internationalization (i18n)

- 46 language files in `i18n/` (from PaperMod upstream)
- Site language is `zh` (Simplified Chinese)
- The file `i18n/zh.yaml` contains UI string translations
- **Do not edit** other language files unless contributing back to PaperMod

---

## Git Workflow

- **Production branch**: `master` (triggers auto-deploy on push)
- **Feature branches**: Use `claude/<feature-name>` or standard feature branch naming
- **Commit language**: Chinese commit messages are used in this repo (follow the existing style)

### Example commit message style (follow existing):
```
新增产品思考系列文章
修复首页头像显示问题
更新 config 中的社交链接
```

---

## Common Tasks for AI Assistants

### Add a new article
1. Create `content/posts/<kebab-case-name>.md`
2. Use the front matter template above
3. Write content in Markdown (Chinese)
4. Set `draft: false` when ready to publish

### Update site metadata
- Edit `config.yml` → `params` section
- Avatar: replace `assets/images/avatar.png` (keep 120×120 px)
- Navigation: edit `menu.main` in `config.yml`

### Customize appearance
- Add CSS overrides to `assets/css/extended/blank.css`
- Use CSS variables from `assets/css/core/theme-vars.css` for colors

### Verify build works
```bash
npx hugo --minify
# Should complete without errors; output goes to public/
```

### Preview changes
```bash
npx hugo server -D
# Open http://localhost:1313/
```

---

## What NOT to Do

- **Do not** edit files in `node_modules/`
- **Do not** commit the `public/` directory (it's in `.gitignore` and built by CI)
- **Do not** commit `.hugo_build.lock` or `resources/_gen/`
- **Do not** change the `baseURL` in `config.yml` unless changing the domain
- **Do not** modify `i18n/` files for other languages unless contributing upstream
- **Do not** add `draft: true` posts to `master` and expect them to appear on the live site (they won't — that's intentional)
- **Do not** use Chinese characters in filenames (use kebab-case English)

---

## Technology Stack Summary

| Component | Technology |
|---|---|
| Static site generator | Hugo v0.149.2+ |
| Theme | PaperMod |
| Search | Fuse.js (client-side) |
| Syntax highlighting | Chroma (monokai) |
| Package manager | npm (for hugo-bin only) |
| CI/CD | GitHub Actions |
| Hosting | GitHub Pages |
| Domain | yuanchenyu.com |
| Content language | Simplified Chinese (zh) |
