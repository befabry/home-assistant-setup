# Remote access

**Phase:** 2 (Steps 5–6); Companion usable on LAN from Step 1

## Goal

Use Home Assistant from phone and TVs at home and away without exposing more than needed.

## Role

**GrapheneOS phone** — primary UI via HA Companion (Android); presence source (Wi-Fi / location as the OS and app allow).  
**Android TV** and optional **Xiaomi TV (3rd gen) box** — living-room dashboards / casting clients only.  
Remote path: HTTPS via DuckDNS + Let’s Encrypt **or** NordVPN Meshnet (already subscribed).

## Depends on

- [server](server.md) — HA reachable on LAN
- Optional reverse proxy later (Nginx Proxy Manager) if using DuckDNS + LE

## Provides to

- User — dashboards, notifications, lock/camera alerts
- Automations — phone-based presence / arrival
- TVs — wall/lean-back view of HA (not Thread/Matter infrastructure)

## Integration path

1. Local: HA IP / hostname on LAN (phone + Android/Xiaomi TV clients)  
2. Remote: **either** DuckDNS + LE **or** NordVPN Meshnet (pick one primary)

## Notes

- Thread Border Router is dedicated hardware ([security](security.md)), not the Android/Xiaomi TVs.
- GrapheneOS: Companion works as an Android app; reliable push may need sandboxed Play services or an alternate notify path (e.g. ntfy) — decide when enabling remote alerts.
- Meshnet avoids opening HA to the public internet; DuckDNS is the classic HA pattern.
