---
name: it-cli
description: "Use the `its` CLI to query and manage IT infrastructure from one shell — across endpoint/RMM agents, Entra ID users & groups, Intune devices, Microsoft 365 (Exchange Online, SharePoint, Outlook), Dokploy deployments, Bitwarden vaults, UniFi network & UniFi Protect CCTV, Wrike tickets, Azure resources, Cloudflare, Power BI, Power Platform, PeopleHR, and Business Central. Reach for `its` for any question about servers, devices, users, licences, passwords, deployments, network, tickets, files, or cloud resources. Prefer the `its` wrapper over the bare `dokploy`, `bw`, `az`, or Exchange Online PowerShell. Trigger on: agent/device health, user lookup, group membership, licence assignment, password/TOTP retrieval, deployment status, firewall rules, ticket updates, mailbox management, security audits, onboarding/offboarding, or any infrastructure management task."
---

# IT CLI (`its`)

`its` is a unified CLI over the SaaS and self-hosted tools IT admins run day to day. One command shape, one set of output modes, one set of flags — across every provider. Reach for it first whenever the user asks about infrastructure, identity, endpoints, deployments, passwords, network, tickets, files, or cloud resources.

Binary: `its` (globally installed). Install/update from <https://github.com/sling86/its-releases>.

## Invocation

```
its <provider> <resource> [action] [positional_args...] [--flags]
```

- Default action is `list` when omitted.
- Every command supports `--ai` for machine-readable JSON.
- Help at every level: `its --help` · `its <provider>` · `its <provider> <resource> help`.

## Always use `--ai` when processing output

When you intend to parse or reason about the result, **always** pass `--ai` — output becomes minified single-line JSON. Only omit it when the user explicitly wants the human-formatted table.

## Global commands (cross-provider)

```bash
its status                      # Health check — every provider, configured/reachable
its config                      # Show configuration — env vars (masked), secrets, sessions
its digest                      # Morning snapshot — counts + warnings across providers
its find <query>                # Cross-provider text search (users, devices, tickets, files)
its health <hostname|username>  # Cross-provider device/user health
its audit <user|UPN>            # Security audit — MFA, sign-ins, risk, endpoint, compliance
its inventory [--unifi]         # Device gap matrix across endpoint sources
its onboard preview <email>     # Starter readiness check (read-only)
its auth login                  # OAuth sign-in (browser PKCE; --device-code for headless)
its auth status                 # Inspect delegated identity + token cache
its auth doctor                 # Full diagnostic — env + delegated + per-resource swap
its auth scopes [provider]      # Required Delegated + Application Graph scopes per provider
its watch <provider> <resource> # Re-run on an interval with change highlighting
its diff <provider> <resource> <snapshot.json>   # Diff live vs saved JSON
its export [--data]             # Export config (+ optionally live data)
```

## Global output flags (work on every command)

| Flag                | Purpose                                                                 |
| ------------------- | ---------------------------------------------------------------------- |
| `--ai`              | Minified JSON output (use this when parsing)                            |
| `--json`            | Pretty-printed JSON                                                     |
| `--csv` / `--tsv`   | Delimited output                                                        |
| `--sort <column>`   | Sort by column (case-insensitive, partial names OK)                     |
| `--order asc\|desc` | Sort direction (default asc)                                            |
| `--filter col=val`  | Filter rows — case-insensitive substring, comma for OR                  |
| `--fields a,b,c`    | Select columns — works in all output modes, partial names OK           |
| `--limit N`         | Cap output to N rows (server-side where supported)                      |
| `--count`           | Return only the row count                                              |
| `--no-cache`        | Bypass cache (force fresh data)                                         |
| `--stdin`           | Fan a command out over a piped JSON array (one call per row)            |
| `--dry-run`         | Preview mutations — print method + URL + body, skip send               |
| `--include-secrets` | Disable global secret redaction (audit-logged — see Safety)            |
| `--auth <mode>`     | OAuth mode for Graph providers — `auto` (default; delegated→app), `delegated`, `app` |

### How `--filter` works

- **Syntax:** `--filter column=value`
- **Column:** case-insensitive, partial names work (`company` matches `companyName`).
- **Value:** case-insensitive **substring** match.
- **Multiple values:** comma-separated = OR (`--filter status=online,overdue`).

### How `--fields` works

- **Syntax:** `--fields col1,col2,col3`
- Case-insensitive, partial names work; filters table columns AND JSON object keys.
- Example: `its exo domains --fields domain,type --json` returns only those two keys.

## Providers

For a command you can already name, prefer **live help** — `its <provider> <resource> help` (always current, near-zero cost). Read the bundled reference (`reference/<alias>.md`, start at [`reference/index.md`](./reference/index.md)) to **discover** what a provider offers — but the big providers' files are large, so switch to live help once you know the command.

