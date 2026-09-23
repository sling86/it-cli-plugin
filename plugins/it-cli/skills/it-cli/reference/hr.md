# PeopleHR (`hr`)

PeopleHR — bulk employee directory, upcoming and recent starters/leavers, per-employee timesheets (explicit IN/OUT pairs), booked holiday, sickness, other authorised leave and lateness, plus `drift detect` / `drift apply` to keep Entra ID in step with HR. A bulk-read scoped key denies single-record employee endpoints and bulk leave reads, so employee lookups go through the bulk list with a client-side filter, and leave is fetched one person at a time.

> Auto-generated reference. Configure: `its hr setup`. For a command you can name, prefer live help `its hr <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## drift

### `its hr drift detect`
Detect drift between PeopleHR and Entra ID across employeeId, jobTitle, department, officeLocation, employeeType, companyName, employeeHireDate, manager and displayName, plus PHR-only / Entra-only orphans. Read-only. A blank Entra field where PeopleHR has a value counts as drift; a blank PeopleHR value never does.
Flags: `--domain` Entra UPN domain to audit (e.g. example.com). Defaults to every domain seen in active Entra users. · `--company` Restrict PHR side to this company (substring match against Company DisplayValue). Default: search globally. · `--include-disabled` Include disabled Entra accounts (default: only enabled).
```bash
its hr drift detect
its hr drift detect --domain example.com
its hr drift detect --filter apply=true
```

### `its hr drift apply`
Make Entra ID agree with PeopleHR for the drift `hr drift detect` reports. Writes one PATCH per user plus a manager link where needed. Requires --confirm. Never blanks a field, never rewrites displayName, and holds back companyName unless PHR_COMPANY_MAP names the target. Preview first with `hr drift detect` or global --dry-run.
Flags: `--domain` Entra UPN domain to audit (e.g. example.com). Defaults to every domain seen in active Entra users. · `--company` Restrict PHR side to this company (substring match against Company DisplayValue). Default: search globally. · `--include-disabled` Include disabled Entra accounts (default: only enabled). · `--field` Only write these fields, comma-separated. One of: employeeId, jobTitle, department, officeLocation, employeeType, companyName, employeeHireDate, manager. · `--user` Only update this UPN. · `--confirm` Actually write to Entra ID.
```bash
its hr drift apply --dry-run --confirm
its hr drift apply --field officeLocation --confirm
its hr drift apply --user someone@example.com --confirm
```

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

## timesheets

### `its hr timesheets get <employee>`
Get one employee's PeopleHR timesheet rows — up to three TimeIn/TimeOut pairs per day. Read-only. Unlike raw terminal punches these carry an explicit in/out direction. Dates use YYYY-MM-DD; default range is the last 30 days.
Flags: `--from` Start date (YYYY-MM-DD); defaults to 30 days ago · `--to` End date (YYYY-MM-DD); defaults to today

## lates

### `its hr lates get <employee>`
Get one employee's recorded lateness events. Read-only — these are what a manager has logged in PeopleHR, not something inferred from clocking data.
Flags: `--from` Start date (YYYY-MM-DD); defaults to 30 days ago · `--to` End date (YYYY-MM-DD); defaults to today

## holidays

### `its hr holidays get <employee>`
Get one employee's booked holiday. Read-only. Cancelled, declined and rejected requests are not counted as time off.
Flags: `--from` Start date (YYYY-MM-DD); defaults to 30 days ago · `--to` End date (YYYY-MM-DD); defaults to today

## otherleave

### `its hr otherleave get <employee>`
Get one employee's non-sickness authorised absence — unpaid leave, parental, compassionate, birthday leave. Read-only. The reason is a leave-type picklist, but it can still be somebody's bereavement; treat it as personal data.
Flags: `--from` Start date (YYYY-MM-DD); defaults to 30 days ago · `--to` End date (YYYY-MM-DD); defaults to today

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
