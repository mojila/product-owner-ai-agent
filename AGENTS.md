# AGENTS.md — Product Owner Orchestration Protocol

You are operating the **Product Owner AI Agent** product portfolio: a knowledge base compiled by agents for agents. The portfolio combines two systems:

- **The hierarchy** — every app is a bounded product space with its own Domains, Knowledge, and `AGENTS.md`, so agents always work in the right scope.
- **The wiki protocol** — adapted from [llm-wiki](https://github.com/nvk/llm-wiki). Raw sources are ingested immutably, then compiled incrementally into interconnected markdown articles (PRDs, knowledge, references). Indexes navigate; the log records activity; inventory tracks durable things. The human rarely edits compiled output directly — that is the agent's domain.

**The metaphor**: raw sources are source code, you are the compiler, the portfolio is the executable.

## Architecture

```
Product Owner AI Agent/
├── AGENTS.md                      # This file — root protocol
├── .kilo/
│   ├── agent/product-owner.md     # Product Owner orchestrator agent
│   ├── command/new-app.md         # /new-app  — scaffold a new app
│   └── command/prd.md             # /prd      — author a PRD for an app
└── apps/                          # THE HUB — portfolio registry, no content
    ├── _index.md                  # Registry: app → one-liner → domains → status
    ├── _template/                 # Scaffold template (never a real app)
    │   ├── AGENTS.md              # App-level agent + wiki protocol (template)
    │   ├── README.md
    │   ├── config.md              # Title, scope, conventions
    │   ├── log.md                 # Activity log
    │   ├── _index.md              # Master index: stats, navigation, recent changes
    │   ├── inbox/                 # Drop zone — user dumps PRD requests here
    │   ├── raw/                   # Immutable source material (interviews, research)
    │   ├── knowledge/             # Compiled wiki articles (concepts, topics, refs)
    │   ├── inventory/             # Durable tracking (ideas, candidates, entities)
    │   ├── domains/
    │   │   └── _template/
    │   │       ├── AGENTS.md      # Domain-level agent (template)
    │   │       ├── log.md
    │   │       └── knowledge/     # Domain-level compiled knowledge
    │   └── prds/
    │       ├── PRD-TEMPLATE.md    # Canonical PRD template
    │       └── _index.md
    └── <app>/                     # Real apps appear here — each is a topic wiki
        ├── AGENTS.md              # App-level agent + wiki protocol
        ├── README.md              # One-paragraph overview
        ├── config.md              # Title, scope, conventions
        ├── log.md                 # Activity log for this app
        ├── _index.md              # Master index
        ├── inbox/                 # Drop zone
        ├── raw/                   # Immutable sources: interviews/, research/, feedback/
        ├── knowledge/             # Compiled articles: concepts/, topics/, references/
        ├── inventory/             # Durable tracking: ideas/, candidates/, entities/
        ├── domains/<domain>/      # Bounded sub-problems
        │   ├── AGENTS.md
        │   ├── log.md
        │   └── knowledge/
        └── prds/                  # Output artifacts: YYYY-MM-DD-<slug>.md
```

## Level Responsibilities

| Level | Owns | AGENTS.md defines |
|-------|------|-------------------|
| Root (`AGENTS.md`) | Portfolio protocol, hub rules | How levels relate, wiki protocol, PRD workflow, scaffold rules |
| App (`apps/<app>/AGENTS.md`) | A whole product (a topic wiki) | App purpose, wiki layers, architecture conventions, tech stack, glossary, interaction rules with its domains |
| Domain (`apps/<app>/domains/<domain>/AGENTS.md`) | A bounded sub-problem | Domain vocabulary, business rules, data ownership, rules other domains must respect |

Knowledge (`knowledge/`) holds **compiled** durable facts — synthesized from raw sources, not copied. Raw sources (`raw/`) are immutable receipts of what was learned. Never duplicate a fact at two levels without cross-linking.

## Core Principles

1. **One app, one wiki.** Each app is a full, isolated topic wiki. The `apps/` hub is just a registry.
2. **Indexes are navigation.** Every existing wiki-managed directory has `_index.md` with a contents table. Read indexes first, never scan blindly. Keep them current.
3. **Raw is immutable.** Once ingested, sources are never modified. All synthesis happens in `knowledge/`. Explicit retraction is the exception.
4. **Articles are synthesized, not copied.** Draw from multiple sources, contextualize, connect. Think PRD and glossary, not clipboard.
5. **Dual-linking.** Every cross-reference uses both formats on the same line: `[[slug|Name]]` ([Name](../knowledge/topics/slug.md)). Obsidian reads the wikilink, the agent reads the markdown link, GitHub renders it.
6. **Incremental by default.** Only compile new sources unless explicitly asked for a full recompile.
7. **Honest gaps.** If the portfolio doesn't have the answer, say so and list the ingest suggestion.
8. **Confidence scoring.** PRDs and knowledge articles carry `confidence: high|medium|low` in frontmatter based on source quality.
9. **Activity log.** Append every operation to each app's `log.md` and the app's index. Format: `## [YYYY-MM-DD] operation | Description`. Never edit existing entries.
10. **Structural guardian.** After writes, run lightweight checks: index freshness, orphan files, missing `_index.md`. Fix trivia silently, warn on structure, never block the user.
11. **Archive is quiet preservation.** Retired apps move to `apps/.archive/<slug>/`. Normal query/compile/PRD context excludes them unless explicitly included.
12. **Rules are layered.** Root stays protocol; app-specific details live in app files; domain rules never leak up.

## File Formats

### _index.md (every existing wiki-managed directory)

```markdown
# [Directory Name] Index

> [One-line description]

Last updated: YYYY-MM-DD

## Contents

| File | Summary | Tags | Updated |
|------|---------|------|---------|
| [file.md](file.md) | One-sentence summary | tag1, tag2 | YYYY-MM-DD |

## Categories

- **category**: file1.md, file2.md

## Recent Changes

- YYYY-MM-DD: Description
```

App `_index.md` additionally has Statistics and Quick Navigation sections.

### Raw Source (raw/)

```yaml
---
title: "Title"
kind: interview|research|feedback|reference|notes
source: "URL or filepath or MANUAL"
ingested: YYYY-MM-DD
tags: [tag1, tag2]
summary: "2-3 sentence summary"
---
```

Immutable. Written by ingest, never edited after.

### Knowledge Article (knowledge/)

```yaml
---
title: "Article Title"
category: concept|topic|reference
sources: [raw/interviews/file1.md, raw/research/file2.md]
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2]
aliases: [alternate names]
confidence: high|medium|low
summary: "2-3 sentence summary"
---
```

Body includes abstract, sections, `## See Also` (dual-links, bidirectional), `## Sources` (links to `raw/`).

### Inventory Record (inventory/)

```yaml
---
title: "Thing To Track"
kind: idea|candidate|entity|task|question|artifact
status: proposed|active|blocked|ingested|superseded|archived
priority: p0|p1|p2|p3
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2]
summary: "Why this record exists"
sources: [raw/...]
---
```

Inventory tracks durable things the portfolio should remember but that are not raw sources or compiled articles: feature ideas, ingest candidates, people/orgs, open questions, tasks. Never cite inventory as factual evidence — facts need raw/knowledge sources.

### log.md

```
## [YYYY-MM-DD] operation | Description
```

Operations: `init`, `ingest`, `compile`, `query`, `prd`, `author`, `refresh`, `archive`, `inventory`, `boot`.

## Operations

### Boot / Briefing

On any start, resume, or "where we left off" briefing, begin with: `<portfolio> booted from <repo-root> (apps: N)`. Read `apps/_index.md` and the target app's `_index.md` + `log.md`, then summarize in-flight work, open PRDs, and inventory next actions.

### Init (new-app)

Create an app (topic wiki). Always require a name → slug (kebab-case). If the hub doesn't exist, create it first (just `_index.md` + `log.md`-style registry). Refuse to create over an existing `apps/<slug>` or `apps/.archive/<slug>`. Copy `apps/_template/` → `apps/<slug>/`, fill `AGENTS.md`, `README.md`, `config.md`, create domains, then update `apps/_index.md` and write the app's first `log.md` entry.

### Ingest

Convert external material into an immutable raw source file.

- **Interviews**: user/product conversations → `raw/interviews/`
- **Research**: web articles, docs, competitor analysis → `raw/research/`
- **Feedback**: bug reports, support calls, NPS comments → `raw/feedback/`
- **Reference**: specs, contracts, standards → `raw/reference/`
- **Notes**: user-provided quoted text → `raw/notes/`
- **Inbox**: scan `inbox/` for dumped files, process each by type, move to `inbox/.processed/`

Slug: `YYYY-MM-DD-descriptive-slug.md`. Auto-detect kind where possible. Update all indexes after each ingestion. If 5+ uncompiled sources accumulate, suggest compilation. If the user wants to track or decide later, use an inventory record instead of ingesting.

### Compile

Transform raw sources into knowledge articles and PRDs. Incremental by default.

1. Survey: read indexes, identify uncompiled sources.
2. Extract: key concepts, facts, relationships from each source.
3. Map: which concepts need new articles vs updates to existing.
4. Classify: concept (bounded idea), topic (broad theme), reference (curated list).
5. Write: synthesized articles with dual-links, frontmatter, confidence scores, aliases.
6. Bidirectional links: if A links to B, ensure B links to A.
7. Update all indexes.

Do not compile inventory records as articles. Archived apps are skipped by default.

### Query

Answer questions from portfolio content only. Three depths:

- **Quick**: Read indexes only. Fastest.
- **Standard** (default): Read relevant articles + full-text search. Follow See Also links.
- **Deep**: Read everything active, search raw sources, peek sibling apps.

Always cite sources. Note confidence levels. Identify gaps. Never use training data — only portfolio content. For meta-questions about inventory/backlogs, read inventory indexes and answer as an operational listing; for factual questions, inventory is not evidence.

### PRD Authoring (prd)

The core delivery workflow — see `.kilo/agent/product-owner.md` and `apps/_template/prds/PRD-TEMPLATE.md`. A PRD is a compiled output artifact: it synthesizes raw sources + existing knowledge into a requirements document with frontmatter, confidence, dual-links to knowledge, and an Open Questions section for unresolved items.

- **Understand** → **Discover** (read hub, app, domains, knowledge) → **Write** (from template) → **File** (update indexes + log + knowledge/CHANGELOG).

### Refresh

Freshness check for knowledge articles and PRDs. Re-visit sources, detect changed/contradicted facts, present a human-gated assessment: skip / update / flag / retract. Never auto-updates — the human confirms every change.

### Inventory

Track durable things: ideas, candidates, entities, tasks, questions, artifacts. `inventory add <kind> "title"`, `inventory list`, `inventory update <path>`. Present compact tables in chat; open full records only when asked. Route `kind: idea` through the Ideas workflow (Concept → Idea → Project: evidence-backed knowledge → inventory idea → PRD/delivery).

### Archive

Quietly preserve retired apps. `apps/<slug>` → `apps/.archive/<slug>`, update `apps/_index.md`, write log entries. Everything hides archive by default.

### Lint (Structural Guardian)

Auto-run after write operations:

1. Hub (`apps/`) should only contain `_index.md`, `_template/`, app wikis, and `.archive/`. Warn on anything else; never delete.
2. Index freshness: file counts match index rows. Auto-fix by regenerating the affected index.
3. Orphan detection: files not in any index → add them.
4. Missing core directories → create with empty `_index.md`.
5. Registry sync: every app registered in `apps/_index.md`; no ghost entries.

Silent when clean. Auto-fix trivia. Warn on structural problems. Never block.

## Key Commands / Agents

- **Product Owner agent** (`.kilo/agent/product-owner.md`) — the orchestrator. Select it to author PRDs and maintain the portfolio.
- **`/new-app <Name>`** — scaffolds `apps/<slug>/` from `_template` (wiki-enabled, with inbox/raw/knowledge/inventory/log).
- **`/prd <app> <request>`** — runs the PRD authoring workflow inside an app.

## File Naming

- Raw sources: `YYYY-MM-DD-descriptive-slug.md`
- Knowledge articles: `descriptive-slug.md` (no date — living documents)
- PRDs: `apps/<app>/prds/YYYY-MM-DD-<slug>.md`
- Inventory records: `descriptive-slug.md`
- All lowercase, hyphens, no special chars, max 60 chars

## Tag Convention

Lowercase, hyphenated. Specific over general (`auth-security` not `security`). Normalize — no near-duplicates.

## Obsidian Compatibility

The root repo is an Obsidian vault (`apps/` included). Every app can also be opened as a vault. `.obsidian/` config exists at root only — no nested vaults. Dual-links power the graph view. Aliases enable search by alternate names. Tags are natively read.

## Rules

1. Root `AGENTS.md` stays a protocol — app-specific details live in app files.
2. Every app and domain must have an `AGENTS.md` and maintain its indexes.
3. PRDs are written to `apps/<app>/prds/YYYY-MM-DD-<slug>.md`, never at root.
4. `apps/_index.md` is the registry — keep it current when adding, archiving, or changing apps or domains.
5. Do not fabricate requirements; unresolved items go to Open Questions.
6. Raw sources are immutable; all synthesis happens in `knowledge/` and `prds/`.
7. Append every operation to the relevant `log.md`; never edit past entries.