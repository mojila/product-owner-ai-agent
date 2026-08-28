# Product Owner AI Agent

A knowledge-base portfolio — compiled by agents, for agents — that turns any product request into a crisp, actionable PRD. The repository combines two systems:

- **The hierarchy** — every app is a bounded product space with its own Domains, Knowledge, and `AGENTS.md`, so agents always work in the right scope.
- **The wiki protocol** — adapted from [llm-wiki](https://github.com/nvk/llm-wiki). Raw sources are ingested immutably, then compiled incrementally into interconnected markdown articles (PRDs, knowledge, references). Indexes navigate; the log records activity; inventory tracks durable things.

**The metaphor**: raw sources are source code, the agent is the compiler, the portfolio is the executable.

## Goals

1. **Turn any product request into a PRD.** Understand the ask, discover what the portfolio already knows, and synthesize a testable, traceable requirements document.
2. **Never fabricate.** Every requirement traces back to discovery. Unresolved items go to Open Questions, unverifiable claims are flagged, and every article carries a `confidence` score.
3. **Build an ever-improving knowledge base.** Interviews, research, and feedback are ingested once (immutably) and compiled into interconnected knowledge that compounds across PRDs.
4. **Scale across products.** The portfolio holds many apps, each an isolated topic wiki. Domains bound sub-problems; indexes navigate; the human stays in control of decisions.

## Core Principles

- **One app, one wiki.** Each app is a full, isolated topic wiki; `apps/` is only a registry.
- **Indexes are navigation.** Read `_index.md` files first — never scan blindly.
- **Raw is immutable.** Sources are never edited after ingest; synthesis happens in `knowledge/` and `prds/`.
- **Articles are synthesized, not copied.** Think PRD and glossary, not clipboard.
- **Dual-linking.** Every cross-reference uses both `[[slug|Name]]` (Obsidian) and `[Name](path)` (markdown) on the same line.
- **Honest gaps.** If the portfolio doesn't have the answer, say so and list the ingest suggestion.

## Repository Structure

```
Product Owner AI Agent/
├── README.md                  # This file
├── AGENTS.md                  # Root protocol — read this first
├── CLAUDE.md                  # Imports AGENTS.md + Claude Code-specific wiring
├── .kilo/                     # Kilo configuration
│   ├── agent/product-owner.md # Product Owner orchestrator agent
│   └── command/               # /new-app, /prd commands
├── .claude/                   # Claude Code equivalent config
│   ├── agents/product-owner.md # Product Owner subagent
│   ├── commands/              # /new-app, /prd (context: fork → product-owner)
│   └── skills/                # Skills (e.g. obsidian-cli)
└── apps/
    ├── _index.md              # Hub registry: app → one-liner → domains → status
    ├── _template/             # Scaffold template (never a real app)
    └── <app>/                 # Real apps — each a topic wiki
        ├── AGENTS.md          # App-level agent + wiki protocol
        ├── README.md          # One-paragraph overview
        ├── config.md          # Title, scope, conventions
        ├── log.md             # Activity log (append-only)
        ├── _index.md          # Master index
        ├── inbox/             # Drop zone — dump PRD requests here
        ├── raw/               # Immutable sources: interviews/, research/, feedback/
        ├── knowledge/         # Compiled articles: concepts/, topics/, references/
        ├── inventory/         # Durable tracking: ideas/, candidates/, entities/
        ├── domains/<domain>/  # Bounded sub-problems (own AGENTS.md + knowledge/)
        └── prds/              # Output: YYYY-MM-DD-<slug>.md
```

Apps are added under `apps/<slug>/` by scaffolding from `apps/_template/`. The template is a placeholder, never a real app. Archived apps live in `apps/.archive/` and are hidden from normal workflows.

## How to Use

### 1. Start with the Product Owner agent

Select the **Product Owner** agent (Kilo: `.kilo/agent/product-owner.md`; Claude Code: `.claude/agents/product-owner.md`). It orchestrates the whole workflow. On any start or "where we left off", it boots by reading `apps/_index.md` and the target app's index + log.

### 2. Commands

| Command | Purpose |
|---------|---------|
| `/new-app <Name>` | Scaffold a new app: copies `apps/_template/` → `apps/<slug>/`, fills in placeholders, creates domains, updates the registry |
| `/prd <app> <request>` | Author a PRD for an app using the Understand → Discover → Write → File workflow |

### 3. PRD workflow

A PRD is a compiled output artifact synthesized from raw sources + existing knowledge. The agent runs:

**Understand → Discover → Write → File → Log**

- **Understand** — clarify ambiguity, state assumptions explicitly rather than over-questioning.
- **Discover** — read the app's `_index.md`, `AGENTS.md`, `config.md`, domain knowledge, and follow See Also links. Multi-domain PRDs dispatch one subagent per domain.
- **Write** — from `apps/_template/prds/PRD-TEMPLATE.md`, with required frontmatter (`title`, `app`, `status`, `date`, `owner`, `related_domains`, `sources`, `confidence`, `tags`, `summary`) and sections: Problem Statement, Goals & Non-Goals, Personas, User Stories, FR-xx requirements (testable "SHALL" language), NFRs, Domain mapping, Acceptance Criteria, Assumptions, Open Questions, Success Metrics.
- **File** — PRD to `apps/<app>/prds/YYYY-MM-DD-<slug>.md`; index and log updates; new durable facts compiled into `knowledge/`.

### 4. Everyday operations

| Operation | What it does |
|-----------|--------------|
| **Boot** | Briefing: read hub + app indexes, summarize in-flight work |
| **Ingest** | Convert external material into immutable `raw/` sources (interviews, research, feedback, reference, notes) |
| **Compile** | Transform raw sources into knowledge articles and PRDs |
| **Query** | Answer questions from portfolio content only, with sources and confidence |
| **Refresh** | Human-gated freshness check of knowledge/PRDs — never auto-updates |
| **Inventory** | Track durable things: `inventory add <kind> "title"`, `inventory list`, `inventory update` |
| **Archive** | Quietly retire apps to `apps/.archive/` |
| **Lint** | Auto structural guardian: index freshness, orphans, registry sync |

### 5. Consumption in Obsidian

The repo is an Obsidian vault. Open it in Obsidian to navigate the graph, use wikilinks (`[[slug|Name]]`), aliases for alternate names, and tags — while agents read the same files as markdown.

## Rules to Respect

- PRDs are written to `apps/<app>/prds/YYYY-MM-DD-<slug>.md`, never at root.
- Raw sources are immutable; all synthesis happens in `knowledge/` and `prds/`.
- Every cross-reference uses dual-links; every article carries `confidence`.
- Append every operation to `log.md`; never edit past entries.
- App-specific rules live in app files — root `AGENTS.md` stays a protocol.

## Getting Started

1. Select the **Product Owner** agent.
2. Run `/new-app <Name>` to scaffold your first app.
3. Dump requests into the app's `inbox/` or run `/prd <app> <request>` to author a PRD.