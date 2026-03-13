---
name: track-retro
description: Use when the user wants to run a sprint retrospective, review what worked and what didn't, or extract process improvements from recent work
---

## Instructions

1. Resolve tracker root: read `.dev-context/config.md` if present and extract `tracker_root`. If the file is absent or the field is missing, default to `.tracker/`.

2. Validate that the resolved tracker root path stays within the workspace root. If the path contains `..` segments that escape the workspace root, abort and report an error.

3. Verify templates exist: check that `<tracker-root>/templates/` exists and contains `retrospective.md`. If missing, automatically copy all templates from the plugin's `core/templates/` directory to `<tracker-root>/templates/` (including the `stage-contracts/` subdirectory). Inform the user: "Templates were missing — copied from plugin. Consider running `/dev-context-init` if this is a fresh workspace."

4. Ask the user for the project name if not already provided. Validate the project name matches `[a-z0-9-]+` only. Reject any value containing path separators (`/`, `\`) or `..`. Abort with an error if validation fails.

5. Read `<tracker-root>/projects/<name>/CONTEXT.md`.

6. Find the most recently completed sprint by listing files in `<tracker-root>/projects/<name>/sprints/` and selecting the file with the latest name (sprints are named sequentially, e.g., `sprint-001.md`). Read the sprint file and check the `status` frontmatter field. If the status is `active` (not `completed`), warn the user that this sprint is still active and ask whether to proceed with the retro anyway or close the sprint first via `/track-sprint close`.

7. Read the sprint record. Extract:
   - Sprint number and date range
   - Planned items vs. completed items
   - Unplanned work added during the sprint
   - Items carried over to the next sprint

8. Read `<tracker-root>/projects/<name>/incidents.md`. Filter to incidents whose date falls within the sprint's date range.

9. Present a sprint summary to the user:
   - Sprint outcome (planned vs. completed ratio, carry-over count)
   - Incidents that occurred during the sprint with their prevention suggestions
   - Unplanned work that was added

10. Facilitate a retrospective discussion in three parts:

   **What Worked?**
   Ask the user what went well. Record their response.

   **What Didn't?**
   Ask the user what was difficult or did not go as planned. Seed the conversation by referencing incidents logged during the sprint and any carried-over items. Record the user's response.

   **Process Changes?**
   Based on incidents from the sprint, propose concrete process changes (for example: "Add integration test step before marking items done" if there were regression incidents). Present proposals one at a time. The user confirms, edits, or rejects each one.

11. For each confirmed or edited process change: create a chore item in the project's backlog at `<tracker-root>/projects/<name>/backlog.md`.

12. Save the retrospective to `<tracker-root>/projects/<name>/retrospectives/sprint-NNN-retro.md` where `NNN` is the zero-padded sprint number. Use the retrospective template from `<tracker-root>/templates/retrospective.md`. Populate the template with the sprint summary, user responses, and confirmed process changes.

13. Present the completed retrospective file content to the user for review before saving. Ask for confirmation or edits.
