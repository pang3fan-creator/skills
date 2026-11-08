# 浏览器执行层适配

本技能默认用 **ego-browser**（已验证）。其他浏览器工具只要能做到下列能力，就能照同一批页面文件执行。

## 必需的浏览器能力

1. 打开/导航到 URL；
2. 等待页面内 JS 条件（轮询执行一个 JS 表达式直到为真）；
3. 在页面里执行 JS 读 DOM（`innerText`、`textContent`、`textarea.value`）；
4. 填表单、点击按钮、上传本地文件；
5. 把控制权交还用户（登录、人工确认时）。

## ego-browser 用法要点

- 用 `ego-browser nodejs` 执行脚本；一个任务一个 TaskSpace，复用同一个 Page。
- `page.goto(url)` → `page.waitForFunction(expr, undefined, {timeout})` → `page.evaluate(fn)`。
- 页面内 JS 用字符串表达式传入 `waitForFunction`，例如：
  `"(() => { const s=document.querySelector('#status'); return (!s || s.offsetParent===null) && document.querySelector('#result')?.innerText.trim().length>200; })()"`
- 读取本机文件：脚本里用 `await import("node:fs/promises")`；写文件用绝对路径。
- 用户接管时任务会停：不要重试，等用户回来再继续同一 TaskSpace。
- 详细 API 以 ego-browser 技能自带的 guide 为准（先跑 `ego-browser skill`）。

## 其他浏览器（Claude Code / Codex 等）

- 有 Playwright / Chrome DevTools MCP 的 agent：把上面的能力逐条映射过去，页面文件不需要改。
- 没有浏览器工具的 agent：本技能退化为「人工操作手册」——把页面文件里的 URL、参数、步骤、提取字段念给用户，让用户自己查、把结果贴回来。不要伪造数据。

## 安全与边界

- 只在 `seo.web.cafe` 域内操作；不碰浏览器里其他站点的标签页。
- 不读取、不导出用户的 cookie/令牌；登录态由用户在浏览器里自己持有。
- 不并发开多个浏览器任务；不批量请求站内 `*/api/*` 接口（robots.txt 禁止）。
- 每次任务结束在读一次额度，把消耗写进回答。
