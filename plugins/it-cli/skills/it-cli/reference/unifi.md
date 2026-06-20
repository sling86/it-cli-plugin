# UniFi Network (`unifi`)

UniFi Network controller — devices, clients, WLANs, networks, firewall, events, alarms, vouchers.

> Auto-generated reference. Configure: `its unifi setup`. For a command you can name, prefer live help `its unifi <resource> help` (always current) — read this file to discover what exists. [Index](./index.md)

## sites

### `its unifi sites`
List all UniFi sites. Surfaces the most common fields; pass --json for raw shape.
Flags: `--site` Site name override
```bash
its unifi sites
its unifi sites --json
its unifi sites --watch
```

### `its unifi sites health`
Site health — WAN/WLAN/LAN subsystem status. Live health check across the resource's dependencies.
Flags: `--site` Site name override
```bash
its unifi sites health
its unifi sites health --json
its unifi sites health --watch
```

### `its unifi sites sysinfo`
Controller version, uptime, and IP addresses. Returns the controller version + uptime.
Flags: `--site` Site name override
```bash
its unifi sites sysinfo
its unifi sites sysinfo --json
```

## devices

### `its unifi devices`
List all UniFi devices with name, MAC, IP, type, state, uptime, clients.
Flags: `--type` Filter by type (ap|switch|gateway|all) · `--site` Site name override
```bash
its unifi devices
its unifi devices --site "Office"
its unifi devices --watch
```

### `its unifi devices get <mac>`
Get device detail by MAC address. Pass the id (or any natural identifier) as the positional arg.
Flags: `--site` Site name override
```bash
its unifi devices get <mac>
its unifi devices get <mac> --json
```

### `its unifi devices restart <mac>`
Restart a device by MAC address. Stop + start in one call.
Flags: `--confirm` Confirm the restart · `--site` Site name override
```bash
its unifi devices restart <mac> --confirm
its unifi devices restart <mac> --confirm --json
```

### `its unifi devices locate <mac>`
Toggle locate LED on a device. Flash the device's locate LED.
Flags: `--enable` Enable locate LED (default true, pass --no-enable to disable) · `--site` Site name override
```bash
its unifi devices locate <mac>
its unifi devices locate <mac> --json
```

### `its unifi devices upgrade <mac>`
Trigger firmware upgrade on a device. Trigger a firmware upgrade.
Flags: `--confirm` Confirm the upgrade · `--site` Site name override
```bash
its unifi devices upgrade <mac> --confirm
its unifi devices upgrade <mac> --confirm --json
```

### `its unifi devices provision <mac>`
Force re-provision a device. Force the device to re-pull its provisioning.
Flags: `--site` Site name override
```bash
its unifi devices provision <mac>
its unifi devices provision <mac> --json
```

### `its unifi devices power-cycle`
PoE power cycle a switch port. Cycle PoE on a port. --confirm required.
Flags: `--mac` Switch MAC address · `--port` Port index to power cycle · `--confirm` Confirm the power cycle · `--site` Site name override
```bash
its unifi devices power-cycle --mac <mac> --port 5
its unifi devices power-cycle --mac <mac> --port 5 --json
```

### `its unifi devices leds`
Toggle all site LEDs on or off. Toggle the controller's status LEDs.
Flags: `--enable` Enable LEDs (true/false) · `--site` Site name override
```bash
its unifi devices leds off
its unifi devices leds off --json
```

### `its unifi devices poe`
PoE power summary for all switches. Returns the PoE budget + per-port consumption.
Flags: `--site` Site name override
```bash
its unifi devices poe
its unifi devices poe --json
```

## clients

### `its unifi clients`
List online clients. Surfaces the most common fields; pass --json for raw shape.
Flags: `--filter` Filter by connection type (wired|wireless|all) · `--site` Site name override
```bash
its unifi clients
its unifi clients --filter wireless
its unifi clients --filter wired
```

