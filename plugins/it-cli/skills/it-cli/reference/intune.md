# Intune (`intune`)

Microsoft Intune device management — managed devices, apps, platform scripts, remediations, compliance policies, ESP, Autopilot.

> Auto-generated reference. Configure: `its intune setup`. For a command you can name, prefer live help `its intune <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## devices

### `its intune devices`
List Intune-managed devices. Surfaces the most common fields; pass --json for raw shape.
Flags: `--top` Maximum results (default all, paginates automatically) · `--all` Fetch all results (overrides --top)
```bash
its intune devices
its intune devices --top 50
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

### `its intune devices primary-user <device>`
Show a device's primary user. Resolve the device by id or name.
```bash
its intune devices primary-user -UD-MP27XZ31
```

### `its intune devices set-primary-user <device> <user>`
Reassign a device's primary user (requires --confirm) — the device handover step. Without --confirm it previews the change, naming the current primary user it would replace.
Flags: `--confirm` Apply the reassignment
```bash
its intune devices set-primary-user -UD-MP27XZ31 jane.smith@example.com
its intune devices set-primary-user -UD-MP27XZ31 jane.smith@example.com --confirm
```

### `its intune devices noncompliant`
List devices failing compliance. Returns devices failing compliance checks.
```bash
its intune devices noncompliant
```

### `its intune devices recovery-keys [device]`
List escrowed BitLocker recovery keys — metadata only, never key material. Pass a device to see just its keys; omit it for the tenant-wide escrow list. Reading a key's value needs the delegated-only BitlockerKey.Read.All, which this app-only provider does not hold: use the Entra portal or Company Portal for the key itself.
```bash
its intune devices recovery-keys -UD-MP27XZ31
its intune devices recovery-keys
```

### `its intune devices rotate-bitlocker <device>`
Rotate a device's BitLocker recovery key (requires --confirm). The device rotates at its next check-in and escrows the new key; anyone holding the printed or copied old key loses access at that point. Needs DeviceManagementManagedDevices.PrivilegedOperations.All.
Flags: `--confirm` Apply the rotation
```bash
its intune devices rotate-bitlocker -UD-MP27XZ31
its intune devices rotate-bitlocker -UD-MP27XZ31 --confirm
```

### `its intune devices wipe <device>`
Factory-reset a device. Everything on it is erased and it has to be re-enrolled — cannot be undone. Takes an EXACT device name, serial or id (never a user or partial name), and --confirm must repeat the device name. Without it, shows the target and does nothing.
Flags: `--confirm` The device's name, repeated, to carry out the wipe · `--keep-user-data` Windows: reset but keep user files · `--keep-enrollment` Keep enrolment state and Entra account · `--protected` Windows: protected wipe — cannot be bypassed by a power cycle, but can leave the device unbootable if interrupted
```bash
its intune devices wipe -UD-MP27XZ31
its intune devices wipe -UD-MP27XZ31 --confirm "-UD-MP27XZ31"
```

### `its intune devices retire <device>`
Retire a device: company data, apps and management removed, personal data left. The device leaves Intune and must re-enrol to come back. EXACT device name, serial or id only, and --confirm must repeat the device name.
Flags: `--confirm` The device's name, repeated, to carry out the retire
```bash
its intune devices retire LEAVER-IPHONE
its intune devices retire LEAVER-IPHONE --confirm "LEAVER-IPHONE"
```

### `its intune devices rename <device> <new_name>`
Rename a managed device (Intune setDeviceName). Exact device name, serial or id only. Windows names are checked (1-15 chars, letters/digits/hyphens); the new name lands at the device's next check-in and needs a restart on Windows.
```bash
its intune devices rename DESKTOP-8H2K1 CCD-LAP-042
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

### `its intune apps assign <app>`
Add ONE group assignment to an Intune app (--intent required|available|uninstall, --exclude for an exclusion). Additive: existing assignments are kept. Refuses a group the app already targets.
Flags: `--group` Group id or exact name · `--intent <required|available|uninstall>` Assignment intent · `--exclude` Exclude the group instead
```bash
its intune apps assign "Company Portal" --group "All Shop Tablets" --intent required
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
Flags: `--top` Maximum results (default all, paginates automatically) · `--all` Fetch all results (overrides --top)
```bash
its intune autopilot devices
```

