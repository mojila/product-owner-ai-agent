---
description: Scaffold a new app from the _template placeholder
agent: product-owner
---

Scaffold a new app under `apps/` named: $ARGUMENTS

Follow the Product Owner agent's New App Workflow:
1. Derive a kebab-case slug from the app name.
2. Copy `apps/_template/` → `apps/<slug>/`.
3. Replace `_template` placeholders with the real app and domain names (AGENTS.md, README.md, config.md, _index.md, log.md, domains/).
4. Add vertical domains, each with AGENTS.md + knowledge/_index.md + log.md.
5. Update `apps/_index.md` registry.
6. Do NOT author a PRD yet — inform the user the app is scaffolded and ask if they want `/prd`.