# Heat pump

**Phase:** 1 (Step 2)

## Goal

Control the Atlantic heat pump from Home Assistant (schedules, away, vacation pre-heat).

## Role

Main heating/cooling plant. Target behaviours: lower setpoints at night/away; pre-heat on off-peak before return from vacation.

## Depends on

- [server](server.md) — HA instance
- Overkiz / Cozytouch account (if cloud path) — region EU
- [network](network.md) — fixed IP recommended for the indoor unit / bridge if it has one

## Provides to

- HA — climate entities
- Shared climate story with [vmc](vmc.md) and [fans](fans.md)

## Integration path

**Open.** Preferred candidates:

1. Overkiz (built-in HA) — simplest if Atlantic is on that cloud  
2. Cozytouch custom integration — if Overkiz falls short  

Final choice deferred until credentials and device visibility are verified.

## Notes

- Kept separate from [vmc](vmc.md) even if both land on Overkiz — install path may diverge.
- Cloud dependency is accepted for Phase 1; revisit only if local API appears later.
