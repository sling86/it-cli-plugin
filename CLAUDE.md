# CLAUDE.md

This repo is the **public companion plugin** for the `its` IT-systems CLI (binaries: `sling86/its-releases`; source is private).

## Generated content — do not hand-edit

`plugins/it-cli/skills/it-cli/` is synced from the private CLI source:

- `reference/*.md` — auto-generated from the live `CommandDef`s.
- `SKILL.md` — hand-maintained **upstream** (sanitised driving guide), then copied here.

Edits made directly in this repo will be overwritten on the next sync. To change the skill, change it upstream and re-run the publish step.

## Layout

- `.claude-plugin/marketplace.json` — marketplace manifest.
- `plugins/it-cli/.claude-plugin/plugin.json` — plugin manifest.
- `plugins/it-cli/skills/it-cli/` — the skill (synced).

British English throughout.
