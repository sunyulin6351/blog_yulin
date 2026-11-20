# 站点维护说明

面向仓库维护者的操作手册。README 只做对外介绍，具体怎么跑、怎么写、怎么发都在这里。

## 目录结构

```
.
├── .github/workflows/      # hugo.yml（构建部署）、microblog.yml（Issue → 想法）
├── archetypes/             # hugo new 使用的模板
├── assets/
│   ├── css/extended/       # 覆盖 PaperMod 的自定义样式
│   └── images/             # 会被 Hugo 处理的图片
├── content/
│   ├── posts/              # 长文（文件名 YYYYMMDD.md，用 slug 固定 URL）
│   ├── micro/              # 想法（短记录，YYYY-MM-DD-HHMM.md）
│   ├── about.md            # /about/
│   ├── archives.md         # /archives/
│   ├── search.md           # /search/
│   ├── categories/         # 分类索引页
│   └── tags/               # 标签索引页
├── docs/MAINTENANCE.md     # 本文档
├── layouts/                # 模板覆盖：404、搜索、micro、partials、OG/JSON-LD
├── static/                 # 原样输出：图片、favicon、llms.txt、CNAME
├── themes/PaperMod/        # 主题（Git 子模块）
└── hugo.toml               # 站点配置
```

## 本地命令

```bash
hugo server -D
hugo --minify
```

构建产物默认写入 `public/`，日常预览可使用 `hugo --minify --destination .build`。

首次克隆记得带上子模块：`git clone --recurse-submodules https://github.com/sunyulin6351/blog_yulin.git`。

## 内容约定

- 文章放在 `content/posts/`，文件名使用 `YYYYMMDD.md`。
- 长文要写 `slug`，避免以后修改标题时改变 URL。
- `description` 控制在 120 字以内，用于 meta description 和分享卡片。
- 修改旧文时补 `lastmod: YYYY-MM-DD`，它会用于 sitemap 和 JSON-LD 的 `dateModified`。
- 微记录放在 `content/micro/`，由 `microblog` workflow 从 Issue 自动生成。
- 标签使用一套固定词表（人工智能、方法论、认知、习惯、读书、影视、哲学、个人成长、极客、工具、效率、计算机、产业观察、AI Agent、LLM、工程思维），避免「AI / 人工智能」「书评 / 读书笔记」这类同义词分散内容。

## 新建文章

```bash
hugo new posts/文章名称.md
```

支持的 Front Matter 字段：

```yaml
---
title: "文章标题"
date: 2026-01-01
slug: "article-slug"
draft: false
categories: ["分类名"]
tags: ["标签1", "标签2"]
description: "一句话说明这篇文章解决什么问题（建议 60–100 字）"
---
```

- `slug` 用于生成稳定 URL，改标题不会改变链接；要改 slug 时，用 `aliases` 保留旧地址。
- `description` 用于 meta description 和社交分享；不写时模板会自动截取正文摘要的前 100 字。
- 新建文章可参考 `content/posts/template.md`（`draft: true`，不会发布）。

## 想法（micro）

站点 `/micro/` 是短记录流，首页显示最近 5 条。它不进分类、不需要成文，适合一句话的观察、灵感和日常记录。

有两种发布方式：

**方式一：开一个 Issue（手机上也能发）**

在仓库里新建 Issue，**标题就是想法正文**。提交后 `.github/workflows/microblog.yml` 会：

1. 在 `content/micro/` 生成 `YYYY-MM-DD-HHMM-<issue号>.md`，时间取北京时间；
2. 自动提交到 `main`，触发 Hugo 构建并部署；
3. 部署完成后关闭这个 Issue。

为防滥用，只有仓库所有者自己创建的 Issue 会触发这套流程。

**方式二：直接写文件**

在 `content/micro/` 下新建 `YYYY-MM-DD-HHMM.md`，front matter 只需要三行：

