# Bitwarden (`bw`)

Bitwarden vault — search items, get passwords, browse folders.

> Auto-generated command reference. Do not edit by hand.
> Configure with `its bw setup`; get live help with `its bw <resource> help`.

[← Provider index](./index.md)

## Resources

- [items](#items)
- [folders](#folders)
- [password](#password)
- [profile](#profile)
- [dashboard](#dashboard)
- [pin](#pin)
- [session](#session)
- [vaults](#vaults)
- [audit](#audit)
- [doctor](#doctor)

### items

| Command | Description |
|---------|-------------|
| `its bw items` | List all vault items. Surfaces the most common fields; pass --json for raw shape. |
| `its bw items search <query>` | Search vault items by name, username, URL, or notes. Substring match across the most relevant fields; case-insensitive. |
| `its bw items get <id>` | Get a vault item by ID (includes password and fields). Pass the id (or any natural identifier) as the positional arg. |
| `its bw items totp <query>` | Generate current TOTP code for an item. Returns the current TOTP code — refresh every 30s. |
| `its bw items trash` | List trashed vault items. Returns soft-deleted items in the trash bin. |
| `its bw items recent` | List recently modified vault items. Returns the N most recently modified items. |
| `its bw items favourites` | List favourite vault items. Items the user has starred. |
| `its bw items create <name>` | Create a new vault item (login, note, card, or identity). Idempotent on duplicate names — use update/edit to mutate an existing record. |
| `its bw items update <id>` | Update a vault item (all fields are replaced — omitted fields are cleared) |
| `its bw items move <id>` | Move vault items to a folder. Move an item between folders. --confirm required. |
| `its bw items delete <id>` | Move a vault item to trash (soft-delete, recoverable). Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record. |
| `its bw items restore <id>` | Restore a vault item from the trash. Restore a soft-deleted item from trash. |
| `its bw items purge <id>` | PERMANENTLY delete a vault item. This CANNOT be undone. |

#### `its bw items`

List all vault items. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--type` | `` | Filter by type (login/note/card/identity) | — |
| `--folder` | `` | Filter by folder name | — |
| `--favourite` | `` | Show only favourites | — |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw items

its bw items --collection "Servers"

# Re-runs every 10s — handy for dashboards or incident response.
its bw items --watch
```

#### `its bw items search <query>`

Search vault items by name, username, URL, or notes. Substring match across the most relevant fields; case-insensitive.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw items search "github"

# Pipe-friendly output — use with jq / scripts.
its bw items search "github" --json
```

#### `its bw items get <id>`

Get a vault item by ID (includes password and fields). Pass the id (or any natural identifier) as the positional arg.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--copy` | `-c` | Copy the secret to the OS clipboard instead of printing it. Auto-clears after --clear-after seconds. | — |
| `--clear-after` | `` | Seconds before the clipboard is wiped (0 disables). Only meaningful with --copy. | 30 |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw items get "Server admin"

# Pipe-friendly output — use with jq / scripts.
its bw items get "Server admin" --json
```

#### `its bw items totp <query>`

Generate current TOTP code for an item. Returns the current TOTP code — refresh every 30s.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--copy` | `-c` | Copy the secret to the OS clipboard instead of printing it. Auto-clears after --clear-after seconds. | — |
| `--clear-after` | `` | Seconds before the clipboard is wiped (0 disables). Only meaningful with --copy. | 30 |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
# Generate current TOTP — refreshes every 30s
its bw items totp "Server admin"

# Pipe-friendly output — use with jq / scripts.
its bw items totp "Server admin" --json
```

#### `its bw items trash`

List trashed vault items. Returns soft-deleted items in the trash bin.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw items trash

# Pipe-friendly output — use with jq / scripts.
its bw items trash --json
```

#### `its bw items recent`

List recently modified vault items. Returns the N most recently modified items.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--days` | `` | Look-back period in days | 7 |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw items recent

# Pipe-friendly output — use with jq / scripts.
its bw items recent --json
```

#### `its bw items favourites`

List favourite vault items. Items the user has starred.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw items favourites

# Pipe-friendly output — use with jq / scripts.
its bw items favourites --json
```

#### `its bw items create <name>`

Create a new vault item (login, note, card, or identity). Idempotent on duplicate names — use update/edit to mutate an existing record.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--type` | `` | Item type: login (default), note, card, identity | — |
| `--username` | `` | Login username | — |
| `--password` | `` | Login password | — |
| `--uri` | `` | Login URL | — |
| `--totp` | `` | TOTP secret or otpauth URI | — |
| `--notes` | `` | Notes | — |
| `--notes-file` | `` | Read notes from a UTF-8 file (use for notes > ~15KB — Windows command-line cap) | — |
| `--folder` | `` | Folder name (created if it does not exist) | — |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw items create "Server admin" --username admin --password "P@ssw0rd" --url https://server.example.com

its bw items create "API keys" --type note --notes "stuff"
```

#### `its bw items update <id>`

Update a vault item (all fields are replaced — omitted fields are cleared).

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--name` | `` | New name | — |
| `--username` | `` | Login username | — |
| `--password` | `` | Login password | — |
| `--uri` | `` | Login URL | — |
| `--totp` | `` | TOTP secret | — |
| `--notes` | `` | Notes | — |
| `--notes-file` | `` | Read notes from a UTF-8 file (use for notes > ~15KB — Windows command-line cap) | — |
| `--folder` | `` | Folder name (created if needed) | — |
| `--confirm` | `` | Confirm the update | — |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw items update <item-id> --password "NewP@ss"

# Pipe-friendly output — use with jq / scripts.
its bw items update <item-id> --password "NewP@ss" --json
```

#### `its bw items move <id>`

Move vault items to a folder. Move an item between folders. --confirm required.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--folder` | `` | Destination folder name (created if needed) | — |
| `--confirm` | `` | Confirm the move | — |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw items move <item-id> --folder "Servers"

# Pipe-friendly output — use with jq / scripts.
its bw items move <item-id> --folder "Servers" --json
```

#### `its bw items delete <id>`

Move a vault item to trash (soft-delete, recoverable). Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--confirm` | `` | Confirm the deletion | — |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
# Moves to trash, recoverable for 30 days
its bw items delete <item-id> --confirm
```

#### `its bw items restore <id>`

Restore a vault item from the trash. Restore a soft-deleted item from trash.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--confirm` | `` | Confirm the restore | — |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw items restore <item-id>

# Pipe-friendly output — use with jq / scripts.
its bw items restore <item-id> --json
```

#### `its bw items purge <id>`

PERMANENTLY delete a vault item. This CANNOT be undone.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--confirm` | `` | Confirm permanent deletion (REQUIRED) | — |
| `--yes-permanently-delete` | `` | Double-confirm that you understand this is irreversible | — |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
# CANNOT be undone
its bw items purge <item-id> --confirm
```

---

### folders

| Command | Description |
|---------|-------------|
| `its bw folders` | List all vault folders. Surfaces the most common fields; pass --json for raw shape. |
| `its bw folders get <name>` | List items in a folder by name. Pass the id (or any natural identifier) as the positional arg. |
| `its bw folders summary` | List folders with item counts. Quick one-screen view — designed for dashboards / `--watch`. |
| `its bw folders create <name>` | Create a new folder. Idempotent on duplicate names — use update/edit to mutate an existing record. |
| `its bw folders delete <name>` | Delete a folder (items in it are moved to No Folder, not deleted) |

#### `its bw folders`

List all vault folders. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw folders

# Pipe-friendly output — use with jq / scripts.
its bw folders --json

# Re-runs every 10s — handy for dashboards or incident response.
its bw folders --watch
```

#### `its bw folders get <name>`

List items in a folder by name. Pass the id (or any natural identifier) as the positional arg.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw folders get "Servers"

# Pipe-friendly output — use with jq / scripts.
its bw folders get "Servers" --json
```

#### `its bw folders summary`

List folders with item counts. Quick one-screen view — designed for dashboards / `--watch`.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw folders summary

# Pipe-friendly output — use with jq / scripts.
its bw folders summary --json

# Re-runs every 10s — handy for dashboards or incident response.
its bw folders summary --watch
```

#### `its bw folders create <name>`

Create a new folder. Idempotent on duplicate names — use update/edit to mutate an existing record.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw folders create "Servers"

# Pipe-friendly output — use with jq / scripts.
its bw folders create "Servers" --json
```

#### `its bw folders delete <name>`

Delete a folder (items in it are moved to No Folder, not deleted).

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--confirm` | `` | Confirm folder deletion | — |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw folders delete "Old stuff" --confirm
```

---

### password

| Command | Description |
|---------|-------------|
| `its bw password <query>` | Get the password for an item by search query. Surfaces the most common fields; pass --json for raw shape. |

#### `its bw password <query>`

Get the password for an item by search query. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--copy` | `-c` | Copy the secret to the OS clipboard instead of printing it. Auto-clears after --clear-after seconds. | — |
| `--clear-after` | `` | Seconds before the clipboard is wiped (0 disables). Only meaningful with --copy. | 30 |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
# Print the password (mask if shared terminal)
its bw password "server-login"

# Copy to clipboard, auto-clear after 30s
its bw password "server-login" --copy

# Print password (mask in shared terminals)
its bw password "server-login"

# Copy to clipboard, auto-clear after 30s
its bw password "server-login" --copy

# Re-runs every 10s — handy for dashboards or incident response.
its bw password "server-login" --watch
```

---

### profile

| Command | Description |
|---------|-------------|
| `its bw profile` | Show vault profile information. Surfaces the most common fields; pass --json for raw shape. |

#### `its bw profile`

Show vault profile information. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw profile

# Pipe-friendly output — use with jq / scripts.
its bw profile --json

# Re-runs every 10s — handy for dashboards or incident response.
its bw profile --watch
```

---

### dashboard

| Command | Description |
|---------|-------------|
| `its bw dashboard` | Vault summary statistics. Surfaces the most common fields; pass --json for raw shape. |

#### `its bw dashboard`

Vault summary statistics. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw dashboard

# Pipe-friendly output — use with jq / scripts.
its bw dashboard --json

# Re-runs every 10s — handy for dashboards or incident response.
its bw dashboard --watch
```

---

### pin

| Command | Description |
|---------|-------------|
| `its bw pin reset` | Change the PIN used to encrypt the master password. Drop the resource's state — use --confirm. |

#### `its bw pin reset`

Change the PIN used to encrypt the master password. Drop the resource's state — use --confirm.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
# Re-encrypts master password with a new PIN
its bw pin reset

# Pipe-friendly output — use with jq / scripts.
its bw pin reset --json
```

---

### session

| Command | Description |
|---------|-------------|
| `its bw session unlock` | Unlock vault — skip PIN prompt for subsequent commands. Begin an interactive session — see `bw session unlock`. |
| `its bw session lock` | Lock vault and destroy the active session. End the current session. |
| `its bw session` | Check if a vault session is active. Surfaces the most common fields; pass --json for raw shape. |

#### `its bw session unlock`

Unlock vault — skip PIN prompt for subsequent commands. Begin an interactive session — see `bw session unlock`.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--ttl` | `` | Session duration in minutes (default 480 = 8 hours) | — |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
# PIN-prompts, decrypts master password, stores session
its bw session unlock

# Pipe-friendly output — use with jq / scripts.
its bw session unlock --json
```

#### `its bw session lock`

Lock vault and destroy the active session. End the current session.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw session lock

# Pipe-friendly output — use with jq / scripts.
its bw session lock --json
```

#### `its bw session`

Check if a vault session is active. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw session list

# Pipe-friendly output — use with jq / scripts.
its bw session list --json

# Re-runs every 10s — handy for dashboards or incident response.
its bw session list --watch
```

---

### vaults

| Command | Description |
|---------|-------------|
| `its bw vaults` | List configured vault profiles. Surfaces the most common fields; pass --json for raw shape. |
| `its bw vaults create <name>` | Save the current default vault (or custom credentials) as a named profile |
| `its bw vaults delete <name>` | Delete a named vault profile (local config only — does NOT touch the actual vault or its data) |

#### `its bw vaults`

List configured vault profiles. Surfaces the most common fields; pass --json for raw shape.

**Examples:**

```bash
its bw vaults

# Pipe-friendly output — use with jq / scripts.
its bw vaults --json

# Re-runs every 10s — handy for dashboards or incident response.
its bw vaults --watch
```

#### `its bw vaults create <name>`

Save the current default vault (or custom credentials) as a named profile.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--url` | `` | Server URL (defaults to current BW_URL) | — |
| `--email` | `` | Email (defaults to current BW_EMAIL) | — |
| `--client-id` | `` | API client ID (for API key auth) | — |
| `--client-secret` | `` | API client secret (for API key auth) | — |

**Examples:**

```bash
its bw vaults create "personal"

# Pipe-friendly output — use with jq / scripts.
its bw vaults create "personal" --json
```

#### `its bw vaults delete <name>`

Delete a named vault profile (local config only — does NOT touch the actual vault or its data).

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--confirm` | `` | Required to actually remove the profile | — |

**Examples:**

```bash
# Local config only — doesn't touch the actual vault
its bw vaults delete "personal" --confirm
```

---

### audit

| Command | Description |
|---------|-------------|
| `its bw audit` | Full vault health audit (weak passwords, reuse, duplicates, cleanup issues) |
| `its bw audit weak` | Find logins with weak passwords. Identifies weak passwords; pair with `bw audit reused`. |
| `its bw audit reused` | Find passwords reused across multiple logins. Identifies passwords shared across multiple items. |
| `its bw audit exposed` | Check passwords against Have I Been Pwned breaches (k-anonymity safe) |
| `its bw audit duplicates` | Detect duplicate logins (domain+username, name+username matching) |
| `its bw audit unfiled` | Vault items with no folder assigned (hygiene issue). Items with no folder assignment. |
| `its bw audit cleanup` | Detect vault hygiene issues (skeleton logins, missing fields, empty items) |
| `its bw audit vault-report` | One-shot vault hygiene snapshot — counts, unfiled breakdown, weak/reused/duplicates, and per-folder coverage. Composes audit weak/reused/duplicates/unfiled so the numbers stay in sync with the individual commands. |

#### `its bw audit`

Full vault health audit (weak passwords, reuse, duplicates, cleanup issues).

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
# Weak passwords, reuse, duplicates, cleanup
its bw audit

# Pipe-friendly output — use with jq / scripts.
its bw audit --json

# Re-runs every 10s — handy for dashboards or incident response.
its bw audit --watch
```

#### `its bw audit weak`

Find logins with weak passwords. Identifies weak passwords; pair with `bw audit reused`.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw audit weak

# Pipe-friendly output — use with jq / scripts.
its bw audit weak --json
```

#### `its bw audit reused`

Find passwords reused across multiple logins. Identifies passwords shared across multiple items.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw audit reused

# Pipe-friendly output — use with jq / scripts.
its bw audit reused --json
```

#### `its bw audit exposed`

Check passwords against Have I Been Pwned breaches (k-anonymity safe).

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
# k-anonymity safe — sends only password hash prefix
its bw audit exposed

# Pipe-friendly output — use with jq / scripts.
its bw audit exposed --json
```

#### `its bw audit duplicates`

Detect duplicate logins (domain+username, name+username matching).

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
# Items with identical name/username
its bw audit duplicates

# Pipe-friendly output — use with jq / scripts.
its bw audit duplicates --json
```

#### `its bw audit unfiled`

Vault items with no folder assigned (hygiene issue). Items with no folder assignment.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--type` | `` | Filter by type: login, note, card, identity | — |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
# Items missing folder/collection
its bw audit unfiled

# Pipe-friendly output — use with jq / scripts.
its bw audit unfiled --json
```

#### `its bw audit cleanup`

Detect vault hygiene issues (skeleton logins, missing fields, empty items).

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
# Skeleton logins, missing fields, empty items
its bw audit cleanup

# Pipe-friendly output — use with jq / scripts.
its bw audit cleanup --json
```

#### `its bw audit vault-report`

One-shot vault hygiene snapshot — counts, unfiled breakdown, weak/reused/duplicates, and per-folder coverage. Composes audit weak/reused/duplicates/unfiled so the numbers stay in sync with the individual commands.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--min-items` | `` | Only show folders with this many items or more (default 1) | 1 |
| `--vault` | `` | Named vault profile (omit for default) | — |

**Examples:**

```bash
its bw audit vault-report

# Pipe-friendly output — use with jq / scripts.
its bw audit vault-report --json
```

---

### doctor

| Command | Description |
|---------|-------------|
| `its bw doctor` | Local health check — vault profiles, active sessions, 2FA-remember token age, master-password store presence. No network calls. |

#### `its bw doctor`

Local health check — vault profiles, active sessions, 2FA-remember token age, master-password store presence. No network calls.

**Examples:**

```bash
# Auth, session, 2FA, PIN status
its bw doctor

# Pipe-friendly output — use with jq / scripts.
its bw doctor --json

# Re-runs every 10s — handy for dashboards or incident response.
its bw doctor --watch
```

---
