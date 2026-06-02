# Business Central (`bc`)

Business Central (Dynamics 365) — tenant-level companies list, multi-company entity queries via OData, record get. Reuses Entra app credentials (TENANT_ID/CLIENT_ID/CLIENT_SECRET) but requires Business Central API permission granted and an ApplicationUser with a Permission Set inside each BC company..

> Auto-generated command reference. Do not edit by hand.
> Configure with `its bc setup`; get live help with `its bc <resource> help`.

[← Provider index](./index.md)

## Resources

- [companies](#companies)
- [query](#query)
- [record](#record)
- [health](#health)

### companies

| Command | Description |
|---------|-------------|
| `its bc companies` | List all BC companies visible to the service principal. Surfaces the most common fields; pass --json for raw shape. |
| `its bc companies get <nameOrId>` | Resolve a company by name/id/partial match. Pass the id (or any natural identifier) as the positional arg. |

#### `its bc companies`

List all BC companies visible to the service principal. Surfaces the most common fields; pass --json for raw shape.

**Examples:**

```bash
its bc companies

# Pipe-friendly output — use with jq / scripts.
its bc companies --json

# Re-runs every 10s — handy for dashboards or incident response.
its bc companies --watch
```

#### `its bc companies get <nameOrId>`

Resolve a company by name/id/partial match. Pass the id (or any natural identifier) as the positional arg.

**Examples:**

```bash
its bc companies get "Head Office"

# Pipe-friendly output — use with jq / scripts.
its bc companies get "Head Office" --json
```

---

### query

| Command | Description |
|---------|-------------|
| `its bc query get <entity>` | Query any BC entity — OData passthrough with filter/top/select |

#### `its bc query get <entity>`

Query any BC entity — OData passthrough with filter/top/select.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--company` | `` | Company name/id (default: first company) | — |
| `--filter` | `` | OData $filter expression | — |
| `--top` | `` | Max records (default 50) | 50 |
| `--select` | `` | $select fields (comma-separated) | — |
| `--orderby` | `` | $orderby expression | — |

**Examples:**

```bash
its bc query get --company <company-id> --entity items

# Pipe-friendly output — use with jq / scripts.
its bc query get --company <company-id> --entity items --json
```

---

### record

| Command | Description |
|---------|-------------|
| `its bc record get <entity> <id>` | Get a single BC record by entity + ID. Pass the id (or any natural identifier) as the positional arg. |

#### `its bc record get <entity> <id>`

Get a single BC record by entity + ID. Pass the id (or any natural identifier) as the positional arg.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--company` | `` | Company name/id (default: first company) | — |

**Examples:**

```bash
its bc record get items <item-id>

# Pipe-friendly output — use with jq / scripts.
its bc record get items <item-id> --json
```

---

### health

| Command | Description |
|---------|-------------|
| `its bc health get` | Probe BC connectivity (lists companies). Pass the id (or any natural identifier) as the positional arg. |

#### `its bc health get`

Probe BC connectivity (lists companies). Pass the id (or any natural identifier) as the positional arg.

**Examples:**

```bash
its bc health

# Pipe-friendly output — use with jq / scripts.
its bc health --json
```

---
