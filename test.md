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

## 3. 样式测试（EPA 文章，NexT 嵌入版 2026-10-12）

```js
() => {
  const s = document.querySelector('.epa-scope');
  const cs = s ? getComputedStyle(s) : null;
  return JSON.stringify({
    scope: !!s,
    bg: cs && cs.backgroundColor,        // rgb(253, 246, 227) 纸色
    fontSize: cs && cs.fontSize,          // 18px
    padding: cs && cs.padding             // 28px 36px 60px 起头
  });
}
```

- [ ] scope = true，bg 纸色、fontSize 18px、padding 含 28px 36px
- [ ] NexT 全局无污染：导航字体仍为 Lato/PingFang（非微软雅黑），`document.querySelector('.menu-item a')` fontFamily 不含 "Microsoft YaHei" 开头

### 3.1 目录侧边栏 + 布局测试（视口 1440px，EPA 文章）

```js
() => {
  const panel = document.getElementById('panel-kn');
  const toc = document.querySelector('#panel-kn > .toc');
  const main = document.querySelector('#panel-kn > .epa-main');
  const inner = document.querySelector('.main-inner');
  const sidebar = document.querySelector('.sidebar');
  return JSON.stringify({
    flex: getComputedStyle(panel).display === 'flex',
    tocSticky: getComputedStyle(toc).position === 'sticky',
    tocW: Math.round(toc.getBoundingClientRect().width),          // ≈230
    mainW: Math.round(main.getBoundingClientRect().width),        // ≥700（1440 视口）
    innerW: Math.round(inner.getBoundingClientRect().width),      // ≈1105（Muse 加宽后）
    nexTSidebarHidden: !sidebar || getComputedStyle(sidebar).display === 'none'
  });
}
```

- [ ] 全部符合注释值
- [ ] tab 切换：切到"章节测试" panel-kn 隐藏、题卡渲染（.q-card > 0）、可作答（.option.selected）；切回恢复 flex
- [ ] 锚点跳转：点击 TOC 链接 scrollY 变化
- [ ] 视口 <1280px：panel-kn display=`block`、toc position=`static`（回落叠放）
- [ ] 非 EPA 页（归档/分类/首页）：NexT sidebar 正常显示（站点概览）

### 3.2 首页摘要测试

- [ ] 首页文章卡片显示 description 摘要 + "阅读全文"按钮
- [ ] 首页源码无 `var Q`（题库 JS 不泄漏到首页）、无 `<div class="epa-scope">`

## 4. 线上抽查（部署后）

- [ ] 首页 https://agnon1573.github.io/Blog/ 正常列出文章摘要（见 3.2）
- [ ] 分类页 `/Blog/categories/决策推理/` 可访问
- [ ] 抽查文章页（建议第3章，公式最多）：NexT article 框架（标题/日期/分类/摘要）、TOC 侧栏、公式（见第 2 节）、tab 作答、giscus 评论区加载（新文章显示"Discussion not found"属正常，首评自动创建）
- [ ] 部署后 GitHub Pages 构建需 1~3 分钟，未生效时用 `?v=N` 参数刷新并等待重试

## 5. Git 同步测试（框架变更后必做，见 rule.md 规则 1）

- [ ] `git -C <博客目录> status` 无未提交的框架文件（package.json、package-lock.json、_config*.yml、themes/ 等）
- [ ] 框架变更已提交并推送到 `hexo` 分支
- [ ] gh-pages 分支为最新部署（`hexo deploy` 输出 INFO Deploy done）

## 已知可忽略项

- hexo deploy 输出中文乱码（PowerShell GBK 显示问题）
- MathJax 首次加载慢（cdn.jsdelivr.net，内网代理）
- Playwright MCP 浏览器无法访问 localhost / file: URL（代理拦截），测试用线上 URL 或 data: URL
