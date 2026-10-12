---
title: Hello World —— 我的第一篇博客
date: 2026-10-12 10:00:00
tags:
  - 博客
  - Hexo
categories:
  - 随笔
---

欢迎来到我的博客！这是使用 Hexo + NexT 主题搭建的静态博客站点。

## 为什么选择静态博客

- **速度快**：纯静态页面，无需数据库和后端
- **免费托管**：GitHub Pages 提供免费域名和 HTTPS
- **写作友好**：用 Markdown 写作，`hexo new "文章标题"` 即可创建新文章

## 代码高亮示例

```python
def hello_world():
    print("Hello, Hexo!")
```

## 如何创建新文章

在项目根目录执行：

```bash
hexo new "我的新文章"
```

Hexo 会在 `source/_posts/` 下生成 Markdown 文件，编辑后执行 `hexo server` 本地预览，`hexo generate` 生成静态页面。

> 评论功能基于 Giscus，登录 GitHub 即可留言。
