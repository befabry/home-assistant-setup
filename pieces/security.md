# Security

**Phase:** 2 (Steps 5–6)

## Goal

Lock, doorbell, and camera signals into HA with local streams where possible.

## Role

Aqara **U400** (Matter-over-Thread) for access; Aqara G400 (or similar) doorbell with RTSP/ONVIF plus Matter doorbell events. Presence + lock automations (auto-lock on leave; unlock via phone/NFC/fingerprint patterns later).

## Depends on

- HA Matter integration
- Thread Border Router via [radio](radio.md) — **Dongle-M + OTBR** (same box; must be in Thread/OTBR role, not Zigbee-only)
- [network](network.md) — **fixed IP for cameras** (RTSP); fixed IP for Dongle-M if on PoE/Ethernet
- [remote-access](remote-access.md) — GrapheneOS Companion notifications away from home

## Provides to

- HA — lock, camera, doorbell entities
- Presence / arrival automations
- Dashboard security tab (later UI work)

## Integration path

1. Lock: Matter-over-Thread → HA Matter (Dongle-M must be running as TBR/OTBR, not Zigbee-only)  
2. Camera/doorbell: RTSP/ONVIF into HA; Matter for button events if offered  

## Notes

- Android TV / Xiaomi TV box are **clients** (dashboards), not Thread Border Routers.
- “Dongle does Zigbee + Thread” still means **enable OTBR / Thread mode** for the lock — Zigbee-only firmware is not enough.
- U400 has **no Wi-Fi** — Thread + Matter only; Dongle-M OTBR is mandatory for HA.
- UWB / Apple Home Key auto-unlock needs Apple gear — skip in this house. Use fingerprint, code, NFC, physical key, and HA/Matter remote instead.
- If the lock is flaky at the door: move Dongle-M closer (PoE), then drop an **Aqara Power Plug H2** (Thread mode) between dongle and door ([radio](radio.md)).
