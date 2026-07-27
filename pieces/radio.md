# Radio (Zigbee + Matter / Thread)

**Phase:** 1 (Step 1); Matter/Thread hardened in Phase 2

## Goal

Bridge Zigbee and Thread radios into Home Assistant on one piece of hardware: **SONOFF Dongle Max (Dongle-M)**.

## Role

**Dongle-M** (ESP32 + EFR32MG24, PoE / Ethernet / Wi-Fi / USB): Zigbee coordinator for sensors, plugs, switches; and — when Thread firmware / OTBR is enabled — the **Thread Border Router** for Matter-over-Thread devices (e.g. lock).

### What “Thread Border Router” means

Thread is a low-power mesh (like Zigbee’s cousin). Matter-over-Thread gadgets speak Thread on the air, not Wi-Fi. A **Thread Border Router (TBR)** is the gateway that translates between that Thread mesh and your normal LAN/IP network where HA lives. Without a TBR, the lock never reaches Home Assistant.

On this stack the TBR is not a TV or phone: it is the Dongle-M in Thread/OTBR mode (OpenThread Border Router add-on/app talking to the stick’s Thread radio), plus HA’s Matter integration.

## Depends on

- [server](server.md) — HA + Z2M + Mosquitto (USB **or** TCP to Dongle-M)
- [network](network.md) — **fixed IP** if Dongle-M is on Ethernet/PoE/Wi-Fi
- PoE injector/switch or USB-C power

## Provides to

- [switches](switches.md), [outlets](outlets.md), [lighting](lighting.md), [garden](garden.md) — Zigbee mesh
- [security](security.md) — Thread/Matter path when OTBR is up on this dongle
- HA — Zigbee via MQTT (Z2M); Matter-over-Thread via Matter + OTBR

## Integration path

1. **Zigbee (Phase 1):** Dongle-M → Zigbee2MQTT (USB or `tcp://…`) → Mosquitto → HA  
2. **Thread/Matter (Phase 2):** same Dongle-M as OpenThread RCP → HA OTBR → Thread network → Matter (Aqara U400 lock, etc.)

Firmware/mode is switched from the Dongle-M web console. Prefer Z2M over ZHA for Zigbee. Do not run ZHA and Z2M on the same radio.

**Both at once:** Dongle-M can do Zigbee + Thread (Sonoff multiprotocol / Silicon Labs multiprotocol path). That is the intended “one box” setup; if multiprotocol misbehaves, fall back to Zigbee-only on Dongle-M and add a second Thread stick later — not the expected plan.

## Two meshes (do not mix)

| Mesh | Grown by | Not grown by |
|------|----------|--------------|
| **Zigbee** | Dual-/single-gang switches ([switches](switches.md)), Zigbee plugs ([outlets](outlets.md)) | Matter-over-Wi‑Fi, Thread-mode H2 |
| **Thread** | Matter-over-Thread outlets/plugs ([outlets](outlets.md)) | Zigbee switches/plugs, Matter-over-Wi‑Fi |

## Notes

- PoE placement can be away from the Mini PC (better RF, less USB fuss) — treat it like a small network appliance with a DHCP reservation. Prefer Dongle-M near the door if the lock is the weak link.
- If Thread range is still weak: add a mains **Matter-over-Thread** outlet as a Thread router — Aqara **Power Plug H2** (plug-in) or **Wall Outlet H2 EU** (in-wall). Pair in **Thread/Matter mode**, not Zigbee. Product table: [outlets](outlets.md).
- Zigbee plugs and Matter-over-Wi‑Fi do **not** extend Thread. Thread outlets do **not** extend Zigbee.
- Android TV / Xiaomi box are HA clients only ([remote-access](remote-access.md)), not a TBR.
- Garden may still add a second Zigbee coordinator (SLZB-06) later — see [garden](garden.md).