# Server

**Phase:** 1 (Step 1)

## Goal

Run Home Assistant and companion containers on owned hardware.

## Role

Brain of the house: Minisforum N5 Pro (Ryzen AI 9, 32GB RAM, 1TB SSD) on Debian, Docker for service isolation. HDD bays for media/backups later (Jellyfin etc. are out of scope for this PoC study).

## Depends on

- Physical Mini PC, Ethernet to LAN
- [network](network.md) — stable addressing for the host

## Provides to

- Runtime for HA, Mosquitto, Zigbee2MQTT (and later HACS / Matter stack)
- USB and/or Ethernet for the [radio](radio.md) Dongle-M (PoE preferred)

## Integration path

Debian (headless) → Docker + Compose when Step 1 install starts. HA uses host networking later for LocalTuya mDNS ([fans](fans.md)).

## Notes

- Compose files are not in-repo yet; interaction study only.
- Future services (Jellyfin, Vaultwarden, Gitea) share this host but are not PoC blockers.
