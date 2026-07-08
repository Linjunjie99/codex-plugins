# codex-plugins

Personal Codex plugin marketplace for TU.

## Plugins

- `slides`: creates browser-based HTML slide presentations with modular slide files, layout templates, themes, and page-by-page navigation.

## Local setup

From this repository root:

```powershell
codex plugin marketplace add .
codex plugin add slides@codex-plugins
```

After installing or updating a plugin, start a new Codex thread so the newly installed skill is loaded.

## Repository layout

```text
.agents/plugins/marketplace.json
plugins/slides/.codex-plugin/plugin.json
plugins/slides/skills/slides/SKILL.md
```
