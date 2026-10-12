# 博客功能清单

## 基础框架

- Hexo 静态站点生成 + NexT 主题
- GitHub Pages 托管：https://agnon1573.github.io/Blog/
- 部署：`hexo deploy` → SSH over 443（`git@ssh.github.com:Agnon1573/Blog.git`，gh-pages 分支）
- 源码管理：Blog 仓库 `hexo` 分支

## 内容

- 12 篇 EPA 决策推理章节文章（`source/_posts/第*章-*.html`，2026-10-12 全站统一框架改版）：
  - 嵌入 NexT 布局（article 结构：标题/发表时间/分类/摘要 + 上下篇导航 + giscus 评论）
  - 首页摘要列表：front-matter `description` + `<!--more-->`，首页只显示摘要不渲染题库 JS
  - MathJax 3 由 NexT per-page 按需加载（`mathjax: true`），CDN 覆盖为 jsdelivr `tex-chtml-full.js`
  - 章节内交互保留：知识清单/章节测试 tab、在线作答/自动评分、目录锚点导航
  - 分类/标签：决策推理
- hexo-generator-category 插件：分类页 `/categories/决策推理/`

## EPA → NexT 嵌入改写（改写脚本要点，脚本在 temp\opencode\next_embed.py）

- CSS scope 化：`:root`/`body` → `.epa-scope`（原 body 规则删 max-width/margin，保留字体/背景/padding）；`*` → `.epa-scope, .epa-scope *`；元素/类/id 选择器全部加 `.epa-scope ` 前缀；@media 内递归处理
- 删除 ExamPass 品牌头（`#exampass-header` + `hr.header-divider`）与顶层 `<h1>`（NexT post-title 承担）
- `#panel-kn` 预重排：`.toc` 后兄弟包进 `<div class="epa-main">`（替代原运行时重排 JS，该 JS 已删除）
- 保留 2 个脚本：题库数据 `var Q` + build()、tab 切换 `__epaSwitch`（均带防御性检查，与 NexT 兼容）
- 布局适配 patch（文章内第二个 `<style>`）：
  - ≥1280px：`#panel-kn.active` flex、`.toc` sticky 230px 左侧栏、`.epa-main` flex:1
  - `body:has(.epa-scope)` 隐藏 NexT 主题侧栏（避免双侧栏）、`.main-inner` 加宽 `width: min(1160px, 100%)`（Muse 默认 `.main-inner{width:700px}` 须覆盖 width 而非仅 max-width）
  - table/pre/mjx display 公式横向滚动、`[id]` scroll-margin-top 14px

## 站点风格（博客园式门户）

- 侧边栏：右侧（`sidebar.position: right`），非文章页显示站点概览（头像/日志数/分类数/标签数）
- 首页：摘要 + 阅读全文按钮（`excerpt_description` + `read_more_btn`）
- 搜索（hexo-generator-searchdb 本地搜索）、阅读进度条、返回顶部、代码复制按钮、文章版权声明 CC BY-NC-SA 4.0

## 样式（EPA 文章，scope 化后）

- `.epa-scope` 容器：纸色背景（--paper #fdf6e3）、微软雅黑 13.5pt、padding 28px 36px，嵌在 NexT 白色 post-body 内形成纸张卡片效果
- 宽屏（≥1280px）：TOC 230px sticky 左侧栏 + `.epa-main` 并排约 780px（视口 1440）；`<1280px` 回落 block/static 叠放
- 响应式断点保留：@media 1024px 平板（12pt）；640px 手机（table 横向滚动、pre overflow-x auto）

## 评论

- giscus（GitHub Discussions），注入模板 `source/_data/giscus.njk`，全站文章页可用

## 已修复问题（勿回退）

- 公式内裸 `<` 转义为 `&lt;`（第1章 1 处、第3章 2 处，原始文件与发布文件均已修复）
- 分类页 404（补装 hexo-generator-category）
- HTTPS push 403（改 SSH over 443 + proxy-connect.py 隧道）
