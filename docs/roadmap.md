# Roadmap

This roadmap is intentionally high-level. It describes development stages rather than fixed release commitments.

## Phase 1 — Reference prototype

- Establish a defined OpenWrt baseline.
- Use Wi-Fi station mode as the hostile uplink.
- Use wired LAN as the protected client network.
- Disable unnecessary services and interfaces.
- Establish explicit firewall zones and default-deny behavior.
- Capture all accepted configuration in version-controlled automation.

## Phase 2 — VPN-first transport

- Establish authenticated VPN transport.
- Enforce fail-closed client routing when the VPN is unavailable.
- Control DNS and test for leakage.
- Verify IPv4 and IPv6 behavior.

## Phase 3 — Hostile-network verification

- Build a repeatable hostile-network lab.
- Automate isolation and leak tests.
- Test malicious or malformed DHCP, DNS, routing, and IPv6 behavior.
- Test VPN failure and recovery.
- Record verification evidence for the reference device.

## Phase 4 — Captive portal and emergency web access

- Add Wi-Fi scanning and uplink selection through the protected management interface.
- Detect captive portals.
- Provide a constrained onboarding state for voucher, terms, payment, or other web-based authentication.
- Add an explicit emergency web-access mechanism without creating general WAN fallback.

## Phase 5 — Protected Wi-Fi

- Add an optional protected wireless client network.
- Prefer separation between hostile uplink radio and protected AP where hardware permits.
- Support differentiated policy for wired and wireless protected clients.

## Phase 6 — Hardware abstraction and community verification

- Formalize logical interface roles and device profiles.
- Publish compatibility test procedures.
- Accept community-contributed device profiles and verification evidence.
- Classify hardware as Reference, Verified, Experimental, or Unknown.

## Phase 7 — Enterprise evolution

Potential future capabilities include:

- strong firewall device identity,
- endpoint identity and firewall-to-endpoint pairing,
- central management and inventory,
- policy distribution,
- certificate and key lifecycle management,
- signed updates and release channels,
- revocation,
- secure boot / hardware-backed key storage where available,
- attestation,
- controlled corporate access edge integration, and
- endpoint/user-aware authorization independent of VPN membership.
