# AGENTS.md — Product Owner Orchestration Protocol

You are operating the **Product Owner AI Agent** product portfolio. This is the root protocol for a Product Owner Agent that creates and maintains PRDs for any app, organized as a strict hierarchy where every app has its own Domains and Knowledge, and every app — and every domain inside an app — carries its own `AGENTS.md`.

## Hierarchy

```
Product Owner AI Agent/
├── AGENTS.md                      # This file — root protocol
├── .kilo/
│   ├── agent/product-owner.md     # Product Owner orchestrator agent
│   ├── command/new-app.md         # /new-app  — scaffold a new app
│   └── command/prd.md             # /prd      — author a PRD for an app
└── apps/
    ├── _index.md                  # Portfolio registry: app → one-liner → domains
    ├── _template/                 # Placeholder + scaffold template (no real apps yet)
    │   ├── AGENTS.md              # App-level agent (template)
    │   ├── README.md
    │   ├── knowledge/             # App-level knowledge base
    │   ├── domains/
    │   │   └── _template/
    │   │       ├── AGENTS.md      # Domain-level agent (template)
    │   │       └── knowledge/     # Domain-level knowledge base
    │   └── prds/
    │       ├── PRD-TEMPLATE.md    # Canonical PRD template
    │       └── _index.md
    └── <app>/                     # Real apps appear here
        ├── AGENTS.md
        ├── README.md
        ├── knowledge/
        ├── domains/<domain>/AGENTS.md + knowledge/
        └── prds/
```

## Level Responsibilities

| Level | Owns | AGENTS.md defines |
|-------|------|-------------------|
| Root (`AGENTS.md`) | Portfolio protocol, hierarchy rules | How levels relate, PRD workflow, scaffold rules |
| App (`apps/<app>/AGENTS.md`) | Whole product scope | App purpose, architecture conventions, tech stack, glossary, interaction rules with its domains |
| Domain (`apps/<app>/domains/<domain>/AGENTS.md`) | A bounded sub-problem | Domain vocabulary, business rules, data ownership, rules other domains must respect |

Knowledge bases (`knowledge/`) hold the durable facts for each level — glossary terms, decisions, constraints. Manuals live at the level they belong. Never duplicate a fact at two levels without cross-linking.

## Key Commands / Agents

- **Product Owner agent** (`.kilo/agent/product-owner.md`) — the orchestrator. Select it to author PRDs for any app.
- **`/new-app <Name>`** — scaffolds `apps/<slug>/` from `_template` with AGENTS.md, README, domains, knowledge, prds.
- **`/prd <app> <request>`** — runs the PRD authoring workflow inside an app.

## Rules

1. Root `AGENTS.md` stays a protocol — app-specific details live in app files.
2. Every app and domain must have an `AGENTS.md`.
3. PRDs are written to `apps/<app>/prds/YYYY-MM-DD-<slug>.md`, never at root.
4. `apps/_index.md` is the registry — keep it current when adding apps or domains.
5. Do not fabricate requirements; unresolved items go to Open Questions.