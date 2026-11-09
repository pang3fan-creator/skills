# AGENTS.md

本仓库是 `skills` / `subagents` / `commands` / `plugins` / `hooks` / 全局级 `AGENTS.md` 的源头仓库（`plugins` / `hooks` 为预留目录）。内容全是 Markdown，没有构建、测试或 lint。安装与分发不在本仓库职责内——怎么装、怎么用，由使用者决定。

## 先读

新建或修改技能前，加载 `skill-creator` 技能；本仓库现有技能是风格范例。

## 注意

- `agents/AGENTS.md` 是全局级 `AGENTS.md`，其他 Agent 通过软链接加载，改动会影响所有项目里的 agent；保持通用，不要写仓库专属内容。
- `agents/AGENTS.md` 里的 `Karpathy` 节是外部原文逐字引入，不要逐行修改。

## 改完

Markdown 与 YAML 能正常解析；结构有变化时顺手更新 `README.md`。
