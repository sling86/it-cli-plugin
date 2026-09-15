# Factory attendance (`attendance`)

Read-only ZKTeco attendance collection over TCP 4370. Uses an explicit terminal allow-list, sequential transfers, device-count verification, Europe/London wall time and a private 30-day cache. Raw punches remain separate from inferred first/last summaries. Optional PeopleHR joins use only reviewed TimeAndAttendanceId values — never name matching or EmployeeId guesses.

> Auto-generated reference. Configure: `its attendance setup`. For a command you can name, prefer live help `its attendance <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## terminals

### `its attendance terminals`
Check every explicitly configured ZKTeco terminal sequentially. Read-only: reports user/log counts and never requests fingerprint templates or device changes.
```bash
its attendance terminals
```

## events

### `its attendance events`
List raw terminal punches in Europe/London time. Records have no IN/OUT state. Every fresh transfer is sequential and rejected unless received rows equal the device's advertised log count.
Flags: `--since` Start of the local Europe/London window; default -30d, maximum 30 days · `--until` End of the local Europe/London window; default now · `--terminal` Restrict collection to one configured terminal name or host · `--peoplehr` Join only reviewed PeopleHR TimeAndAttendanceId mappings; never guesses by name or EmployeeId · `--refresh` Ignore the short-lived private cache and perform a fresh, count-checked transfer
```bash
its attendance events
its attendance events --since -7d --until 2026-09-14
its attendance events --terminal "Factory A" --since -1d
its attendance events --peoplehr --since -7d --json
```

## summary

### `its attendance summary`
Group raw punches by person and local day. First/last and observed-window fields are inference only — not clock-in/out, paid hours, lateness, overtime, absence or current presence.
Flags: `--since` Start of the local Europe/London window; default -30d, maximum 30 days · `--until` End of the local Europe/London window; default now · `--terminal` Restrict collection to one configured terminal name or host · `--peoplehr` Join only reviewed PeopleHR TimeAndAttendanceId mappings; never guesses by name or EmployeeId · `--refresh` Ignore the short-lived private cache and perform a fresh, count-checked transfer
```bash
its attendance summary
its attendance summary --peoplehr --since -7d
```

## exceptions

### `its attendance exceptions`
Report single/odd-punch person-days and punches whose terminal user record has been deleted. Only complete local days inside the requested window are checked, so a cut-off cannot invent a missing punch. These are review prompts, not proof of absence.
Flags: `--since` Start of the local Europe/London window; default -30d, maximum 30 days · `--until` End of the local Europe/London window; default now · `--terminal` Restrict collection to one configured terminal name or host · `--peoplehr` Join only reviewed PeopleHR TimeAndAttendanceId mappings; never guesses by name or EmployeeId · `--refresh` Ignore the short-lived private cache and perform a fresh, count-checked transfer · `--type <all|single-punch-day|odd-punch-count|unknown-identity>` Restrict exception type
```bash
its attendance exceptions --since -7d
its attendance exceptions --type single-punch-day --since -30d
```
