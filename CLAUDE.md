@AGENTS.md

## Claude Code

The protocol above is imported from `AGENTS.md` and is shared with Kilo. This section holds Claude Code-specific wiring only.

### Where things live

| Concern | Path |
|---------|------|
| Product Owner subagent | `.claude/agents/product-owner.md` |
| Slash commands | `.claude/commands/new-app.md`, `.claude/commands/prd.md` |
| Skills | `.claude/skills/<name>/SKILL.md` |
| Canonical workflow (shared) | `.kilo/agent/product-owner.md` |

`/new-app` and `/prd` both run with `context: fork` + `agent: product-owner`, so each executes in a forked Product Owner subagent rather than in the main conversation.

### Tool mapping

The Kilo agent definition names Kilo tools. In Claude Code: `read`→`Read`, `write`→`Write`, `edit`→`Edit`, `grep`→`Grep`, `glob`→`Glob`, `bash`→`Bash`, `webfetch`→`WebFetch`, `task`→`Agent` (subagents), `question`→ ask in chat. There is no `LS` tool — list directories with `Bash` or `Glob`.

### Reminders

- Read `apps/_index.md` before touching any app; read the app's `_index.md` and `AGENTS.md` before writing into it.
- PRDs go to `apps/<app>/prds/YYYY-MM-DD-<slug>.md`, never at root.
- `raw/` is immutable. Synthesize in `knowledge/` and `prds/`.
- Append to the relevant `log.md` after every operation; never edit past entries.
