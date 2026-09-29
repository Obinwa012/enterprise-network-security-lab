# Lessons Learned

1. **CLI is the source of truth.** The GUI is convenient, but `show system interface` and `show system dhcp server` settle arguments.
2. **Validate from the endpoint, not just the firewall.** A DHCP scope isn't "working" until a real client gets a lease. A ping isn't "working" until both directions are tested.
3. **FortiGate has two different "allow" concepts.** `allowaccess` controls traffic *to* the FortiGate. Firewall policies control traffic *through* it. Confusing the two wastes time.
4. **Start simple, then segment.** One VLAN and one LAN proved L2/L3 before adding 802.1Q, policies, and NAT complexity.
5. **Document the failures.** The DHCP mismatch and the missing `allowaccess ping` are more valuable in a portfolio than a screenshot of a green checkmark — they show the troubleshooting loop.
6. **Sanitize before publishing.** Serial numbers, passwords, and public IPs don't belong in a public repo. Screenshot first, redact second, push third.
