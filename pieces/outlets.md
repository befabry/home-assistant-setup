# Outlets

**Phase:** 1 Zigbee plugs (Step 4); Matter-over-Thread outlets in Phase 2 with the lock

## Goal

Place **mains-powered routers** where wall switches are not enough — Zigbee for sensors, Thread for the lock path. Optional metering on Zigbee plugs feeds [energy](energy.md).

## Role

Two radios, two jobs (do not mix):

1. **Zigbee plugs / USB repeaters** — grow the Zigbee mesh without rewiring every light box; optional power sensors for [energy](energy.md).
2. **Matter-over-Thread outlets** — grow the **Thread** mesh for the U400. They do **not** help Zigbee.

Fans are Wi-Fi; they help neither. Matter-over-Wi‑Fi outlets help neither.

Wall switches ([switches](switches.md)) stay the preferred Zigbee mesh in rooms you rewire; outlets fill gaps and the Thread path.

## Zigbee plugs (Phase 1)

| Product | Use |
|---------|-----|
| Sonoff S40ZB Lite / S60ZB (etc.) | Easy Zigbee router; prefer metering models if you also want [energy](energy.md) |
| ZBMICRO on USB PSU | Pure Zigbee repeater (no switched load) |
| Wall `MINI-ZB1GSP` | Metering on a switched circuit — [switches](switches.md); else `ZBMINIR2` or a plug |

Path: Zigbee plug → Zigbee2MQTT → HA.

## Thread outlets (Phase 2)

Mains Matter-over-**Thread** only. Pair in **Thread/Matter mode**, not Zigbee mode.

| Product | Form | Pick when |
|---------|------|-----------|
| **Aqara Power Plug H2 EU** | Plug-in | No rewiring; quick hop on the lock RF path |
| **Aqara Wall Outlet H2 EU** | [In-wall Schuko](https://www.aqara.com/en/product/wall-outlet-h2-eu/) (55 mm plate) | Permanent socket near door / weak Thread path |

Path: outlet → Thread → Dongle-M OTBR → HA Matter — [radio](radio.md), [security](security.md).

Zigbee mode on an H2 only grows Zigbee (redundant if wall switches already cover that room).

## Depends on

- [radio](radio.md) — Zigbee2MQTT for Zigbee plugs; OTBR + Matter for Thread outlets
- Always-powered socket (or USB PSU for repeaters)

## Provides to

- Zigbee mesh — easy routers where [switches](switches.md) are absent
- Thread mesh — routers between Dongle-M and the lock when range is weak
- [energy](energy.md) — power/energy entities from metering Zigbee plugs (and optional `MINI-ZB1GSP`)
- HA — switch entities (cut loads that *may* be shed; not fans)

## Integration path

1. **Zigbee:** plug → Z2M → HA  
2. **Thread:** H2 in Thread mode → Matter → HA (after OTBR is up)

## Notes

- Leave Zigbee plugs on loads that stay powered. Do not hard-cut a Tuya fan’s supply.
- Mesh-only Zigbee: cheap router plug or ZBMICRO is enough; metering is for [energy](energy.md).
- Thread placement: RF path between Dongle-M and the door; always-on load (lamp, charger).
