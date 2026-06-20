# Tactical RMM (`rmm`)

Tactical RMM endpoint management — agents, alerts, software, services, updates, scripts, checks, tasks, policies.

> Auto-generated reference. Configure: `its rmm setup`. For a command you can name, prefer live help `its rmm <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## agents

### `its rmm agents`
List all RMM agents with status, hostname, OS, site. Surfaces the most common fields; pass --json for raw shape.
Flags: `--status` Filter by status · `--type` Filter by type · `--client` Filter by client name · `--site` Filter by site name · `--rebooted-since` Only agents whose last boot is more recent than this — ISO timestamp or relative span (7d, 24h, 30m)
```bash
its rmm agents
its rmm agents --status offline
its rmm agents --site "head-office"
its rmm agents --rebooted-since 24h
its rmm agents --rebooted-since 2026-06-14
its rmm agents --json
its rmm agents --ai | ai "which agents are overdue?"
```

### `its rmm agents stale`
List agents overdue for 7+ days (stale/abandoned). Sorted oldest-first so the worst cases surface first. Use --days to tune the threshold.
Flags: `--days` Minimum days since last seen (default 7)
```bash
its rmm agents stale
its rmm agents stale --days 30
its rmm agents stale --json
```

### `its rmm agents search <query>`
Fuzzy substring match across hostname, client, site, and logged-in username. Case-insensitive — useful when you only know part of a name.
```bash
its rmm agents search "OFFICE-PC"
its rmm agents search "jane.smith"
```

### `its rmm agents get <agent>`
Full agent detail — hardware, OS, last-seen, public IP, disks, version, maintenance state. Accepts hostname, username, or agent UUID.
```bash
its rmm agents get OFFICE-PC-01
its rmm agents get jane.smith
its rmm agents get 12345678-1234-1234-1234-123456789abc
```

### `its rmm agents ping <agent>`
Live connectivity check — synchronously waits for the agent to respond. Returns last-seen + current online state.
```bash
its rmm agents ping OFFICE-PC-01
its rmm agents ping aaron.lock
```

### `its rmm agents reboot <agent>`
Reboot the agent's host OS. Destructive — needs --confirm. Returns immediately; the host may take 1-3 minutes to come back.
Flags: `--confirm` Confirm the reboot
```bash
its rmm agents reboot OFFICE-PC-01 --confirm
its rmm agents reboot c3e5f1a7-... --confirm
```

### `its rmm agents remove <agent>`
Permanently delete the agent record from RMM. Doesn't uninstall the local agent service — pair with the RMM uninstall script if the device is being decommissioned.
Flags: `--confirm` Confirm the removal
```bash
its rmm agents remove OFFICE-PC-01 --confirm
```

### `its rmm agents prune`
Bulk-remove stale agent records last seen before a threshold (and never-seen agents). Destructive — previews the target list by default and needs --confirm to delete. Doesn't uninstall the local agent service. Pair with `agents stale` to inspect first.
Flags: `--older-than` Age threshold — days (30) or a span (30d, 12h, 2w). Agents last seen before this are pruned. · `--confirm` Actually delete the matched agent records
```bash
its rmm agents prune --older-than 90d
its rmm agents prune --older-than 90d --confirm
its rmm agents prune --older-than 6w --confirm
```

### `its rmm agents run <agent>`
Execute a one-shot shell command on the target agent. Returns stdout + stderr + exit code. Use --shell powershell|cmd|bash; default timeout is 90s.
Flags: `--shell` Shell type · `--cmd` Command to execute (use --file for multi-line scripts) · `--file` Path to a local script file (overrides --cmd). PowerShell multi-line scripts auto-wrapped via base64 + temp file. · `--raw` Disable auto-wrapping for multi-line PowerShell (sends as-is) · `--timeout` Timeout in seconds
```bash
its rmm agents run OFFICE-PC-01 --shell powershell --cmd "Get-Process"
its rmm agents run OFFICE-PC-01 --shell cmd --cmd "ipconfig /all"
its rmm agents run LINUX-WEB-01 --shell bash --cmd "uname -a && uptime"
```

### `its rmm agents history <agent>`
Command + script execution history for one agent. Shows time, type, exit status, who ran it, and the truncated payload.
```bash
its rmm agents history OFFICE-PC-01
its rmm agents history OFFICE-PC-01 --days 7
```

### `its rmm agents notes <agent>`
Free-form notes attached to the agent — common workflow is documenting incident actions or device peculiarities.
```bash
its rmm agents notes OFFICE-PC-01
its rmm agents notes OFFICE-PC-01 --json
```

### `its rmm agents pending <agent>`
Queued reboots, script runs, and update installs that the agent hasn't picked up yet. Useful when a command seems to have stalled.
```bash
its rmm agents pending OFFICE-PC-01
its rmm agents pending OFFICE-PC-01 --json
```

### `its rmm agents wake <agent>`
Send a magic packet via another online agent on the same LAN. Doesn't work across VLANs / VPN — agent must be on a network with a reachable RMM peer.
```bash
its rmm agents wake OFFICE-PC-01
its rmm agents wake OFFICE-PC-01 --site "Head Office"
```

### `its rmm agents edit <agent>`
Reassign an agent's client/site or update description (PUT /agents/{id}/).
Flags: `--client` Client name or ID to move the agent into · `--site` Site name or ID to move the agent into (must belong to --client if both given) · `--description` New description text
```bash
its rmm agents edit OFFICE-PC-01 --site <site-id>
its rmm agents edit OFFICE-PC-01 --description "Marketing — Jane's desk"
```

### `its rmm agents refresh <agent>`
Trigger a WMI/sysinfo refresh on an agent — repopulates hardware fields (serial, model, etc.).
```bash
its rmm agents refresh OFFICE-PC-01
its rmm agents refresh OFFICE-PC-01 --json
```

## dashboard

### `its rmm dashboard`
Single-screen fleet health view — online/offline/overdue counts, server vs workstation, maintenance mode, reboot-pending, failing checks. Cheap to call; suitable for --watch.
```bash
its rmm dashboard
its rmm dashboard --json
its rmm dashboard
its rmm dashboard --watch
its rmm dashboard --watch
```

## clients

### `its rmm clients`
Top-level RMM client list with each client's sites flattened into one row. Use IDs from here as --client filter values elsewhere.
```bash
its rmm clients
its rmm clients --json
its rmm clients --watch
```

### `its rmm clients create`
Create a client (POST /clients/). TRMM also creates its first site in the same call — pass --site for its name (default 'Default'). Idempotent — skips if a client of that name already exists.
Flags: `--name` New client name · `--site` Name of the client's initial site (default 'Default')

### `its rmm clients delete <client>`
Delete a client by ID or name (DELETE /clients/{id}/). --confirm required. If the client still has agents, pass --move-to-site <siteId> to reassign them first (TRMM refuses otherwise).
Flags: `--confirm` Confirm deletion · `--move-to-site` Site ID to move this client's agents to before deleting

## sites

### `its rmm sites`
All RMM sites across every client, with per-site agent_count. Site IDs feed --site filters on agent commands.
```bash
its rmm sites
its rmm sites --client "Head Office"
its rmm sites --watch
```

### `its rmm sites create`
Create a site under a client (POST /clients/sites/). Idempotent — skips if a site of that name already exists for the client.
Flags: `--client` Client name or ID · `--name` New site name

### `its rmm sites delete <site_id>`
Delete a site by ID (DELETE /clients/sites/{id}/). Fails if the site still has agents. --confirm required.
Flags: `--confirm` Confirm deletion

## processes

### `its rmm processes <agent>`
Live process snapshot from the agent — sorted by CPU%, top 50. Use `processes top` instead when you only want the heavy hitters.
```bash
its rmm processes OFFICE-PC-01
its rmm processes OFFICE-PC-01 --name "chrome"
its rmm processes OFFICE-PC-01 --watch
```

### `its rmm processes top <agent>`
Top processes by CPU usage (live snapshot via PowerShell).
Flags: `--count` Number of processes to show
```bash
its rmm processes top OFFICE-PC-01
its rmm processes top OFFICE-PC-01 --json
```

### `its rmm processes kill <agent_id>`
Terminate a process by PID. Destructive — needs --confirm. Use `processes list` first to find the PID.
Flags: `--pid` Process ID to kill · `--confirm` Confirm the kill
```bash
its rmm processes kill OFFICE-PC-01 --pid 1234 --confirm
its rmm processes kill OFFICE-PC-01 --name "notepad" --confirm
```

## services

### `its rmm services <agent>`
Windows / Linux service inventory for one agent. Filter with --status running|stopped|paused.
```bash
its rmm services OFFICE-PC-01
its rmm services OFFICE-PC-01 --status running
its rmm services OFFICE-PC-01 --status stopped
```

### `its rmm services get <agent>`
Service detail — startup type, dependencies, current state, last exit code.
Flags: `--name` Service name
```bash
its rmm services get OFFICE-PC-01 --service spooler
its rmm services get OFFICE-PC-01 --service spooler --json
```

### `its rmm services control <agent>`
Control a service (start/stop/restart). Stop requires --confirm.
Flags: `--name` Service name · `--action` Action to perform · `--confirm` Required for stop action
```bash
its rmm services control OFFICE-PC-01 --service spooler --action restart
its rmm services control OFFICE-PC-01 --service spooler --action start
```

### `its rmm services enable <agent>`
Set startup type to Automatic and start the service if stopped. Idempotent.
Flags: `--name` Service name
```bash
its rmm services enable OFFICE-PC-01 --service spooler
its rmm services enable OFFICE-PC-01 --service spooler --json
```

### `its rmm services disable <agent>`
Set a service startup type to Disabled (requires --confirm).
Flags: `--name` Service name · `--confirm` Confirm the disable
```bash
its rmm services disable OFFICE-PC-01 --service spooler --confirm
its rmm services disable OFFICE-PC-01 --service spooler --confirm --json
```

## updates

### `its rmm updates report`
Fleet Windows-Update compliance — fans out across agents and lists those with pending updates, most-critical-then-most-behind first, with separate critical/important columns. Narrow with --client/--site; --all also lists fully-patched agents. Pipe --csv/--json to export. One API call per agent, so it's slower than a single-agent query.
Flags: `--client` Only agents in this client (substring) · `--site` Only agents in this site (substring) · `--all` Include fully-patched agents in the table
```bash
its rmm updates report
its rmm updates report --client "Candle Retail"
its rmm updates report --all
```

### `its rmm updates <agent>`
Windows Update queue — pending, installed, failed. Shows KB, severity, install state, and the per-KB approval action (approve/ignore/nothing) — only `approve` updates get installed.
```bash
its rmm updates OFFICE-PC-01
its rmm updates OFFICE-PC-01 --json
its rmm updates OFFICE-PC-01 --watch
```

### `its rmm updates scan [agent]`
Force a re-scan of Microsoft Update. Single agent, or fan out across a client/site/all-online to refresh the pending list before an `updates report`.
Flags: `--all-online` Fan out to every online agent · `--client` Fan out to online agents in this client (substring) · `--site` Fan out to online agents in this site (substring) · `--policy` Fan out to online agents under this automation policy id · `--confirm` Required to execute a fan-out (preview without it)
```bash
its rmm updates scan OFFICE-PC
its rmm updates scan --client "Candle Retail"
its rmm updates scan --client "Candle Retail" --confirm
its rmm updates scan OFFICE-PC-01
its rmm updates scan OFFICE-PC-01 --json
```

### `its rmm updates install [agent]`
Install APPROVED pending Windows updates (TRMM installs only updates with action=approve — approve them first with `its rmm updates approve`). Single agent, or fan out across a client/site/all-online — both require --confirm (fan-out previews the targets without it). Single-agent install no-ops with a hint if nothing is approved. Reboot-after-install is controlled by the agent's patch policy (see `its rmm policies patch-policy`), not this command.
Flags: `--all-online` Fan out to every online agent · `--client` Fan out to online agents in this client (substring) · `--site` Fan out to online agents in this site (substring) · `--policy` Fan out to online agents under this automation policy id · `--confirm` Required to execute a fan-out (preview without it)
```bash
its rmm updates install OFFICE-PC --confirm
its rmm updates install --client "Candle Retail"
its rmm updates install --client "Candle Retail" --confirm
its rmm updates install OFFICE-PC-01 --confirm
its rmm updates install --client "Candle Retail"
its rmm updates install --client "Candle Retail" --confirm
```

### `its rmm updates approve <agent>`
Approve pending Windows updates so `updates install` will install them — TRMM installs ONLY approved updates, so an unapproved box is a no-op install. Target one --kb KB5034441 or --all-pending (the latter needs --confirm).
Flags: `--kb` KB to act on (e.g. KB5034441 or 5034441) · `--all-pending` Act on every not-yet-installed update · `--confirm` Required for --all-pending
```bash
its rmm updates approve OFFICE-PC --kb KB5034441
its rmm updates approve OFFICE-PC --all-pending --confirm
```

### `its rmm updates defer <agent>`
Defer (ignore) pending Windows updates so install skips them. Target one --kb KB5034441 or --all-pending (the latter needs --confirm). Re-approve later with `updates approve`.
Flags: `--kb` KB to act on (e.g. KB5034441 or 5034441) · `--all-pending` Act on every not-yet-installed update · `--confirm` Required for --all-pending
```bash
its rmm updates defer OFFICE-PC --kb KB5034441
its rmm updates defer OFFICE-PC --all-pending --confirm
```

## software

### `its rmm software <agent>`
Full installed-software inventory for one agent. Includes publisher, version, install date. Useful for compliance reports.
```bash
its rmm software OFFICE-PC-01
its rmm software OFFICE-PC-01 --json
its rmm software OFFICE-PC-01 --watch
```

### `its rmm software search <query>`
Fleet-wide title hunt — given a publisher or product name, find every agent that has it installed. Useful for incident response (e.g. find every box running a vulnerable version).
```bash
its rmm software search "chrome"
its rmm software search "Adobe"
```

## alerts

### `its rmm alerts`
Active and resolved alerts across the fleet. Filter by --severity info|warning|error or --resolved.
Flags: `--severity` Filter by severity (info/warning/error) · `--resolved` Show resolved alerts
```bash
its rmm alerts --status active
its rmm alerts --client "Head Office"
its rmm alerts --severity error
```

### `its rmm alerts get <alert_id>`
Alert detail — full message, source check, snooze state, agent it fired on.
```bash
its rmm alerts get <alert-id>
its rmm alerts get <alert-id> --json
```

## scripts

### `its rmm scripts`
All saved automation scripts in the RMM script library. Returns name, shell, category, default timeout.
```bash
its rmm scripts
its rmm scripts --json
its rmm scripts --watch
```

### `its rmm scripts get <script_id>`
Script detail — body, default args, category, hash, last edited timestamp.
```bash
its rmm scripts get <script-id>
its rmm scripts get <script-id> --json
```

### `its rmm scripts run [agent]`
Execute a saved RMM script on a target agent, or fan it out across a fleet with --all-online/--client/--site/--policy (online agents only). Streams stdout/stderr back; use --timeout for long-running jobs (default 120s). Fan-out previews the target list and needs --confirm to execute.
Flags: `--script` Script ID · `--args` Script arguments (comma-separated) · `--timeout` Timeout in seconds · `--raw` Print the script's raw stdout (and stderr) directly, instead of JSON-wrapped output with escaped \r\n (single-agent only) · `--all-online` Fan out to every online agent · `--client` Fan out to online agents in this client (substring) · `--site` Fan out to online agents in this site (substring) · `--policy` Fan out to online agents under this automation policy id · `--confirm` Required to execute a fan-out run (preview without it)
```bash
its rmm scripts run OFFICE-PC --script 12
its rmm scripts run --all-online --script 12
its rmm scripts run --all-online --script 12 --confirm
its rmm scripts run --client "Candle Retail" --script 12 --confirm
its rmm scripts run --site "head-office" --script 12 --confirm
its rmm scripts run OFFICE-PC-01 --script "Restart Print Spooler"
its rmm scripts run OFFICE-PC-01 --script "Long Audit" --timeout 600
```

### `its rmm scripts upload-local <agent> <path>`
Upload a local .ps1/.sh/.py script to TRMM, run it on the agent, capture output, and delete the script afterwards. Use for ad-hoc D:/it-scripts/... invocations (Fix-DymoPrinter, Invoke-CorruptionCheck, Get-ShutdownDiagnostics). Pass --keep to leave the script registered.
Flags: `--shell` Script shell (defaults to auto-detect from extension: ps1=powershell, sh=shell, py=python, nu=nushell, ts=deno) · `--args` Script arguments (comma-separated) · `--timeout` Timeout in seconds (default 120) · `--keep` Leave the uploaded script registered in TRMM after execution · `--category` Category for the uploaded script (default 'Ad-hoc')
```bash
its rmm scripts upload-local ./fix-printers.ps1
its rmm scripts upload-local ./linux-housekeeping.sh --shell bash
```

### `its rmm scripts delete <script_id>`
Permanently remove a script from the library. Destructive — needs --confirm.
Flags: `--confirm` Confirm the deletion
```bash
its rmm scripts delete <script-id> --confirm
```

### `its rmm scripts upsert <name> <path>`
Idempotently push a local script to TRMM by name — creates if missing, updates if present (PUT). Works around the TRMM POST /scripts/ quirk where the response is a plain string, not the created object — we re-list to resolve the new ID.
Flags: `--shell` Shell: powershell, cmd, shell, python, nushell, deno (auto from extension) · `--category` Category (default 'Ad-hoc') · `--description` Script description (overwritten on update) · `--timeout` Default timeout seconds (default 120) · `--args` Default args, comma-separated
```bash
its rmm scripts upsert "Restart Spooler" ./fix-spooler.ps1
its rmm scripts upsert "Restart Spooler" ./fix-spooler.ps1 --json
```

## checks

### `its rmm checks <agent>`
Scheduled health checks attached to one agent. Includes last-run result + cadence.
```bash
its rmm checks OFFICE-PC-01
its rmm checks OFFICE-PC-01 --json
its rmm checks OFFICE-PC-01 --watch
```

### `its rmm checks failing`
Fleet-wide failing-checks sweep — fans out across online agents and surfaces every check currently failing, worst-first. The fast 'what's red across the estate right now' view. Narrow with --client/--site.
Flags: `--client` Only agents in this client (substring) · `--site` Only agents in this site (substring)
```bash
its rmm checks failing
its rmm checks failing --client "Candle Retail"
```

### `its rmm checks results <agent>`
Drill into one check's live result on an agent — status, last run, fail count, return code, and captured stdout/stderr. Pass --check <id> (find it via `its rmm checks <agent>`).
Flags: `--check` Check ID to inspect
```bash
its rmm checks results OFFICE-PC --check 7
```

### `its rmm checks run <agent>`
Force all of an agent's checks to run now (POST /checks/<agent>/run/) instead of waiting for the next scheduled cycle. Returns immediately; fresh results land on the agent's next check-in.
```bash
its rmm checks run OFFICE-PC
```

### `its rmm checks create <agent>`
Attach a check to an agent. Use --type to pick: diskspace/cpuload/memory/ping/winsvc/script (defaults to script when --script is given). Tune alerting with --severity, --fails, --interval.
Flags: `--type` Check type · `--severity` Alert severity (default error) · `--fails` Failures before alert (default 1) · `--interval` Run interval seconds, 0 = inherit (default 0) · `--disk` diskspace: drive (e.g. C:) · `--error` diskspace/cpuload/memory: error threshold % · `--warning` diskspace/cpuload/memory: warning threshold % · `--ip` ping: host/IP to ping · `--service` winsvc: Windows service name · `--restart-if-stopped` winsvc: restart the service if found stopped · `--pass-if-pending` winsvc: pass when start is pending · `--pass-if-missing` winsvc: pass when the service doesn't exist · `--script` script: script ID · `--timeout` script: timeout seconds (default 120) · `--args` script: arguments (comma-separated)
```bash
its rmm checks create OFFICE-PC --type diskspace --disk C: --error 10 --warning 25
its rmm checks create OFFICE-PC --type cpuload --error 90 --warning 75
its rmm checks create SRV-01 --type winsvc --service Spooler --restart-if-stopped
its rmm checks create SRV-01 --type ping --ip 10.10.0.1
its rmm checks create OFFICE-PC --script 42 --timeout 300
its rmm checks create OFFICE-PC-01 --script <script-id> --interval 600
its rmm checks create OFFICE-PC-01 --script <script-id> --interval 600 --json
```

### `its rmm checks edit <agent>`
Retune an existing check without delete+recreate — change interval, severity, fail-count, or thresholds. PUT is partial, so only the flags you pass change.
Flags: `--check` Check ID to edit · `--severity` Alert severity · `--fails` Failures before alert · `--interval` Run interval seconds · `--error` Error threshold % (diskspace/cpuload/memory) · `--warning` Warning threshold % (diskspace/cpuload/memory) · `--timeout` Script check: timeout seconds · `--args` Script check: comma-separated script args (replaces existing)
```bash
its rmm checks edit OFFICE-PC --check 7 --warning 60 --error 80
its rmm checks edit OFFICE-PC --check 7 --severity warning --fails 3
its rmm checks edit OFFICE-PC --check 7 --timeout 300 --args -Verbose,-Force
```

### `its rmm checks delete [agent_id]`
Remove a scheduled check from an agent. Destructive — needs --confirm.
Flags: `--check` Check ID to delete · `--confirm` Confirm the deletion
```bash
its rmm checks delete <check-id> --confirm
```

## tasks

### `its rmm tasks <agent>`
Recurring task schedule attached to one agent — script + interval + next-run.
```bash
its rmm tasks OFFICE-PC-01
its rmm tasks OFFICE-PC-01 --json
its rmm tasks OFFICE-PC-01 --watch
```

### `its rmm tasks create <agent>`
Create an automated task that runs a script on an agent. Default is a manual task (run on demand); add --daily-time HH:MM for a daily schedule (+ --weekdays to limit days). Resolves the agent by id/hostname/username, including offline agents.
Flags: `--script` Script ID to run · `--name` Task name (default: derived from the script ID) · `--args` Script arguments (comma-separated, e.g. -Mode,Notify) · `--timeout` Per-run timeout in seconds (default 90) · `--daily-time` Run daily at HH:MM (24h) — turns this into a scheduled task · `--weekdays` With --daily-time: restrict to these days (mon,tue,wed,thu,fri,sat,sun); default every day · `--run-asap` Run as soon as possible if a scheduled run was missed
```bash
its rmm tasks create <agent-id> --name "Weekly reboot" --script <script-id> --cron "0 3 * * 0"
its rmm tasks create <agent-id> --name "Weekly reboot" --script <script-id> --cron "0 3 * * 0" --json
```

### `its rmm tasks delete`
Remove a scheduled task from an agent. Destructive — needs --confirm.
Flags: `--task` Task ID to delete · `--confirm` Confirm the deletion
```bash
its rmm tasks delete --task <task-id> --confirm
```

## policies

### `its rmm policies`
RMM automation policies — packages of checks + scheduled tasks applied across clients/sites.
```bash
its rmm policies
its rmm policies --json
its rmm policies --watch
```

### `its rmm policies get <policy_id>`
Policy detail — included checks, tasks, target agents/sites.
```bash
its rmm policies get <policy-id>
its rmm policies get <policy-id> --json
```

### `its rmm policies checks <policy_id>`
List checks attached to a policy (uses the asymmetric `/automation/policies/<id>/checks/` GET route
```bash
its rmm policies checks <policy-id>
its rmm policies checks <policy-id> --json
```

### `its rmm policies add-check <policy_id>`
Add a check to a policy (applies to every agent under it). --type: diskspace/cpuload/memory/ping/winsvc/script (defaults to script when --script is given). Uses POST /checks/ with `policy` set and `agent` OMITTED — including agent:null returns 404 because the route resolver hits the agent path first
Flags: `--type` Check type · `--severity` Alert severity (default error; script branch defaults warning) · `--fails` Failures before alert (default 1) · `--interval` Run interval seconds (script defaults 86400 daily, others 0=inherit) · `--disk` diskspace: drive (e.g. C:) · `--error` diskspace/cpuload/memory: error threshold % · `--warning` diskspace/cpuload/memory: warning threshold % · `--ip` ping: host/IP · `--service` winsvc: Windows service name · `--restart-if-stopped` winsvc: restart if stopped · `--pass-if-pending` winsvc: pass when start pending · `--pass-if-missing` winsvc: pass when service absent · `--script` script: Script ID · `--timeout` script: timeout seconds (default 90)
```bash
its rmm policies add-check 4 --type diskspace --disk C: --error 10 --warning 25
its rmm policies add-check 4 --type winsvc --service Spooler --restart-if-stopped
its rmm policies add-check 4 --script 42
its rmm policies add-check <policy-id> --script <script-id>
its rmm policies add-check <policy-id> --script <script-id> --json
```

### `its rmm policies patch-policy <policy_id>`
Edit a policy's Windows Update schedule + per-severity approvals (WinUpdatePolicy). Partial update — only the flags you pass change. --confirm required: applies to EVERY agent under the policy.
Flags: `--run-time-hour` Install hour 0–23 (hour-only — no minutes) · `--frequency` Schedule frequency (daily = daily/weekly + --days) · `--days` Weekdays for daily/weekly schedule, e.g. mon,wed,fri (run_time_days) · `--day-of-month` Day of month 1–31 for monthly schedule (run_time_day) · `--reboot` Reboot after install · `--critical` Critical updates · `--important` Important updates · `--moderate` Moderate updates · `--low` Low updates · `--other` Other updates · `--confirm` Apply — affects every agent under the policy

## diagnostics

### `its rmm diagnostics <agent>`
System health snapshot — CPU, RAM, disk, power plan, uptime, top processes.
```bash
its rmm diagnostics OFFICE-PC-01
its rmm diagnostics OFFICE-PC-01 --json
its rmm diagnostics OFFICE-PC-01 --watch
```

## doctor

### `its rmm doctor`
Tactical RMM health snapshot — agent status, failing checks, active alerts, sites with no online agents.
```bash
its rmm doctor
its rmm doctor --json
its rmm doctor --watch
```

## custom-fields

### `its rmm custom-fields`
List TRMM custom field definitions across all models (agent / client / site).
Flags: `--model` Filter by model

### `its rmm custom-fields set <agent> <field> <value>`
Set a custom field value on an agent. Accepts the field by numeric id OR by exact name (looked up against /core/customfields/). Writes both `value` and the typed column TRMM actually reads.
