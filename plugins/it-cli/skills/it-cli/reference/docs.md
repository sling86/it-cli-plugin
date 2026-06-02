# Docs UI (`docs`)

Interactive help UI for the `its` CLI. `docs serve` launches a browser-based command explorer (tree, search, examples) bound to 127.0.0.1 with a random per-session token. `docs open <command>` deep-links to a single page. `docs search <q>` and `docs show <topic>` render help in the terminal..

> Auto-generated command reference. Do not edit by hand.
> Configure with `its docs setup`; get live help with `its docs <resource> help`.

[← Provider index](./index.md)

## Resources

- [build](#build)
- [serve](#serve)
- [open](#open)
- [search](#search)
- [show](#show)

### build

| Command | Description |
|---------|-------------|
| `its docs build` | Compile the interactive help UI browser assets (Vite build + embedded-assets manifest) |

#### `its docs build`

Compile the interactive help UI browser assets (Vite build + embedded-assets manifest).

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--force` | `` | Rebuild even if up-to-date assets already exist | false |

**Examples:**

```bash
its docs build

# Rebuild even if assets are present
its docs build --force
```

---

### serve

| Command | Description |
|---------|-------------|
| `its docs serve` | Launch the interactive help UI on 127.0.0.1 and open a browser |

#### `its docs serve`

Launch the interactive help UI on 127.0.0.1 and open a browser.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--port` | `` | Bind port (0 = random) | 0 |
| `--no-open` | `` | Don't auto-open the browser | false |
| `--theme` | `` | Theme | auto |

**Examples:**

```bash
its docs serve

# Bind a known port (useful for bookmarks)
its docs serve --port 4242

# Print URL only — don't launch a browser
its docs serve --no-open
```

---

### open

| Command | Description |
|---------|-------------|
| `its docs open <topic>` | Launch the help UI and open the browser to a specific command page |

#### `its docs open <topic>`

Launch the help UI and open the browser to a specific command page.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--port` | `` | Bind port (0 = random) | 0 |
| `--theme` | `` | Theme | auto |

**Examples:**

```bash
its docs open rmm agents list

its docs open entra users
```

---

### search

| Command | Description |
|---------|-------------|
| `its docs search <query>` | Fuzzy search every command — terminal output. Surfaces the most common fields; pass --json for raw shape. |

#### `its docs search <query>`

Fuzzy search every command — terminal output. Surfaces the most common fields; pass --json for raw shape.

**Flags:**

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--limit` | `` | Max results | 25 |

**Examples:**

```bash
its docs search "offline agents"

its docs search dokploy apps
```

---

### show

| Command | Description |
|---------|-------------|
| `its docs show <topic>` | Show one command's help in the terminal (ANSI markdown). Surfaces the most common fields; pass --json for raw shape. |

#### `its docs show <topic>`

Show one command's help in the terminal (ANSI markdown). Surfaces the most common fields; pass --json for raw shape.

**Examples:**

```bash
its docs show rmm

its docs show rmm agents

its docs show rmm agents list
```

---
