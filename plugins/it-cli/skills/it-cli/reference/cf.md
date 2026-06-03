# Cloudflare (`cf`)

Cloudflare API v4 — zones (domains), DNS records, Cloudflare Tunnels (cloudflared), accounts.

> Auto-generated reference. Configure: `its cf setup`. For a command you can name, prefer live help `its cf <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## accounts

### `its cf accounts`
List Cloudflare accounts the token can see. Surfaces the most common fields; pass --json for raw shape.
```bash
its cf accounts
its cf accounts --json
its cf accounts --watch
```

## zones

### `its cf zones`
List Cloudflare zones (domains). Surfaces the most common fields; pass --json for raw shape.
Flags: `--name` Filter by exact domain name
```bash
its cf zones
its cf zones --name example.com
its cf zones
its cf zones --name example.com
its cf zones --watch
```

### `its cf zones get <zone>`
Show zone details (accepts domain name or zone id). Pass the id (or any natural identifier) as the positional arg.
```bash
its cf zones get example.com
its cf zones get example.com --json
```

### `its cf zones purge <zone>`
Purge zone cache (everything, or specific URLs with --file).
Flags: `--file` URL to purge (repeat for multiple). Omit to purge everything.
```bash
its cf zones purge example.com --confirm
```

## dns

### `its cf dns`
List DNS records for a zone. Surfaces the most common fields; pass --json for raw shape.
Flags: `--zone` Zone domain or id (required) · `--type` Filter by record type · `--name` Filter by record name (e.g. www.example.com)
```bash
its cf dns --zone example.com
its cf dns --zone example.com --type A
its cf dns --zone example.com --name www
its cf dns --zone example.com
its cf dns --zone example.com --type A
its cf dns --zone example.com --watch
```

### `its cf dns get <record_id>`
Show a single DNS record. Pass the id (or any natural identifier) as the positional arg.
Flags: `--zone` Zone domain or id (required)
```bash
its cf dns get <record-id> --zone example.com
its cf dns get <record-id> --zone example.com --json
```

### `its cf dns create`
Create a DNS record. Idempotent on duplicate names — use update/edit to mutate an existing record.
Flags: `--zone` Zone domain or id · `--type` Record type · `--name` Record name (e.g. www or www.example.com) · `--content` Record content/value · `--ttl` TTL in seconds (1=auto, default 1) · `--proxied` Proxy through Cloudflare · `--priority` Priority (MX records) · `--comment` Record comment
```bash
its cf dns create --zone example.com --type A --name www --content 1.2.3.4
its cf dns create --zone example.com --type CNAME --name app --content app.example.dokploy.com --proxied false
```

### `its cf dns update <record_id>`
Patch fields on an existing DNS record. PATCH semantics — only the supplied fields change.
Flags: `--zone` Zone domain or id · `--type` New type · `--name` New name · `--content` New content · `--ttl` New TTL (1=auto) · `--proxied` Proxy through Cloudflare · `--priority` New priority · `--comment` New comment
```bash
its cf dns update <record-id> --zone example.com --content 5.6.7.8
its cf dns update <record-id> --zone example.com --content 5.6.7.8 --json
```

### `its cf dns delete <record_id>`
Delete a DNS record. Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record.
Flags: `--zone` Zone domain or id
```bash
its cf dns delete --zone example.com --name www --confirm
```

## tunnels

### `its cf tunnels`
List Cloudflare tunnels (cloudflared) for the account. Surfaces the most common fields; pass --json for raw shape.
Flags: `--account` Account id (defaults to CLOUDFLARE_ACCOUNT_ID)
```bash
its cf tunnels
its cf tunnels --json
its cf tunnels --watch
```

### `its cf tunnels get <tunnel>`
Show tunnel details (accepts name or id). Pass the id (or any natural identifier) as the positional arg.
Flags: `--account` Account id
```bash
its cf tunnels get <tunnel-id>
its cf tunnels get <tunnel-id> --json
```

### `its cf tunnels connections <tunnel>`
List active cloudflared connections for a tunnel. Returns API connections for the resource.
Flags: `--account` Account id
```bash
its cf tunnels connections <tunnel-id>
its cf tunnels connections <tunnel-id> --json
```

### `its cf tunnels delete <tunnel>`
Delete a Cloudflare tunnel (destructive — prompts for confirmation unless --yes). Use --cascade to drop active connections first.
Flags: `--account` Account id · `--cascade` Force-delete even if connections are still active · `--yes` Skip the confirmation prompt
```bash
its cf tunnels delete <tunnel-id> --yes
```

### `its cf tunnels routes <tunnel>`
List public hostname ingress rules for a tunnel. Routes defined in the configuration.
Flags: `--account` Account id
```bash
its cf tunnels routes <tunnel-id>
its cf tunnels routes <tunnel-id> --json
```

## token

### `its cf token url`
Print a Cloudflare dashboard URL pre-filled with the scopes the `cf` provider needs (Zone:Read, DNS:Edit, Tunnel:Read, Account:Read).
Flags: `--name` Token name shown on the Cloudflare dashboard
```bash
its cf token url
its cf token url --json
```

### `its cf token request`
Open the prefilled Cloudflare token page in the browser, then prompt for the new token and save it (keychain + .env). Captures the account id automatically.
Flags: `--name` Token name shown on the Cloudflare dashboard · `--no-open` Just print the URL, don't launch the browser
```bash
its cf token request
its cf token request --no-open
```
