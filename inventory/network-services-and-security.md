# Active Network, Services, and Security Summary

**Observed:** 2026-11-18  
**Platform:** OpenWrt x86_64  
**Purpose:** Sanitized operational summary based on live command output. This is documentation, not a restorable backup.

## Network and firewall

- Firewall backend: nftables, with the `inet fw4` table present.
- The firewall service command reported `active with no instances`, but the live nftables ruleset was present. The ruleset is the stronger evidence that firewall rules were loaded.
- Default firewall policies:
  - Input: `REJECT`
  - Output: `ACCEPT`
  - Forward: `REJECT`
- LAN zone: input, output, and forwarding are accepted.
- WAN zone: input is rejected, output is accepted, forwarding is dropped.
- LAN-to-WAN forwarding is configured.
- WAN masquerading and MTU fixing are enabled.
- The live ruleset showed input and forward base-chain policies of `drop`, LAN traffic routed to the LAN accept chain, and WAN traffic routed to the WAN reject chain.
- Custom TTL rule chains set IPv4 TTL to 65 on selected uplink interfaces.

## RPCD and LuCI access

- RPCD login entry uses the `root` account.
- The configured authenticated root ACL has `read='*'` and `write='*'`. This is broad administrative access and should remain restricted to trusted administrators.
- The unauthenticated ACL inspected grants only `session.access` and `session.login` under ubus.
- The unauthenticated section in `luci-base.json` permits LuCI feature probing through `luci.getFeatures`.
- LuCI base ACL also contains authenticated permissions for selected file, UCI, and ubus operations.
- No password, password hash, token, private key, or credential value is recorded here.

## Services observed listening

The following listeners were observed in a live `netstat -lntup` snapshot. IP addresses and device identifiers are intentionally omitted.

| Service | Observed ports | Notes |
|---|---|---|
| uhttpd / LuCI | TCP 80 | Bound to wildcard IPv4 and IPv6 addresses in this snapshot |
| Dropbear SSH | TCP 22 | Bound to wildcard IPv4 and IPv6 addresses in this snapshot |
| Samba | TCP 445, 139 | Bound to LAN addresses; Samba configuration binds to `br-lan` |
| dnsmasq | TCP/UDP 53 | DNS listeners on local/network addresses |
| dnsmasq DHCP | UDP 67 | DHCP server |
| NetBIOS name/datagram service | UDP 137, 138 | Samba/NetBIOS discovery |
| Avahi | UDP 5353 | Local service discovery |
| NTP | UDP 123 | Time service |
| odhcpd | UDP 547 | DHCPv6 service |

Listener output is a point-in-time observation. It does not by itself establish internet reachability; firewall policy and interface binding also matter.

## Samba shared storage

The Samba configuration is intentionally designed for convenient trusted-LAN sharing:

- Share name: `Storage`
- Share path: `/mnt/storage`
- Interface binding: `br-lan`
- `guest ok = yes`
- `read only = no`
- `force_root = 1`
- New file create mask: `0666`
- New directory mask: `0777`
- Samba security mode: `USER`
- Guest mapping: `Bad User`
- `bind interfaces only = Yes`

**Owner-confirmed design decision:** all devices connected to the trusted Wi-Fi/LAN are intentionally allowed to open and write to the Storage share without entering a username or password. This guest-write behavior is intentional and is not classified as an accidental misconfiguration.

**Security implication:** `force_root = 1` means file operations through this share can be performed with root privileges. Any untrusted or compromised device admitted to the LAN may therefore alter or create files with broad privileges. Keep Samba restricted to the trusted LAN and do not expose its ports through WAN port forwarding. Consider a separate guest/IoT network if untrusted devices need internet access but should not have storage access.

## SSH and administrative exposure

- Dropbear was running.
- TCP port 22 was listening on wildcard IPv4 and IPv6 addresses in the observed snapshot.
- Password authentication was previously observed enabled in the Dropbear configuration.
- Restrict administrative access to trusted LAN devices. If changing to SSH keys or disabling password authentication, verify key-based access in a second session before closing the existing SSH session to avoid lockout.

## Scope and limitations

- This file records only settings and service behavior actually observed in the command output.
- IP addresses, MAC addresses, SSIDs, UUIDs, DUIDs, credentials, and other infrastructure identifiers are intentionally excluded.
- This is not a full configuration export, firmware build recipe, or disk image.
- Recheck listener and firewall state after any network, firewall, Samba, or package changes.
