# Configuration

## FortiGate 500E (FortiOS 7.0.17)

### System
- Hostname: `FG-500E-LAB`
- Mode: NAT, standalone, VDOM `root`

### mgmt
- IP: 192.168.1.99/24
- allowaccess: ping, https, ssh

### port1 (LAN)
- IP: 192.168.10.1/24
- allowaccess: ping, https, ssh (ping added during troubleshooting)
- DHCP server: enabled
  - Range: 192.168.10.100 – 192.168.10.200
  - Netmask: 255.255.255.0
  - Gateway: 192.168.10.1 (same as interface IP)
  - DNS: same as system DNS
  - Lease: 604800 seconds (7 days)

### Verification commands used
- `show system interface`
- `show system dhcp server`
- `get router info routing-table all`
- `execute ping 192.168.10.100`

## Cisco Catalyst C9200

- Gi1/0/2 → FortiGate port1: connected, 1 Gbps, full duplex, access VLAN 1
- Gi1/0/1 → endpoint NIC 2: access VLAN 1

## Endpoint (Windows)

- NIC 1 (mgmt): 192.168.1.110/24, gateway 192.168.1.99
- NIC 2 (lab): DHCP, received 192.168.10.100/24, gateway 192.168.10.1
