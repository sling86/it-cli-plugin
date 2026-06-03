# GitHub (`gh`)

GitHub via the local `gh` CLI. Piggybacks on the user's existing `gh auth` — no PAT needed. Today: standard branch-protection (block), per-repo webhook setup, webhook list..

> Auto-generated reference. Configure: `its gh setup`. For a command you can name, prefer live help `its gh <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## branch-protect

### `its gh branch-protect apply <repo>`
Apply the standard branch-protection block to <owner/repo> on the named branch (default `main`). 1 approving review, dismiss stale, no force push, no delete, conversation resolution required.
Flags: `--branch` Branch to protect · `--dry-run` Print the planned PUT body without sending

### `its gh branch-protect show <repo>`
Show current branch-protection settings for <owner/repo>@<branch>. Read-only — no mutation.
Flags: `--branch` Branch

## webhook

### `its gh webhook setup <repo> <url>`
Create a push-event webhook on <owner/repo> pointing at <url>. Idempotent — bails if a hook with the same URL already exists.
Flags: `--secret` Webhook secret (defaults to a stable string per URL) · `--events` Comma-separated events (default: push)

### `its gh webhook <repo>`
List webhooks on <owner/repo>.
