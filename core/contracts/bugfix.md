# Bugfix Contract

## Inputs
| Source | File/Location | Section/Scope | Why |
|--------|--------------|---------------|-----|
| Bug report | Provided by user | Full | What's broken |
| Project CLAUDE.md | Project root | Architecture | Where to look |
| Existing code | Relevant source files | As needed | Current state |

## Process
1. Reproduce the bug (confirm it exists, understand conditions)
2. Diagnose root cause (not symptoms)
3. Write a failing test that captures the bug (if test infra exists)
4. Implement the minimal fix
5. Run tests / verify the fix
6. Run audit checks
7. Update session state

## Outputs
| Artifact | Location | Format |
|----------|----------|--------|
| Fix | Project source files | Per project conventions |
| Session state | Session state file | DCM session format |

## Audit
| Check | Pass Condition |
|-------|---------------|
| Root cause | Fix addresses cause, not symptom |
| Regression | No existing tests broken |
| Scope | Only the bug is fixed -- no drive-by refactoring |
