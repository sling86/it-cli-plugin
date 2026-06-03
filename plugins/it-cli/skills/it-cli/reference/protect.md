# UniFi Protect (`protect`)

UniFi Protect CCTV — cameras, NVR status, storage, motion events, snapshots.

> Auto-generated reference. Configure: `its protect setup`. For a command you can name, prefer live help `its protect <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## cameras

### `its protect cameras`
List all Protect cameras with status. Surfaces the most common fields; pass --json for raw shape.
```bash
its protect cameras
its protect cameras --json
its protect cameras --watch
```

### `its protect cameras get <id>`
Get camera details. Pass the id (or any natural identifier) as the positional arg.
```bash
its protect cameras get <camera-id>
its protect cameras get <camera-id> --json
```

### `its protect cameras offline`
List disconnected/offline cameras. Returns recently-disconnected clients.
```bash
its protect cameras offline
its protect cameras offline --json
```

## nvr

### `its protect nvr`
Show NVR status, storage, and capacity. Surfaces the most common fields; pass --json for raw shape.
```bash
its protect nvr
its protect nvr --json
its protect nvr --watch
```

## events

### `its protect events`
List recent Protect events (motion, smart detections). Surfaces the most common fields; pass --json for raw shape.
Flags: `--hours` Hours to look back (default 24) · `--limit` Maximum events to return (default 30)
```bash
its protect events --since 1h
its protect events --camera <camera-id> --since 24h
its protect events --since 1h --watch
```

## dashboard

### `its protect dashboard`
Protect overview — NVR, cameras, storage, recent motion. Surfaces the most common fields; pass --json for raw shape.
```bash
its protect dashboard
its protect dashboard --json
its protect dashboard --watch
```
