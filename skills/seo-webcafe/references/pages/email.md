# 邮箱提取器

把邮箱从一堆文本里捞出来并去重，Outreach 整理联系人用。

## 入口

- URL: https://seo.web.cafe/email/
- 深链: 无（纯本地实时提取）
- 自动执行: 否（需要手动填写/点击）

## 成本

免费（纯本地运行，内容不上传）。

## 操作步骤

1. 打开页面，在 `#input` 粘贴文本。
2. 结果立即出现在 textarea `#output` 的 value 里（不是 innerText）；`#stats` 显示「找到 N 个邮箱（去重前 M 个）」。
3. 用 `document.querySelector('#output').value` 读取，别读 innerText。
4. 按需切换 `#mode`（每行一个/逗号分隔/只要域名）。

## 等待完成

`#stats` 文本包含「找到」

## 提取

#output 的 value + #stats 文本

## 实测样例

示例文本 → 找到 3 个邮箱（去重前 4 个）。

## 失败与边界

- 输出是 textarea 的 value，DOM innerText 为空属于正常现象。

<!-- 验证记录：2026-11-08 live 跑测，见 evals/e2e 与 /tmp/wc-live-*.json（研究记录） -->
