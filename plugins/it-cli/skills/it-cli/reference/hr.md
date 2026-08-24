# PeopleHR (`hr`)

PeopleHR — bulk employee directory, upcoming and recent starters/leavers. tenant key is bulk-read scoped (single-record endpoints return Access Denied), so lookups go through the bulk list + client-side filter.

> Auto-generated reference. Configure: `its hr setup`. For a command you can name, prefer live help `its hr <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## drift

### `its hr drift detect`
Detect drift between PeopleHR and Entra ID. Reports field mismatches plus PHR-only / Entra-only orphans. Read-only.
Flags: `--domain` Entra UPN domain to audit (e.g. example.com). Defaults to every domain seen in active Entra users. · `--company` Restrict PHR side to this company (substring match against Company DisplayValue). Default: search globally. · `--include-disabled` Include disabled Entra accounts (default: only enabled).

## absences

### `its hr absences get <employee>`
Get one employee's sickness-absence records. Contains special-category health data. Dates use YYYY-MM-DD; free-text notes are omitted unless --include-notes is passed.
Flags: `--from` Start date (YYYY-MM-DD); defaults to 1 January this year · `--to` End date (YYYY-MM-DD); defaults to today · `--include-notes` Include free-text absence comments

### `its hr absences summary <employee>`
Summarise one employee's sickness absence for a calendar year: episodes, days, longest spell, Bradford factor, emergency leave, outstanding return-to-work interviews, and reason breakdown. Contains special-category health data.
Flags: `--year` Calendar year; defaults to current year

### `its hr absences team`
Rank a manager's direct reports or a department by Bradford factor for one year. Contains special-category health data. Refuses teams larger than 25.
Flags: `--manager` Manager email, EmployeeId, or exact full name · `--department` Exact department name · `--year` Calendar year; defaults to current year

## org

### `its hr org chain <employee>`
Show an employee's management chain up to the top of the tree — level 1 is their direct manager.
```bash
its hr org chain jane.smith@example.com
```

### `its hr org reports <employee>`
List an employee's direct reports, or the whole sub-tree with --recursive. Level 1 is a direct report.
Flags: `--recursive` Include reports of reports, all the way down · `--depth` Limit --recursive to this many levels
```bash
its hr org reports jane.smith@example.com
its hr org reports jane.smith@example.com --recursive
```

### `its hr org leadership`
List employees with no manager set — the top of the tree, plus anyone PeopleHR is missing a reporting line for.
```bash
its hr org leadership
```

## employees

### `its hr employees`
List all employees. Surfaces the most common fields; pass --json for raw shape.
Flags: `--leavers` Include employees who have left
```bash
its hr employees
its hr employees --watch
```

### `its hr employees search <query>`
Search employees by name/email/role/department/location. Substring match across the most relevant fields; case-insensitive.
Flags: `--leavers` Include employees who have left
```bash
its hr employees search "jane"
```

### `its hr employees get <email>`
Get employee details by email (client-side filter). Match is exact on email address — not a fuzzy/name lookup.
```bash
its hr employees get <employee-id>
```

## starters

### `its hr starters`
Upcoming starters — employees with StartDate in the future. Surfaces the most common fields; pass --json for raw shape.
Flags: `--days` Window in days (default 30)
```bash
its hr starters
its hr starters --watch
```

### `its hr starters recent`
Recent starters — employees with StartDate in the past window.
Flags: `--days` Window in days (default 30)
```bash
its hr starters recent --days 30
```

## leavers

### `its hr leavers`
Upcoming leavers — employees with LeavingDate in the future.
Flags: `--days` Window in days (default 30)
```bash
its hr leavers
its hr leavers --watch
```

### `its hr leavers recent`
Recent leavers — employees with LeavingDate in the past window.
Flags: `--days` Window in days (default 30)
```bash
its hr leavers recent --days 30
```
