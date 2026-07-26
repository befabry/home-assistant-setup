# VMC (mechanical ventilation)

**Phase:** 1 (Step 2)

## Goal

Drive ventilation speed from humidity / temperature (and manual override).

## Role

House air exchange. Raise speed when humidity or temp crosses thresholds; coordinate with [heat-pump](heat-pump.md) climate strategy.

## Depends on

- [server](server.md) — HA
- Humidity/temp sensors (Zigbee via [radio](radio.md) or built-in from HVAC brand)
- [network](network.md) — fixed IP if the VMC bridge is IP-based

## Provides to

- HA — fan/speed or switch entities depending on path
- Comfort automations alongside heat pump and [fans](fans.md)

## Integration path

**Open.** Candidates in preference order:

1. Overkiz (same ecosystem as Atlantic, if exposed)  
2. Aldes MQTT bridge (if brand/API matches)  
3. Fallback: smart switch + humidity sensor (coarse on/off or stepped control)

Decide after inventorying the actual VMC brand and whether it appears under Overkiz.

## Notes

- Separate file from heat pump on purpose — fallback may be switch-based while heat pump stays cloud.
- Document the chosen path here once locked; leave the others struck or deleted.
