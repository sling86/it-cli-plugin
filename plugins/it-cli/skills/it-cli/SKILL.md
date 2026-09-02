---
name: it-cli
description: "Use the `its` CLI to query and manage IT infrastructure from one shell — across endpoint/RMM agents, Entra ID users & groups, Intune devices, Microsoft 365 (Exchange Online, SharePoint, Outlook, service health & message centre), Dokploy deployments, Bitwarden vaults, UniFi network & UniFi Protect CCTV, Wrike tickets, Azure resources, Cloudflare, Power BI, Power Platform, PeopleHR, and Business Central. Reach for `its` for any question about servers, devices, users, licences, passwords, deployments, network, tickets, files, or cloud resources. Prefer the `its` wrapper over the bare `dokploy`, `bw`, `az`, or Exchange Online PowerShell. Trigger on: agent/device health, user lookup, group membership, licence assignment, password/TOTP retrieval, deployment status, firewall rules, ticket updates, mailbox management, security audits, onboarding/offboarding, or any infrastructure management task."
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

## Seeing `***REDACTED***`? Keep the secret out of machine output

Secrets are masked by default — passwords, keys, tokens, TOTP seeds and Bitwarden hidden fields. `--include-secrets` / `--unsafe` is accepted only for interactive human output; JSON/AI modes and redirected stdout reject it. An AI shell therefore cannot retrieve a raw secret through captured command output.

When a human needs a Bitwarden value, ask them to run the clipboard command in their own terminal:

```bash
its bw password "server-login" --copy # clipboard only; auto-clears
```

For providers without a secure destination command, the human may use `--include-secrets` in an interactive terminal. Every use is audit-logged. Never paste the revealed value into chat, a ticket, or an AI transcript.

## Global commands (cross-provider)

```bash
its setup                       # Configuration overview — which providers are set up + the exact setup command for the rest
its status                      # Health check — every provider, configured/reachable (--test probes connectivity)
its config                      # Show configuration — env vars (masked), secrets, sessions
its digest                      # Morning snapshot — counts + warnings across providers
its find <query>                # Cross-provider text search (users, devices, tickets, files)
its health <hostname|username>  # Cross-provider device/user health
its audit <user|UPN>            # Security audit — MFA, sign-ins, risk, endpoint, compliance
its inventory [--unifi]         # Device gap matrix across endpoint sources
its onboard preview <email>     # Starter readiness check (read-only)
its user <upn>                  # Single-user snapshot — Entra + groups + licences + devices + tickets
its resume [--project <p>]      # Open backlog / resume-prompt ctxc memories, grouped by project
its log <text>                  # Append a timestamped entry to today's vault daily note
its auth login                  # OAuth sign-in (browser PKCE; --device-code for headless)
its auth status                 # Inspect delegated identity + token cache
its auth doctor                 # Full diagnostic — env + delegated + per-resource swap
its auth scopes [provider]      # Required Delegated + Application Graph scopes per provider
its secrets                     # Keychain entries (names only, never values); migrate / clear
its trust-cert <url>            # Trust + remember a self-signed TLS cert (TOFU); list / remove <host>
its watch <provider> <resource> # Re-run on an interval with change highlighting
its diff <provider> <resource> <snapshot.json>   # Diff live vs saved JSON
its export [--data]             # Export config (+ optionally live data)
its info                        # Diagnostic snapshot — version, paths, update state, keychain
its tui                         # Interactive browser over every command (needs a TTY)
its update                      # Check for a newer release
```

Full detail for all of these — usage, flags, worked examples — is in
[reference/global.md](reference/global.md), generated from the same source as
the commands themselves.

## Global output flags (work on every command)

| Flag                | Purpose                                                                 |
| ------------------- | ---------------------------------------------------------------------- |
| `--ai`              | Minified JSON output (use this when parsing)                            |
| `--ai-flat`         | Minified JSON without the columnar envelope                            |
| `--json`            | Pretty-printed JSON                                                     |
| `--jsonl`           | NDJSON — one compact JSON record per line                              |
| `--csv` / `--tsv`   | Delimited output                                                        |
| `--sort <column>`   | Sort by column (case-insensitive, partial names OK)                     |
| `--order asc\|desc` | Sort direction (default asc)                                            |
| `--filter col=val`  | Filter rows — case-insensitive substring, comma for OR                  |
| `--fields a,b,c`    | Select columns — works in all output modes, partial names OK           |
| `--limit N`         | Cap output to N rows (server-side where supported)                      |
| `--count`           | Return only the row count                                              |
| `--no-cache`        | Bypass cache (force fresh data)                                         |
| `--stdin`           | Fan out over JSON, NDJSON, scalar, or columnar `--ai` input            |
| `--map arg=.path`   | Explicitly bind an input field to a positional argument; repeatable    |
| `--on-error <mode>` | `stop` (default) or `continue` after a per-record failure              |
| `--max-input N`     | Bound fan-out; required explicitly for irreversible commands           |
| `--dry-run`         | Preview mutations — print method + URL + body, skip send               |
| `--include-secrets` | Human TTY plaintext only; rejected in machine/captured output          |
| `--auth <mode>`     | OAuth mode — `auto`, `delegated`, `app`, or `az`                       |
| `--profile <name>`  | Force a delegated identity for one call                                |

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
| Tactical RMM | `rmm` | Endpoint agents — status, processes, services, updates, software, remote run, live terminal | [rmm](./reference/rmm.md) |
| Entra ID | `entra` | Users, groups, licences, roles, MFA, sign-ins, CA policies, on/offboarding | [entra](./reference/entra.md) |
| Intune | `intune` | Managed devices, apps, scripts, remediations, compliance, ESP, Autopilot | [intune](./reference/intune.md) |
| Exchange Online | `exo` | Distribution groups, mailboxes, permissions, forwarding, rules, message trace | [exo](./reference/exo.md) |
| SharePoint | `sp` | Sites, drives, files, lists, permissions, search | [sp](./reference/sp.md) |
| Outlook | `outlook` | Mail, calendar, contacts — signed-in user (delegated) or any mailbox (`--as <upn>` app-only) | [outlook](./reference/outlook.md) |
| Microsoft Teams | `teams` | Signed-in user's chats, messages, presence (delegated-only) | [teams](./reference/teams.md) |
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
| M365 Service Health | `m365` | Service health, incidents, message centre | [m365](./reference/m365.md) |
| Docs UI | `docs` | Browser-based command explorer | [docs](./reference/docs.md) |

