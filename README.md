# Agent Skills

A collection of agent-agnostic skills, subagents, and commands for AI coding agents (Open Code, CodeX, Claude Code, and compatible tools).

## Skills

| Skill | Description |
|-------|-------------|
| **cn-plain-answer** | Constrain Chinese session output to controlled, plain Chinese; explicit `/skill:` invocation only |
| **content-builder** | Take a keyword from SERP research through an approved outline, human-sounding copy, titles, internal links, and a keyword-density report |
| **eli5** | Explain a topic as if to a five-year-old |
| **seo-preflight-gate** | Pre-publish SEO gate: H1, canonical, noindex, hreflang, sitemap, internal links |
| **seo-webcafe** | Pull live SEO data (KD, keyword ideas, SERPs, DR, valuations, on-page audits) from seo.web.cafe through a browser |

## Subagents

| Group | Agents |
|-------|--------|
| **Planning** | `dev-planner`, `product-manager` |
| **Design** | `design-ui-designer`, `design-ux-architect`, `specialized-workflow-architect` |
| **Engineering** | `engineering-backend-architect`, `engineering-frontend-developer`, `engineering-software-architect`, `engineering-security-engineer`, `engineering-devops-automator`, `engineering-sre` |
| **Quality** | `code-reviewer`, `bug-analyzer`, `testing-api-tester` |

See [subagents/子代理功能介绍.md](subagents/子代理功能介绍.md) for responsibilities and a typical collaboration chain.

## Commands

| Command | Description |
|---------|-------------|
| **docs-audit** | Audit and reorganize `docs/` for progressive disclosure, index coverage, and the AGENTS.md doc-update rule |

## Reference

| Doc | Description |
|-----|-------------|
| [local/Analytics/analytics-kit.md](local/Analytics/analytics-kit.md) | Type-safe analytics client for GSC, GA4, Clarity, Bing Webmaster, Ahrefs, and keyword difficulty |

## Layout

```
agents/       Global AGENTS.md guidelines
commands/     Slash commands
docs/         Project documentation
local/        Reference docs kept outside docs/
skills/       Skill packages (SKILL.md + references/, assets/, scripts/, evals/)
subagents/    Subagent definitions
hooks/        Reserved
mcps/         Reserved
plugins/      Reserved
```
