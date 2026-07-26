# Energy

**Phase:** 1 (Step 4)

## Goal

See consumption of key always-on / high-draw loads and cut non-essentials when away.

## Role

Zigbee smart plugs (and optional USB repeaters): measure usage where useful, and — more importantly early on — **propagate the Zigbee mesh** without rewiring fan or light boxes.

## Depends on

- [radio](radio.md) — Zigbee2MQTT

## Provides to

- HA — power/energy sensors + switch entities via MQTT
- Zigbee mesh — primary easy routers in rooms that have [fans](fans.md) or weak RF
- Away / load-shedding for loads that *may* be cut (not the fans themselves)

## Integration path

Zigbee plugs (e.g. Sonoff S40ZB Lite / S60ZB) → Zigbee2MQTT → HA. Optional pure repeaters (e.g. ZBMICRO on a USB PSU) when you only need mesh, not a switched outlet.

## Notes

- **Yes — sockets are the easy Zigbee mesh path.** Fans are Wi-Fi; Zigbee mesh lives on Zigbee plugs/repeaters/[switches](switches.md). For **Thread** mesh (U400), use separate Matter-over-Thread plugs — [radio](radio.md) (Aqara H2 in Thread mode).
- Leave plugs on loads that stay powered (or use a repeater). Do not use a plug to hard-cut a Tuya fan’s supply.
- Prefer metering plugs when you also want energy view; cheap router plugs/repeaters are enough for mesh-only.
- No separate energy gateway planned for Phase 1.
