# Power Platform (`pa`)

Power Platform admin API — environments, Power Automate cloud flows, Power Apps canvas apps, connections.

> Auto-generated command reference. Do not edit by hand.
> Configure with `its pa setup`; get live help with `its pa <resource> help`.

[← Provider index](./index.md)

## Resources

- [environments](#environments)
- [flows](#flows)
- [apps](#apps)
- [connections](#connections)

### environments

| Command | Description |
|---------|-------------|
| `its pa environments` | List Power Platform environments (admin). Surfaces the most common fields; pass --json for raw shape. |
| `its pa environments get <environment_id>` | Show details for one environment. Pass the id (or any natural identifier) as the positional arg. |

#### `its pa environments`

List Power Platform environments (admin). Surfaces the most common fields; pass --json for raw shape.

**Examples:**

```bash
its pa environments

# Pipe-friendly output — use with jq / scripts.
its pa environments --json

# Re-runs every 10s — handy for dashboards or incident response.
its pa environments --watch
```

#### `its pa environments get <environment_id>`

Show details for one environment. Pass the id (or any natural identifier) as the positional arg.

**Examples:**

```bash
its pa environments get <env-id>

# Pipe-friendly output — use with jq / scripts.
its pa environments get <env-id> --json
```

---

### flows

| Command | Description |
|---------|-------------|
| `its pa flows` | List Power Automate cloud flows. Defaults to all environments — use --environment <id> to scope |
| `its pa flows get <flow_id>` | Show flow details (definition, triggers, actions). Pass the id (or any natural identifier) as the positional arg. |
| `its pa flows stop <flow_id>` | Turn a flow off (admin). Stop the resource. Use --confirm if the action is destructive. |
| `its pa flows start <flow_id>` | Turn a flow on (admin). Start the resource. Idempotent. |
| `its pa flows delete <flow_id>` | Delete a flow (admin). Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record. |
| `its pa flows set-owner <flow_id>` | Change ownership / permissions on a cloud flow (admin). --owner <upn|guid> upserts a principal (default role Owner); --remove <upn|guid> revokes one. Idempotent. Used to reclaim flows from disabled accounts during licence reclaim |
| `its pa flows runs <flow_id>` | List recent runs for a flow. Returns historical run records. |

#### `its pa flows`

List Power Automate cloud flows. Defaults to all environments — use --environment <id> to scope.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--environment` | `-e` | Limit to a single environment (id from `its pa environments`) | — |
| `--state` | `` | Filter by state | — |

**Examples:**

```bash
its pa flows --env <env-id>

its pa flows --env <env-id> --status error

# Re-runs every 10s — handy for dashboards or incident response.
its pa flows --env <env-id> --watch
```

#### `its pa flows get <flow_id>`

Show flow details (definition, triggers, actions). Pass the id (or any natural identifier) as the positional arg.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--environment` | `-e` | Environment id (required) | — |

**Examples:**

```bash
its pa flows get <flow-id> --env <env-id>

# Pipe-friendly output — use with jq / scripts.
its pa flows get <flow-id> --env <env-id> --json
```

#### `its pa flows stop <flow_id>`

Turn a flow off (admin). Stop the resource. Use --confirm if the action is destructive.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--environment` | `-e` | Environment id (required) | — |

**Examples:**

```bash
its pa flows stop <flow-id> --env <env-id>
```

#### `its pa flows start <flow_id>`

Turn a flow on (admin). Start the resource. Idempotent.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--environment` | `-e` | Environment id (required) | — |

**Examples:**

```bash
its pa flows start <flow-id> --env <env-id>

# Pipe-friendly output — use with jq / scripts.
its pa flows start <flow-id> --env <env-id> --json
```

#### `its pa flows delete <flow_id>`

Delete a flow (admin). Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--environment` | `-e` | Environment id (required) | — |

**Examples:**

```bash
its pa flows delete <flow-id> --env <env-id> --confirm
```

#### `its pa flows set-owner <flow_id>`

Change ownership / permissions on a cloud flow (admin). --owner <upn|guid> upserts a principal (default role Owner); --remove <upn|guid> revokes one. Idempotent. Used to reclaim flows from disabled accounts during licence reclaim

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--environment` | `-e` | Environment id (required) | — |
| `--owner` | `` | Principal to grant the role to (UPN or AAD object id). Mutually exclusive with --remove. | — |
| `--remove` | `` | Principal to revoke (UPN or AAD object id). Mutually exclusive with --owner. | — |
| `--role` | `` | Permission tier when granting | Owner |
| `--confirm` | `` | Required to execute the mutation | — |

#### `its pa flows runs <flow_id>`

List recent runs for a flow. Returns historical run records.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--environment` | `-e` | Environment id (required) | — |
| `--top` | `` | Max runs to fetch (default 50) | 50 |

**Examples:**

```bash
its pa flows runs <flow-id> --env <env-id>

# Pipe-friendly output — use with jq / scripts.
its pa flows runs <flow-id> --env <env-id> --json
```

---

### apps

| Command | Description |
|---------|-------------|
| `its pa apps` | List Power Apps canvas apps. Defaults to all envs — scope with --environment <id> |

#### `its pa apps`

List Power Apps canvas apps. Defaults to all envs — scope with --environment <id>.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--environment` | `-e` | Limit to a single environment | — |

**Examples:**

```bash
its pa apps --env <env-id>

# Pipe-friendly output — use with jq / scripts.
its pa apps --env <env-id> --json

# Re-runs every 10s — handy for dashboards or incident response.
its pa apps --env <env-id> --watch
```

---

### connections

| Command | Description |
|---------|-------------|
| `its pa connections` | List connections in an environment. Surfaces the most common fields; pass --json for raw shape. |

#### `its pa connections`

List connections in an environment. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--environment` | `-e` | Environment id (required) | — |

**Examples:**

```bash
its pa connections --env <env-id>

# Pipe-friendly output — use with jq / scripts.
its pa connections --env <env-id> --json

# Re-runs every 10s — handy for dashboards or incident response.
its pa connections --env <env-id> --watch
```

---
