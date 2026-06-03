# PeopleHR (`hr`)

PeopleHR — bulk employee directory, upcoming and recent starters/leavers. tenant key is bulk-read scoped (single-record endpoints return Access Denied), so lookups go through the bulk list + client-side filter..

> Auto-generated reference. Configure: `its hr setup`. For a command you can name, prefer live help `its hr <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## drift

### `its hr drift detect`
Detect drift between PeopleHR and Entra ID. Reports field mismatches plus PHR-only / Entra-only orphans. Read-only.
Flags: `--domain` Entra UPN domain to audit (e.g. example.com). Defaults to every domain seen in active Entra users. · `--company` Restrict PHR side to this company (substring match against Company DisplayValue). Default: search globally. · `--include-disabled` Include disabled Entra accounts (default: only enabled).

## employees

### `its hr employees`
List all employees. Surfaces the most common fields; pass --json for raw shape.
Flags: `--leavers` Include employees who have left
```bash
its hr employees
its hr employees --json
its hr employees --watch
```

### `its hr employees search <query>`
Search employees by name/email/role/department/location. Substring match across the most relevant fields; case-insensitive.
Flags: `--leavers` Include employees who have left
```bash
its hr employees search "jane"
its hr employees search "jane" --json
```

### `its hr employees get <email>`
Get employee details by email (client-side filter). Pass the id (or any natural identifier) as the positional arg.
```bash
its hr employees get <employee-id>
its hr employees get <employee-id> --json
```

## starters

### `its hr starters`
Upcoming starters — employees with StartDate in the future. Surfaces the most common fields; pass --json for raw shape.
Flags: `--days` Window in days (default 30)
```bash
its hr starters
its hr starters --json
its hr starters --watch
```

### `its hr starters recent`
Recent starters — employees with StartDate in the past window.
Flags: `--days` Window in days (default 30)
```bash
its hr starters recent --days 30
its hr starters recent --days 30 --json
```

## leavers

### `its hr leavers`
Upcoming leavers — employees with LeavingDate in the future.
Flags: `--days` Window in days (default 30)
```bash
its hr leavers
its hr leavers --json
its hr leavers --watch
```

### `its hr leavers recent`
Recent leavers — employees with LeavingDate in the past window.
Flags: `--days` Window in days (default 30)
```bash
its hr leavers recent --days 30
its hr leavers recent --days 30 --json
```
