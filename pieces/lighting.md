# Lighting

**Phase:** 1 (Step 4)

## Goal

Motion- and schedule-driven lights without cloud bulbs if avoidable.

## Role

Hallway/bathroom motion lights; sunset outdoor/garden lights later; room scenes from [switches](switches.md) (e.g. paddle → light off, optionally same event turns [fans](fans.md) off via LocalTuya).

## Depends on

- [radio](radio.md) — Zigbee bulbs/sensors or switch-controlled loads
- Motion / climate sensors (battery Zigbee OK; they do not route — need a room [switches](switches.md) router or [outlets](outlets.md) plug)
- [switches](switches.md) for relay loads and/or HA events

## Provides to

- HA — light / binary_sensor entities
- Occupancy-flavoured automations
- Optional scene hooks for dashboards / Companion ([remote-access](remote-access.md))

## Integration path

Zigbee bulbs, motion sensors, and switch loads → Zigbee2MQTT → HA. Shared HA automations can bind one paddle to light + fan without the fan sharing the switched circuit.

## Notes

- Prefer Zigbee wall routers on light circuits (mesh + load) over smart bulbs where rewiring allows.
- Sunset automations can wait until outdoor fixtures exist ([garden](garden.md)).
