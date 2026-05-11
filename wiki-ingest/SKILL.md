---
name: wiki-ingest
description: Compile new material into Chiyu's Obsidian knowledge base. Use when the user says wiki-ingest, asks to store an article/paper/note/session in the knowledge base, says "帮我把这个存进知识库", "ingest 这篇文章/论文/笔记", or provides a source that should become maintained wiki pages rather than a raw archive.
---

# Wiki Ingest

Turn new material into maintained wiki knowledge. Do not merely archive content: save or identify the source in the raw layer, avoid duplicate ingest with the manifest, create a source summary when useful, extract 1-3 focused points, update concept/topic/book pages, link related pages, update the hot cache, and append the operation log.

## Vault

Use this vault path exactly:

`/Users/chiyu/Documents/chiyu-knowledge-base`

Important files:

- Schema: `/Users/chiyu/Documents/chiyu-knowledge-base/WIKI.md`
- Index: `/Users/chiyu/Documents/chiyu-knowledge-base/index.md`
- Log: `/Users/chiyu/Documents/chiyu-knowledge-base/log.md`
- Hot cache: `/Users/chiyu/Documents/chiyu-knowledge-base/wiki/hot.md`
- Overview: `/Users/chiyu/Documents/chiyu-knowledge-base/wiki/overview.md`
- Manifest: `/Users/chiyu/Documents/chiyu-knowledge-base/raw/.manifest.json`
- Raw layer: `/Users/chiyu/Documents/chiyu-knowledge-base/raw/`
- Wiki layer: `/Users/chiyu/Documents/chiyu-knowledge-base/wiki/`

## Workflow

1. Verify the vault exists and read `WIKI.md`, `wiki/hot.md` if present, and `index.md`.
2. Read the source from a file path, URL, or pasted text. If the source URL is from `mp.weixin.qq.com`, do not use WebFetch because it is commonly blocked by verification; use gstack browse instead (`$B goto <url>` followed by `$B text`) to capture the article body. For other sources, keep the normal source-reading flow. If the source is not already saved, save it under the appropriate raw directory:
   - articles/web pages: `raw/articles/YYYYMMDD-title.md`
   - papers: `raw/papers/YYYYMMDD-title.md`
   - teacher-pro output: `raw/teacher-pro-sessions/<topic-or-book>/`
   - roundtable output: `raw/roundtable-sessions/YYYYMMDDTHHMMSS--圆桌-topic__roundtable.md`
   - sparks: `raw/sparks/YYYYMMDD-keywords.md`
3. Check `raw/.manifest.json` before ingesting a raw file. If the same file path and content hash are already recorded, report that it was already ingested and stop unless the user asks for `force ingest`.
4. Determine `source_type`.
5. Create or update a `wiki/sources/` summary page for significant raw sources. Skip source summaries for tiny sparks unless they are being promoted.
6. Identify existing related wiki pages and likely new pages.
7. Ask at most 3 focus questions before doing full integration when the focus is unclear. Do not try to process every possible idea at once.
8. Before creating concept pages, scan the source for all named concepts. For each candidate, ask: "Does this concept have independent meaning and can it be reused in other contexts?" Only create pages that pass this test. See **Concept Page Granularity** section below for rules and examples.
9. Create or update wiki pages:
   - Sources: `wiki/sources/<source-title>.md`
   - Concepts: `wiki/concepts/<concept>.md`
   - Topics: `wiki/topics/topic-<topic>.md`
   - Books: `wiki/books/book-<book>.md`
   - Questions: `wiki/questions/<question-title>.md` when the source is primarily an answer or framed inquiry.
   - Roundtables usually update or create topic pages, with key positions, tensions, judgment frameworks, and open questions.
10. Add `[[bidirectional links]]` for all referenced concepts whose pages exist or are created.
11. Update `index.md` and relevant sub-indexes when new wiki pages or important entry points are added.
12. Update `wiki/overview.md` if the big picture changed.
13. Update `wiki/hot.md` with a short recent-context summary.
14. Update `raw/.manifest.json` with the source hash and pages created/updated.
15. Append to `log.md` using the standard log format.
16. Reply with created/updated pages, source summary, link count, and whether the manifest was updated.

## Concept Page Granularity

Each concept page must represent one independent, reusable unit of knowledge.

Before creating a new concept page, the candidate must pass all three tests:

