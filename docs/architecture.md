# Architecture

## Current State (Phase 1–5)

Two isolated segments, no WAN yet:

- **Management segment** — 192.168.1.0/24
  - FortiGate `mgmt`: 192.168.1.99/24 (HTTPS, SSH, PING)
  - Admin PC NIC 1: 192.168.1.110/24, gateway 192.168.1.99
  - Purpose: out-of-band administration, never carries lab user traffic.

- **Lab LAN segment** — 192.168.10.0/24
  - FortiGate `port1`: 192.168.10.1/24, DHCP server enabled
  - Cisco C9200: Gi1/0/2 uplink to FortiGate, Gi1/0/1 to endpoint (access VLAN 1)
  - Endpoint NIC 2: DHCP client, receives 192.168.10.100

## Design Decisions

1. **Dedicated mgmt interface** — keeps admin traffic off the lab LAN. If a firewall policy or NAT change breaks the LAN, management access survives.
2. **FortiGate as DHCP server** — demonstrates the firewall as a services device, not just a filter. Pool starts at .100 to leave room for static infrastructure (.1–.99).
3. **Single VLAN to start** — VLAN 1 only, so Layer 2 is proven before adding 802.1Q complexity.
4. **No default route yet** — intentional. Validates that directly-connected routing works and makes the need for a default route + policy obvious in the next phase.

## Future Evolution

- Add `port2` as WAN (DHCP client), default route via WAN
- LAN→WAN policy with NAT, DNS
- VLANs: users / servers / guest with inter-VLAN policies
- Security profiles: IPS, AV, Web Filtering, DNS filtering
- Logging to FortiAnalyzer / syslog, traffic analysis
