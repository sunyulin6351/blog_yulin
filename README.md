# 雨林说

[![Hugo](https://img.shields.io/badge/Hugo-PaperMod-blue)](https://github.com/adityatelange/hugo-PaperMod)
[![博客地址](https://img.shields.io/badge/博客-blog.yulin.ltd-green)](https://blog.yulin.ltd/)

这是 **雨林** 的个人博客源码，基于 [Hugo](https://gohugo.io/) 静态网站生成器和 [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题构建，部署在 GitHub Pages 上。

> 我把生活当作一个长期实验项目，用系统思维拆解问题，用小规模试验验证想法，记录每一次迭代的过程。从时间管理到身体训练，从知识输入到表达输出，我希望一点点搭建属于自己的"人生操作系统"。

## 内容方向

- **人生系统** — 时间管理、工具效率、个人工作流优化
- **生活实验** — 影评、阅读、旅行等日常观察与思考
- **产品思考** — 产品与设计相关的想法
- **健身与理财** — 身体管理与个人财务

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
---
```

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
