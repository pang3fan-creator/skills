---
description: 按渐进式披露规范审查并整理 docs/：拆分过长文档、补全索引链接、检查 AGENTS.md 的文档更新要求
argument-hint: "[path]"
---

# Documentation audit

Apply the project's documentation conventions. Default target is `./docs`; use `$ARGUMENTS` when given.

## Conventions

1. **One home.** Documentation pages live under `docs/`. Two files stay at the project root by design: `README.md` (the index) and `AGENTS.md` (agent instructions). Tool configs and generated files are the other exceptions.
2. **Progressive disclosure.** Every file stays small. The entry file holds a summary and a table of contents; details live in per-section files that link back.
3. **Index at the root.** The root `README.md` links to the entry files, so any page is reachable in two hops.
4. **Docs are part of the change.** `AGENTS.md` requires that a behavior change updates its docs. Rules get skipped in practice, so this audit exists.

## Workflow

1. Map the docs: every file under `docs/`, plus the root `README.md` and `AGENTS.md`. Record each file's line count, outbound links, and inbound links. Flag files nothing links to, and links that resolve to nothing.
2. Check the conventions in order. Later checks depend on the shape the earlier ones produce.
3. Report before changing anything. One table: convention, file, what is wrong, smallest fix. When a file breaks several conventions, fix the split before the links.
4. Ask which fixes to apply. Restructuring moves content, so never delete text without approval.

## Judgement calls

- Split when a file holds several sections that stand on their own, and a reader looking for one must scroll past the others. Do not split a file that reads as one argument just to satisfy a line count.
- "Small" means a reader can hold the file in one or two screens. Length is a symptom; the real test is whether the file has one job.
- Links are the structure. A split that leaves no link from the entry file has made the docs worse.
- Keep links relative. Absolute paths break as soon as the repo moves.
- Do not add empty scaffolding: no index entries for pages that do not exist, no placeholder sections.

## Output

Report each convention as PASS or FAIL with evidence, then the fix list. After applying fixes, re-check the link graph: every file reachable, every link resolving.
