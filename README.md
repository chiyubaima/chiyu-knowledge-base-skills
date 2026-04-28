# Personal Knowledge Base Skills

This repository contains a reusable skill set for learning, reading, and maintaining an Obsidian-based personal knowledge base with AI agents.

The system is built around a simple idea:

```text
learn/read/capture -> save raw material -> compile into wiki knowledge -> query and maintain over time
```

It is not just a note-saving workflow. The goal is to help an agent turn articles, excerpts, learning sessions, book notes, and scattered ideas into a living knowledge base that can be searched, updated, linked, and reviewed.

## How The System Works

The knowledge base has two main layers, plus learning workflows that feed those layers.

### Raw Layer

The raw layer stores source material as close to the original as possible:

- articles and web pages
- papers
- book notes
- learning-session outputs
- quick ideas and excerpts
- assets and attachments

Raw files are source records. They should usually be appended or newly created, not rewritten during normal maintenance.

### Wiki Layer

The wiki layer stores maintained knowledge:

- concept pages
- topic synthesis pages
- book pages
- people pages
- meta and maintenance pages

This is where the agent summarizes, links, updates, compares, tracks contradictions, and builds reusable judgment frameworks.

## Included Skills

### `teacher`

Use this when you want to understand a concept, clarify boundaries, or build a transferable mental model.

What it does:

1. Diagnoses the user's current understanding.
2. Explains the concept, boundaries, tradeoffs, and common confusions.
3. Checks understanding lightly.
4. Produces a confirmed knowledge card and conversation highlights.
5. Saves the confirmed result into the raw layer.
6. Hands the raw file to `wiki-ingest` for durable synthesis.

### `read-books`

Use this for guided book study or systematic topic learning.

What it does:

1. Clarifies the learning goal.
2. Plans a multi-module reading path.
3. Creates lesson and progress records in the raw layer.
4. Uses checkpoint questions to confirm understanding before moving on.
5. Runs module-level synthesis at the end of each module.
6. Hands module outputs to `wiki-ingest` to update book and concept pages.

### `wiki-ingest`

Use this when new material should become durable knowledge.

Typical inputs:

- a saved raw note
- a pasted article or excerpt
- a paper
- a learning-session summary
- book-session notes

What it does:

1. Reads the existing knowledge map.
2. Saves or identifies the raw source.
3. Focuses on 1-3 core ideas.
4. Creates or updates concept, topic, or book pages.
5. Adds bidirectional links.
6. Updates the index and operation log.
7. Reports what changed.

### `wiki-query`

Use this when you want answers from the knowledge base.

What it does:

1. Reads the index.
2. Searches maintained wiki pages.
3. Answers only from available wiki material.
4. Cites relevant `[[wiki pages]]`.
5. Calls out missing evidence, low confidence, or unresolved contradictions.

This skill is intentionally conservative: if the wiki does not contain the answer, the agent should say so instead of inventing one.

### `wiki-lint`

Use this for knowledge-base health checks.

It looks for:

- orphan pages
- broken links
- missing frontmatter
- unresolved contradictions
- stale pages
- stub pages
- missing obvious links
- repeated concepts that may deserve their own page

It should produce a prioritized report before making repairs.

### `wiki-spark`

Use this for fast capture.

It saves small ideas, quotes, observations, or unfinished thoughts into the raw layer without forcing a full synthesis step.

Later, related sparks can be compiled with `wiki-ingest`.

## Key Conventions

Concept pages use frontmatter to track both structure and knowledge quality:

```yaml
---
title: "Concept Name"
aliases: []
type: concept
domain: []
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

The system distinguishes:

- `mastery`: how well the user understands the idea
- `confidence`: how reliable the page is
- `evidence_level`: what kind of sources support it
- `last_reviewed`: when it was last checked

## Contradiction Handling

Contradictions should be preserved and tracked instead of silently overwritten.

Example:

```markdown
> [!contradiction] Pending verification
> status: open
> Source A says: ...
> Source B says: ...
```

When clarified:

```markdown
> [!contradiction] Clarified
> status: clarified
> resolution: ...
> resolved_at: YYYY-MM-DD
```

## Design Philosophy

This workflow is optimized for:

- durable knowledge over one-off summaries
- source-backed answers over hallucinated confidence
- gradual synthesis over information hoarding
- judgment frameworks over loose notes
- explicit uncertainty over silent overwrites

The system works best when agents keep each ingest focused, maintain links carefully, and treat the wiki as a living layer that improves over time.

## Reuse

To reuse this system, install or copy the six skill folders into an agent environment that supports local skills:

```text
teacher/
read-books/
wiki-ingest/
wiki-query/
wiki-lint/
wiki-spark/
```

Then configure the target vault path inside the skill instructions for that environment.

For agent-facing operational details, see:

[AGENT_REUSE_GUIDE.md](./AGENT_REUSE_GUIDE.md)
