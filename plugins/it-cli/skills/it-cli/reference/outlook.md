# Outlook (`outlook`)

Microsoft Outlook (Graph) mailbox + calendar — list/search/send mail, manage drafts, organise folders, schedule events, check free/busy, configure auto-reply and inbox rules.

> Auto-generated reference. Configure: `its outlook setup`. For a command you can name, prefer live help `its outlook <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## mail

### `its outlook mail`
List messages from the mailbox. Defaults to Inbox (top 25 by receivedDateTime desc). Use --folder for a specific folder, --filter for OData expressions, --search to switch to keyword search.
Flags: `--folder` Folder ID or well-known name (inbox, sentitems, drafts, deleteditems, archive) · `--top` Number of messages (max 50) · `--skip` Skip first N messages (pagination) · `--filter` OData $filter expression · `--search` KQL-style search query (alternative to --filter) · `--unread` Only unread messages · `--has-attachments` Only messages with attachments · `--from` Filter by sender email address (substring match via OData) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.
```bash
its outlook mail
its outlook mail --unread
its outlook mail --from boss@example.com
its outlook mail --search invoice
its outlook mail --folder sentitems
```

### `its outlook mail get <message_id>`
Get a single message including body, recipients, and flags.
Flags: `--no-body` Skip the body content (faster, smaller output) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook mail headers <message_id>`
Show a message's internet headers + parsed antispam verdict (SCL/SFV/CAT/BCL + spf/dkim/dmarc/compauth). The 'why did this land in Junk' tool. Add --all for every raw header.
Flags: `--all` Show every raw internet header, not just the antispam summary · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook mail search <query>`
Keyword search across the mailbox using Graph $search (KQL syntax).
Flags: `--top` Max results (max 50) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.
```bash
its outlook mail search "subject:invoice"
its outlook mail search "from:boss@example.com"
its outlook mail search renewal
```

### `its outlook mail thread <conversation_id>`
List every message in the same conversation (entire thread).
Flags: `--top` Max messages (max 50) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook mail move <message_id> <folder_id>`
Move a message to another folder.
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook mail copy <message_id> <folder_id>`
Copy a message to another folder.
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook mail read <message_id>`
Mark a message as read.
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook mail unread <message_id>`
Mark a message as unread.
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook mail flag <message_id>`
Set follow-up flag status on a message.
Flags: `--status` Flag status · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook mail categorise <message_id> <categories>`
Set the category list on a message (replaces existing categories).
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook mail delete [message_id]`
Delete one or more messages (moves to Deleted Items). Pass a single id positionally, or pipe `mail list --json` to stdin with --stdin for bulk delete.
Flags: `--confirm` Required for bulk (--stdin) deletes · `--stdin` Read newline/JSON list of message ids from stdin · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.
```bash
its outlook mail delete <message_id>
its outlook mail --filter "from/emailAddress/address eq 'spammer@x'" --json | its outlook mail delete --stdin --confirm
```

### `its outlook mail send`
Send a new email directly (no draft step). Saves a copy in Sent Items.
Flags: `--to` Comma-separated recipients (Name <addr> or addr) · `--cc` Comma-separated CC recipients · `--bcc` Comma-separated BCC recipients · `--subject` Subject line · `--body` Body content · `--body-file` Read body from a UTF-8 file (use for bodies > ~15KB — Windows command-line cap) · `--html` Treat --body / --body-file as HTML (default text) · `--importance` low|normal|high · `--attach` File to attach. Comma-separated for multiple. `path:cid:<id>` syntax marks an attachment inline with that cid (pair with `<img src="cid:<id>">` in body). · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me. · `--var` Template substitution — `--var k1=v1,k2=v2`. Substitutes `${key}` in body/comment after --body-file read.
```bash
its outlook mail send --to user@example.com --subject "Hi" --body "Quick note"
its outlook mail send --to user@example.com --subject "Report" --html --body-file report.html
its outlook mail send --to user@example.com --subject "Welcome" --html --body-file welcome.html --attach "logo.png:cid:logo-1"
```

## drafts

### `its outlook drafts create`
Create a draft email (does not send).
Flags: `--to` Comma-separated recipients · `--cc` CC recipients · `--bcc` BCC recipients · `--subject` Subject line · `--body` Body content · `--body-file` Read body from a UTF-8 file (use this for bodies > ~15KB — Windows command-line cap) · `--html` Treat --body / --body-file as HTML (default text) · `--importance` low|normal|high · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me. · `--var` Template substitution — `--var k1=v1,k2=v2`. Substitutes `${key}` in body/comment after --body-file read.

