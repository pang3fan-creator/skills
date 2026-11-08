# 关键词价值评估（KGR）

输入 intitle 结果数、月搜索量、KD，一次算出 KGR / EKGR / KDROI。

## 入口

- URL: https://seo.web.cafe/kgr/
- 深链: 无（纯本地计算）
- 自动执行: 否（需要手动填写/点击）

## 成本

免费（纯本地运算）。

## 操作步骤

1. 打开页面，填 `#intitle`、`#volume`、`#kd`。
2. 点击 `#go`「计算三项指标」。
3. 读取 `#result`：三个指标 + 判断 + 外链投入回报。

## 等待完成

`#result` 文本 >20

## 提取

#result 全文

## 实测样例

intitle 120 / volume 1000 / KD 30 → KGR 0.120（低竞争黄金词）、EKGR 0.156（中等）、KDROI -69.2%（回报覆盖不了外链成本）。

## 失败与边界

- 三个输入是自填值，数字质量取决于用户给的 intitle/搜索量/KD 口径。

<!-- 验证记录：2026-11-08 live 跑测，见 evals/e2e 与 /tmp/wc-live-*.json（研究记录） -->
