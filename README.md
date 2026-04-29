# Personal Knowledge Base Skills

Reusable AI-agent skills for learning, discussion, capture, ingestion, query, save, and maintenance in an Obsidian-based personal knowledge base.

The system is built around one loop:

```text
learn / discuss / capture -> preserve raw records -> compile sources -> maintain wiki knowledge -> query, save, and lint
```

It is not a plain note-saving workflow. Raw material stays traceable, important sources get summary pages, maintained wiki pages become a living knowledge map, and recent context is cached so future sessions can continue without starting cold.

## How The System Works

The knowledge base has a raw layer, a maintained wiki layer, and a lightweight runtime layer.

### Raw Layer

The raw layer stores source records:

- articles and web pages
- papers and excerpts
- teacher-pro learning sessions
- roundtable discussion records
- sparks, ideas, and quotes
- assets and attachments

Raw files preserve source context. They are the input layer, not the final knowledge layer.

### Wiki Layer

The wiki layer stores maintained knowledge:

- source summary pages
- concept pages
- topic synthesis pages
- book pages based on original sources
- question and saved-answer pages
- people pages
- meta and maintenance pages

Agents compile raw material into this layer by summarizing, linking, resolving boundaries, preserving contradictions, and making reusable judgment frameworks.

### Runtime Layer

The runtime layer keeps the system usable over time:

- `wiki/hot.md`: recent context cache for fast follow-up questions
- `raw/.manifest.json`: ingest manifest to avoid duplicate processing
- `wiki/meta/lint-report-YYYY-MM-DD.md`: persistent health reports
- `wiki/overview.md`: human-readable map of the knowledge base

## Included Skills

### `teacher-pro`

Unified learning skill for topics, books, papers, articles, and URLs.

It detects whether the user provided original source material, marks the session as `original` or `general_knowledge`, writes lesson records into the raw layer, and hands module synthesis to `wiki-ingest`.

### `ljg-roundtable`

Structured multi-perspective debate skill.

It saves the full transcript into the raw layer, then hands the discussion to `wiki-ingest` so key positions, tensions, open questions, and frameworks can become topic or question pages.

### `wiki-ingest`

Turns raw material into maintained wiki knowledge.

It checks the manifest, creates source summaries, updates concept/topic/book/question pages, adds wikilinks, updates `index.md`, `overview.md`, `hot.md`, `log.md`, and records what happened in `raw/.manifest.json`.

### `wiki-query`

Answers from the maintained wiki.

It supports quick, standard, and deep query modes. It starts with `wiki/hot.md`, reads the index, then opens only the pages needed for the chosen depth. Good answers can be saved through `wiki-save`.

### `wiki-save`

Saves valuable conversation output into the wiki.

Use it for reusable answers, decisions, comparisons, frameworks, or session-level synthesis that emerged in chat rather than from a new external source.

### `wiki-lint`

Runs health checks.

It writes persistent lint reports, checks links/frontmatter/source types/manifest references/stale pages, and asks before repairing.

### `wiki-spark`

Fast capture for small ideas, quotes, and unfinished thoughts.

Sparks stay lightweight unless a cluster deserves `wiki-ingest`.

## Key Conventions

Concept pages use frontmatter to track structure and source basis:

```yaml
---
title: "Concept Name"
aliases: []
type: concept
domain: []
layer: ""
status: active
mastery: basic
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - "[[wiki/sources/Source Title]]"
source_type: "original"
related: []
confused_with: []
---
```

Required fields:

```text
title, type, domain, status, created, updated, sources, source_type
```

`source_type` is the key routing field:

- `original`: based on provided source material.
- `general_knowledge`: based on the agent's general knowledge and must be labeled clearly.

## Contradiction Handling

Contradictions should be preserved and tracked instead of silently overwritten.

```markdown
> [!contradiction] Pending verification
> Source A says: ...
> Source B says: ...
```

## Reuse

Install or copy the seven skill folders into an agent environment that supports local skills:

```text
teacher-pro/
ljg-roundtable/
wiki-ingest/
wiki-query/
wiki-save/
wiki-lint/
wiki-spark/
```

Then configure the target vault path inside the skill instructions for that environment.

For agent-facing operational details, see:

[AGENT_REUSE_GUIDE.md](./AGENT_REUSE_GUIDE.md)

## License

MIT