```yaml
---
date: 2026-06-10T08:07:45+08:00
type: micro
draft: false
---

一句话的想法。
```

`date` 请带上 `+08:00` 时区，文件名与 `date` 的日期、时分保持一致，列表按 `date` 倒序展示。

想法页由 `content/micro/_index.md` 的 cascade 统一设置：不进 `sitemap.xml`、`robotsNoIndex`、关闭评论；但它们仍会出现在 RSS 和站内搜索里。最早的两条记录文件名用的是 UTC 小时（`2026-03-05-0259.md` 在站点上显示 10:59），改文件名会同时改掉 URL 和评论映射，所以保持原样即可。

## 发布与部署

推送到 `main` 就会触发 `.github/workflows/hugo.yml`：用 Hugo 0.170.0（extended）执行 `hugo --minify`，再把 `public/` 发布到 GitHub Pages。通常提交后一两分钟内生效；GitHub Pages 的 HTML 缓存约 10 分钟，看不到变化先强制刷新。

`.github/workflows/microblog.yml` 走同一条构建路径，只是多一步：从 Issue 生成想法文件并提交到 `main`。

## PaperMod 主题覆盖清单

PaperMod 是 Git submodule，以下文件是本地覆盖，升级主题时需要逐个对照：

| 文件 | 用途 |
| --- | --- |
| `layouts/404.html` | 中文 404 页 |
| `layouts/index.html` | 首页结构 |
| `layouts/micro/single.html` | 微记录详情页 |
| `layouts/_default/single.html` | 文章详情页 |
| `layouts/_default/micro.html` | 微记录列表 |
| `layouts/_default/terms.html` | 分类/标签索引 |
| `layouts/_default/search.html` | 搜索页与无结果状态 |
| `layouts/_default/rss.xml` | RSS |
| `layouts/_default/index.json` | 搜索索引 |
| `layouts/partials/head.html` | meta、标题、robots |
| `layouts/partials/description.html` | 中文摘要 |
| `layouts/partials/post_meta.html` | 中文阅读时长 |
| `layouts/partials/post_nav_links.html` | 上一篇/下一篇 |
| `layouts/partials/related-posts.html` | 相关文章 |
| `layouts/partials/share_icons.html` | 分享按钮与微信弹窗 |
| `layouts/partials/comments.html` | Giscus 留言（延迟加载） |
| `layouts/partials/extend_footer.html` | 分享、搜索、弹窗、Giscus 脚本 |
| `layouts/partials/home-latest.html` | 首页最新文章 |
| `layouts/partials/home-micro.html` | 首页最新想法 |
| `layouts/partials/templates/opengraph.html` | OG 卡片 |
| `layouts/partials/templates/twitter_cards.html` | Twitter 卡片 |
| `layouts/partials/templates/schema_json.html` | JSON-LD |

升级步骤：

1. 更新 `themes/PaperMod` submodule；
2. 对上述文件做 diff，确认主题上游改动是否需要同步；
3. 运行 `hugo --minify`；
4. 检查首页、文章页、微记录、分类/标签、搜索、404、RSS 和 JSON-LD。

## 分发与性能

- GitHub Pages 的 HTML 缓存约为 10 分钟，强制刷新可绕过本地缓存。
- 如需更稳定的国内访问，可在 GitHub Pages 前加 Cloudflare 等 CDN。
- 评论使用 Giscus，滚动到评论区附近才加载第三方脚本。
- 分享里的微信二维码依赖 `api.qrserver.com`，复制链接是备用路径。
- 目前没有接入统计分析；如需要，优先考虑 Umami 或 Cloudflare Analytics 这类隐私友好方案。

## 已有自动检查

- 构建：GitHub Actions 的 `hugo` workflow。
- 内容发布：`microblog` workflow 从 Issue 生成微记录。
- 站点入口：`static/llms.txt` 提供面向 AI 爬虫的站点摘要。
