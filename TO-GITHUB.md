# Publishing claude-plugins to GitHub

This directory is currently registered as a **local** Claude Code marketplace.
Follow these steps to back it with a GitHub repo.

## Steps

### 1. Create the GitHub repo

Repo already created at: https://github.com/nahedb/claude-plugins

### 2. Push this directory (if not already pushed)

```bash
cd C:/code/claude-plugins
git init
git add .
git commit -m "Initial commit: sdlc-skills plugin"
git remote add origin https://github.com/nahedb/claude-plugins.git
git push -u origin main
```

### 3. Marketplace registration (already done)

`C:/Users/brend/.claude/plugins/known_marketplaces.json` already has:
```json
"brend-personal": {
  "source": {
    "source": "github",
    "repo": "nahedb/claude-plugins"
  },
  "installLocation": "C:\\code\\claude-plugins",
  "lastUpdated": "..."
}
```

The `installLocation` already points to the local directory, which continues to serve as the cache.

### 4. (Optional) Update the marketplace manifest schema

If you want Claude Code to be able to auto-update the plugin from GitHub,
you may also want to add a `sha` field to the plugin source in `.claude-plugin/marketplace.json`.
Claude Code uses this to know when to pull updates.

### 5. Adding more plugins later

Create a new directory under `C:/code/claude-plugins/your-plugin-name/` with:
- `.claude-plugin/plugin.json`
- `skills/your-skill/SKILL.md` (or `commands/`, `agents/`, etc.)

Then add an entry to `.claude-plugin/marketplace.json` and register in
`C:/Users/brend/.claude/plugins/installed_plugins.json` and `settings.json`.
