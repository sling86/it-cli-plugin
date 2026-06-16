# Intune (`intune`)

Microsoft Intune device management — managed devices, apps, platform scripts, remediations, compliance policies, ESP, Autopilot.

> Auto-generated reference. Configure: `its intune setup`. For a command you can name, prefer live help `its intune <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## devices

### `its intune devices`
List Intune-managed devices. Surfaces the most common fields; pass --json for raw shape.
Flags: `--top` Number of results (default 50, paginates automatically) · `--all` Fetch all results (overrides --top)
```bash
its intune devices
its intune devices --all
its intune devices --filter compliance=noncompliant
```

### `its intune devices get <id>`
Get managed device details. Pass the id (or any natural identifier) as the positional arg.
```bash
its intune devices get <serial>
its intune devices get <serial> --json
```

### `its intune devices search <query>`
Search devices by name, user, or serial number. Substring match across the most relevant fields; case-insensitive.
```bash
its intune devices search "jane"
its intune devices search "jane" --json
```

### `its intune devices sync <id>`
Trigger a device sync. Force the device to sync with Intune.
```bash
its intune devices sync <device-id>
its intune devices sync <device-id> --json
```

### `its intune devices noncompliant`
List devices failing compliance. Returns devices failing compliance checks.
```bash
its intune devices noncompliant
its intune devices noncompliant --json
```

## compliance

### `its intune compliance why <device>`
Explain why a device is non-compliant — lists every failing compliance setting (policy, setting, state, error). Resolve the device by id or name.
Flags: `--all` Show every reported setting, not just the failing ones
```bash
its intune compliance why LAPTOP-042
its intune compliance why 12345678-90ab-cdef-1234-567890abcdef
its intune compliance why LAPTOP-042 --all
```

## apps

### `its intune apps`
List managed apps. Surfaces the most common fields; pass --json for raw shape.
Flags: `--top` Number of results (default 50, paginates automatically) · `--all` Fetch all results (overrides --top) · `--with-assignments` Include assignment target group IDs inline (one extra column)
```bash
its intune apps
its intune apps --json
its intune apps --watch
```

### `its intune apps get <id>`
Get app details and assignments. Pass the id (or any natural identifier) as the positional arg.
```bash
its intune apps get <app-id>
its intune apps get <app-id> --json
```

### `its intune apps required`
List apps with required assignments (blocks ESP). Returns apps required by Intune policy.
```bash
its intune apps required
its intune apps required --json
```

## scripts

### `its intune scripts`
List platform scripts. Surfaces the most common fields; pass --json for raw shape.
Flags: `--top` Number of results (default 50, paginates automatically) · `--all` Fetch all results (overrides --top)
```bash
its intune scripts
its intune scripts --json
its intune scripts --watch
```

### `its intune scripts get <id>`
Get platform script details and content. Pass the id (or any natural identifier) as the positional arg.
```bash
its intune scripts get <script-id>
its intune scripts get <script-id> --json
```

### `its intune scripts status <id>`
Get script run status per device. Returns current state plus any pending operations.
```bash
its intune scripts status <script-id>
its intune scripts status <script-id> --json
its intune scripts status <script-id> --watch
```

## remediations

### `its intune remediations`
List proactive remediation scripts. Surfaces the most common fields; pass --json for raw shape.
Flags: `--top` Number of results (default 50, paginates automatically) · `--all` Fetch all results (overrides --top)
```bash
its intune remediations
its intune remediations --json
its intune remediations --watch
```

### `its intune remediations get <id>`
Get remediation script details. Pass the id (or any natural identifier) as the positional arg.
```bash
its intune remediations get <id>
its intune remediations get <id> --json
```

### `its intune remediations status <id>`
Get remediation run status per device. Returns current state plus any pending operations.
```bash
its intune remediations status <id>
its intune remediations status <id> --json
its intune remediations status <id> --watch
```

## policies

### `its intune policies`
List device compliance policies. Surfaces the most common fields; pass --json for raw shape.
Flags: `--top` Number of results (default 50, paginates automatically) · `--all` Fetch all results (overrides --top) · `--with-assignments` Include assignment target group IDs inline (one extra column)
```bash
its intune policies
its intune policies --json
its intune policies --watch
```

### `its intune policies get <id>`
Get compliance policy details. Pass the id (or any natural identifier) as the positional arg.
```bash
its intune policies get <policy-id>
its intune policies get <policy-id> --json
```

### `its intune policies configs`
List device configuration profiles. Configuration profiles applied to the device.
Flags: `--top` Number of results (default 50, paginates automatically) · `--all` Fetch all results (overrides --top) · `--with-assignments` Include assignment target group IDs inline (one extra column)
```bash
its intune policies configs
its intune policies configs --json
```

## esp

### `its intune esp`
List Enrollment Status Page profiles. Surfaces the most common fields; pass --json for raw shape.
```bash
its intune esp
its intune esp --json
its intune esp --watch
```

### `its intune esp get <id>`
Get ESP profile details and tracked apps. Pass the id (or any natural identifier) as the positional arg.
```bash
its intune esp get <profile-id>
its intune esp get <profile-id> --json
```

### `its intune esp update [id]`
Update ESP profile settings (timeout, tracked apps). PATCH semantics — only the supplied fields change.
Flags: `--timeout` Timeout in minutes · `--track-app` Add app ID to tracked apps · `--untrack-app` Remove app ID from tracked apps · `--show-progress` Show installation progress (true/false) · `--allow-use-on-failure` Allow device use on failure (true/false)
```bash
its intune esp update <profile-id> --timeout 120
its intune esp update <profile-id> --timeout 120 --json
```

## autopilot

### `its intune autopilot`
List Autopilot deployment profiles. Surfaces the most common fields; pass --json for raw shape.
```bash
its intune autopilot
its intune autopilot --json
its intune autopilot --watch
```

### `its intune autopilot devices`
List Autopilot-registered devices. Returns devices for the resource.
Flags: `--top` Number of results (default 50, paginates automatically) · `--all` Fetch all results (overrides --top)
```bash
its intune autopilot devices
its intune autopilot devices --json
```

### `its intune autopilot tag <serial> [tag]`
Set group tag on an Autopilot device. Set or clear a tag value.
Flags: `--clear` Remove the group tag
```bash
its intune autopilot tag <serial> --tag "Office-Standard"
its intune autopilot tag <serial> --tag "Office-Standard" --json
```

## group

### `its intune group find <groupId>`
Reverse lookup — list every Intune resource assigned to a group.
```bash
its intune group find "All Devices"
its intune group find "All Devices" --json
```

## settings

### `its intune settings`
List Settings Catalog policies (the modern Intune configuration surface).
Flags: `--top` Number of results (default 50) · `--all` Fetch all results (overrides --top, paginates up to 1000) · `--with-assignments` Expand assignment target group IDs inline
```bash
its intune settings
its intune settings --json
its intune settings --watch
```

### `its intune settings get <id>`
Get a Settings Catalog policy with assignments expanded.
```bash
its intune settings get <policy-id>
its intune settings get <policy-id> --json
```

## intents

### `its intune intents`
List Endpoint Security policy intents (firewall, ASR, BitLocker, etc.).
Flags: `--top` Number of results (default 50) · `--all` Fetch all results (overrides --top, paginates up to 1000) · `--with-assignments` Expand assignment target group IDs inline
```bash
its intune intents
its intune intents --json
its intune intents --watch
```

### `its intune intents get <id>`
Get an Endpoint Security intent with assignments expanded.
```bash
its intune intents get <intent-id>
its intune intents get <intent-id> --json
```

## updates

### `its intune updates`
List Windows Update profiles. --type feature|quality|driver (default feature).
Flags: `--type` Profile category · `--top` Number of results (default 50) · `--all` Fetch all results (overrides --top, paginates up to 1000) · `--with-assignments` Expand assignment target group IDs inline
```bash
its intune updates
its intune updates --json
its intune updates --watch
```

### `its intune updates get <id>`
Get a Windows Update profile by id (auto-detects type — pass --type to disambiguate).
Flags: `--type` Profile category
```bash
its intune updates get <ring-id>
its intune updates get <ring-id> --json
```

## appconfig

### `its intune appconfig`
List mobile app configuration policies (per-app key/value config).
Flags: `--top` Number of results (default 50) · `--all` Fetch all results (overrides --top, paginates up to 1000) · `--with-assignments` Expand assignment target group IDs inline
```bash
its intune appconfig
its intune appconfig --json
its intune appconfig --watch
```

### `its intune appconfig get <id>`
Get an app configuration policy with assignments expanded.
```bash
its intune appconfig get <policy-id>
its intune appconfig get <policy-id> --json
```

## appprotection

### `its intune appprotection`
List App Protection (MAM) policies. --platform ios|android (default ios).
Flags: `--platform` Mobile OS · `--top` Number of results (default 50) · `--all` Fetch all results (overrides --top, paginates up to 1000) · `--with-assignments` Expand assignment target group IDs inline
```bash
its intune appprotection --platform ios
its intune appprotection --platform android
its intune appprotection --platform ios --watch
```

### `its intune appprotection get <id>`
Get an App Protection policy by id. Pass the id (or any natural identifier) as the positional arg.
Flags: `--platform` Mobile OS
```bash
its intune appprotection get <policy-id>
its intune appprotection get <policy-id> --json
```

## doctor

### `its intune doctor`
Intune health snapshot — non-compliant devices, sync staleness, unencrypted endpoints, autopilot pending count.
Flags: `--stale-hours` Threshold for device sync staleness (default 48) · `--top` Maximum devices to scan (default 999)
```bash
its intune doctor
its intune doctor --json
its intune doctor --watch
```

## graph

### `its intune graph get <path>`
Raw Graph GET — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal) · `--raw` Return the response body as raw bytes (no JSON decode). Required for binary endpoints like /content. Currently honoured by the `sp` provider. · `--out` Write the response to this file path instead of stdout. Implies --raw.
```bash
its intune graph get "/deviceManagement/managedDevices?$top=5"
its intune graph get "/deviceManagement/managedDevices?$top=5" --json
```

### `its intune graph post <path>`
Raw Graph POST — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--body` Request body — inline JSON string or @file.json to read from disk · `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its intune graph post "/deviceManagement/managedDevices/<id>/syncDevice"
its intune graph post "/deviceManagement/managedDevices/<id>/syncDevice" --json
```

### `its intune graph patch <path>`
Raw Graph PATCH — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--body` Request body — inline JSON string or @file.json to read from disk · `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its intune graph patch "/deviceManagement/deviceCompliancePolicies/<id>" --body @./patch.json
its intune graph patch "/deviceManagement/deviceCompliancePolicies/<id>" --body @./patch.json --json
```

### `its intune graph put <path>`
Raw Graph PUT — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--body` Request body — inline JSON string or @file.json to read from disk · `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its intune graph put "/deviceManagement/managedDevices/<id>" --body @./body.json
its intune graph put "/deviceManagement/managedDevices/<id>" --body @./body.json --json
```

### `its intune graph delete <path>`
Raw Graph DELETE — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its intune graph delete "/deviceManagement/managedDevices/<id>" --confirm
```