### `its unifi clients get <mac>`
Get client detail by MAC address. Pass the id (or any natural identifier) as the positional arg.
Flags: `--site` Site name override
```bash
its unifi clients get <mac>
its unifi clients get <mac> --json
```

### `its unifi clients search <query>`
Search all known clients by name, hostname, IP, or MAC. Substring match across the most relevant fields; case-insensitive.
Flags: `--site` Site name override
```bash
its unifi clients search "jane"
its unifi clients search "jane" --json
```

### `its unifi clients block <mac>`
Block a client by MAC address. Blocks a client. Reversible via `unblock`.
Flags: `--confirm` Confirm the block · `--site` Site name override
```bash
its unifi clients block <mac>
its unifi clients block <mac> --json
```

### `its unifi clients unblock <mac>`
Unblock a client by MAC address. Re-allows a previously blocked client.
Flags: `--site` Site name override
```bash
its unifi clients unblock <mac>
its unifi clients unblock <mac> --json
```

### `its unifi clients reconnect <mac>`
Force reconnect a client. Force a client to disassociate + re-auth.
Flags: `--site` Site name override
```bash
its unifi clients reconnect <mac>
its unifi clients reconnect <mac> --json
```

### `its unifi clients offline`
List recently disconnected clients. Returns recently-disconnected clients.
Flags: `--hours` Look-back period in hours (default 24) · `--site` Site name override
```bash
its unifi clients offline
its unifi clients offline --json
```

## guests

### `its unifi guests authorise <mac>`
Authorise a guest WiFi client. Permits a guest network device.
Flags: `--minutes` Duration in minutes · `--up` Upload speed limit (Kbps) · `--down` Download speed limit (Kbps) · `--megabytes` Data transfer limit (MB) · `--site` Site name override
```bash
its unifi guests authorise <mac> --duration 1440
its unifi guests authorise <mac> --duration 1440 --json
```

### `its unifi guests unauthorise <mac>`
Revoke guest WiFi authorisation. Revokes a guest authorisation.
Flags: `--site` Site name override
```bash
its unifi guests unauthorise <mac>
its unifi guests unauthorise <mac> --json
```

## networks

### `its unifi networks`
List networks and VLANs. Surfaces the most common fields; pass --json for raw shape.
Flags: `--site` Site name override
```bash
its unifi networks
its unifi networks --json
its unifi networks --watch
```

## wlans

### `its unifi wlans`
List WiFi SSIDs. Surfaces the most common fields; pass --json for raw shape.
Flags: `--site` Site name override
```bash
its unifi wlans
its unifi wlans --json
its unifi wlans --watch
```

### `its unifi wlans toggle <wlan_id>`
Enable or disable a WiFi SSID. Toggle a boolean state; idempotent.
Flags: `--enable` Enable (true) or disable (false) the SSID · `--site` Site name override
```bash
its unifi wlans toggle <wlan-id>
its unifi wlans toggle <wlan-id> --enable
```

### `its unifi wlans password <wlan_id>`
Update WiFi password for an SSID. Rotate a PSK / passphrase. --confirm required.
Flags: `--passphrase` New WiFi passphrase · `--site` Site name override
```bash
its unifi wlans password <wlan-id> --password "new-password"
its unifi wlans password <wlan-id> --password "new-password" --json
```

## firewall

### `its unifi firewall`
List firewall rules. Surfaces the most common fields; pass --json for raw shape.
Flags: `--site` Site name override
```bash
its unifi firewall
its unifi firewall --json
its unifi firewall --watch
```

### `its unifi firewall groups`
List firewall groups. List groups for a resource.
Flags: `--site` Site name override
```bash
its unifi firewall groups
its unifi firewall groups --json
```

## routes

### `its unifi routes`
List static routes. Surfaces the most common fields; pass --json for raw shape.
Flags: `--site` Site name override
```bash
its unifi routes
its unifi routes --json
its unifi routes --watch
```

## portforwards

