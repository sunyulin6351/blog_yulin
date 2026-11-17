# 站点维护说明

## 本地命令

```bash
hugo server -D
hugo --minify
```

构建产物默认写入 `public/`，日常预览可使用 `hugo --minify --destination .build`。

## 内容约定

- 文章放在 `content/posts/`，文件名使用 `YYYYMMDD.md`。
- 长文要写 `slug`，避免以后修改标题时改变 URL。
- `description` 控制在 120 字以内，用于 meta description 和分享卡片。
- 修改旧文时补 `lastmod: YYYY-MM-DD`，它会用于 sitemap 和 JSON-LD 的 `dateModified`。
- 微记录放在 `content/micro/`，由 `microblog` workflow 从 Issue 自动生成。

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
