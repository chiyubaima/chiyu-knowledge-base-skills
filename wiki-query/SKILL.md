---
name: wiki-query
description: Answer questions using only Chiyu's Obsidian knowledge base. Use when the user says wiki-query, "从知识库里找", "我之前学过 X 吗", "知识库里有没有关于 X 的内容", asks to review or summarize what the knowledge base says, or wants a source-grounded answer from local wiki pages.
---

# Wiki Query

Answer from the maintained wiki, not from general memory. Start with the hot cache, choose the right query depth, cite relevant `[[wiki pages]]`, prefer judgment frameworks when present, and say clearly when the knowledge base does not contain enough information.

## Vault

Use this vault path exactly:

`/Users/chiyu/Documents/chiyu-knowledge-base`

Read:

- `/Users/chiyu/Documents/chiyu-knowledge-base/wiki/hot.md`
- `/Users/chiyu/Documents/chiyu-knowledge-base/index.md`
- Relevant files under `/Users/chiyu/Documents/chiyu-knowledge-base/wiki/`

## Workflow

1. Verify the vault exists.
2. Choose query mode:
   - Quick: simple recall or "quick" requests.
   - Standard: default.
   - Deep: comprehensive synthesis, comparison, or "tell me everything" requests.
3. Read `wiki/hot.md` first. If it answers the question, use it and cite the pages it mentions.
4. Read `index.md` to understand the knowledge map.
5. Search `wiki/**/*.md` for the user's terms and adjacent concepts.
6. Read relevant pages according to mode.
7. Answer only from those pages. Use `[[Page Name]]` references in the answer.
8. Prefer `判断框架`, `易混淆点`, and source-backed sections over loose summaries.
9. Mention page quality when useful:
   - `source_type`
   - `mastery`
   - `status`
   - `updated`
   - unresolved `[!contradiction]` blocks
10. If the wiki has no relevant page or insufficient evidence, say so plainly. Do not fill gaps from outside knowledge unless the user explicitly asks for a general answer.
11. If the answer is reusable, propose `wiki-save`. If the gap needs sources, propose `wiki-ingest` or `teacher-pro`.

## Query Modes

Quick:

- Read `wiki/hot.md` and `index.md` only.
- Use for fast recall and recent-context questions.
- If not enough, say that a standard query is needed.

Standard:

- Read `wiki/hot.md`, `index.md`, and 3-5 relevant wiki pages.
- Use for most questions.

Deep:

- Read `wiki/hot.md`, `index.md`, relevant sub-indexes, and all clearly relevant pages.
- Use for cross-topic synthesis and comparisons.
- If the result is valuable, recommend `wiki-save`.

## Answer Style

Use a compact structure:

- direct answer
- supporting wiki references
- limitations or missing areas
- optional next action

Example:

```markdown
知识库里有关于 [[CI-CD]] 的内容。当前页面 source_type 是 general_knowledge，所以它适合复习通用判断框架；如果要作为某本书或论文的结论，需要补充 original 来源。
```

## Rules

- Do not create or update wiki pages during a normal query unless the user asks.
- Do not cite raw files as if they were maintained wiki conclusions; raw files are source material.
- If a page contains an open contradiction, include that caveat in the answer.
- If a query is valuable enough to preserve, suggest saving it through `wiki-save`.
