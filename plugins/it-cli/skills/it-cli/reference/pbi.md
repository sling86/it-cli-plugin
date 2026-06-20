# Power BI (`pbi`)

Power BI Admin API — workspaces, reports, apps, licences, activity log.

> Auto-generated reference. Configure: `its pbi setup`. For a command you can name, prefer live help `its pbi <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## workspaces

### `its pbi workspaces`
List Power BI workspaces (admin API). Surfaces the most common fields; pass --json for raw shape.
Flags: `--top` Max results (default 5000)
```bash
its pbi workspaces
its pbi workspaces --json
its pbi workspaces --watch
```

### `its pbi workspaces members <workspace_id>`
List users/groups with access to a workspace. Returns direct members; nested groups aren't expanded.
```bash
its pbi workspaces members <workspace-id>
its pbi workspaces members <workspace-id> --json
```

### `its pbi workspaces add-user <workspace_id>`
Add a user/group/app to a workspace (admin). Add a primary user; idempotent.
Flags: `--user` User UPN, group ID, or app ID to grant access · `--principal-type` Principal type (default: User) · `--access` Access right (default: Member)
```bash
its pbi workspaces add-user <workspace-id> --user jane.smith@example.com --role Member
its pbi workspaces add-user <workspace-id> --user jane.smith@example.com --role Member --json
```

### `its pbi workspaces update-user <workspace_id>`
Update a user's access right on a workspace. Change the primary user.
Flags: `--user` User UPN, group ID, or app ID whose access to update · `--principal-type` Principal type (default: User) · `--access` New access right
```bash
its pbi workspaces update-user <workspace-id> --user jane.smith@example.com --role Admin
its pbi workspaces update-user <workspace-id> --user jane.smith@example.com --role Admin --json
```

### `its pbi workspaces remove-user <workspace_id>`
Remove a user/group/app from a workspace (admin). Reverse of add-user.
Flags: `--user` User UPN, group ID, or app ID to remove
```bash
its pbi workspaces remove-user <workspace-id> --user jane.smith@example.com --confirm
its pbi workspaces remove-user <workspace-id> --user jane.smith@example.com --confirm --json
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
its pbi reports search "sales" --json
```

### `its pbi reports url <report_id>`
Print the web URL for a report. Returns a reachable connection URL.
```bash
its pbi reports url <report-id>
its pbi reports url <report-id> --json
```

## apps

### `its pbi apps`
List Power BI apps. Use --user <id|upn> to list one user's app access.
Flags: `--user` User ID or UPN — reads artifactAccess for that user · `--top` Max results when listing tenant-wide (default 5000)
```bash
its pbi apps
its pbi apps --json
its pbi apps --watch
```

## licences

### `its pbi licences`
List Power BI licence SKUs with consumption. Surfaces the most common fields; pass --json for raw shape.
```bash
its pbi licences
its pbi licences --json
its pbi licences --watch
```

## activity

### `its pbi activity`
List Power BI activity events. Power BI retains up to 30 days of activity.
Flags: `--since` Time window (e.g. 1h, 1d, 7d, 2w). Default 1d. · `--user` Filter to events by user (UPN). Client-side filter. · `--activity` Filter to a specific activity name (e.g. ViewReport). Client-side filter. · `--top` Limit rows returned (default 200)
```bash
its pbi activity --since 24h
its pbi activity --since 24h --json
its pbi activity --since 24h --watch
```

## my

### `its pbi my login`
Sign in as a Power BI user via device-code flow. Tokens are cached locally (~/.its/secrets/pbi-my-token.json).
```bash
its pbi my login
its pbi my login --json
```

### `its pbi my logout`
Clear the cached Power BI user token. Does not revoke it server-side.
```bash
its pbi my logout
its pbi my logout --json
```

### `its pbi my whoami`
Show the cached Power BI user account and token expiry.
```bash
its pbi my whoami
its pbi my whoami --json
```

### `its pbi my workspaces`
List workspaces accessible to the signed-in user. Workspaces visible to the current principal.
```bash
its pbi my workspaces
its pbi my workspaces --json
```

### `its pbi my reports`
List Power BI reports accessible to the signed-in user.
Flags: `--workspace` Limit to a single workspace ID
```bash
its pbi my reports
its pbi my reports --json
```

### `its pbi my datasets`
List Power BI datasets accessible to the signed-in user.
Flags: `--workspace` Limit to a single workspace ID
```bash
its pbi my datasets
its pbi my datasets --json
```

### `its pbi my add-workspace-user <workspace_id>`
Add a user/group/app to a workspace using the signed-in user's permissions (sidesteps SP admin-API restrictions when you're a workspace admin).
Flags: `--user` User UPN, group ID, or app ID to grant access · `--principal-type` Principal type (default: User) · `--access` Access right (default: Viewer)
```bash
its pbi my add-workspace-user <workspace-id> --user jane.smith@example.com --role Member
its pbi my add-workspace-user <workspace-id> --user jane.smith@example.com --role Member --json
```

### `its pbi my update-workspace-user <workspace_id>`
Update a user's access right on a workspace using the signed-in user's permissions.
Flags: `--user` User UPN, group ID, or app ID whose access to update · `--principal-type` Principal type (default: User) · `--access` New access right
```bash
its pbi my update-workspace-user <workspace-id> --user jane.smith@example.com --role Admin
its pbi my update-workspace-user <workspace-id> --user jane.smith@example.com --role Admin --json
```

### `its pbi my remove-workspace-user <workspace_id>`
Remove a user/group/app from a workspace using the signed-in user's permissions.
Flags: `--user` User UPN, group ID, or app ID to remove
```bash
its pbi my remove-workspace-user <workspace-id> --user jane.smith@example.com --confirm
its pbi my remove-workspace-user <workspace-id> --user jane.smith@example.com --confirm --json
```

### `its pbi my refresh <dataset_id>`
Trigger a refresh of a dataset owned by the signed-in user. Force the provider to re-pull state from the upstream API.
Flags: `--workspace` Workspace ID containing the dataset (recommended) · `--notify` Email notification policy
```bash
its pbi my refresh <dataset-id>
its pbi my refresh <dataset-id> --workspace <workspace-id>
```
