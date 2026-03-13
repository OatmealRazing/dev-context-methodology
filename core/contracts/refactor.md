# Refactor Contract

## Inputs
| Source | File/Location | Section/Scope | Why |
|--------|--------------|---------------|-----|
| Refactor scope | Provided by user | Full | What to change and why |
| Project CLAUDE.md | Project root | Architecture | Current structure |
| Existing code | Relevant source files | As needed | Current state |

## Process
1. Read and understand the refactor scope
2. Verify all existing tests pass before starting
3. Make incremental structural changes (one logical change at a time)
4. Run tests after each change
5. Run audit checks
6. Update session state

## Outputs
| Artifact | Location | Format |
|----------|----------|--------|
| Refactored code | Project source files | Per project conventions |
| Session state | Session state file | DCM session format |

## Audit
| Check | Pass Condition |
|-------|---------------|
| Behavior preservation | All tests still pass |
| Scope | Only the agreed refactor -- no feature changes |
| Readability | Code is clearer than before, not just different |
