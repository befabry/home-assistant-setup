# Garden

**Phase:** 3 (Spring / Summer 2026)

## Goal

Irrigate from rainwater with moisture, weather, and tank guards; extend Zigbee outdoors without RF hope alone.

## Role

Outdoor PoE Zigbee coordinator (SMLIGHT SLZB-06) on buried/outdoor Cat6; valve, pump (smart plug), soil moisture, tank level, weather/rain. Automations skip watering when wet forecast or tank low.

## Depends on

- Outdoor Cat6 + 802.3af PoE + IP65 enclosure
- [network](network.md) — **fixed IP for SLZB-06**
- [radio](radio.md) — second coordinator in Zigbee2MQTT
- Indoor Zigbee routers toward garden ([switches](switches.md))
- Weather source (OpenWeatherMap or local rain sensor)

## Provides to

- HA — valve, pump, moisture, level entities
- Irrigation automation + manual override
- Stronger outdoor Zigbee for future sensors

## Integration path

SLZB-06 (Ethernet/PoE) → Zigbee2MQTT (secondary adapter) → MQTT → HA. Valves/sensors pair to the garden coordinator. Pump via Zigbee/Wi-Fi plug ([energy](energy.md) pattern).

Logic sketch: daily morning trigger → soil dry AND no rain soon AND tank > threshold → run pump/valve for N minutes; emergency stop if no flow.

## Notes

- Not started until 2026; keep mesh placement in mind during Phase 1 switch installs.
- Garden Zigbee devices offline → check PoE and SLZB-06 reservation before RF blame.
