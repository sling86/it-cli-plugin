# Business Central (`bc`)

Business Central (Dynamics 365) — tenant-level companies list, multi-company entity queries via OData, record get. Reuses Entra app credentials (TENANT_ID/CLIENT_ID/CLIENT_SECRET) but requires Business Central API permission granted and an ApplicationUser with a Permission Set inside each BC company..

> Auto-generated reference. Configure: `its bc setup`. For a command you can name, prefer live help `its bc <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## companies

### `its bc companies`
List all BC companies visible to the service principal. Surfaces the most common fields; pass --json for raw shape.
```bash
its bc companies
its bc companies --json
its bc companies --watch
```

### `its bc companies get <nameOrId>`
Resolve a company by name/id/partial match. Pass the id (or any natural identifier) as the positional arg.
```bash
its bc companies get "Head Office"
its bc companies get "Head Office" --json
```

## query

### `its bc query get <entity>`
Query any BC entity — OData passthrough with filter/top/select.
Flags: `--company` Company name/id (default: first company) · `--filter` OData $filter expression · `--top` Max records (default 50) · `--select` $select fields (comma-separated) · `--orderby` $orderby expression
```bash
its bc query get --company <company-id> --entity items
its bc query get --company <company-id> --entity items --json
```

## record

### `its bc record get <entity> <id>`
Get a single BC record by entity + ID. Pass the id (or any natural identifier) as the positional arg.
Flags: `--company` Company name/id (default: first company)
```bash
its bc record get items <item-id>
its bc record get items <item-id> --json
```

## health

### `its bc health get`
Probe BC connectivity (lists companies). Pass the id (or any natural identifier) as the positional arg.
```bash
its bc health
its bc health --json
```
