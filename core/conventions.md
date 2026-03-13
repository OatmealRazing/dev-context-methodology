# Dev Context Methodology (DCM)

Rules for structuring context in Claude Code projects. Canonical plugin reference. All projects follow these patterns.

---

## Five-Layer Model

Agents read down the layers. They stop as soon as they have what they need.

Layer 0: managed by user, out of plugin scope             "Who am I working with?"          (~500 tokens)
Layer 1: workspace root CLAUDE.md                         "What workspace is this?"          (~400 tokens)
Layer 2: project CLAUDE.md                                "What project am I in?"            (~300-800 tokens)
Layer 3: project/.context/sessions/                       "What am I working on right now?"  (loaded selectively)
Layer 4: session artifacts                                "What am I working with?"          (loaded selectively)

Layer 0 is identity, preferences, and learned facts. Managed by user outside the plugin.
Layer 1 is workspace orientation and operating rules. Auto-loaded by Claude Code. References core/ for methodology details.
Layer 2 is project-specific: stack, build commands, conventions. Auto-loaded when scoped to a project.
Layer 3 is active session state: current task, decisions made, next step. Lives in .context/sessions/ inside each project. Loaded when resuming work. Task routing and project context live in the tracker.
Layer 4 is session artifacts: specs, plans, debug logs, intermediate outputs. Loaded only when explicitly relevant.

Token Discipline: Layers 0-2 combined: under 2,000 tokens. These are targets, not hard limits.
One-Way References: Layer N+1 may reference Layer N. Never the reverse. Same-layer references are allowed.

## Task Contracts

Routing:
| Task Type | Load                   | Contract                              |
|-----------|------------------------|---------------------------------------|
| feature   | contracts/feature.md   | Spec > Implement > Verify             |
| bugfix    | contracts/bugfix.md    | Reproduce > Diagnose > Fix > Verify   |
| release   | contracts/release.md   | Changelog > Version > Build > Publish |
| refactor  | contracts/refactor.md  | Scope > Implement > Verify            |
| spike     | (none)                 | Timebox > Research > Findings doc     |
| chore     | (none)                 | Meets Definition of Done              |

Not every project needs all task types. Lightweight projects can skip contracts entirely.

Contract Shape: Every contract follows the Inputs/Process/Outputs structure:
## Inputs
| Source | File/Location | Section/Scope | Why |
## Process
1. Step one
2. Step two
## Outputs
| Artifact | Location | Format |

Override Order: Project contracts/<type>.md overrides core/contracts/<type>.md. Local beats global.

## Session Handoff

.context/ is sessions-only. A single file captures active task state. Not a log. Not history. The directory is optional.

Directory Structure:
project/.context/
  sessions/
    current.md            (active task state)
    paused/               (paused task snapshots)

Session Rules:
1. One active task per project at a time.
2. Switching tasks: move current.md to sessions/paused/[slug].md before starting the new task.
3. Resuming: restore paused file to current.md.
4. Agent updates current.md when a process step completes, when pausing, or when asked.
5. User can edit current.md between sessions to steer direction.
6. Cleared on task completion. Nothing accumulates.

## Naming Conventions
| Item | Convention |
|------|-----------|
| Session directory | project/.context/sessions/ |
| Active session | project/.context/sessions/current.md |
| Paused sessions | project/.context/sessions/paused/ |
| Custom task contracts | project/contracts/ (optional) |
| Default contracts | core/contracts/ |
| Templates | core/templates/ |
| Tracker root | configured in .dev-context/config.md, defaults to .tracker/ |
| Tracker project files | <tracker-root>/projects/<name>/ |
| Tracker insights | <tracker-root>/insights/ |

Folders and files: lowercase-with-hyphens. No spaces in names.

## Tracker Integration

<tracker-root>/ and .context/ are separate systems with different purposes.

| System | Owns |
|--------|------|
| <tracker-root>/ | Lifecycle stage, priority, backlog, task routing, incidents, metrics, sprints, retrospectives |
| .context/ | Active session state (current task, decisions, next step) |

Read Order on Session Start:
1. Project CLAUDE.md
2. <tracker-root>/projects/<name>/CONTEXT.md (lifecycle, routing, project context)
3. .context/sessions/current.md (active task state, if present)

## Quality Guardrails

- Tracker CONTEXT.md files: under 80 lines
- Reference files: under 200 lines (split if longer)
- Layers 0-2 combined: under 2,000 tokens
- Plain English. No jargon. No em dashes.
- Every piece of information has one canonical home. Other files point to it.
- CONTEXT.md files contain routing, not content.
- Docs and reference material are authoritative. Past outputs are artifacts, not templates.
- Contracts include a verification step in the Process section.

## Incident Tracking

The agent self-reports incidents proactively during work, not just when invoked via /track-incident. Five types:

| Type | When to Log |
|------|------------|
| agent-error | Agent took wrong approach, produced bad output, had to redo work |
| agent-correction | User corrected agent's behavior or approach |
| contract-failure | A task contract's audit/verification step failed |
| stage-block | A stage gate could not be met |
| scope-change | Unplanned work disrupted planned sprint work |

Incidents are logged to <tracker-root>/projects/<name>/incidents.md. The "Prevention" field is required for every incident and feeds retrospective "Process Changes."

## Command Reference

Commands:
| Command | Purpose |
|---------|---------|
| /track-new | Onboard a new project |
| /track-status | Show project or workspace status |
| /track-advance | Advance to next lifecycle stage |
| /track-todo | Manage backlog items |
| /track-update | Quick tracker edits |
| /track-refresh | Rebuild overview dashboard |
| /track-incident | Log an incident |
| /track-sprint | Sprint lifecycle management |
| /track-metrics | Generate project metrics |

Skills (activate automatically):
| Skill | Triggers when |
|-------|--------------|
| track-suggest | User asks what to work on next |
| track-insights | User asks for cross-project analysis |
| track-retro | User wants to run a retrospective |
