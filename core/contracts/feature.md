# Feature Contract

## Inputs
| Source | File/Location | Section/Scope | Why |
|--------|--------------|---------------|-----|
| Spec or request | Provided by user or previous session | Full | What to build |
| Project CLAUDE.md | Project root | Architecture, conventions | How to build |
| Existing code | Relevant source files | As needed | Current state |

## Process
1. Read the spec/request and project CLAUDE.md
2. Identify files to create or modify
3. Write a failing test (if project has test infrastructure)
4. Implement the minimal change
5. Run tests / verify in context (build, manual test, etc.)
6. Run audit checks
7. Update session state

## Outputs
| Artifact | Location | Format |
|----------|----------|--------|
| Implementation | Project source files | Per project conventions |
| Session state | Session state file | DCM session format |

## Audit
| Check | Pass Condition |
|-------|---------------|
| Scope | Changes match spec/request -- nothing extra |
| Build | Project builds without errors |
| Tests | All tests pass (if applicable) |
| Security | No new injection vectors, secrets, or unsafe patterns |