### `its intune autopilot sync`
Trigger the Autopilot device sync (the portal's Sync button). Intune allows one manual sync per 10 minutes. Pass --status to read the last sync without triggering one.
Flags: `--status` Report the last sync without triggering a new one
```bash
its intune autopilot sync
its intune autopilot sync --status
```

### `its intune autopilot tag <serial> [tag]`
Set group tag on an Autopilot device. Set or clear a tag value.
Flags: `--clear` Remove the group tag
```bash
its intune autopilot tag ABC1234 "Finance-Laptops"
its intune autopilot tag ABC1234 --clear
its intune autopilot tag <serial> "Office-Standard"
```

### `its intune autopilot import`
Register devices with Autopilot from a hardware-hash CSV (Get-WindowsAutopilotInfo -OutputFile). --group-tag sets or overrides the tag. Waits for Autopilot to accept or reject each one (up to --wait seconds, default 180) and reports per device.
Flags: `--csv` The hardware-hash CSV · `--group-tag` Group tag for every device (overrides the CSV column) · `--wait` Seconds to wait for the import to settle (0 = don't wait)
```bash
its intune autopilot import --csv AutopilotHWID.csv --group-tag Office
```

### `its intune autopilot deregister <serial>`
Remove a device's Autopilot registration by EXACT serial number (e.g. before selling or returning it). --confirm must repeat the serial. Intune refuses while the device is still enrolled — retire/delete it first.
Flags: `--confirm` Repeat the serial number to proceed
```bash
its intune autopilot deregister 5CG1234XYZ --confirm 5CG1234XYZ
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

## audit

### `its intune audit`
Intune audit events — who did what, to which device, when. Covers wipes, retires, BitLocker key rotations, primary-user changes, policy and app edits. Newest first. BitLocker key READS are in the Entra directory audit, not here.
Flags: `--since` How far back — 24h, 7d, 30d, or a date (default 7d) · `--actor` Match the person or app that did it · `--activity` Match the activity (e.g. wipe, bitlocker, delete) · `--category` Match the category (e.g. Device, Compliance, Application) · `--device` Match the affected resource's name or id
```bash
its intune audit
its intune audit --since 30d --activity wipe
its intune audit --since 14d --actor jane.smith@example.com
```

## android

### `its intune android`
Android Enterprise enrolment profiles (fully managed / dedicated / COPE) with token expiry and enrolled-device count.

### `its intune android qr <profile>`
Save an Android Enterprise enrolment profile's QR code as a PNG (--out). The QR carries the enrolment token — anyone holding it can enrol a device, so treat the file like a password. --renew-days N mints a fresh token first (needed when it has expired).
Flags: `--out` PNG file to write (default <profile>.png) · `--renew-days` Mint a new token valid this many days (1-90) first
```bash
its intune android qr "Shop Floor Tablets" --out qr.png
its intune android qr "Shop Floor Tablets" --renew-days 90 --out qr.png
```

## app-configs

### `its intune app-configs`
Managed-device app configuration policies (e.g. Android managed Google Play app config).

### `its intune app-configs create`
Create an app configuration policy from a JSON body (--file) — the Graph mobileAppConfiguration shape, which differs per platform, so it is passed through as-is. Needs @odata.type, displayName and targetedMobileApps.
Flags: `--file` JSON body
```bash
its intune app-configs create --file chrome-kiosk.json
```

### `its intune app-configs assign <config_id>`
Add ONE group to an app configuration policy (--exclude to exclude it). Additive — existing assignments are kept.
Flags: `--group` Group id or exact name · `--exclude` Exclude the group instead
```bash
its intune app-configs assign 3f5944e1-9e4a-4b02-9929-ca0bb290fdb4 --group "Facilities Tablets"
```

## enrolment

### `its intune enrolment restrictions`
Device enrolment restrictions per platform: whether the platform is blocked, personal devices blocked, and OS version limits.

### `its intune enrolment set-restriction <config>`
Change one platform's enrolment restriction: --block-platform true|false, --block-personal true|false. Previews before→after without --confirm. If it 403s while you are signed in, your role lacks it — add --auth app.
Flags: `--platform <android|androidForWork|ios|windows|windowsMobile|mac>` Platform · `--block-platform <true|false>` Block the whole platform · `--block-personal <true|false>` Block personally-owned devices · `--confirm` Apply the change
```bash
its intune enrolment set-restriction "All users and all devices" --platform androidForWork --block-personal true --confirm
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
