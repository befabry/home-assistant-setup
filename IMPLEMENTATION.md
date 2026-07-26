# Implementation roadmap

Gradual move from study to live house. Compose and HA YAML arrive when Step 1 install starts — this file stays the checklist only.

Phase 1 spend ceiling (server + radio + switches + fans + starter sensors): roughly €1900–2500.

---

## Step 1 — Infrastructure

**Goal:** Host runs; Zigbee mesh has a coordinator and early routers.

**Pieces:** [server](pieces/server.md), [network](pieces/network.md), [radio](pieces/radio.md), [switches](pieces/switches.md)

**Prerequisites:** Mini PC available; SONOFF Dongle Max (Dongle-M); router admin access for DHCP reservations.

**Done when:**
- [ ] Debian + Docker on the Mini PC
- [ ] HA + Mosquitto + Zigbee2MQTT containers up (compose added then)
- [ ] Dongle visible to Z2M via `/dev/serial/by-id`
- [ ] DHCP reservations set for known Wi-Fi/Ethernet smart devices
- [ ] First Zigbee switches paired (routers strengthening mesh)

---

## Step 2 — HVAC

**Goal:** Heat pump and VMC controllable from HA (or path chosen if Overkiz is not enough).

**Pieces:** [heat-pump](pieces/heat-pump.md), [vmc](pieces/vmc.md)

**Prerequisites:** Step 1 done; Overkiz/Cozytouch credentials if using cloud path.

**Done when:**
- [ ] Heat pump entities in HA (or documented fallback)
- [ ] VMC path decided and working (Overkiz / Aldes / switch + sensor)
- [ ] Basic schedule or vacation pre-heat sketch noted

---

## Step 3 — Fans (mandatory)

**Goal:** Automate all five Tuya DC fans (season / hours / temp / light-by-hour); LocalTuya is the plumbing.

**Pieces:** [fans](pieces/fans.md) — depends on [network](pieces/network.md) fixed IPs

**Prerequisites:** Step 1 host networking suitable for LocalTuya/mDNS; DHCP reservations for each fan; room temp sensors ideal before tuning.

**Done when:**
- [ ] LocalTuya (HACS) installed
- [ ] All 5 fans discovered (on/off, direction, speed, light + brightness)
- [ ] Summer hour/temp → speed automation live (thresholds tunable)
- [ ] Winter reverse window automation live
- [ ] Fan-light brightness-by-hour when light is on

---

## Step 4 — Energy & lighting

**Goal:** Plugs as routers + energy view; motion lighting basics.

**Pieces:** [energy](pieces/energy.md), [lighting](pieces/lighting.md)

**Prerequisites:** Zigbee mesh healthy (Step 1 switches/radio).

**Done when:**
- [ ] Zigbee plugs paired (prefer energy-monitoring models)
- [ ] Motion → light automation for at least one zone
- [ ] Optional: switch double-tap / scene hook noted

---

## Steps 5–6 — Security & remote access

**Goal:** Lock + doorbell path; secure remote use from GrapheneOS Companion (+ TV dashboards).

**Pieces:** [security](pieces/security.md), [remote-access](pieces/remote-access.md)

**Prerequisites:** Dongle-M Thread/OTBR path up ([radio](pieces/radio.md)) for Matter-over-Thread lock.

**Done when:**
- [ ] Matter integration + Dongle-M OTBR/TBR verified
- [ ] Aqara U400 paired (Matter-over-Thread)
- [ ] Doorbell/camera RTSP path with fixed IP
- [ ] GrapheneOS Companion on LAN; remote path chosen: DuckDNS + LE **or** NordVPN Meshnet
- [ ] Optional: HA dashboard on Android TV / Xiaomi TV box

---

## Spring 2026 — Garden

**Goal:** Irrigation with moisture/weather/tank guards; outdoor Zigbee via PoE.

**Pieces:** [garden](pieces/garden.md)

**Prerequisites:** Outdoor Cat6 + PoE; SLZB-06; indoor mesh strong toward garden side ([switches](pieces/switches.md)).

**Done when:**
- [ ] SLZB-06 online with fixed IP; second Zigbee coordinator in Z2M
- [ ] Valve, pump, soil moisture, tank level paired
- [ ] Automation: moisture + no rain + tank OK → water; manual override on dashboard