### `its outlook drafts reply <message_id>`
Create a reply draft. By default replies to the sender only; use --all to Reply-All. Does not send — use `drafts send <id>` after edits.
Flags: `--comment` Inline comment prepended to the reply body · `--comment-file` Read --comment from a UTF-8 file (bypasses Windows ~32K command-line cap) · `--all` Reply to all recipients · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me. · `--var` Template substitution — `--var k1=v1,k2=v2`. Substitutes `${key}` in body/comment after --body-file read.

### `its outlook drafts forward <message_id>`
Create a forward draft.
Flags: `--to` Comma-separated recipients · `--comment` Inline comment prepended to the forwarded body · `--comment-file` Read --comment from a UTF-8 file (bypasses Windows ~32K command-line cap) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me. · `--var` Template substitution — `--var k1=v1,k2=v2`. Substitutes `${key}` in body/comment after --body-file read.

### `its outlook drafts update <draft_id>`
Patch an existing draft. Any of subject/body/to/cc/bcc/importance/categories.
Flags: `--subject` Replace subject · `--body` Replace body · `--body-file` Read replacement body from a UTF-8 file (use for bodies > ~15KB — Windows command-line cap) · `--html` Treat --body / --body-file as HTML · `--to` Replace To recipients (comma-separated) · `--cc` Replace CC recipients · `--bcc` Replace BCC recipients · `--importance` low|normal|high · `--categories` Replace categories (comma-separated) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me. · `--var` Template substitution — `--var k1=v1,k2=v2`. Substitutes `${key}` in body/comment after --body-file read.

### `its outlook drafts send <draft_id>`
Send an existing draft.
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook drafts`
List draft messages (convenience for `mail list --folder drafts`).
Flags: `--top` Number of drafts (max 50) · `--skip` Skip first N (pagination) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

## folders

### `its outlook folders`
List mail folders with counts (totalItemCount, unreadItemCount).
Flags: `--top` Max folders (default 50, max 100) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook folders get <folder_id>`
Get a single mail folder by ID or well-known name (inbox, sentitems, drafts, deleteditems, archive).
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook folders create <name>`
Create a new mail folder (optionally nested under a parent).
Flags: `--parent` Parent folder ID (omit for top-level) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

## attachments

### `its outlook attachments <message_id>`
List attachments on a message.
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook attachments get <message_id> [attachment_id]`
Get a single attachment (includes contentBytes for fileAttachment). Pass --save-all <dir> to dump every attachment on the message instead of one.
Flags: `--save` Write contentBytes of the single attachment to this local path (decoded) · `--save-all` Save every attachment on the message into this directory (created if missing) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.
```bash
its outlook attachments get <message_id> <attachment_id> --save invoice.pdf
its outlook attachments get <message_id> --save-all ./out
```

### `its outlook attachments add <message_id>`
Attach a file to a draft message. Pass --file <path> to read from disk, or --content-bytes (base64) directly.
Flags: `--file` Local file path — read + base64-encoded · `--name` Override attachment display name (defaults to basename of --file) · `--content-type` MIME type (default application/octet-stream) · `--content-bytes` Base64-encoded file content (alternative to --file) · `--is-inline` Mark as inline attachment (renders in body via cid: ref, not as paperclip) · `--content-id` Content-ID for cid: targeting in HTML body (e.g. `cid:logo-1` ↔ --content-id logo-1) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.
```bash
its outlook attachments add <draft_id> --file logo.png --content-type image/png --is-inline --content-id logo-1
```

### `its outlook attachments delete <message_id> [attachment_id]`
Delete attachment(s) from a message. Single: pass <message_id> <attachment_id>. Bulk: pass <message_id> and pipe `attachments list --json` to stdin with --stdin.
Flags: `--confirm` Required for bulk (--stdin) deletes · `--stdin` Read newline/JSON list of attachment ids from stdin · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.
```bash
its outlook attachments delete <message_id> <attachment_id> --confirm
its outlook attachments <message_id> --json | its outlook attachments delete <message_id> --stdin --confirm
```

## events

### `its outlook events`
List calendar events between two dates (calendarView — includes expanded recurrences).
Flags: `--start` Start ISO date/datetime (default: today) · `--end` End ISO date/datetime (default: 7 days from start) · `--top` Max events (default 50, max 100) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.
```bash
its outlook events
its outlook events --start 2026-05-25 --end 2026-06-01
```

