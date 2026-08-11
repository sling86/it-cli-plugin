# Power Platform (`pa`)

Power Platform admin API — environments, Power Automate cloud flows, Power Apps canvas apps, connections.

> Auto-generated reference. Configure: `its pa setup`. For a command you can name, prefer live help `its pa <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## environments

### `its pa environments`
List Power Platform environments (admin). Surfaces the most common fields; pass --json for raw shape.
```bash
its pa environments
its pa environments --watch
```

### `its pa environments get <environment_id>`
Show details for one environment. Pass the id (or any natural identifier) as the positional arg.
```bash
its pa environments get <env-id>
```

## flows

### `its pa flows`
List Power Automate cloud flows. Defaults to all environments — use --environment <id> to scope.
Flags: `--environment` Limit to a single environment (id from `its pa environments`) · `--state <Started|Stopped|Suspended>` Filter by state
```bash
its pa flows --environment <env-id>
its pa flows --environment <env-id> --state Suspended
its pa flows --environment <env-id> --watch
```

### `its pa flows get <flow_id>`
Show flow details (definition, triggers, actions). Pass the id (or any natural identifier) as the positional arg.
Flags: `--environment` Environment id (required)
```bash
its pa flows get <flow-id> --environment <env-id>
```

### `its pa flows permissions <flow_id>`
List a flow's owners/co-owners (ACL entries). Each entry's `aclId` is the id used to remove it — not the principal's object id.
Flags: `--environment` Environment id (required)

### `its pa flows stop <flow_id>`
Turn a flow off (admin). Stop the resource. Use --confirm if the action is destructive.
Flags: `--environment` Environment id (required)
```bash
its pa flows stop 8f1c2d3e-... --environment Default-1a2b3c4d
its pa flows stop <flow-id> --environment <env-id>
```

### `its pa flows start <flow_id>`
Turn a flow on (admin). Start the resource. Idempotent.
Flags: `--environment` Environment id (required)
```bash
its pa flows start 8f1c2d3e-... --environment Default-1a2b3c4d
its pa flows start <flow-id> --environment <env-id>
```

### `its pa flows delete <flow_id>`
Delete a flow (admin). Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record.
Flags: `--environment` Environment id (required) · `--confirm` Confirm deletion
```bash
its pa flows delete 8f1c2d3e-4a5b-6c7d-8e9f-0a1b2c3d4e5f --environment Default-1a2b3c4d --confirm
its pa flows list --environment Default-1a2b3c4d
its pa flows delete <flow-id> --environment <env-id> --confirm
```

### `its pa flows set-owner <flow_id>`
Change ownership / permissions on a cloud flow (admin). --owner <upn|guid> upserts a principal (default role CanEdit = full co-owner); --remove <upn|guid> revokes one. Idempotent. Used to reclaim flows from disabled accounts during licence reclaim
Flags: `--environment` Environment id (required) · `--owner` Principal to grant the role to (UPN or AAD object id). Mutually exclusive with --remove. · `--remove` Principal to revoke (UPN or AAD object id). Mutually exclusive with --owner. · `--role <CanEdit|CanViewWithShare|CanView>` Permission tier when granting · `--confirm` Required to execute the mutation
```bash
its pa flows set-owner 8f1c2d3e-... --environment Default-1a2b3c4d --owner jane.smith@example.com --role CanEdit --confirm
its pa flows set-owner 8f1c2d3e-... --environment Default-1a2b3c4d --remove jane.smith@example.com --confirm
```

### `its pa flows runs <flow_id>`
List recent runs for a flow. Returns historical run records.
Flags: `--environment` Environment id (required) · `--top` Max runs to fetch (default 50)
```bash
its pa flows runs <flow-id> --environment <env-id>
```

## apps

### `its pa apps`
List Power Apps canvas apps. Defaults to all envs — scope with --environment <id>.
Flags: `--environment` Limit to a single environment
```bash
its pa apps --environment <env-id>
its pa apps --environment <env-id> --watch
```

## connections

### `its pa connections`
List connections in an environment. Surfaces the most common fields; pass --json for raw shape.
Flags: `--environment` Environment id (required)
```bash
its pa connections --environment <env-id>
its pa connections --environment <env-id> --watch
```
