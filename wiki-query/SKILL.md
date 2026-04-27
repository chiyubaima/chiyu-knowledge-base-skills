---
name: wiki-query
description: Answer questions using only Chiyu's Obsidian knowledge base. Use when the user says wiki-query, "从知识库里找", "我之前学过 X 吗", "知识库里有没有关于 X 的内容", asks to review or summarize what the knowledge base says, or wants a source-grounded answer from local wiki pages.
---

# Wiki Query

Answer from the maintained wiki, not from general memory. Cite the relevant `[[wiki pages]]`, prefer judgment frameworks when present, and say clearly when the knowledge base does not contain enough information.

## Vault

Use this vault path exactly:

`/Users/chiyu/Documents/chiyu-knowledge-base`

Read:

- `/Users/chiyu/Documents/chiyu-knowledge-base/index.md`
- Relevant files under `/Users/chiyu/Documents/chiyu-knowledge-base/wiki/`

## Workflow

1. Verify the vault exists.
2. Read `index.md` to understand the knowledge map.
3. Search `wiki/**/*.md` for the user's terms and adjacent concepts.
4. Read the most relevant pages.
5. Answer only from those pages. Use `[[Page Name]]` references in the answer.
6. Prefer `判断框架`, `易混淆点`, and source-backed sections over loose summaries.
7. Mention page quality when useful:
   - `confidence`
   - `evidence_level`
   - `last_reviewed`
   - unresolved `[!contradiction]` blocks
8. If the wiki has no relevant page or insufficient evidence, say so plainly. Do not fill gaps from outside knowledge unless the user explicitly asks for a general answer.
9. If the query itself reveals a useful missing page or recurring question, propose `wiki-spark` or `wiki-ingest`.

## Answer Style

Use a compact structure:

- direct answer
- supporting wiki references
- limitations or missing areas
- optional next action

Example:

```markdown
知识库里有关于 [[CI-CD]] 的内容。当前页面 confidence 是 medium，主要来源是 teacher session，所以它适合复习基本判断框架，但还缺少项目复盘支撑。
```

## Rules

- Do not create or update wiki pages during a normal query unless the user asks.
- Do not cite raw files as if they were maintained wiki conclusions; raw files are source material.
- If a page contains an open contradiction, include that caveat in the answer.
- If a query is valuable enough to preserve, suggest saving it into `wiki/meta/query-log.md` or integrating it through `wiki-ingest`.
