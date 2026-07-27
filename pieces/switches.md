# Switches

**Phase:** 1 Zigbee (Step 1); Matter later (Phase 2)

## Goal

Put a mains Zigbee **router** in each important room so battery sensors can talk reliably — and control light (and fan, where needed) from the wall.

## Role

Primary intent in this house:

1. **Mesh anchor** — router-class wall module (neutral) stays powered and carries traffic for room temp/humidity. Fans are Wi-Fi; they do not help here.
2. **Wall control** — light via real relay; fan (if present) via detach → HA → LocalTuya only.
3. Fan mains stay permanent; light may be cut by its relay.

Matter switches do **not** route Zigbee unless they are Zigbee-based with a Matter bridge. Matter-over-Wi‑Fi modules do not grow Zigbee or Thread.

## Pick dual-gang or single-gang

| Box / need | Use |
|------------|-----|
| Fan room — light + fan paddle in one box | **Dual-gang** |
| Light only, single paddle, or no room for dual | **Single-gang** |
| No neutral / skip wall work | Zigbee plug or USB repeater — [outlets](outlets.md) |

Both classes are Zigbee **routers** (neutral required). Same Z2M detach idea on the channel that must stay hot.

## Dual-gang module

Fan rooms: mesh + light relay + detached fan paddle → HA.

| Product | Model | Pick when |
|---------|-------|-----------|
| **Orb-ZBW2** | `MINI-ZB2GS-E` | You want the Fusion faceplate kit |
| **MINI DUO** | `MINI-ZB2GS` | [DIY module](https://sonoff.tech/en-fr/products/sonoff-mini-duo-2-gang-zigbee-smart-switch-mini-zb2gs) behind existing paddles |

Same chip, wiring, and Z2M behaviour: dual-channel Zigbee **router**, neutral required, per-channel detach.

### Room pattern

| Gang | Load | Detach relay | Behaviour |
|------|------|--------------|-----------|
| **1 — Light** | Ceiling / room light | `DISABLE` | Paddle toggles relay → light on/off as usual |
| **2 — Fan** | Tuya fan feed | `ENABLE` | Relay left **on** (fan always powered); paddle → `action: toggle_l2` → HA → LocalTuya on/off |

Wiring sketch: L+N to module; gang-1 load = light; gang-2 load = fan **with detach on**. Do not run fan without detach / always-hot.

Z2M: `detach_relay_outlet1` / `detach_relay_outlet2` independently. Detached actions are `toggle_l1` / `toggle_l2` only — **not** long-press or multi-click for HA.

### Gestures / dimming / fan speed (ceiling)

| Want | On dual-gang? |
|------|------------------|
| Light on/off | Yes — gang 1 relay |
| Fan on/off, fan stays powered | Yes — gang 2 detach + HA → LocalTuya |
| Long-press → light brightness | **No** — relay switch, Z2M only exposes `toggle_l*`; not a dimmer |
| Multi-press → fan speed steps | **No** as HA events. Device “stepper” is for its own relays and **conflicts with detach** (Sonoff: detach disables stepper) |

For brightness/speed from the wall: not dual-gang’s job — fan remotes / Companion, or HA automations ([fans](fans.md)). Optional later tactile pad is comfort only.

## Single-gang module

Light-only boxes, corridors, or “mesh + one load” when dual does not fit.

| Product | Model | Pick when |
|---------|-------|-----------|
| **ZBMINIR2** | `ZBMINIR2` | [Default simple](https://sonoff.tech/products/sonoff-zbmini-extreme-zigbee-smart-switch-zbminir2) — router + relay + detach. “Extreme” is the product line name, not extra features. |
| **MINI ONE PM** | `MINI-ZB1GSP` | Same job **plus** power metering — only if you care about Wh on that circuit |

Both: Zigbee **router**, neutral required, detach in Z2M. No dimmer / multi-press for HA.

| Load | Detach | Behaviour |
|------|--------|-----------|
| Room light | `DISABLE` | Paddle toggles relay |
| Fan only (no light gang) | `ENABLE` | Relay left on; paddle → HA → LocalTuya |
| Smart bulb (always powered) | `ENABLE` | Paddle → HA / scene; bulb stays on mesh |

Z2M: `detach_relay_outlet1` (`ENABLE` / `DISABLE`). Detached action is toggle-style only — not a dimmer.

## Depends on

- [radio](radio.md) — Zigbee2MQTT / Dongle-M
- Neutral wire in the switch box

## Provides to

- Zigbee mesh — parents for battery temp/humidity (and motion) in that room
- HA — light switch entity; dual-gang also fan `action` / automations
- [lighting](lighting.md) — relay load
- [fans](fans.md) — dual-gang gang 2 (or single detached) → LocalTuya
- [outlets](outlets.md) / [energy](energy.md) — optional metering only if you bought `MINI-ZB1GSP`

## Integration path

- Dual- or single-gang → Zigbee2MQTT → HA  
- Fan automation (when used): paddle action → `fan.turn_on` / `turn_off` (LocalTuya entity)  
- **Later:** Matter wall gear only if needed ([security](security.md)) — does not replace Zigbee routers

## Router vs end-device (do not mix up)

| Model | Zigbee role | Neutral | Use |
|-------|-------------|---------|-----|
| **Dual-gang** | Router | Required | Fan rooms — see [Dual-gang module](#dual-gang-module) |
| **Single-gang** (`ZBMINIR2`, optional `MINI-ZB1GSP`) | Router | Required | Light-only / simple — see [Single-gang module](#single-gang-module) |
| **ZBMINIL2** | End-device | Not required | No-neutral only — **no** mesh help |
| ZBM5 | Router (typical) | Check model | Other faceplate layouts |

With L+N, the module stays on the mesh even when the light relay is open.

Plugs remain a mesh fallback — [outlets](outlets.md) — if a room has no neutral or you skip wall work. Thread outlets are a **different** mesh — same file / [radio](radio.md).

## Notes

- Fan rooms: one dual-gang covers mesh + light + fan paddle.
- Other rooms: one single-gang is enough for mesh + light.
- Place a router toward the garden elevation early.
- Battery sensors never route; ZBMINIL2 never routes.
