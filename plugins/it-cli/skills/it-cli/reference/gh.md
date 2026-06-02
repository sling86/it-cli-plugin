# GitHub (`gh`)

GitHub via the local `gh` CLI. Piggybacks on the user's existing `gh auth` — no PAT needed. Today: standard branch-protection (block), per-repo webhook setup, webhook list..

> Auto-generated command reference. Do not edit by hand.
> Configure with `its gh setup`; get live help with `its gh <resource> help`.

[← Provider index](./index.md)

## Resources

- [branch-protect](#branch-protect)
- [webhook](#webhook)

### branch-protect

| Command | Description |
|---------|-------------|
| `its gh branch-protect apply <repo>` | Apply the standard branch-protection block to <owner/repo> on the named branch (default `main`). 1 approving review, dismiss stale, no force push, no delete, conversation resolution required. |
| `its gh branch-protect show <repo>` | Show current branch-protection settings for <owner/repo>@<branch>. Read-only — no mutation. |

#### `its gh branch-protect apply <repo>`

Apply the standard branch-protection block to <owner/repo> on the named branch (default `main`). 1 approving review, dismiss stale, no force push, no delete, conversation resolution required.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--branch` | `` | Branch to protect | main |
| `--dry-run` | `` | Print the planned PUT body without sending | — |

#### `its gh branch-protect show <repo>`

Show current branch-protection settings for <owner/repo>@<branch>. Read-only — no mutation.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--branch` | `` | Branch | main |

---

### webhook

| Command | Description |
|---------|-------------|
| `its gh webhook setup <repo> <url>` | Create a push-event webhook on <owner/repo> pointing at <url>. Idempotent — bails if a hook with the same URL already exists. |
| `its gh webhook <repo>` | List webhooks on <owner/repo> |

#### `its gh webhook setup <repo> <url>`

Create a push-event webhook on <owner/repo> pointing at <url>. Idempotent — bails if a hook with the same URL already exists.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--secret` | `` | Webhook secret (defaults to a stable string per URL) | — |
| `--events` | `` | Comma-separated events (default: push) | push |

#### `its gh webhook <repo>`

List webhooks on <owner/repo>.

---
