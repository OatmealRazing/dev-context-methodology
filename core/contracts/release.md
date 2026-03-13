# Release Contract

## Inputs
| Source | File/Location | Section/Scope | Why |
|--------|--------------|---------------|-----|
| Project CLAUDE.md | Project root | Build/deploy section | Release process |
| Changelog / commits | Git log since last release | Full | What changed |
| Version files | Package/manifest files | Version field | Current version |

## Process
1. Review commits since last release
2. Draft changelog entry
3. Determine version bump (semver)
4. Update version in all relevant files
5. Build and verify
6. Run audit checks
7. Checkpoint: present changelog + version to user for approval
8. Tag and finalize

## Outputs
| Artifact | Location | Format |
|----------|----------|--------|
| Changelog | Per project convention | Markdown |
| Version bump | Package/manifest files | Per project convention |
| Git tag | Repository | vX.Y.Z |

## Audit
| Check | Pass Condition |
|-------|---------------|
| Version consistency | All version files match |
| Changelog accuracy | Every user-facing change documented |
| Build | Clean build at tagged commit |

## Checkpoints
| After Step | Agent Presents | Human Decides |
|------------|---------------|---------------|
| 6 | Changelog draft + proposed version | Approve, edit, or change version |
