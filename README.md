# Networking Master Reference

> A simple networking reference for study, interviews, troubleshooting, and daily work.

## Overview

This guide is meant to be:

- Easy to read quickly
- Clear enough for fast review
- Clean in GitHub
- Useful for students, sysadmins, network engineers, and interview prep

---

## Table of Contents

- [1. Network Models](#1-network-models)
- [2. IPv4 Addressing Basics](#2-ipv4-addressing-basics)
- [3. IPv6 Basics](#3-ipv6-basics)
- [4. DNS Reference](#4-dns-reference)
- [5. Common Ports](#5-common-ports)
- [6. Core Network Protocols](#6-core-network-protocols)
- [7. Switching, VLANs, and Layer 2](#7-switching-vlans-and-layer-2)
- [8. Routing Basics](#8-routing-basics)
- [9. NAT, ACLs, Firewalls, and VPNs](#9-nat-acls-firewalls-and-vpns)
- [10. DHCP Reference](#10-dhcp-reference)
- [11. Wireless Networking](#11-wireless-networking)
- [12. Ethernet, Cabling, and Media](#12-ethernet-cabling-and-media)
- [13. Essential Network Commands](#13-essential-network-commands)
- [14. Troubleshooting Flow](#14-troubleshooting-flow)
- [15. Common Network Numbers and Defaults](#15-common-network-numbers-and-defaults)
- [16. Network Design Concepts](#16-network-design-concepts)
- [17. Very Common Exam / Interview Terms](#17-very-common-exam--interview-terms)
- [18. Mini Glossary](#18-mini-glossary)

---

## 1. Network Models

### OSI Model

| Layer | Name | Typical Scope |
|---|---|---|
| 1 | Physical | Cables, signals, optics, radio |
| 2 | Data Link | MAC, Ethernet, VLAN, STP, switching |
| 3 | Network | IPv4, IPv6, routing, ICMP |
| 4 | Transport | TCP, UDP |
| 5 | Session | Session control |
| 6 | Presentation | Encryption, formatting, encoding |
| 7 | Application | HTTP, HTTPS, DNS, DHCP, SMTP, SSH |

### TCP/IP Model

| Layer | Name | Typical Scope |
|---|---|---|
| 1 | Link | Physical + Data Link |
| 2 | Internet | IP, ICMP, ARP/ND, routing |
| 3 | Transport | TCP, UDP |
| 4 | Application | DNS, HTTP, SMTP, DHCP, SSH, and more |

### Common PDU Names

| Layer | PDU |
|---|---|
| L1 | Bits |
| L2 | Frame |
| L3 | Packet |
| L4 | Segment (TCP) / Datagram (UDP) |

---

## 2. IPv4 Addressing Basics

### IPv4 Address Size

- **32 bits**
- Example: `192.168.1.10`

### Address Types

- **Unicast** -> one sender to one receiver
- **Broadcast** -> one sender to all devices in a subnet
- **Multicast** -> one sender to a group
- **Anycast** -> one sender to the nearest available destination

### Private IPv4 Ranges

- `10.0.0.0/8`
- `172.16.0.0/12`
- `192.168.0.0/16`

### Special IPv4 Ranges

| Range / Address | Purpose |
|---|---|
| `0.0.0.0` | Unspecified / default route context |
| `127.0.0.0/8` | Loopback |
| `169.254.0.0/16` | APIPA / Link-local |
| `224.0.0.0/4` | Multicast |
| `240.0.0.0/4` | Reserved |
| `255.255.255.255` | Limited broadcast |
| `100.64.0.0/10` | Carrier-Grade NAT (CGNAT) |

### Subnetting Formula

- **Total addresses** = `2^(32 - prefix)`
- **Usable hosts** = `2^(32 - prefix) - 2`

**Exceptions**
- `/31` is often used on point-to-point links
- `/32` is a single host route

### CIDR / Netmask / Wildcard / Total / Usable

| Prefix | Netmask | Wildcard | Total Addresses | Usable Hosts |
|---|---|---|---:|---:|
| `/8` | `255.0.0.0` | `0.255.255.255` | 16777216 | 16777214 |
| `/16` | `255.255.0.0` | `0.0.255.255` | 65536 | 65534 |
| `/24` | `255.255.255.0` | `0.0.0.255` | 256 | 254 |
| `/25` | `255.255.255.128` | `0.0.0.127` | 128 | 126 |
| `/26` | `255.255.255.192` | `0.0.0.63` | 64 | 62 |
| `/27` | `255.255.255.224` | `0.0.0.31` | 32 | 30 |
| `/28` | `255.255.255.240` | `0.0.0.15` | 16 | 14 |
| `/29` | `255.255.255.248` | `0.0.0.7` | 8 | 6 |
| `/30` | `255.255.255.252` | `0.0.0.3` | 4 | 2 |
| `/31` | `255.255.255.254` | `0.0.0.1` | 2 | P2P use |
| `/32` | `255.255.255.255` | `0.0.0.0` | 1 | 1 host |

### Useful Subnetting Notes

- **Network address** = first address in the subnet
- **Broadcast address** = last address in the subnet
- **First usable** = network + 1
- **Last usable** = broadcast - 1

### Example

For `192.168.1.0/24`:

- **Network:** `192.168.1.0`
- **Usable range:** `192.168.1.1 - 192.168.1.254`
- **Broadcast:** `192.168.1.255`

---

## 3. IPv6 Basics

### IPv6 Address Size

- **128 bits**
- Example: `2001:db8::1`

### IPv6 Key Points

- There is no broadcast
- IPv6 uses multicast and anycast
- Addresses are usually written in hexadecimal
- Leading zeros can be removed
- One continuous block of zeros can be shortened with `::`

### Common IPv6 Ranges

| Prefix | Purpose |
|---|---|
| `::1/128` | Loopback |
| `::/0` | Default route |
| `fe80::/10` | Link-local |
| `fc00::/7` | Unique Local Address (ULA) |
| `2000::/3` | Global unicast |
| `ff00::/8` | Multicast |

### Common IPv6 Prefixes

- `/64` is the standard subnet size
- `/128` is a single host

### IPv6 Essentials

- **SLAAC** -> Stateless Address Autoconfiguration
- **DHCPv6** -> IPv6 address and option assignment
- **NDP** -> Neighbor Discovery Protocol
- **ICMPv6** -> required for normal IPv6 operation

---

## 4. DNS Reference

### What DNS Does

DNS turns names into IP addresses and can also return other types of records.

### Common Public DNS Resolvers

| Provider | Primary | Secondary |
|---|---|---|
| Google | `8.8.8.8` | `8.8.4.4` |
| Cloudflare | `1.1.1.1` | `1.0.0.1` |
| Quad9 | `9.9.9.9` | `149.112.112.112` |
| OpenDNS | `208.67.222.222` | `208.67.220.220` |
| AdGuard | `94.140.14.14` | `94.140.15.15` |

### Common DNS Record Types

| Record | Purpose |
|---|---|
| `A` | Hostname to IPv4 |
| `AAAA` | Hostname to IPv6 |
| `CNAME` | Alias to another name |
| `MX` | Mail server |
| `NS` | Authoritative name server |
| `TXT` | Text data, SPF, domain verification |
| `PTR` | Reverse lookup |
| `SOA` | Start of authority |
| `SRV` | Service locator |
| `CAA` | Certificate authority authorization |
| `NAPTR` | Rewrite / service discovery |
| `DS` | DNSSEC delegation signer |
| `DNSKEY` | DNSSEC public key |
| `RRSIG` | DNSSEC signature |

### DNS Ports

| Port | Protocol | Use |
|---|---|---|
| `53` | UDP | Standard queries |
| `53` | TCP | Zone transfers / large responses |
| `853` | TCP | DNS over TLS (DoT) |
| `443` | TCP | DNS over HTTPS (DoH) |

### DNS Lookup Order (Simplified)

`Client -> Resolver -> Root -> TLD -> Authoritative -> Answer`

---

## 5. Common Ports

| Port | Protocol | Service |
|---|---|---|
| `20/21` | TCP | FTP |
| `22` | TCP | SSH |
| `23` | TCP | Telnet |
| `25` | TCP | SMTP |
| `53` | TCP/UDP | DNS |
| `67/68` | UDP | DHCP |
| `69` | UDP | TFTP |
| `80` | TCP | HTTP |
| `88` | TCP/UDP | Kerberos |
| `110` | TCP | POP3 |
| `123` | UDP | NTP |
| `135` | TCP | RPC Endpoint Mapper |
| `137-139` | TCP/UDP | NetBIOS |
| `143` | TCP | IMAP |
| `161/162` | UDP | SNMP / Traps |
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
| `1521` | TCP | Oracle DB |
| `1701` | UDP | L2TP |
| `1723` | TCP | PPTP |
| `1812/1813` | UDP | RADIUS Auth/Acct |
| `2049` | TCP/UDP | NFS |
| `3306` | TCP | MySQL |
| `3389` | TCP | RDP |
| `4500` | UDP | IPsec NAT-T |
| `5060` | UDP/TCP | SIP |
| `5061` | TCP | SIP-TLS |
| `5432` | TCP | PostgreSQL |
| `5900` | TCP | VNC |
| `8080` | TCP | HTTP Alternate |
| `8443` | TCP | HTTPS Alternate |

---

## 6. Core Network Protocols

### Layer 2 / Neighboring

- **ARP** -> IPv4 address to MAC resolution
- **NDP** -> IPv6 neighbor discovery
- **STP** -> prevents switching loops
- **LLDP** -> device discovery
- **LACP** -> link aggregation

### Layer 3 / Routing

- IPv4
- IPv6
- ICMP / ICMPv6
- OSPF
- RIP
- EIGRP
- BGP

### Layer 4 / Transport

- **TCP** -> reliable and connection-oriented
- **UDP** -> fast and connectionless

### Application / Services

- DNS
- DHCP / DHCPv6
- HTTP / HTTPS
- SMTP / IMAP / POP3
- SNMP
- NTP
- SSH
- LDAP
- RADIUS / TACACS+

### ICMP Message Examples

- Echo request / reply
- Destination unreachable
- Time exceeded
- Redirect

### TCP 3-Way Handshake

1. SYN
2. SYN-ACK
3. ACK

### TCP Common Flags

- SYN
- ACK
- FIN
- RST
- PSH
- URG
- ECE
- CWR

---

## 7. Switching, VLANs, and Layer 2

### Key Terms

- **MAC Address** -> Layer 2 hardware address
- **CAM Table** -> switch MAC address table
- **Broadcast Domain** -> devices that receive broadcasts
- **Collision Domain** -> the area where collisions can happen on shared media

### Switch Port Types

- **Access Port** -> carries one VLAN
- **Trunk Port** -> carries multiple VLANs
- **Native VLAN** -> untagged VLAN on an 802.1Q trunk

### VLAN Basics

- A VLAN is logical separation at Layer 2
- The common default VLAN is **VLAN 1**
- Inter-VLAN routing needs a Layer 3 device or an SVI

### 802.1Q Tagging

- Adds a VLAN tag to an Ethernet frame
- Native VLAN traffic is usually untagged

### STP / RSTP

**Purpose:** stop loops in switched networks

Common RSTP states:

- Discarding
- Learning
- Forwarding

### Link Aggregation

- **LACP** is the IEEE standard for bundling links
- It improves bandwidth and redundancy

### PoE Standards

| Standard | Common Name |
|---|---|
| `802.3af` | PoE |
| `802.3at` | PoE+ |
| `802.3bt` | PoE++ |

---

## 8. Routing Basics

### Routing Concepts

- Connected route
- Static route
- Default route
- Dynamic routing
- Longest prefix match always wins

### Default Routes

- IPv4 -> `0.0.0.0/0`
- IPv6 -> `::/0`

### Common Routing Protocols

#### RIP
- Distance vector
- Metric: hop count
- Best for small and simple networks

#### OSPF
- Link-state
- Metric: cost
- Supports areas
- Common in enterprise internal routing

#### EIGRP
- Advanced distance vector / hybrid
- Common in Cisco-focused environments

#### BGP
- Path vector
- Used between autonomous systems
- Built for Internet-scale routing

### First Hop Redundancy

- HSRP
- VRRP
- GLBP

### Routing Terms

- Next hop
- Metric
- Administrative distance
- Convergence
- Redistribution
- Summarization

---

## 9. NAT, ACLs, Firewalls, and VPNs

### NAT Types

- **Static NAT** -> one private IP maps to one public IP
- **Dynamic NAT** -> uses a pool of public IPs
- **PAT / NAPT** -> many private IPs share one public IP by using ports

### Translation Directions

- **SNAT** -> Source NAT
- **DNAT** -> Destination NAT

### ACLs

- **Standard ACL** -> filters mainly by source
- **Extended ACL** -> filters by source, destination, port, and protocol

### Firewall Types

- Stateless firewall
- Stateful firewall
- Next-generation firewall (NGFW)

### VPN Types

- Site-to-site VPN
- Remote access VPN
- SSL VPN
- IPsec VPN
- GRE tunnel

### IPsec Terms

| Term | Meaning |
|---|---|
| AH | Authentication Header |
| ESP | Encapsulating Security Payload |
| IKE | Key exchange |
| NAT-T | IPsec through NAT using UDP 4500 |

---

## 10. DHCP Reference

### DHCP Purpose

DHCP gives devices IP settings automatically.

### DHCP DORA Process

1. Discover
2. Offer
3. Request
4. Acknowledge

### Common DHCP Parameters

- IP address
- Subnet mask
- Default gateway
- DNS server
- Lease time

### DHCP Ports

- **Client:** UDP `68`
- **Server:** UDP `67`

---

## 11. Wireless Networking

### Wi-Fi Standards

| Standard | Band |
|---|---|
| `802.11a` | 5 GHz |
| `802.11b` | 2.4 GHz |
| `802.11g` | 2.4 GHz |
| `802.11n` | 2.4 / 5 GHz |
| `802.11ac` | 5 GHz |
| `802.11ax` | 2.4 / 5 / 6 GHz (Wi-Fi 6 / 6E) |

### Common Wi-Fi Terms

- **SSID** -> network name
- **BSSID** -> radio MAC identity
- **Channel Width** -> 20 / 40 / 80 / 160 MHz
- **MIMO** -> multiple antennas / streams
- **RSSI** -> signal strength indicator

### 2.4 GHz Notes

- Longer range
- More interference
- Common non-overlapping channels: **1, 6, 11**

### 5 GHz Notes

- Higher throughput
- Shorter range
- More channels

### 6 GHz Notes

- Cleaner spectrum
- Newer devices
- Common in Wi-Fi 6E / Wi-Fi 7 setups

### Wireless Security

- **WEP** -> old and insecure
- **WPA** -> legacy
- **WPA2** -> common standard
- **WPA3** -> stronger modern standard
- **PSK** -> pre-shared key
- **Enterprise** -> 802.1X / RADIUS

---

## 12. Ethernet, Cabling, and Media

### Common Ethernet Speeds

- 10BASE-T
- 100BASE-TX
- 1000BASE-T
- 10GBASE-T
- 25G / 40G / 100G / 400G Ethernet in higher-end environments

### Copper Cable Categories

| Cable | Typical Use |
|---|---|
| Cat5e | Common for 1G |
| Cat6 | Better performance |
| Cat6a | Better for 10G and lower crosstalk |

### Typical Twisted Pair Distance

- Up to **100 meters** for standard Ethernet runs

### Fiber Types

- Multimode Fiber (MMF)
- Single-mode Fiber (SMF)

### Common Fiber Connector Terms

- LC
- SC
- ST

### Transceiver Terms

- SFP
- SFP+
- SFP28
- QSFP+
- QSFP28

### Duplex Settings

- Half-duplex
- Full-duplex

### Important Layer 1 / Layer 2 Issues

- Speed mismatch
- Duplex mismatch
- Bad cable
- CRC errors
- Interface flaps

---

## 13. Essential Network Commands

### Windows

```powershell
ipconfig /all
ping 8.8.8.8
tracert example.com
nslookup example.com
arp -a
route print
netstat -ano
pathping example.com
```

### Linux / macOS

```bash
ip addr
ip route
ping 8.8.8.8
traceroute example.com
dig example.com
nslookup example.com
arp -a
ss -tulpn
netstat -tulpn
tcpdump -i eth0
mtr example.com
curl -I https://example.com
nc -vz host port
```

### Cross-Platform Useful Tools

- **Wireshark** -> packet capture and analysis
- **Nmap** -> port scanning and service discovery
- **iperf3** -> throughput testing
- **telnet / nc** -> simple TCP connectivity testing

### Quick Examples

**Ping the gateway**
```bash
ping 192.168.1.1
```

**Check a DNS A record**
```bash
nslookup example.com
dig example.com A
```

**Trace the path**
```bash
tracert 8.8.8.8
traceroute 8.8.8.8
```

**Check listening ports**
```bash
netstat -ano
ss -lntup
```

---

## 14. Troubleshooting Flow

### Work Layer by Layer

#### 1) Physical
- Power
- Cable
- Link light
- Speed / duplex
- SFP / transceiver

#### 2) Data Link
- Correct VLAN
- MAC learning
- STP blocking
- Port security

#### 3) Network
- IP address
- Subnet mask / prefix
- Default gateway
- Route table
- ARP / NDP
- ACL / firewall

#### 4) Transport
- TCP / UDP port open
- Session setup
- RST / timeout

#### 5) Application
- DNS records
- Service status
- Certificates
- Authentication
- Application misconfiguration

### Quick Isolation Questions

- Can I ping localhost?
- Can I ping my own IP?
- Can I ping the gateway?
- Can I ping a remote IP?
- Can I resolve DNS?
- Can I reach the service port?
- Is the firewall blocking it?

### Common Symptoms

| Symptom | Likely Cause |
|---|---|
| No link | Cable, NIC, or switch problem |
| APIPA address | DHCP failure |
| Can ping IP but not name | DNS problem |
| Same VLAN works only | Routing problem |
| Intermittent drops | Duplex, RF, bad cable, congestion |
| Slow network | Errors, congestion, MTU, QoS, server load |

---

## 15. Common Network Numbers and Defaults

### General

| Item | Common Value |
|---|---|
| Ethernet MTU | `1500` bytes |
| Jumbo frame MTU | `9000` bytes |
| TCP MSS over 1500 MTU | `1460` bytes |
| HTTPS default port | `443` |
| DNS default port | `53` |
| SSH default port | `22` |

### TTL / Hop Limit

- IPv4 uses **TTL**
- IPv6 uses **Hop Limit**

Common starting values you often see:

- `64`
- `128`
- `255`

### QoS / DSCP Terms

- Best Effort
- AF classes
- EF (often used for voice)

### Voice / Real-Time Considerations

- Low latency
- Low jitter
- Low packet loss
- QoS matters

---

## 16. Network Design Concepts

### Segmentation

- VLANs separate Layer 2 domains
- Subnets separate Layer 3 domains
- Firewalls enforce policy between zones

### Redundancy

- Dual links
- Dual switches
- Link aggregation
- First-hop redundancy
- Dynamic routing failover

### High Availability

- Remove single points of failure
- Use redundant power and uplinks
- Monitor devices and links

### Security Best Practices

- Disable unused ports
- Use SSH instead of Telnet
- Use SNMPv3 instead of SNMPv1/v2c when possible
- Change default credentials
- Use ACLs and firewalls
- Segment users, servers, voice, and management networks
- Keep firmware and software updated
- Use WPA2/WPA3 for wireless
- Log to Syslog / SIEM
- Protect management plane access

---

## 17. Very Common Exam / Interview Terms

- Broadcast domain
- Collision domain
- CIDR
- Subnet mask
- Wildcard mask
- Default gateway
- ARP
- NAT / PAT
- VLAN
- STP / RSTP
- Trunk / Access port
- OSPF / BGP
- TCP vs UDP
- DNS A / AAAA / CNAME / MX / TXT
- DHCP DORA
- MTU / MSS
- SLAAC / DHCPv6
- HSRP / VRRP
- QoS
- VPN
- Load balancer
- Reverse proxy

---

## 18. Mini Glossary

| Term | Meaning |
|---|---|
| ACL | Access Control List |
| AF | Assured Forwarding |
| AH | Authentication Header |
| APIPA | Automatic Private IP Addressing |
| ARP | Address Resolution Protocol |
| AS | Autonomous System |
| BGP | Border Gateway Protocol |
| CAM | Content Addressable Memory |
| CIDR | Classless Inter-Domain Routing |
| CRC | Cyclic Redundancy Check |
| DHCP | Dynamic Host Configuration Protocol |
| DNS | Domain Name System |
| ESP | Encapsulating Security Payload |
| FHRP | First Hop Redundancy Protocol |
| HSRP | Hot Standby Router Protocol |
| ICMP | Internet Control Message Protocol |
| IKE | Internet Key Exchange |
| IPsec | Internet Protocol Security |
| LACP | Link Aggregation Control Protocol |
| LDAP | Lightweight Directory Access Protocol |
| LLDP | Link Layer Discovery Protocol |
| MAC | Media Access Control |
| MSS | Maximum Segment Size |
| MTU | Maximum Transmission Unit |
| NAT | Network Address Translation |
| NDP | Neighbor Discovery Protocol |
| NTP | Network Time Protocol |
| OSPF | Open Shortest Path First |
| PAT | Port Address Translation |
| PDU | Protocol Data Unit |
| PoE | Power over Ethernet |
| QoS | Quality of Service |
| RADIUS | Remote Authentication Dial-In User Service |
| RSTP | Rapid Spanning Tree Protocol |
| SFP | Small Form-factor Pluggable |
| SIP | Session Initiation Protocol |
| SLAAC | Stateless Address Autoconfiguration |
| SMB | Server Message Block |
| SNMP | Simple Network Management Protocol |
| SOA | Start of Authority |
| SSH | Secure Shell |
| SSID | Service Set Identifier |
| STP | Spanning Tree Protocol |
| SVI | Switched Virtual Interface |
| TCP | Transmission Control Protocol |
| TLS | Transport Layer Security |
| UDP | User Datagram Protocol |
| ULA | Unique Local Address |
| VLAN | Virtual Local Area Network |
| VPN | Virtual Private Network |
| VRRP | Virtual Router Redundancy Protocol |
| WEP | Wired Equivalent Privacy |
| WPA | Wi-Fi Protected Access |

---