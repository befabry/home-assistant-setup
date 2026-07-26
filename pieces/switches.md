# Switches

**Phase:** 1 Zigbee (Step 1); Matter later (Phase 2)

## Goal

Put a mains Zigbee **router** in each important room so battery sensors can talk reliably — and drive fan + light from one dual-gang wall switch.

## Role

Primary intent in this house:

1. **Mesh anchor** — router-class wall module (neutral) stays powered and carries traffic for room temp/humidity. Fans are Wi-Fi; they do not help here.
2. **Dual-gang control** — one paddle for the light (real relay), one for the fan (HA → LocalTuya only).
3. Fan mains stay permanent; light may be cut by its relay.

Matter switches do **not** route Zigbee unless they are Zigbee-based with a Matter bridge.

## Proposed room pattern (default)

**Hardware:** Sonoff **Orb-ZBW2** (model **MINI-ZB2GS-E**) — dual-channel, Zigbee **router**, neutral required, per-channel detach in Z2M.

| Gang | Load | Detach relay | Behaviour |
|------|------|--------------|-----------|
| **1 — Light** | Ceiling / room light | `DISABLE` | Paddle toggles relay → light on/off as usual |
| **2 — Fan** | Tuya fan feed | `ENABLE` | Relay left **on** (fan always powered); paddle → `action: toggle_l2` → HA → LocalTuya on/off |

Wiring sketch: L+N to module; gang-1 load = light; gang-2 load = fan **with detach on**. Do not run fan without detach / always-hot.

Z2M: `detach_relay_outlet1` / `detach_relay_outlet2` independently. Detached actions are `toggle_l1` / `toggle_l2` only — **not** long-press or multi-click for HA.

### Gestures / dimming / fan speed (ceiling)

| Want | On MINI-ZB2GS-E? |
|------|------------------|
| Light on/off | Yes — gang 1 relay |
| Fan on/off, fan stays powered | Yes — gang 2 detach + HA → LocalTuya |
| Long-press → light brightness | **No** — relay switch, Z2M only exposes `toggle_l*`; not a dimmer |
| Multi-press → fan speed steps | **No** as HA events. Device “stepper” is for its own relays and **conflicts with detach** (Sonoff: detach disables stepper) |

For brightness/speed from the wall: not Orb-ZBW2’s job — fan remotes / Companion, or HA automations ([fans](fans.md)). Optional later tactile pad is comfort only.

Fallback if the box is single-gang or no room for dual: **ZBMINIR2** on the light (or detached single for fan events) + separate light control.

## Depends on

- [radio](radio.md) — Zigbee2MQTT / Dongle-M
- Neutral wire in the switch box

## Provides to

- Zigbee mesh — parents for battery temp/humidity (and motion) in that room
- HA — two channels: light switch entity + fan `action` / automations
- [lighting](lighting.md) — gang 1 relay
- [fans](fans.md) — gang 2 events → LocalTuya

## Integration path

- Orb-ZBW2 → Zigbee2MQTT → HA  
- Gang 2 automation: paddle action → `fan.turn_on` / `turn_off` (LocalTuya entity)  
- **Later:** Matter wall gear only if needed ([security](security.md))

## Router vs end-device (do not mix up)

| Model | Zigbee role | Neutral | Use |
|-------|-------------|---------|-----|
| **Orb-ZBW2 (MINI-ZB2GS-E)** | Router | Required | **Default** dual-gang: light + fan |
| **ZBMINIR2** | Router | Required | Single-gang / behind existing plate |
| **ZBMINIL2** | End-device | Not required | No-neutral only — **no** mesh help |
| ZBM5, MINI-ZB1GSP | Router (typical) | Check model | Other layouts if Orb-ZBW2 does not fit |

With L+N, the module stays on the mesh even when the light relay is open.

Plugs remain a mesh fallback — [energy](energy.md) — if a room has no neutral or you skip wall work.

## Notes

- One Orb-ZBW2 per fan room covers mesh + light + fan paddle.
- Place a router toward the garden elevation early.
- Battery sensors never route; ZBMINIL2 never routes.
