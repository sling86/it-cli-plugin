# Business Central (`bc`)

Business Central (Dynamics 365) — companies, multi-company entity queries via OData (standard v2.0 or a custom `--api publisher/group/version` route), record get/create/update/delete and bound actions (writes need an explicit `--env` and `--confirm`), environments (list, sandbox copy/delete, operations via the Admin Centre API), installed apps (list, updates, install, uninstall, update) and BC users/permission sets. Runs app-only as a dedicated app registration (BC_CLIENT_ID/BC_CLIENT_SECRET; falls back to the shared Entra credentials). That app needs: the Business Central API roles, an entry in each environment's BC page *Microsoft Entra Applications* (registration is per environment — production and every sandbox) with a user holding D365 AUTOMATION + D365 READ (add a write permission set only to test writes), and authorisation under the Admin Centre's *Authorized Microsoft Entra apps* for environments and apps.

> Auto-generated reference. Configure: `its bc setup`. For a command you can name, prefer live help `its bc <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## companies

### `its bc companies`
List all BC companies visible to the service principal. Surfaces the most common fields; pass --json for raw shape.
Flags: `--env` BC environment name (default: BC_ENVIRONMENT)
```bash
its bc companies
its bc companies --watch
```

### `its bc companies get <nameOrId>`
Resolve a company by name/id/partial match. Pass the id (or any natural identifier) as the positional arg.
Flags: `--env` BC environment name (default: BC_ENVIRONMENT)
```bash
its bc companies get "Head Office"
```

## environments

### `its bc environments`
List all BC environments (Production/Sandbox) on the tenant.

### `its bc environments copy <source>`
Copy an environment into a new sandbox (Admin Centre API; sandbox only). The copy carries the source's per-tenant extensions and Entra app registrations, so copy Production to get a sandbox the CLI app can already use. Previews without --confirm; the copy runs in the background — follow it with `environments operations`.
Flags: `--name` Name for the new environment · `--confirm` Apply the change
```bash
its bc environments copy Production --name Sandbox-Test --confirm
```

### `its bc environments delete <name>`
Delete a SANDBOX environment (Admin Centre API). Refuses anything that isn't a sandbox. Soft-deleted for 14 days. Previews without --confirm.
Flags: `--confirm` Apply the change
```bash
its bc environments delete Sandbox-Test --confirm
```

### `its bc environments operations <name>`
Operations on an environment (copy, create, delete, update) with status — how to follow a copy.

## entities

### `its bc entities`
List entity sets exposed by the BC API (OData service document). Tenant-level, so --company has no effect; pass --api publisher/group/version to discover a custom API's entities (e.g. /platformSync/v2.0). Listing an entity is not proof you can read it — a 403 on query means the app's BC user lacks a permission set.
Flags: `--env` BC environment name (default: BC_ENVIRONMENT) · `--api` Custom API route <publisher>/<group>/<version> (default: standard v2.0 API)

## extensions

### `its bc extensions`
List published AL extensions with their versions — the direct answer to "which environment is on which app version?". Prefers the Automation API (per-company, includes per-tenant extensions); where that identity is refused, falls back to the Admin Centre route, which needs a delegated Dynamics 365 admin (--auth az) and covers AppSource apps only. The summary says which answered.
Flags: `--env` BC environment name (default: BC_ENVIRONMENT) · `--company` Company name/id (default: first company) · `--publisher` Only extensions from this publisher (substring)
```bash
its bc extensions list --env Production
its bc extensions list --env Production --publisher ```

### `its bc extensions get <name>`
Get one extension's published version by name (substring, case-insensitive) — for gating on "is this environment on >= x.y.z?".
Flags: `--env` BC environment name (default: BC_ENVIRONMENT) · `--company` Company name/id (default: first company)
```bash
its bc extensions get platformSync --env Production
```

## query

### `its bc query get <entity>`
Query a BC API entity, or a published web service with --odata-v4 --company <exact name>. Supports filter/top/select and follows pages (up to 20 pages).
Flags: `--odata-v4` Read a published ODataV4 web service; requires --company <exact case-sensitive name>, cannot use --api · `--company` Company name/id (default: first company); exact name required with --odata-v4 · `--filter` OData $filter expression · `--top` Max records (default 50) · `--select` $select fields (comma-separated) · `--orderby` $orderby expression · `--all` Fetch all pages (up to 10000 records; overrides --top) · `--env` BC environment name (default: BC_ENVIRONMENT) · `--api` Custom API route <publisher>/<group>/<version> (default: standard v2.0 API)
```bash
its bc query get items --company <company-id>
```

## record

### `its bc record get <entity> <id>`
Get a single BC record by entity + ID. Pass the id (or any natural identifier) as the positional arg.
Flags: `--company` Company name/id (default: first company) · `--env` BC environment name (default: BC_ENVIRONMENT) · `--api` Custom API route <publisher>/<group>/<version> (default: standard v2.0 API)
```bash
its bc record get items <item-id>
```

### `its bc record create <entity>`
Create a record. Needs --env <name> spelled out (never the default) and --body '{…}' or @file.json. Previews without --confirm.
Flags: `--company` Company name or id, exact (REQUIRED when the environment has more than one company) · `--env` BC environment name — REQUIRED for writes, never defaulted · `--api` Custom API route <publisher>/<group>/<version> (default: standard v2.0 API) · `--body` JSON body — inline or @path/to/file.json · `--confirm` Apply the change
```bash
its bc record create customers --env Support --body '{"displayName":"Test Ltd"}' --confirm
```

### `its bc record update <entity> <id>`
Change fields on a record (PATCH). Reads the record first and sends its etag, so a record changed since fails instead of being overwritten. Previews the changed fields without --confirm.
Flags: `--company` Company name or id, exact (REQUIRED when the environment has more than one company) · `--env` BC environment name — REQUIRED for writes, never defaulted · `--api` Custom API route <publisher>/<group>/<version> (default: standard v2.0 API) · `--body` JSON body — inline or @path/to/file.json · `--confirm` Apply the change
```bash
its bc record update customers <id> --env Support --body '{"phoneNumber":"01234"}' --confirm
```

### `its bc record delete <entity> <id>`
Delete a record. Reads it first and shows what would go; sends its etag. Previews without --confirm.
Flags: `--company` Company name or id, exact (REQUIRED when the environment has more than one company) · `--env` BC environment name — REQUIRED for writes, never defaulted · `--api` Custom API route <publisher>/<group>/<version> (default: standard v2.0 API) · `--confirm` Apply the change
```bash
its bc record delete customers <id> --env Support --confirm
```

### `its bc record call <entity> <id> <action>`
Run a bound action on a record (POST …/Microsoft.NAV.<action>), e.g. post or release. The action name must match the AL page's [ServiceEnabled] procedure. Previews without --confirm.
Flags: `--company` Company name or id, exact (REQUIRED when the environment has more than one company) · `--env` BC environment name — REQUIRED for writes, never defaulted · `--api` Custom API route <publisher>/<group>/<version> (default: standard v2.0 API) · `--body` Optional JSON body for the action — inline or @file.json · `--confirm` Apply the change
```bash
its bc record call salesInvoices <id> post --env Support --confirm
```

## users

### `its bc users`
Business Central users: user name, state, contact email. Automation API — if it 403s, rerun with --auth az or give the app's BC user the D365 AUTOMATION permission set.
Flags: `--company` Company (name or id) whose API endpoint to use · `--env` BC environment name (default: BC_ENVIRONMENT)

### `its bc users get <user>`
One BC user and every permission set they hold (with the company each applies to — blank = all companies).
Flags: `--company` Company (name or id) whose API endpoint to use · `--env` BC environment name (default: BC_ENVIRONMENT)

### `its bc users permission-sets`
Every permission set that can be assigned (system and extension-supplied).
Flags: `--company` Company (name or id) whose API endpoint to use · `--env` BC environment name (default: BC_ENVIRONMENT)

### `its bc users create`
Not possible — explains why. The BC Automation API has no user create (only GET and PATCH state/expiryDate), and a user's default company or profile is not on that entity. BC users come from Entra: license the person in Entra, then use the BC Users page (Update Users from Microsoft 365) and `its bc users add-permission`.

### `its bc users update <user>`
Enable or disable a BC user and/or set their expiry date (the Automation API's only user write — it cannot create users; they arrive from Entra). Needs an explicit --env. Previews without --confirm; reads the user back.
Flags: `--state <Enabled|Disabled>` Enabled or Disabled · `--expiry` Expiry date (ISO, e.g. 2027-01-01), or 'none' to clear · `--confirm` Apply the change · `--company` Company (name or id) whose API endpoint to use · `--env` BC environment name (default: BC_ENVIRONMENT)
```bash
its bc users update jo.bloggs@example.com --state Disabled --env Support --confirm
```

### `its bc users add-permission <user>`
Give a BC user a permission set — in one company (--in-company) or all (default). Previews without --confirm; reads the user's permissions back.
Flags: `--set` Permission set id, e.g. D365 BUS FULL ACCESS · `--in-company` Company NAME the set applies in (omit = all companies) · `--confirm` Apply the change · `--company` Company (name or id) whose API endpoint to use · `--env` BC environment name (default: BC_ENVIRONMENT)
```bash
its bc users add-permission jo.bloggs@example.com --set "D365 BUS FULL ACCESS" --in-company "CRONUS UK Ltd." --confirm
```

### `its bc users remove-permission <user>`
Take a permission set off a BC user (the assignment matching --set and --in-company). Previews without --confirm.
Flags: `--set` Permission set id, e.g. D365 BUS FULL ACCESS · `--in-company` Company NAME the set applies in (omit = all companies) · `--confirm` Apply the change · `--company` Company (name or id) whose API endpoint to use · `--env` BC environment name (default: BC_ENVIRONMENT)
```bash
its bc users remove-permission jo.bloggs@example.com --set "D365 BUS FULL ACCESS" --in-company "CRONUS UK Ltd." --confirm
```

### `its bc users user-setup [user]`
User Setup: posting date limits, approval and time sheet admin, responsibility centre filters. Reads /mcpDiagnostics/v2.0 (userSetups).
Flags: `--company` Company name or id, exact (REQUIRED when the environment has more than one company) · `--env` BC environment name (default: BC_ENVIRONMENT) · `--top` Max rows (default 50)

### `its bc users warehouse-employees [user]`
Which locations a user works at as a warehouse employee, and their default. Reads /mcpDiagnostics/v2.0 (warehouseEmployees).
Flags: `--company` Company name or id, exact (REQUIRED when the environment has more than one company) · `--env` BC environment name (default: BC_ENVIRONMENT) · `--top` Max rows (default 50)

## apps

### `its bc apps`
Apps installed on an environment, with appId, state and whether they can be uninstalled (Admin Centre API).
Flags: `--env` BC environment name (default: BC_ENVIRONMENT)

### `its bc apps updates`
Newer versions available for installed global apps, with anything that must be installed or updated first.
Flags: `--env` BC environment name (default: BC_ENVIRONMENT)

### `its bc apps upload <file>`
Upload a per-tenant extension (.app up to 50 MB) and install or update it (Admin Centre API pteInstall). Needs --env spelled out, --accept-eula and --confirm; previews the file and settings without --confirm. The install runs in the background — follow it with `apps scheduled` and `apps operations`. Try it in a sandbox first.
Flags: `--env` BC environment name (default: BC_ENVIRONMENT) · `--schedule <Immediate|UpdateWindow|NextMinorUpdate|NextMajorUpdate>` When to install: Immediate (default), UpdateWindow, NextMinorUpdate, NextMajorUpdate (a brand-new app can only be Immediate or UpdateWindow) · `--sync-mode <Add|ForceSync>` Schema sync: Add (default) or ForceSync (can drop data — only when you mean it) · `--with-dependencies` Also install or update apps this one needs (default: stop and list them) · `--accept-eula` Agree to the publisher's terms (required) · `--confirm` Upload and install
```bash
its bc apps upload ./THF_Reporting_1.0.0.0.app --env Support --accept-eula --confirm
```

### `its bc apps scheduled`
Per-tenant extension installs and updates that are scheduled but haven't run yet.
Flags: `--env` BC environment name (default: BC_ENVIRONMENT)

### `its bc apps operations <appId>`
Install, update and uninstall operations for one app, with status and any error — how to follow an upload.
Flags: `--env` BC environment name (default: BC_ENVIRONMENT)

### `its bc apps install <appId>`
Install a global app on an environment (Admin Centre API). Needs --env spelled out. --accept-eula is you agreeing to the publisher's terms. Previews without --confirm.
Flags: `--env` BC environment name (default: BC_ENVIRONMENT) · `--accept-eula` Agree to the publisher's EULA and Microsoft's marketplace terms (required to install) · `--version` Target version (default: latest) · `--with-dependencies` Also install or update apps this one needs (default: stop and list them) · `--confirm` Apply the change
```bash
its bc apps install <appId> --env Support --confirm
```

### `its bc apps uninstall <appId>`
Uninstall an app. Data is kept unless --delete-data. Lists dependents that must go first. Needs --env spelled out. Previews without --confirm.
Flags: `--env` BC environment name (default: BC_ENVIRONMENT) · `--delete-data` Also delete the app's data (cannot be undone) · `--confirm` Apply the change
```bash
its bc apps uninstall <appId> --env Support --confirm
```

### `its bc apps update <appId>`
Update a global app to --version (find it with `apps updates`). Needs --env spelled out. Previews without --confirm.
Flags: `--env` BC environment name (default: BC_ENVIRONMENT) · `--version` Target version (required) · `--with-dependencies` Also install or update apps this one needs (default: stop and list them) · `--confirm` Apply the change
```bash
its bc apps update <appId> --env Support --version 28.5.1.0 --confirm
```

## health

### `its bc health get`
Probe BC connectivity by listing companies. Takes no positional argument; use --env to target a non-default environment.
Flags: `--env` BC environment name (default: BC_ENVIRONMENT)
```bash
its bc health
```

## warehouse

### `its bc warehouse bins`
Warehouse bins. Reads /platformSync/v2.0 (bins).
Flags: `--location` Location code · `--bin` Bin code · `--company` Company name or id, exact (REQUIRED when the environment has more than one company) · `--env` BC environment name (default: BC_ENVIRONMENT) · `--top` Max rows (default 50)

### `its bc warehouse bin-contents`
Which items each bin is set up for, and whether movement is blocked. This route has no on-hand quantity — use `warehouse entries` for movements. Reads /mcp/v2.0 (binContents).
Flags: `--item` Item number · `--bin` Bin code · `--location` Location code · `--blocked` Only bin contents with movement blocked · `--company` Company name or id, exact (REQUIRED when the environment has more than one company) · `--env` BC environment name (default: BC_ENVIRONMENT) · `--top` Max rows (default 50)

### `its bc warehouse picks`
Warehouse pick lines (Take and Place). Reads /mcp/v2.0 (warehouseActivityLineDetails, activity type Pick).
Flags: `--status <open|started|done>` open (nothing handled), started (part handled) or done · `--location` Location code · `--company` Company name or id, exact (REQUIRED when the environment has more than one company) · `--env` BC environment name (default: BC_ENVIRONMENT) · `--top` Max rows (default 50)

### `its bc warehouse entries`
Registered warehouse entries (movements), oldest first (BC cannot sort this route; narrow with --item, --location, --from). Reads /mcp/v2.0 (warehouseEntries).
Flags: `--item` Item number · `--from` Registered on or after this date (YYYY-MM-DD) · `--location` Location code · `--company` Company name or id, exact (REQUIRED when the environment has more than one company) · `--env` BC environment name (default: BC_ENVIRONMENT) · `--top` Max rows (default 50)

### `its bc warehouse reservations`
Reservation entries, oldest first (BC cannot sort this route). --source-doc matches the source document number (sourceId). Reads /platformSync/v2.0 (reservationEntries).
Flags: `--item` Item number · `--source-doc` Source document number · `--location` Location code · `--company` Company name or id, exact (REQUIRED when the environment has more than one company) · `--env` BC environment name (default: BC_ENVIRONMENT) · `--top` Max rows (default 50)

## jobqueue

### `its bc jobqueue`
Job queue entries, most recently changed first. Reads standard v2.0 (jobQueueEntries).
Flags: `--status <error|ready|on-hold|in-process|finished>` Only this status · `--company` Company name or id, exact (REQUIRED when the environment has more than one company) · `--env` BC environment name (default: BC_ENVIRONMENT) · `--top` Max rows (default 50)

### `its bc jobqueue errors`
Failed job queue runs with the error text, newest first. Default window is the last 24 hours. Reads standard v2.0 (jobQueueLogEntries, status Error).
Flags: `--since` Window start: -24h, -7d, a date or an ISO time (default -24h) · `--company` Company name or id, exact (REQUIRED when the environment has more than one company) · `--env` BC environment name (default: BC_ENVIRONMENT) · `--top` Max rows (default 50)
