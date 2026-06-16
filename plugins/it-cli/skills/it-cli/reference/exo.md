# Exchange Online (`exo`)

Exchange Online — distribution groups, shared mailboxes, permissions, mail flow rules, accepted domains.

> Auto-generated reference. Configure: `its exo setup`. For a command you can name, prefer live help `its exo <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## groups

### `its exo groups`
List all distribution groups. Surfaces the most common fields; pass --json for raw shape.
```bash
its exo groups
its exo groups --json
its exo groups --watch
```

### `its exo groups get <group>`
Get distribution group details. Pass the id (or any natural identifier) as the positional arg.
```bash
its exo groups get "All Staff"
its exo groups get "All Staff" --json
```

### `its exo groups members <group>`
List members of a distribution group. Returns direct members; nested groups aren't expanded.
```bash
its exo groups members "All Staff"
its exo groups members "All Staff" --json
```

### `its exo groups create <name>`
Create a new distribution group. Idempotent on duplicate names — use update/edit to mutate an existing record.
Flags: `--alias` Email alias (before @domain) · `--managed-by` Owner email address · `--type` Group type
```bash
its exo groups create "Marketing Team" --type Distribution --primary-smtp marketing@example.com
its exo groups create "Marketing Team" --type Distribution --primary-smtp marketing@example.com --json
```

### `its exo groups delete <group>`
Delete a distribution group. Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record.
Flags: `--confirm` Required to perform this destructive deletion
```bash
its exo groups delete sales@thf.co.uk --confirm
its exo groups delete "Old DL" --confirm
```

### `its exo groups add-member <group> <member>`
Add a member to a distribution group. Idempotent — already-a-member is a no-op.
```bash
its exo groups add-member "All Staff" --user jane.smith@example.com
its exo groups add-member "All Staff" --user jane.smith@example.com --json
```

### `its exo groups remove-member <group> <member>`
Remove a member from a distribution group. Reverse of add-member. --confirm required.
Flags: `--confirm` Required to remove the member
```bash
its exo groups remove-member sales@thf.co.uk jo@thf.co.uk --confirm
its exo groups remove-member "All Staff" jane.smith@example.com
its exo groups remove-member "All Staff" jane.smith@example.com --json
```

## mailboxes

### `its exo mailboxes`
List mailboxes (optionally filtered by type). Surfaces the most common fields; pass --json for raw shape.
Flags: `--type` Mailbox type filter
```bash
its exo mailboxes
its exo mailboxes --type SharedMailbox
its exo mailboxes --type RoomMailbox
```

### `its exo mailboxes get <mailbox>`
Get mailbox details (accepts partial name, alias, or email).
```bash
its exo mailboxes get jane.smith@example.com
its exo mailboxes get jane.smith@example.com --json
```

### `its exo mailboxes stats <mailbox>`
Get mailbox size and item statistics. Aggregated statistics — counts, totals, percentiles.
```bash
its exo mailboxes stats jane.smith@example.com
its exo mailboxes stats jane.smith@example.com --json
```

### `its exo mailboxes create <name> <alias>`
Create a shared mailbox. Idempotent on duplicate names — use update/edit to mutate an existing record.
```bash
its exo mailboxes create --shared --upn support@example.com --name "Support"
its exo mailboxes create --shared --upn support@example.com --name "Support" --json
```

### `its exo mailboxes permissions <mailbox>`
List non-inherited permissions on a mailbox. Returns access grants on the resource.
```bash
its exo mailboxes permissions shared@example.com
its exo mailboxes permissions shared@example.com --json
```

### `its exo mailboxes add-permission <mailbox> <user>`
Grant mailbox access to a user. Grant a new permission. Idempotent.
Flags: `--rights` Access rights to grant
```bash
its exo mailboxes add-permission shared@example.com --user jane.smith@example.com --rights FullAccess
its exo mailboxes add-permission shared@example.com --user jane.smith@example.com --rights FullAccess --json
```

### `its exo mailboxes remove-permission <mailbox> <user>`
Remove mailbox access from a user. Revoke a permission. --confirm required.
Flags: `--rights` Access rights to revoke · `--confirm` Required to revoke the permission
```bash
its exo mailboxes remove-permission shared@thf.co.uk jo@thf.co.uk --confirm
its exo mailboxes remove-permission shared@example.com jane.smith@example.com
its exo mailboxes remove-permission shared@example.com jane.smith@example.com --json
```

### `its exo mailboxes forwarding <mailbox>`
Show forwarding configuration for a mailbox. Audit pass — find mailboxes with forwarding configured.
```bash
its exo mailboxes forwarding jane.smith@example.com
its exo mailboxes forwarding jane.smith@example.com --json
```

### `its exo mailboxes user-access <user>`
List all shared mailboxes a user has FullAccess to (scans all 400+ shared mailboxes, may take up to 5 minutes).
```bash
its exo mailboxes user-access jane.smith@example.com
its exo mailboxes user-access jane.smith@example.com --json
```

### `its exo mailboxes set-forwarding <mailbox> <target>`
Configure mailbox forwarding. Set a mailbox forward. --confirm required.
Flags: `--keep-copy` Also deliver to the original mailbox · `--confirm` Required to set forwarding (mail-exfiltration risk)
```bash
its exo mailboxes set-forwarding jo@thf.co.uk manager@thf.co.uk --confirm
its exo mailboxes set-forwarding jane.smith@example.com --to "external@vendor.com" --keep
its exo mailboxes set-forwarding jane.smith@example.com --to "external@vendor.com" --keep --json
```

### `its exo mailboxes set-type <mailbox> <type>`
Convert a mailbox between user and shared (Set-Mailbox -Type). Common at offboarding — flip a leaver's mailbox to Shared.
Flags: `--confirm` Required to change the mailbox type
```bash
its exo mailboxes set-type jo@thf.co.uk SharedMailbox --confirm
```

### `its exo mailboxes set-visibility <mailbox>`
Hide or show a mailbox in the global address list (GAL). Pass exactly one of --hide / --show.
Flags: `--hide` Hide from the GAL · `--show` Show in the GAL

## rules

### `its exo rules`
List mail flow (transport) rules. Surfaces the most common fields; pass --json for raw shape.
```bash
its exo rules
its exo rules
its exo rules --json
its exo rules --watch
```

### `its exo rules get <name>`
Get one transport rule's full condition/action shape (Get-TransportRule). Flags mail-exfiltration verbs (RedirectMessageTo, BlindCopyTo) if present.
```bash
its exo rules get "External forward block"
```

### `its exo rules audit`
Audit every transport rule for mail-exfiltration verbs (RedirectMessageTo, BlindCopyTo, AddToRecipients, CopyTo). A silent redirect/blind-copy to an external address is a classic data-exfiltration tripwire — review every flagged rule.
```bash
its exo rules audit
its exo rules audit --json
```

## domains

### `its exo domains`
List accepted email domains. Surfaces the most common fields; pass --json for raw shape.
```bash
its exo domains
its exo domains --json
its exo domains --watch
```

## trace

### `its exo trace`
Search message trace (last N days). Surfaces the most common fields; pass --json for raw shape.
Flags: `--sender` Sender email address · `--recipient` Recipient email address · `--subject` Subject contains (case-insensitive) · `--days` Number of days to search back
```bash
its exo trace --sender jane.smith@example.com --since 24h
its exo trace --recipient external@vendor.com --since 7d
its exo trace --sender jane.smith@example.com --since 24h --watch
```

### `its exo trace detail <trace-id> <recipient>`
Get hop-by-hop detail for a traced message. Single mailbox detail incl. quotas + rules.
```bash
its exo trace detail <msg-id>
its exo trace detail <msg-id> --json
```

### `its exo trace historical`
Submit a historical message-trace search (Start-HistoricalSearch) for the 11–90 day window that `trace list` (10-day cap) can't reach. Async — returns a JobId; poll with `trace historical-status <jobId>`.
Flags: `--sender` Sender email address · `--recipient` Recipient email address · `--days` Days back to search (max 90) · `--notify` Email address to notify when the CSV is ready (required by EXO)

### `its exo trace historical-status <job-id>`
Poll a historical search job (Get-HistoricalSearch). FileUrl is populated once Status is Done.

## autoreply

### `its exo autoreply get <mailbox>`
Check out-of-office / auto-reply status for a mailbox. Pass the id (or any natural identifier) as the positional arg.
```bash
its exo autoreply get jane.smith@example.com
its exo autoreply get jane.smith@example.com --json
```

### `its exo autoreply enable <mailbox>`
Enable out-of-office auto-reply. Switch to active state. Idempotent.
Flags: `--internal` Internal message (HTML or plain text) · `--external` External message (HTML or plain text) · `--start` Start time for scheduled OOF (e.g. 2026-03-20T09:00) · `--end` End time for scheduled OOF (e.g. 2026-03-25T17:00)
```bash
its exo autoreply enable jane.smith@example.com --internal "Out of office until Monday" --external "On leave — contact support@example.com"
its exo autoreply enable jane.smith@example.com --internal "Out of office until Monday" --external "On leave — contact support@example.com" --json
```

### `its exo autoreply disable <mailbox>`
Disable out-of-office auto-reply. Switch to inactive state. Idempotent.
```bash
its exo autoreply disable jane.smith@example.com
its exo autoreply disable jane.smith@example.com --json
```

## recipients

### `its exo recipients search <query>`
Search all recipient types (users, groups, contacts, shared mailboxes).
```bash
its exo recipients search "jane"
its exo recipients search "jane" --json
```

### `its exo recipients send-as <identity>`
List Send-As permissions on a mailbox or group. Returns SendAs delegates for a mailbox.
```bash
its exo recipients send-as shared@example.com
its exo recipients send-as shared@example.com --json
```

### `its exo recipients add-send-as <identity> <trustee>`
Grant Send-As permission. Idempotent — already-granted delegates are skipped.
```bash
its exo recipients add-send-as shared@example.com jane.smith@example.com
its exo recipients add-send-as shared@example.com jane.smith@example.com --json
```

### `its exo recipients remove-send-as <identity> <trustee>`
Revoke Send-As permission. Reverse of add-send-as.
Flags: `--confirm` Required to revoke the Send-As permission
```bash
its exo recipients remove-send-as shared@thf.co.uk jo@thf.co.uk --confirm
its exo recipients remove-send-as shared@example.com jane.smith@example.com
its exo recipients remove-send-as shared@example.com jane.smith@example.com --json
```

## forwarding

### `its exo forwarding check [upn]`
Audit auto-forwarding posture — transport rules + outbound spam policies (+ optional mailbox).
```bash
its exo forwarding check
its exo forwarding check --json
```
