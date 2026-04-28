# Knowledge Base Skills - Agent Reuse Guide

> Audience: AI agents that need to reuse or reinstall this Obsidian knowledge-base workflow.
> Goal: Make the system usable quickly without reading the original setup conversation.

## What This Is

This repository contains 6 skills for learning, discussion, capture, ingestion, query, and maintenance:

- `teacher-pro`: unified learning skill for topics, books, papers, articles, and URLs.
- `ljg-roundtable`: structured multi-perspective roundtable discussion.
- `wiki-ingest`: compile raw material into maintained wiki pages.
- `wiki-query`: answer questions using only maintained wiki pages.
- `wiki-lint`: inspect and repair knowledge-base health problems.
- `wiki-spark`: quickly capture small ideas or excerpts into the raw layer.

`teacher-pro` replaces the older separate `teacher` and `read-books` skills. Do not reinstall or refer to those old folders.

## Vault

Use this vault path exactly in this environment:

`/Users/chiyu/Documents/chiyu-knowledge-base`

Always read the vault schema before writing:

`/Users/chiyu/Documents/chiyu-knowledge-base/WIKI.md`

## Current Vault Layout

Expected top-level files:

```text
WIKI.md
index.md
log.md
raw/
wiki/
```

Current raw/wiki layout:

```text
raw/
├── articles/
├── papers/
├── teacher-pro-sessions/
├── roundtable-sessions/
├── sparks/
└── assets/

wiki/
├── concepts/
├── topics/
├── books/
├── people/
└── meta/
```

The `roundtable-sessions/` raw folder is used by `ljg-roundtable`. If the local WIKI schema omits it, treat the live vault directory plus this skill repository as the operational source for roundtable captures.

## Repository Layout

```text
knowledge-base/
├── README.md
├── AGENT_REUSE_GUIDE.md
├── teacher-pro/
│   ├── SKILL.md
│   └── agents/openai.yaml
├── ljg-roundtable/
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   └── references/original-prompt.md
├── wiki-ingest/
│   ├── SKILL.md
│   └── agents/openai.yaml
├── wiki-query/
│   ├── SKILL.md
│   └── agents/openai.yaml
├── wiki-lint/
│   ├── SKILL.md
│   └── agents/openai.yaml
└── wiki-spark/
    ├── SKILL.md
    └── agents/openai.yaml
```

## Skill Selection

| User intent | Skill |
|---|---|
| "我想了解 X" | `teacher-pro` |
| "我想读这本书" | `teacher-pro` |
| "帮我学这篇论文/文章/链接" | `teacher-pro` |
| "圆桌讨论 X" | `ljg-roundtable` |
| "roundtable / 辩论 / 多角度讨论 X" | `ljg-roundtable` |
| "帮我把这个存进知识库" | `wiki-ingest` |
| "ingest 这篇文章/论文/笔记" | `wiki-ingest` |
| "从知识库里找..." | `wiki-query` |
| "我之前学过 X 吗" | `wiki-query` |
| "知识库健康检查" | `wiki-lint` |
| "记一个想法" | `wiki-spark` |
| "先记下来" | `wiki-spark` |

When in doubt:

- Use `teacher-pro` for learning or guided reading.
- Use `ljg-roundtable` for multi-perspective exploration.
- Use `wiki-spark` for fast capture.
- Use `wiki-ingest` for durable synthesis.
- Use `wiki-query` for retrieval.
- Use `wiki-lint` for maintenance.

## Source Type Rules

Every wiki page must carry `source_type`:

- `original`: based on provided source material, such as a book, paper, article, URL, or raw file.
- `general_knowledge`: based on the agent's general knowledge rather than a provided source.

Routing:

- `source_type: original` may create or update `wiki/books/`, `wiki/concepts/`, or `wiki/topics/`.
- `source_type: general_knowledge` must not create `wiki/books/`; route it to `wiki/topics/` or `wiki/concepts/`.
- If unsure, ask before writing.

Required wiki frontmatter:

```yaml
---
title: "概念名"
aliases: []
type: concept
domain: []
layer: ""
status: active
mastery: basic
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - "[[raw/...]]"
source_type: "original"
related: []
confused_with: []
---
```

Required fields are:

```text
title, type, domain, status, created, updated, sources, source_type
```

