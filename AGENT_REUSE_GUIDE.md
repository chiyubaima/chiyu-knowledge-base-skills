# Chiyu Knowledge Base Skills - Agent Reuse Guide

> Audience: AI agents that need to reuse or reinstall Chiyu's Obsidian knowledge-base workflows.
> Goal: Make the system usable quickly without reading the original setup conversation.

## What This Is

This repository contains 4 Codex-compatible skills for maintaining an Obsidian-based personal knowledge base:

- `wiki-ingest`: compile new source material into maintained wiki pages.
- `wiki-query`: answer questions using only existing wiki pages.
- `wiki-lint`: inspect and repair knowledge-base health problems.
- `wiki-spark`: quickly capture small ideas or excerpts into the raw layer.

The knowledge base itself is not stored in this repository. It lives in Obsidian:

`/Users/chiyu/Documents/chiyu-knowledge-base`

Core principle:

```text
raw source material -> agent compilation -> maintained wiki pages
```

Do not treat saved raw notes as finished knowledge. Raw files preserve source records; wiki pages are the maintained synthesis layer.

## Repository Layout

```text
knowledge-base/
├── README.md
├── AGENT_REUSE_GUIDE.md
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

Each skill is self-contained. Read the relevant `SKILL.md` before executing that workflow.

## Expected Vault Layout

Before using the skills, verify the local vault exists:

```bash
ls /Users/chiyu/Documents/chiyu-knowledge-base
```

Expected top-level files and folders:

```text
WIKI.md
index.md
log.md
raw/
wiki/
```

Expected content layout:

```text
chiyu-knowledge-base/
├── WIKI.md
├── raw/
│   ├── articles/
│   ├── papers/
│   ├── teacher-sessions/
│   ├── read-books-sessions/
│   ├── sparks/
│   └── assets/
├── wiki/
│   ├── concepts/
│   ├── topics/
│   ├── books/
│   ├── people/
│   └── meta/
├── index.md
└── log.md
```

If the vault is missing, stop and ask the user to sync/open the Obsidian vault first.

## How To Install Or Reuse

For Codex-style skill loading, place the 4 skill folders where the target agent can discover local skills. Common options:

- Keep this repository as the current workspace and explicitly read/use the skill folders.
- Copy the 4 folders into the agent's skills directory.
- Install from the GitHub repository if the agent supports skill installation from repos.

The GitHub repository is:

`https://github.com/chiyubaima/chiyu-knowledge-base-skills`

After installation, verify that the agent can see these skill names:

```text
wiki-ingest
wiki-query
wiki-lint
wiki-spark
```

## Skill Selection

Use this decision table:

| User intent | Skill |
|---|---|
| "帮我把这个存进知识库" | `wiki-ingest` |
| "ingest 这篇文章/论文/笔记" | `wiki-ingest` |
| "从知识库里找..." | `wiki-query` |
| "我之前学过 X 吗" | `wiki-query` |
| "知识库里有没有关于 X 的内容" | `wiki-query` |
| "检查一下知识库" | `wiki-lint` |
| "知识库健康检查" | `wiki-lint` |
| "记一个想法" | `wiki-spark` |
| "存一条摘录" | `wiki-spark` |
| "先记下来" | `wiki-spark` |

When in doubt:

- Use `wiki-spark` for fast capture.
- Use `wiki-ingest` for durable synthesis.
- Use `wiki-query` for retrieval.
- Use `wiki-lint` for maintenance.

## Operating Model

The vault has two layers:

### Raw Layer

Path:

`/Users/chiyu/Documents/chiyu-knowledge-base/raw/`

Purpose:

- Preserve original sources.
- Store teacher/read-books outputs.
- Capture sparks and excerpts.

Default rule:

Do not rewrite existing raw files. Add new raw files when the user provides new material or explicitly corrects a source record.

### Wiki Layer

Path:

`/Users/chiyu/Documents/chiyu-knowledge-base/wiki/`

Purpose:

- Maintain synthesized concept, topic, book, people, and meta pages.
- Add bidirectional links.
- Resolve or track contradictions.
- Keep knowledge durable and queryable.

Default rule:

Agents may update wiki pages, `index.md`, and `log.md` according to the active skill's workflow.

## `wiki-ingest` In One Screen

Use when new source material should become maintained knowledge.

Workflow:

1. Read `/Users/chiyu/Documents/chiyu-knowledge-base/WIKI.md`.
2. Read `/Users/chiyu/Documents/chiyu-knowledge-base/index.md`.
3. Read the source from a file, URL, or pasted text.
4. Save the source to `raw/` if it is not already saved.
5. Identify existing related wiki pages and possible new pages.
6. Ask up to 3 focus questions if the integration target is unclear.
7. Create or update pages under `wiki/concepts/`, `wiki/topics/`, or `wiki/books/`.
8. Add `[[bidirectional links]]`.
9. Update `index.md` when new pages or important entry points are added.
10. Append an operation entry to `log.md`.
11. Summarize created/updated pages and link count.

New concept pages should include:

