---
name: wiki-save
description: Save valuable conversation output, analysis, decisions, or reusable answers into the Obsidian knowledge base. Use when the user says wiki-save, "保存这个回答", "把这段讨论存进知识库", "save this", "file this", or when a query answer or discussion result is worth keeping as a durable wiki page.
---

# Wiki Save

Save high-value conversation output into the maintained wiki layer. Use this for durable answers, decisions, frameworks, and synthesis that emerge from chat rather than from a new external source.

## Vault

Use this vault path exactly:

`/Users/chiyu/Documents/chiyu-knowledge-base`

Write durable saved pages under:

- `wiki/questions/` for valuable answers to user questions
- `wiki/topics/` for reusable synthesis or frameworks
- `wiki/meta/` for decisions, maintenance notes, or session summaries

## Workflow

1. Verify the vault exists and read `WIKI.md`, `index.md`, and `wiki/hot.md` if present.
2. Identify the content worth saving:
   - a reusable answer
   - a decision and its rationale
   - a judgment framework
   - a comparison or synthesis
   - a session summary with future value
3. Ask for a short title if the user did not provide one.
4. Choose the destination:
   - `wiki/questions/` when saving a specific Q&A or answer.
   - `wiki/topics/` when saving a reusable framework or cross-concept synthesis.
   - `wiki/meta/` when saving a decision, session note, or maintenance artifact.
5. Write a page with required frontmatter. Use `source_type: general_knowledge` unless the saved answer explicitly cites existing `original` wiki sources.
6. Link related wiki pages with `[[wikilinks]]`.
7. Update `index.md`.
8. Append to `log.md`.
9. Update `wiki/hot.md`.
10. Reply with the saved page path and important links.

## Page Templates

Question page:

```yaml
---
title: "问题标题"
type: question
domain: []
status: active
created: YYYY-MM-DD
updated: YYYY-MM-DD
question: "用户原始问题"
answer_quality: solid
sources: []
source_type: "general_knowledge"
related: []
---
```

Topic or framework page:

```yaml
---
title: "topic-主题名"
type: topic
domain: []
status: active
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
source_type: "general_knowledge"
related: []
---
```

Decision page:

```yaml
---
title: "决策标题"
type: meta
domain: []
status: active
created: YYYY-MM-DD
updated: YYYY-MM-DD
decision_date: YYYY-MM-DD
sources: []
source_type: "general_knowledge"
related: []
---
```

## Log Format

Append only:

```markdown
## [YYYY-MM-DD] save | 标题
- 来源：conversation
- 新建：wiki/questions/...
- 更新：index.md, wiki/hot.md
- 新建链接：...
```

## Rules

- Do not save routine lookup answers.
- If an answer is already represented in an existing wiki page, update that page instead of creating a duplicate.
- Save knowledge in declarative present tense. Do not write "the user asked" unless saving a session summary.
- If the saved answer depends on existing wiki pages, cite them in `sources` or `related`.
- Use `wiki-ingest` instead when the primary input is a new external source or raw file.