**Microsoft providers share one Entra app registration.** Entra, Intune, SharePoint, Power BI and Business Central reuse `TENANT_ID` / `CLIENT_ID` / `CLIENT_SECRET`. Set up once, several providers light up.

**Auth profiles — run some providers as one identity, others as another.** `its auth login --profile <name>` signs into a named slot (browser picker = pick a different account each time); `its auth use <profile> --default | --provider a,b,c` maps providers to it (stored in `~/.its/auth-map.json`). Resolution: `--profile` flag > per-provider map > default > the unnamed slot. Typical setup: an admin account as `default` for tenant tools (entra/intune/sp), your own user for `teams`/`outlook`. `its auth status` lists every profile + the map. No map set = single-identity behaviour, unchanged.

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

The downstream command accepts a JSON array, one JSON object, NDJSON, a scalar, or the columnar envelope emitted by `--ai`. Each input record must bind at least one positional argument. Command-owned `pipeFrom` metadata is preferred; use repeatable `--map arg=.path` when the input shape differs. The temporary legacy identifier fallback warns when used.

```bash
# Command metadata maps agent_id to the required agent argument
its rmm agents --filter status=overdue --jsonl \
  | its rmm agents ping --stdin --jsonl

# Explicit mapping overrides command metadata when needed
its rmm agents --jsonl \
  | its rmm agents get --stdin --map agent=.agent_id --jsonl

# `-` marks the argument that comes from the piped record. Positionals you type
# fill from the left, so without it the date would land in the taskId slot.
its wrike tickets --filter status=active --jsonl \
  | its wrike tickets set-due - 2026-09-01 --stdin --max-input 50
```

Only the first positional falls back to guessing a field, so a `-` after it needs `--map` or command-owned `pipeFrom`. `-` is only special under `--stdin`.

Commands with no positional arguments reject record fan-out. Mutations validate every binding before the first write and run sequentially. Fan-out defaults to 100 records; irreversible commands require an explicit `--max-input`. Per-record failures go to stderr, successful records go to stdout, and any failure produces a non-zero exit code.

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
- **Secret redaction is on by default.** Any key that looks like a credential (`password`, `clientSecret`, `privateKey`, `refresh_token`, `apiKey`, `token`, `secret`, …) and any value shaped like a PEM key or JWT is replaced with `***REDACTED***` in output. Bitwarden **hidden custom fields** are also masked in every mode, and **live TOTP codes** are masked whenever output isn't an interactive terminal (piped / `--ai` / captured) — so nothing secret lands in an AI transcript by default. `its secrets` only reports `SET`/`NOT SET`, never values.
- `--include-secrets` / `--unsafe` reveals plaintext only in interactive human output and is audit-logged to `~/.its/audit.log`; machine formats and redirected stdout reject it. Never paste revealed output into chat, tickets, or AI tools. Bitwarden `--copy` is the secure human hand-off, but it also requires an interactive terminal.
- To hand a secret to another command, use the file sink, not stdout: `its bw items get <id> --to-file <path>` / `its bw password <q> --to-file <path>` write a 0600 file and print only the byte count (no TTY needed), and `its dokploy env set <app> KEY --from-file <path>` / `--from-stdin` reads one back. Never `KEY=$SECRET` on the command line — argv is world-readable via `/proc/<pid>/cmdline` and lands in shell history.
- Some items keep their secret in a hidden custom field with an empty password (API keys especially). `its bw items get <id> --field "<name>" --to-file <path>` sends that field instead. A wrong name lists the available ones and writes no file.
- **Bitwarden needs an unlocked session before any of that works.** The vault is PIN-locked; the prompt needs a real terminal, so in an AI shell, cron or a pipe every `its bw` command fails until a human runs `its bw session unlock` in their own terminal (8 hours). The failure text suggests `its bw setup`, which cannot unlock anything — ignore it and ask the user to unlock. `its bw setup --check` reports the true state: `Configured but LOCKED` vs a missing configuration.
- Use `its <provider> setup --check` if unsure whether a provider is configured — the line under the checklist says whether it is usable *now*, which is the different and usually more useful question.
