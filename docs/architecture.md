# Architecture

This document will describe the evolving technical architecture of SecureTravelFirewall.

The stable architectural intent is captured separately in [`project-principles.md`](project-principles.md).

## Initial conceptual model

```text
Trusted client
     |
     v
SecureTravelFirewall
     |
     +-- hostile uplink (Wi-Fi or Ethernet)
     |
     +== authenticated VPN transport == secure destination
```

The first prototype focuses on a protected wired client, a hostile Wi-Fi uplink, fail-closed VPN-first routing, and minimal service exposure.

Detailed interface mapping and implementation decisions will be added as the reference platform is configured and tested.
