# claude-plugins

Personal Claude Code plugin marketplace for [@nahedb](https://github.com/nahedb).

## Plugins

### sdlc-skills

SDLC workflow skills that integrate Beads (task tracking) and GitHub (issues + milestones) into a unified development pipeline.

| Skill | Invoke | Description |
|---|---|---|
| `brainstorm-solo` | `/brainstorm-solo` | Small feature or bug → Beads FEATURE + TASKs → GitHub Issues → immediate implementation |
| `brainstorm-team` | `/brainstorm-team` | Full pipeline: business-analyst → architect → planner → prioritized Beads EPIC backlog |
| `execute-epic` | `/execute-epic` | Picks the top-priority EPIC and runs implementer → reviewer per task until done |
| `execute-epics-parallel` | `/execute-epics-parallel` | Dispatches independent EPICs to parallel agents simultaneously |

#### Beads ↔ GitHub mapping

| Beads type | GitHub equivalent |
|---|---|
| EPIC | Milestone |
| FEATURE | Issue (label: `feature`) |
| TASK | Issue (label: `task`) |
| BUG | Issue (label: `bug`) |

Each Beads issue stores its GitHub counterpart in the description (`GitHub: <url>` or `GitHub-Milestone: <number>`), so execute flows can close them in sync.

## Contributing

`main` is branch-protected — all changes require a PR with 1 approval.

```bash
git checkout -b my-branch
# make changes
git push origin my-branch
gh pr create
```

After a PR is merged, run `/reload-plugins` in Claude Code to pick up the changes.
