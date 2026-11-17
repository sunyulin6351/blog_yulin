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

## 技术栈

| 组件 | 说明 |
|------|------|
| 静态生成器 | [Hugo](https://gohugo.io/) |
| 主题 | [PaperMod](https://github.com/adityatelange/hugo-PaperMod) |
| 评论系统 | [Giscus](https://giscus.app/)（基于 GitHub Discussions） |
| 搜索 | Fuse.js 全文搜索 |
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
draft: false
categories: ["分类名"]
tags: ["标签1", "标签2"]
description: "一句话说明这篇文章解决什么问题（建议 60–100 字）"
---
```

`description` 会用于 meta description 和社交分享；不写时模板会自动截取正文摘要的前 100 字。

## 目录结构

```
.
├── content/
│   └── posts/       # 博客文章（Markdown）
├── static/          # 静态资源（图片、音频等）
├── assets/          # 自定义 CSS/JS
├── themes/PaperMod/ # 主题（Git 子模块）
└── hugo.toml        # 站点配置
```

## 许可

博客文章内容版权归作者所有，转载请注明出处。
