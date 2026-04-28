---
name: wiki-ingest
description: Compile new material into Chiyu's Obsidian knowledge base. Use when the user says wiki-ingest, asks to store an article/paper/note/session in the knowledge base, says "帮我把这个存进知识库", "ingest 这篇文章/论文/笔记", or provides a source that should become maintained wiki pages rather than a raw archive.
---

# Wiki Ingest

Turn new material into maintained wiki knowledge. Do not merely archive content: save the source in the raw layer when needed, read the existing knowledge map, extract 1-3 focused points, update concept/topic/book pages, link related pages, and append the operation log.

## Vault

Use this vault path exactly:

`/Users/chiyu/Documents/chiyu-knowledge-base`

Important files:

- Schema: `/Users/chiyu/Documents/chiyu-knowledge-base/WIKI.md`
- Index: `/Users/chiyu/Documents/chiyu-knowledge-base/index.md`
- Log: `/Users/chiyu/Documents/chiyu-knowledge-base/log.md`
- Raw layer: `/Users/chiyu/Documents/chiyu-knowledge-base/raw/`
- Wiki layer: `/Users/chiyu/Documents/chiyu-knowledge-base/wiki/`

## Workflow

1. Verify the vault exists and read `WIKI.md` plus `index.md`.
2. Read the source from a file path, URL, or pasted text. If the source is not already saved, save it under the appropriate raw directory:
   - articles/web pages: `raw/articles/YYYYMMDD-title.md`
   - papers: `raw/papers/YYYYMMDD-title.md`
   - teacher output: `raw/teacher-sessions/YYYYMMDD-concept.md`
   - read-books output: `raw/read-books-sessions/<book-name>/`
   - roundtable output: `raw/roundtable-sessions/YYYYMMDDTHHMMSS--圆桌-topic__roundtable.md`
3. Identify existing related wiki pages and likely new pages.
4. Ask at most 3 focus questions before doing full integration when the focus is unclear. Do not try to process every possible idea at once.
5. Create or update wiki pages:
   - Concepts: `wiki/concepts/<concept>.md`
   - Topics: `wiki/topics/topic-<topic>.md`
   - Books: `wiki/books/book-<book>.md`
   - Roundtables usually update or create topic pages, with key positions, tensions, judgment frameworks, and open questions.
6. Add `[[bidirectional links]]` for all referenced concepts whose pages exist or are created.
7. Update `index.md` when new wiki pages or important entry points are added.
8. Append to `log.md` using the standard log format.
9. Reply with the created/updated pages and a short link count summary.

## Concept Page Template

Use this frontmatter for new concept pages:

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

Use this body structure:

```markdown
# 概念名
> 一句话定义

## 核心定义
## 所属层级与边界
## 解决的问题 / 适用场景
## 为什么不用另一个方案
## 原理摘要
## 易混淆点
## 判断框架
## 后续延伸

---
*来源：[[...]] | 掌握深度：基础/中级/高级*
```

For decision-oriented knowledge only, add optional sections such as `## 不适用场景`, `## 常见误用`, or `## 反例`. Do not add these mechanically to pure definition, person, book, or historical background pages.

## Confidence Fields

Use these fields to keep pages honest:

- `mastery`: user's learning depth, such as `basic`, `intermediate`, `advanced`
- `confidence`: page reliability, such as `low`, `medium`, `high`
- `evidence_level`: source base, such as `personal`, `secondary`, `primary`, `mixed`
- `last_reviewed`: last substantive review date, or blank if never reviewed

Default new pages to conservative values. Raise confidence only when multiple sources, primary sources, or real project experience support the page.

## Contradictions

Never silently overwrite contradictions. Preserve both claims and mark status:

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

## Log Format

Append only:

```markdown
## [YYYY-MM-DD] ingest | 标题
- 来源：...
- 新建：...
- 更新：...
- 新建链接：...
```

## Rules

- Keep raw source files as source records. Do not rewrite raw files unless the user is explicitly adding a new source or correcting the raw record.
- Wiki pages are maintained summaries and may be updated.
- A teacher knowledge card is not a wiki concept page. Store it in raw first, then ingest it into wiki pages.
- Teacher "判断框架" content is high-value and must be integrated into the concept page's `判断框架` section.
- Roundtable discussions are high-value for topic synthesis. Preserve core tensions and open questions; do not flatten multiple positions into a fake consensus.
- If the vault path is missing, stop and report that the local vault is not available at the configured path.
