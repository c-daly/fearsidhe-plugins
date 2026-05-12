# fearsidhe-plugins

Personal Claude Code plugin marketplace by c-daly.

## Plugins

- **agent-swarm** — enforcement system for agent-based workflows
- **memory** — durable agent observation memory (user/feedback/project/reference)
- **continuity** — cross-project surfacing and meta-concerns curation

## Usage

```
/plugin marketplace add github:c-daly/fearsidhe-plugins
/plugin install memory@fearsidhe-plugins
/plugin install continuity@fearsidhe-plugins
/plugin install agent-swarm@fearsidhe-plugins
```

## Maintenance

Each plugin is SHA-pinned in `.claude-plugin/marketplace.json`. To bump:

1. Push to the plugin's own repo (`c-daly/<plugin>`).
2. Update the `commit` and `sha` fields for that plugin in `marketplace.json`.
3. Commit + push this repo.
4. In Claude Code: `/plugin marketplace update fearsidhe-plugins` and `/plugin update <plugin>@fearsidhe-plugins`.
