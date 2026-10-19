# Yodeck (`yodeck`)

Yodeck digital signage (API v2) — list players (Yodeck calls them screens) with workspace, online/offline, last seen and TV status; get one player in full; `doctor` checks the token. Read-only.

> Auto-generated reference. Configure: `its yodeck setup`. For a command you can name, prefer live help `its yodeck <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## players

### `its yodeck players`
List Yodeck players (screens): id, name, workspace, online/offline, last seen, screen (TV) status.
Flags: `--online` Only online players · `--offline` Only offline players · `--workspace` Workspace ID · `--q` Search by name
```bash
its yodeck players --json
its yodeck players --offline
```

### `its yodeck players get <id>`
One Yodeck player in full: state, hardware, software, resolution, uptime.

## doctor

### `its yodeck doctor`
Is Yodeck set up and reachable? Says 'not set up' plainly if the token is missing.
