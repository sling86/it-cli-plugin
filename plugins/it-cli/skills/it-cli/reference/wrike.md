# Wrike (`wrike`)

Wrike project management — IT tickets, tasks, contacts, spaces, folders, workflows, onboarding, leavers.

> Auto-generated reference. Configure: `its wrike setup`. For a command you can name, prefer live help `its wrike <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## tickets

### `its wrike tickets`
List IT support tickets. Surfaces the most common fields; pass --json for raw shape.
Flags: `--status` Filter by status · `--priority` Filter by priority/importance · `--limit` Maximum number of tickets
```bash
its wrike tickets
its wrike tickets --importance High
its wrike tickets --watch
```

### `its wrike tickets stats`
Ticket analytics — median resolve time, backlog, bus-factor risk by assignee.
Flags: `--since` Only include tickets created in the last N days (default 90)
```bash
its wrike tickets stats
its wrike tickets stats --json
```

### `its wrike tickets active`
List active IT tickets. Returns whichever record the API key currently targets.
```bash
its wrike tickets active
its wrike tickets active --json
```

### `its wrike tickets mine`
List tickets assigned to you (reads ITS_USER_EMAIL or --email).
Flags: `--status` Filter by status (default: Active) · `--email` Email to match on (overrides ITS_USER_EMAIL env var)
```bash
its wrike tickets mine
its wrike tickets mine --json
```

### `its wrike tickets get <idOrPermalink>`
Get full ticket details with comments. Pass the id (or any natural identifier) as the positional arg.
```bash
its wrike tickets get <task-id>
its wrike tickets get <task-id> --json
```

### `its wrike tickets search <query>`
Search IT/BC support tickets by title. Substring match across the most relevant fields; case-insensitive.
```bash
its wrike tickets search "printer"
its wrike tickets search "printer" --json
```

### `its wrike tickets create`
Create a new IT support ticket. Idempotent on duplicate names — use update/edit to mutate an existing record.
Flags: `--title` Ticket title (required) · `--description` Ticket description (inline plain text) · `--description-file` Read description body from a file path · `--description-stdin` Read description body from stdin · `--markdown` Treat description as Markdown (converts to Wrike-safe HTML). Mutually exclusive with --html. · `--html` Description is raw Wrike-safe HTML. Mutually exclusive with --markdown. · `--assignee` Comma-separated user IDs or names (e.g. 'KUAABCDE,Jane Smith') · `--due` Due date (YYYY-MM-DD) · `--importance` Importance (High, Normal, Low)
```bash
its wrike tickets create --title "Outlook crashes" --description "Repro: open shared mailbox"
its wrike tickets create --title "Outlook crashes" --description "Repro: open shared mailbox" --json
```

### `its wrike tickets set-due <taskId> <dueDate>`
Set due date on a ticket (YYYY-MM-DD). Set the task's due date.
```bash
its wrike tickets set-due <task-id> 2026-06-01
its wrike tickets set-due <task-id> 2026-06-01 --json
```

### `its wrike tickets assign <taskId> <userId>`
Add an assignee to a ticket. Idempotent — assigning twice is a no-op.
```bash
its wrike tickets assign <task-id> --user jane.smith@example.com
its wrike tickets assign <task-id> --user jane.smith@example.com --json
```

### `its wrike tickets update-title <taskId> <title>`
Change a ticket's title. Rename the task — keeps the same ID.
```bash
its wrike tickets update-title <task-id> "New title"
its wrike tickets update-title <task-id> "New title" --json
```

### `its wrike tickets update-importance <taskId> <importance>`
Change ticket importance (High, Normal, Low). Set Low / Normal / High importance.
```bash
its wrike tickets update-importance <task-id> High
its wrike tickets update-importance <task-id> High --json
```

### `its wrike tickets update-status <taskId> <status>`
Change ticket status. Move the task to a different workflow stage.
```bash
its wrike tickets update-status <task-id> --status "Completed"
its wrike tickets update-status <task-id> --status "Completed" --json
```

### `its wrike tickets update-description <taskId> [text]`
Replace a ticket's description. Same formatting rules as add-comment: plain text by default (\n → <br>, @Name auto-mentions), --markdown for bold/italic/links/bullets, --html for raw. Wrike renders <br>, <a>, <b>, <i>, and mention anchors only.
Flags: `--file` Read comment body from a file path · `--stdin` Read comment body from stdin · `--html` Send body as raw HTML. Wrike only renders <br>, <a>, <b>, <i>, and mention anchors — other tags are ignored. Use --markdown for richer formatting. · `--markdown` Convert Markdown to Wrike-safe HTML. Supported: **bold**, _italic_, `code` (rendered italic), # headings (rendered bold), [text](url), - bullets (flattened to •). In this mode, mention users with plain @Name (auto-resolves). Mutually exclusive with --html. Recommended over --html. · `--skip-mention-check` Bypass the @mention markup guard Use only when posting a literal string that happens to contain mention-like markup.
```bash
its wrike tickets update-description <task-id> --markdown "**Repro:** open Outlook → file → ..."
its wrike tickets update-description <task-id> --markdown "**Repro:** open Outlook → file → ..." --json
```

### `its wrike tickets add-comment <taskId> [text]`
Add a comment to a ticket. Prefer --markdown (bold/italic/links/bullets + plain @Name auto-resolves to mentions). Plain text also works (\n → <br>, @Name auto-resolved). Use --html only when you need raw anchor-based mentions; the mention markup guard rejects mismatched modes unless --skip-mention-check is set. Wrike only renders <br>, <a>, <b>, <i>, and mention anchors.
Flags: `--file` Read comment body from a file path · `--stdin` Read comment body from stdin · `--html` Send body as raw HTML. Wrike only renders <br>, <a>, <b>, <i>, and mention anchors — other tags are ignored. Use --markdown for richer formatting. · `--markdown` Convert Markdown to Wrike-safe HTML. Supported: **bold**, _italic_, `code` (rendered italic), # headings (rendered bold), [text](url), - bullets (flattened to •). In this mode, mention users with plain @Name (auto-resolves). Mutually exclusive with --html. Recommended over --html. · `--skip-mention-check` Bypass the @mention markup guard Use only when posting a literal string that happens to contain mention-like markup.
```bash
its wrike tickets add-comment <task-id> --text "Replicated, escalating"
its wrike tickets add-comment <task-id> --text "Replicated, escalating" --json
```

### `its wrike tickets update-comment <commentId> [text]`
Replace the body of an existing comment. Same formatting rules as add-comment (--markdown / --html / plain text). Use the commentId returned by add-comment, or read it from `tickets get`.
Flags: `--file` Read comment body from a file path · `--stdin` Read comment body from stdin · `--html` Send body as raw HTML. Wrike only renders <br>, <a>, <b>, <i>, and mention anchors — other tags are ignored. Use --markdown for richer formatting. · `--markdown` Convert Markdown to Wrike-safe HTML. Supported: **bold**, _italic_, `code` (rendered italic), # headings (rendered bold), [text](url), - bullets (flattened to •). In this mode, mention users with plain @Name (auto-resolves). Mutually exclusive with --html. Recommended over --html. · `--skip-mention-check` Bypass the @mention markup guard Use only when posting a literal string that happens to contain mention-like markup.
```bash
its wrike tickets update-comment <comment-id> --text "fixed typo"
its wrike tickets update-comment <comment-id> --text "fixed typo" --json
```

### `its wrike tickets delete-comment <commentId>`
Delete a comment by ID. Requires --confirm. Use the commentId from add-comment output or `tickets get`.
Flags: `--confirm` Confirm deletion (irreversible)
```bash
its wrike tickets delete-comment <comment-id> --confirm
its wrike tickets delete-comment <comment-id> --confirm --json
```

### `its wrike tickets narrative [taskId]`
Deep per-ticket narrative with recent comments + ball-is-with heuristic. Prioritises active chatter > waiting-on-them > waiting-on-us > ancient. Pass a single taskId, or --active / --mine for a batch view.
Flags: `--active` All active IT tickets (ignores --mine) · `--mine` Only tickets assigned to you (via ITS_USER_EMAIL or --email) · `--email` Email to match for --mine (overrides ITS_USER_EMAIL) · `--comments` Comments to show per ticket (default 3) · `--limit` Maximum tickets to process for batch modes (default 25)
```bash
its wrike tickets narrative <task-id>
its wrike tickets narrative <task-id> --json
```

### `its wrike tickets attachments <taskId>`
List attachments on a ticket. List attachments; pair with `attach`.
```bash
its wrike tickets attachments <task-id>
its wrike tickets attachments <task-id> --json
```

### `its wrike tickets download <taskId> [attachmentId]`
Download an attachment from a ticket. Stream the resource to a local file.
Flags: `--output` Output file path (defaults to original filename in cwd)
```bash
its wrike tickets download <task-id> <attachment-id> --out ./screenshot.png
its wrike tickets download <task-id> <attachment-id> --out ./screenshot.png --json
```

### `its wrike tickets attach <taskId> <filePath>`
Attach a file to a ticket. Upload a local file as an attachment.
```bash
its wrike tickets attach <task-id> --file ./screenshot.png
its wrike tickets attach <task-id> --file ./screenshot.png --json
```

## tasks

### `its wrike tasks <folder>`
List tasks in a folder or project (accepts name or ID). Surfaces the most common fields; pass --json for raw shape.
Flags: `--status` Filter by status · `--limit` Maximum number of tasks
```bash
its wrike tasks
its wrike tasks --json
its wrike tasks --watch
```

### `its wrike tasks search <query>`
Search tasks across all of Wrike (use --folder or --space to scope, accepts names).
Flags: `--folder` Scope to a folder/project (name or ID) · `--space` Scope to a space (name or ID)
```bash
its wrike tasks search "deploy"
its wrike tasks search "deploy" --json
```

### `its wrike tasks get <idOrPermalink>`
Get full task details with comments. Pass the id (or any natural identifier) as the positional arg.
```bash
its wrike tasks get https://www.wrike.com/open.htm?id=...
its wrike tasks get https://www.wrike.com/open.htm?id=... --json
```

### `its wrike tasks create <folder>`
Create a new task in a folder or project (accepts name or ID).
Flags: `--title` Task title · `--description` Task description · `--description-file` Read description from a UTF-8 file (use for descriptions > ~15KB — Windows command-line cap) · `--status` Task status · `--importance` Task importance (High, Normal, Low)
```bash
its wrike tasks create "My folder" --title "New task"
its wrike tasks create "My folder" --title "New task" --json
```

### `its wrike tasks set-due <taskId> <dueDate>`
Set due date on a task (YYYY-MM-DD). Set the task's due date.
```bash
its wrike tasks set-due <task-id> 2026-06-01
its wrike tasks set-due <task-id> 2026-06-01 --json
```

### `its wrike tasks update-title <taskId> <title>`
Change a task's title. Rename the task — keeps the same ID.
```bash
its wrike tasks update-title <task-id> "New title"
its wrike tasks update-title <task-id> "New title" --json
```

### `its wrike tasks update-importance <taskId> <importance>`
Change task importance (High, Normal, Low). Set Low / Normal / High importance.
```bash
its wrike tasks update-importance <task-id> Low
its wrike tasks update-importance <task-id> Low --json
```

### `its wrike tasks update-status <taskId> <status>`
Change task status. Move the task to a different workflow stage.
```bash
its wrike tasks update-status <task-id> "In Progress"
its wrike tasks update-status <task-id> "In Progress" --json
```

### `its wrike tasks update-description <taskId> [text]`
Replace a task's description. Same formatting rules as add-comment: plain text by default (\n → <br>, @Name auto-mentions), --markdown for bold/italic/links/bullets, --html for raw. Wrike renders <br>, <a>, <b>, <i>, and mention anchors only.
Flags: `--file` Read comment body from a file path · `--stdin` Read comment body from stdin · `--html` Send body as raw HTML. Wrike only renders <br>, <a>, <b>, <i>, and mention anchors — other tags are ignored. Use --markdown for richer formatting. · `--markdown` Convert Markdown to Wrike-safe HTML. Supported: **bold**, _italic_, `code` (rendered italic), # headings (rendered bold), [text](url), - bullets (flattened to •). In this mode, mention users with plain @Name (auto-resolves). Mutually exclusive with --html. Recommended over --html. · `--skip-mention-check` Bypass the @mention markup guard Use only when posting a literal string that happens to contain mention-like markup.
```bash
its wrike tasks update-description <task-id> --markdown "## Plan\n- step 1"
its wrike tasks update-description <task-id> --markdown "## Plan\n- step 1" --json
```

### `its wrike tasks add-comment <taskId> [text]`
Add a comment to a task. Prefer --markdown (bold/italic/links/bullets + plain @Name auto-resolves to mentions). Plain text also works (\n → <br>, @Name auto-resolved). Use --html only when you need raw anchor-based mentions; the mention markup guard rejects mismatched modes unless --skip-mention-check is set. Wrike only renders <br>, <a>, <b>, <i>, and mention anchors.
Flags: `--file` Read comment body from a file path · `--stdin` Read comment body from stdin · `--html` Send body as raw HTML. Wrike only renders <br>, <a>, <b>, <i>, and mention anchors — other tags are ignored. Use --markdown for richer formatting. · `--markdown` Convert Markdown to Wrike-safe HTML. Supported: **bold**, _italic_, `code` (rendered italic), # headings (rendered bold), [text](url), - bullets (flattened to •). In this mode, mention users with plain @Name (auto-resolves). Mutually exclusive with --html. Recommended over --html. · `--skip-mention-check` Bypass the @mention markup guard Use only when posting a literal string that happens to contain mention-like markup.
```bash
its wrike tasks add-comment <task-id> --markdown "**done** — pushed to main"
its wrike tasks add-comment <task-id> --markdown "**done** — pushed to main" --json
```

### `its wrike tasks attach <taskId> <filePath>`
Attach a file to a task. Upload a local file as an attachment.
```bash
its wrike tasks attach <task-id> ./diagram.png
its wrike tasks attach <task-id> ./diagram.png --json
```

## projects

### `its wrike projects`
List Wrike projects (optionally within a space — accepts name or ID).
Flags: `--space` Filter to a space (name or ID)
```bash
its wrike projects
its wrike projects "IT Operations"
its wrike projects --watch
```

### `its wrike projects search <query>`
Search Wrike projects by title (accepts space name or ID). Substring match across the most relevant fields; case-insensitive.
Flags: `--space` Scope to a space (name or ID)
```bash
its wrike projects search "migration"
its wrike projects search "migration" --json
```

### `its wrike projects tasks <project>`
List tasks in a project (accepts project name or ID).
Flags: `--status` Filter by task status · `--limit` Maximum number of tasks
```bash
its wrike projects tasks "Sprint 23"
its wrike projects tasks "Sprint 23" --json
```

## contacts

### `its wrike contacts`
List all Wrike contacts. Surfaces the most common fields; pass --json for raw shape.
Flags: `--filter` Filter by name
```bash
its wrike contacts
its wrike contacts --json
its wrike contacts --watch
```

### `its wrike contacts search <query>`
Search contacts by name or email. Substring match across the most relevant fields; case-insensitive.
```bash
its wrike contacts search jane.smith@example.com
its wrike contacts search jane.smith@example.com --json
```

### `its wrike contacts find [query]`
Find contacts by name and/or email domain (for populating --assignee).
Flags: `--email` Filter by email substring (e.g. '@example.com' for the whole domain)
```bash
its wrike contacts find jane.smith@example.com
its wrike contacts find jane.smith@example.com --json
```

### `its wrike contacts get <user_id>`
Get a single contact by user ID. Pass the id (or any natural identifier) as the positional arg.
```bash
its wrike contacts get <user-id>
its wrike contacts get <user-id> --json
```

## spaces

### `its wrike spaces`
List all Wrike spaces. Surfaces the most common fields; pass --json for raw shape.
```bash
its wrike spaces
its wrike spaces --json
its wrike spaces --watch
```

## folders

### `its wrike folders <space>`
List folders in a space (accepts space name or ID). Surfaces the most common fields; pass --json for raw shape.
```bash
its wrike folders "IT Operations"
its wrike folders "IT Operations" --json
its wrike folders "IT Operations" --watch
```

## workflows

### `its wrike workflows`
List all workflows with statuses. Surfaces the most common fields; pass --json for raw shape.
```bash
its wrike workflows
its wrike workflows --json
its wrike workflows --watch
```

## custom-fields

### `its wrike custom-fields`
List custom field definitions. Surfaces the most common fields; pass --json for raw shape.
Flags: `--filter` Filter by field name
```bash
its wrike custom-fields
its wrike custom-fields --json
its wrike custom-fields --watch
```

## item-types

### `its wrike item-types`
List custom item types (request types). Surfaces the most common fields; pass --json for raw shape.
```bash
its wrike item-types
its wrike item-types --json
its wrike item-types --watch
```

## onboarding

### `its wrike onboarding get <permalink>`
Extract new starter onboarding details from a task. Pass the id (or any natural identifier) as the positional arg.
```bash
its wrike onboarding get <task-id>
its wrike onboarding get <task-id> --json
```

## leavers

### `its wrike leavers`
List IT - Leaver tickets. Defaults to Active; pass --status Completed for closed ones. Surfaces Last Day + days-until-leave.
Flags: `--status` Filter by status (default: Active; 'all' for any) · `--limit` Maximum results

### `its wrike leavers complete <idOrPermalink>`
Orchestrate the IT offboarding flow for a leaver ticket — disable + revoke sessions, clear manager, set extensionAttribute13=Leaver, convert mailbox to shared + GAL-hide, remove licences and groups, then mark the Wrike ticket Completed (only on full success). The status comment is returned as a draft — never posted directly, per wrike-comment-approval. Destructive — needs --confirm. Use --dry-run first.
Flags: `--confirm` Required to execute mutations — otherwise dry-run · `--dry-run` Print the plan without running anything · `--user` Override the resolved Entra UPN (skip ticket → user lookup) · `--skip` Comma-separated steps to skip (disable, sessions, manager, ext13, mailbox, licences, groups, wrike)

### `its wrike leavers get <idOrPermalink>`
Get full leaver-ticket details with comments. Pass the id (or Wrike permalink) as the positional arg.

## dashboard

### `its wrike dashboard`
Overview of IT tickets by status. Surfaces the most common fields; pass --json for raw shape.
```bash
its wrike dashboard
its wrike dashboard --json
its wrike dashboard --watch
```
