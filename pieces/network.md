# Network

**Phase:** 1 (Step 1)

## Goal

Stable addressing so local integrations survive reboots.

## Role

LAN backbone + DHCP reservations for every Wi-Fi/Ethernet device that HA talks to by IP. Zigbee/Matter addressing is separate ([radio](radio.md), [switches](switches.md)).

## Depends on

- Router with DHCP reservation / static lease support

## Provides to

- [fans](fans.md), cameras ([security](security.md)), garden PoE gateway ([garden](garden.md)), **Dongle-M** if on Ethernet/PoE/Wi-Fi ([radio](radio.md)) — mandatory fixed IPs
- Heat pump / VMC / Matter Wi-Fi switches — recommended fixed IPs
- [server](server.md) — known host address on LAN

## Integration path

Router DHCP reservations (not static config on every device). Suggested ranges:

| Range | Purpose |
|-------|---------|
| `.1–.50` | Router, switches, infrastructure |
| `.51–.100` | Dynamic pool (phones, guests) |
| `.101–.150` | Smart home devices |
| `.151–.200` | Servers / Docker hosts |

## Notes

- LocalTuya and RTSP break silently when IPs drift — fix reservations once, reference this file from device pieces.
- 2.4 GHz Wi-Fi for Tuya fans; Ethernet for server and future garden PoE.
