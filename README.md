# 雨林说

[![Hugo](https://img.shields.io/badge/Hugo-PaperMod-blue)](https://github.com/adityatelange/hugo-PaperMod)
[![博客地址](https://img.shields.io/badge/博客-blog.yulin.ltd-green)](https://blog.yulin.ltd/)

这是 **雨林** 的个人博客源码，基于 [Hugo](https://gohugo.io/) 静态网站生成器和 [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题构建，部署在 GitHub Pages 上。

> 我把生活当作一个长期实验项目，用系统思维拆解问题，用小规模试验验证想法，记录每一次迭代的过程。从时间管理到身体训练，从知识输入到表达输出，我希望一点点搭建属于自己的"人生操作系统"。

## 内容方向

文章按「想回答什么问题」分成三个分类，一篇文章只进一个分类，更细的主题交给标签：

- **人生系统** — 方法、习惯、认知、价值观与个人工作流
- **技术思考** — AI、计算机基础、极客精神与产业观察
- **生活实验** — 观影、读书、旅行与日常记录

标签使用一套固定词表（人工智能、方法论、认知、习惯、读书、影视、哲学、个人成长、极客、工具、效率、计算机、产业观察、AI Agent、LLM、工程思维），避免「AI / 人工智能」「书评 / 读书笔记」这类同义词分散内容。新建文章可参考 `content/posts/template.md` 里的规则。

产品、健身、理财等方向暂不单独立栏，等文章多到能撑起一个栏目时再拆。

## 想法（micro）

长文之外还有一条「想法」流，站点地址是 `/micro/`，首页显示最近 5 条。它不进分类、不需要成文，适合一句话的观察、灵感和日常记录。

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

## 技术栈

| 组件 | 说明 |
|------|------|
| 静态生成器 | [Hugo](https://gohugo.io/) |
| 主题 | [PaperMod](https://github.com/adityatelange/hugo-PaperMod) |
| 评论系统 | [Giscus](https://giscus.app/)（基于 GitHub Discussions） |
| 搜索 | Fuse.js 全文搜索 |
| 订阅 | RSS（`/index.xml`） |
| 部署 | GitHub Pages |

## 本地开发

**前提条件**：已安装 [Hugo](https://gohugo.io/installation/)（extended 版本）。

```bash
# 克隆仓库（含子模块）
git clone --recurse-submodules https://github.com/sunyulin6351/blog_yulin.git
cd blog_yulin

# 启动本地预览服务器
hugo server -D
```

浏览器打开 `http://localhost:1313` 即可预览。

## 新建文章

```bash
hugo new posts/文章名称.md
```

文章存放在 `content/posts/` 目录下，支持以下 Front Matter 字段：

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

`slug` 用于生成稳定 URL；改标题不会改变链接。如果之后修改 slug，记得在 front matter 里用 `aliases` 保留旧地址。

`description` 会用于 meta description 和社交分享；不写时模板会自动截取正文摘要的前 100 字。

修改旧文章时，建议补一个 `lastmod: YYYY-MM-DD`，它会用于 sitemap 和文章结构化数据的更新时间。

## 发布与部署

推送到 `main` 就会触发 `.github/workflows/hugo.yml`：用 Hugo 0.170.0（extended）执行 `hugo --minify`，再把 `public/` 发布到 GitHub Pages。通常提交后一两分钟内生效；GitHub Pages 的 HTML 缓存约 10 分钟，看不到变化先强制刷新。

`.github/workflows/microblog.yml` 走同一条构建路径，只是多一步：从 Issue 生成想法文件并提交到 `main`。

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
├── docs/MAINTENANCE.md     # 维护说明：主题覆盖清单、升级步骤
├── layouts/                # 模板覆盖：404、搜索、micro、partials、OG/JSON-LD
├── static/                 # 原样输出：图片、favicon、llms.txt、CNAME
├── themes/PaperMod/        # 主题（Git 子模块）
└── hugo.toml               # 站点配置
```

## 许可

博客文章内容版权归作者所有，转载请注明出处。
