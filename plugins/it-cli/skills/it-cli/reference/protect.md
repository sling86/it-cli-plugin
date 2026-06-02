# UniFi Protect (`protect`)

UniFi Protect CCTV — cameras, NVR status, storage, motion events, snapshots.

> Auto-generated command reference. Do not edit by hand.
> Configure with `its protect setup`; get live help with `its protect <resource> help`.

[← Provider index](./index.md)

## Resources

- [cameras](#cameras)
- [nvr](#nvr)
- [events](#events)
- [dashboard](#dashboard)

### cameras

| Command | Description |
|---------|-------------|
| `its protect cameras` | List all Protect cameras with status. Surfaces the most common fields; pass --json for raw shape. |
| `its protect cameras get <id>` | Get camera details. Pass the id (or any natural identifier) as the positional arg. |
| `its protect cameras offline` | List disconnected/offline cameras. Returns recently-disconnected clients. |

#### `its protect cameras`

List all Protect cameras with status. Surfaces the most common fields; pass --json for raw shape.

**Examples:**

```bash
its protect cameras

# Pipe-friendly output — use with jq / scripts.
its protect cameras --json

# Re-runs every 10s — handy for dashboards or incident response.
its protect cameras --watch
```

#### `its protect cameras get <id>`

Get camera details. Pass the id (or any natural identifier) as the positional arg.

**Examples:**

```bash
its protect cameras get <camera-id>

# Pipe-friendly output — use with jq / scripts.
its protect cameras get <camera-id> --json
```

#### `its protect cameras offline`

List disconnected/offline cameras. Returns recently-disconnected clients.

**Examples:**

```bash
its protect cameras offline

# Pipe-friendly output — use with jq / scripts.
its protect cameras offline --json
```

---

### nvr

| Command | Description |
|---------|-------------|
| `its protect nvr` | Show NVR status, storage, and capacity. Surfaces the most common fields; pass --json for raw shape. |

#### `its protect nvr`

Show NVR status, storage, and capacity. Surfaces the most common fields; pass --json for raw shape.

**Examples:**

```bash
# Storage usage, retention, health
its protect nvr

# Pipe-friendly output — use with jq / scripts.
its protect nvr --json

# Re-runs every 10s — handy for dashboards or incident response.
its protect nvr --watch
```

---

### events

| Command | Description |
|---------|-------------|
| `its protect events` | List recent Protect events (motion, smart detections). Surfaces the most common fields; pass --json for raw shape. |

#### `its protect events`

List recent Protect events (motion, smart detections). Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--hours` | `` | Hours to look back (default 24) | 24 |
| `--limit` | `` | Maximum events to return (default 30) | 30 |

**Examples:**

```bash
its protect events --since 1h

its protect events --camera <camera-id> --since 24h

# Re-runs every 10s — handy for dashboards or incident response.
its protect events --since 1h --watch
```

---

### dashboard

| Command | Description |
|---------|-------------|
| `its protect dashboard` | Protect overview — NVR, cameras, storage, recent motion. Surfaces the most common fields; pass --json for raw shape. |

#### `its protect dashboard`

Protect overview — NVR, cameras, storage, recent motion. Surfaces the most common fields; pass --json for raw shape.

**Examples:**

```bash
its protect dashboard

# Pipe-friendly output — use with jq / scripts.
its protect dashboard --json

# Re-runs every 10s — handy for dashboards or incident response.
its protect dashboard --watch
```

---
