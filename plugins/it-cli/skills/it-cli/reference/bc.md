# Business Central (`bc`)

Business Central (Dynamics 365) — tenant-level companies list, multi-company entity queries via OData, record get. Uses a dedicated BC app registration (BC_CLIENT_ID/BC_CLIENT_SECRET) or falls back to the shared Entra credentials (TENANT_ID/CLIENT_ID/CLIENT_SECRET); either app needs the Business Central API permission granted and an ApplicationUser with a Permission Set inside each BC company.

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

## entities

### `its bc entities`
List entity sets exposed by the BC API (OData service document).
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
Query any BC entity — OData passthrough with filter/top/select.
Flags: `--company` Company name/id (default: first company) · `--filter` OData $filter expression · `--top` Max records (default 50) · `--select` $select fields (comma-separated) · `--orderby` $orderby expression · `--all` Fetch all pages (up to 10000 records; overrides --top) · `--env` BC environment name (default: BC_ENVIRONMENT) · `--api` Custom API route <publisher>/<group>/<version> (default: standard v2.0 API)
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

## health

### `its bc health get`
Probe BC connectivity by listing companies. Takes no positional argument; use --env to target a non-default environment.
Flags: `--env` BC environment name (default: BC_ENVIRONMENT)
```bash
its bc health
```
