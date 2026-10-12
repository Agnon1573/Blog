# 博客测试清单

> 任何框架变更（见 rule.md 规则 1）或内容修改部署后，按本清单执行。全部通过方可收尾。

## 1. 本地构建测试

```powershell
hexo clean; hexo g
```

- [ ] 生成无 ERROR（WARNING 可接受）
- [ ] `public/` 下 12 篇 EPA 文章 HTML 均生成
- [ ] `public/categories/决策推理/index.html` 存在（分类页不 404）

## 2. 公式渲染测试（核心）

在已部署页面（或等价环境）用浏览器执行：

```js
() => {
  const t = document.body.textContent;
  return JSON.stringify({
    mjx: document.querySelectorAll('mjx-container').length,
    residual: (t.match(/\$\$/g) || []).length  // 必须为 0
  });
}
```

- [ ] `mjx` 数量 > 0（抽查页第1、3章应分别有数百~上千）
- [ ] `residual` = 0（无未渲染的 `$$` 残留）
- [ ] 第1章"分位数预测偏差"块级公式完整显示，`<` 显示为小于号
- [ ] 控制台无 MathJax "Missing or unrecognized delimiter" 报错

新增文章后，先检查公式内是否有裸 `<` 后跟字母（见 rule.md 规则 3），有则转义 `&lt;`。

## 3. 样式测试

```js
() => {
  const b = getComputedStyle(document.body);
  return JSON.stringify({ maxWidth: b.maxWidth, fontSize: b.fontSize, padding: b.padding });
}
```

- [ ] maxWidth = `1100px`
- [ ] fontSize = `18px`（即 13.5pt）
- [ ] padding 含 `28px 36px`

## 4. 线上抽查（部署后）

- [ ] 首页 https://agnon1573.github.io/Blog/ 正常列出文章
- [ ] 分类页 `/Blog/categories/决策推理/` 可访问
- [ ] 抽查 1 篇文章页：标题、目录锚点、公式（见第 2 节）
- [ ] giscus 评论区加载（显示"0 条评论"或评论内容）

## 5. Git 同步测试（框架变更后必做，见 rule.md 规则 1）

- [ ] `git -C <博客目录> status` 无未提交的框架文件（package.json、package-lock.json、_config*.yml、themes/ 等）
- [ ] 框架变更已提交并推送到 `hexo` 分支
- [ ] gh-pages 分支为最新部署（`hexo deploy` 输出 INFO Deploy done）

## 已知可忽略项

- hexo deploy 输出中文乱码（PowerShell GBK 显示问题）
- MathJax 首次加载慢（cdn.jsdelivr.net，内网代理）
- Playwright MCP 浏览器无法访问 localhost / file: URL（代理拦截），测试用线上 URL 或 data: URL
