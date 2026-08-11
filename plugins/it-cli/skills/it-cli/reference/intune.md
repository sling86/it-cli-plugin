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
```

### `its intune devices search <query>`
Search devices by name, user, or serial number. Substring match across the most relevant fields; case-insensitive.
```bash
its intune devices search "jane"
```

### `its intune devices sync <id>`
Trigger a device sync. Force the device to sync with Intune.
```bash
its intune devices sync 8f1c2d3e-4a5b-6c7d-8e9f-0a1b2c3d4e5f
its intune devices sync <device-id>
```

### `its intune devices noncompliant`
List devices failing compliance. Returns devices failing compliance checks.
```bash
its intune devices noncompliant
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
its intune apps --watch
```

### `its intune apps get <id>`
Get app details and assignments. Pass the id (or any natural identifier) as the positional arg.
```bash
its intune apps get <app-id>
```

### `its intune apps required`
List apps with required assignments (blocks ESP). Returns apps required by Intune policy.
```bash
its intune apps required
```

## scripts

### `its intune scripts`
List platform scripts. Surfaces the most common fields; pass --json for raw shape.
Flags: `--top` Number of results (default 50, paginates automatically) · `--all` Fetch all results (overrides --top)
```bash
its intune scripts
its intune scripts --watch
```

### `its intune scripts get <id>`
Get platform script details and content. Pass the id (or any natural identifier) as the positional arg.
```bash
its intune scripts get <script-id>
```

### `its intune scripts status <id>`
Get script run status per device. Returns current state plus any pending operations.
```bash
its intune scripts status <script-id>
its intune scripts status <script-id> --watch
```

## remediations

### `its intune remediations`
List proactive remediation scripts. Surfaces the most common fields; pass --json for raw shape.
Flags: `--top` Number of results (default 50, paginates automatically) · `--all` Fetch all results (overrides --top)
```bash
its intune remediations
its intune remediations --watch
```

### `its intune remediations get <id>`
Get remediation script details. Pass the id (or any natural identifier) as the positional arg.
```bash
its intune remediations get <id>
```

### `its intune remediations status <id>`
Get remediation run status per device. Returns current state plus any pending operations.
```bash
its intune remediations status <id>
its intune remediations status <id> --watch
```

## policies

### `its intune policies`
List device compliance policies. Surfaces the most common fields; pass --json for raw shape.
Flags: `--top` Number of results (default 50, paginates automatically) · `--all` Fetch all results (overrides --top) · `--with-assignments` Include assignment target group IDs inline (one extra column)
```bash
its intune policies
its intune policies --watch
```

### `its intune policies get <id>`
Get compliance policy details. Pass the id (or any natural identifier) as the positional arg.
```bash
its intune policies get <policy-id>
```

### `its intune policies configs`
List device configuration profiles. Configuration profiles applied to the device.
Flags: `--top` Number of results (default 50, paginates automatically) · `--all` Fetch all results (overrides --top) · `--with-assignments` Include assignment target group IDs inline (one extra column)
```bash
its intune policies configs
```

## esp

### `its intune esp`
List Enrollment Status Page profiles. Surfaces the most common fields; pass --json for raw shape.
```bash
its intune esp
its intune esp --watch
```

### `its intune esp get <id>`
Get ESP profile details and tracked apps. Pass the id (or any natural identifier) as the positional arg.
```bash
its intune esp get <profile-id>
```

### `its intune esp update [id]`
Update ESP profile settings (timeout, tracked apps). PATCH semantics — only the supplied fields change.
Flags: `--timeout` Timeout in minutes · `--track-app` Add app ID to tracked apps · `--untrack-app` Remove app ID from tracked apps · `--show-progress` Show installation progress (true/false) · `--allow-use-on-failure` Allow device use on failure (true/false)
```bash
its intune esp update 8f1c2d3e-... --timeout 3600
its intune esp update 8f1c2d3e-... --track-app 9a8b7c6d-...
its intune esp update <profile-id> --timeout 120
```

## autopilot

### `its intune autopilot`
List Autopilot deployment profiles. Surfaces the most common fields; pass --json for raw shape.
```bash
its intune autopilot
its intune autopilot --watch
```

### `its intune autopilot devices`
List Autopilot-registered devices. Returns devices for the resource.
Flags: `--top` Number of results (default 50, paginates automatically) · `--all` Fetch all results (overrides --top)
```bash
its intune autopilot devices
```

### `its intune autopilot tag <serial> [tag]`
Set group tag on an Autopilot device. Set or clear a tag value.
Flags: `--clear` Remove the group tag
```bash
its intune autopilot tag ABC1234 "Finance-Laptops"
its intune autopilot tag ABC1234 --clear
its intune autopilot tag <serial> "Office-Standard"
```

## group

### `its intune group find <groupId>`
Reverse lookup — list every Intune resource assigned to a group.
```bash
its intune group find "All Devices"
```

## assignments

### `its intune assignments audit`
Audit Intune assignments — per-target matrix (which configs/compliance/apps hit each group) plus orphan resources with no assignment. Group GUIDs are resolved to display names.
Flags: `--top` Number of each resource type to fetch (default 100) · `--all` Fetch up to 1000 of each resource type (overrides --top)
```bash
its intune assignments audit
its intune assignments audit --all
```

## settings

### `its intune settings`
List Settings Catalog policies (the modern Intune configuration surface).
Flags: `--top` Number of results (default 50) · `--all` Fetch all results (overrides --top, paginates up to 1000) · `--with-assignments` Expand assignment target group IDs inline
```bash
its intune settings
its intune settings --watch
```

### `its intune settings get <id>`
Get a Settings Catalog policy with assignments expanded.
```bash
its intune settings get <policy-id>
```

## intents

### `its intune intents`
List Endpoint Security policy intents (firewall, ASR, BitLocker, etc.).
Flags: `--top` Number of results (default 50) · `--all` Fetch all results (overrides --top, paginates up to 1000) · `--with-assignments` Expand assignment target group IDs inline
```bash
its intune intents
its intune intents --watch
```

### `its intune intents get <id>`
Get an Endpoint Security intent with assignments expanded.
```bash
its intune intents get <intent-id>
```

## updates

### `its intune updates`
List Windows Update profiles. --type feature|quality|driver (default feature).
Flags: `--type <feature|quality|driver>` Profile category · `--top` Number of results (default 50) · `--all` Fetch all results (overrides --top, paginates up to 1000) · `--with-assignments` Expand assignment target group IDs inline
```bash
its intune updates
its intune updates --watch
```

### `its intune updates get <id>`
Get a Windows Update profile by id (auto-detects type — pass --type to disambiguate).
Flags: `--type <feature|quality|driver>` Profile category
```bash
its intune updates get <ring-id>
```

## appconfig

### `its intune appconfig`
List mobile app configuration policies (per-app key/value config).
Flags: `--top` Number of results (default 50) · `--all` Fetch all results (overrides --top, paginates up to 1000) · `--with-assignments` Expand assignment target group IDs inline
```bash
its intune appconfig
its intune appconfig --watch
```

### `its intune appconfig get <id>`
Get an app configuration policy with assignments expanded.
```bash
its intune appconfig get <policy-id>
```

## appprotection

### `its intune appprotection`
List App Protection (MAM) policies. --platform ios|android (default ios).
Flags: `--platform <ios|android>` Mobile OS · `--top` Number of results (default 50) · `--all` Fetch all results (overrides --top, paginates up to 1000) · `--with-assignments` Expand assignment target group IDs inline
```bash
its intune appprotection --platform ios
its intune appprotection --platform android
its intune appprotection --platform ios --watch
```

### `its intune appprotection get <id>`
Get an App Protection policy by id. Pass the id (or any natural identifier) as the positional arg.
Flags: `--platform <ios|android>` Mobile OS
```bash
its intune appprotection get <policy-id>
```

## doctor

### `its intune doctor`
Intune health snapshot — non-compliant devices, sync staleness, unencrypted endpoints, autopilot pending count.
Flags: `--stale-hours` Threshold for device sync staleness (default 48) · `--top` Maximum devices to scan (default 999)
```bash
its intune doctor
its intune doctor --watch
```

## graph

### `its intune graph get <path>`
Raw Graph GET — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal) · `--raw` Return the response body as raw bytes (no JSON decode). Required for binary endpoints like /content. Currently honoured by the `sp` provider. · `--out` Write the response to this file path instead of stdout. Implies --raw.
```bash
its intune graph get /users
its intune graph get /administrativeUnits --beta
its intune graph get /users --header ConsistencyLevel=eventual
its intune graph get "/deviceManagement/managedDevices?$top=5"
```

### `its intune graph post <path>`
Raw Graph POST — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--body` Request body — inline JSON string or @file.json to read from disk · `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its intune graph post /users --body '{"displayName":"Jane Smith"}'
its intune graph post /administrativeUnits --beta
its intune graph post /users --header ConsistencyLevel=eventual
its intune graph post "/deviceManagement/managedDevices/<id>/syncDevice"
```

### `its intune graph patch <path>`
Raw Graph PATCH — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--body` Request body — inline JSON string or @file.json to read from disk · `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its intune graph patch /users --body '{"displayName":"Jane Smith"}'
its intune graph patch /administrativeUnits --beta
its intune graph patch /users --header ConsistencyLevel=eventual
its intune graph patch "/deviceManagement/deviceCompliancePolicies/<id>" --body @./patch.json
```

### `its intune graph put <path>`
Raw Graph PUT — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--body` Request body — inline JSON string or @file.json to read from disk · `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its intune graph put /users --body '{"displayName":"Jane Smith"}'
its intune graph put /administrativeUnits --beta
its intune graph put /users --header ConsistencyLevel=eventual
its intune graph put "/deviceManagement/managedDevices/<id>" --body @./body.json
```

### `its intune graph delete <path>`
Raw Graph DELETE — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its intune graph delete /groups/8f1c2d3e-4a5b-6c7d-8e9f-0a1b2c3d4e5f
its intune graph delete /administrativeUnits --beta
its intune graph delete /users --header ConsistencyLevel=eventual
its intune graph delete "/deviceManagement/managedDevices/<id>"
```
