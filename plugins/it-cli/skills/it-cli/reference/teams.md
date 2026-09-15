# Teams (`teams`)

Microsoft Teams (Graph) for the logged-in user — list recent chats, read chat messages, check your presence. Delegated-only (runs as you via `its auth login`).

> Auto-generated reference. Configure: `its teams setup`. For a command you can name, prefer live help `its teams <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## chats

### `its teams chats`
Your recent Teams chats (1:1, group, meeting), most-recently-active first. Delegated — run `its auth login` first.
Flags: `--limit` Max chats to show (0 for every chat)
```bash
its teams chats
its teams chats --limit 0
```

### `its teams chats messages <chat_id>`
Read messages in one of your chats, newest first. Pass a chat ID from `its teams chats`.
Flags: `--limit` Max matching messages (0 for all; default 20) · `--since` Oldest message time (ISO, local date or -7d/-24h) · `--until` Newest message time (ISO or local date/time) · `--from` Sender display-name substring
```bash
its teams chats messages 19:abc...@thread.v2
```

### `its teams chats images <chat_id>`
Download pasted and attached images from a chat using delegated Graph auth. Pages to the time boundary, checks image signatures and never overwrites an existing file.
Flags: `--since` Oldest message time (default -1d) · `--until` Newest message time · `--from` Sender display-name substring · `--output` Directory to write images (default current directory)
```bash
its teams chats images 19:abc...@thread.v2 --since -1d --output ./teams-images
```

### `its teams chats send <chat_id>`
Send a message to one of your chats, as you. Pass a chat ID from `its teams chats`. Delegated-only, and needs the ChatMessage.Send scope — after granting it, run `its auth login` again or the cached token still won't carry it.
Flags: `--message` Message text · `--message-file` Read the message from a UTF-8 file (use for long bodies — Windows command-line cap) · `--html` Treat the message as HTML (default plain text)
```bash
its teams chats send 19:abc...@thread.v2 --message "on my way"
its teams chats send 19:abc...@thread.v2 --html --message-file note.html
```

## presence

### `its teams presence get`
Your current Teams presence (availability + activity).
```bash
its teams presence
```
