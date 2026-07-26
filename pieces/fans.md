# Fans

**Phase:** 1 (Step 3) — mandatory

## Goal

Automate five Tuya DC ceiling fans from Home Assistant: season, hours, temperature, and fan-light brightness by time of day. Local control via LocalTuya is the means; **automations are the target**.

## Role

- **Summer:** in set hours, if room temp in a band → fan on, forward, speed from temp.
- **Winter:** in set hours → reverse, low speed (push warm air down); optional link to [heat-pump](heat-pump.md).
- **Fan light:** when on, brightness follows an hour-of-day curve (HA sets it — no wall dimmer).
- Manual override: Orb-ZBW2 on/off, fan remote, Companion.

Cloud Tuya avoided; LocalTuya on LAN.

## Depends on

- [server](server.md) — HA with host networking (or macvlan) for mDNS / discovery
- [network](network.md) — **fixed IP per fan** (DHCP reservation mandatory)
- LocalTuya via HACS — entities: on/off, **direction**, **speed %**, light on/off + **brightness**
- Room temp sensors (Zigbee via [switches](switches.md) / [radio](radio.md) mesh)
- Optional [switches](switches.md) — wall on/off into HA (not fan mains)

## Provides to

- HA — fan + light entities and the climate/comfort automations below

## Integration path

Tuya Wi-Fi fans → LocalTuya (HACS) → Home Assistant → **automations**.

Wall: dual-gang **Orb-ZBW2** ([switches](switches.md)) — gang 1 room light or unused; gang 2 fan feed with **detach** (fan always powered). Remotes for occasional manual dim/speed.

Never put a phase-cut dimmer or classic multi-speed switch on the fan mains.

## Target automations

Tune numbers live once hardware is up; don’t freeze thresholds in docs.

**Summer — cool / circulate**

- Between hours `H1–H2`: if room temp in `T_low–T_high` → fan on, direction forward, speed from a small table (warmer → higher %).
- Outside hours or below `T_low` → off (or low, if you prefer continuous air).

**Winter — push heat down**

- Between hours `H3–H4`: direction **reverse**, low/fixed speed; ignore summer temp table.
- Optional: only when heat pump is heating.

**Fan light — brightness by hour (when on)**

- Trigger: light turned on, or periodic while on.
- Action: set brightness from time of day (e.g. brighter morning, dimmer evening).
- Manual change: either stick until next on, or get overwritten on the next schedule tick — pick one when implementing.

ponytail: ship one automation package after all five fans + per-room temp exist.

## Manual / later (not the goal)

- Fan RF remote or Companion for override.
- Optional later: battery Zigbee rotary/scene pad or a small wall panel → HA → LocalTuya if you want tactile dim/speed. Comfort only; automations stay primary.

## Notes

- Fans are **Wi-Fi**, not Zigbee. Mesh for sensors = [switches](switches.md) / [energy](energy.md).
- Orb-ZBW2 fan gang: `detach_relay_outlet` ENABLE, relay left on.
- Fan-integrated light is LocalTuya only; a separate room light can use a real relay on its own circuit.
- If a fan goes “offline” after reboot, check DHCP reservation first.
- Five fans in scope for Step 3 — see [IMPLEMENTATION](../IMPLEMENTATION.md).
