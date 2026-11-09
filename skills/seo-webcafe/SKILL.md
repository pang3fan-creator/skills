---
name: seo-webcafe
description: Operate the seo.web.cafe (哥飞 SEO 工具箱) web UI through a browser to pull live SEO data rather than advice, including keyword difficulty (哥飞版 KD), keyword ideas, demand translation, SERP breakdowns, competitor traffic keywords, domain DR and availability, domain history, website valuation, backlink price checks, AdSense pre-checks, on-page audits, page coaching, Google Trends, Stripe checkout referral leaderboard. Runs on the account's daily web credits, never on paid API tokens (no wc_mcp_ token, no wallet spend). Use when the user wants real SEO numbers, or mentions seo.web.cafe, 哥飞工具箱, 哥飞版 KD, 「查一下这个词的难度」「这个站靠什么词吃流量」「这个域名值多少钱」「帮我选词」「拆解竞品」「页面体检」. Do not use for SEO methodology, content writing or audits that need no live data (those are seo-audit / ai-seo / programmatic-seo / content-builder).
---

# 哥飞 SEO 工具箱（网页版）

通过浏览器操作 `seo.web.cafe` 的网页工具，拿真实的实时数据，全程只消耗账号的**每日赠送额度**（VIP 每日 500 分，网页专用），不碰官方收费 API、不生成 `wc_mcp_` 令牌、不花钱包积分。

核心事实：**大多数工具支持带参数的深链自动执行**。导航到 `https://seo.web.cafe/<tool>/?参数` 后页面会自己跑，你只需要等结果、读 DOM。只有少数页面（DR、批量域名）需要填表点击。

## 前置检查（每次任务开始都做）

1. **浏览器工具**：用 ego-browser（或任何 headed 浏览器 MCP）。先按 ego-browser 技能开一个 TaskSpace；一次任务复用同一个 Page。
2. **登录态与额度**：打开任意工具页后，页面顶部应有「今日剩 N」。没有登录态就先交给用户（handOff）让他登录，**不要尝试代填账号密码**。
3. **额度基线**：记下开始时的「今日剩 N」，结束时再读一次，差值就是本次花费。

读取额度的一段可靠代码（在页面内执行）：

```js
const quota = await page.evaluate(() => {
  const m = document.body.innerText.match(/今日剩\s*(\d+)/);
  return m ? Number(m[1]) : null;
});
```

## 通用执行循环

1. **路由**：按下面的「页面索引」找到对应文件，读 `references/pages/<tool>.md`。一次任务通常要用 2–5 个页面。
2. **深链优先**：按页面文件里的深链参数拼 URL 并导航；页面会自动执行。需要手动填表的页面按文件里的步骤做。
3. **等待完成**：用页面文件给出的「等待完成」条件（通常是 `#status` 隐藏 + 结果容器可见）。不要用固定秒数，也不要用「结果容器长度 > 100」这种会早停的弱条件。
4. **提取**：只读页面 DOM 里真实存在的文本。长结果截取关键部分，不要把整页倒给用户。
5. **报账**：在回答末尾写「本次消耗 N 分（今日剩 M）」。没查到扣分信息时如实说「页面未显示扣分」。

一个完整的 ego-browser 脚本骨架：

```js
const task = await taskSpace("seo-webcafe <任务名>");
const page = task.page("p1");
await page.goto("https://seo.web.cafe/kd/?keyword=ai+image+editor&gl=us", { timeout: 60000 });
await page.waitForFunction(
  "(() => { const s=document.querySelector('#status'); const n=document.querySelector('#scoreNum'); return (!s || s.offsetParent===null) && n && /^\\d+(\\.\\d+)?$/.test(n.textContent.trim()); })()",
  undefined, { timeout: 120000 }
);
const data = await page.evaluate(() => ({
  score: document.querySelector("#scoreNum")?.textContent.trim(),
  level: document.querySelector("#scoreLevel")?.textContent.trim(),
  reasons: document.querySelector("#reasons")?.innerText.trim(),
}));
```

## 成本纪律（最重要）

- **先查价再跑**：每页的开销写在页面里（「每次消耗 X 积分」），运行时现读，不要凭记忆。总表见 `references/costs.md`。
- **贵操作先确认**：预计单次或一轮超过 10 分（关键词拓展、站点出词、需求翻译、需求挖掘机、AdSense 预检、Agent 对话），先把预估告诉用户，得到同意再跑。
- **剩余低于 20 分**：停止大规模查询，把已有结果整理完，提醒用户额度状况（每天北京时间 8:00 重置）。
- **缓存优先**：KD/SERP 缓存 7 天，域名数据/DR 缓存 30 天，趋势有共享缓存；同词重复查通常不扣或扣得少。同一次任务里不要重复查同一个词/域名。
- **免费页面先用**：文本长度、KGR、收入拆解、投流周转、红人报价、邮箱提取、流量 CSV 解析、Spam 查询、Stripe 引荐榜、GSC 与三个模拟器，全部 0 积分（见 `references/costs.md`）。

