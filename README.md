# 我的 Hexo 博客

基于 Hexo 7 + NexT 8.29 主题的静态博客，部署在 GitHub Pages。

## 本地使用

```bash
npm install          # 安装依赖（首次）
npx hexo server      # 本地预览 http://localhost:4000
npx hexo new "文章标题"   # 新建文章（生成在 source/_posts/）
npx hexo clean       # 清理缓存
npx hexo generate    # 生成静态页面到 public/
```

## 目录结构

```
├── _config.yml          # Hexo 主配置（站点信息、URL、部署）
├── _config.next.yml     # NexT 主题配置（菜单、搜索、评论）
├── source/
│   ├── _posts/          # 博客文章（Markdown）
│   ├── about/index.md   # 关于页
│   ├── tags/index.md    # 标签页
│   └── categories/index.md  # 分类页
└── scaffolds/           # 新建文章模板
```

## 部署到 GitHub Pages（首次）

1. 在 GitHub 创建仓库：`yourname.github.io`（yourname 为你的 GitHub 用户名）
2. 修改两处配置：
   - `_config.yml` 的 `url:` 改为 `https://yourname.github.io/`
   - `_config.yml` 末尾 `deploy.repo:` 改为你的仓库地址
3. 在博客目录初始化 git 并推送到 GitHub（保存源码）：
   ```bash
   git init
   git add .
   git commit -m "init blog"
   git remote add origin https://github.com/yourname/blog-source.git
   git push -u origin main
   ```
4. 发布静态页面（把 public/ 推到 yourname.github.io 仓库的 main 分支）：
   ```bash
   npx hexo clean && npx hexo deploy
   ```
5. 等 1-2 分钟，访问 `https://yourname.github.io`

> 注意：本目录位于 opencodeTest 仓库内。若 git init 为独立仓库，父仓库会将其视为嵌套仓库，建议把博客目录单独移出或加入父仓库的忽略列表。

## 评论功能（Giscus）

评论基于 GitHub Discussions，无需服务器：

1. 在 GitHub 上为博客源码仓库安装 [giscus app](https://github.com/apps/giscus)
2. 仓库 Settings → General → Features 勾选 **Discussions**
3. 到 [giscus.app](https://giscus.app/zh-CN) 填入仓库名，生成 `repo-id` 和 `category-id`
4. 把生成的参数填入 `_config.next.yml` 的 `giscus:` 段

## 搜索功能

已内置本地搜索（hexo-generator-searchdb + NexT local search），无需配置，
站点右上角搜索图标可用。数据文件为 `public/search.xml`。
