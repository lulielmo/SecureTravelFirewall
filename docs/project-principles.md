# SecureTravelFirewall Project Principles

This document captures the stable principles that guide the design and development of SecureTravelFirewall.

The purpose is to preserve architectural intent as implementation details evolve. Specific mechanisms may change over time, but changes that contradict these principles should be treated as deliberate architectural decisions and reviewed accordingly.

## 1. Treat the uplink network as hostile

Any external network used by SecureTravelFirewall is considered untrusted by default.

This includes hotel, airport, conference, café, public, guest, and other third-party networks. The design must not depend on the uplink behaving correctly or honestly.

## 2. The protected client does not join the hostile network

A protected laptop, phone, or other endpoint should communicate only with the protected side of SecureTravelFirewall.

The endpoint should not need credentials, addressing, routing information, DNS information, or any other configuration from the hostile uplink network.

SecureTravelFirewall is the only component that connects directly to the hostile network.

## 3. VPN-first operation

The normal communication path for protected clients is through an authenticated and encrypted VPN transport.

If the VPN becomes unavailable, client traffic must not silently fall back to direct Internet access through the hostile network.

Loss of VPN connectivity should result in a fail-closed state.

## 4. Direct Internet access is an explicit exception

Some situations require communication before a VPN can be established, for example captive-portal login or emergency web access.

Such access must be:

- explicitly initiated,
- constrained to the minimum practical scope,
- temporary,
- clearly distinguishable from normal secure operation, and
- removed automatically or explicitly when no longer needed.

Direct access must never become an implicit fallback path.

## 5. Captive-portal onboarding is a distinct security state

Connecting to networks that require voucher entry, payment, terms acceptance, or other browser-based interaction is a required future capability.

Captive-portal interaction should occur in a dedicated onboarding mode with deliberately limited connectivity.

Successful portal authentication should transition the device toward normal VPN-protected operation, not toward unrestricted direct Internet access.

## 6. OpenWrt is the baseline platform

SecureTravelFirewall is built as a reproducible transformation of a defined OpenWrt baseline release.

A SecureTravelFirewall release must identify the OpenWrt release against which it was designed and tested.

Changing the OpenWrt baseline is considered a product change and requires regression testing.

## 7. SecureTravelFirewall is a transformation layer, not a fork by default

Where practical, the project should rely on:

- configuration,
- package selection,
- scripts,
- declarative device profiles,
- policy files, and
- additional project-specific components

rather than maintaining a long-lived fork of OpenWrt.

The preferred build model is:

```text
Official OpenWrt baseline
        +
SecureTravelFirewall configuration and tooling
        +
Device profile
        =
Reproducible SecureTravelFirewall instance
```

## 8. Configuration must be reproducible and version-controlled

The state required to recreate a SecureTravelFirewall instance must live in the repository rather than only on a configured device.

Manual changes made through LuCI or the command line are acceptable during exploration, but accepted configuration changes should subsequently be captured in version-controlled configuration or automation.

The target is configuration-as-code rather than appliance-by-hand.

## 9. Prefer declarative and idempotent automation

Installation and configuration tooling should describe desired state where practical and should be safe to re-run.

Automation should fail clearly when prerequisites are not met instead of producing a partially configured security device.

Examples of conditions that should be detected include:

- unexpected OpenWrt version,
- unsupported device capabilities,
- insufficient storage,
- unavailable required packages, and
- incompatible network or radio topology.

## 10. Security policy should be hardware-independent

The security model must be expressed in generic logical roles wherever possible, such as:

- hostile uplink,
- protected LAN,
- protected Wi-Fi,
- management interface,
- VPN transport, and
- emergency/onboarding path.

Hardware-specific profiles should map physical radios, ports, switches, and interfaces to these roles.

Security policy should not be rewritten separately for each router model.

## 11. Hardware support is evidence-based

SecureTravelFirewall should not claim support for every OpenWrt-compatible device.

Device support should be classified according to verification evidence. A possible support model is:

- **Reference** — actively developed and tested by project maintainers.
- **Verified** — full required test suite completed successfully on the device.
- **Experimental** — basic functionality demonstrated, but full verification incomplete.
- **Unknown** — OpenWrt may support the hardware, but SecureTravelFirewall has not been verified on it.

Community contributors are encouraged to add device profiles and verification evidence for hardware unavailable to the maintainers.

## 12. Community hardware contributions must not weaken security requirements

Hardware compatibility work may adapt how the generic model maps to a device, but it must not silently weaken the security model.

A device limitation must be documented as a limitation rather than worked around by relaxing a security requirement without explicit architectural review.

## 13. The test suite is part of the product

Security claims should be accompanied by repeatable tests wherever practical.

The project should strive to represent important security properties as:

```text
Requirement
    -> implementation
    -> verification procedure
    -> expected result
    -> observed result
```

Examples include tests for:

- WAN-to-LAN isolation,
- VPN kill-switch behavior,
- DNS leakage,
- IPv6 bypass,
- management-plane exposure,
- unexpected broadcast or multicast leakage,
- captive-portal isolation, and
- hostile-network manipulation.

Testing documentation and tooling should be published alongside the firewall implementation so users can verify the security properties themselves.

## 14. The hostile-network lab is a first-class development tool

The project should maintain a repeatable hostile-network test environment capable of simulating malicious or malformed network behavior.

The purpose is not only penetration testing, but validation of assumptions about DHCP, DNS, routing, IPv6, captive portals, VPN failure, and other network behavior.

Security should be demonstrated through evidence, not inferred solely from configuration review.

## 15. Minimize exposed attack surface

SecureTravelFirewall should expose as few services as practical, particularly on the hostile uplink.

Management services must not be reachable from the hostile network by default.

Features that materially increase attack surface should require a clear benefit and deliberate design review.

## 16. Keep security identities distinct

In future enterprise use, the identity of the travel firewall, endpoint, and user must be treated as separate security assertions.

For example:

- Firewall X proves the identity of Firewall X.
- Laptop Y proves the identity of Laptop Y.
- User Z proves the identity of User Z.

An authenticated VPN tunnel must not by itself imply that every endpoint or user behind the tunnel is trusted.

## 17. VPN provides secure transport, not automatic trust

The long-term enterprise design should avoid treating VPN membership as equivalent to being inside a trusted corporate LAN.

The preferred direction is an authenticated transport to a controlled corporate access edge where endpoint identity, user identity, policy, and resource authorization can be evaluated independently.

## 18. Personal first, enterprise-aware

The first practical target is a personal SecureTravelFirewall that protects ordinary client devices on untrusted networks.

The design should nevertheless avoid decisions that unnecessarily prevent later evolution toward enterprise capabilities such as:

- strong device identity,
- firewall-to-endpoint pairing,
- central management,
- signed configuration and firmware,
- attestation,
- policy distribution,
- inventory,
- revocation, and
- controlled access to corporate resources.

The project should not implement enterprise complexity before it is needed, but it should preserve a credible migration path.

## 19. Prefer simple, inspectable mechanisms

When two approaches provide comparable security, prefer the one that is easier to inspect, reproduce, test, and reason about.

Complexity is itself a security cost.

## 20. Security-sensitive changes require regression testing

Changes to firewall rules, routing, DNS, VPN handling, network-interface mapping, captive-portal behavior, package versions, or the OpenWrt baseline should trigger the relevant regression tests before being considered verified.
