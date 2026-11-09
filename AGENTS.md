# AGENTS.md

本仓库是 skills / subagents / commands 的源头仓库，内容全是 Markdown，没有构建、测试或 lint。安装与分发不在本仓库职责内——怎么装、用不用 Magpie，由使用者决定。

## 先读

新建或修改技能前，加载 `skill-creator` 技能；本仓库现有技能是风格范例。

## 注意

- `agents/AGENTS.md` 软链到 `~/.pi/agent/AGENTS.md`，改动会影响所有项目里的 agent；保持通用，不要写仓库专属内容。
- 其中 `Karpathy` 节是外部原文逐字引入：不要逐行改，要么原样保留，要么整节删除。

## 改完

Markdown 与 YAML 能正常解析；结构有变化时顺手更新 `README.md`。