## `teacher-pro` In One Screen

Use when the user wants to learn a topic, read a book, study a paper/article, or learn from a URL.

Workflow:

1. Decide whether the user provided reliable source material.
2. Mark source basis as `original` or `general_knowledge`.
3. If `general_knowledge`, explicitly tell the user before lesson 1.
4. Ask learning goal, current level, and desired outcome, one question at a time.
5. Plan 4-6 modules with 3-5 lessons each.
6. Create `raw/teacher-pro-sessions/<topic>/`.
7. Maintain `进度.md` with source type, goals, path, mastery status, next lesson, and ingested knowledge.
8. Write each lesson as a complete raw learning record.
9. Use checkpoint questions before moving on.
10. At module end, run synthesis and trigger `wiki-ingest`.

Boundary:

Teacher-pro files are raw learning records. Durable wiki knowledge must be compiled by `wiki-ingest`.

## `ljg-roundtable` In One Screen

Use for structured debate, multi-perspective exploration, or truth-seeking roundtable discussion.

Workflow:

1. Read `ljg-roundtable/references/original-prompt.md`.
2. Extract the core issue.
3. Propose 3-5 real representative figures.
4. Align definitions.
5. Run dynamic response rounds.
6. Summarize each round with a core dispute, ASCII framework, and next question.
7. Continue until the user says `止`.
8. Save the full transcript to `raw/roundtable-sessions/YYYYMMDDTHHMMSS--圆桌-topic__roundtable.md`.
9. Trigger `wiki-ingest`.

Boundary:

Roundtable output is raw discussion material. Wiki synthesis should preserve tensions, positions, judgment frameworks, and open questions instead of flattening debate into one answer.

## `wiki-ingest` In One Screen

Use when source material should become maintained knowledge.

Workflow:

1. Read `WIKI.md` and `index.md`.
2. Read the source file, URL, or pasted text.
3. Determine `source_type`.
4. Identify related wiki pages and likely new pages.
5. Ask up to 3 focus questions if the target is unclear.
6. Create or update concept/topic/book pages.
7. Add `[[bidirectional links]]`.
8. Update `index.md`.
9. Append an `ingest` entry to `log.md`.
10. Summarize created/updated pages and link count.

## `wiki-query` In One Screen

Use when the user asks what the knowledge base says.

Workflow:

1. Read `index.md`.
2. Search `wiki/**/*.md`.
3. Read relevant pages.
4. Answer only from those pages.
5. Cite pages using `[[Page Name]]`.
6. Mention `source_type`, stale pages, or open contradictions when relevant.
7. If the wiki lacks evidence, say so clearly.

## `wiki-lint` In One Screen

Use for health checks and maintenance.

Check:

- Orphan pages.
- Broken wiki links.
- Missing required frontmatter.
- Missing or invalid `source_type`.
- Book pages not based on `source_type: original`.
- Open contradictions.
- Stub pages.
- Stale active pages.

Ask before repairing.

## `wiki-spark` In One Screen

Use for quick capture without full synthesis.

Save to:

`raw/sparks/YYYYMMDD-keywords.md`

Then lightly scan wiki pages for obvious related links. Do not turn a spark into a full concept page unless the user asks for ingest.

## Logging

Every write workflow should append to `log.md`:

```markdown
## [YYYY-MM-DD] 操作类型 | 标题
- 来源：...
- 新建：...
- 更新：...
- 新建链接：...
```

Allowed operation types:

```text
ingest
query
lint
spark
```

Normal read-only `wiki-query` does not need a log entry.

## Common Failure Modes

Avoid these:

- Reinstalling old `teacher` or `read-books`.
- Writing learning-session files outside `raw/teacher-pro-sessions/`.
- Creating book pages from `general_knowledge`.
- Answering `wiki-query` from general model knowledge.
- Saving raw files and calling the task done without ingesting.
- Creating wiki pages without `source_type`.
- Flattening roundtable disagreements into fake consensus.

## Minimal Test

```bash
ls /Users/chiyu/Documents/chiyu-knowledge-base
ls /Users/chiyu/Documents/chiyu-knowledge-base/raw/teacher-pro-sessions
```

Then try:

```text
wiki-query：知识库里有哪些关于 Git 的内容？
```
