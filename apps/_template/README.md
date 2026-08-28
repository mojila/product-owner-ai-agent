# <App Name> — Overview

One paragraph: what this app does, for whom, and why it exists.

---

This is a **wiki-managed app** scaffolded from `apps/_template/`. Structure:

```
<app>/
├── AGENTS.md              # App-level agent protocol
├── README.md              # This overview
├── config.md              # Title, scope, conventions, domains
├── log.md                 # Activity log
├── _index.md              # Master index: stats, navigation, recent changes
├── inbox/                 # Drop zone — dump PRD requests & material here
├── raw/                   # Immutable sources (interviews/, research/, feedback/, ...)
├── knowledge/             # Compiled articles (concepts/, topics/, references/)
├── inventory/             # Durable tracking (ideas/, candidates/, entities/)
├── domains/<domain>/      # Bounded sub-problems (AGENTS.md + knowledge/)
└── prds/                  # PRD output artifacts (YYYY-MM-DD-<slug>.md)
```

Read [AGENTS.md](AGENTS.md) before working inside this app; read [prds/PRD-TEMPLATE.md](prds/PRD-TEMPLATE.md) before authoring a PRD.