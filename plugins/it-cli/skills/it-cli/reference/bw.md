# Bitwarden (`bw`)

Bitwarden vault — search items, get passwords, browse folders.

> Auto-generated reference. Configure: `its bw setup`. For a command you can name, prefer live help `its bw <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## items

### `its bw items`
List all vault items. Surfaces the most common fields; pass --json for raw shape.
Flags: `--type` Filter by type (login/note/card/identity) · `--folder` Filter by folder name · `--favourite` Show only favourites · `--vault` Named vault profile (omit for default)
```bash
its bw items
its bw items --collection "Servers"
its bw items --watch
```

### `its bw items search <query>`
Search vault items by name, username, URL, or notes. Substring match across the most relevant fields; case-insensitive.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw items search "github"
its bw items search "github" --json
```

### `its bw items get <id>`
Get a vault item by ID (includes password and fields). Pass the id (or any natural identifier) as the positional arg.
Flags: `--copy` Copy the secret to the OS clipboard instead of printing it. Auto-clears after --clear-after seconds. · `--clear-after` Seconds before the clipboard is wiped (0 disables). Only meaningful with --copy. · `--vault` Named vault profile (omit for default)
```bash
its bw items get "Server admin"
its bw items get "Server admin" --json
```

### `its bw items totp <query>`
Generate current TOTP code for an item. Returns the current TOTP code — refresh every 30s.
Flags: `--copy` Copy the secret to the OS clipboard instead of printing it. Auto-clears after --clear-after seconds. · `--clear-after` Seconds before the clipboard is wiped (0 disables). Only meaningful with --copy. · `--vault` Named vault profile (omit for default)
```bash
its bw items totp "Server admin"
its bw items totp "Server admin" --json
```

### `its bw items trash`
List trashed vault items. Returns soft-deleted items in the trash bin.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw items trash
its bw items trash --json
```

### `its bw items recent`
List recently modified vault items. Returns the N most recently modified items.
Flags: `--days` Look-back period in days · `--vault` Named vault profile (omit for default)
```bash
its bw items recent
its bw items recent --json
```

### `its bw items favourites`
List favourite vault items. Items the user has starred.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw items favourites
its bw items favourites --json
```

### `its bw items create <name>`
Create a new vault item (login, note, card, or identity). Idempotent on duplicate names — use update/edit to mutate an existing record.
Flags: `--type` Item type: login (default), note, card, identity · `--username` Login username · `--password` Login password · `--uri` Login URL · `--totp` TOTP secret or otpauth URI · `--notes` Notes · `--notes-file` Read notes from a UTF-8 file (use for notes > ~15KB — Windows command-line cap) · `--folder` Folder name (created if it does not exist) · `--field` Custom text field(s) — comma-separated name=value (e.g. --field lan_ip=10.0.0.1,rack=A3). On update, upserts by name. · `--field-hidden` Custom hidden field(s) — comma-separated name=value. Stored as a secret (masked in the UI like a password). · `--vault` Named vault profile (omit for default)
```bash
its bw items create "Router" --username admin --password "s3cret"
its bw items create "Router" --field lan_ip=10.0.0.1 --field-hidden api_token=abc123
its bw items create "Server admin" --username admin --password "P@ssw0rd" --url https://server.example.com
its bw items create "API keys" --type note --notes "stuff"
```

### `its bw items update <id>`
Update a vault item. Preserve-by-default: only the flags you pass change — everything omitted (password, notes, URIs, TOTP, custom fields) is left intact. Use --field-remove to drop a custom field.
Flags: `--name` New name · `--username` Login username · `--password` Login password · `--uri` Login URL · `--totp` TOTP secret · `--notes` Notes · `--notes-file` Read notes from a UTF-8 file (use for notes > ~15KB — Windows command-line cap) · `--folder` Folder name (created if needed) · `--field` Custom text field(s) — comma-separated name=value (e.g. --field lan_ip=10.0.0.1,rack=A3). On update, upserts by name. · `--field-hidden` Custom hidden field(s) — comma-separated name=value. Stored as a secret (masked in the UI like a password). · `--field-remove` Custom field name(s) to remove — comma-separated (e.g. --field-remove old_ip,legacy_token). · `--confirm` Confirm the update · `--vault` Named vault profile (omit for default)
```bash
its bw items update <id> --field lan_ip=10.0.0.2 --confirm
its bw items update <id> --field-remove lan_ip --confirm
its bw items update <item-id> --password "NewP@ss"
its bw items update <item-id> --password "NewP@ss" --json
```

### `its bw items move <id>`
Move vault items to a folder. Move an item between folders. --confirm required.
Flags: `--folder` Destination folder name (created if needed) · `--confirm` Confirm the move · `--vault` Named vault profile (omit for default)
```bash
its bw items move <item-id> --folder "Servers"
its bw items move <item-id> --folder "Servers" --json
```

### `its bw items delete <id>`
Move a vault item to trash (soft-delete, recoverable). Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record.
Flags: `--confirm` Confirm the deletion · `--vault` Named vault profile (omit for default)
```bash
its bw items delete <item-id> --confirm
```

### `its bw items restore <id>`
Restore a vault item from the trash. Restore a soft-deleted item from trash.
Flags: `--confirm` Confirm the restore · `--vault` Named vault profile (omit for default)
```bash
its bw items restore <item-id>
its bw items restore <item-id> --json
```

### `its bw items purge <id>`
PERMANENTLY delete a vault item. This CANNOT be undone.
Flags: `--confirm` Confirm permanent deletion (REQUIRED) · `--yes-permanently-delete` Double-confirm that you understand this is irreversible · `--vault` Named vault profile (omit for default)
```bash
its bw items purge <item-id> --confirm
```

