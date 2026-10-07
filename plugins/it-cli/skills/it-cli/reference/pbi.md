# Power BI (`pbi`)

Power BI Admin API — workspaces, reports, apps, licences, activity log.

> Auto-generated reference. Configure: `its pbi setup`. For a command you can name, prefer live help `its pbi <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## workspaces

### `its pbi workspaces`
List Power BI workspaces (admin API). Surfaces the most common fields; pass --json for raw shape.
Flags: `--top` Max results (default 5000)
```bash
its pbi workspaces
its pbi workspaces --watch
```

### `its pbi workspaces members <workspace_id>`
List users/groups with access to a workspace. Returns direct members; nested groups aren't expanded.
```bash
its pbi workspaces members <workspace-id>
```

### `its pbi workspaces add-user <workspace_id>`
Add a user/group/app to a workspace (admin). Add a primary user; idempotent.
Flags: `--user` User UPN, group ID, or app ID to grant access · `--principal-type <User|Group|App>` Principal type (default: User) · `--access <Admin|Member|Contributor|Viewer>` Access right (default: Member)
```bash
its pbi workspaces add-user 8f1c2d3e-... --user jane.smith@example.com --principal-type User --access Member
its pbi workspaces add-user <workspace-id> --user jane.smith@example.com --access Member
```

### `its pbi workspaces update-user <workspace_id>`
Update a user's access right on a workspace. Change the primary user.
Flags: `--user` User UPN, group ID, or app ID whose access to update · `--principal-type <User|Group|App>` Principal type (default: User) · `--access <Admin|Member|Contributor|Viewer>` New access right
```bash
its pbi workspaces update-user 8f1c2d3e-... --user jane.smith@example.com --principal-type User --access Admin
its pbi workspaces update-user <workspace-id> --user jane.smith@example.com --access Admin
```

### `its pbi workspaces remove-user <workspace_id>`
Remove a user/group/app from a workspace (admin, requires --confirm). Reverse of add-user.
Flags: `--user` User UPN, group ID, or app ID to remove · `--confirm` Confirm the removal
```bash
its pbi workspaces remove-user 8f1c2d3e-... --user jane.smith@example.com --confirm
its pbi workspaces remove-user <workspace-id> --user jane.smith@example.com --confirm
```

## reports

### `its pbi reports`
List Power BI reports (all workspaces or --workspace <id>). Surfaces the most common fields; pass --json for raw shape.
Flags: `--workspace` Limit to a single workspace ID
```bash
its pbi reports
its pbi reports --workspace <workspace-id>
its pbi reports --watch
```

### `its pbi reports search <query>`
Fuzzy search reports by name across all workspaces. Substring match across the most relevant fields; case-insensitive.
```bash
its pbi reports search "sales"
```

### `its pbi reports url <report_id>`
Print the web URL for a report. Returns a reachable connection URL.
```bash
its pbi reports url <report-id>
```

## apps

### `its pbi apps`
List Power BI apps. Use --user <id|upn> to list one user's app access (`its pbi access --user` shows every type).
Flags: `--user` User ID or UPN — reads artifactAccess for that user · `--top` Max results when listing tenant-wide (default 5000)
```bash
its pbi apps
its pbi apps --watch
```

### `its pbi apps users <app_id>`
Who can open a Power BI app (its audience): users, groups and service principals with their access right. Admin API, 200 calls/hour.
```bash
its pbi apps users f089354e-8366-4e18-aea3-4cb4a3a50b48
```

## access

### `its pbi access`
Every Power BI item one user can reach (reports, apps, datasets, dashboards…) with their access right. Admin API, 200 calls/hour.
Flags: `--user` User ID or UPN (required) · `--type <Report|PaginatedReport|Dashboard|Dataset|Dataflow|App|Workspace|PersonalGroup|Capacity>` Only this artifact type (filtered server-side)
```bash
its pbi access --user jane.smith@example.com
its pbi access --user jane.smith@example.com --type Report
```

## licences

### `its pbi licences`
List Power BI licence SKUs with consumption. Surfaces the most common fields; pass --json for raw shape.
```bash
its pbi licences
its pbi licences --watch
```

## activity

### `its pbi activity`
List Power BI activity events. Power BI retains up to 30 days of activity.
Flags: `--since` Time window (e.g. 1h, 1d, 7d, 2w). Default 1d. · `--user` Filter to events by user (UPN). Client-side filter. · `--activity` Filter to a specific activity name (e.g. ViewReport). Client-side filter. · `--top` Limit rows returned (default 200)
```bash
its pbi activity --since 24h
its pbi activity --since 24h --watch
```

## scan

### `its pbi scan workspaces`
Every workspace id in the tenant, personal "My workspace" ones included (the plain `workspaces` list omits those). Admin API, service principal.

### `its pbi scan run`
Scan the tenant with the Scanner API: workspaces, items, owners, users with access, data sources, lineage, model tables, measures and Power Query text. Saves the full result under --out and prints only counts plus which detail levels the tenant returned. Read-only; limit about 500 scans an hour, 100 workspaces each.
Flags: `--out` Directory for the raw scan JSON (created, mode 0700; files 0600). Required — the content is sensitive, so it never goes to the terminal · `--workspace` Comma-separated workspace ids (default: every workspace, personal ones included) · `--detail` Which detail to ask for: lineage, datasources, schema, expressions, users (default: all five) · `--timeout` Seconds to wait for each batch (default 600) · `--poll-interval` Seconds between status checks (default 5)
```bash
its pbi scan run --out ./data/scanner
its pbi scan run --workspace <id> --detail schema,expressions --out ./data/scanner
```

## api

### `its pbi api get <path>`
Raw read-only GET against the Power BI REST API (api.powerbi.com): pass a /v1.0/myorg/... or /v2.0/myorg/... path. Uses the CLI's own admin token; GET only. --to-file saves the JSON (mode 0600) and prints only a count.
Flags: `--to-file` Write the JSON here (mode 0600) instead of printing it
```bash
its pbi api get /v1.0/myorg/admin/widelySharedArtifacts/publishedToWeb
its pbi api get /v1.0/myorg/admin/datasets/<id>/datasources --to-file ./data/ds.json
```

## my

### `its pbi my login`
Sign in as a Power BI user. Opens your browser (authorisation code + PKCE, like `its auth login`) and caches the token locally (~/.its/secrets/pbi-my-token.json). --device-code uses the device-code flow instead, which 's Conditional Access blocks (AADSTS53003).
Flags: `--device-code` Use the device-code flow instead of the browser (blocked at )
```bash
its pbi my login
its pbi my login --device-code
```

### `its pbi my logout`
Clear the cached Power BI user token. Does not revoke it server-side.
```bash
its pbi my logout
```

### `its pbi my whoami`
Show the cached Power BI user account and token expiry.
```bash
its pbi my whoami
```

### `its pbi my workspaces`
List workspaces accessible to the signed-in user. Workspaces visible to the current principal.
```bash
its pbi my workspaces
```

### `its pbi my reports`
List Power BI reports accessible to the signed-in user.
Flags: `--workspace` Limit to a single workspace ID
```bash
its pbi my reports
```

### `its pbi my datasets`
List Power BI datasets accessible to the signed-in user.
Flags: `--workspace` Limit to a single workspace ID
```bash
its pbi my datasets
```

### `its pbi my add-workspace-user <workspace_id>`
Add a user/group/app to a workspace using the signed-in user's permissions (sidesteps SP admin-API restrictions when you're a workspace admin).
Flags: `--user` User UPN, group ID, or app ID to grant access · `--principal-type <User|Group|App>` Principal type (default: User) · `--access <Admin|Member|Contributor|Viewer>` Access right (default: Viewer)
```bash
its pbi my add-workspace-user 8f1c2d3e-... --user jane.smith@example.com --principal-type User --access Member
its pbi my add-workspace-user <workspace-id> --user jane.smith@example.com --access Member
```

### `its pbi my update-workspace-user <workspace_id>`
Update a user's access right on a workspace using the signed-in user's permissions.
Flags: `--user` User UPN, group ID, or app ID whose access to update · `--principal-type <User|Group|App>` Principal type (default: User) · `--access <Admin|Member|Contributor|Viewer>` New access right
```bash
its pbi my update-workspace-user 8f1c2d3e-... --user jane.smith@example.com --principal-type User --access Contributor
its pbi my update-workspace-user <workspace-id> --user jane.smith@example.com --access Admin
```

### `its pbi my remove-workspace-user <workspace_id>`
Remove a user/group/app from a workspace using the signed-in user's permissions (requires --confirm).
Flags: `--user` User UPN, group ID, or app ID to remove · `--confirm` Confirm the removal
```bash
its pbi my remove-workspace-user 8f1c2d3e-... --user jane.smith@example.com --confirm
its pbi my remove-workspace-user <workspace-id> --user jane.smith@example.com --confirm
```

### `its pbi my refresh <dataset_id>`
Trigger a refresh of a dataset owned by the signed-in user. Force the provider to re-pull state from the upstream API.
Flags: `--workspace` Workspace ID containing the dataset (recommended) · `--notify <NoNotification|MailOnCompletion|MailOnFailure>` Email notification policy
```bash
its pbi my refresh 9a8b7c6d-... --workspace 8f1c2d3e-...
its pbi my refresh <dataset-id>
its pbi my refresh <dataset-id> --workspace <workspace-id>
```

### `its pbi my refresh-schedule <dataset_id>`
A dataset's scheduled refresh: enabled, days, times, time zone. Delegated (`its pbi my login`).
Flags: `--workspace` Workspace ID that holds the dataset (needed for datasets you do not own)

### `its pbi my refreshes <dataset_id>`
A dataset's refresh history, newest first (Power BI keeps the last 60). Shows status, start/end and the error CODE only, never the message (it can hold connection detail). Delegated.
Flags: `--workspace` Workspace ID that holds the dataset (needed for datasets you do not own) · `--top` How many runs (default 20, max 60)

### `its pbi my datasources <dataset_id>`
The data sources a dataset reads from: type, server/database or URL, and the connection id (what Power BI calls gatewayId; with no gateways it is the owner's personal cloud connection). No credentials — the API never returns them. Delegated.
Flags: `--workspace` Workspace ID that holds the dataset (needed for datasets you do not own)

### `its pbi my refresh-status`
Which refreshable datasets in a workspace are failing right now: last status and time, failures in the history, the most recent error code, and whether the schedule is on. Two reads per dataset, paced. Delegated.
Flags: `--workspace` Workspace ID (required) · `--failing` Only datasets whose latest run failed
```bash
its pbi my refresh-status --workspace <id> --failing
```
