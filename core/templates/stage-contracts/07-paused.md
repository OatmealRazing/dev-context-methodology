# Stage 07: Paused

## Inputs

| Source | Location | Why |
|--------|----------|-----|
| Current stage outputs | ./stages/ | Preserve state at time of pause |
| Backlog | ./backlog.md | Snapshot of remaining work |

## Process

1. Document reason for pausing
2. Record current state (active sprint status, in-progress items)
3. Define resumption criteria (what would trigger un-pausing)
4. Move in-progress items back to ready in backlog
5. Cancel active sprint if one exists (with reason: paused)

## Outputs

| Artifact | Location | Format |
|----------|----------|--------|
| Pause record | ./stages/07-paused.md | Reason, state snapshot, resumption criteria |

## Gate — Exit Criteria

- [ ] Pause reason documented
- [ ] Current state preserved
- [ ] Resumption criteria defined
- [ ] Active work safely suspended