### `its outlook events get <event_id>`
Get a single calendar event by ID.
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook events create`
Create a calendar event. Times default to UTC unless --tz is set.
Flags: `--subject` Event subject · `--start` Start datetime (ISO, e.g. 2026-05-25T09:00:00) · `--end` End datetime (ISO) · `--tz` Time zone (default UTC) · `--location` Location display name · `--body` Body content · `--body-file` Read body from a UTF-8 file (use for bodies > ~15KB — Windows command-line cap) · `--html` Treat --body / --body-file as HTML · `--attendees` Comma-separated email addresses (required) · `--optional-attendees` Comma-separated optional attendees · `--all-day` Mark as all-day event · `--online` Create Teams meeting link · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me. · `--var` Template substitution — `--var k1=v1,k2=v2`. Substitutes `${key}` in body/comment after --body-file read.

### `its outlook events update <event_id>`
Update a calendar event (subject, times, location, all-day, online-meeting).
Flags: `--subject` Replace subject · `--start` Replace start datetime (ISO) · `--end` Replace end datetime (ISO) · `--tz` Time zone for --start/--end (default UTC) · `--location` Replace location · `--body` Replace body · `--body-file` Read replacement body from a UTF-8 file (use for bodies > ~15KB — Windows command-line cap) · `--html` Treat --body / --body-file as HTML · `--all-day` Set isAllDay · `--online` Toggle isOnlineMeeting · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me. · `--var` Template substitution — `--var k1=v1,k2=v2`. Substitutes `${key}` in body/comment after --body-file read.

### `its outlook events delete <event_id>`
Delete a calendar event.
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook events respond <event_id> <response>`
Respond to a meeting invite — accept, decline, or tentatively accept.
Flags: `--comment` Reply comment to the organiser · `--comment-file` Read --comment from a UTF-8 file (bypasses Windows ~32K command-line cap) · `--no-response` Don't send a reply to the organiser · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me. · `--var` Template substitution — `--var k1=v1,k2=v2`. Substitutes `${key}` in body/comment after --body-file read.

### `its outlook events availability <schedules>`
Check free/busy across one or more mailboxes between two times.
Flags: `--start` Start ISO datetime · `--end` End ISO datetime · `--interval` View interval in minutes (default 30) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.
```bash
its outlook events availability "a@b.com,c@d.com" --start 2026-05-21T09:00:00Z --end 2026-05-21T17:00:00Z
```

## settings

### `its outlook settings get`
Get full mailbox settings (time zone, locale, working hours, auto-reply state).
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

## autoreply

### `its outlook autoreply get`
Get current automatic reply (out-of-office) settings.
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook autoreply set`
Configure automatic reply. Use --status to toggle disabled/alwaysEnabled/scheduled. For scheduled, pass --start and --end.
Flags: `--status` disabled | alwaysEnabled | scheduled · `--audience` External audience: none | contactsOnly | all · `--internal` Internal reply message · `--external` External reply message · `--start` Scheduled start (ISO datetime) · `--end` Scheduled end (ISO datetime) · `--tz` Time zone for --start/--end (default UTC) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.
```bash
its outlook autoreply set --status disabled
its outlook autoreply set --status scheduled --start 2026-05-25T09:00:00 --end 2026-05-30T17:00:00 --tz "GMT Standard Time" --internal "Out of office, back Mon." --external "Out of office."
```

## categories

### `its outlook categories`
List master categories (named colour labels available for `mail categorise`).
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

## rules

### `its outlook rules`
List inbox message rules.
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook rules create`
Create an inbox rule. Conditions + actions take raw JSON (Graph schema). See https://learn.microsoft.com/graph/api/resources/messagerule for shape.
Flags: `--name` Display name · `--sequence` Run-order (lower runs first) · `--disabled` Create in disabled state · `--conditions` Conditions JSON (e.g. '{"fromAddresses":[{"emailAddress":{"address":"x@y"}}]}') · `--actions` Actions JSON (e.g. '{"moveToFolder":"<folderId>"}') · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

### `its outlook rules delete <rule_id>`
Delete an inbox rule.
Flags: `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

## contacts

### `its outlook contacts search <query>`
Search the /people graph for matching contacts and frequent collaborators.
Flags: `--top` Max results (default 10, max 25) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.

## triage

### `its outlook triage`
Triage the unread inbox — buckets messages into ACTION REQUIRED / FYI / NOISE with a one-line recommendation. Read-only; no mutations.
Flags: `--top` Max unread messages to scan (default 25, max 50) · `--include-read` Also include read messages (default: unread only) · `--user` Override mailbox UPN (app-only auth). Default: OUTLOOK_DEFAULT_USER or /me.
```bash
its outlook triage
its outlook triage --top 50
```
