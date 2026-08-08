# UniFi Protect (`protect`)

UniFi Protect CCTV — cameras, NVR status, storage, motion events, snapshots.

> Auto-generated reference. Configure: `its protect setup`. For a command you can name, prefer live help `its protect <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## cameras

### `its protect cameras`
List all Protect cameras with status. Surfaces the most common fields; pass --json for raw shape.
Flags: `--site` Named Protect profile (UNIFI_PROTECT_*_<SITE>), or `all` to fan out across every configured site
```bash
its protect cameras
its protect cameras --watch
```

### `its protect cameras get <id>`
Get camera details. Pass the id (or any natural identifier) as the positional arg.
Flags: `--site` Named Protect profile (UNIFI_PROTECT_*_<SITE>), or `all` to fan out across every configured site
```bash
its protect cameras get <camera-id>
```

### `its protect cameras offline`
List disconnected/offline cameras — those whose state is not CONNECTED.
Flags: `--site` Named Protect profile (UNIFI_PROTECT_*_<SITE>), or `all` to fan out across every configured site
```bash
its protect cameras offline
```

### `its protect cameras snapshot <camera_id>`
Get the full-resolution snapshot URL for a camera (authenticated endpoint). Pass the camera id from `cameras list`.
Flags: `--width` Snapshot width in px · `--height` Snapshot height in px · `--site` Named Protect profile (UNIFI_PROTECT_*_<SITE>), or `all` to fan out across every configured site

## nvr

### `its protect nvr`
Show NVR status, storage, and capacity. Surfaces the most common fields; pass --json for raw shape.
Flags: `--site` Named Protect profile (UNIFI_PROTECT_*_<SITE>), or `all` to fan out across every configured site
```bash
its protect nvr
its protect nvr --watch
```

## events

### `its protect events`
List Protect events (motion, smart detections). Surfaces the most common fields; pass --json for raw shape.
Flags: `--hours` Hours to look back (ignored when --start is given) · `--start` Window start — ISO, local 'YYYY-MM-DD HH:mm', or epoch ms · `--end` Window end (default: now) · `--camera` Camera name or ID — comma-separate for several · `--types <motion|smartDetectZone|ring|sensorMotion>` Event types, server-side filter · `--smart <person|vehicle|animal|package>` Smart-detection labels, filtered client-side (the NVR ignores the query param) · `--all` Page through the whole window — the fix for silently truncated busy sites · `--limit` Maximum events to return (default 30, ignored with --all) · `--site` Named Protect profile (UNIFI_PROTECT_*_<SITE>), or `all` to fan out across every configured site
```bash
its protect events --hours 1
its protect events --filter camera=<camera-name> --hours 24
its protect events --hours 1 --watch
```

### `its protect events thumbnail <event_id>`
Download the historical still for an event. Identifiable imagery — --output is required so the write is always deliberate.
Flags: `--output` File to write the JPEG to · `--width` Thumbnail width in px · `--site` Named Protect profile (UNIFI_PROTECT_*_<SITE>), or `all` to fan out across every configured site

## footage

### `its protect footage export <camera>`
Export MP4 footage for one camera over an exact window. Identifiable footage — --output is required so the write is always deliberate.
Flags: `--start` Window start — ISO, local 'YYYY-MM-DD HH:mm', or epoch ms · `--end` Window end (default: now) · `--minutes` Window length from --start when --end is omitted · `--output` File to write the MP4 to · `--site` Named Protect profile (UNIFI_PROTECT_*_<SITE>), or `all` to fan out across every configured site

## dashboard

### `its protect dashboard`
Protect overview — NVR, cameras, storage, recent motion. Surfaces the most common fields; pass --json for raw shape.
Flags: `--site` Named Protect profile (UNIFI_PROTECT_*_<SITE>), or `all` to fan out across every configured site
```bash
its protect dashboard
its protect dashboard --watch
```
