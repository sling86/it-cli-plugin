# GitHub (`gh`)

GitHub via the local `gh` CLI. Piggybacks on the user's existing `gh auth` — no PAT needed. Today: standard branch-protection (block), per-repo webhook setup, webhook list.

> Auto-generated reference. Configure: `its gh setup`. For a command you can name, prefer live help `its gh <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## branch-protect

### `its gh branch-protect apply <repo>`
Apply the standard branch-protection block to <owner/repo> on the named branch (default `main`). 1 approving review, dismiss stale, no force push, no delete, conversation resolution required.
Flags: `--branch` Branch to protect · `--dry-run` Print the planned PUT body without sending
```bash
its gh branch-protect apply acme/storefront --dry-run
its gh branch-protect apply acme/storefront --branch main
```

### `its gh branch-protect show <repo>`
Show current branch-protection settings for <owner/repo>@<branch>. Read-only — no mutation.
Flags: `--branch` Branch

### `its gh branch-protect remove <repo>`
Remove ALL branch protection from <owner/repo>@<branch> (default `main`). Permanent — use --confirm. This does not restore the previous rules; re-apply with `branch-protect apply`.
Flags: `--branch` Branch to unprotect (default main) · `--confirm` Required to perform this destructive removal
```bash
its gh branch-protect remove acme/storefront --confirm
```

## webhook

### `its gh webhook setup <repo> <url>`
Create a push-event webhook on <owner/repo> pointing at <url>. Idempotent — bails if a hook with the same URL already exists.
Flags: `--secret` Webhook secret (defaults to a random secret, printed once - save it) · `--events` Comma-separated events (default: push)
```bash
its gh webhook setup acme/storefront https://dok.example.com/api/deploy --events push
```

### `its gh webhook <repo>`
List webhooks on <owner/repo>.

### `its gh webhook delete <repo> <hook>`
Delete a webhook from <owner/repo> by id (from `webhook list`) or by its exact URL. Permanent — use --confirm. Deliveries stop immediately; GitHub keeps no undo.
Flags: `--confirm` Required to perform this destructive deletion
```bash
its gh webhook delete acme/storefront 12345678 --confirm
its gh webhook delete acme/storefront https://dok.example.com/api/deploy --confirm
```
