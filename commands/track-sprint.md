---
description: Create, close, or show sprint status for a project
argument-hint: create|close|status [project name]
---

Manage sprints for a project. Subcommands: `create`, `close`, `status` (default).

**Step 1: Resolve tracker root**
Read `.dev-context/config.md` if it exists and extract the tracker root path. If not present, use `.tracker/`. Validate that the resolved tracker root path resolves within the workspace root -- reject if it contains `..` segments or resolves outside the workspace. This is path traversal protection.

**Step 2: Validate project name**
Validate the name matches `[a-z0-9-]` only. Reject any name containing path separators (`/`, `\`), `..`, spaces, or uppercase letters.

**Step 3: Parse subcommand**
Read the subcommand from the argument. If no subcommand is given, default to `status`.

---

## Subcommand: status

1. List files in `<tracker-root>/projects/<name>/sprints/`. Find the most recent sprint file by filename (sprint-NNN.md, highest N).
2. If no sprint files exist, report that no sprint has been started for this project.
3. Read the sprint file and display:
   - Sprint number and goal
   - Planned items with their current status (done / in progress / not started)
   - Any unplanned items added during the sprint
   - Days remaining until the sprint end date (calculate from today's date)

---

## Subcommand: create

1. Read `<tracker-root>/projects/<name>/CONTEXT.md` and `<tracker-root>/projects/<name>/backlog.md`.
2. Determine the next sprint number: list files in `<tracker-root>/projects/<name>/sprints/`, find the highest NNN, increment by 1. If no sprints exist, start at 001.
3. Ask the user for:
   - Sprint goal (1-2 sentences)
   - Sprint end date
   - Which backlog items to pull into the sprint (present the ready/prioritized items as options)
4. Create `<tracker-root>/projects/<name>/sprints/sprint-NNN.md` using the following structure:

```
---
sprint: NNN
status: active
goal: <goal>
start_date: YYYY-MM-DD
end_date: YYYY-MM-DD
---

## Goal

<goal>

## Planned Items

| Item | Size | Status |
|------|------|--------|
| <backlog item> | <size> | not started |

## Unplanned Items

| Item | Size | Status |
|------|------|--------|

## Outcome

(Filled in at close)
```

5. Use today's date for `start_date`.
6. Confirm: report sprint number, goal, item count, and end date.

Estimation scale reference:

| Size | Meaning |
|------|---------|
| XS | Minutes. Trivial change, no decisions. |
| S | A few minutes to an hour. Straightforward, no unknowns. |
| M | A few hours. Clear approach, some decisions to make. |
| L | A full session or more. Significant scope or unknowns. |
| XL | Multiple sessions. Should probably be broken down further. |

---

## Subcommand: close

1. Find the most recent active sprint file in `<tracker-root>/projects/<name>/sprints/`.
2. If no active sprint exists, report that and stop.
3. Read the sprint file. Calculate:
   - Planned items completed (X of Y)
   - Unplanned items added (N items)
   - Carried-over items (planned items not completed)
4. Update the sprint file:
   - Set frontmatter `status` to `completed`
   - Fill in the Outcome section:

```
## Outcome

- **Planned**: X of Y completed
- **Unplanned**: N items added
- **Carried over**: list of incomplete planned items
```

5. Move incomplete planned items back to `<tracker-root>/projects/<name>/backlog.md`. Add them at the top of the backlog under a `Carried Over` note, or merge into existing backlog sections as appropriate. Increment `item_count` in the backlog frontmatter by the number of carried-over items.
6. Ask: "Run a retrospective now? (`/track-retro <name>`)"
7. Confirm: report sprint number, completion ratio, and any carried-over items.
