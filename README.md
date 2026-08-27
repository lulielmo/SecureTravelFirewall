# SecureTravelFirewall

A reproducible OpenWrt-based travel firewall for secure use on untrusted networks.

SecureTravelFirewall is intended to place a dedicated security layer between a trusted client device and an untrusted access network such as hotel, airport, conference, café, or other guest Wi-Fi.

The project starts with a personal travel-firewall use case, while keeping a future enterprise-oriented design in mind.

## Core idea

The protected client should never connect directly to the untrusted network. The firewall connects outward, while the client communicates only with the protected side of the firewall.

Normal operation should prefer authenticated VPN transport. Direct Internet access, when needed for exceptional cases such as captive portals or emergency web access, should be explicit, constrained, and temporary.

## Documentation

- [`docs/project-principles.md`](docs/project-principles.md) — stable architectural and project principles.
- [`docs/architecture.md`](docs/architecture.md) — evolving technical architecture.
- [`docs/roadmap.md`](docs/roadmap.md) — planned capabilities and development stages.
- [`docs/testing-strategy.md`](docs/testing-strategy.md) — how security properties are verified.

## Status

Early prototype / architecture phase.

The first reference platform is an OpenWrt-compatible Netgear router. The long-term goal is to keep the SecureTravelFirewall security model as hardware-independent as practical.