- Independent meaning: Does this concept have a clear definition outside the source material?
- Reusability: Could pages about completely different topics link to this page?
- Non-container: Is this itself a concept, rather than a collection of multiple concepts?

Create independent concept pages for the following kinds of correctly scoped concepts:

- Named technologies, models, or algorithms: `AUC.md`, `GBDT.md`, `特征泄露.md`
- Named phenomena or failure modes: `模型退化.md`, `过拟合与欠拟合.md`
- Named design principles or patterns: `基线模型.md`, `向量检索.md`
- Named metrics or evaluation methods: `NDCG.md`, `离线在线一致性.md`

Do not create concept pages for poorly scoped containers:

- Course names or lesson titles: ~~`业务问题到模型选择.md`~~, ~~`模型问题诊断.md`~~
- Broad frameworks that wrap multiple concepts: ~~`函数方法论.md`~~; split into smaller concepts instead.
- Category labels: ~~`模型特征.md`~~; split into smaller concepts instead.
- Workflow descriptions: ~~`训练样本.md`~~; split into smaller concepts instead.

When the source has no clear concept boundary, which is common in teacher-pro courses:

- Scan the source and list all named concepts.
- Apply the three-test check to each name.
- Create only the pages that pass, even if one lesson yields 8 separate pages.
- Fold high-level summaries into `wiki/topics/` instead of creating broad concept pages.

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
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - "[[raw/...]]"
source_type: "original"
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

## Source Type

Every wiki page must identify its source basis:

- `source_type: original`: based on provided source material such as a book, paper, article, URL, or raw source file.
- `source_type: general_knowledge`: based on the agent's general knowledge rather than a provided source.

Routing rule:

- `original` material may create or update `wiki/books/`, `wiki/concepts/`, or `wiki/topics/` depending on content.
- `general_knowledge` material must not create `wiki/books/`; route it to `wiki/topics/` or `wiki/concepts/` and clearly mark `source_type: general_knowledge`.
- If `source_type` is unclear, ask before writing.

## Source Summary Pages

Create a source summary in `wiki/sources/` for substantial raw inputs such as articles, papers, teacher-pro module outputs, and roundtable transcripts.

```yaml
---
title: "来源标题"
type: source
domain: []
status: active
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - "[[raw/...]]"
source_type: "original"
related: []
---
```

Suggested body:

```markdown
# 来源标题

## 来源摘要
## 关键主张
## 可提炼概念
## 与已有知识的关系
## 矛盾 / 待核实
```

Concept/topic/book pages should prefer citing `wiki/sources/` pages when a source summary exists, while still preserving raw links in the source page.

## Manifest / Delta Tracking

Use `raw/.manifest.json` to avoid re-processing unchanged raw files.

Suggested schema:

```json
{
  "sources": {
    "raw/articles/example.md": {
      "hash": "sha256...",
      "ingested_at": "YYYY-MM-DD",
      "source_type": "original",
      "source_page": "wiki/sources/example.md",
      "pages_created": [],
      "pages_updated": []
    }
  }
}
```

Before ingesting:

1. Compute a stable hash of the raw file.
2. If the same path and hash exist in the manifest, stop and report "already ingested" unless the user asks for `force ingest`.
3. If content changed or force is requested, proceed and update the manifest.

## Hot Cache

Update `wiki/hot.md` after every successful ingest.

Keep it under about 500 words:

```markdown
---
title: "Hot Cache"
type: meta
status: active
updated: YYYY-MM-DD
---

# Recent Context

## Last Updated
YYYY-MM-DD — ...

## Key Recent Facts
- ...

## Recent Changes
- Created: ...
- Updated: ...

## Active Threads
- ...
```

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
- `raw/.manifest.json` is the only raw file this skill may maintain.
- Wiki pages are maintained summaries and may be updated.
- `teacher-pro` replaces the old `teacher` and `read-books` workflows. Its session files live under `raw/teacher-pro-sessions/`.
- A teacher-pro lesson or progress file is not a wiki page. Store it in raw first, then ingest it into wiki pages.
- Teacher-pro "判断框架" and module synthesis content is high-value and must be integrated into the relevant concept/topic/book page.
- Roundtable discussions are high-value for topic synthesis. Preserve core tensions and open questions; do not flatten multiple positions into a fake consensus.
- If the vault path is missing, stop and report that the local vault is not available at the configured path.
