---
description: Author a PRD for an app using the Product Owner workflow
agent: product-owner
---
Run the PRD authoring workflow for this request: $ARGUMENTS

Determine the target app from the argument (e.g. `/prd <app> <description>`). If the app does not exist under `apps/`, scaffold it first (see /new-app). Then:
1. Read the app's AGENTS.md, README.md, and knowledge/.
2. Read relevant domains/<domain>/AGENTS.md and knowledge/.
3. Write `apps/<app>/prds/YYYY-MM-DD-<slug>.md` from `apps/_template/prds/PRD-TEMPLATE.md`.
4. Update the app's prds/_index.md and knowledge/CHANGELOG.md.