### `its unifi portforwards`
List every WAN port-forward rule — your inbound attack surface. Columns: WAN port → forward IP:port, protocol and source restriction ("any" = open to the whole internet). Pass --json for the raw shape.
Flags: `--site` Site name override
```bash
its unifi portforwards
its unifi port-forwards
its unifi portforwards --site t7kq3dcp
its unifi portforwards --json
```

## port-forwards

### `its unifi port-forwards`
List every WAN port-forward rule — your inbound attack surface. Columns: WAN port → forward IP:port, protocol and source restriction ("any" = open to the whole internet). Pass --json for the raw shape.
Flags: `--site` Site name override
```bash
its unifi portforwards
its unifi port-forwards
its unifi portforwards --site t7kq3dcp
its unifi portforwards --json
```

## ports

### `its unifi ports`
List every WAN port-forward rule — your inbound attack surface. Columns: WAN port → forward IP:port, protocol and source restriction ("any" = open to the whole internet). Pass --json for the raw shape.
Flags: `--site` Site name override
```bash
its unifi portforwards
its unifi port-forwards
its unifi portforwards --site t7kq3dcp
its unifi portforwards --json
its unifi ports
its unifi ports --json
its unifi ports --watch
```

## events

### `its unifi events`
List recent events. Surfaces the most common fields; pass --json for raw shape.
Flags: `--hours` Look-back period in hours (default 24) · `--limit` Maximum number of events (default 50) · `--site` Site name override
```bash
its unifi events --since 1h
its unifi events --since 1h --json
its unifi events --since 1h --watch
```

## alarms

### `its unifi alarms`
List alarms with archived status. Surfaces the most common fields; pass --json for raw shape.
Flags: `--site` Site name override
```bash
its unifi alarms
its unifi alarms --json
its unifi alarms --watch
```

### `its unifi alarms count`
Count active (non-archived) alarms. Returns a single number — cheap for thresholds.
Flags: `--site` Site name override
```bash
its unifi alarms count
its unifi alarms count --json
```

### `its unifi alarms archive`
Archive all alarms. Archive (soft-delete) the record. Reversible.
Flags: `--confirm` Confirm archiving all alarms · `--site` Site name override
```bash
its unifi alarms archive --confirm
its unifi alarms archive --confirm --json
```

## rogue

### `its unifi rogue`
List detected rogue access points. Surfaces the most common fields; pass --json for raw shape.
Flags: `--hours` Look-back period in hours (default 24) · `--site` Site name override
```bash
its unifi rogue
its unifi rogue --json
its unifi rogue --watch
```

## vouchers

### `its unifi vouchers`
List guest WiFi vouchers. Surfaces the most common fields; pass --json for raw shape.
Flags: `--site` Site name override
```bash
its unifi vouchers
its unifi vouchers --json
its unifi vouchers --watch
```

### `its unifi vouchers create`
Create guest WiFi vouchers. Idempotent on duplicate names — use update/edit to mutate an existing record.
Flags: `--minutes` Voucher duration in minutes · `--count` Number of vouchers to create (default 1) · `--quota` Number of uses per voucher (0 = unlimited, default 0) · `--note` Note to attach to the vouchers · `--site` Site name override
```bash
its unifi vouchers create --duration 1440 --count 5
its unifi vouchers create --duration 1440 --count 5 --json
```

### `its unifi vouchers revoke <id>`
Revoke/delete a guest voucher. Reverse an assignment. --confirm where required.
Flags: `--confirm` Confirm the revocation · `--site` Site name override
```bash
its unifi vouchers revoke <voucher-id> --confirm
its unifi vouchers revoke <voucher-id> --confirm --json
```

## dashboard

### `its unifi dashboard`
Comprehensive site overview — health, devices, clients, alarms.
Flags: `--site` Site name override
```bash
its unifi dashboard
its unifi dashboard --json
its unifi dashboard --watch
```

## audit

### `its unifi audit`
Audit RF and firewall/VLAN posture — co-channel overlap, unisolated VLANs, insecure WiFi defaults.
Flags: `--site` Site name override
