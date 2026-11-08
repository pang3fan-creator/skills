# E2E 工件：pdf→video 方向选词（2026-11-08）

验证 `skills/seo-webcafe` 的完整执行链路：ego-browser 驱动 `seo.web.cafe` 网页工具，全程只消耗账号每日赠送额度，不走官方付费 API。

- 环境：ego-browser（headed）、Web.Cafe 账号（VIP）、浏览器内已登录
- 开始额度：**29 分**（页面「今日剩」）
- 结束额度：**15 分**
- 本次总消耗：**14 分**
- 运行时间：2026-11-08T13:12:31.350Z → 2026-11-08T21:13:29
- 原始记录：`/tmp/wc-e2e.json`、`/tmp/wc-e2e2.json`（研究阶段临时文件，可复跑生成）

## 执行步骤

1. **关键词拓展**：导航深链 `https://seo.web.cafe/ideas/?q=pdf+to+video&gl=us`，页面自动执行（未点击任何按钮）。
   - 等待条件：`#tbody tr` 数量 > 3 且 `#status` 隐藏
   - 结果：返回 100 行；额度 29 → 19（扣 **10 分**，符合「每 50 词 5 分」）
   - 采样前 3 行（页面原文）：
     - `pdf` · 450k · → +10% · $3.04 · 低 · 预筛难度 100 · 信息
     - `pdf editor` · 246k · → 0% · $5.1 · 中 · 66 · 交易
     - `i love pdf` · 246k · → -7% · $1.77 · 低 · 30 · 信息
   - 观察：种子「pdf to video」被拓成了泛 PDF 词，意图漂移——这类结果不能直接当候选。

2. **按规则筛候选**：过滤 搜索量 ≥ 500、预筛难度 ≤ 45，按搜索量取前 2 个，逐个跑 KD 精评。

3. **KD 精评**（深链 `https://seo.web.cafe/kd/?keyword=<词>&gl=us`，等 `#scoreNum` 为数字且 `#status` 隐藏）：
- **i love pdf** → `44.5` / 中等 · 品牌词；月搜索量（12 个月均值）：903.9 万 · 快速上升期 ↑；扣分 19→19（耗时 3369ms）
- **pdf merge** → `59.7` / 中等；；扣分 19→18（耗时 6582ms）
- **pdf to video** → `42.6` / 中等；；扣分 18→16（耗时 5289ms）
- **pdf to video converter** → `30.4` / 容易；；扣分 16→15（耗时 3711ms）
   - 注意 `i love pdf` 被识别为**品牌词**：页面同时给出品牌词口径 44.5 与常规口径 79.4，引用时必须说明「44.5 是第三方以衍生内容进入的难度」。
   - `pdf to video converter` 30.4「容易」是这轮里唯一明显可做的位。

## 成本账

| 步骤 | 页面 | 额度前 | 额度后 | 扣分 |
|---|---|---|---|---|
| 拓词 | ideas | 29 | 19 | 10 |
| KD 精评 | kd（i love pdf） | 19 | 19 | 0（命中缓存） |
| KD 精评 | kd（pdf merge） | 19 | 18 | 1 |
| KD 精评 | kd（pdf to video） | 18 | 16 | 2 |
| KD 精评 | kd（pdf to video converter） | 16 | 15 | 1 |

结论：**成本主要来自拓词（10 分），精评单次 1–2 分**。一轮「拓词 + 4 个词精评」≈ 14 分，按每日 500 分大约能跑 30+ 轮。

## 结果与建议

- 「pdf to video」方向确实存在可做的词：`pdf to video converter`（KD 30.4，容易）、`pdf to video`（KD 42.6，中等），且都来自 `translate` 页对「convert pdf lecture notes into short videos」的翻译结果——**先 translation 再 KD 精评**比直接拿种子词拓词更准（本次拓词漂移了）。
- 不建议碰 `i love pdf` 这类品牌词（正面争夺口径 79.4）。
- 下一步可做：对 `pdf to video converter` 用 `serp` 看前十盘面，或在 `audit` 检测自己的落地页。

## 复跑方式

按 `SKILL.md` 前置检查 → 读 `references/pages/ideas.md` / `kd.md` → 用同样的深链与等待条件执行。注意拓词一次 10 分、先确认预算。
