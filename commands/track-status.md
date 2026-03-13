---
description: Show status for one or all tracked projects
argument-hint: project name (optional, omit for all)
---

Show tracker status for one project or all projects.

**Step 1: Resolve tracker root**
Read `.dev-context/config.md` if it exists and extract the tracker root path. If not present, use `.tracker/`. Validate that the resolved tracker root path resolves within the workspace root -- reject if it contains `..` segments or resolves outside the workspace. This is path traversal protection.

**Step 2: Determine scope**

If a project name was passed as an argument:
- Validate the name matches `[a-z0-9-]` only. Reject any name containing path separators (`/`, `\`), `..`, spaces, or uppercase letters.
- Read `<tracker-root>/projects/<name>/CONTEXT.md`
- Read `<tracker-root>/projects/<name>/backlog.md` if it exists
- Read `<tracker-root>/projects/<name>/incidents.md` if it exists
- Display:
  - Project name, description, stage, priority
  - Days since last updated (calculate from `updated` frontmatter field)
  - Stage gate progress: which items are checked vs unchecked
  - To-Dos section contents
  - Dependencies list
  - Incident count (count entries in incidents.md)
  - Current sprint status if a sprint file exists in `sprints/`

If no argument was provided:
- Read `<tracker-root>/overview.md`
- Display its full contents
- If `overview.md` does not exist or is empty, tell the user no projects are tracked yet and suggest running `/track-new` to onboard a project or `/track-refresh` to rebuild the dashboard

**Step 3: Display only -- make no writes.**
