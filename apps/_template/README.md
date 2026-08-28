# <App Name> — Placeholder Template

This is the **placeholder app template**. No real apps exist in the portfolio yet. This directory (`_template`) is the canonical starter structure that the Product Owner agent copies via `/new-app` when a real app is scaffolded.

```
apps/_template/
├── AGENTS.md              # App-level agent — scope, conventions, domain interactions
├── README.md              # One-paragraph app overview
├── knowledge/             # App-level knowledge base (facts, glossary, decisions)
│   ├── _index.md
│   └── CHANGELOG.md
├── domains/
│   └── _template/         # Domain scaffold to duplicate per real domain
│       ├── AGENTS.md      # Domain-level agent
│       └── knowledge/
│           └── _index.md
└── prds/
    ├── PRD-TEMPLATE.md    # Canonical PRD template
    └── _index.md
```

Placeholder file purpose:

| File | Purpose |
|------|---------|
| `apps/_template/AGENTS.md` | Template for app-level agent instructions |
| `apps/_template/domains/_template/AGENTS.md` | Template for domain-level agent instructions |
| `apps/_template/prds/PRD-TEMPLATE.md` | Canonical PRD outline every PRD must follow |
| `knowledge/` dirs | Where durable facts live per level |

**Important**: `_template` must never appear in the app registry (`apps/_index.md`) as a real app. The Product Owner agent must interpret "no apps yet" by looking at this placeholder.