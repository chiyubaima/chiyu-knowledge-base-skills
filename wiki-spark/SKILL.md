---
name: wiki-spark
description: Quickly capture small ideas, excerpts, quotes, observations, or unfinished thoughts into Chiyu's Obsidian knowledge base raw layer. Use when the user says wiki-spark, "记一个想法", "存一条摘录", "先记下来", "存个灵感", or wants fast capture without full wiki integration.
---

# Wiki Spark

Save small fragments safely without doing a full ingest. A spark is raw capture first; later, related sparks can be compiled through `wiki-ingest`.

## Vault

Use this vault path exactly:

`/Users/chiyu/Documents/chiyu-knowledge-base`

Save sparks under:

`/Users/chiyu/Documents/chiyu-knowledge-base/raw/sparks/`

## Workflow

1. Verify the vault exists.
2. Accept pasted text, a short user description, or an excerpt.
3. Extract 2-4 concise keywords for the filename.
4. Create `raw/sparks/YYYYMMDD-keywords.md`.
5. Use this format:

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

6. Quickly scan `wiki/` for related pages.
7. If clearly related, append a small reference to the relevant wiki page or report the suggested relation for user confirmation when uncertain.
8. If the spark connects to an active thread, update `wiki/hot.md` with a one-line note.
9. Reply with the saved raw path and related pages.

## Rules

- Keep spark capture lightweight. Do not turn it into a full concept page.
- Do not force categories or over-polish the user's wording.
- If 5 or more sparks cluster around the same topic, suggest a future `wiki-ingest`.
- Do not edit unrelated wiki pages unless the relationship is obvious.
- Do not create `wiki/sources/` pages for ordinary sparks; only promote clustered or high-value sparks through `wiki-ingest`.
- If the vault path is missing, stop and report that the local vault is not available at the configured path.
