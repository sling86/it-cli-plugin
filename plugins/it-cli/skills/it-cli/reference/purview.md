# Purview (`purview`)

Microsoft Purview compliance, read-only — retention policies and their rules (locations, action, duration) and retention labels with their publish and auto-apply policies.

> Auto-generated reference. Configure: `its purview setup`. For a command you can name, prefer live help `its purview <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## retention

### `its purview retention`
List retention policies and their rules: where they apply (OneDrive, SharePoint, Exchange, Teams), the action and the duration. Read-only.
```bash
its purview retention list --ai
```

## labels

### `its purview labels`
List retention labels, with the policies that publish them and the ones that apply them automatically (and their query). Read-only.
