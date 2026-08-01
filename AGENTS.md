# AGENTS.md

illagerCPR 个人主页仓库（GitHub Pages）。单页纯静态站点，无构建、无测试、无依赖。

## 仓库事实

- 默认分支是 `master`（不是 main）。推送 `master` 即触发 GitHub Pages 自动构建部署，无需手动部署。
- 唯一入口：根目录 `index.html`（零依赖，HTML + 内联 CSS/JS，中英文界面）。
- 构建部署约需 1 分钟，验证上线：`gh api repos/illagerCPR/illagercpr.github.io/actions/runs --jq '.workflow_runs[0] | {status, conclusion}'`，或 curl 检查线上内容。

## 页面约定

- 卡片分两类：网页项目（链接到 `https://illagercpr.github.io/<name>/`）和开源项目（链接到 `https://github.com/illagerCPR/<name>`）。新卡片必须归属正确的 section 并带对应前缀的链接。
- 网页项目卡片默认带 `.meta` 标签；例外：`10492` 卡片不写标签（保持仅有标题 + 描述）。
- 网页项目区末尾保留 2 个 `.card.soon` 占位卡片，新项目上线时替换其中一张（保留至少 1 个占位）。
- hero 头像使用固定 GitHub 头像 URL（`https://avatars.githubusercontent.com/u/63698328?v=4&s=192`），随 GitHub 头像自动更新，勿替换为本地文件。
- 新卡片添加到 `.grid` 容器内并带 `fade` 类（滚动淡入动画）。

## 本地预览

playwright-cli 禁止 `file://` 协议，必须先起本地服务器再验证：

```powershell
Start-Process python -ArgumentList "-m","http.server","8787","--bind","127.0.0.1" -WorkingDirectory "E:\ClaudeCode\github-main-page" -WindowStyle Hidden
```

验证后清理：`Remove-Item .playwright-cli -Recurse -Force`。

## 提交

- 提交信息风格：`feat: ...` / `docs: ...` 等英文前缀。
- 完成后推送 `master` 并等待构建成功再报告完成。
