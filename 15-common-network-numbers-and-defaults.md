# 15. Common Network Numbers and Defaults

Networking has many numbers that appear again and again: default ports, MTU values, TTL values, protocol numbers, private IP ranges, Wi-Fi channels, Ethernet speeds, and more.

You do not need to memorize every number in networking, but some values are common enough that they are worth knowing well. These numbers help with troubleshooting, design, firewall rules, interviews, and day-to-day operations.

This guide explains the most useful network numbers and defaults in a practical way.

---

## Table of Contents

- [1. Why Network Numbers Matter](#1-why-network-numbers-matter)
- [2. Quick Reference Table](#2-quick-reference-table)
- [3. Common Port Numbers](#3-common-port-numbers)
- [4. IP Address Sizes](#4-ip-address-sizes)
- [5. IPv4 Private Ranges](#5-ipv4-private-ranges)
- [6. Special IPv4 Ranges](#6-special-ipv4-ranges)
- [7. Important IPv6 Prefixes](#7-important-ipv6-prefixes)
- [8. Common CIDR Prefixes](#8-common-cidr-prefixes)
- [9. Ethernet MTU](#9-ethernet-mtu)
- [10. Jumbo Frames](#10-jumbo-frames)
- [11. TCP MSS](#11-tcp-mss)
- [12. MTU vs MSS](#12-mtu-vs-mss)
- [13. TTL and Hop Limit](#13-ttl-and-hop-limit)
- [14. Common TTL Starting Values](#14-common-ttl-starting-values)
- [15. DNS TTL Values](#15-dns-ttl-values)
- [16. Ethernet Speeds](#16-ethernet-speeds)
- [17. Common Cable Distance Limits](#17-common-cable-distance-limits)
- [18. Wi-Fi Bands and Channels](#18-wi-fi-bands-and-channels)
- [19. VLAN Numbers](#19-vlan-numbers)
- [20. Common Administrative Distances](#20-common-administrative-distances)
- [21. Common Routing Protocol Numbers and Ports](#21-common-routing-protocol-numbers-and-ports)
- [22. TCP and UDP Basics](#22-tcp-and-udp-basics)
- [23. ICMP Type Examples](#23-icmp-type-examples)
- [24. QoS and DSCP Values](#24-qos-and-dscp-values)
- [25. Voice and Real-Time Traffic Numbers](#25-voice-and-real-time-traffic-numbers)
- [26. Common VPN Numbers](#26-common-vpn-numbers)
- [27. Common Security and Authentication Numbers](#27-common-security-and-authentication-numbers)
- [28. Common Time and Lease Values](#28-common-time-and-lease-values)
- [29. Common Wireless Signal Values](#29-common-wireless-signal-values)
- [30. Common Troubleshooting Clues](#30-common-troubleshooting-clues)
- [31. Best Practices](#31-best-practices)
- [32. Quick Summary](#32-quick-summary)

---

## 1. Why Network Numbers Matter

Many network problems are easier to understand when you know the common default values.

For example:

- If you see port `443`, you probably think of HTTPS.
- If you see an IP address starting with `169.254`, you probably think of DHCP failure.
- If you see MTU `1500`, you know it is the normal Ethernet MTU.
- If you see TCP MSS `1460`, you know it matches Ethernet MTU `1500` with normal IPv4 and TCP headers.
- If you see VLAN `1`, you know it is often the default VLAN.
- If you see TTL values around `64`, `128`, or `255`, you may guess the original starting TTL used by the sender.

Knowing these values does not replace real troubleshooting, but it helps you recognize patterns much faster.

---

## 2. Quick Reference Table

Here are some common values you should recognize quickly.

| Item | Common Value |
|---|---:|
| Ethernet MTU | `1500` bytes |
| Jumbo frame MTU | commonly `9000` bytes |
| TCP MSS over 1500 MTU, IPv4 | `1460` bytes |
| IPv4 address size | `32` bits |
| IPv6 address size | `128` bits |
| MAC address size | `48` bits |
| VLAN ID range | `1 - 4094` usable |
| Default VLAN | usually `1` |
| HTTPS | TCP `443` |
| HTTP | TCP `80` |
| SSH | TCP `22` |
| DNS | UDP/TCP `53` |
| DHCP IPv4 server/client | UDP `67/68` |
| DHCPv6 client/server | UDP `546/547` |
| NTP | UDP `123` |
| SNMP | UDP `161/162` |
| BGP | TCP `179` |
| RDP | TCP `3389` |
| SMB | TCP `445` |
| MySQL | TCP `3306` |
| PostgreSQL | TCP `5432` |
| Microsoft SQL Server | TCP `1433` |
| Common TTL starts | `64`, `128`, `255` |
| IPv4 default route | `0.0.0.0/0` |
| IPv6 default route | `::/0` |
| IPv4 loopback | `127.0.0.0/8` |
| IPv6 loopback | `::1/128` |
| APIPA / IPv4 link-local | `169.254.0.0/16` |

---

## 3. Common Port Numbers

Ports identify services running on a device.

The most common ports are worth memorizing because they appear in firewall rules, logs, packet captures, interviews, and troubleshooting.

### Very common ports

| Port | Protocol | Service |
|---:|---|---|
| `20/21` | TCP | FTP |
| `22` | TCP | SSH |
| `23` | TCP | Telnet |
| `25` | TCP | SMTP |
| `53` | UDP/TCP | DNS |
| `67/68` | UDP | DHCP IPv4 |
| `69` | UDP | TFTP |
| `80` | TCP | HTTP |
| `88` | TCP/UDP | Kerberos |
| `110` | TCP | POP3 |
| `123` | UDP | NTP |
| `135` | TCP | Microsoft RPC Endpoint Mapper |
| `137-139` | TCP/UDP | NetBIOS |
| `143` | TCP | IMAP |
| `161/162` | UDP | SNMP / SNMP traps |
| `179` | TCP | BGP |
| `389` | TCP/UDP | LDAP |
| `443` | TCP | HTTPS |
| `445` | TCP | SMB/CIFS |
| `500` | UDP | IKE / IPsec |
| `514` | UDP | Syslog |
| `520` | UDP | RIP |
| `546/547` | UDP | DHCPv6 |
| `636` | TCP | LDAPS |
| `853` | TCP | DNS over TLS |
| `993` | TCP | IMAPS |
| `995` | TCP | POP3S |
| `1194` | UDP/TCP | OpenVPN |
| `1433` | TCP | Microsoft SQL Server |
| `1521` | TCP | Oracle Database |
| `1701` | UDP | L2TP |
| `1723` | TCP | PPTP |
| `1812/1813` | UDP | RADIUS authentication/accounting |
| `2049` | TCP/UDP | NFS |
| `3306` | TCP | MySQL / MariaDB |
| `3389` | TCP | RDP |
| `4500` | UDP | IPsec NAT-T |
| `5060` | TCP/UDP | SIP |
| `5061` | TCP | SIP over TLS |
| `5432` | TCP | PostgreSQL |
| `5900` | TCP | VNC |
| `8080` | TCP | HTTP alternate |
| `8443` | TCP | HTTPS alternate |

### Practical note

A port number suggests a service, but it does not prove the service.

For example, port `443` usually means HTTPS, but an administrator can run a different service on that port. Always verify when accuracy matters.

---

## 4. IP Address Sizes

IP addresses are used at Layer 3.

### IPv4

IPv4 addresses are `32` bits long.

Example:

```text
192.168.1.10
```

IPv4 is written in dotted decimal notation.

### IPv6

IPv6 addresses are `128` bits long.

Example:

```text
2001:db8::10
```

IPv6 is written in hexadecimal and separated by colons.

### Comparison

| Version | Size | Example |
|---|---:|---|
| IPv4 | `32` bits | `192.168.1.10` |
| IPv6 | `128` bits | `2001:db8::10` |

---

## 5. IPv4 Private Ranges

Private IPv4 addresses are used inside internal networks.

They are not normally routed on the public Internet.

| Range | CIDR | Common Use |
|---|---|---|
| `10.0.0.0 - 10.255.255.255` | `10.0.0.0/8` | Large private networks |
| `172.16.0.0 - 172.31.255.255` | `172.16.0.0/12` | Medium/large private networks |
| `192.168.0.0 - 192.168.255.255` | `192.168.0.0/16` | Home/small office networks |

### Common examples

Home routers often use:

```text
192.168.0.0/24
192.168.1.0/24
```

Enterprise networks often use:

```text
10.0.0.0/8
```

### Important point

Private IP addresses usually need NAT to reach the public Internet.

---

## 6. Special IPv4 Ranges

Some IPv4 ranges have special meanings.

| Range / Address | Purpose |
|---|---|
| `0.0.0.0` | Unspecified address / default route context |
| `0.0.0.0/0` | Default route |
| `127.0.0.0/8` | Loopback |
| `127.0.0.1` | Localhost |
| `169.254.0.0/16` | APIPA / link-local IPv4 |
| `224.0.0.0/4` | Multicast |
| `240.0.0.0/4` | Reserved |
| `255.255.255.255` | Limited broadcast |
| `100.64.0.0/10` | Carrier-Grade NAT |
| `192.0.2.0/24` | Documentation example range |
| `198.51.100.0/24` | Documentation example range |
| `203.0.113.0/24` | Documentation example range |

### APIPA clue

If a Windows client has an address like:

```text
169.254.20.15
```

that usually means the client failed to get an address from DHCP.

### Loopback clue

If you ping:

```text
127.0.0.1
```

or:

```text
localhost
```

you are testing the local TCP/IP stack, not the physical network.

---

## 7. Important IPv6 Prefixes

IPv6 also has important prefixes to recognize.

| Prefix | Purpose |
|---|---|
| `::1/128` | Loopback |
| `::/0` | Default route |
| `fe80::/10` | Link-local |
| `fc00::/7` | Unique Local Address |
| `2000::/3` | Global unicast |
| `ff00::/8` | Multicast |
| `2001:db8::/32` | Documentation examples |

### Link-local

IPv6 link-local addresses start with:

```text
fe80::/10
```

They are used on the local link and are very important for IPv6 neighbor discovery and routing behavior.

### ULA

Unique Local Addresses come from:

```text
fc00::/7
```

In practice, many ULA networks start with `fd`.

### Global unicast

Global unicast IPv6 addresses commonly come from:

```text
2000::/3
```

These are public Internet-routable IPv6 addresses when routing and policy allow it.

---

## 8. Common CIDR Prefixes

CIDR prefix length tells you how many bits are used for the network part of an address.

### IPv4 common prefixes

| Prefix | Netmask | Total Addresses | Usable Hosts |
|---|---|---:|---:|
| `/8` | `255.0.0.0` | `16,777,216` | `16,777,214` |
| `/16` | `255.255.0.0` | `65,536` | `65,534` |
| `/24` | `255.255.255.0` | `256` | `254` |
| `/25` | `255.255.255.128` | `128` | `126` |
| `/26` | `255.255.255.192` | `64` | `62` |
| `/27` | `255.255.255.224` | `32` | `30` |
| `/28` | `255.255.255.240` | `16` | `14` |
| `/29` | `255.255.255.248` | `8` | `6` |
| `/30` | `255.255.255.252` | `4` | `2` |
| `/31` | `255.255.255.254` | `2` | Point-to-point use |
| `/32` | `255.255.255.255` | `1` | Single host |

### IPv6 common prefixes

| Prefix | Common Meaning |
|---|---|
| `/128` | One IPv6 address |
| `/64` | Standard LAN subnet |
| `/56` | Common small site/customer delegation |
| `/48` | Common site/organization allocation |
| `/32` | Large provider allocation example |
| `/0` | Default route |

### Important IPv6 note

In IPv6, `/64` is the standard subnet size for normal LANs. You usually do not subnet IPv6 the same way you conserve IPv4 host addresses.

---

## 9. Ethernet MTU

MTU stands for **Maximum Transmission Unit**.

It is the largest Layer 3 packet size that can be carried without fragmentation on a link.

The normal Ethernet MTU is:

```text
1500 bytes
```

### Why MTU matters

MTU problems can cause strange issues, such as:

- Websites partially loading
- VPN traffic failing
- Large downloads failing
- Some applications working while others fail
- ICMP working but TCP sessions failing

### Ethernet frame vs MTU

The MTU usually refers to the IP packet payload inside the Ethernet frame, not the full Ethernet frame size.

A standard Ethernet frame includes extra fields such as:

- Destination MAC
- Source MAC
- EtherType
- Frame check sequence

So the full frame on the wire is larger than 1500 bytes.

---

## 10. Jumbo Frames

Jumbo frames are Ethernet frames with an MTU larger than the standard `1500` bytes.

A common jumbo MTU value is:

```text
9000 bytes
```

### Why jumbo frames are used

Jumbo frames can reduce overhead in some high-throughput environments.

They are often used in:

- Data centers
- Storage networks
- iSCSI
- Backup networks
- High-performance computing
- Some virtualization environments

### Important rule

Jumbo frames must be supported end-to-end.

That means the following devices must agree:

- Server NIC
- Switch ports
- Routers or Layer 3 interfaces
- Storage system
- Virtual switches
- Firewalls in the path

### Common mistake

Enabling jumbo frames on only one device can create hard-to-diagnose connectivity problems.

Use jumbo frames only when you have a reason and can control the full path.

---

## 11. TCP MSS

MSS stands for **Maximum Segment Size**.

It is the largest amount of TCP payload data that can fit in one TCP segment.

With standard Ethernet MTU `1500`, normal IPv4, and normal TCP headers:

```text
TCP MSS = 1460 bytes
```

Why?

```text
1500 byte MTU
- 20 byte IPv4 header
- 20 byte TCP header
= 1460 byte TCP MSS
```

### IPv6 MSS

With IPv6, the base IPv6 header is larger.

Typical calculation:

```text
1500 byte MTU
- 40 byte IPv6 header
- 20 byte TCP header
= 1440 byte TCP MSS
```

### Why MSS matters

MSS is important for:

- TCP performance
- VPNs
- tunnels
- firewalls
- path MTU problems
- MSS clamping

---

## 12. MTU vs MSS

MTU and MSS are related, but they are not the same thing.

| Term | Meaning |
|---|---|
| MTU | Maximum IP packet size on a link |
| MSS | Maximum TCP payload size inside a TCP segment |

### Simple example

If MTU is `1500`, TCP MSS is usually `1460` for IPv4.

The missing `40` bytes are used by:

```text
20 bytes IPv4 header
20 bytes TCP header
```

### VPN and tunnel note

Tunnels add extra headers.

Examples:

- IPsec
- GRE
- VXLAN
- WireGuard
- OpenVPN

Because these headers take extra space, the effective MTU may be lower.

That is why VPN environments often need careful MTU or MSS tuning.

---

## 13. TTL and Hop Limit

TTL stands for **Time To Live**.

In IPv4, TTL limits how many router hops a packet can pass through.

In IPv6, the similar field is called **Hop Limit**.

### Why TTL exists

TTL prevents packets from looping forever.

Every router that forwards the packet reduces the TTL by 1.

If TTL reaches 0, the packet is dropped.

### Example

A packet starts with TTL `64`.

After passing through 5 routers, the TTL becomes:

```text
59
```

### Traceroute

Tools like `traceroute` and `tracert` use TTL behavior to discover the path between two hosts.

---

## 14. Common TTL Starting Values

Different operating systems often use different starting TTL values.

Common starting values include:

```text
64
128
255
```

### Typical meaning

| Starting TTL | Often Seen From |
|---:|---|
| `64` | Linux, Unix-like systems, many network devices |
| `128` | Windows systems |
| `255` | Some network devices and routing protocols |

### Important warning

You cannot identify an operating system perfectly from TTL alone.

But TTL can give a clue.

### Example

If you receive a packet with TTL `117`, it may have started at `128` and crossed 11 hops.

```text
128 - 11 = 117
```

This is only an estimate, not proof.

---

## 15. DNS TTL Values

DNS TTL is different from IP TTL.

DNS TTL controls how long a DNS answer can be cached.

### Common DNS TTL values

| TTL | Time |
|---:|---|
| `60` | 1 minute |
| `300` | 5 minutes |
| `600` | 10 minutes |
| `1800` | 30 minutes |
| `3600` | 1 hour |
| `86400` | 24 hours |

### When low DNS TTL is useful

Low TTL is useful before:

- Website migration
- Mail migration
- DNS provider change
- Load balancer cutover
- Disaster recovery testing

### When high DNS TTL is useful

Higher TTL is useful for stable records that rarely change.

It improves caching and reduces DNS query load.

---

## 16. Ethernet Speeds

Ethernet speeds have increased over time.

### Common speeds

| Name | Speed | Common Use |
|---|---:|---|
| `10BASE-T` | 10 Mbps | Legacy Ethernet |
| `100BASE-TX` | 100 Mbps | Older access networks |
| `1000BASE-T` | 1 Gbps | Common wired LAN |
| `2.5GBASE-T` | 2.5 Gbps | Newer access/Wi-Fi uplinks |
| `5GBASE-T` | 5 Gbps | Newer access/Wi-Fi uplinks |
| `10GBASE-T` | 10 Gbps | Servers, uplinks, high-end desktops |
| `25G` | 25 Gbps | Data center servers |
| `40G` | 40 Gbps | Data center uplinks |
| `100G` | 100 Gbps | Core/data center/provider networks |
| `400G` | 400 Gbps | High-end provider/data center networks |

### Practical note

If two devices negotiate the wrong speed or duplex, performance can become very poor.

Always check interface speed and duplex during troubleshooting.

---

## 17. Common Cable Distance Limits

### Twisted pair copper Ethernet

A very common copper Ethernet distance limit is:

```text
100 meters
```

This applies to many standard twisted-pair Ethernet deployments.

### Common copper categories

| Cable | Common Use |
|---|---|
| Cat5e | 1 Gbps commonly |
| Cat6 | 1 Gbps and some 10 Gbps over shorter distances |
| Cat6a | Better for 10 Gbps up to 100 meters |

### Fiber distances

Fiber distance depends on:

- Fiber type
- Transceiver type
- Speed
- Wavelength
- Optics budget
- Connector quality

### Multimode vs single-mode

| Fiber Type | Common Use |
|---|---|
| Multimode fiber | Shorter data center and campus links |
| Single-mode fiber | Longer-distance links |

### Practical note

Do not assume all fiber links support the same distance. Always check the optics and fiber type.

---

## 18. Wi-Fi Bands and Channels

Wi-Fi commonly uses these frequency bands:

| Band | Common Notes |
|---|---|
| 2.4 GHz | Longer range, more interference |
| 5 GHz | Higher speed, more channels, shorter range |
| 6 GHz | Cleaner spectrum, newer devices |

### 2.4 GHz channels

The most common non-overlapping 2.4 GHz channels are:

```text
1, 6, 11
```

### Channel width examples

Common Wi-Fi channel widths include:

```text
20 MHz
40 MHz
80 MHz
160 MHz
```

### Practical rule

In crowded environments, wider channels are not always better.

A 20 MHz channel can be more stable than a wide channel in a noisy environment.

---

## 19. VLAN Numbers

VLAN IDs are 12 bits in the 802.1Q tag.

The normal usable VLAN range is:

```text
1 - 4094
```

### Reserved values

| VLAN ID | Meaning |
|---:|---|
| `0` | Used for priority tagging, not a normal VLAN |
| `1` | Default VLAN on many switches |
| `4095` | Reserved |

### Default VLAN

Many switches use VLAN `1` as the default VLAN.

Security best practice often recommends not using VLAN 1 for normal user traffic or management traffic.

### Practical example

A simple network may use:

| VLAN | Purpose |
|---:|---|
| `10` | Management |
| `20` | Users |
| `30` | Voice |
| `40` | Servers |
| `50` | Guests |

---

## 20. Common Administrative Distances

Administrative distance is used by routers to choose between routes learned from different sources.

Lower administrative distance is preferred.

Cisco-style common values:

| Route Source | Administrative Distance |
|---|---:|
| Connected | `0` |
| Static | `1` |
| External BGP | `20` |
| EIGRP internal | `90` |
| OSPF | `110` |
| RIP | `120` |
| EIGRP external | `170` |
| Internal BGP | `200` |

### Example

If a router learns the same prefix from OSPF and RIP, it usually prefers OSPF because:

```text
OSPF AD 110 < RIP AD 120
```

### Important point

Administrative distance chooses between routing sources. Metric chooses between routes from the same routing protocol.

---

## 21. Common Routing Protocol Numbers and Ports

Not every routing protocol uses TCP or UDP ports.

Some use IP protocol numbers directly.

| Protocol | Number / Port | Notes |
|---|---|---|
| ICMP | IP protocol `1` | IPv4 control messages |
| TCP | IP protocol `6` | Transport protocol |
| UDP | IP protocol `17` | Transport protocol |
| GRE | IP protocol `47` | Tunnel protocol |
| ESP | IP protocol `50` | IPsec encryption payload |
| AH | IP protocol `51` | IPsec authentication header |
| OSPF | IP protocol `89` | Link-state routing |
| VRRP | IP protocol `112` | First-hop redundancy |
| EIGRP | IP protocol `88` | Cisco routing protocol |
| BGP | TCP `179` | Path vector routing |
| RIP | UDP `520` | Distance vector routing |

### Practical firewall note

GRE, ESP, AH, OSPF, and VRRP are not TCP or UDP ports.

If a firewall rule only allows TCP or UDP ports, it may not allow these protocols unless explicitly configured.

---

## 22. TCP and UDP Basics

TCP and UDP both use ports, but they work differently.

### TCP

TCP protocol number:

```text
6
```

TCP is connection-oriented and reliable.

Common TCP services:

- HTTP
- HTTPS
- SSH
- SMTP
- IMAP
- RDP
- SMB
- Databases

### UDP

UDP protocol number:

```text
17
```

UDP is connectionless and lightweight.

Common UDP services:

- DNS
- DHCP
- NTP
- SNMP
- VoIP media
- VPN traffic

### TCP handshake

TCP commonly starts with a 3-way handshake:

```text
SYN
SYN-ACK
ACK
```

### Common TCP flags

| Flag | Meaning |
|---|---|
| SYN | Start connection |
| ACK | Acknowledge data |
| FIN | Gracefully close connection |
| RST | Reset connection |
| PSH | Push data to application |
| URG | Urgent data |

---

## 23. ICMP Type Examples

ICMP is used for control and error messages.

### Common IPv4 ICMP examples

| Type | Meaning |
|---:|---|
| `0` | Echo Reply |
| `3` | Destination Unreachable |
| `5` | Redirect |
| `8` | Echo Request |
| `11` | Time Exceeded |

### Ping

Ping uses ICMP Echo Request and Echo Reply.

```text
Echo Request -> Echo Reply
```

### Traceroute

Traceroute relies heavily on TTL / Hop Limit behavior and ICMP responses.

### IPv6 note

ICMPv6 is even more important than ICMP in IPv4. IPv6 uses ICMPv6 for neighbor discovery, router advertisements, path MTU discovery, and more.

Blocking ICMPv6 carelessly can break IPv6.

---

## 24. QoS and DSCP Values

QoS stands for **Quality of Service**.

It is used to classify, mark, prioritize, and manage network traffic.

DSCP stands for **Differentiated Services Code Point**.

DSCP markings are placed in the IP header to identify traffic treatment.

### Common QoS terms

| Term | Meaning |
|---|---|
| Best Effort | Normal traffic with no special priority |
| AF | Assured Forwarding classes |
| EF | Expedited Forwarding, commonly used for voice |
| CS | Class Selector values |

### Common DSCP values

| Name | Decimal | Common Use |
|---|---:|---|
| Best Effort | `0` | Normal traffic |
| CS1 | `8` | Often used for low-priority/scavenger traffic |
| AF11 | `10` | Assured Forwarding class 1 low drop preference |
| AF21 | `18` | Assured Forwarding class 2 low drop preference |
| AF31 | `26` | Assured Forwarding class 3 low drop preference |
| AF41 | `34` | Assured Forwarding class 4 low drop preference |
| EF | `46` | Voice RTP commonly |
| CS6 | `48` | Network control in some designs |
| CS7 | `56` | Network control in some designs |

### EF for voice

Voice RTP traffic is commonly marked as:

```text
DSCP EF 46
```

This helps voice packets receive low-latency treatment.

### Important note

QoS markings only help if the network devices trust and act on them.

Marking traffic as EF does not magically guarantee priority unless switches, routers, and WAN devices are configured correctly.

---

## 25. Voice and Real-Time Traffic Numbers

Voice and real-time media are sensitive to delay, jitter, and packet loss.

### Common targets

| Metric | Good Target |
|---|---:|
| One-way latency | Under `150 ms` is commonly preferred |
| Jitter | Under `30 ms` is commonly preferred |
| Packet loss | Under `1%` is commonly preferred |

These are general practical targets, not absolute rules for every system.

### Voice signaling

| Protocol | Port | Use |
|---|---:|---|
| SIP | `5060` TCP/UDP | Call signaling |
| SIP-TLS | `5061` TCP | Encrypted SIP signaling |

### Voice media

Voice media often uses RTP over UDP.

RTP port ranges depend on the vendor and configuration.

Example ranges may look like:

```text
10000-20000/UDP
16384-32767/UDP
```

### Important voice note

Opening SIP ports alone is often not enough. RTP media ports must also work.

A common symptom is:

```text
Call connects, but there is no audio.
```

This often points to RTP, NAT, firewall, or codec path issues.

---

## 26. Common VPN Numbers

VPNs often use specific ports and protocols.

| VPN / Protocol | Port or Protocol | Notes |
|---|---|---|
| IKE | UDP `500` | IPsec negotiation |
| IPsec NAT-T | UDP `4500` | IPsec through NAT |
| ESP | IP protocol `50` | IPsec encrypted payload |
| AH | IP protocol `51` | IPsec authentication header |
| L2TP | UDP `1701` | Often paired with IPsec |
| PPTP | TCP `1723` plus GRE | Legacy VPN, generally avoided |
| OpenVPN | UDP/TCP `1194` | Common default |
| WireGuard | UDP `51820` commonly | Common default, configurable |
| SSL VPN | TCP `443` commonly | Vendor-dependent |

### Practical firewall note

IPsec may need more than one thing allowed:

```text
UDP 500
UDP 4500
ESP protocol 50
```

Depending on NAT and configuration, the exact requirements can vary.

---

## 27. Common Security and Authentication Numbers

Authentication and directory services have important default ports.

| Service | Port | Use |
|---|---:|---|
| Kerberos | TCP/UDP `88` | Authentication |
| LDAP | TCP/UDP `389` | Directory access |
| LDAPS | TCP `636` | LDAP over TLS |
| RADIUS Auth | UDP `1812` | Network authentication |
| RADIUS Accounting | UDP `1813` | Accounting logs |
| TACACS+ | TCP `49` | Network device administration |

### Active Directory note

Microsoft Active Directory environments use many ports, including DNS, Kerberos, LDAP, SMB, RPC, and others.

If a domain-joined machine cannot authenticate, do not check only one port. AD depends on several services working together.

---

## 28. Common Time and Lease Values

Time-related values are important in DHCP, DNS, authentication, logs, and certificates.

### DHCP leases

Common DHCP lease times vary by network.

| Environment | Common Lease Idea |
|---|---|
| Guest Wi-Fi | Shorter leases |
| Office LAN | Medium or longer leases |
| Lab networks | Shorter leases |
| Stable server-like devices | Reservations or static IPs |

Example lease values:

```text
1 hour
4 hours
8 hours
24 hours
7 days
```

### NTP

NTP uses:

```text
UDP 123
```

Time sync is important for:

- Kerberos
- Certificates
- Logs
- Security investigations
- Distributed systems

### Kerberos time sensitivity

Kerberos is sensitive to clock differences. If client and server clocks are too far apart, authentication may fail.

A commonly seen tolerance is around 5 minutes in many environments, though exact policy can vary.

---

## 29. Common Wireless Signal Values

Wireless signal strength is often shown in dBm.

Values are negative numbers.

Closer to zero is stronger.

### RSSI guide

| RSSI | General Meaning |
|---:|---|
| `-30 dBm` | Excellent, very close |
| `-50 dBm` | Very good |
| `-60 dBm` | Good |
| `-67 dBm` | Often considered good for voice/video target designs |
| `-70 dBm` | Usable but weaker |
| `-80 dBm` | Poor |
| `-90 dBm` | Very poor |

### SNR

SNR stands for **Signal-to-Noise Ratio**.

Higher SNR is better.

General guide:

| SNR | Meaning |
|---:|---|
| `30 dB+` | Very good |
| `20 dB` | Good |
| `10 dB` | Weak |
| `< 10 dB` | Poor |

### Practical note

A strong signal does not always mean a good connection. Interference, channel utilization, roaming, client drivers, and AP capacity also matter.

---

## 30. Common Troubleshooting Clues

Common numbers can quickly point you toward the likely issue.

### `169.254.x.x`

Likely DHCP failure.

Check:

- DHCP server
- DHCP relay
- VLAN
- switch port
- wireless association
- scope exhaustion

### `127.0.0.1`

Localhost.

This tests the local machine, not the network path.

### `0.0.0.0/0`

Default route.

If a device has no default route, it may reach only local subnet destinations.

### `::/0`

IPv6 default route.

Same idea as IPv4 default route.

### MTU `1500`

Normal Ethernet MTU.

If tunnels are used, effective MTU may be smaller.

### MSS `1460`

Normal TCP MSS for IPv4 over Ethernet MTU `1500`.

### Port `53`

DNS.

If users can reach IPs but not names, check DNS.

### Port `67/68`

DHCP.

If clients cannot get addresses, check DHCP traffic.

### Port `443`

HTTPS.

Most modern web traffic uses this port.

### Port `3389`

RDP.

Be careful exposing this publicly.

### Port `445`

SMB.

Should not be exposed directly to the public Internet.

---

## 31. Best Practices

### Do not memorize blindly

Memorization helps, but understanding is more important.

Know what the number means and why it matters.

### Keep a reference nearby

Even experienced engineers use references.

A good engineer does not memorize everything. A good engineer knows what to check and how to verify it.

### Verify with tools

Use commands like:

```bash
ip addr
ip route
ss -tulpn
dig
ping
traceroute
curl
nc
nmap
```

On Windows:

```powershell
ipconfig /all
route print
netstat -ano
nslookup
Test-NetConnection
```

### Be careful with defaults

Defaults are common, but not guaranteed.

Examples:

- SSH usually uses port `22`, but it can be changed.
- HTTPS usually uses port `443`, but not always.
- Jumbo frames often use MTU `9000`, but exact values vary.
- DNS TTLs depend on configuration.
- Voice RTP port ranges depend on vendor.

### Document your own environment

Every network should have its own reference for:

- VLAN IDs
- Subnets
- DHCP scopes
- DNS servers
- Gateway addresses
- Firewall rules
- VPN ports
- QoS markings
- Wireless channels
- Management IPs

### Use consistent numbering

Consistent design makes troubleshooting easier.

Example:

| VLAN | Subnet | Purpose |
|---:|---|---|
| `10` | `10.10.10.0/24` | Management |
| `20` | `10.10.20.0/24` | Users |
| `30` | `10.10.30.0/24` | Voice |
| `40` | `10.10.40.0/24` | Servers |
| `50` | `10.10.50.0/24` | Guests |

This kind of pattern is easier to understand than random numbering.

---

## 32. Quick Summary

Some network numbers appear constantly in real environments.

The most important ones to remember are:

- Ethernet MTU is usually `1500` bytes
- Jumbo frame MTU is commonly around `9000` bytes
- TCP MSS over 1500 MTU with IPv4 is usually `1460` bytes
- IPv4 addresses are `32` bits
- IPv6 addresses are `128` bits
- MAC addresses are `48` bits
- IPv4 default route is `0.0.0.0/0`
- IPv6 default route is `::/0`
- IPv4 loopback is `127.0.0.0/8`
- IPv6 loopback is `::1/128`
- APIPA uses `169.254.0.0/16`
- Private IPv4 ranges are `10.0.0.0/8`, `172.16.0.0/12`, and `192.168.0.0/16`
- DNS uses port `53`
- HTTP uses port `80`
- HTTPS uses port `443`
- SSH uses port `22`
- DHCP uses UDP `67/68`
- NTP uses UDP `123`
- SNMP uses UDP `161/162`
- BGP uses TCP `179`
- SMB uses TCP `445`
- RDP uses TCP `3389`
- Common TTL starts include `64`, `128`, and `255`
- DSCP EF `46` is commonly used for voice
- Voice traffic needs low latency, low jitter, and low packet loss

Knowing these values helps you read logs, understand packet captures, build firewall rules, and troubleshoot faster.
