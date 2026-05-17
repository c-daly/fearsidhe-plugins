# Edit flow

This repo is the canonical editing clone of `c-daly/fearsidhe-plugins`.

## To bump a plugin SHA pin

1. Edit `.claude-plugin/marketplace.json` here.
2. Commit and push:
   ```
   git add .claude-plugin/marketplace.json
   git commit -m "<plugin>: bump SHA pin to <sha>"
   git push
   ```
3. Claude Code's runtime clone at `~/.claude/plugins/marketplaces/fearsidhe-plugins/` pulls from origin on the next marketplace refresh (or trigger via `/plugin marketplace update fearsidhe-plugins` in a CC session).

## What NOT to edit

Do NOT create a duplicate clone of this repo at `~/.claude/plugins/`. That co-location is what caused the 2026-05-17 corruption incident: an agent ran `git reset --hard FETCH_HEAD` while in the wrong cwd, repointing the marketplace's local `main` branch at the memory plugin's HEAD and producing a botched cross-history rebase. Recovery required `git rebase --abort` followed by `git reset --hard origin/main`.

`~/.claude/plugins/` is reserved for Claude Code runtime infrastructure only:

- `cache/` — installed plugin versions per SHA pin
- `marketplaces/` — CC's own clones of known marketplaces (CC-managed; never edit directly)
- `data/` — plugin runtime data
- `installed_plugins.json`, `known_marketplaces.json`, `install-counts-cache.json` — CC state files
- `memory/`, `continuity/`, `agent-swarm/` — plugin source dev trees (each its own git repo)
- `.claude/`, `.context/` — untracked CC / agent-swarm local state

The plugins-root directory is no longer a git repository at its root.

## The three locations, summarized

| Path | What it is | Git? |
|---|---|---|
| `~/projects/fearsidhe-plugins/` | This repo — editing clone | Yes (`c-daly/fearsidhe-plugins`) |
| `~/.claude/plugins/marketplaces/fearsidhe-plugins/` | CC's runtime clone | Yes (CC-managed; pulls from origin) |
| `~/.claude/plugins/` | CC runtime root | No (not a git repo) |

Data flows: edit here → push to origin → CC pulls into its runtime clone → CC installs from the SHA pin into `cache/`.