## 数据诚实

- 数字只能来自页面；页面没有就说「没查到」，不要用记忆或估算补齐。
- 说清口径：KD 是「哥飞版」；流量是 SimilarWeb 估算；DR 来自 Ahrefs；趋势是 0–100 相对热度不是搜索量；估值是区间不是成交价。
- 页面给的结论（如「值得买」「谨慎」「不是新词」）可以引用，但要连带它给出的依据。
- 结果有时效：页面会写搜索/缓存时间，引用时带上日期。

## 页面索引

| 任务 | 页面文件 |
|---|---|
| 判断一个词能不能做（KD 分数、原因、外链预算、前十盘面） | `references/pages/kd.md` |
| 看某个词第一页为什么是它们 | `references/pages/serp.md` |
| 一个种子词拓出一片词 | `references/pages/ideas.md` |
| 一句英文需求 → 有搜索支撑的词 | `references/pages/translate.md` |
| 一个词/站滚雪球挖需求与同类站 | `references/pages/mine.md` |
| 任意网站靠哪些词吃流量 | `references/pages/sitekw.md` |
| 页面 On Page 体检（40+ 项） | `references/pages/audit.md` |
| 页面军师（推断词 + P0/P1/P2 建议） | `references/pages/review.md` |
| 域名 DR（单个/批量） | `references/pages/dr.md` |
| 域名能不能注册（批量生成/粘贴清单） | `references/pages/bulk.md` |
| 域名前世与改版史 | `references/pages/history.md` |
| 网站值多少钱 / 月收入 | `references/pages/worth.md` |
| 这条外链值不值这个价 | `references/pages/backlink.md` |
| AdSense 过审预检（73 项） | `references/pages/adsense.md` |
| 谷歌趋势（新词/回落/适不适合上站） | `references/pages/trends.md` |
| 外链 Spam 判定（免费） | `references/pages/spam.md` |
| Stripe 收银台引荐流量榜（免费） | `references/pages/referring.md` |
| 产品起名（跳转 Agent 执行） | `references/pages/domain.md` |
| SEO Agent 一句话综合问答 | `references/pages/chat.md` |
| 文本长度/TDK 检查（免费） | `references/pages/string.md` |
| KGR / EKGR / KDROI（免费） | `references/pages/kgr.md` |
| 收入目标拆解（免费） | `references/pages/money.md` |
| 投流资金周转（免费） | `references/pages/cashflow.md` |
| 红人报价评估（免费） | `references/pages/influencer.md` |
| 邮箱提取（免费） | `references/pages/email.md` |
| 流量 CSV 分析（免费） | `references/pages/traffic.md` |

跳过的页面：首页、`/api/`（官方付费 API 文档）、`/extension/`、`/level/`（自测分级）、`/gsc/` 与三个模拟器（给人练手，对取数无用）、`/wallet/`。

## 失败时的标准动作

- 掉登录 / 看不到「今日剩」→ handOff 给用户登录，不要绕。
- 额度不足 / 页面提示 402、429、已满 → 停止，报告已查到的结果，告诉用户 8:00 重置或去积分中心；不要换工具硬凑。
- 页面报错（抓取失败、522、目标不是 HTML、上游超时）→ 记录原文错误，换一个可访问的目标；同一目标不要反复重试。
- 页面改版、找不到等待条件里的元素 → 中止并报告「页面结构可能已变」，不要盲跑。
- 详细的失败清单与话术见 `references/failures.md`。

## 不要做的事

- 不用官方 API、CLI 或 MCP 令牌（那是钱包付费路线，本技能存在的意义就是绕开它）。
- 不直接用脚本请求站内 `*/api/*` 内部接口批量取数——`robots.txt` 明确禁止，且容易触发风控。只操作人类可见的页面。
- 不在用户未同意时跑大额查询；不把一次任务的所有词都跑一遍 KD。
- 不把页面结果包装成自己的分析结论；引用数据要带来源与口径。
- 其他浏览器工具（Playwright MCP、Chrome DevTools MCP 等）的适配方式见 `references/browser-adapter.md`。
