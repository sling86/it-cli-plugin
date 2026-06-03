# Docs UI (`docs`)

Interactive help UI for the `its` CLI. `docs serve` launches a browser-based command explorer (tree, search, examples) bound to 127.0.0.1 with a random per-session token. `docs open <command>` deep-links to a single page. `docs search <q>` and `docs show <topic>` render help in the terminal..

> Auto-generated reference. Configure: `its docs setup`. For a command you can name, prefer live help `its docs <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## build

### `its docs build`
Compile the interactive help UI browser assets (Vite build + embedded-assets manifest).
Flags: `--force` Rebuild even if up-to-date assets already exist
```bash
its docs build
its docs build --force
```

## serve

### `its docs serve`
Launch the interactive help UI on 127.0.0.1 and open a browser.
Flags: `--port` Bind port (0 = random) · `--no-open` Don't auto-open the browser · `--theme` Theme
```bash
its docs serve
its docs serve --port 4242
its docs serve --no-open
```

## open

### `its docs open <topic>`
Launch the help UI and open the browser to a specific command page.
Flags: `--port` Bind port (0 = random) · `--theme` Theme
```bash
its docs open rmm agents list
its docs open entra users
```

## search

### `its docs search <query>`
Fuzzy search every command — terminal output. Surfaces the most common fields; pass --json for raw shape.
Flags: `--limit` Max results
```bash
its docs search "offline agents"
its docs search dokploy apps
```

## show

### `its docs show <topic>`
Show one command's help in the terminal (ANSI markdown). Surfaces the most common fields; pass --json for raw shape.
```bash
its docs show rmm
its docs show rmm agents
its docs show rmm agents list
```
