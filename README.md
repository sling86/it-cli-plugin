# it-cli — Claude Code plugin for the `its` IT-systems CLI

[![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-d97757?logo=anthropic&logoColor=white)](https://docs.claude.com/en/docs/claude-code/plugins)
[![Providers](https://img.shields.io/badge/providers-19-2563eb)](#providers)
[![Commands](https://img.shields.io/badge/commands-639-2563eb)](#providers)
[![its release](https://img.shields.io/github/v/release/sling86/its-releases?include_prereleases&label=its%20release&sort=semver&logo=github)](https://github.com/sling86/its-releases/releases)
[![Downloads](https://img.shields.io/github/downloads/sling86/its-releases/total?label=downloads&logo=github)](https://github.com/sling86/its-releases/releases)
[![Licence](https://img.shields.io/badge/licence-proprietary-lightgrey)](#licence)

Teach [Claude Code](https://claude.com/claude-code) to drive [**`its`**](https://github.com/sling86/its-releases) — one CLI over the SaaS and self-hosted tools IT admins run all day. Install the plugin and just ask: *"which RMM agents are offline?"*, *"show unlicensed Entra users"*, *"redeploy the web app"*, *"rotate the shared env on staging"* — Claude reaches for `its` and runs it.

One command shape — `its <provider> <resource> [action]` — across **19 providers, 639 commands**, with consistent output modes (`--ai` / `--json` / `--csv`), filtering, and **secret redaction on by default**.

> **Tags:** `claude-code` · `claude-code-plugin` · `cli` · `devops` · `sysadmin` · `microsoft-365` · `entra` · `intune` · `bitwarden` · `dokploy` · `unifi`

## Quick start

**1 — install the `its` CLI** (prebuilt binaries — no Bun, no clone):

```bash
# Linux  → ~/.local/bin
curl -fsSL https://github.com/sling86/its-releases/releases/latest/download/install.sh | bash
```

```powershell
# Windows → %LOCALAPPDATA%\Programs\its (+ PATH)
irm https://github.com/sling86/its-releases/releases/latest/download/install.ps1 | iex
```

**2 — add the plugin** in Claude Code:

```
/plugin marketplace add sling86/it-cli-plugin
/plugin install it-cli@it-cli
```

That's it. Claude now knows how to drive `its` — when to use it, the flags, the safety rules, and the full command surface (loaded on demand, per provider).

## Providers

| Alias | Provider | Covers |
| ----- | -------- | ------ |
| `rmm` | Tactical RMM | Endpoint agents — status, processes, services, updates, remote run |
| `entra` | Entra ID | Users, groups, licences, roles, MFA, sign-ins, CA policies, on/offboarding |
| `intune` | Intune | Managed devices, apps, scripts, remediations, compliance, ESP, Autopilot |
| `exo` | Exchange Online | Distribution groups, mailboxes, permissions, forwarding, rules, trace |
| `sp` | SharePoint | Sites, drives, files, lists, permissions, search |
| `outlook` | Outlook | Signed-in user's mail, calendar, contacts (delegated) |
| `dokploy` | Dokploy | Apps, projects, databases, domains, env (app + shared), deployments |
| `bw` | Bitwarden | Vault search, item/password/TOTP retrieval, password audits |
| `unifi` | UniFi Network | Devices, clients, WLANs, firewall, PoE, alarms, guests |
| `protect` | UniFi Protect | Cameras, NVR storage, motion events |
| `wrike` | Wrike | Tickets, tasks, projects, contacts |
| `az` | Azure | Subscriptions, VMs, resources, Key Vault, storage, costs |
| `cf` | Cloudflare | Zones, DNS, tunnels |
| `pbi` | Power BI | Workspaces, reports, datasets |
| `pa` | Power Platform | Environments, apps, flows |
| `hr` | PeopleHR | Employee directory, starters, leavers |
| `bc` | Business Central | Companies, OData entity queries |
| `gh` | GitHub | Branch protection, webhooks |
| `docs` | Docs UI | Browser-based command explorer |

Plus cross-provider globals: `its find` · `its health` · `its audit` · `its inventory` · `its digest`.

## What's in the plugin

| Path | Purpose |
| ---- | ------- |
| `plugins/it-cli/skills/it-cli/SKILL.md` | Driving guide — invocation, output modes, global flags, safety, provider index |
| `plugins/it-cli/skills/it-cli/reference/` | Per-provider command reference (progressive disclosure — one file per provider) |

The skill is lean by design: ~350 tokens always-on, with each provider's full command list pulled in only when needed.

## Maintenance

Everything under `plugins/it-cli/skills/it-cli/` is **generated and synced** from the `its` CLI's own command definitions — **don't hand-edit it here**. The per-provider reference is produced from the live `CommandDef`s and the curated `SKILL.md` is maintained upstream, so the docs can't drift from the CLI.

## Licence

The `its` CLI is distributed under a [proprietary licence](https://github.com/sling86/its-releases) that permits free personal and internal-organisation use. This repo is the Claude Code plugin (documentation) for it.
