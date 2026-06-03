# Dokploy (`dokploy`)

Dokploy self-hosted PaaS management — projects, apps, databases, deployments, domains, registries, notifications.

> Auto-generated reference. Configure: `its dokploy setup`. For a command you can name, prefer live help `its dokploy <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## projects

### `its dokploy projects`
Every project in this Dokploy server, with a per-project tally of applications and databases. The starting point for navigating most other dokploy commands.
```bash
its dokploy projects
its dokploy projects --json
its dokploy projects
its dokploy projects --json
its dokploy projects --watch
```

### `its dokploy projects get <projectId>`
Full project detail — owner, env, plus the contained apps/databases/composes. Use the project ID from `dokploy projects`.
```bash
its dokploy projects get <project-id>
its dokploy projects get <project-id> --json
```

### `its dokploy projects create`
Create an empty Dokploy project. Apps and databases are added afterwards via `dokploy apps create` / `databases create`.
Flags: `--name` Project name · `--description` Project description
```bash
its dokploy projects create --name "my-app"
its dokploy projects create --name "my-app" --json
```

### `its dokploy projects delete <projectId>`
Permanently delete a project AND every app/database it contains. Destructive — needs --confirm.
Flags: `--confirm` Confirm deletion
```bash
its dokploy projects delete <project-id> --confirm
```

## apps

### `its dokploy apps`
Every application across every project — name, project, status, last-deploy. Filter with --project to scope to one project.
Flags: `--project` Filter by project name
```bash
its dokploy apps
its dokploy apps --project my-project
its dokploy apps --ai | its dokploy apps health --stdin
```

### `its dokploy apps get <applicationId>`
Get full detail for an application — source config, mounts, env keys, replicas, last deploy, and live container image.
```bash
its dokploy apps get <app-id>
its dokploy apps get <app-id> --json
```

### `its dokploy apps create`
Scaffold an empty application inside a project. Wire it to a source (git, docker, dockerfile) afterwards via `dokploy apps set-source`.
Flags: `--project` Project ID · `--name` Application display name · `--appName` Docker app name (auto-slugified from name if omitted) · `--description` Application description
```bash
its dokploy apps create --project <project-id> --name "my-api"
its dokploy apps create --project <project-id> --name "my-api" --json
```

### `its dokploy apps delete <applicationId>`
Permanently delete an application — container, build artefacts, env. Destructive — needs --confirm. The project remains.
Flags: `--confirm` Confirm deletion
```bash
its dokploy apps delete <app-id> --confirm
```

### `its dokploy apps deploy <app>`
Trigger deployment, optionally wait for healthy and show logs.
Flags: `--wait` Wait for container to become healthy · `--logs` Show last 50 lines of logs after deploy
```bash
its dokploy apps deploy <app-id>
its dokploy apps deploy <app-id> --wait
```

### `its dokploy apps stop <applicationId>`
Stop the running container without deleting it. Restart via `dokploy apps start`.
```bash
its dokploy apps stop <app-id>
```

### `its dokploy apps start <applicationId>`
Start a previously stopped container. Idempotent — no-op if already running.
```bash
its dokploy apps start <app-id>
its dokploy apps start <app-id> --json
```

### `its dokploy apps restart <applicationId>`
Stop + start in one call. Doesn't rebuild — use `dokploy apps deploy` if the image needs to change.
```bash
its dokploy apps restart <app-id>
its dokploy apps restart <app-id> --json
```

### `its dokploy apps set-source <app>`
Wire an application to a source provider. --type github links a GitHub App repo; --type docker pins to a registry image.
Flags: `--type` Source type · `--github-id` Dokploy GitHub provider ID (its dokploy git list) · `--owner` GitHub owner / org · `--repo` GitHub repository name · `--branch` Branch (default main) · `--build-path` Path inside the repo to use as build context (default /) · `--watch-paths` Comma-separated glob list to limit auto-deploy triggers (default ** = any change) · `--image` Docker image reference (for --type docker) · `--username` Registry username (optional, for private registries) · `--password` Registry password / token (optional) · `--registry-url` Registry URL — e.g. ghcr.io or docker.io. Leave blank for Docker Hub. · `--dry-run` Print the planned request without sending
```bash
its dokploy apps set-source <app-id> --type github --repo owner/repo --branch main
its dokploy apps set-source <app-id> --type github --repo owner/repo --branch main --json
```

### `its dokploy apps set-build <app>`
Set build type for an application (dockerfile, nixpacks, heroku_buildpacks, paketo_buildpacks, static, railpack). Dockerfile path/context configurable.
Flags: `--type` Build type · `--dockerfile` Path to the Dockerfile (default Dockerfile) · `--context` Docker build context (default .) · `--stage` Multi-stage build target stage · `--publish-dir` Static SPA publish directory (type=static only) · `--dry-run` Print the planned request without sending
```bash
its dokploy apps set-build <app-id> --type nixpacks
its dokploy apps set-build <app-id> --type dockerfile --dockerfile "./Dockerfile"
```

### `its dokploy apps rebuild <app>`
Force a fresh build from source (clones, builds image, deploys). Distinct from `redeploy` (re-uses last image) and `deploy` which is the alias for this. Internally maps to application.deploy — `application.rebuild` returns 404.
```bash
its dokploy apps rebuild <app-id>
its dokploy apps rebuild <app-id> --json
```

### `its dokploy apps wait-deploy <app>`
Poll an application's deployments until the latest (or --since <id>) transitions from running to done/error. Exit code reflects the final state: 0 on done, 1 on error or timeout. Suitable for GitHub Actions
Flags: `--timeout` Timeout in seconds (default 600) · `--interval` Poll interval in seconds (default 5) · `--since` Wait for this specific deployment ID. Avoids the race where two deploys fire close together and the latest snapshot changes mid-poll.
```bash
its dokploy apps wait-deploy <app-id> --timeout 600
its dokploy apps wait-deploy <app-id> --timeout 600 --json
```

### `its dokploy apps redeploy <applicationId>`
Redeploy an application without rebuilding. Redeploys the existing container; doesn't rebuild from source.
```bash
its dokploy apps redeploy <app-id>
its dokploy apps redeploy <app-id> --json
```

### `its dokploy apps logs <app>`
Show container logs for an application via Dokploy API (no SSH). Pass --follow to stream live (SSH-based) or --build to read the docker build log (the build log is the only place where 'image build failed' errors are visible).
Flags: `--follow` Follow log output (uses SSH stream) · `--tail` Number of lines to show · `--since` Show logs since duration (e.g. 1h, 30m). Only honoured by --follow / --build paths — readLogs API ignores it. · `--build` Read the docker build log (/etc/dokploy/logs/<appName>/*.log) instead of the running container log · `--ssh` Force SSH path even without --follow (legacy fallback)
```bash
its dokploy apps logs <app-id>
its dokploy apps logs <app-id> --lines 200
```

### `its dokploy apps monitoring <app>`
Resource-usage snapshot for a running app — CPU%, memory MB, disk, network rx/tx, block I/O. Empty arrays mean Dokploy hasn't sampled the container yet (first sample takes ~1 min).
Flags: `--samples` Latest N samples to show (default 5)
```bash
its dokploy apps monitoring <app-id>
its dokploy apps monitoring <app-id> --json
```

### `its dokploy apps traefik <app>`
Show the Traefik routing config (router + service + middlewares) Dokploy generated for the application. Useful for debugging 404/SSL/redirect issues at the proxy layer.
```bash
its dokploy apps traefik <app-id>
its dokploy apps traefik <app-id> --json
```

### `its dokploy apps status <app>`
Show container state, uptime, restart count, image, and domain.
```bash
its dokploy apps status <app-id>
its dokploy apps status <app-id> --json
its dokploy apps status <app-id> --watch
```

### `its dokploy apps clone <source> <newName>`
Clone an application — copies Docker settings, env vars, and creates a domain.
Flags: `--domain` Domain hostname for the new app · `--image` Override the Docker image · `--env-override` Override env var (KEY=VALUE, repeatable via comma-separated)
```bash
its dokploy apps clone <source-id> "my-api-staging"
its dokploy apps clone <source-id> "my-api-staging" --json
```

### `its dokploy apps shell <app> [cmd]`
Open an interactive shell in the running container via SSH + docker exec. Pass [cmd] for a one-shot (e.g. 'bun run db:migrate'). Defaults to sh.
Flags: `--shell` Shell to use when no [cmd] is given (default sh)
```bash
its dokploy apps shell <app-id>
its dokploy apps shell <app-id> --json
```

### `its dokploy apps migrate <app>`
Run a migration command inside the running app container. Defaults to `bun run db:migrate`. Lighter wrapper than `apps shell <id> '<cmd>'` — useful for ad-hoc migrations on apps that don't auto-migrate on boot.
Flags: `--cmd` Migration command (default `bun run db:migrate`). Use `npm run db:migrate` etc. for non-bun projects.
```bash
its dokploy apps migrate <app-id>
its dokploy apps migrate <app-id> --cmd "bun run db:seed"
```

### `its dokploy apps health`
Lightweight health sweep across ALL apps — replicas / last deploy / domain reachable. One row per app. Use `apps doctor <id>` for the deep dive on a specific app.
Flags: `--project` Filter by project name · `--fail-only` Show only apps with non-OK verdict
```bash
its dokploy apps health <app-id>
its dokploy apps health <app-id> --json
its dokploy apps health <app-id> --watch
```

### `its dokploy apps doctor <app>`
Run a battery of parallel checks (env, mounts, webhook, replicas, image, healthcheck, recent log scan, last build). One green/red checklist instead of probing five separate commands.
Flags: `--no-ssh` Skip checks that need SSH (host-path verify, build/service log scan)
```bash
its dokploy apps doctor <app-id>
its dokploy apps doctor <app-id> --json
```

### `its dokploy apps bootstrap <name>`
Bootstrap a new Dokploy app end-to-end: resolves or creates the project, creates the application, wires the GitHub source, sets the build type, pushes env vars, creates the domain, and triggers the first deploy. Idempotent — re-running with the same --name skips already-completed steps. Use --dry-run first.
Flags: `--repo` GitHub source as <owner/repo> · `--branch` Git branch (default main) · `--domain` Domain to assign (e.g. app.example.com) · `--port` Container port the app listens on (default 3000) · `--env-file` Path to .env file to push · `--project` Project name or id (default: same as --name; created if missing) · `--github-id` Dokploy GitHub provider id (default: first provider from `github providers`) · `--dockerfile` Dockerfile path inside the repo (default Dockerfile) · `--context` Docker build context (default .) · `--build-type` Build type (default dockerfile) · `--no-deploy` Skip the final rebuild/deploy step · `--dry-run` Print the plan without sending any mutations

### `its dokploy apps cert-status <app>`
Read Traefik acme.json for the app's domains — surfaces certificate state, expiry, last LE error. SSH-backed (reads from the dokploy-traefik container).

## databases

### `its dokploy databases`
List all databases across projects. Surfaces the most common fields; pass --json for raw shape.
```bash
its dokploy databases
its dokploy databases --json
its dokploy databases --watch
```

### `its dokploy databases url <id>`
Compose a connection URL for a database — uses the docker-network appName by default (sibling apps in the same project), or external host:port with --external. URL is printed plain to stdout for `eval` / `export DATABASE_URL=$(its dokploy databases url ...)`.
Flags: `--type` Database type: postgres, mysql, mariadb, mongo, redis · `--external` Compose using the externally-mapped port instead of the docker-network appName (requires the DB to have an external port configured) · `--host` Override hostname (default: appName for internal, dok host for external)
```bash
its dokploy databases url <db-id>
its dokploy databases url <db-id> --json
```

### `its dokploy databases get`
Get full details for a database (appName, credentials, port).
Flags: `--type` Database type: postgres, mysql, mariadb, mongo, redis
```bash
its dokploy databases get <db-id>
its dokploy databases get <db-id> --json
```

### `its dokploy databases create`
Create a database (postgres, mysql, mariadb, mongo, or redis).
Flags: `--project` Project ID · `--type` Database type: postgres, mysql, mariadb, mongo, redis · `--name` Database display name · `--dbName` Internal database name · `--dbUser` Database user (default varies by type) · `--dbPassword` Database password · `--image` Docker image (default varies by type) · `--description` Description
```bash
its dokploy databases create --type postgres --name "app-db" --project <project-id>
its dokploy databases create --type postgres --name "app-db" --project <project-id> --json
```

### `its dokploy databases deploy <databaseId>`
Deploy a database instance. Trigger a fresh deploy from the wired source.
Flags: `--type` Database type: postgres, mysql, mariadb, mongo, redis
```bash
its dokploy databases deploy <db-id>
its dokploy databases deploy <db-id> --json
```

### `its dokploy databases stop <databaseId>`
Stop a database instance. Stop the resource. Use --confirm if the action is destructive.
Flags: `--type` Database type: postgres, mysql, mariadb, mongo, redis
```bash
its dokploy databases stop <db-id>
```

### `its dokploy databases delete <databaseId>`
Delete a database instance. Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record.
Flags: `--type` Database type: postgres, mysql, mariadb, mongo, redis · `--confirm` Confirm deletion
```bash
its dokploy databases delete <db-id> --confirm
```

## deployments

### `its dokploy deployments <applicationId>`
List deployments for an application. Surfaces the most common fields; pass --json for raw shape.
```bash
its dokploy deployments <app-id>
its dokploy deployments <app-id> --json
its dokploy deployments <app-id> --watch
```

### `its dokploy deployments queue`
Show pending/active deployments across the whole Dokploy server (not scoped to one app). Useful when a deploy seems stuck — empty queue means the worker is idle.
```bash
its dokploy deployments queue
its dokploy deployments queue --json
```

### `its dokploy deployments kill <deploymentId>`
Kill an in-flight deployment by ID (sends SIGTERM to the build process).
```bash
its dokploy deployments kill <deployment-id>
```

## domains

### `its dokploy domains <applicationId>`
List domains for an application. Surfaces the most common fields; pass --json for raw shape.
```bash
its dokploy domains <app-id>
its dokploy domains <app-id> --json
its dokploy domains <app-id> --watch
```

### `its dokploy domains create <applicationId>`
Add a domain to an application. Idempotent on duplicate names — use update/edit to mutate an existing record.
Flags: `--host` Domain hostname (e.g. app.example.com) · `--port` Container port (default: 3000) · `--https` Enable HTTPS (default: true) · `--cert` Certificate type (default: letsencrypt)
```bash
its dokploy domains create <app-id> --host "app.example.com" --port 3000
its dokploy domains create <app-id> --host "app.example.com" --port 3000 --json
```

### `its dokploy domains check [host]`
Inspect the live TLS certificate for a domain — issuer, validity window, days-to-expiry, SAN list. Hits the host directly, no Dokploy API. Use without args to check ALL domains across all apps.
Flags: `--warn-days` Days remaining below which to flag warn (default 30)
```bash
its dokploy domains check "app.example.com"
its dokploy domains check "app.example.com" --json
```

### `its dokploy domains delete <domainId>`
Remove a domain. Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record.
Flags: `--confirm` Confirm deletion
```bash
its dokploy domains delete <domain-id> --confirm
```

## env

### `its dokploy env <applicationId>`
Show environment variables for an application. Keys are visible by default; values are redacted unless --show-values is set.
Flags: `--show-values` Show actual values (otherwise redacted as ***)
```bash
its dokploy env <app-id>
its dokploy env <app-id> --json
its dokploy env <app-id> --watch
```

### `its dokploy env push <applicationId>`
Push an env file to an application. Upload local state to the upstream.
Flags: `--file` Path to env file
```bash
its dokploy env push <app-id> --file .env.production
its dokploy env push <app-id> --file .env.production --json
```

### `its dokploy env set <applicationId> <pairs>`
Set one or more env vars (KEY=value) without affecting others.
```bash
its dokploy env set <app-id> --key DEBUG --value "true"
its dokploy env set <app-id> --key DEBUG --value "true" --json
```

### `its dokploy env pull <applicationId>`
Pull env vars from an application to a local file. Download upstream state to local.
Flags: `--file` Output file path
```bash
its dokploy env pull <app-id> --file .env.local
its dokploy env pull <app-id> --file .env.local --json
```

## registries

### `its dokploy registries`
List configured container registries. Surfaces the most common fields; pass --json for raw shape.
```bash
its dokploy registries
its dokploy registries --json
its dokploy registries --watch
```

## destinations

### `its dokploy destinations`
List configured backup destinations (S3/Wasabi). Surfaces the most common fields; pass --json for raw shape.
```bash
its dokploy destinations
its dokploy destinations --json
its dokploy destinations --watch
```

## notifications

### `its dokploy notifications`
List configured notification channels. Surfaces the most common fields; pass --json for raw shape.
```bash
its dokploy notifications
its dokploy notifications --json
its dokploy notifications --watch
```

## dashboard

### `its dokploy dashboard`
Overview of all projects, applications, and databases. Surfaces the most common fields; pass --json for raw shape.
```bash
its dokploy dashboard
its dokploy dashboard --json
its dokploy dashboard --watch
```

## mounts

### `its dokploy mounts <app>`
List mounts (bind/volume/file) for an application. Surfaces the most common fields; pass --json for raw shape.
```bash
its dokploy mounts <app-id>
its dokploy mounts <app-id> --json
its dokploy mounts <app-id> --watch
```

### `its dokploy mounts add <app>`
Add a mount to an application (--type bind|volume|file). Use --ensure-host-path with bind to create the host directory.
Flags: `--type` Mount type · `--src` Source: host path (bind), volume name (volume), or initial content path (file) · `--dst` Mount path inside the container · `--content` Inline file content (--type file) · `--content-file` Read --content from a local UTF-8 file (use for content > ~15KB — Windows command-line cap) · `--ensure-host-path` For --type bind: create the host directory on the swarm node and chown to uid 1000 (avoids 'bind source path does not exist' deploy failures). Requires SSH.
```bash
its dokploy mounts add <app-id> --type bind --host /data --container /app/data
its dokploy mounts add <app-id> --type bind --host /data --container /app/data --json
```

### `its dokploy mounts update <mountId>`
Update an existing mount. PATCH semantics — only the supplied fields change.
Flags: `--src` New source (host path / volume name / file path) · `--dst` New mount path inside the container · `--content` New inline file content (file mounts) · `--content-file` Read --content from a local UTF-8 file (use for content > ~15KB — Windows command-line cap)
```bash
its dokploy mounts update <mount-id> --host "/data" --container "/app/data"
its dokploy mounts update <mount-id> --host "/data" --container "/app/data" --json
```

### `its dokploy mounts remove <mountId>`
Remove a mount. Permanent — use --confirm.
```bash
its dokploy mounts remove <mount-id> --confirm
```

## webhook

### `its dokploy webhook check <app>`
Diff the Dokploy expected webhook against the GitHub repo's actual hooks. Catches the autoDeploy:true-but-no-webhook drift. Detects GitHub App installations (no per-repo hook needed).
```bash
its dokploy webhook check <app-id>
its dokploy webhook check <app-id> --json
```

### `its dokploy webhook setup <app>`
Create the GitHub webhook on the repo wired to the Dokploy auto-deploy endpoint. Idempotent — if a hook with the same URL already exists, it's left alone.
Flags: `--secret` Webhook secret (defaults to a stable string per applicationId)
```bash
its dokploy webhook setup <app-id>
its dokploy webhook setup <app-id> --json
```

### `its dokploy webhook <app>`
List GitHub webhooks on the repo backing this application. Surfaces the most common fields; pass --json for raw shape.
```bash
its dokploy webhook list <app-id>
its dokploy webhook list <app-id> --json
its dokploy webhook list <app-id> --watch
```

## nodes

### `its dokploy nodes`
List Docker Swarm nodes (host, availability, role, engine version).
```bash
its dokploy nodes
its dokploy nodes --json
its dokploy nodes --watch
```

### `its dokploy nodes info <nodeId>`
Detailed info for a single swarm node (CPU, memory, labels, status).
```bash
its dokploy nodes info <node-id>
its dokploy nodes info <node-id> --json
```

### `its dokploy nodes apps`
List swarm services (replicated apps) running across nodes.
```bash
its dokploy nodes apps
its dokploy nodes apps --json
```

### `its dokploy nodes stats`
Live container resource stats across all swarm containers (CPU%, MemUsage, BlockIO, NetIO).
```bash
its dokploy nodes stats
its dokploy nodes stats --json
```

## cluster

### `its dokploy cluster join-token`
Get the `docker swarm join` command for adding a manager or worker node. Pass --role manager|worker (default worker).
Flags: `--role` Node role: manager or worker
```bash
its dokploy cluster join-token --role worker
its dokploy cluster join-token --role manager
```

## containers

### `its dokploy containers`
List ALL containers on the Dokploy host (not scoped to app). Surfaces the most common fields; pass --json for raw shape.
```bash
its dokploy containers
its dokploy containers --json
its dokploy containers --watch
```

### `its dokploy containers config <containerId>`
Get full Docker config for a container (env, mounts, network, labels).
```bash
its dokploy containers config <container-id>
its dokploy containers config <container-id> --json
```

### `its dokploy containers start <containerId>`
Start a container by ID (not app name). Lower-level than `apps start` — operates on a single container without re-deploying.
```bash
its dokploy containers start <container-id>
its dokploy containers start <container-id> --json
```

### `its dokploy containers stop <containerId>`
Stop a container by ID (not app name). Lower-level than `apps stop` — operates on a single container without re-deploying.
```bash
its dokploy containers stop <container-id>
```

### `its dokploy containers restart <containerId>`
Restart a container by ID (not app name). Lower-level than `apps restart` — operates on a single container without re-deploying.
```bash
its dokploy containers restart <container-id>
its dokploy containers restart <container-id> --json
```

### `its dokploy containers kill <containerId>`
Kill a container by ID (not app name). Lower-level than `apps kill` — operates on a single container without re-deploying.
```bash
its dokploy containers kill <container-id> --confirm
```

### `its dokploy containers remove <containerId>`
Remove a container by ID (not app name). Lower-level than `apps stop` — operates on a single container without re-deploying.
```bash
its dokploy containers remove <container-id> --confirm
```

## maintenance

### `its dokploy maintenance disk`
Docker disk usage on the Dokploy host (Images / Containers / Volumes / Build Cache) including reclaimable space.
```bash
its dokploy maintenance disk
its dokploy maintenance disk --json
```

### `its dokploy maintenance web`
Dokploy web-server settings (server IP, HTTPS, certificate type, host).
```bash
its dokploy maintenance web
its dokploy maintenance web --json
```

### `its dokploy maintenance traefik`
Global Traefik dynamic config (Dokploy-wide, not per-app). Use `apps traefik` for a single app's routing.
```bash
its dokploy maintenance traefik
its dokploy maintenance traefik --json
```

### `its dokploy maintenance clean`
Reclaim disk on the Dokploy host. --target images|volumes|containers|builder|monitoring|all (destructive — requires --confirm). Default target is `images` (safest).
Flags: `--target` What to clean · `--confirm` Confirm the cleanup
```bash
its dokploy maintenance clean --target builder --confirm
its dokploy maintenance clean --target images --confirm
```

### `its dokploy maintenance time`
Server time + timezone (sanity check for cron drift / TLS errors).
```bash
its dokploy maintenance time
its dokploy maintenance time --json
```

## traefik

### `its dokploy traefik logs`
Tail the dokploy-traefik container logs over SSH. Useful for debugging 502/cert issues at the proxy layer.
Flags: `--tail` Lines to tail (default 200)

### `its dokploy traefik acme`
Read the Traefik acme.json from the dokploy-traefik container — surfaces every Let's Encrypt certificate the host knows about. Read-only over SSH.

## github

### `its dokploy github providers`
List GitHub App installations configured in Dokploy.
```bash
its dokploy github providers
its dokploy github providers --json
```

### `its dokploy github repos <githubId>`
Repos visible to a GitHub App installation. Pass the `githubId` from `github providers` (NOT the inner `gitProviderId` — different IDs). Accepts either and auto-resolves.
```bash
its dokploy github repos <github-id>
its dokploy github repos <github-id> --json
```

### `its dokploy github branches <owner> <repo>`
Branches in a GitHub repo (uses the GitHub App, no PAT needed).
Flags: `--githubId` Specific Dokploy GitHub provider ID (optional; default first match)
```bash
its dokploy github branches owner repo
its dokploy github branches owner repo --json
```

## users

### `its dokploy users`
List users + roles in the active Dokploy organization. Surfaces the most common fields; pass --json for raw shape.
```bash
its dokploy users
its dokploy users --json
its dokploy users --watch
```

### `its dokploy users me`
Current user (the API token's owner) + org context + role.
```bash
its dokploy users me
its dokploy users me --json
```

### `its dokploy users invite`
Invite a user to the active Dokploy organization by email (user.invitation).
Flags: `--email` Invitee email address (required) · `--role` Initial role
```bash
its dokploy users invite --email jane.smith@example.com
its dokploy users invite --email jane.smith@example.com --json
```

### `its dokploy users remove <userId>`
Remove a user from the active Dokploy organization (requires --confirm).
Flags: `--confirm` Confirm the removal
```bash
its dokploy users remove <user-id> --confirm
```

## orgs

### `its dokploy orgs`
Organizations the API key has access to. Surfaces the most common fields; pass --json for raw shape.
```bash
its dokploy orgs
its dokploy orgs --json
its dokploy orgs --watch
```

### `its dokploy orgs active`
The currently active organization for the API key. Returns whichever record the API key currently targets.
```bash
its dokploy orgs active
its dokploy orgs active --json
```

## compose

### `its dokploy compose get <composeId>`
Get a docker-compose stack by composeId. Pass the id (or any natural identifier) as the positional arg.
```bash
its dokploy compose get <compose-id>
its dokploy compose get <compose-id> --json
```

### `its dokploy compose services <composeId>`
List services declared inside a compose stack (compose.loadServices).
```bash
its dokploy compose services <compose-id>
its dokploy compose services <compose-id> --json
```

### `its dokploy compose config <composeId>`
Show the resolved docker-compose YAML Dokploy will deploy (after env substitution).
```bash
its dokploy compose config <compose-id>
its dokploy compose config <compose-id> --json
```

### `its dokploy compose deploy <composeId>`
Trigger a deploy on a compose stack. Trigger a fresh deploy from the wired source.
```bash
its dokploy compose deploy <compose-id>
its dokploy compose deploy <compose-id> --json
```

### `its dokploy compose stop <composeId>`
Stop a compose stack. Stop the resource. Use --confirm if the action is destructive.
```bash
its dokploy compose stop <compose-id>
```

### `its dokploy compose redeploy <composeId>`
Redeploy a compose stack (rebuild + deploy). Redeploys the existing container; doesn't rebuild from source.
```bash
its dokploy compose redeploy <compose-id>
its dokploy compose redeploy <compose-id> --json
```

### `its dokploy compose delete <composeId>`
Delete a compose stack permanently (requires --confirm). Stops the stack first, then removes the record from Dokploy.
Flags: `--confirm` Confirm the delete
```bash
its dokploy compose delete <compose-id> --confirm
```

## backup

### `its dokploy backup get <backupId>`
Get a backup config by id (schedule, destination, prefix). Pass the id (or any natural identifier) as the positional arg.
```bash
its dokploy backup get <backup-id>
its dokploy backup get <backup-id> --json
```

### `its dokploy backup files <destinationId>`
List backup files present in a remote destination (S3/MinIO bucket etc.).
```bash
its dokploy backup files <destination-id>
its dokploy backup files <destination-id> --json
```

### `its dokploy backup run <backupId>`
Trigger a manual backup run for a given backup config (backup.manualBackup).
```bash
its dokploy backup run <backup-id>
its dokploy backup run <backup-id> --json
```

### `its dokploy backup delete <backupId>`
Delete a backup configuration (requires --confirm). Does NOT delete stored backup files on the remote destination.
Flags: `--confirm` Confirm the delete
```bash
its dokploy backup delete <backup-id> --confirm
```

## schedule

### `its dokploy schedule <id>`
List cron schedules attached to an application, compose stack, or the server.
Flags: `--type` Schedule type
```bash
its dokploy schedule <id>
its dokploy schedule <id> --json
its dokploy schedule <id> --watch
```

### `its dokploy schedule get <scheduleId>`
Get a schedule by id. Pass the id (or any natural identifier) as the positional arg.
```bash
its dokploy schedule get <schedule-id>
its dokploy schedule get <schedule-id> --json
```

### `its dokploy schedule run <scheduleId>`
Trigger a schedule manually (schedule.runManually). Execute the named job/script.
```bash
its dokploy schedule run <schedule-id>
its dokploy schedule run <schedule-id> --json
```

### `its dokploy schedule delete <scheduleId>`
Delete a schedule (requires --confirm). Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record.
Flags: `--confirm` Confirm the delete
```bash
its dokploy schedule delete <schedule-id> --confirm
```

## git

### `its dokploy git`
List all configured git providers across GitHub Apps, GitLab OAuth apps, and Bitbucket workspaces.
```bash
its dokploy git
its dokploy git --json
its dokploy git --watch
```

### `its dokploy git setup`
Set up a new git provider. GitHub is UI-only (prints dashboard URL); GitLab/Bitbucket use credentials passed via flags.
Flags: `--type` Git provider type · `--name` Display name (any string — shows in Dokploy UI) · `--url` GitLab instance URL (default https://gitlab.com) · `--application-id` GitLab OAuth Application ID · `--secret` GitLab OAuth Application Secret · `--redirect-uri` GitLab OAuth redirect URI (must match the app config). Defaults to <DOKPLOY_URL>/api/providers/gitlab/callback. · `--group` GitLab group name (optional, scopes to a group) · `--username` Bitbucket username (for type=bitbucket) · `--app-password` Bitbucket app password — generate at https://bitbucket.org/account/settings/app-passwords/ · `--workspace` Bitbucket workspace slug (optional, scopes repo discovery) · `--dry-run` Print the planned request without sending
```bash
its dokploy git setup --type gitlab
its dokploy git setup --type bitbucket
```

### `its dokploy git delete <id>`
Remove a git provider. Pass --type so the right delete endpoint is used.
Flags: `--type` Git provider type · `--confirm` Confirm the delete
```bash
its dokploy git delete <id> --type gitlab --confirm
```
