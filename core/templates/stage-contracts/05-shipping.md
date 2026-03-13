# Stage 05: Shipping

## Inputs

| Source | Location | Why |
|--------|----------|-----|
| Developing outputs | ./stages/04-developing.md | Confirms all Must/Should items done |
| Definition of Done | ./definition-of-done.md | Quality standard |
| Backlog | ./backlog.md | Verify no critical items remaining |

## Process

1. Final code review and quality check
2. Security review (dependencies, inputs, secrets)
3. Write or update user-facing documentation
4. Tag version in repository
5. Build release artifacts
6. Deploy to release channels
7. Announce release (if applicable)

## Outputs

| Artifact | Location | Format |
|----------|----------|--------|
| Shipping summary | ./stages/05-shipping.md | Version, channels, release notes |

## Gate — Exit Criteria

- [ ] Final review completed
- [ ] Security review passed
- [ ] User documentation exists
- [ ] Version tagged in repository
- [ ] Released to at least one channel
- [ ] Deployed and verified working
