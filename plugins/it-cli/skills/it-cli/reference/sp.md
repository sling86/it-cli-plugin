# SharePoint (`sp`)

SharePoint Online — sites, document libraries, lists, files, search, permissions, pages.

> Auto-generated reference. Configure: `its sp setup`. For a command you can name, prefer live help `its sp <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## sites

### `its sp sites`
List all SharePoint sites. Surfaces the most common fields; pass --json for raw shape.
Flags: `--all` Include personal (OneDrive) sites
```bash
its sp sites
its sp sites --json
its sp sites --watch
```

### `its sp sites get <siteId>`
Get site details by ID. Pass the id (or any natural identifier) as the positional arg.
```bash
its sp sites get <site-id>
its sp sites get <site-id> --json
```

### `its sp sites search <query>`
Search sites by name. Substring match across the most relevant fields; case-insensitive.
```bash
its sp sites search "marketing"
its sp sites search "marketing" --json
```

### `its sp sites root`
Get the root site. Returns the document library root.
```bash
its sp sites root
its sp sites root --json
```

### `its sp sites subsites <siteId>`
List child sites. Returns child sites of the given site.
```bash
its sp sites subsites <site-id>
its sp sites subsites <site-id> --json
```

### `its sp sites structure <siteId>`
Get site structure (drives, lists, subsites). Walks the site hierarchy + drives.
```bash
its sp sites structure <site-id>
its sp sites structure <site-id> --json
```

## drives

### `its sp drives <siteId>`
List document libraries on a site. Surfaces the most common fields; pass --json for raw shape.
```bash
its sp drives <site-id>
its sp drives <site-id> --json
its sp drives <site-id> --watch
```

### `its sp drives root <siteId>`
List files at document library root. Returns the document library root.
Flags: `--drive` Drive ID · `--top` Number of items to return · `--all` Fetch all results (overrides --top)
```bash
its sp drives root <site-id>
its sp drives root <site-id> --json
```

### `its sp drives folder <siteId>`
List folder contents. Returns the contents of a folder by path.
Flags: `--drive` Drive ID · `--path` Item ID or /path to folder
```bash
its sp drives folder <site-id> --path "Shared Documents/Marketing"
its sp drives folder <site-id> --path "Shared Documents/Marketing" --json
```

### `its sp drives get <siteId>`
Get file or folder details. Pass the id (or any natural identifier) as the positional arg.
Flags: `--drive` Drive ID · `--item` Item ID
```bash
its sp drives get <site-id> --path "Shared Documents/report.pdf"
its sp drives get <site-id> --path "Shared Documents/report.pdf" --json
```

## lists

### `its sp lists <siteId>`
List all lists on a site. Surfaces the most common fields; pass --json for raw shape.
Flags: `--all` Include hidden lists
```bash
its sp lists <site-id>
its sp lists <site-id> --json
its sp lists <site-id> --watch
```

### `its sp lists get <siteId>`
Get list details. Pass the id (or any natural identifier) as the positional arg.
Flags: `--list` List ID
```bash
its sp lists get <site-id> --list <list-id>
its sp lists get <site-id> --list <list-id> --json
```

### `its sp lists columns <siteId>`
Get column definitions for a list. Returns column definitions for a list.
Flags: `--list` List ID · `--all` Include hidden columns
```bash
its sp lists columns <site-id> --list <list-id>
its sp lists columns <site-id> --list <list-id> --json
```

### `its sp lists items <siteId>`
List items from a list. Returns rows of a list, with column values.
Flags: `--list` List ID · `--top` Number of items to return · `--filter` OData filter expression · `--orderby` OData orderBy expression
```bash
its sp lists items <site-id> --list <list-id>
its sp lists items <site-id> --list <list-id> --json
```

### `its sp lists create-item <siteId>`
Create a list item. Idempotent on natural-key collision; use update-item to mutate.
Flags: `--list` List ID · `--fields` JSON string of field values
```bash
its sp lists create-item <site-id> --list <list-id> --json '{"Title":"New row"}'
its sp lists create-item <site-id> --list <list-id> --json '{"Title":"New ticket","Priority":"High","Assignee":"jane.smith@example.com"}'
```

### `its sp lists update-item <siteId>`
Update a list item. PATCH — only supplied fields change.
Flags: `--list` List ID · `--item` Item ID · `--fields` JSON string of field values to update
```bash
its sp lists update-item <site-id> --list <list-id> --item <item-id> --json '{"Title":"Updated"}'
its sp lists update-item <site-id> --list <list-id> --item <item-id> --json '{"Status":"Closed","ResolvedBy":"jane.smith@example.com"}'
```

### `its sp lists delete-item <siteId>`
Delete a list item. Permanent — use --confirm.
Flags: `--list` List ID · `--item` Item ID · `--confirm` Confirm deletion
```bash
its sp lists delete-item <site-id> --list <list-id> --item <item-id> --confirm
its sp lists delete-item <site-id> --list <list-id> --item <item-id> --confirm --json
```

## files

### `its sp files download`
Download a drive item to disk (--out) or pipe binary-safe to stdout. Resolves the pre-signed @microsoft.graph.downloadUrl from item metadata and fetches that — `sp graph get .../content` corrupts binary on the UTF-8 path.
Flags: `--site` Site ID (with --drive --item|--path) · `--user` User UPN — operate on /users/<upn>/drive · `--drive` Drive ID (with --item or --path) · `--item` Drive item ID · `--path` Item path relative to drive root (e.g. /Folder/file.docx) · `--url` Pre-signed @microsoft.graph.downloadUrl (skips metadata lookup) · `--out` Local file path to write to. Omit to pipe to stdout.
```bash
its sp files download --user tony@example.com --item 01Q3JEFHMUOTAVKHPGWNBJPEDKM376OQH6 --out out.docx
its sp files download --site <siteId> --drive <driveId> --path "/Folder/file.pdf" --out file.pdf
its sp files download --url "https://.../download" --out file.bin
its sp files download --user tony@example.com --item <id> | sha256sum
```

### `its sp files upload <siteId>`
Upload a text file. Stream a local file to the resource.
Flags: `--drive` Drive ID · `--path` Parent path (default /) · `--name` File name · `--content` Text content to upload · `--content-file` Read --content from a local UTF-8 file (use for content > ~15KB — Windows command-line cap)
```bash
its sp files upload <site-id> --path "Shared Documents/report.pdf" --file ./report.pdf
its sp files upload <site-id> --path "Shared Documents/report.pdf" --file ./report.pdf --json
```

### `its sp files folder <siteId>`
Create a folder. Returns the contents of a folder by path.
Flags: `--drive` Drive ID · `--parent` Parent item ID · `--name` Folder name
```bash
its sp files folder <site-id> --path "Shared Documents"
its sp files folder <site-id> --path "Shared Documents" --json
```

### `its sp files delete <siteId>`
Delete a file or folder (moves to recycle bin). Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record.
Flags: `--drive` Drive ID · `--item` Item ID · `--confirm` Confirm deletion
```bash
its sp files delete <site-id> --path "Shared Documents/old.docx" --confirm
```

### `its sp files move <siteId>`
Move or rename a file. Move an item between folders. --confirm required.
Flags: `--drive` Drive ID · `--item` Item ID · `--name` New file name · `--parent` New parent folder ID
```bash
its sp files move <site-id> --from "Shared Documents/old.docx" --to "Shared Documents/new.docx"
its sp files move <site-id> --from "Shared Documents/old.docx" --to "Shared Documents/new.docx" --json
```

### `its sp files checkout <siteId>`
Check out a file for editing. Locks the item against concurrent edits.
Flags: `--drive` Drive ID · `--item` Item ID
```bash
its sp files checkout <site-id> --path "Shared Documents/draft.docx"
its sp files checkout <site-id> --path "Shared Documents/draft.docx" --json
```

### `its sp files checkin <siteId>`
Check in a file. Releases the lock after editing.
Flags: `--drive` Drive ID · `--item` Item ID · `--comment` Check-in comment · `--type` Check-in type: minor, major, or overwrite
```bash
its sp files checkin <site-id> --path "Shared Documents/draft.docx" --comment "v2"
its sp files checkin <site-id> --path "Shared Documents/draft.docx" --comment "v2" --json
```

### `its sp files versions <siteId>`
List file version history. Returns version history for a file.
Flags: `--drive` Drive ID · `--item` Item ID
```bash
its sp files versions --item <item-id>
its sp files versions --item <item-id> --json
```

### `its sp files restore <siteId>`
Restore a file to a previous version. Restore a soft-deleted item from trash.
Flags: `--drive` Drive ID · `--item` Item ID · `--version` Version ID to restore · `--confirm` Confirm restore
```bash
its sp files restore <site-id> --item <item-id> --version <version-id>
its sp files restore <site-id> --item <item-id> --version <version-id> --json
```

## search

### `its sp search <query>`
Search across SharePoint. Surfaces the most common fields; pass --json for raw shape.
Flags: `--type` Entity type: driveItem, listItem, list, or site · `--top` Maximum results to return
```bash
its sp search "quarterly report"
its sp search "quarterly report" --json
its sp search "quarterly report" --watch
```

## permissions

### `its sp permissions <siteId>`
List app-level site permissions. Surfaces the most common fields; pass --json for raw shape.
```bash
its sp permissions <site-id>
its sp permissions <site-id> --json
its sp permissions <site-id> --watch
```

### `its sp permissions item <siteId>`
List sharing permissions on a file or folder. Single record detail.
Flags: `--drive` Drive ID · `--item` Item ID
```bash
its sp permissions item <site-id> --item <item-id>
its sp permissions item <site-id> --item <item-id> --json
```

### `its sp permissions share <siteId>`
Create a sharing link. Creates a sharing link / direct grant.
Flags: `--drive` Drive ID · `--item` Item ID · `--type` Link type: view, edit, or embed · `--scope` Link scope: anonymous or organization
```bash
its sp permissions share --item <item-id> --user jane.smith@example.com --role read
its sp permissions share --item <item-id> --user jane.smith@example.com --role read --json
```

### `its sp permissions grant-app <siteId>`
Grant the it-cli app (or another app via --app) a Sites.Selected role on one site. Useful for bootstrapping the role needed by `its sp groups *`. Requires Sites.FullControl.All on the CALLING credentials — typically via a separate admin app (SP_ADMIN_CLIENT_ID/SP_ADMIN_CLIENT_SECRET) or a one-off elevation.
Flags: `--role` Role to grant: read, write, or fullcontrol · `--app` Client (object) id of the app to grant. Defaults to the current SP_CLIENT_ID / CLIENT_ID. · `--name` Display name to store with the grant. Defaults to 'its-cli'.

### `its sp permissions remove <siteId>`
Remove a sharing permission. Permanent — use --confirm.
Flags: `--drive` Drive ID · `--item` Item ID · `--permission` Permission ID to remove · `--confirm` Confirm removal
```bash
its sp permissions remove <site-id> --item <item-id> --perm <perm-id> --confirm
```

## groups

### `its sp groups <site>`
List SharePoint site groups (Owners/Members/Visitors + custom) on a site. Pass a site URL or Graph site id.

### `its sp groups members <site> <group>`
List members of an SP site group. Accept group id or title.

### `its sp groups add-member <site> <group> <principal>`
Add a UPN, Entra security-group object id, or pre-formed claim LoginName to an SP site group. Idempotent.

### `its sp groups remove-member <site> <group> <principal>`
Remove a member from an SP site group. Destructive — use --confirm.
Flags: `--confirm` Confirm removal

## pages

### `its sp pages <siteId>`
List modern pages on a site. Surfaces the most common fields; pass --json for raw shape.
```bash
its sp pages <site-id>
its sp pages <site-id> --json
its sp pages <site-id> --watch
```

### `its sp pages get <siteId>`
Get page details. Pass the id (or any natural identifier) as the positional arg.
Flags: `--page` Page ID
```bash
its sp pages get <site-id> --page <page-id>
its sp pages get <site-id> --page <page-id> --json
```

## dashboard

### `its sp dashboard`
Comprehensive SharePoint overview. Surfaces the most common fields; pass --json for raw shape.
```bash
its sp dashboard
its sp dashboard --json
its sp dashboard --watch
```

## graph

### `its sp graph get <path>`
Raw Graph GET — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal) · `--raw` Return the response body as raw bytes (no JSON decode). Required for binary endpoints like /content. Currently honoured by the `sp` provider. · `--out` Write the response to this file path instead of stdout. Implies --raw.
```bash
its sp graph get "/sites/<site-id>/lists"
its sp graph get "/sites/<site-id>/lists" --json
```

### `its sp graph post <path>`
Raw Graph POST — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--body` Request body — inline JSON string or @file.json to read from disk · `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its sp graph post "/sites/<site-id>/lists" --body @./new-list.json
its sp graph post "/sites/<site-id>/lists" --body @./new-list.json --json
```

### `its sp graph patch <path>`
Raw Graph PATCH — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--body` Request body — inline JSON string or @file.json to read from disk · `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its sp graph patch "/sites/<site-id>/lists/<list-id>" --body '{"displayName":"Renamed"}'
its sp graph patch "/sites/<site-id>/lists/<list-id>" --body --json
```

### `its sp graph put <path>`
Raw Graph PUT — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--body` Request body — inline JSON string or @file.json to read from disk · `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its sp graph put "/sites/<site-id>/drive/items/<item-id>/content" --body @./file.bin
its sp graph put "/sites/<site-id>/drive/items/<item-id>/content" --body @./file.bin --json
```

### `its sp graph delete <path>`
Raw Graph DELETE — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its sp graph delete "/sites/<site-id>/lists/<list-id>" --confirm
```