```yaml
---
title: "概念名"
aliases: []
type: concept
domain: []
layer: ""
status: active
mastery: basic
confidence: low
evidence_level: personal
created: YYYY-MM-DD
updated: YYYY-MM-DD
last_reviewed:
sources:
  - "[[raw/...]]"
related: []
confused_with: []
---
```

Do not silently overwrite contradictions. Use a contradiction callout with status:

```markdown
> [!contradiction] 待核实
> status: open
> 来源A（日期）说：...
> 来源B（日期）说：...
```

When clarified:

```markdown
> [!contradiction] 已澄清
> status: clarified
> resolution: ...
> resolved_at: YYYY-MM-DD
```

Add `不适用场景`, `常见误用`, or `反例` only when the page is decision-oriented and the content supports those sections.

## `wiki-query` In One Screen

Use when the user wants to know what the knowledge base says.

Workflow:

1. Read `index.md`.
2. Search relevant files under `wiki/**/*.md`.
3. Read the most relevant pages.
4. Answer only from those wiki pages.
5. Cite pages using `[[Page Name]]`.
6. Prefer `判断框架`, `易混淆点`, and source-backed sections.
7. Mention low confidence, missing review dates, or open contradictions when relevant.
8. If the wiki lacks evidence, say so clearly.

Do not invent content from general model knowledge unless the user explicitly asks for a general answer outside the knowledge base.

## `wiki-lint` In One Screen

Use for health checks and maintenance.

Workflow:

1. Read `WIKI.md`.
2. Scan `wiki/**/*.md`.
3. Produce a prioritized report.
4. Ask the user which issues to repair.
5. Apply confirmed repairs only.
6. Append results to `wiki/meta/lint-history.md`.
7. Append a `lint` entry to `log.md`.

High-priority checks:

- Orphan pages.
- Broken wiki links.
- Missing required frontmatter.
- Open contradictions.

Medium-priority checks:

- Stub pages.
- Missing obvious links.
- Active pages that are stale or missing `last_reviewed`.
- Pages missing `confidence` or `evidence_level`.

Exploratory checks:

- Repeated mentions of concepts without concept pages.
- Uncreated `后续延伸` pages.
- Clusters of sparks that deserve ingest.

## `wiki-spark` In One Screen

Use for quick capture without full synthesis.

Workflow:

1. Receive pasted text, excerpt, or an idea.
2. Extract 2-4 keywords for the filename.
3. Save to `raw/sparks/YYYYMMDD-keywords.md`.
4. Use the standard spark format:

```markdown
---
date: YYYY-MM-DD
type: spark
tags: []
---

# 标题

> 原文/想法内容

---
*记录时间：YYYY-MM-DD*
```

5. Lightly scan `wiki/` for obvious related pages.
6. Report the saved path and likely related pages.

Do not turn a spark into a full concept page unless the user asks for ingest.

## Teacher And Read-Books Integration

If a teacher-style session produces a knowledge card:

1. Save the knowledge card and conversation essence to:

   `raw/teacher-sessions/YYYYMMDD-concept.md`

2. Run `wiki-ingest` on that raw file.
3. Preserve the session's `判断框架` in the concept page's `判断框架` section.

If a read-books session finishes a module:

1. Save session files under:

   `raw/read-books-sessions/<book-name>/`

2. Maintain or create:

   `wiki/books/book-<book-name>.md`

3. Run `wiki-ingest` on the module outputs.
4. Update the book page and related concept pages.

## Required Logging

Every write workflow should append to `log.md`.

Use this shape:

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

For normal `wiki-query`, do not write to `log.md` unless the query creates or updates durable knowledge.

## Quality Rules

- Keep each ingest focused on 1-3 core points.
- Prefer durable judgment frameworks over broad summaries.
- Use `[[bidirectional links]]` for concepts.
- Link text should match the target page title.
- Track contradictions instead of hiding them.
- Keep confidence conservative.
- Treat `mastery` as the user's learning depth.
- Treat `confidence` as the page's reliability.
- Treat `evidence_level` as the source basis: `personal`, `secondary`, `primary`, or `mixed`.
- Treat `last_reviewed` as a maintenance signal.

## Common Failure Modes

Avoid these:

- Saving an article into `raw/` and calling the task done.
- Answering a `wiki-query` from general knowledge instead of wiki pages.
- Rewriting raw source records during maintenance.
- Adding every possible concept from a source in one ingest.
- Creating concept pages without updating `index.md` and `log.md`.
- Resolving contradictions without evidence or user confirmation.
- Adding `反例` sections to every page mechanically.

## Minimal First Test

After installing the skills, run these checks:

```bash
ls /Users/chiyu/Documents/chiyu-knowledge-base
ls /Users/chiyu/Documents/chiyu-knowledge-base/wiki/concepts
```

Then try a read-only query:

```text
wiki-query：知识库里有哪些关于 Git 的内容？
```

Expected behavior:

- The agent reads `index.md`.
- The agent searches `wiki/`.
- The agent answers only from matching pages.
- The agent names the relevant `[[wiki pages]]`.

If that works, the system is ready for normal use.
