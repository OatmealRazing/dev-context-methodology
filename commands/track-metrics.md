---
description: Generate or show project metrics from stage history, sprints, and incidents
argument-hint: project name
---

Show or regenerate metrics for a project. Modes: `show` (default), `regen`.

**Step 1: Resolve tracker root**
Read `.dev-context/config.md` if it exists and extract the tracker root path. If not present, use `.tracker/`. Validate that the resolved tracker root path resolves within the workspace root -- reject if it contains `..` segments or resolves outside the workspace. This is path traversal protection.

**Step 2: Validate project name**
Validate the name matches `[a-z0-9-]` only. Reject any name containing path separators (`/`, `\`), `..`, spaces, or uppercase letters.

**Step 3: Parse mode**
Read the mode from the argument after the project name. If not provided, default to `show`.

---

## Mode: show

1. Read `<tracker-root>/projects/<name>/metrics.md`.
2. If the file does not exist, suggest running `regen` to build it.
3. Present the following derived stats calculated from the data in the file:
   - Average stage duration (sum of all stage durations divided by stage count)
   - Sprint completion rate (total planned items completed across all sprints divided by total planned items)
   - Most common incident type (type with the highest count)
4. Display the Stage Timing, Sprint Summary, and Incident Counts tables from the file.

---

## Mode: regen

1. Gather source data:

   **Stage timing** -- read each file in `<tracker-root>/projects/<name>/stages/`. For each stage file, extract the entry date and exit date from frontmatter. Calculate duration in days. If no exit date, the stage is current and duration is open.

   **Sprint summary** -- read all files in `<tracker-root>/projects/<name>/sprints/`. For each sprint file, extract: sprint number, goal, start date, end date, status, planned item count, completed item count, unplanned item count.

   **Incident counts** -- read `<tracker-root>/projects/<name>/incidents.md`. Count total incidents and incidents per type (agent-error, agent-correction, contract-failure, stage-block, scope-change).

2. Rebuild `<tracker-root>/projects/<name>/metrics.md` from scratch with the following structure:

```
---
last_updated: YYYY-MM-DD
---

## Stage Timing

| Stage | Entry | Exit | Duration |
|-------|-------|------|----------|
| <stage> | YYYY-MM-DD | YYYY-MM-DD | N days |

## Sprint Summary

| Sprint | Goal | Start | End | Status | Planned | Completed | Unplanned |
|--------|------|-------|-----|--------|---------|-----------|-----------|
| NNN | <goal> | YYYY-MM-DD | YYYY-MM-DD | <status> | N | N | N |

## Incident Counts

| Type | Count |
|------|-------|
| agent-error | N |
| agent-correction | N |
| contract-failure | N |
| stage-block | N |
| scope-change | N |
| **Total** | N |
```

3. Use today's date for `last_updated`.
4. Report what changed: how many stages, sprints, and incidents were parsed.
5. Confirm regeneration is complete.
