# Casa Amado — Self-Hosted Smart Home

A fully self-hosted, local-first home automation system for a private residence,
built with a "zero critical cloud dependencies" philosophy: all critical
automation runs locally, with cloud services used only for non-critical
access layers.

## Infrastructure

- **Virtualization:** Proxmox VE, running multiple isolated LXC containers
  (one application per container, no Docker-in-LXC) for services such as
  DNS/ad-blocking, reverse proxy, network controller, monitoring, and
  password management.
- **Home Automation Core:** Home Assistant OS, with configuration fully
  managed as modular YAML (`packages/` structure, one file per domain:
  energy, climate, security, presence, voice).
- **Zigbee:** Zigbee2MQTT with a dedicated coordinator, following a strict
  physical/logical entity abstraction pattern so hardware can be swapped
  without touching dashboards or automations.
- **Device firmware:** ESPHome for custom sensors (e.g. water flow
  metering with persistent counters).
- **Network:** VLAN-segmented (management, servers, IoT, cameras, guest,
  energy), managed through a dedicated SDN controller.
- **Voice assistant:** Fully local speech-to-text and text-to-speech
  pipeline, with cloud LLM used only for non-critical intent handling
  (no code execution, no persistent memory of sensitive data).

## Energy Management

- Local Modbus/TCP integration with a hybrid solar inverter and battery
  storage system — no cloud dependency for monitoring or automation.
- Custom energy dashboards showing production, consumption, battery
  state of charge, and self-sufficiency ratio.
- `utility_meter` helpers for monthly/yearly consumption and cost
  tracking, feeding into a personal energy report workflow.

## Security & Access

- Reverse-proxied remote access via a zero-trust tunnel, with no open
  inbound ports on the residential router.
- Two-factor authentication enforced on all administrative interfaces.
- Self-hosted password manager for credential storage, with root/recovery
  credentials kept offline in physical form.

## Design Philosophy

- **Stability over features.** When choosing between a feature-rich
  fragile option and a simpler stable one, stability wins.
- **Local-first.** Every automation critical to daily living (energy,
  climate, security) must continue working without an internet
  connection.
- **Modular & maintainable.** Consistent naming conventions, no code
  duplication, clear separation between physical hardware entities and
  the logical entities automations depend on.

## Home Appliance Integration Goal

Currently integrating Home Connect–compatible appliances (Siemens
dishwasher) into the Home Assistant instance for status monitoring and
energy consumption tracking, as part of the broader energy dashboard
described above.

---

*This is a private residential project. Some implementation details are
intentionally omitted from this public description.*
