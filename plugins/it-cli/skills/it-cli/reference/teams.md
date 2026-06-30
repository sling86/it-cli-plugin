# Teams (`teams`)

Microsoft Teams (Graph) for the logged-in user — list recent chats, read chat messages, check your presence. Delegated-only (runs as you via `its auth login`).

> Auto-generated reference. Configure: `its teams setup`. For a command you can name, prefer live help `its teams <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## chats

### `its teams chats`
Your recent Teams chats (1:1, group, meeting), most-recently-active first. Delegated — run `its auth login` first.
Flags: `--limit` Max chats to return (1-50)
```bash
its teams chats
its teams chats --limit 50
```

### `its teams chats messages <chat_id>`
Read messages in one of your chats, newest first. Pass a chat ID from `its teams chats`.
Flags: `--limit` Max messages to return (1-50)
```bash
its teams chats messages 19:abc...@thread.v2
```

## presence

### `its teams presence get`
Your current Teams presence (availability + activity).
```bash
its teams presence
```
