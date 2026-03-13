---
description: Advance a project to its next lifecycle stage
argument-hint: project name
---

Advance a tracked project to its next lifecycle stage.

**Step 1: Resolve tracker root**
Read `.dev-context/config.md` if it exists and extract the tracker root path. If not present, use `.tracker/`. Validate that the resolved tracker root path resolves within the workspace root -- reject if it contains `..` segments or resolves outside the workspace. This is path traversal protection.

**Step 2: Validate project name**
Validate the name matches `[a-z0-9-]` only. Reject any name containing path separators (`/`, `\`), `..`, spaces, or uppercase letters.

**Step 3: Read current state**
Read `<tracker-root>/projects/<name>/CONTEXT.md` and extract the current stage.

**Step 4: Determine next stage**
Standard order: Idea > Planning > Setup > Developing > Shipping > Maintaining

Special transitions available from any stage: Paused, Archived

If the project is currently Paused, read the `paused_from` field in CONTEXT.md frontmatter to determine the previous stage. Ask whether to resume (restore to that stage) or archive.

If the user wants to transition to Paused: ask for a reason, record the current stage in the `paused_from` frontmatter field, and add the reason in CONTEXT.md under a "Pause/Archive Notes" section.

If the user wants to transition to Archived: ask for a reason and record it in CONTEXT.md under a "Pause/Archive Notes" section.

**Step 5: Check gate criteria**
Read the stage contract for the current stage from `<tracker-root>/templates/stage-contracts/`. File naming convention: `01-idea.md`, `02-planning.md`, `03-setup.md`, `04-developing.md`, `05-shipping.md`, `06-maintaining.md`.

Compare the gate checklist in the contract against the gate checklist recorded in the project's CONTEXT.md.

Present the checklist to the user showing which items are checked and which remain unchecked.

If items are unchecked: tell the user which items remain, then ask whether they want to (a) go back and complete them first, or (b) skip the gate and advance anyway.

**Step 6: Confirm before writing**
Ask the user to confirm the transition before making any changes.

**Step 7: Update CONTEXT.md**
- Change `stage` frontmatter value to the new stage
- Update `updated` date
- Keep the completed gate checklist as historical record under a "Completed Gates" section
- Append the new stage's gate checklist under "Stage Gate: <New Stage>"

**Step 8: Save stage transition record**
Write a file to `<tracker-root>/projects/<name>/stages/<date>-<old-stage>-to-<new-stage>.md` with:
- Transition date
- From stage, to stage
- Gate checklist state at time of transition
- Reason (if paused or archived)

**Step 9: Run `/track-refresh`** to rebuild the overview.

**Step 10: Confirm** the transition to the user.