| Provider | Alias | What it covers | Reference |
| -------- | ----- | -------------- | --------- |
| Tactical RMM | `rmm` | Endpoint agents — status, processes, services, updates, software, remote run | [rmm](./reference/rmm.md) |
| Entra ID | `entra` | Users, groups, licences, roles, MFA, sign-ins, CA policies, on/offboarding | [entra](./reference/entra.md) |
| Intune | `intune` | Managed devices, apps, scripts, remediations, compliance, ESP, Autopilot | [intune](./reference/intune.md) |
| Exchange Online | `exo` | Distribution groups, mailboxes, permissions, forwarding, rules, message trace | [exo](./reference/exo.md) |
| SharePoint | `sp` | Sites, drives, files, lists, permissions, search | [sp](./reference/sp.md) |
| Outlook | `outlook` | Signed-in user's mail, calendar, contacts (delegated) | [outlook](./reference/outlook.md) |
| Dokploy | `dokploy` | Apps, projects, databases, domains, env vars, deployments | [dokploy](./reference/dokploy.md) |
| Bitwarden | `bw` | Vault search, item/password/TOTP retrieval, password audits | [bw](./reference/bw.md) |
| UniFi Network | `unifi` | Devices, clients, WLANs, firewall, PoE, alarms, guests | [unifi](./reference/unifi.md) |
| UniFi Protect | `protect` | Cameras, NVR storage, motion events | [protect](./reference/protect.md) |
| Wrike | `wrike` | Tickets, tasks, projects, contacts | [wrike](./reference/wrike.md) |
| Azure | `az` | Subscriptions, VMs, resources, Key Vault, storage, costs | [az](./reference/az.md) |
| Cloudflare | `cf` | Zones, DNS, tunnels | [cf](./reference/cf.md) |
| Power BI | `pbi` | Workspaces, reports, datasets (tenant + delegated `my`) | [pbi](./reference/pbi.md) |
| Power Platform | `pa` | Environments, apps, flows | [pa](./reference/pa.md) |
| PeopleHR | `hr` | Employee directory, starters, leavers | [hr](./reference/hr.md) |
| Business Central | `bc` | Companies, OData entity queries | [bc](./reference/bc.md) |
| GitHub | `gh` | Branch protection, webhooks (via local `gh`) | [gh](./reference/gh.md) |
| Docs UI | `docs` | Browser-based command explorer | [docs](./reference/docs.md) |

**Microsoft providers share one Entra app registration.** Entra, Intune, SharePoint, Power BI and Business Central reuse `TENANT_ID` / `CLIENT_ID` / `CLIENT_SECRET`. Set up once, several providers light up.

## Power queries — combine flags

```bash
# Top 5 offline agents, most recently seen first
its rmm agents --filter status=offline --sort last_seen --order desc --limit 5 --ai

# Count of online agents
its rmm agents --filter status=online --count

# All servers as CSV
its rmm agents --filter type=server --csv

# Distribution group names + emails only
its exo groups --fields displayName,primarySmtpAddress --ai
```

## Cross-command pipelines (`--stdin`)

Fan a command out over a piped JSON array. The downstream command reads stdin, extracts the first usable identifier per row (id, agent_id, upn, hostname, mac, email, name) and runs once per row.

```bash
# Ping every overdue agent
its rmm agents --filter status=overdue --ai | its rmm agents ping --stdin

# Full details for every user in a department
its entra users --filter dept=IT --ai | its entra users get --stdin --ai
```

stdout of the runner is a summary + per-row results + per-row failures; exit code is non-zero if any row failed.

## Prefer `its` over the bare vendor CLIs

`its` wraps several official CLIs — prefer the wrapper:

| Bare CLI | Use instead | Why |
| -------- | ----------- | --- |
| `dokploy app list` / `app deploy` | `its dokploy apps` / `apps deploy` | Name resolution, unified output, caching |
| `bw list items` / `bw get` | `its bw items` / `its bw items get` | Speaks the vault REST API directly — no `bw` binary dependency, PIN-encrypted session, 2FA device-remember, TOTP, audits |
| `az vm list` / `az account` | `its az vm` / `its az account` | Same flags/filters/exports as every other provider |
| `az ad app permission grant --scope X` | `its entra consent add-scope <appId> "X"` | `az` **replaces** the whole scope list (destructive); `its consent` does a read-modify-write UNION |
| `az rest --url https://graph.microsoft.com/...` | `its entra graph get /...` (or `intune graph`, `sp graph`) | Uses the provider's Graph token with the correct scopes |
| `Get-Mailbox` / `Get-TransportRule` | `its exo mailboxes` / `its exo rules` | No manual Connect-ExchangeOnline; same output pipeline |

If `its` exposes a command for what you want, use it. Only reach for the bare CLI if `its` genuinely doesn't cover it.

## Safety rules

- **Destructive actions** (delete, remove, disable, block, reboot, purge) — confirm with the user before executing. Most accept `--confirm`; preview mutations with `--dry-run` first.
- **Secret redaction is on by default.** Any key that looks like a credential (`password`, `clientSecret`, `privateKey`, `refresh_token`, `apiKey`, `token`, `secret`, …) and any value shaped like a PEM key or JWT is replaced with `***REDACTED***` in output.
- Opt out with `--include-secrets` only when you genuinely need the raw value (e.g. a BitLocker recovery key for a handover). **Every use is audit-logged** to `~/.its/audit.log`. Never paste `--include-secrets` output into chat, tickets, or AI tools.
- Use `its <provider> setup --check` if unsure whether a provider is configured.
