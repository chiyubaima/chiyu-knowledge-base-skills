# Personal Knowledge Base Skills

Reusable AI-agent skills for learning, discussion, capture, and maintenance in an Obsidian-based personal knowledge base.

The system is built around one loop:

```text
learn / discuss / capture -> preserve raw records -> compile into wiki knowledge -> query and maintain
```

It is not a plain note-saving workflow. Raw material stays traceable, while maintained wiki pages become a living knowledge map with links, source types, contradictions, and judgment frameworks.

## How The System Works

The knowledge base has two layers.

### Raw Layer

The raw layer stores source records:

- articles and web pages
- papers and excerpts
- teacher-pro learning sessions
- roundtable discussion records
- sparks, ideas, and quotes
- assets and attachments

Raw files should preserve source context. They are the input layer, not the final knowledge layer.

### Wiki Layer

The wiki layer stores maintained knowledge:

- concept pages
- topic synthesis pages
- book pages based on original sources
- people pages
- meta and maintenance pages

Agents compile raw material into this layer by summarizing, linking, resolving boundaries, preserving contradictions, and making reusable judgment frameworks.

## Included Skills

### `teacher-pro`

Use this for learning a topic, book, paper, article, or URL.

It replaces the older separate `teacher` and `read-books` workflows.

What it does:

1. Detects whether the user provided original source material.
2. Marks the source basis as `original` or `general_knowledge`.
3. Plans a mastery-learning path.
4. Writes lesson and progress records into the raw layer.
5. Runs checkpoint questions before moving forward.
6. At module boundaries, hands the session output to `wiki-ingest`.

Key rule:

- `source_type: original` may support book pages, concept pages, or topic pages.
- `source_type: general_knowledge` must not be treated as a book-source conclusion and should be routed to topic or concept synthesis.

### `ljg-roundtable`

Use this for structured multi-perspective debate.

What it does:

1. Extracts the core issue.
2. Invites representative thinkers with distinct positions.
3. Runs a moderated dialectical discussion.
4. Generates ASCII thinking frameworks and a final knowledge network.
5. Saves the full discussion into the raw layer.
6. Hands the discussion record to `wiki-ingest` for topic synthesis.

### `wiki-ingest`

Use this when raw material should become maintained knowledge.

What it does:

1. Reads the existing knowledge map.
2. Identifies or saves the raw source.
3. Focuses on 1-3 core ideas.
4. Creates or updates concept, topic, or book pages.
5. Adds bidirectional links.
6. Writes required frontmatter, including `source_type`.
7. Updates the index and operation log.

### `wiki-query`

Use this when you want answers from the knowledge base.

What it does:

1. Reads the index.
2. Searches maintained wiki pages.
3. Answers only from available wiki material.
4. Cites relevant `[[wiki pages]]`.
5. Calls out source type, missing evidence, stale pages, or unresolved contradictions.

If the wiki does not contain the answer, the agent should say so instead of inventing one.

### `wiki-lint`

Use this for knowledge-base health checks.

It looks for:

- orphan pages
- broken links
- missing required frontmatter
- unresolved contradictions
- stale active pages
- stub pages
- source-type mistakes, such as book pages not based on original sources

It should produce a prioritized report before making repairs.

### `wiki-spark`

Use this for fast capture.

It saves small ideas, quotes, observations, or unfinished thoughts into the raw layer without forcing a full synthesis step.

Later, related sparks can be compiled with `wiki-ingest`.

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
  - "[[raw/...]]"
source_type: "original"
related: []
confused_with: []
---
```

Required fields:

```text
title, type, domain, status, created, updated, sources, source_type
```

`source_type` is the most important routing field:

- `original`: based on provided source material.
- `general_knowledge`: based on the agent's general knowledge and must be labeled clearly.

## Contradiction Handling

Contradictions should be preserved and tracked instead of silently overwritten.

```markdown
> [!contradiction] Pending verification
> Source A says: ...
> Source B says: ...
```

## Design Philosophy

This workflow is optimized for:

- durable knowledge over one-off summaries
- source-backed answers over unsupported confidence
- clear distinction between original sources and general knowledge
- gradual synthesis over information hoarding
- judgment frameworks over loose notes
- explicit uncertainty over silent overwrites

## Reuse

Install or copy the six skill folders into an agent environment that supports local skills:

```text
teacher-pro/
ljg-roundtable/
wiki-ingest/
wiki-query/
wiki-lint/
wiki-spark/
```

Then configure the target vault path inside the skill instructions for that environment.

For agent-facing operational details, see:

[AGENT_REUSE_GUIDE.md](./AGENT_REUSE_GUIDE.md)

## License

MIT
