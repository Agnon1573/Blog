# 博客维护规则

## 规则 1：框架变更必须测试并同步 git（最高优先级）

任何 blog 框架层面的变更（包括但不限于：Hexo 版本升级、NexT 主题升级/配置、插件安装/移除/升级、package.json / package-lock.json 变化、_config.yml / _config.next.yml 构建配置调整、Node 版本切换）：

1. **必须先测试**：按 `test.md` 清单完整执行测试，全部通过后才算变更完成。
2. **测试通过后必须同步 git**：将框架相关变更（package.json、package-lock.json、配置文件等）提交并推送到 Blog 仓库 `hexo` 分支（源码分支），保证源码仓库与实际运行框架一致。
3. 禁止只改本地不提交、或未测试就直接部署上线。

## 规则 2：部署与分支

- 部署产物只进 `gh-pages` 分支（通过 `hexo deploy`，SSH over 443：`git@ssh.github.com:Agnon1573/Blog.git`）。
- 源码（文章、配置、主题自定义）只进 `hexo` 分支，两者不混用。
- 每次部署后，若源码有变动，同步提交 `hexo` 分支。

## 规则 3：EPA 文章处理

- 12 篇 EPA 文章保持 `layout: false` 原样输出，不嵌入 NexT 布局，不改内部样式结构。
- 公式文本中裸 `<` 后跟字母（如 `y_t<f_{p,t}`、`<S, A, R, S′>` 且位于数学环境中）必须转义为 `&lt;`，否则浏览器解析为 bogus 标签导致 MathJax 渲染失败。
- 行内代码 `` `<S, A, R, S′>` `` 中的 `<` 属于正常文本，无需处理。
- 修改 EPA 原始文件（D:\learning\决策推理科目二\EPA\）前，先备份到 `C:\Users\Z00504~1\AppData\Local\Temp\opencode\epa-backup\`。

## 规则 4：新增文章

- 新文章 front-matter 必须含 title、date、categories、tags。
- 非 EPA 类文章使用默认 NexT 布局（不写 `layout: false`）。
- 文章日期错开（精确到分钟），保证首页排序稳定。

## 规则 5：隐私安全（红线）

- 严禁提交账号、密码、密钥、token 等隐私类文件或内容到 git（包括 .ssh 私钥、含 token 的配置、账号密码明文）。
- 提交前检查 `git status` / `git diff --staged`，确认无敏感文件；一旦发现误提交，立即从历史中清除并作废泄露的凭据。

## 规则 6：环境注意

- 内网需走代理 proxyuk.huawei.com:8080；git push 大 body 走 HTTPS 会被 403 拦截，用 SSH。
- hexo deploy 输出乱码是 PowerShell GBK 显示问题，不代表失败。
- MathJax 从 cdn.jsdelivr.net 加载，内网可能慢，渲染延迟 ≠ 失败。
