# Lab Log

## 2026-09-28 — Phases 1–5

- Serial console via SecureCRT, admin login, forced password change.
- Hostname set to FG-500E-LAB. Dashboard set to Comprehensive.
- mgmt verified: 192.168.1.99/24, allowaccess ping/https/ssh.
- Management PC 192.168.1.110/24, ping to 192.168.1.99 OK.
- port1 set to 192.168.10.1/24, DHCP enabled.
- Found DHCP pool mismatch between GUI assumption and CLI (`show system dhcp server`). Corrected to 192.168.10.100–200, gateway 192.168.10.1, lease 604800.
- C9200: Gi1/0/2 to FortiGate port1, Gi1/0/1 to PC NIC2. Verified connected / 1G / full / access VLAN 1.
- Endpoint NIC2 got 192.168.10.100 via DHCP.
- Ping PC→FortiGate (192.168.10.1) initially failed. `show system interface port1` showed no ping in allowaccess. Added it, retested OK both directions.
- HTTPS GUI via https://192.168.10.1 OK.
- Routing table: only 192.168.1.0/24 and 192.168.10.0/24 directly connected. No default route.
- Firewall Policy list empty — implicit deny only.

## Next

- Configure port2 as WAN, default route, LAN→WAN policy with NAT, DNS, outbound test.
