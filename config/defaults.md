# Plugin Defaults

## Tracker

- **Tracker root**: `.tracker/` (relative to workspace root)
- **Team mode**: solo

## Paths

All paths are relative to workspace root. The tracker root can be overridden in `.dev-context/config.md`.

## Backlog Frontmatter Fields

| Field | Type | Description |
|-------|------|-------------|
| `item_count` | int | Total number of items ever added to the backlog. Incremented on add and sprint carry-over. Never decremented. |
| `ready_count` | int | Number of items marked complete (`[x]`). Incremented when items are completed. Never decremented. |
| `last_refined` | date | Date of last backlog modification. |

## Overridable Config

The following files are copied to `.dev-context/` during init. Users edit their copies; plugin defaults remain untouched.

| File | Purpose |
|------|---------|
| definition-of-done.md | Quality checklist for completed items |
| item-types.md | Task type definitions and estimation scale |
| priorities.md | Project and backlog priority levels |
