# Testing Strategy

Security claims in SecureTravelFirewall should be supported by repeatable verification wherever practical.

## Goals

Testing should demonstrate that the implementation behaves correctly when the uplink network is unreliable, malformed, or actively hostile.

The project should test both intended functionality and the absence of unintended communication paths.

## Test model

Each important security property should eventually be expressed in a form similar to:

```text
Requirement
    -> threat or failure mode
    -> test procedure
    -> expected result
    -> observed result
    -> PASS / FAIL
```

## Initial test areas

### WAN-to-LAN isolation

Verify that hosts on the hostile network cannot discover, initiate communication with, or otherwise reach protected clients.

### VPN kill switch

Verify that loss of the VPN does not cause protected-client traffic to fall back to the hostile uplink.

### DNS leakage

Verify that protected-client DNS traffic uses only the intended resolver path and does not escape directly to the hostile network.

### IPv6 bypass

Verify that IPv6 cannot create a communication path that bypasses IPv4 firewall or VPN policy.

### Management-plane exposure

Verify that administrative services are not reachable from the hostile uplink.

### Broadcast and multicast leakage

Inspect ARP, mDNS, LLMNR, NetBIOS, IPv6 neighbour discovery, and other local-discovery traffic for unintended exposure.

### Hostile network infrastructure

Test behavior when the uplink provides malicious or malformed:

- DHCP,
- DNS,
- default gateway information,
- IPv6 router advertisements,
- routing information,
- MTU or connectivity behavior, and
- captive-portal redirects.

### Captive-portal isolation

When onboarding mode is implemented, verify that temporary portal access does not become unrestricted direct Internet access.

## Hostile-network lab

A dedicated lab environment should be maintained alongside the firewall implementation.

The lab should allow packet capture on the hostile side and controlled manipulation of network services so that leakage and unexpected behavior can be observed directly.

Testing should be reproducible enough that a contributor can run the same relevant tests against a different supported OpenWrt device and submit the results as verification evidence.

## Regression testing

Changes affecting any of the following should trigger relevant regression tests:

- OpenWrt baseline,
- installed packages,
- firewall rules,
- network interfaces,
- routing,
- VPN configuration,
- DNS handling,
- IPv6 handling,
- device profiles,
- captive-portal handling, and
- management services.

The long-term goal is to automate as much of this suite as practical and use it as release evidence rather than relying on configuration review alone.
