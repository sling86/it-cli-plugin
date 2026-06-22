# Entra ID (`entra`)

Microsoft Entra ID (Azure AD) identity management — users, groups, licences, roles, sign-ins, security, directory, onboarding.

> Auto-generated reference. Configure: `its entra setup`. For a command you can name, prefer live help `its entra <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## users

### `its entra users`
List users from the Entra ID directory. Defaults to first 50 — use --all to page through every user, --filter for OData expressions, or --domain for a quick UPN-suffix match.
Flags: `--top` Number of results (max 100) · `--all` Fetch all results (overrides --top) · `--filter` OData filter expression · `--search` Fuzzy substring match across displayName, mail, and UPN (Graph $search) · `--enabled` Only show enabled accounts · `--domain` Filter by UPN domain (e.g. example.com) — server-side endsWith match
```bash
its entra users
its entra users --all
its entra users --enabled
its entra users --domain example.com
its entra users --search smith
its entra users --fields displayName,upn,jobTitle
```

### `its entra users update <id>`
Update user properties. Use named flags for common fields, --set k=v,k=v for any other Graph user property.
Flags: `--displayName` Display name · `--jobTitle` Job title · `--department` Department · `--company` Company name · `--office` Office location · `--phone` Mobile phone · `--employeeId` Employee ID (PeopleHR EmployeeId) · `--streetAddress` Street address · `--city` City · `--state` State/county · `--postalCode` Postal code · `--country` Country · `--usage` Usage location (e.g. GB) · `--manager` Set manager (UPN or GUID) · `--clear-manager` Remove the manager assignment (top of org) · `--set` Freeform Graph property passthrough, comma-separated key=value pairs (e.g. --set extensionAttribute1=foo,givenName=Bar)
```bash
its entra users update jane.smith@example.com --department "Marketing"
its entra users update jane.smith@example.com --manager boss@example.com
its entra users update jane.smith@example.com --set "officeLocation=London,jobTitle=Lead"
```

### `its entra users search <query>`
Fuzzy substring search across displayName, mail, UPN, jobTitle, department. Server-side startsWith for fast filtering — best for partial-name lookups.
```bash
its entra users search "jane"
its entra users search "marketing"
```

### `its entra users get <id>`
Full user profile — display fields, manager, sign-in state, employee metadata, on-prem sync flag, business phones, additional mail aliases.
```bash
its entra users get jane.smith@example.com
its entra users get 8d3b9c8f-...-aaa
its entra users get jane.smith@example.com --json
```

### `its entra users groups <id>`
All groups the user is a direct member of — security, M365, distribution, mail-enabled. Doesn't expand transitive (parent-of-parent) membership; use `users transitive-groups` for that.
```bash
its entra users groups jane.smith@example.com
its entra users groups jane.smith@example.com --json
```

### `its entra users chain <id>`
Follow manager pointers up the org tree and render the chain. Useful for access-request escalation — stops at no-manager or a cycle.
Flags: `--depth` Maximum manager depth to follow (default 10)
```bash
its entra users chain jane.smith@example.com
its entra users chain jane.smith@example.com --json
```

### `its entra users licences <id>`
Per-SKU licence assignments with service-plan level detail — which SKUs the user has, plus which features are enabled or disabled inside each.
```bash
its entra users licences jane.smith@example.com
its entra users licences jane.smith@example.com --json
```

### `its entra users invite`
Send a B2B guest invitation to an external user. Send an invitation; the recipient takes the action.
Flags: `--email` External email address (required) · `--name` Display name (defaults to email prefix) · `--redirect` Post-redemption redirect URL · `--message` Custom invitation message body · `--no-email` Create guest without sending the invitation email · `--usage` ISO country code — also sets usageLocation on the created user
```bash
its entra users invite contractor@vendor.com --redirect "https://myapps.microsoft.com"
its entra users invite contractor@vendor.com --no-email --redirect "https://myapps.microsoft.com"
```

### `its entra users create`
Create a new user. Idempotent on duplicate names — use update/edit to mutate an existing record.
Flags: `--displayName` Display name · `--upn` User principal name · `--password` Initial password · `--jobTitle` Job title · `--department` Department · `--company` Company name · `--office` Office location · `--usage` Usage location (e.g. GB)
```bash
its entra users create --upn jane.smith@example.com --displayName "Jane Smith" --password "TempP@ss!" --force-change
its entra users create --upn jane.smith@example.com --displayName "Jane Smith" --password "TempP@ss!" --force-change --json
```

### `its entra users enable <id>`
Enable a user account. Switch to active state. Idempotent.
```bash
its entra users enable jane.smith@example.com
its entra users enable jane.smith@example.com --json
```

### `its entra users bootstrap-admin <id>`
Macro: create a TAP for the user, then optionally exclude them from CA policies that would block initial sign-in (e.g. phishing-resistant MFA admin policy). Emits a checklist with re-inclusion reminders.
Flags: `--ttl` TAP lifetime (e.g. 1h, 4h, 1d). Default 1h · `--one-time` TAP usable only once · `--no-tap` Skip TAP creation (only run policy exclusions) · `--exclude-policies` Comma-separated CA policy IDs or displayNames to exclude the user from. Default empty — pass the Microsoft-managed phishing-resistant admin policy ID if you need it. · `--dry-run` Print actions without applying
```bash
its entra users bootstrap-admin newadmin@example.com
its entra users bootstrap-admin newadmin@example.com --json
```

### `its entra users stale`
Find users with no recent successful sign-in — candidates for offboarding/clean-up. Defaults to enabled accounts with `lastSuccessfulSignInDateTime` older than 90 days; pass --days, --include-disabled, --include-guests to widen.
Flags: `--days` Threshold — flag accounts with no successful sign-in within this many days (default 90) · `--include-disabled` Include accounts where accountEnabled is false (default: only enabled) · `--include-guests` Include guest (#EXT#) accounts (default: only members) · `--include-never` Also flag accounts that have never had a successful sign-in (lastSuccessfulSignInDateTime is null) · `--top` Page size for the underlying Graph query (default 999) · `--all` Fetch every user (paginates) instead of capping at --top
```bash
its entra users stale
its entra users stale --days 180
its entra users stale --include-disabled
```

### `its entra users disable <id>`
Disable a user account (requires --confirm). Pass --revoke-sessions to also kill active refresh tokens + cookies — recommended during offboarding so the user is forced out of M365 immediately.
Flags: `--confirm` Confirm the disable · `--revoke-sessions` Also call /revokeSignInSessions — invalidates refresh tokens and forces re-auth on all clients
```bash
its entra users disable jane.smith@example.com --confirm
its entra users disable jane.smith@example.com --revoke --confirm
```

### `its entra users revoke-sessions <id>`
Revoke all sign-in sessions for a user (POST /users/{id}/revokeSignInSessions). Forces re-auth on all clients without disabling the account.
Flags: `--confirm` Confirm the revoke
```bash
its entra users revoke-sessions jane.smith@example.com
its entra users revoke-sessions jane.smith@example.com --confirm
```

### `its entra users set-password <id>`
Set a user's password (requires --confirm). The calling principal needs Password Administrator / User Administrator — app-only User.ReadWrite.All returns 403. Add --force-change to require a reset at next sign-in.
Flags: `--password` New password · `--force-change` Force a password change at next sign-in · `--confirm` Confirm the password set

### `its entra users delete <id>`
Soft-delete a user (requires --confirm). Recoverable for 30 days via directory/deletedItems.
Flags: `--confirm` Confirm the deletion

### `its entra users transfer <id>`
Internal transfer (Company A→B): rename UPN + swap primary SMTP (optionally keep old as alias) + set company / department / manager in one pass. Requires --confirm. Cloud-only accounts (proxyAddresses must be Entra-writable).
Flags: `--new-upn` New userPrincipalName (also becomes primary SMTP) · `--new-company` New companyName · `--new-department` New department · `--new-manager` New manager (UPN or GUID) · `--keep-old-smtp-as-alias` Keep the old primary SMTP as a secondary alias · `--confirm` Confirm the transfer

### `its entra users reinstate <id>`
Reverse a recent offboarding (requires --confirm): re-enable the account, optionally reset the password, and replay the directory audit log from the last N minutes to re-add removed group memberships. Licence removals are reported (re-assign manually).
Flags: `--from-audit-minutes` Audit window to replay group re-adds from (default 60) · `--password` Also set a new password · `--force-change` Force password change at next sign-in (with --password) · `--confirm` Confirm the reinstate

## groups

### `its entra groups`
List Entra ID groups. Surfaces the most common fields; pass --json for raw shape. Use --dynamic to narrow to dynamic-membership groups via server-side $filter (auto-fetches all pages).
Flags: `--top` Number of results · `--all` Fetch all results (overrides --top) · `--filter` OData filter expression · `--dynamic` Only dynamic-membership groups. Server-side $filter (groupTypes/any(t:t eq 'DynamicMembership')); auto-fetches all pages so the result is the true tenant count.
```bash
its entra groups --top 200
its entra groups --filter "startswith(displayName,'IT')"
its entra groups --top 200 --watch
```

### `its entra groups search <query>`
Search groups by name. Substring match across the most relevant fields; case-insensitive.
```bash
its entra groups search "all staff"
its entra groups search "all staff" --json
```

### `its entra groups get <group_id>`
Get group details. Pass the id (or any natural identifier) as the positional arg.
```bash
its entra groups get <group-id>
its entra groups get <group-id> --json
```

### `its entra groups members <group_id>`
List members of a group. Pass --include-sps (or --all) to also surface service principals, nested groups, and devices — useful for groups like 'Fabric Admin API Service Principals' that the user-only view shows as empty.
Flags: `--include-sps` Include service principals + non-user members (SPs, nested groups, devices) · `--all` Alias for --include-sps
```bash
its entra groups members <group-id>
its entra groups members <group-id> --all
```

### `its entra groups create`
Create a new security group. Idempotent on duplicate names — use update/edit to mutate an existing record.
Flags: `--displayName` Group display name · `--description` Group description · `--mail` Mail-enabled
```bash
its entra groups create --displayName "Marketing"
its entra groups create --displayName "Marketing" --json
```

### `its entra groups add-member <group_id>`
Add a user to a group. Refuses dynamic-membership groups — Graph accepts the call but the dynamic engine immediately overrides. Also refuses if the candidate is disabled / a leaver / has an active namesake (lesson 326 — Adam picked the wrong Steve/Colette/Nick). Pass --force to override either guard.
Flags: `--user` User ID to add · `--force` Override the dynamic-group guard AND the leaver / disabled / namesake guard · `--skip-leaver-check` Skip just the leaver/disabled/namesake guard (still enforces the dynamic-group guard)
```bash
its entra groups add-member <group-id> --user jane.smith@example.com
its entra groups add-member <group-id> --user jane.smith@example.com --json
```

### `its entra groups remove-member <group_id>`
Remove a user from a group (requires --confirm). Refuses dynamic-membership groups; pass --force to override.
Flags: `--user` User ID to remove · `--confirm` Confirm the removal · `--force` Override the dynamic-group guard
```bash
its entra groups remove-member <group-id> --user jane.smith@example.com --confirm
its entra groups remove-member <group-id> --user jane.smith@example.com --confirm --json
```

### `its entra groups edit-rule <group_id>`
Edit a dynamic group's membershipRule. --add-upn appends an OR exception (grants a user who doesn't match the rule); --remove-upn strips one; --set-rule replaces the whole rule. add/remove never drop existing members. --confirm required; without it, prints the current→new diff
Flags: `--add-upn` Append `or (user.userPrincipalName -eq "<upn>")` to the rule · `--remove-upn` Strip the OR exception for this UPN from the rule · `--set-rule` Replace the ENTIRE membershipRule (re-evaluates the whole group) · `--confirm` Apply the change · `--force` Skip the --add-upn existence check (allow a UPN that doesn't resolve)

### `its entra groups audit-rules [group_id]`
Scan dynamic groups' membershipRules for dead user exceptions — hardcoded userPrincipalName/objectId/mail clauses whose account no longer exists (missing) or is disabled (a leaver still pinned). Pass a group ID to scan one; default scans all dynamic groups. --all also lists the OK refs.
Flags: `--all` Include refs that resolve OK, not just dead/disabled

## licences

### `its entra licences`
List all subscribed SKUs/licences. Available column gets a ⚠ marker when fewer than 10% of seats remain (or none) — useful for catching exhaustion before the next assign request fails.
```bash
its entra licences
its entra licences --filter available=0
its entra licences --json
its entra licences
its entra licences --json
its entra licences --watch
```

### `its entra licences assign <user_id>`
Assign one or more licences to a user. Pass --sku as a comma-separated list to assign several atomically in a single Graph call (parallel single-assigns trip 409 Directory_ConcurrencyViolation). Idempotent.
Flags: `--sku` SKU ID (GUID), or comma-separated list of GUIDs
```bash
its entra licences assign jane@x.com --sku <guid>
its entra licences assign jane@x.com --sku <guid1>,<guid2>,<guid3>
its entra licences assign jane.smith@example.com --sku SPB
its entra licences assign jane.smith@example.com --sku SPB --location GB
```

### `its entra licences remove <user_id>`
Remove a licence from a user (requires --confirm). Permanent — use --confirm.
Flags: `--sku` SKU ID (GUID) · `--confirm` Confirm the removal
```bash
its entra licences remove jane.smith@example.com --sku SPB --confirm
its entra licences remove jane.smith@example.com --all --confirm
```

### `its entra licences users <sku_id>`
List users assigned a specific licence SKU. List by resource membership; use --json for the raw shape.
```bash
its entra licences users --sku SPB
its entra licences users --sku SPB --json
```

### `its entra licences unlicensed`
Find enabled users with no licence assignments. Read-only — useful for billing audits.
```bash
its entra licences unlicensed
its entra licences unlicensed --json
```

### `its entra licences audit`
Licence usage report with availability and over-allocation warnings.
```bash
its entra licences audit
its entra licences audit --json
```

### `its entra licences waste`
Find disabled accounts with reclaimable licences. Read-only — useful for finding reclaimable seats.
Flags: `--sku` Filter to specific SKU ID
```bash
its entra licences waste
its entra licences waste --json
```

## roles

### `its entra roles`
List activated directory roles. Surfaces the most common fields; pass --json for raw shape.
```bash
its entra roles
its entra roles --json
its entra roles --watch
```

### `its entra roles members <role_id>`
List members of a directory role. Returns direct members; nested groups aren't expanded.
```bash
its entra roles members <role-id>
its entra roles members <role-id> --json
```

### `its entra roles assign <user_id>`
Assign a directory role to a user. Idempotent — assigning twice is a no-op.
Flags: `--role` Role definition ID
```bash
its entra roles assign jane.smith@example.com --role <role-def-id>
its entra roles assign jane.smith@example.com --role <role-def-id> --json
```

### `its entra roles remove <user_id>`
Remove a directory role from a user (requires --confirm). Permanent — use --confirm.
Flags: `--role` Role definition ID · `--confirm` Confirm the removal
```bash
its entra roles remove jane.smith@example.com --role <role-def-id> --confirm
```

### `its entra roles assignments`
List all role assignments with role names. Returns the assignment record, not the principal — pair with `users get` for the readable name.
```bash
its entra roles assignments
its entra roles assignments --json
```

## signin

### `its entra signin`
List recent sign-in logs. Surfaces the most common fields; pass --json for raw shape.
Flags: `--filter` OData filter expression · `--top` Number of results · `--all` Fetch all results (overrides --top) · `--user` Filter by user ID or UPN (auto-detected) · `--app` Filter by app display name · `--failed` Only failed sign-ins (errorCode ne 0) · `--status` Filter by sign-in status (alias for --failed=true when 'failure', --failed=false when 'success') · `--since` Look-back window (e.g. 1h, 6h, 1d, 7d)
```bash
its entra signin --since 1h
its entra signin --failed --since 24h
its entra signin --user jane.smith@example.com --since 7d
its entra signin --app "Microsoft Teams" --since 1d
```

### `its entra signin summary <user_id>`
Sign-in summary for a user (last sign-in timestamps). Quick one-screen view — designed for dashboards / `--watch`.
```bash
its entra signin summary jane.smith@example.com
its entra signin summary jane.smith@example.com --json
its entra signin summary jane.smith@example.com --watch
```

### `its entra signin explain <id>`
Decode a single sign-in event: error code, failureReason, and the CA policies that fired (appliedConditionalAccessPolicies). Pass the sign-in ID from `signin list`.
```bash
its entra signin explain <signin-id>
its entra signin explain <signin-id> --json
```

### `its entra signin suspicious`
List sign-ins with risk indicators. Bounded by --since; defaults to last 24h.
Flags: `--days` Look-back period in days
```bash
its entra signin suspicious --days 7
its entra signin suspicious --days 7 --json
```

## tap

### `its entra tap create <user_id>`
Create a Temporary Access Pass for a user (does not work for B2B guests).
Flags: `--ttl` Lifetime (e.g. 30m, 1h, 8h, 1d). Default 1h · `--one-time` Pass is usable only once · `--start-time` ISO 8601 timestamp for scheduled start (e.g. 2026-04-21T09:00:00Z)
```bash
its entra tap create jane.smith@example.com
its entra tap create jane.smith@example.com --lifetime 480 --usable-once false
```

### `its entra tap <user_id>`
List active Temporary Access Passes for a user (codes redacted).
```bash
its entra tap
its entra tap --json
its entra tap --watch
```

### `its entra tap revoke <user_id> <method_id>`
Revoke a Temporary Access Pass. Reverse an assignment. --confirm where required.
```bash
its entra tap revoke jane.smith@example.com <method-id>
its entra tap revoke jane.smith@example.com <method-id> --json
```

## ca

### `its entra ca`
List all Conditional Access policies. Surfaces the most common fields; pass --json for raw shape.
Flags: `--state` Filter by state
```bash
its entra ca
its entra ca --json
its entra ca --watch
```

### `its entra ca enabled`
List only enabled CA policies. Filters by `state eq 'enabled'`.
```bash
its entra ca enabled
its entra ca enabled --json
```

### `its entra ca get <id_or_name>`
Show full CA policy JSON by id or displayName. Pass the id (or any natural identifier) as the positional arg.
```bash
its entra ca get <policy-id>
its entra ca get <policy-id> --json
```

### `its entra ca patch <id_or_name>`
Patch a CA policy (state or full JSON body). PATCH semantics — only the supplied fields change.
Flags: `--state` New state · `--body` Raw JSON body to PATCH (overrides --state) — inline or @path/to/file.json · `--dry-run` Print the PATCH request without sending
```bash
its entra ca patch <policy-id> --state enabled
its entra ca patch <policy-id> --state enabledForReportingButNotEnforced
```

### `its entra ca exclude-guests <id_or_name>`
Add 'All guest and external users' to excludeGuestsOrExternalUsers on a policy.
Flags: `--dry-run` Print the PATCH request without sending
```bash
its entra ca exclude-guests <policy-id>
its entra ca exclude-guests <policy-id> --json
```

### `its entra ca create`
Create a Conditional Access policy from a JSON body or file (@path).
Flags: `--body` Inline JSON or @path/to/policy.json — the POST body for the new policy · `--dry-run` Print the POST request without sending
```bash
its entra ca create @./policy.json
its entra ca create @./policy.json --json
```

### `its entra ca delete <id_or_name>`
Delete a Conditional Access policy by id or displayName. Permanent — use --confirm. Audit trail (if the upstream supports it) keeps the deletion record.
Flags: `--dry-run` Print the DELETE request without sending
```bash
its entra ca delete "Block legacy auth" --confirm
```

### `its entra ca exclude-user <id_or_name> <user>`
Add a user to a CA policy's excludeUsers — atomic mutation, no need to rebuild the users object.
Flags: `--dry-run` Print the PATCH request without sending
```bash
its entra ca exclude-user <policy-id> --user jane.smith@example.com
its entra ca exclude-user "Require MFA for admins" --user jane.smith@example.com
```

### `its entra ca unexclude-user <id_or_name> <user>`
Remove a user from a CA policy's excludeUsers list. Reverse of `exclude-user`. Idempotent.
Flags: `--dry-run` Print the PATCH request without sending
```bash
its entra ca unexclude-user <policy-id> jane.smith@example.com
its entra ca unexclude-user <policy-id> jane.smith@example.com --json
```

### `its entra ca why-blocked <user>`
Show which enabled CA policies target a user, plus the grant controls each requires. Resolves group + role membership.
Flags: `--include-disabled` Also evaluate disabled and reportOnly policies
```bash
its entra ca why-blocked jane.smith@example.com
its entra ca why-blocked jane.smith@example.com --json
```

### `its entra ca named-locations`
List trusted/named locations referenced by CA policies.
```bash
its entra ca named-locations
its entra ca named-locations --json
```

## authmethods

### `its entra authmethods policy`
Fetch the full Authentication Methods Policy — shows which methods (TAP, FIDO2, SMS, etc.) are enabled tenant-wide and any per-method targets.
```bash
its entra authmethods policy
its entra authmethods policy --json
```

### `its entra authmethods get <method>`
Get a single authentication method configuration by id (TemporaryAccessPass, Fido2, Sms, MicrosoftAuthenticator, Email, Voice, X509Certificate, etc.).
```bash
its entra authmethods get TemporaryAccessPass
its entra authmethods get TemporaryAccessPass --json
```

### `its entra authmethods enable <method>`
Enable a method tenant-wide (sets state=enabled on the method configuration). Pass --target <groupId> or --target all-users to scope.
Flags: `--target` Optional include target — group ID or 'all_users'. Adds a default include target if specified. · `--dry-run` Print the PATCH request without sending
```bash
its entra authmethods enable Fido2
its entra authmethods enable Fido2 --json
```

### `its entra authmethods disable <method>`
Disable a method tenant-wide (state=disabled). Switch to inactive state. Idempotent.
Flags: `--dry-run` Print the PATCH request without sending
```bash
its entra authmethods disable Sms
its entra authmethods disable Sms --json
```

### `its entra authmethods patch <method>`
Patch a method configuration with a custom body (inline or @path/to/file.json).
Flags: `--body` Inline JSON or @path/to/body.json — the PATCH body for the method config · `--dry-run` Print the PATCH request without sending
```bash
its entra authmethods patch TemporaryAccessPass @./tap-policy.json
its entra authmethods patch TemporaryAccessPass @./tap-policy.json --json
```

## consent

### `its entra consent <client_app_id>`
List delegated OAuth2 permission grants for a client app (existing consent).
Flags: `--resource-app-id` Filter by resource appId (default: Microsoft Graph). Use '*' for all.
```bash
its entra consent <app-id>
its entra consent <app-id> --json
its entra consent <app-id> --watch
```

### `its entra consent add-scope <client_app_id> <scopes>`
Safely add delegated scope(s) to an app — UNION with existing scope, never replaces.
Flags: `--resource-app-id` Resource appId (default: Microsoft Graph) · `--principal-id` Grant on behalf of a specific user (consentType=Principal). Default: AllPrincipals (tenant-wide) · `--dry-run` Print the request without sending
```bash
its entra consent add-scope <app-id> Mail.Read
its entra consent add-scope <app-id> Mail.Read --json
```

### `its entra consent remove-scope <client_app_id> <scopes>`
Remove delegated scope(s) from an existing grant — keeps others intact.
Flags: `--resource-app-id` Resource appId (default: Microsoft Graph) · `--principal-id` Per-user grant — default AllPrincipals (tenant-wide) · `--dry-run` Print the request without sending
```bash
its entra consent remove-scope <app-id> Mail.Read
its entra consent remove-scope <app-id> Mail.Read --json
```

### `its entra consent app-roles <client_app_id>`
List application-permission grants (appRoleAssignments) for a client app.
```bash
its entra consent app-roles <app-id>
its entra consent app-roles <app-id> --json
```

### `its entra consent app-role-grant <client_app_id> <role>`
Grant an application-permission (app-role) to a client app — equivalent to consenting an Application permission in the portal.
Flags: `--resource-app-id` Resource appId (default: Microsoft Graph) · `--dry-run` Print the request without sending
```bash
its entra consent app-role-grant <app-id> User.Read.All
its entra consent app-role-grant <app-id> User.Read.All --json
```

### `its entra consent app-role-revoke <client_app_id> <assignment_id>`
Revoke an app-role assignment by id. Revokes a previously-granted app-role grant. Permanent.
Flags: `--resource-app-id` Resource appId (default: Microsoft Graph) · `--confirm` Confirm the revoke
```bash
its entra consent app-role-revoke <app-id> <assignment-id>
its entra consent app-role-revoke <app-id> <assignment-id> --json
```

## xtenant

### `its entra xtenant default`
Show the default cross-tenant access policy. Returns the tenant-default policy / record.
```bash
its entra xtenant default
its entra xtenant default --json
```

### `its entra xtenant trust-mfa <on_off>`
Toggle inboundTrust.isMfaAccepted on the default policy.
Flags: `--dry-run` Print the PATCH without sending
```bash
its entra xtenant trust-mfa on
its entra xtenant trust-mfa on --json
```

### `its entra xtenant trust-device <on_off>`
Toggle inboundTrust.isCompliantDeviceAccepted on the default policy.
Flags: `--dry-run` Print the PATCH without sending
```bash
its entra xtenant trust-device on
its entra xtenant trust-device on --json
```

### `its entra xtenant partners`
List per-partner cross-tenant access overrides. Returns per-partner overrides — use `partner-set` to mutate.
```bash
its entra xtenant partners
its entra xtenant partners --json
```

### `its entra xtenant partner-add <tenant_id>`
Start a per-partner cross-tenant access config. Bootstraps a fresh per-partner override record.
```bash
its entra xtenant partner-add <partner-tenant-id>
its entra xtenant partner-add <partner-tenant-id> --json
```

### `its entra xtenant partner-set <tenant_id>`
Patch a partner cross-tenant access record. Patches an existing partner record.
Flags: `--trust-mfa` on or off · `--trust-device` on or off · `--trust-haadj` on or off (Hybrid Azure AD Joined) · `--dry-run` Print the PATCH without sending
```bash
its entra xtenant partner-set <partner-tenant-id> --trust-mfa on
its entra xtenant partner-set <partner-tenant-id> --trust-mfa on --json
```

## audit

### `its entra audit`
List directory audit logs. Surfaces the most common fields; pass --json for raw shape.
Flags: `--filter` OData filter expression · `--top` Number of results · `--all` Fetch all results (overrides --top) · `--category` Filter by category · `--initiated-by` Filter by initiating user ID (GUID) or app ID · `--target` Filter by target resource ID (GUID) · `--activity` Filter by activityDisplayName (exact match, e.g. 'Add member to group') · `--result` Filter by result
```bash
its entra audit --since 24h
its entra audit --user jane.smith@example.com
its entra audit --since 24h --watch
```

## security

### `its entra security risky`
List risky users from Identity Protection. Returns users with non-none risk score.
```bash
its entra security risky
its entra security risky --json
```

### `its entra security mfa <user_id>`
Check MFA readiness for a user. Returns user's strong-auth methods plus a readiness flag.
```bash
its entra security mfa
its entra security mfa --json
```

### `its entra security admin-mfa`
Audit all admin users for MFA readiness. Audit pass — flags any admin without strong MFA.
```bash
its entra security admin-mfa
its entra security admin-mfa --json
```

## directory

### `its entra directory org`
Get tenant organisation details. Returns the tenant organisation profile.
```bash
its entra directory org
its entra directory org --json
```

### `its entra directory domains`
List verified domains. Returns verified + provisioning domains.
```bash
its entra directory domains
its entra directory domains --json
```

### `its entra directory deleted`
List recently deleted users. Returns soft-deleted users still within the 30-day restore window.
Flags: `--top` Number of results · `--all` Fetch all results (overrides --top)
```bash
its entra directory deleted
its entra directory deleted --json
```

### `its entra directory tree <user_id>`
Build org tree from a root user (recursive). Recursive — pass --depth to bound the walk.
Flags: `--depth` Max depth (1-5, default 3)
```bash
its entra directory tree
its entra directory tree --json
```

### `its entra directory summary`
Aggregate users by company, department, location. Quick one-screen view — designed for dashboards / `--watch`.
```bash
its entra directory summary
its entra directory summary --json
its entra directory summary --watch
```

### `its entra directory app-usage`
Licence and app usage summary. Aggregated app + licence usage across the tenant.
```bash
its entra directory app-usage
its entra directory app-usage --json
```

## onboarding

### `its entra onboarding summary <user_id>`
Onboarding snapshot: user profile, licences, groups, manager.
```bash
its entra onboarding summary jane.smith@example.com
its entra onboarding summary jane.smith@example.com --json
its entra onboarding summary jane.smith@example.com --watch
```

### `its entra onboarding copy-groups`
Copy group memberships from one user to another. Idempotent — already-shared groups are skipped.
Flags: `--source` Source user ID or UPN · `--target` Target user ID or UPN
```bash
its entra onboarding copy-groups --from peer@example.com --to jane.smith@example.com
its entra onboarding copy-groups --from peer@example.com --to jane.smith@example.com --json
```

### `its entra onboarding convert-mailbox <user_id>`
Convert user mailbox to shared (disable + remove licences). Requires --confirm.
Flags: `--confirm` Confirm the conversion
```bash
its entra onboarding convert-mailbox jane.smith@example.com --confirm
its entra onboarding convert-mailbox jane.smith@example.com --confirm --json
```

## offboarding

### `its entra offboarding summary <user_id>`
Offboarding checklist: disable account, list licences and groups to remove.
```bash
its entra offboarding summary jane.smith@example.com
its entra offboarding summary jane.smith@example.com --json
its entra offboarding summary jane.smith@example.com --watch
```

### `its entra offboarding run <user_id>`
Execute offboarding: disable account, revoke licences, remove from groups. Requires --confirm.
Flags: `--confirm` Confirm the offboarding
```bash
its entra offboarding run jane.smith@example.com --confirm
its entra offboarding run jane.smith@example.com --confirm --json
```

## break-glass

### `its entra break-glass audit`
Audit break-glass accounts (BG01 + BG02) — GA role, CA exclusions, sign-in hygiene, password age, FIDO2. Non-zero exit on any gap. Override UPNs with ENTRA_BG01_UPN / ENTRA_BG02_UPN.
Flags: `--bg01` Override BG01 UPN (defaults to ENTRA_BG01_UPN env) · `--bg02` Override BG02 UPN (defaults to ENTRA_BG02_UPN env)
```bash
its entra break-glass audit
its entra break-glass audit --json
```

## whoami

### `its entra whoami show`
Dump the current Entra CLIENT_ID, app display name, Graph app-role assignments (application permissions), delegated scopes, and the live access token's claims. Use before any mutation to check 'will I have permission'
```bash
its entra whoami
its entra whoami --json
```

## doctor

### `its entra doctor`
Parallel health check on tenant — expiring secrets, admin MFA gaps, risky users, licensed-disabled accounts, stale guests.
Flags: `--secret-warn-days` Flag app credentials expiring within this many days (default 60) · `--stale-days` Threshold for stale enabled accounts (default 90)
```bash
its entra doctor
its entra doctor --json
its entra doctor --watch
```

## apps

### `its entra apps register <name>`
Bootstrap a new Entra app registration end-to-end — creates the application, its service principal, and a 12-month client secret. Returns the appId, objectId, SP id, and the plaintext secret (only shown once).
Flags: `--redirect-uri` Web redirect URI. Repeatable — pass multiple --redirect-uri flags or comma-separate. · `--audience` Sign-in audience · `--secret-name` Display name for the client secret · `--months` Client secret lifetime in months · `--no-secret` Skip generating a client secret

### `its entra apps add-password <app>`
Rotate / add a client secret on an existing app registration. The plaintext secretText is only returned once.
Flags: `--name` Display name for the secret · `--months` Lifetime in months

## admin-bootstrap

### `its entra admin-bootstrap run <user_id>`
Unblock an admin who can't sign in for phishing-resistant MFA. Tries TAP first; falls back to a per-policy excludeUsers on the Microsoft-managed phish-resistant admin policy. Always emits a re-inclusion checklist.
Flags: `--ttl` TAP lifetime in minutes (default 60) · `--one-time` TAP is single-use · `--no-fallback` Don't exclude from the phish-resistant policy if TAP fails — just report. · `--dry-run` Show the plan without mutating

## graph

### `its entra graph get <path>`
Raw Graph GET — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal) · `--raw` Return the response body as raw bytes (no JSON decode). Required for binary endpoints like /content. Currently honoured by the `sp` provider. · `--out` Write the response to this file path instead of stdout. Implies --raw.
```bash
its entra graph get "/users?$top=5"
its entra graph get "/identityGovernance" --beta
```

### `its entra graph post <path>`
Raw Graph POST — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--body` Request body — inline JSON string or @file.json to read from disk · `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its entra graph post "/users/$count" --body @./payload.json
its entra graph post "/users/$count" --body @./payload.json --json
```

### `its entra graph patch <path>`
Raw Graph PATCH — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--body` Request body — inline JSON string or @file.json to read from disk · `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its entra graph patch "/users/<id>" --body '{"jobTitle":"Lead"}'
its entra graph patch "/users/<id>" --body --json
```

### `its entra graph put <path>`
Raw Graph PUT — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--body` Request body — inline JSON string or @file.json to read from disk · `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its entra graph put "/users/<id>/photo/$value" --body @./photo.jpg
its entra graph put "/users/<id>/photo/$value" --body @./photo.jpg --json
```

### `its entra graph delete <path>`
Raw Graph DELETE — pass any /v1.0 or /beta path (use --beta for beta).
Flags: `--beta` Use /beta instead of /v1.0 · `--header` Extra headers as comma-separated K=V pairs (e.g. Prefer=return=minimal)
```bash
its entra graph delete "/users/<id>" --confirm
```
