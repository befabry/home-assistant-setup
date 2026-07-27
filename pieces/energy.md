# Energy

**Phase:** 1 (Step 4) — view; shedding can wait until presence is trustworthy

## Goal

See consumption of key always-on / high-draw loads, and later cut non-essentials when away. Hardware that reports power lives on [outlets](outlets.md) (and optional metering switches) — this piece is the **automations and dashboards**, not the mesh plan.

## Role

- **Observe:** power / energy entities from metering Zigbee plugs (or `MINI-ZB1GSP`) into HA history + a simple energy view.
- **Act (later):** turn off shedable loads when the house is empty — never fans, heat pump, or VMC as a blunt “away” kill.

Zigbee/Thread mesh strategy is [outlets](outlets.md) and [switches](switches.md), not here.

## Depends on

- [outlets](outlets.md) — metering Zigbee plugs (or wall metering switch)
- [radio](radio.md) — Zigbee2MQTT
- Optional [remote-access](remote-access.md) — presence for away shedding

## Provides to

- HA — energy dashboard / history; away load-shedding automations when ready

## Integration path

Metering devices already in Z2M → pick entities for the HA energy dashboard → optional automations on power thresholds or presence.

No separate energy gateway planned for Phase 1.

## Notes

- Prefer metering plugs on loads you care about (PC, washer, always-on junk); mesh-only plugs need not meter.
- Do not put consumption goals on Thread H2 outlets — they exist for the lock mesh ([outlets](outlets.md)).
- Fans stay Wi-Fi / LocalTuya; do not use plug cut-off as fan “off”.
