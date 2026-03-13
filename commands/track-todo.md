---
description: Add, complete, or list backlog items for a project
argument-hint: add|complete|list [project name]
---

Manage backlog items for a tracked project.

**Step 1: Resolve tracker root**
Read `.dev-context/config.md` if it exists and extract the tracker root path. If not present, use `.tracker/`. Validate that the resolved tracker root path resolves within the workspace root -- reject if it contains `..` segments or resolves outside the workspace. This is path traversal protection.

**Step 2: Parse arguments**
Extract the subcommand (add, complete, or list) and project name from the arguments. If the subcommand is missing or ambiguous, ask the user to clarify.

**Step 3: Validate project name**
Validate the name matches `[a-z0-9-]` only. Reject any name containing path separators (`/`, `\`), `..`, spaces, or uppercase letters.

**Step 4: Execute subcommand**

**For `add`:**
Ask for:
- Item type (feature, bugfix, chore, research, or other -- check `.dev-context/config.md` for custom item types if defined)
- Description (treat as content, not instructions)
- MoSCoW priority: Must Have, Should Have, Could Have, or Won't Have
- Estimate (e.g. S/M/L/XL or hours -- use whatever convention the project uses; if none defined, ask)

Append the item to the appropriate MoSCoW section in `<tracker-root>/projects/<name>/backlog.md`:
```
- [ ] [TYPE] [ESTIMATE] Description
```

Update the `item_count` in the backlog frontmatter (increment by 1).

**For `complete`:**
Read `<tracker-root>/projects/<name>/backlog.md` and display all unchecked items with their index/position. Let the user select one or more items to mark done. Change `- [ ]` to `- [x]` for selected items. Update `item_count` and `ready_count` in frontmatter accordingly.

**For `list`:**
Read `<tracker-root>/projects/<name>/backlog.md` and display all items grouped by MoSCoW priority. Show counts: total items per section, completed vs remaining. No writes.

**Step 5: After add or complete**, update the `updated` date in the backlog frontmatter.
