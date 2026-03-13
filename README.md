# Dev Context Methodology (DCM)

A Claude Code plugin for structured project lifecycle management. Provides context layering, task contracts, project tracking, sprint management, incident logging, and cross-project insights.

## Install

```
claude plugin add github:alexshrk/dev-context-methodology
```

## Quick Start

After installing, run the init wizard in your workspace:

```
/dev-context-init
```

This walks you through:
1. Workspace setup (generates CLAUDE.md, .tracker/, .dev-context/)
2. First project onboarding (optional)
3. Customizing defaults (optional)

## Commands

| Command | Purpose |
|---------|---------|
| `/track-new` | Onboard a new project |
| `/track-status` | Show project or workspace status |
| `/track-advance` | Advance to next lifecycle stage |
| `/track-todo` | Manage backlog items |
| `/track-update` | Quick tracker edits |
| `/track-refresh` | Rebuild overview dashboard |
| `/track-incident` | Log an incident |
| `/track-sprint` | Sprint lifecycle management |
| `/track-metrics` | Generate project metrics |

## Skills (activate automatically)

| Skill | Triggers when |
|-------|--------------|
| `track-suggest` | You ask what to work on next |
| `track-insights` | You ask for cross-project analysis |
| `track-retro` | You want to run a retrospective |

## Methodology

For a tool-agnostic description of DCM, see [methodology/DCM.md](methodology/DCM.md).

## What This Plugin Accesses

All operations are local. The plugin only reads and writes files within your workspace root:
- `.dev-context/` — your editable config
- `.tracker/` — operational project tracking data
- `CLAUDE.md` — workspace and project context files
- `.context/` — per-project session state

No external network calls. No data leaves your machine.

## License

MIT
