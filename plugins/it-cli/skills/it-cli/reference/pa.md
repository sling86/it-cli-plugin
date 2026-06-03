# Power Platform (`pa`)

Power Platform admin API — environments, Power Automate cloud flows, Power Apps canvas apps, connections.

> Auto-generated reference. Configure: `its pa setup`. For a command you can name, prefer live help `its pa <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## environments

### `its pa environments`
List Power Platform environments (admin). Surfaces the most common fields; pass --json for raw shape.
```bash
its pa environments
its pa environments --json
its pa environments --watch
```

### `its pa environments get <environment_id>`
Show details for one environment. Pass the id (or any natural identifier) as the positional arg.
```bash
its pa environments get <env-id>
its pa environments get <env-id> --json
```

## flows

### `its pa flows`
List Power Automate cloud flows. Defaults to all environments — use --environment <id> to scope.
Flags: `--environment` Limit to a single environment (id from `its pa environments`) · `--state` Filter by state
```bash
its pa flows --env <env-id>
its pa flows --env <env-id> --status error
its pa flows --env <env-id> --watch
```

### `its pa flows get <flow_id>`
Show flow details (definition, triggers, actions). Pass the id (or any natural identifier) as the positional arg.
Flags: `--environment` Environment id (required)
```bash
its pa flows get <flow-id> --env <env-id>
its pa flows get <flow-id> --env <env-id> --json
```

### `its pa flows stop <flow_id>`
Turn a flow off (admin). Stop the resource. Use --confirm if the action is destructive.
Flags: `--environment` Environment id (required)
```bash
its pa flows stop <flow-id> --env <env-id>
```

### `its pa flows start <flow_id>`
Turn a flow on (admin). Start the resource. Idempotent.
Flags: `--environment` Environment id (required)
```bash
its pa flows start <flow-id> --env <env-id>
its pa flows start <flow-id> --env <env-id> --json
```

### `its pa flows delete <flow_id>`
Delete a flow (admin). Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record.
Flags: `--environment` Environment id (required)
```bash
its pa flows delete <flow-id> --env <env-id> --confirm
```

### `its pa flows set-owner <flow_id>`
Change ownership / permissions on a cloud flow (admin). --owner <upn|guid> upserts a principal (default role Owner); --remove <upn|guid> revokes one. Idempotent. Used to reclaim flows from disabled accounts during licence reclaim
Flags: `--environment` Environment id (required) · `--owner` Principal to grant the role to (UPN or AAD object id). Mutually exclusive with --remove. · `--remove` Principal to revoke (UPN or AAD object id). Mutually exclusive with --owner. · `--role` Permission tier when granting · `--confirm` Required to execute the mutation

### `its pa flows runs <flow_id>`
List recent runs for a flow. Returns historical run records.
Flags: `--environment` Environment id (required) · `--top` Max runs to fetch (default 50)
```bash
its pa flows runs <flow-id> --env <env-id>
its pa flows runs <flow-id> --env <env-id> --json
```

## apps

### `its pa apps`
List Power Apps canvas apps. Defaults to all envs — scope with --environment <id>.
Flags: `--environment` Limit to a single environment
```bash
its pa apps --env <env-id>
its pa apps --env <env-id> --json
its pa apps --env <env-id> --watch
```

## connections

### `its pa connections`
List connections in an environment. Surfaces the most common fields; pass --json for raw shape.
Flags: `--environment` Environment id (required)
```bash
its pa connections --env <env-id>
its pa connections --env <env-id> --json
its pa connections --env <env-id> --watch
```
