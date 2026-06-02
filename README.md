# it-cli-plugin

A [Claude Code](https://claude.com/claude-code) plugin that teaches Claude how to drive [`its`](https://github.com/sling86/its-releases) — a unified CLI over the SaaS and self-hosted tools IT admins run day to day.

One skill, one command shape (`its <provider> <resource> [action]`), every provider: Tactical RMM, Entra ID, Intune, Exchange Online, SharePoint, Outlook, Dokploy, Bitwarden, UniFi Network & Protect, Wrike, Azure, Cloudflare, Power BI, Power Platform, PeopleHR, Business Central.

## Install

**1. Install the `its` CLI** (prebuilt binaries — no Bun, no clone):

```bash
# Linux
curl -fsSL https://github.com/sling86/its-releases/releases/latest/download/install.sh | bash
```

```powershell
# Windows
irm https://github.com/sling86/its-releases/releases/latest/download/install.ps1 | iex
```

**2. Add this plugin marketplace in Claude Code:**

```
/plugin marketplace add sling86/it-cli-plugin
/plugin install it-cli@it-cli
```

Then ask Claude about your infrastructure — "which RMM agents are offline?", "show me unlicensed Entra users", "redeploy the web app" — and it will reach for `its`.

## What's in it

| Path | Purpose |
| ---- | ------- |
| `plugins/it-cli/skills/it-cli/SKILL.md` | The driving guide — invocation, output modes, global flags, safety |
| `plugins/it-cli/skills/it-cli/reference/` | Per-provider command reference (one file per provider) |

## Maintenance

The skill content under `plugins/it-cli/skills/it-cli/` is **generated and synced** from the `its` CLI's own command definitions — do not hand-edit it here. The per-provider reference is produced from the live `CommandDef`s; the curated `SKILL.md` is maintained upstream. Changes flow in via a publish step, so the reference can't drift from the CLI.

## Licence

The `its` CLI is distributed under a [proprietary licence](https://github.com/sling86/its-releases) that permits free personal and internal-organisation use. This plugin provides documentation for using it.
