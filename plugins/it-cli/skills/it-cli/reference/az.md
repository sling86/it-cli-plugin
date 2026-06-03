# Azure CLI (`az`)

Azure CLI — subscriptions, VMs, storage, Key Vault, networking, web apps, costs.

> Auto-generated reference. Configure: `its az setup`. For a command you can name, prefer live help `its az <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## account

### `its az account`
List all Azure subscriptions. Surfaces the most common fields; pass --json for raw shape.
```bash
its az account
its az account --json
its az account --watch
```

### `its az account get`
Show current active subscription. Pass the id (or any natural identifier) as the positional arg.
```bash
its az account get
its az account get --json
```

### `its az account set <subscription>`
Switch active subscription. Single-value write; idempotent on no-op.
```bash
its az account set --subscription <sub-id>
its az account set --subscription "Production"
```

## groups

### `its az groups`
List resource groups. Surfaces the most common fields; pass --json for raw shape.
Flags: `--subscription` Subscription name or ID
```bash
its az groups
its az groups --json
its az groups --watch
```

## resources

### `its az resources`
List Azure resources. Surfaces the most common fields; pass --json for raw shape.
Flags: `--rg` Filter by resource group · `--type` Filter by resource type · `--subscription` Subscription name or ID
```bash
its az resources
its az resources --rg my-rg
its az resources --watch
```

### `its az resources get <id>`
Show resource detail by ID. Pass the id (or any natural identifier) as the positional arg.
```bash
its az resources get /subscriptions/<sub>/resourceGroups/<rg>/...
its az resources get /subscriptions/<sub>/resourceGroups/<rg>/... --json
```

## vm

### `its az vm`
List virtual machines with power state. Surfaces the most common fields; pass --json for raw shape.
Flags: `--rg` Resource group · `--subscription` Subscription
```bash
its az vm
its az vm --status running
its az vm --watch
```

### `its az vm get <name>`
Show VM detail. Pass the id (or any natural identifier) as the positional arg.
Flags: `--rg` Resource group (required)
```bash
its az vm get my-vm
its az vm get my-vm --json
```

### `its az vm start <name>`
Start a VM. Start the resource. Idempotent.
Flags: `--rg` Resource group (required) · `--confirm` Confirm the operation
```bash
its az vm start my-vm --rg my-rg
its az vm start my-vm --rg my-rg --no-wait
```

### `its az vm stop <name>`
Stop a VM (still billed — use deallocate to stop billing). Stop the resource. Use --confirm if the action is destructive.
Flags: `--rg` Resource group (required) · `--confirm` Confirm the operation
```bash
its az vm stop my-vm --rg my-rg
```

### `its az vm restart <name>`
Restart a VM. Stop + start in one call.
Flags: `--rg` Resource group (required) · `--confirm` Confirm the operation
```bash
its az vm restart my-vm --rg my-rg
its az vm restart my-vm --rg my-rg --json
```

### `its az vm deallocate <name>`
Deallocate a VM (stops billing). Stop + release compute resources. Billing pauses.
Flags: `--rg` Resource group (required) · `--confirm` Confirm the operation
```bash
its az vm deallocate my-vm --rg my-rg
its az vm deallocate my-vm --rg my-rg --json
```

## storage

### `its az storage`
List storage accounts. Surfaces the most common fields; pass --json for raw shape.
Flags: `--rg` Filter by resource group · `--subscription` Subscription
```bash
its az storage
its az storage --json
its az storage --watch
```

## keyvault

### `its az keyvault`
List Key Vaults. Surfaces the most common fields; pass --json for raw shape.
Flags: `--rg` Filter by resource group · `--subscription` Subscription
```bash
its az keyvault
its az keyvault --json
its az keyvault --watch
```

### `its az keyvault secrets <vault>`
List secret names in a Key Vault. List vault secret names (values aren't returned by default).
```bash
its az keyvault secrets --vault my-kv
its az keyvault secrets --vault my-kv --json
```

## nsg

### `its az nsg`
List network security groups. Surfaces the most common fields; pass --json for raw shape.
Flags: `--rg` Filter by resource group · `--subscription` Subscription
```bash
its az nsg
its az nsg --json
its az nsg --watch
```

### `its az nsg get <name>`
Show NSG detail with security rules. Pass the id (or any natural identifier) as the positional arg.
Flags: `--rg` Resource group (required)
```bash
its az nsg get my-nsg --rg my-rg
its az nsg get my-nsg --rg my-rg --json
```

## vnet

### `its az vnet`
List virtual networks. Surfaces the most common fields; pass --json for raw shape.
Flags: `--rg` Filter by resource group · `--subscription` Subscription
```bash
its az vnet
its az vnet --json
its az vnet --watch
```

### `its az vnet get <name>`
Show VNet detail with subnets. Pass the id (or any natural identifier) as the positional arg.
Flags: `--rg` Resource group (required)
```bash
its az vnet get my-vnet --rg my-rg
its az vnet get my-vnet --rg my-rg --json
```

## webapp

### `its az webapp`
List web apps. Surfaces the most common fields; pass --json for raw shape.
Flags: `--rg` Filter by resource group · `--subscription` Subscription
```bash
its az webapp
its az webapp --json
its az webapp --watch
```

### `its az webapp get <name>`
Show web app detail. Pass the id (or any natural identifier) as the positional arg.
Flags: `--rg` Resource group (required)
```bash
its az webapp get my-app --rg my-rg
its az webapp get my-app --rg my-rg --json
```

### `its az webapp restart <name>`
Restart a web app. Stop + start in one call.
Flags: `--rg` Resource group (required) · `--confirm` Confirm the operation
```bash
its az webapp restart my-app --rg my-rg
its az webapp restart my-app --rg my-rg --json
```

## cost

### `its az cost summary`
Cost summary for current billing period by resource group. Quick one-screen view — designed for dashboards / `--watch`.
Flags: `--period` Time period (MonthToDate, BillingMonthToDate, TheLastMonth) · `--subscription` Subscription
```bash
its az cost summary
its az cost summary --json
its az cost summary --watch
```