## folders

### `its bw folders`
List all vault folders. Surfaces the most common fields; pass --json for raw shape.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw folders
its bw folders --json
its bw folders --watch
```

### `its bw folders get <name>`
List items in a folder by name. Pass the id (or any natural identifier) as the positional arg.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw folders get "Servers"
its bw folders get "Servers" --json
```

### `its bw folders summary`
List folders with item counts. Quick one-screen view — designed for dashboards / `--watch`.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw folders summary
its bw folders summary --json
its bw folders summary --watch
```

### `its bw folders create <name>`
Create a new folder. Idempotent on duplicate names — use update/edit to mutate an existing record.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw folders create "Servers"
its bw folders create "Servers" --json
```

### `its bw folders delete <name>`
Delete a folder (items in it are moved to No Folder, not deleted).
Flags: `--confirm` Confirm folder deletion · `--vault` Named vault profile (omit for default)
```bash
its bw folders delete "Old stuff" --confirm
```

## password

### `its bw password <query>`
Get the password for an item by search query. Surfaces the most common fields; pass --json for raw shape.
Flags: `--copy` Copy the secret to the OS clipboard instead of printing it. Auto-clears after --clear-after seconds. · `--clear-after` Seconds before the clipboard is wiped (0 disables). Only meaningful with --copy. · `--vault` Named vault profile (omit for default)
```bash
its bw password "server-login" --include-secrets
its bw password "server-login" --copy
its bw password "server-login"
its bw password "server-login" --copy
its bw password "server-login" --watch
```

## profile

### `its bw profile`
Show vault profile information. Surfaces the most common fields; pass --json for raw shape.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw profile
its bw profile --json
its bw profile --watch
```

## dashboard

### `its bw dashboard`
Vault summary statistics. Surfaces the most common fields; pass --json for raw shape.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw dashboard
its bw dashboard --json
its bw dashboard --watch
```

## pin

### `its bw pin reset`
Change the PIN used to encrypt the master password. Drop the resource's state — use --confirm.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw pin reset
its bw pin reset --json
```

## session

### `its bw session unlock`
Unlock vault — skip PIN prompt for subsequent commands. Begin an interactive session — see `bw session unlock`.
Flags: `--ttl` Session duration in minutes (default 480 = 8 hours) · `--vault` Named vault profile (omit for default)
```bash
its bw session unlock
its bw session unlock --json
```

### `its bw session lock`
Lock vault and destroy the active session. End the current session.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw session lock
its bw session lock --json
```

### `its bw session`
Check if a vault session is active. Surfaces the most common fields; pass --json for raw shape.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw session list
its bw session list --json
its bw session list --watch
```

## vaults

### `its bw vaults`
List configured vault profiles. Surfaces the most common fields; pass --json for raw shape.
```bash
its bw vaults
its bw vaults --json
its bw vaults --watch
```

### `its bw vaults create <name>`
Save a named vault profile — its own host, account and master password (use for a second vault on a different server).
Flags: `--url` Server URL (defaults to current BW_URL) · `--email` Email (defaults to current BW_EMAIL) · `--client-id` API client ID (for API key auth) · `--client-secret` API client secret (for API key auth)
```bash
its bw vaults create "personal"
its bw vaults create "personal" --json
```

### `its bw vaults delete <name>`
Delete a named vault profile (local config only — does NOT touch the actual vault or its data).
Flags: `--confirm` Required to actually remove the profile
```bash
its bw vaults delete "personal" --confirm
```

## audit

### `its bw audit`
Full vault health audit (weak passwords, reuse, duplicates, cleanup issues).
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw audit
its bw audit --json
its bw audit --watch
```

### `its bw audit weak`
Find logins with weak passwords. Identifies weak passwords; pair with `bw audit reused`.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw audit weak
its bw audit weak --json
```

### `its bw audit reused`
Find passwords reused across multiple logins. Identifies passwords shared across multiple items.
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw audit reused
its bw audit reused --json
```

### `its bw audit exposed`
Check passwords against Have I Been Pwned breaches (k-anonymity safe).
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw audit exposed
its bw audit exposed --json
```

### `its bw audit duplicates`
Detect duplicate logins (domain+username, name+username matching).
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw audit duplicates
its bw audit duplicates --json
```

### `its bw audit unfiled`
Vault items with no folder assigned (hygiene issue). Items with no folder assignment.
Flags: `--type` Filter by type: login, note, card, identity · `--vault` Named vault profile (omit for default)
```bash
its bw audit unfiled
its bw audit unfiled --json
```

### `its bw audit cleanup`
Detect vault hygiene issues (skeleton logins, missing fields, empty items).
Flags: `--vault` Named vault profile (omit for default)
```bash
its bw audit cleanup
its bw audit cleanup --json
```

### `its bw audit vault-report`
One-shot vault hygiene snapshot — counts, unfiled breakdown, weak/reused/duplicates, and per-folder coverage. Composes audit weak/reused/duplicates/unfiled so the numbers stay in sync with the individual commands.
Flags: `--min-items` Only show folders with this many items or more (default 1) · `--vault` Named vault profile (omit for default)
```bash
its bw audit vault-report
its bw audit vault-report --json
```

## doctor

### `its bw doctor`
Local health check — vault profiles, active sessions, 2FA-remember token age, master-password store presence. No network calls.
```bash
its bw doctor
its bw doctor --json
its bw doctor --watch
```
