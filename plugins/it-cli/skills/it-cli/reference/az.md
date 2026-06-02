# Azure CLI (`az`)

Azure CLI — subscriptions, VMs, storage, Key Vault, networking, web apps, costs.

> Auto-generated command reference. Do not edit by hand.
> Configure with `its az setup`; get live help with `its az <resource> help`.

[← Provider index](./index.md)

## Resources

- [account](#account)
- [groups](#groups)
- [resources](#resources)
- [vm](#vm)
- [storage](#storage)
- [keyvault](#keyvault)
- [nsg](#nsg)
- [vnet](#vnet)
- [webapp](#webapp)
- [cost](#cost)

### account

| Command | Description |
|---------|-------------|
| `its az account` | List all Azure subscriptions. Surfaces the most common fields; pass --json for raw shape. |
| `its az account get` | Show current active subscription. Pass the id (or any natural identifier) as the positional arg. |
| `its az account set <subscription>` | Switch active subscription. Single-value write; idempotent on no-op. |

#### `its az account`

List all Azure subscriptions. Surfaces the most common fields; pass --json for raw shape.

**Examples:**

```bash
its az account

# Pipe-friendly output — use with jq / scripts.
its az account --json

# Re-runs every 10s — handy for dashboards or incident response.
its az account --watch
```

#### `its az account get`

Show current active subscription. Pass the id (or any natural identifier) as the positional arg.

**Examples:**

```bash
its az account get

# Pipe-friendly output — use with jq / scripts.
its az account get --json
```

#### `its az account set <subscription>`

Switch active subscription. Single-value write; idempotent on no-op.

**Examples:**

```bash
its az account set --subscription <sub-id>

# Subscription display name is accepted in place of GUID.
its az account set --subscription "Production"
```

---

### groups

| Command | Description |
|---------|-------------|
| `its az groups` | List resource groups. Surfaces the most common fields; pass --json for raw shape. |

#### `its az groups`

List resource groups. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--subscription` | `-s` | Subscription name or ID | — |

**Examples:**

```bash
its az groups

# Pipe-friendly output — use with jq / scripts.
its az groups --json

# Re-runs every 10s — handy for dashboards or incident response.
its az groups --watch
```

---

### resources

| Command | Description |
|---------|-------------|
| `its az resources` | List Azure resources. Surfaces the most common fields; pass --json for raw shape. |
| `its az resources get <id>` | Show resource detail by ID. Pass the id (or any natural identifier) as the positional arg. |

#### `its az resources`

List Azure resources. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Filter by resource group | — |
| `--type` | `` | Filter by resource type | — |
| `--subscription` | `-s` | Subscription name or ID | — |

**Examples:**

```bash
its az resources

its az resources --rg my-rg

# Re-runs every 10s — handy for dashboards or incident response.
its az resources --watch
```

#### `its az resources get <id>`

Show resource detail by ID. Pass the id (or any natural identifier) as the positional arg.

**Examples:**

```bash
its az resources get /subscriptions/<sub>/resourceGroups/<rg>/...

# Pipe-friendly output — use with jq / scripts.
its az resources get /subscriptions/<sub>/resourceGroups/<rg>/... --json
```

---

### vm

| Command | Description |
|---------|-------------|
| `its az vm` | List virtual machines with power state. Surfaces the most common fields; pass --json for raw shape. |
| `its az vm get <name>` | Show VM detail. Pass the id (or any natural identifier) as the positional arg. |
| `its az vm start <name>` | Start a VM. Start the resource. Idempotent. |
| `its az vm stop <name>` | Stop a VM (still billed — use deallocate to stop billing). Stop the resource. Use --confirm if the action is destructive. |
| `its az vm restart <name>` | Restart a VM. Stop + start in one call. |
| `its az vm deallocate <name>` | Deallocate a VM (stops billing). Stop + release compute resources. Billing pauses. |

#### `its az vm`

List virtual machines with power state. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Resource group | — |
| `--subscription` | `-s` | Subscription | — |

**Examples:**

```bash
its az vm

its az vm --status running

# Re-runs every 10s — handy for dashboards or incident response.
its az vm --watch
```

#### `its az vm get <name>`

Show VM detail. Pass the id (or any natural identifier) as the positional arg.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Resource group (required) | — |

**Examples:**

```bash
its az vm get my-vm

# Pipe-friendly output — use with jq / scripts.
its az vm get my-vm --json
```

#### `its az vm start <name>`

Start a VM. Start the resource. Idempotent.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Resource group (required) | — |
| `--confirm` | `` | Confirm the operation | — |

**Examples:**

```bash
its az vm start my-vm --rg my-rg

# Return immediately; don't block for boot.
its az vm start my-vm --rg my-rg --no-wait
```

#### `its az vm stop <name>`

Stop a VM (still billed — use deallocate to stop billing). Stop the resource. Use --confirm if the action is destructive.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Resource group (required) | — |
| `--confirm` | `` | Confirm the operation | — |

**Examples:**

```bash
its az vm stop my-vm --rg my-rg
```

#### `its az vm restart <name>`

Restart a VM. Stop + start in one call.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Resource group (required) | — |
| `--confirm` | `` | Confirm the operation | — |

**Examples:**

```bash
its az vm restart my-vm --rg my-rg

# Pipe-friendly output — use with jq / scripts.
its az vm restart my-vm --rg my-rg --json
```

#### `its az vm deallocate <name>`

Deallocate a VM (stops billing). Stop + release compute resources. Billing pauses.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Resource group (required) | — |
| `--confirm` | `` | Confirm the operation | — |

**Examples:**

```bash
# Cheaper than stop — releases compute reservation
its az vm deallocate my-vm --rg my-rg

# Pipe-friendly output — use with jq / scripts.
its az vm deallocate my-vm --rg my-rg --json
```

---

### storage

| Command | Description |
|---------|-------------|
| `its az storage` | List storage accounts. Surfaces the most common fields; pass --json for raw shape. |

#### `its az storage`

List storage accounts. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Filter by resource group | — |
| `--subscription` | `-s` | Subscription | — |

**Examples:**

```bash
its az storage

# Pipe-friendly output — use with jq / scripts.
its az storage --json

# Re-runs every 10s — handy for dashboards or incident response.
its az storage --watch
```

---

### keyvault

| Command | Description |
|---------|-------------|
| `its az keyvault` | List Key Vaults. Surfaces the most common fields; pass --json for raw shape. |
| `its az keyvault secrets <vault>` | List secret names in a Key Vault. List vault secret names (values aren't returned by default). |

#### `its az keyvault`

List Key Vaults. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Filter by resource group | — |
| `--subscription` | `-s` | Subscription | — |

**Examples:**

```bash
its az keyvault

# Pipe-friendly output — use with jq / scripts.
its az keyvault --json

# Re-runs every 10s — handy for dashboards or incident response.
its az keyvault --watch
```

#### `its az keyvault secrets <vault>`

List secret names in a Key Vault. List vault secret names (values aren't returned by default).

**Examples:**

```bash
its az keyvault secrets --vault my-kv

# Use with jq to diff against another vault.
its az keyvault secrets --vault my-kv --json
```

---

### nsg

| Command | Description |
|---------|-------------|
| `its az nsg` | List network security groups. Surfaces the most common fields; pass --json for raw shape. |
| `its az nsg get <name>` | Show NSG detail with security rules. Pass the id (or any natural identifier) as the positional arg. |

#### `its az nsg`

List network security groups. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Filter by resource group | — |
| `--subscription` | `-s` | Subscription | — |

**Examples:**

```bash
its az nsg

# Pipe-friendly output — use with jq / scripts.
its az nsg --json

# Re-runs every 10s — handy for dashboards or incident response.
its az nsg --watch
```

#### `its az nsg get <name>`

Show NSG detail with security rules. Pass the id (or any natural identifier) as the positional arg.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Resource group (required) | — |

**Examples:**

```bash
its az nsg get my-nsg --rg my-rg

# Pipe-friendly output — use with jq / scripts.
its az nsg get my-nsg --rg my-rg --json
```

---

### vnet

| Command | Description |
|---------|-------------|
| `its az vnet` | List virtual networks. Surfaces the most common fields; pass --json for raw shape. |
| `its az vnet get <name>` | Show VNet detail with subnets. Pass the id (or any natural identifier) as the positional arg. |

#### `its az vnet`

List virtual networks. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Filter by resource group | — |
| `--subscription` | `-s` | Subscription | — |

**Examples:**

```bash
its az vnet

# Pipe-friendly output — use with jq / scripts.
its az vnet --json

# Re-runs every 10s — handy for dashboards or incident response.
its az vnet --watch
```

#### `its az vnet get <name>`

Show VNet detail with subnets. Pass the id (or any natural identifier) as the positional arg.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Resource group (required) | — |

**Examples:**

```bash
its az vnet get my-vnet --rg my-rg

# Pipe-friendly output — use with jq / scripts.
its az vnet get my-vnet --rg my-rg --json
```

---

### webapp

| Command | Description |
|---------|-------------|
| `its az webapp` | List web apps. Surfaces the most common fields; pass --json for raw shape. |
| `its az webapp get <name>` | Show web app detail. Pass the id (or any natural identifier) as the positional arg. |
| `its az webapp restart <name>` | Restart a web app. Stop + start in one call. |

#### `its az webapp`

List web apps. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Filter by resource group | — |
| `--subscription` | `-s` | Subscription | — |

**Examples:**

```bash
its az webapp

# Useful when feeding a compliance report.
its az webapp --json

# Re-runs every 10s — handy for dashboards or incident response.
its az webapp --watch
```

#### `its az webapp get <name>`

Show web app detail. Pass the id (or any natural identifier) as the positional arg.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Resource group (required) | — |

**Examples:**

```bash
its az webapp get my-app --rg my-rg

# Pipe-friendly output — use with jq / scripts.
its az webapp get my-app --rg my-rg --json
```

#### `its az webapp restart <name>`

Restart a web app. Stop + start in one call.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--rg` | `` | Resource group (required) | — |
| `--confirm` | `` | Confirm the operation | — |

**Examples:**

```bash
its az webapp restart my-app --rg my-rg

# Pipe-friendly output — use with jq / scripts.
its az webapp restart my-app --rg my-rg --json
```

---

### cost

| Command | Description |
|---------|-------------|
| `its az cost summary` | Cost summary for current billing period by resource group. Quick one-screen view — designed for dashboards / `--watch`. |

#### `its az cost summary`

Cost summary for current billing period by resource group. Quick one-screen view — designed for dashboards / `--watch`.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--period` | `` | Time period (MonthToDate, BillingMonthToDate, TheLastMonth) | MonthToDate |
| `--subscription` | `-s` | Subscription | — |

**Examples:**

```bash
# Current billing period spend by service
its az cost summary

# Pipe-friendly output — use with jq / scripts.
its az cost summary --json

# Re-runs every 10s — handy for dashboards or incident response.
its az cost summary --watch
```

---
