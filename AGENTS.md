# AGENTS.md

illagerCPR 个人主页仓库（GitHub Pages）。单页纯静态站点，无构建、无测试、无依赖。

## 仓库事实

- 默认分支是 `master`（不是 main）。推送 `master` 即触发 GitHub Pages 自动构建部署，无需手动部署。
- 唯一入口：根目录 `index.html`（零依赖，HTML + 内联 CSS/JS，中文界面 + 英文标签）。
- 构建部署约需 1 分钟。验证上线：

  ```bash
  gh api repos/illagerCPR/illagercpr.github.io/actions/runs --jq '.workflow_runs[0] | {status, conclusion}'
  ```

  或直接 curl 检查线上内容。

## 不可改动的内容

- `robots.txt` 原样保留，不得修改。
- `index.html` 中的 Bing 站长认证标签必须保留，值逐字不变：

  ```html
  <meta name="msvalidate.01" content="7CC5F80953B78F78F664A58A171CC826">
  ```

## 页面结构

- 双栏布局：左栏 `.rail` 是 sticky 身份栏（头像、名字、定位语、章节导航、外链、主题开关）；右栏 `main` 是筛选条 + 两个 section。
- 两个 section 固定为「网页项目」`#web` 与「开源项目」`#oss`。
- 网页项目区：2 个 `.plate` 版面 + 2 个目录行 + 1 个 `.soon-item` 占位行。
- 开源项目区：Tiderift 是 feature 行，其余为普通目录行。

## 增删项目

- 新条目必须带 `.entry` 与 `data-kind="web|oss"`、`data-stack="js|py|java|rust|node"`。筛选与计数依赖这两个属性，漏写会导致条目在筛选时消失或计数错误。
- 网页项目链接到 `https://illagercpr.github.io/<name>/`，开源项目链接到 `https://github.com/illagerCPR/<name>`，不要混用前缀。
- 网页项目条目默认带 `.tags`；例外：`10492` 不写标签（保持仅有标题 + 描述）。
- 新增语言时，必须同时添加对应的 `.fchip[data-filter]` 按钮与条目的 `data-stack`，并同步更新 `totals` 计数。
- 占位行使用 `.soon-item`，不可点击；新项目上线时替换它，并始终保留 1 个。
- 头像使用固定 GitHub 头像 URL（`https://avatars.githubusercontent.com/u/63698328?v=4&s=192`），随 GitHub 头像自动更新，勿替换为本地文件。加载失败时脚本回退为 `iC` 字形，不要删除该逻辑。
- favicon 使用根目录 `favicon.ico`（多尺寸 16–256px，`<link rel="icon">` 引用），勿改回内联 data URI。
- 首屏元素用 `.anim` + `style="--i:n"` 做 60ms 递增的进入动画；`prefers-reduced-motion` 下会自动关闭。

## 主题与样式

- 默认跟随系统明暗偏好；手动切换后写入 `localStorage.theme`，以手动选择为准，并监听系统变化（仅在未手动选择时生效）。
- 所有颜色走 `:root` / `html[data-theme="dark"]` 的 CSS 变量，不要在组件里写死颜色。
- 不引入 Web Font、CDN、框架或构建步骤；单文件是硬约束。

## 本地预览

playwright-cli 禁止 `file://` 协议，必须先起本地服务器：

```bash
python3 -m http.server 8787 --bind 127.0.0.1
```

浏览器访问 <http://127.0.0.1:8787/>。验证后清理 `.playwright-cli`。

## 提交

- 提交信息风格：`feat: ...` / `docs: ...` 等英文前缀。
- 提交必须 GPG 签名（密钥指纹 `D2E7DBACB6E233780954B2DEF09BEB5215872019`），提交后检查签名信息。
- 推送到 GitHub 必须使用隐私邮箱 `63698328+illagerCPR@users.noreply.github.com`。
- 完成后推送 `master`，等待构建成功后再报告完成。
