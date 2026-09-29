# Troubleshooting

## Issue 1 — DHCP pool mismatch

**Symptom:** Expected pool 192.168.10.100–200 based on GUI entry, but endpoint behavior needed confirmation.

**Investigation:** Ran `show system dhcp server` in the CLI. The CLI showed a different range than what was assumed from the GUI (earlier state showed 192.168.10.2–254).

**Root cause:** GUI input and actual running config diverged — the CLI is the source of truth on FortiOS.

**Fix:** Corrected the DHCP scope in the CLI/GUI to 192.168.10.100–200, verified again with `show system dhcp server`.

**Retest:** Endpoint renewed and received 192.168.10.100. Pass.

**Lesson:** Always verify network services from the CLI, not just the GUI. `show` before `set`.

## Issue 2 — ICMP to port1 failed after DHCP success

**Symptom:** Endpoint had a valid DHCP lease (192.168.10.100) but `ping 192.168.10.1` failed.

**Investigation:** Ran `show system interface port1`. The `allowaccess` line did not include `ping`.

**Root cause:** FortiGate administrative access controls ICMP to its own interfaces separately from forwarded traffic. DHCP working proved L2/L3 to the FortiGate's DHCP daemon, but ICMP to the interface IP requires explicit `allowaccess ping`.

**Fix:** Added ping to allowaccess on port1:
`config system interface` → `edit port1` → `set allowaccess ping https ssh` → `end`

**Retest:** `ping 192.168.10.1` from PC: 4/4 replies. `execute ping 192.168.10.100` from FortiGate: 5/5 replies. Pass.

**Lesson:** On FortiGate, "can I reach the firewall itself" (admin access) and "can traffic pass through the firewall" (policies) are two different questions. This issue proved the first half.

## Issue 3 — No internet / no default route (expected)

**Symptom:** No WAN connectivity.

**Investigation:** `get router info routing-table all` showed only directly connected routes: 192.168.1.0/24 via mgmt, 192.168.10.0/24 via port1.

**Root cause:** No WAN interface configured, no default route, no LAN→WAN policy, no NAT. By design at this phase.

**Next step:** Configure port2 as WAN, add default route, create firewall policy with NAT.
