# claude-plugins repo — workflow notes

Repo: https://github.com/nahedb/claude-plugins

## Branch protection (main is protected)

`main` requires a pull request with **1 approval** before merging.
Force pushes and direct pushes to `main` are blocked.

**All skill edits must go through a branch + PR:**

```bash
cd C:/code/claude-plugins
git checkout -b <branch-name>
# make changes
git add <files>
git commit -m "description"
git push origin <branch-name>
gh pr create --title "..." --body "..."
# get PR approved, then merge via GitHub
```

## Marketplace registration (already done)

`C:/Users/brend/.claude/plugins/known_marketplaces.json` is configured:
```json
"brend-personal": {
  "source": { "source": "github", "repo": "nahedb/claude-plugins" },
  "installLocation": "C:\\Users\\brend\\.claude\\plugins\\marketplaces\\brend-personal"
}
```

After merging a PR, run `/reload-plugins` in Claude Code to pick up the changes.

## Adding more plugins later

Create a new directory under `C:/code/claude-plugins/your-plugin-name/` with:
- `.claude-plugin/plugin.json`
- `skills/your-skill/SKILL.md` (or `commands/`, `agents/`, etc.)

Then add an entry to `.claude-plugin/marketplace.json`, register in
`C:/Users/brend/.claude/plugins/installed_plugins.json` and `settings.json`.
