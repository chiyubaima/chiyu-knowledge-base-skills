---
name: wiki-lint
description: Run health checks and maintenance on Chiyu's Obsidian knowledge base. Use when the user says wiki-lint, "检查一下知识库", "知识库健康检查", asks for orphan pages, missing frontmatter, stale pages, unresolved contradictions, broken links, or wants a priority report before repairs.
---

# Wiki Lint

Inspect the maintained wiki for structural problems and knowledge decay. Produce a prioritized report first; only repair after the user confirms the scope.

## Vault

Use this vault path exactly:

`/Users/chiyu/Documents/chiyu-knowledge-base`

Inspect:

- `WIKI.md`
- `index.md`
- `log.md`
- `wiki/**/*.md`

Write reports to:

- `wiki/meta/lint-history.md`

## Workflow

1. Verify the vault exists and read `WIKI.md`.
2. Scan all Markdown files under `wiki/`.
3. Produce a health report grouped by priority.
4. Ask which issues to repair.
5. Apply confirmed repairs only.
6. Append the lint run to `log.md`.
7. Append detailed results to `wiki/meta/lint-history.md`.

## Checks

High priority:

- Orphan wiki pages with no inbound links.
- Open contradictions: `[!contradiction]` blocks with `status: open` or no status.
- Missing required frontmatter: `title`, `type`, `domain`, `status`, `sources`.
- Broken wiki links whose target pages do not exist.

Medium priority:

- Stub pages: `status: stub` or body shorter than 10 meaningful lines.
- Missing obvious links where a known concept title is mentioned without `[[...]]`.
- Stale active pages: `updated` older than 6 months or missing `last_reviewed`.
- Pages without `confidence` or `evidence_level`.

Exploratory:

- Concepts mentioned by multiple pages but missing their own concept page.
- `后续延伸` items without pages.
- Clusters of 5 or more related sparks/raw notes that may deserve `wiki-ingest`.

## Repair Rules

- Ask before editing.
- Do not edit raw source files.
- Prefer minimal structural repairs over rewriting content.
- For contradictions, do not resolve content unless there is enough evidence in the wiki or user confirms the resolution.
- Add optional `不适用场景`, `常见误用`, or `反例` sections only for decision-oriented pages where the content supports them.

## Report Format

Use this shape:

```markdown
# Wiki Lint Report - YYYY-MM-DD

## Red
- ...

## Yellow
- ...

## Green / Explore
- ...

## Suggested Repairs
- ...
```

## Log Format

Append only:

```markdown
## [YYYY-MM-DD] lint | 健康检查
- 来源：wiki/
- 新建：...
- 更新：...
- 新建链接：...
```
