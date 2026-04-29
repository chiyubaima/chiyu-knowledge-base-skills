# Knowledge Base Skills - Agent Reuse Guide

> Audience: AI agents that need to reuse or reinstall this Obsidian knowledge-base workflow.
> Goal: Make the system usable quickly without reading the original setup conversation.

## What This Is

This repository contains 7 skills:

- `teacher-pro`: unified learning skill for topics, books, papers, articles, and URLs.
- `ljg-roundtable`: structured multi-perspective roundtable discussion.
- `wiki-ingest`: compile raw material into maintained wiki pages.
- `wiki-query`: answer questions using only maintained wiki pages.
- `wiki-save`: save valuable conversation output into the wiki.
- `wiki-lint`: inspect and repair knowledge-base health problems.
- `wiki-spark`: quickly capture small ideas or excerpts into the raw layer.

`teacher-pro` replaces the older separate `teacher` and `read-books` skills. Do not reinstall or refer to those old folders.

## Vault

Use this vault path exactly in this environment:

`/Users/chiyu/Documents/chiyu-knowledge-base`

Always read the vault schema before writing:

`/Users/chiyu/Documents/chiyu-knowledge-base/WIKI.md`

## Current Vault Layout

```text
raw/
├── .manifest.json
├── articles/
├── papers/
├── teacher-pro-sessions/
├── roundtable-sessions/
├── sparks/
└── assets/

wiki/
├── hot.md
├── overview.md
├── sources/
├── concepts/
├── topics/
├── books/
├── questions/
├── people/
└── meta/
```

Top-level files:

```text
WIKI.md
index.md
log.md
raw/
wiki/
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
| "wiki-query deep ..." | `wiki-query` |
| "保存这个回答" | `wiki-save` |
| "把这段讨论存进知识库" | `wiki-save` |
| "知识库健康检查" | `wiki-lint` |
| "记一个想法" | `wiki-spark` |

When in doubt:

- Use `teacher-pro` for learning or guided reading.
- Use `ljg-roundtable` for multi-perspective exploration.
- Use `wiki-spark` for fast capture.
- Use `wiki-ingest` for source-to-wiki synthesis.
- Use `wiki-query` for retrieval.
- Use `wiki-save` for valuable chat output.
- Use `wiki-lint` for maintenance.

## Source Type Rules

Every wiki page must carry `source_type`:

- `original`: based on provided source material, such as a book, paper, article, URL, or raw file.
- `general_knowledge`: based on the agent's general knowledge rather than a provided source.

Routing:

- `source_type: original` may create or update `wiki/books/`, `wiki/concepts/`, or `wiki/topics/`.
- `source_type: general_knowledge` must not create `wiki/books/`; route it to `wiki/topics/`, `wiki/concepts/`, or `wiki/questions/`.
- If unsure, ask before writing.

Required wiki fields:

```text
title, type, domain, status, created, updated, sources, source_type
```

## Runtime Files

### `wiki/hot.md`

Recent context cache. Read first during query. Update after ingest, save, major query, roundtable completion, and teacher-pro module synthesis. Keep under about 500 words.

### `raw/.manifest.json`

Tracks raw file hashes and ingest outputs. Check it before ingesting a raw file. If the same path and hash already exist, do not re-ingest unless the user says `force`.

### `wiki/sources/`

One source summary page per substantial raw source. Concept/topic/book pages should cite source summary pages when available.

### `wiki/questions/`

Saved answers, research questions, and durable synthesis from user queries or roundtable questions.

### `wiki/meta/lint-report-YYYY-MM-DD.md`

Persistent lint report for each health check.

## `teacher-pro` In One Screen

1. Decide whether the user provided reliable source material.
2. Mark source basis as `original` or `general_knowledge`.
3. If `general_knowledge`, explicitly tell the user before lesson 1.
4. Ask learning goal, current level, and desired outcome, one question at a time.
5. Plan 4-6 modules with 3-5 lessons each.
6. Create `raw/teacher-pro-sessions/<topic>/`.
7. Maintain `进度.md`.
8. Write each lesson as a complete raw learning record.
9. Use checkpoint questions before moving on.
10. At module end, run synthesis and trigger `wiki-ingest`.

## `ljg-roundtable` In One Screen

1. Read `ljg-roundtable/references/original-prompt.md`.
2. Extract the core issue.
3. Propose 3-5 real representative figures.
4. Align definitions.
5. Run dynamic response rounds.
6. Summarize each round with a core dispute, ASCII framework, and next question.
7. Continue until the user says `止`.
8. Save the full transcript to `raw/roundtable-sessions/YYYYMMDDTHHMMSS--圆桌-topic__roundtable.md`.
9. Trigger `wiki-ingest`.

## `wiki-ingest` In One Screen

1. Read `WIKI.md`, `wiki/hot.md`, and `index.md`.
2. Read the source file, URL, or pasted text.
3. Check `raw/.manifest.json`.
4. Determine `source_type`.
5. Create or update a `wiki/sources/` page for substantial sources.
6. Create or update concept/topic/book/question pages.
7. Add `[[bidirectional links]]`.
8. Update `index.md`, `wiki/overview.md`, `wiki/hot.md`, and `log.md`.
9. Update `raw/.manifest.json`.
10. Summarize created/updated pages and link count.

## `wiki-query` In One Screen

Modes:

- Quick: `wiki/hot.md` + `index.md` only.
- Standard: hot + index + 3-5 relevant pages.
- Deep: broad synthesis across all relevant pages.

Answer only from wiki pages. If the result is valuable, suggest `wiki-save`.

## `wiki-save` In One Screen

Use for valuable chat output, decisions, frameworks, and reusable answers.

1. Identify the content worth saving.
2. Ask for a short title if needed.
3. Choose `wiki/questions/`, `wiki/topics/`, or `wiki/meta/`.
4. Write required frontmatter.
5. Link related pages.
6. Update `index.md`, `log.md`, and `wiki/hot.md`.

## `wiki-lint` In One Screen

Check:

- Orphan pages.
- Broken wiki links.
- Missing required frontmatter.
- Missing or invalid `source_type`.
- Book pages not based on `source_type: original`.
- Manifest references to missing pages.
- Source pages missing raw links.
- Open contradictions.
- Stub pages.
- Stale active pages.

Write `wiki/meta/lint-report-YYYY-MM-DD.md` and ask before repairing.

## `wiki-spark` In One Screen

Save to:

`raw/sparks/YYYYMMDD-keywords.md`

Keep sparks lightweight. Promote clusters through `wiki-ingest`.

## Common Failure Modes

Avoid these:

- Writing learning-session files outside `raw/teacher-pro-sessions/`.
- Creating book pages from `general_knowledge`.
- Answering `wiki-query` from general model knowledge.
- Saving raw files and calling the task done without ingesting.
- Creating wiki pages without `source_type`.
- Re-ingesting unchanged raw files without checking the manifest.
- Flattening roundtable disagreements into fake consensus.

## Minimal Test

```bash
ls /Users/chiyu/Documents/chiyu-knowledge-base/wiki/hot.md
ls /Users/chiyu/Documents/chiyu-knowledge-base/raw/.manifest.json
```

Then try:

```text
wiki-query quick：最近知识库在关注什么？
```
