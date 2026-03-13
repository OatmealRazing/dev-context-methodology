# {{WORKSPACE_NAME}}

{{WORKSPACE_DESCRIPTION}}

## Operating Rules

- Lazy loading: don't scan/read subdirectories on startup. Only load files for the active project.
- Scoped operations: all file ops scoped to active project's directory.
- One-way references: Layer N+1 may reference Layer N, never reverse.

## Working on a Project

1. Scope to project subdirectory
2. Read project's CLAUDE.md
3. Read {{TRACKER_PATH}}/projects/<name>/CONTEXT.md for stage, routing, and to-dos
4. If project has .context/sessions/current.md: read for active task state
5. Limit file ops to project folder
6. Update tracker and session state as work progresses

## Project Tracker ({{TRACKER_PATH}})

Source of truth for lifecycle stage, priority, and to-dos.
Commands: /track-new, /track-status, /track-advance, /track-todo, /track-suggest, /track-update, /track-refresh, /track-incident, /track-sprint, /track-retro, /track-insights, /track-metrics

{{TEAM_SECTION}}

## Incident Self-Reporting

When working on any project, proactively log incidents using /track-incident when you:
- Take a wrong approach or produce bad output that requires redo (agent-error)
- Get corrected by the user on behavior or approach (agent-correction)
- Fail a task contract's audit or verification step (contract-failure)
- Cannot meet a stage gate's exit criteria (stage-block)
- Encounter unplanned work that disrupts planned sprint work (scope-change)

Log the incident immediately when detected. Include a Prevention field describing what would prevent recurrence. These feed into retrospectives and cross-project insights.
