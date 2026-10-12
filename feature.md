# 博客功能清单

## 基础框架

- Hexo 静态站点生成 + NexT 主题
- GitHub Pages 托管：https://agnon1573.github.io/Blog/
- 部署：`hexo deploy` → SSH over 443（`git@ssh.github.com:Agnon1573/Blog.git`，gh-pages 分支）
- 源码管理：Blog 仓库 `hexo` 分支

## 内容

- 12 篇 EPA 决策推理章节文章（`source/_posts/第*章-*.html`）：
  - `layout: false` 原样输出，保留 ExamPass Assistant 完整样式
  - MathJax 3 公式渲染（inlineMath `$`/`\(`、displayMath `$$`/`\[`）
  - 章节内交互：知识清单、章节测试按钮、目录锚点导航
  - 分类/标签：决策推理
- hexo-generator-category 插件：分类页 `/categories/决策推理/`

## 样式（EPA 文章，CSS patch 已应用）

- 正文 max-width 1100px（原 860px）、padding 28px 36px
- 字号 13.5pt（原 12pt）
- 响应式断点：@media 1024px 平板（12pt、max-width 100%）；640px 手机（table 横向滚动、pre overflow-x auto）

## 评论

- giscus（GitHub Discussions），注入模板 `source/_data/giscus.njk`，全站文章页可用

## 已修复问题（勿回退）

- 公式内裸 `<` 转义为 `&lt;`（第1章 1 处、第3章 2 处，原始文件与发布文件均已修复）
- 分类页 404（补装 hexo-generator-category）
- HTTPS push 403（改 SSH over 443 + proxy-connect.py 隧道）
