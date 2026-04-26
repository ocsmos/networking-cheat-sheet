# 18. Mini Glossary

This glossary explains common networking terms in simple English.

The goal is not only to define each term, but also to show where it appears in real networks and why it matters during configuration, troubleshooting, interviews, and daily network work.

---

## Table of Contents

- [1. How to Use This Glossary](#1-how-to-use-this-glossary)
- [2. Terms by Category](#2-terms-by-category)
- [3. Full Glossary](#3-full-glossary)
  - [ACL](#acl)
  - [AF](#af)
  - [AH](#ah)
  - [APIPA](#apipa)
  - [ARP](#arp)
  - [AS](#as)
  - [BGP](#bgp)
  - [CAM](#cam)
  - [CIDR](#cidr)
  - [CRC](#crc)
  - [DHCP](#dhcp)
  - [DNS](#dns)
  - [ESP](#esp)
  - [FHRP](#fhrp)
  - [HSRP](#hsrp)
  - [ICMP](#icmp)
  - [IKE](#ike)
  - [IPsec](#ipsec)
  - [LACP](#lacp)
  - [LDAP](#ldap)
  - [LLDP](#lldp)
  - [MAC](#mac)
  - [MSS](#mss)
  - [MTU](#mtu)
  - [NAT](#nat)
  - [NDP](#ndp)
  - [NTP](#ntp)
  - [OSPF](#ospf)
  - [PAT](#pat)
  - [PDU](#pdu)
  - [PoE](#poe)
  - [QoS](#qos)
  - [RADIUS](#radius)
  - [RSTP](#rstp)
  - [SFP](#sfp)
  - [SIP](#sip)
  - [SLAAC](#slaac)
  - [SMB](#smb)
  - [SNMP](#snmp)
  - [SOA](#soa)
  - [SSH](#ssh)
  - [SSID](#ssid)
  - [STP](#stp)
  - [SVI](#svi)
  - [TCP](#tcp)
  - [TLS](#tls)
  - [UDP](#udp)
  - [ULA](#ula)
  - [VLAN](#vlan)
  - [VPN](#vpn)
  - [VRRP](#vrrp)
  - [WEP](#wep)
  - [WPA](#wpa)
- [4. Quick Review Table](#4-quick-review-table)
- [5. Final Notes](#5-final-notes)

---

## 1. How to Use This Glossary

This glossary is useful when you see an abbreviation and need a quick but clear explanation.

Each entry includes:

- **Full name**
- **Simple meaning**
- **Where it is used**
- **Why it matters**
- **Small example or practical note**

You can use it for:

- Network study
- Interview preparation
- Troubleshooting
- Reading documentation
- Reviewing firewall, routing, and switching concepts
- Understanding logs and configuration examples

---

## 2. Terms by Category

### Switching and Layer 2

- ARP
- CAM
- CRC
- LACP
- LLDP
- MAC
- PoE
- RSTP
- STP
- SVI
- VLAN

### Routing and Layer 3

- AS
- BGP
- CIDR
- FHRP
- HSRP
- ICMP
- NDP
- OSPF
- ULA
- VRRP

### Transport and Performance

- MSS
- MTU
- TCP
- UDP
- QoS
- AF

### Security and VPN

- ACL
- AH
- ESP
- IKE
- IPsec
- NAT
- PAT
- RADIUS
- SSH
- TLS
- VPN

### Services and Applications

- APIPA
- DHCP
- DNS
- LDAP
- NTP
- SIP
- SLAAC
- SMB
- SNMP
- SOA
- SSID
- WEP
- WPA

---

## 3. Full Glossary

---

## ACL

**Full name:** Access Control List

An ACL is a set of rules used to allow or deny traffic.

ACLs are common on routers, switches, firewalls, and cloud security systems. They are usually based on information such as source IP, destination IP, protocol, and port number.

### Where it is used

- Routers
- Firewalls
- Layer 3 switches
- Cloud security groups
- Network access control
- Traffic filtering

### Why it matters

ACLs control who can talk to whom. A wrong ACL can block important traffic or accidentally allow traffic that should be denied.

### Simple example

Allow SSH only from the admin subnet:

```text
Source: 10.10.50.0/24
Destination: 10.10.10.20
Protocol: TCP
Port: 22
Action: Allow
```

Block everything else:

```text
Action: Deny
```

### Common mistake

Forgetting that many ACL systems have an implicit deny at the end. If traffic is not explicitly allowed, it may be blocked.

---

## AF

**Full name:** Assured Forwarding

AF is a group of DSCP values used in QoS to mark traffic with different levels of forwarding priority and drop preference.

It is often written as values like:

```text
AF11
AF21
AF31
AF41
```

### Where it is used

- QoS policies
- WAN links
- Voice and video networks
- Enterprise traffic classification
- ISP traffic engineering

### Why it matters

AF helps networks treat different kinds of traffic differently during congestion.

For example, business-critical traffic may receive better forwarding treatment than normal bulk traffic.

### Simple example

A company may classify traffic like this:

```text
Voice signaling -> AF31
Video traffic   -> AF41
Bulk backup     -> AF11
```

### Practical note

AF does not magically create more bandwidth. It only helps decide how traffic is handled when the network is busy.

---

## AH

**Full name:** Authentication Header

AH is part of IPsec. It provides authentication and integrity for IP packets.

In simple terms, AH helps verify that packets are really from the expected source and were not changed in transit.

### Where it is used

- IPsec VPNs
- Secure network tunnels
- Environments that require packet integrity checking

### Why it matters

AH helps protect against packet tampering, but it does not encrypt the packet payload.

### AH vs ESP

| Feature | AH | ESP |
|---|---|---|
| Authentication | Yes | Yes, if configured |
| Integrity | Yes | Yes, if configured |
| Encryption | No | Yes |
| NAT-friendly | Usually problematic | Better with NAT-T |

### Practical note

In many real-world IPsec VPN deployments, ESP is more commonly used than AH, especially when encryption and NAT traversal are needed.

---

## APIPA

**Full name:** Automatic Private IP Addressing

APIPA is a fallback IPv4 address range used when a device cannot get an address from DHCP.

The range is:

```text
169.254.0.0/16
```

### Where it is used

- Windows clients
- Some other operating systems
- Local link communication when DHCP fails

### Why it matters

If a device has an address like:

```text
169.254.23.10
```

it usually means the device failed to get a DHCP address.

### Common causes

- DHCP server is down
- DHCP scope is exhausted
- Wrong VLAN
- DHCP relay is missing
- Cable or Wi-Fi issue
- Firewall blocking DHCP

### Troubleshooting tip

When you see a `169.254.x.x` address, check DHCP first.

---

## ARP

**Full name:** Address Resolution Protocol

ARP is used in IPv4 networks to map an IPv4 address to a MAC address.

A device may know the destination IP address, but to send an Ethernet frame on the local network, it needs the destination MAC address.

### Where it is used

- IPv4 LANs
- Ethernet networks
- Local subnet communication
- Gateway discovery at Layer 2

### Why it matters

Without ARP, IPv4 devices on an Ethernet LAN would not know which MAC address belongs to which IP address.

### Simple example

A laptop wants to send traffic to its gateway:

```text
Gateway IP: 192.168.1.1
```

The laptop asks:

```text
Who has 192.168.1.1?
```

The router replies:

```text
192.168.1.1 is at AA:BB:CC:11:22:33
```

### Useful command

Windows, Linux, and macOS:

```bash
arp -a
```

### IPv6 note

IPv6 does not use ARP. IPv6 uses NDP instead.

---

## AS

**Full name:** Autonomous System

An Autonomous System is a group of IP networks managed by one organization under one routing policy.

Each AS has an Autonomous System Number, usually called an ASN.

### Where it is used

- BGP
- ISP routing
- Internet routing
- Large enterprise networks
- Cloud provider routing

### Why it matters

The Internet is made of many autonomous systems connected together. BGP is used to exchange routing information between them.

### Simple example

An ISP may have an ASN like:

```text
AS64500
```

A company connected to multiple ISPs may also have its own ASN.

### Practical note

Private ASNs are often used inside labs, private networks, or internal BGP designs.

---

## BGP

**Full name:** Border Gateway Protocol

BGP is the routing protocol used to exchange routes between autonomous systems.

It is the main routing protocol of the Internet.

### Where it is used

- ISP networks
- Internet routing
- Data centers
- Cloud connectivity
- Multi-homed enterprise networks
- Large-scale internal routing designs

### Why it matters

BGP decides how traffic moves between large networks. It is policy-driven, which means administrators can influence routing decisions using attributes and rules.

### Simple example

A company connected to two ISPs may use BGP to advertise its public IP prefix to both providers.

```text
Company network -> ISP 1
Company network -> ISP 2
```

If one ISP link fails, traffic can still reach the company through the other ISP.

### Common BGP ideas

- AS path
- Local preference
- MED
- Route advertisements
- Route filtering
- Peering
- eBGP
- iBGP

### Practical note

BGP is powerful, but mistakes can have a large impact. Always filter and document BGP changes carefully.

---

## CAM

**Full name:** Content Addressable Memory

In switching, the CAM table is where a switch stores MAC address information.

It tells the switch which MAC address is reachable through which port.

### Where it is used

- Ethernet switches
- Layer 2 forwarding
- VLAN-based switching

### Why it matters

The CAM table lets a switch forward frames intelligently instead of flooding every frame everywhere.

### Simple example

A switch learns:

```text
MAC Address          Port
AA:AA:AA:AA:AA:01    Gi0/1
BB:BB:BB:BB:BB:02    Gi0/2
```

If traffic is going to `BB:BB:BB:BB:BB:02`, the switch sends it only out `Gi0/2`.

### Common issue

If the same MAC address appears on different ports repeatedly, that may indicate:

- Switching loop
- Misconfigured link aggregation
- Virtualization behavior
- MAC flapping

---

## CIDR

**Full name:** Classless Inter-Domain Routing

CIDR is a way to write IP networks using prefix length notation.

Example:

```text
192.168.1.0/24
```

The `/24` means the first 24 bits are the network part.

### Where it is used

- IP addressing
- Routing tables
- Firewall rules
- Cloud networking
- Subnetting
- Route summarization

### Why it matters

CIDR replaced older classful addressing and made IP address allocation more flexible.

### Simple examples

```text
10.0.0.0/8
172.16.0.0/12
192.168.1.0/24
192.168.1.128/25
```

### Practical note

CIDR is one of the most important skills in networking. You need it for subnetting, firewall rules, routing, VPNs, and cloud networks.

---

## CRC

**Full name:** Cyclic Redundancy Check

CRC is an error-checking method used to detect corrupted data.

In Ethernet, CRC errors usually mean a frame was damaged before it arrived correctly.

### Where it is used

- Ethernet frames
- Network interface counters
- Storage systems
- Data transmission checks

### Why it matters

CRC errors are often a sign of Layer 1 problems.

### Common causes

- Bad cable
- Dirty fiber
- Faulty transceiver
- Bad NIC
- Duplex mismatch
- Electrical interference
- Damaged switch port

### Troubleshooting tip

If an interface shows many CRC errors, check the physical layer first.

Look at:

- Cable
- SFP/transceiver
- Fiber cleanliness
- Speed and duplex
- Interface counters

---

## DHCP

**Full name:** Dynamic Host Configuration Protocol

DHCP automatically assigns network settings to clients.

It commonly gives:

- IP address
- Subnet mask
- Default gateway
- DNS servers
- Lease time

### Where it is used

- Home networks
- Enterprise networks
- Wi-Fi networks
- Guest networks
- Data centers
- Cloud networks

### Why it matters

Without DHCP, administrators would need to manually configure network settings on every device.

### DORA process

DHCP commonly uses this process:

```text
Discover
Offer
Request
Acknowledge
```

### DHCP ports

```text
Server: UDP 67
Client: UDP 68
```

### Troubleshooting tip

If a client gets a `169.254.x.x` address, DHCP likely failed.

---

## DNS

**Full name:** Domain Name System

DNS translates names into IP addresses and other records.

Example:

```text
example.com -> 93.184.216.34
```

### Where it is used

- Web browsing
- Email delivery
- Active Directory
- Cloud services
- APIs
- Internal applications

### Why it matters

Many problems that look like Internet problems are actually DNS problems.

### Common DNS records

| Record | Purpose |
|---|---|
| A | Name to IPv4 address |
| AAAA | Name to IPv6 address |
| CNAME | Alias to another name |
| MX | Mail servers |
| TXT | Text and verification data |
| NS | Name servers |
| PTR | Reverse DNS |

### Troubleshooting tip

If you can reach an IP address but not a domain name, check DNS.

---

## ESP

**Full name:** Encapsulating Security Payload

ESP is part of IPsec. It can provide encryption, authentication, and integrity for IP packets.

### Where it is used

- IPsec VPNs
- Site-to-site VPNs
- Remote access VPNs
- Secure tunnels

### Why it matters

ESP is commonly used to protect traffic inside IPsec VPN tunnels.

### ESP can provide

- Confidentiality through encryption
- Integrity checking
- Authentication
- Anti-replay protection

### Practical note

When IPsec is used through NAT, NAT-T often encapsulates ESP traffic in UDP port `4500`.

---

## FHRP

**Full name:** First Hop Redundancy Protocol

FHRP is a general term for protocols that provide default gateway redundancy.

### Where it is used

- Enterprise LANs
- Data centers
- Layer 3 switching
- Router redundancy designs

### Why it matters

Clients usually have one default gateway. If that gateway fails, clients may lose connectivity.

FHRP solves this by providing a virtual gateway shared by multiple routers or Layer 3 switches.

### Common FHRP protocols

- HSRP
- VRRP
- GLBP

### Simple example

Two routers share a virtual IP:

```text
Virtual gateway: 192.168.1.1
Router A:        192.168.1.2
Router B:        192.168.1.3
```

Clients use `192.168.1.1` as the default gateway.

If Router A fails, Router B can take over.

---

## HSRP

**Full name:** Hot Standby Router Protocol

HSRP is a Cisco first-hop redundancy protocol.

It allows multiple routers or Layer 3 switches to share a virtual default gateway.

### Where it is used

- Cisco networks
- Campus LANs
- Data centers
- Redundant gateway designs

### Why it matters

HSRP keeps client traffic working if the active gateway fails.

### Simple example

```text
Virtual IP: 192.168.10.1
Active router: Router A
Standby router: Router B
```

Clients use the virtual IP as their default gateway.

If Router A fails, Router B becomes active.

### Related term

VRRP is similar but is an open standard.

---

## ICMP

**Full name:** Internet Control Message Protocol

ICMP is used for network control messages, errors, and diagnostics.

The most famous ICMP tool is `ping`.

### Where it is used

- Ping
- Traceroute behavior
- Error messages
- Path MTU discovery
- Network troubleshooting

### Why it matters

ICMP helps devices report network problems.

### Common ICMP messages

- Echo request
- Echo reply
- Destination unreachable
- Time exceeded
- Redirect

### Practical note

Blocking all ICMP can make troubleshooting harder and may break some network functions, especially in IPv6 where ICMPv6 is essential.

---

## IKE

**Full name:** Internet Key Exchange

IKE is used by IPsec to negotiate security settings and keys.

### Where it is used

- IPsec VPNs
- Site-to-site VPNs
- Remote access VPNs

### Why it matters

Before IPsec can encrypt traffic, both sides need to agree on security parameters. IKE handles that negotiation.

### Common ports

```text
UDP 500   IKE
UDP 4500  IKE/IPsec NAT-T
```

### Common versions

- IKEv1
- IKEv2

IKEv2 is newer and generally preferred in modern designs.

### Troubleshooting tip

If an IPsec VPN fails to come up, check IKE settings such as:

- Pre-shared key or certificate
- Encryption algorithms
- Authentication method
- Peer IP address
- NAT-T
- Phase 1 / Phase 2 settings

---

## IPsec

**Full name:** Internet Protocol Security

IPsec is a suite of protocols used to secure IP traffic.

It is commonly used for VPNs.

### Where it is used

- Site-to-site VPNs
- Remote access VPNs
- Secure tunnels
- Cloud VPN connections
- Router-to-router encryption

### Why it matters

IPsec can protect traffic across untrusted networks, such as the Internet.

### Important IPsec parts

- IKE
- AH
- ESP
- Security associations
- Encryption algorithms
- Authentication methods
- NAT-T

### Simple example

A branch office connects securely to headquarters over the Internet:

```text
Branch firewall <--IPsec VPN--> Headquarters firewall
```

Traffic between the two sites is encrypted.

---

## LACP

**Full name:** Link Aggregation Control Protocol

LACP is used to bundle multiple physical links into one logical link.

It is part of IEEE 802.3ad / 802.1AX link aggregation.

### Where it is used

- Switch uplinks
- Server connections
- Storage networks
- Data center links
- Redundant connections

### Why it matters

LACP can improve redundancy and bandwidth.

If one physical link fails, the bundle can continue working with the remaining links.

### Simple example

Four 1 Gbps links can be bundled into one logical port-channel:

```text
4 x 1 Gbps links -> 1 logical LACP bundle
```

### Practical note

A single flow usually does not use all links at the same time. Traffic is distributed based on a hashing method.

---

## LDAP

**Full name:** Lightweight Directory Access Protocol

LDAP is used to access and query directory services.

### Where it is used

- User directories
- Microsoft Active Directory
- Authentication systems
- Application user lookup
- Group membership lookup

### Why it matters

Many applications use LDAP to check users, groups, and directory information.

### Common ports

```text
389/TCP  LDAP
636/TCP  LDAPS
```

### Simple example

An application may ask LDAP:

```text
Does user alice exist?
Which groups does alice belong to?
```

### Security note

Use LDAPS or another protected method when sending sensitive authentication-related information.

---

## LLDP

**Full name:** Link Layer Discovery Protocol

LLDP is a vendor-neutral protocol used by network devices to advertise information about themselves to directly connected neighbors.

### Where it is used

- Switches
- Routers
- Access points
- IP phones
- Servers
- Network inventory systems

### Why it matters

LLDP helps you discover what is connected to each switch port.

### Information LLDP may show

- Device name
- Port ID
- VLAN information
- Capabilities
- Management IP
- System description

### Simple example

A switch may show that port `Gi1/0/10` is connected to:

```text
Device: AP-Floor2
Port: eth0
Management IP: 10.10.20.15
```

### Related term

CDP is Cisco’s proprietary discovery protocol. LLDP is open standard.

---

## MAC

**Full name:** Media Access Control

A MAC address is a Layer 2 hardware address used on Ethernet and Wi-Fi networks.

### Where it is used

- Ethernet switching
- Wi-Fi communication
- ARP
- DHCP reservations
- Access control
- Switch MAC tables

### Format example

```text
AA:BB:CC:11:22:33
```

or:

```text
AA-BB-CC-11-22-33
```

### Why it matters

Switches use MAC addresses to forward Ethernet frames inside a LAN.

### MAC vs IP

| Address | Layer | Purpose |
|---|---|---|
| MAC | Layer 2 | Local network delivery |
| IP | Layer 3 | Logical addressing and routing |

### Practical note

MAC addresses are important locally, but routers do not forward traffic based on the original source MAC across the Internet.

---

## MSS

**Full name:** Maximum Segment Size

MSS is the largest amount of TCP payload data that can be sent in one TCP segment.

### Where it is used

- TCP communication
- VPN troubleshooting
- MTU problems
- Performance tuning
- Path MTU issues

### Why it matters

If MSS is too large for the network path, packets may be fragmented or dropped.

### Common value

On a normal Ethernet network with MTU 1500:

```text
MTU: 1500 bytes
IPv4 header: 20 bytes
TCP header: 20 bytes
MSS: 1460 bytes
```

### VPN note

VPN tunnels add extra headers, which reduce the effective space available for user data. MSS clamping may be used to avoid fragmentation problems.

---

## MTU

**Full name:** Maximum Transmission Unit

MTU is the largest packet size that can be sent over a network link without fragmentation.

### Where it is used

- Ethernet
- VPNs
- WAN links
- Cloud networking
- Tunnels
- Jumbo frames

### Common value

Standard Ethernet MTU is usually:

```text
1500 bytes
```

Jumbo frames often use:

```text
9000 bytes
```

### Why it matters

MTU problems can cause strange connectivity issues.

For example, small pings may work, but large transfers fail.

### Common causes of MTU issues

- VPN overhead
- GRE tunnels
- IPsec tunnels
- PPPoE
- Cloud network encapsulation
- Blocked ICMP path MTU discovery

### Troubleshooting tip

If some websites or applications hang while others work, MTU may be worth checking.

---

## NAT

**Full name:** Network Address Translation

NAT changes IP address information in packets as they pass through a router or firewall.

### Where it is used

- Home routers
- Firewalls
- Internet edge networks
- Cloud networks
- IPv4 conservation
- Port forwarding

### Why it matters

NAT allows private IP addresses to access the Internet using public IP addresses.

### Common NAT types

- Static NAT
- Dynamic NAT
- PAT / NAT overload
- Source NAT
- Destination NAT

### Simple example

A private client:

```text
192.168.1.50
```

accesses the Internet through a public IP:

```text
203.0.113.10
```

The firewall translates the source address from private to public.

### Practical note

NAT is very common in IPv4. IPv6 often reduces the need for NAT, although firewalling is still important.

---

## NDP

**Full name:** Neighbor Discovery Protocol

NDP is used in IPv6 for neighbor discovery and related local network functions.

IPv6 does not use ARP.

### Where it is used

- IPv6 networks
- Router discovery
- Neighbor discovery
- Duplicate address detection
- Address resolution

### Why it matters

NDP is essential for IPv6 communication on local networks.

### NDP handles

- Finding neighboring devices
- Resolving IPv6 addresses to MAC addresses
- Discovering routers
- Learning prefixes
- Detecting duplicate addresses

### Practical note

NDP uses ICMPv6. Blocking ICMPv6 carelessly can break IPv6 networks.

---

## NTP

**Full name:** Network Time Protocol

NTP is used to synchronize clocks across devices.

### Where it is used

- Servers
- Routers
- Switches
- Firewalls
- Domain controllers
- Logs and SIEM systems
- Security appliances

### Common port

```text
123/UDP
```

### Why it matters

Correct time is important for:

- Logs
- Authentication
- Certificates
- Kerberos
- Troubleshooting
- Security investigations
- Distributed systems

### Simple example

If firewall logs show one time and server logs show a different time, troubleshooting becomes much harder.

### Practical note

Always configure reliable time sources in production networks.

---

## OSPF

**Full name:** Open Shortest Path First

OSPF is a link-state routing protocol used inside an organization.

### Where it is used

- Enterprise networks
- Campus networks
- Data centers
- Internal routing
- Multi-router environments

### Why it matters

OSPF lets routers automatically learn routes and adapt when network paths change.

### Key ideas

- Areas
- Cost metric
- Link-state database
- Neighbor relationships
- Shortest path calculation

### Simple example

If Router A has a LAN behind it and Router B needs to reach that LAN, OSPF can advertise the route automatically.

### Practical note

OSPF is commonly used for internal routing, while BGP is commonly used between organizations or at Internet edges.

---

## PAT

**Full name:** Port Address Translation

PAT is a type of NAT where many private devices share one public IP address by using different port numbers.

It is also called NAT overload.

### Where it is used

- Home routers
- Office firewalls
- Internet edge devices
- IPv4 networks

### Why it matters

PAT allows many internal devices to access the Internet using one public IPv4 address.

### Simple example

Internal clients:

```text
192.168.1.10
192.168.1.11
192.168.1.12
```

all share:

```text
203.0.113.5
```

The firewall tracks sessions using ports.

### Example translation

```text
192.168.1.10:51510 -> 203.0.113.5:40001
192.168.1.11:51511 -> 203.0.113.5:40002
```

### Practical note

PAT is one reason many devices can share one home Internet public IP address.

---

## PDU

**Full name:** Protocol Data Unit

PDU is a general term for a unit of data at a specific network layer.

### Where it is used

- OSI model
- Protocol analysis
- Packet captures
- Networking theory
- Troubleshooting discussions

### Common PDU names

| Layer | PDU Name |
|---|---|
| Layer 1 | Bits |
| Layer 2 | Frame |
| Layer 3 | Packet |
| Layer 4 | Segment or Datagram |

### Why it matters

PDU names help you speak clearly about where a problem exists.

### Simple example

If you are analyzing Ethernet, you may talk about frames.

If you are analyzing IP routing, you may talk about packets.

If you are analyzing TCP, you may talk about segments.

---

## PoE

**Full name:** Power over Ethernet

PoE allows Ethernet cables to carry both data and electrical power.

### Where it is used

- IP phones
- Wireless access points
- IP cameras
- Door access systems
- Small network devices

### Why it matters

PoE reduces the need for separate power adapters and electrical outlets near devices.

### Common PoE standards

| Standard | Common Name |
|---|---|
| 802.3af | PoE |
| 802.3at | PoE+ |
| 802.3bt | PoE++ |

### Troubleshooting tip

If an access point or IP phone does not power on, check:

- Switch PoE budget
- Port PoE status
- Cable quality
- Device power requirement
- PoE standard support

---

## QoS

**Full name:** Quality of Service

QoS is used to classify, prioritize, shape, or police network traffic.

### Where it is used

- Voice networks
- Video conferencing
- WAN links
- Enterprise networks
- Service provider networks
- Congested links

### Why it matters

Not all traffic has the same needs.

Voice traffic needs low delay and low jitter.

Bulk backup traffic may tolerate delay.

QoS helps protect important real-time traffic when the network is busy.

### Common QoS concepts

- Classification
- Marking
- Queuing
- Shaping
- Policing
- DSCP
- CoS

### Practical note

QoS is most useful during congestion. If there is no congestion, QoS may not visibly change performance.

---

## RADIUS

**Full name:** Remote Authentication Dial-In User Service

RADIUS is used for centralized authentication, authorization, and accounting.

### Where it is used

- VPN login
- Wi-Fi Enterprise authentication
- 802.1X
- Network device admin login
- Remote access systems

### Common ports

```text
1812/UDP  Authentication
1813/UDP  Accounting
```

Older systems may use:

```text
1645/UDP
1646/UDP
```

### Why it matters

RADIUS lets network devices ask a central server whether a user or device should be allowed access.

### Simple example

A user connects to enterprise Wi-Fi.

The access point or controller asks the RADIUS server:

```text
Is this user allowed to connect?
```

The RADIUS server replies with accept or reject.

---

## RSTP

**Full name:** Rapid Spanning Tree Protocol

RSTP is a faster version of STP.

It helps prevent Layer 2 loops in switched networks.

### Where it is used

- Ethernet switching
- Redundant switch links
- Campus LANs
- Access/distribution networks

### Why it matters

Layer 2 loops can cause broadcast storms and network outages.

RSTP blocks redundant paths until they are needed.

### Common RSTP states

- Discarding
- Learning
- Forwarding

### STP vs RSTP

RSTP converges faster than traditional STP.

### Practical note

Even if you design networks carefully, STP/RSTP is still an important safety mechanism.

---

## SFP

**Full name:** Small Form-factor Pluggable

An SFP is a small transceiver module used in network equipment.

It allows switches, routers, and firewalls to connect using fiber or copper modules.

### Where it is used

- Switch uplinks
- Fiber connections
- Router interfaces
- Firewall interfaces
- Data center networks
- ISP handoffs

### Common related modules

- SFP
- SFP+
- SFP28
- QSFP+
- QSFP28

### Why it matters

SFP modules give network devices flexible media options.

For example, the same switch slot may support different transceivers depending on distance and cable type.

### Troubleshooting tip

If a fiber link is down, check:

- SFP compatibility
- Fiber type
- Connector type
- TX/RX polarity
- Optical power levels
- Speed support

---

## SIP

**Full name:** Session Initiation Protocol

SIP is used to set up, manage, and end voice or video sessions.

### Where it is used

- VoIP phones
- PBX systems
- SIP trunks
- Video calling
- Unified communications

### Common ports

```text
5060/TCP or UDP  SIP
5061/TCP         SIP over TLS
```

### Why it matters

SIP handles call signaling. It helps devices agree on how to establish a call.

### SIP vs RTP

| Protocol | Purpose |
|---|---|
| SIP | Call setup and signaling |
| RTP | Carries voice/video media |

### Practical note

If SIP works but there is no audio, the problem may be with RTP media ports, NAT, firewall rules, or codec negotiation.

---

## SLAAC

**Full name:** Stateless Address Autoconfiguration

SLAAC allows IPv6 devices to automatically create their own IPv6 addresses.

### Where it is used

- IPv6 networks
- Home networks
- Enterprise IPv6 networks
- Router advertisement environments

### Why it matters

IPv6 devices can often configure addresses without a traditional DHCP server.

### How it works in simple terms

A router sends Router Advertisements.

The client learns the IPv6 prefix and creates its own address.

### SLAAC vs DHCPv6

| Method | Role |
|---|---|
| SLAAC | Client creates its own IPv6 address |
| DHCPv6 | Server provides address or options |

### Practical note

Many IPv6 networks use SLAAC for addressing and DHCPv6 or router advertisements for additional information such as DNS.

---

## SMB

**Full name:** Server Message Block

SMB is a protocol used for file sharing, printer sharing, and other network resource access.

### Where it is used

- Windows file sharing
- File servers
- Network shares
- Printer sharing
- Active Directory environments
- Some NAS systems

### Common port

```text
445/TCP
```

Older NetBIOS-related SMB may use:

```text
137-139/TCP/UDP
```

### Why it matters

SMB is very common in enterprise networks, especially Windows environments.

### Simple example

A user accesses a file share:

```text
\\fileserver\shared
```

That often uses SMB.

### Security note

SMB should not be exposed directly to the public Internet.

---

## SNMP

**Full name:** Simple Network Management Protocol

SNMP is used to monitor and manage network devices.

### Where it is used

- Switch monitoring
- Router monitoring
- Firewall monitoring
- Printer monitoring
- UPS monitoring
- Network management systems

### Common ports

```text
161/UDP  SNMP queries
162/UDP  SNMP traps
```

### Why it matters

SNMP lets monitoring tools collect information such as:

- Interface traffic
- Interface status
- CPU usage
- Memory usage
- Device temperature
- Error counters

### Versions

- SNMPv1
- SNMPv2c
- SNMPv3

### Security note

SNMPv3 is preferred because it supports stronger authentication and encryption.

---

## SOA

**Full name:** Start of Authority

SOA is a DNS record that contains authority and administrative information for a DNS zone.

### Where it is used

- DNS zones
- Authoritative DNS servers
- Zone transfers
- DNS troubleshooting

### Why it matters

The SOA record tells DNS systems important details about a zone.

### Common SOA fields

- Primary name server
- Responsible contact
- Serial number
- Refresh timer
- Retry timer
- Expire timer
- Negative caching value

### Simple example

A DNS zone may have an SOA serial like:

```text
2026042701
```

This can mean the first DNS zone change made on April 27, 2026.

### Practical note

When DNS changes do not appear on secondary DNS servers, check the SOA serial number.

---

## SSH

**Full name:** Secure Shell

SSH is used for secure remote command-line access.

### Where it is used

- Linux servers
- Unix systems
- Network devices
- Cloud servers
- Git access
- Secure tunneling

### Common port

```text
22/TCP
```

### Why it matters

SSH replaced insecure remote access methods like Telnet in most modern environments.

### Common uses

- Remote administration
- File transfer with SCP/SFTP
- Port forwarding
- Automation
- Git over SSH

### Security note

Protect SSH with:

- Strong authentication
- Key-based login where possible
- MFA where available
- Restricted source IPs
- Updated software
- Logging and monitoring

---

## SSID

**Full name:** Service Set Identifier

SSID is the Wi-Fi network name that users see when they connect to wireless.

### Where it is used

- Wi-Fi networks
- Access points
- Wireless controllers
- Client wireless settings

### Simple example

A Wi-Fi network may appear as:

```text
Company-WiFi
Guest-WiFi
HomeNetwork
```

These names are SSIDs.

### Why it matters

SSID identifies the wireless network, but it is not the same thing as security.

A visible SSID can still be secure if it uses strong encryption and authentication.

### Related terms

- BSSID: Usually the MAC address of the access point radio
- ESSID: Extended service set across multiple APs

### Practical note

Do not rely on hiding the SSID as a real security control. Use WPA2/WPA3 and strong authentication instead.

---

## STP

**Full name:** Spanning Tree Protocol

STP prevents Layer 2 loops in switched Ethernet networks.

### Where it is used

- Ethernet switches
- Redundant links
- Access networks
- Campus networks

### Why it matters

Layer 2 loops can cause broadcast storms, MAC table instability, and severe outages.

STP blocks some links to create a loop-free topology.

### Simple example

If two switches are connected by multiple paths, STP may block one path to prevent a loop.

If the active path fails, STP can unblock a backup path.

### Related term

RSTP is a faster version of STP.

---

## SVI

**Full name:** Switched Virtual Interface

An SVI is a virtual Layer 3 interface for a VLAN on a Layer 3 switch.

### Where it is used

- Layer 3 switches
- Inter-VLAN routing
- VLAN gateways
- Switch management

### Why it matters

SVIs allow a Layer 3 switch to route traffic between VLANs.

### Simple example

```text
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
```

This SVI can act as the default gateway for VLAN 10.

### Practical note

If users in one VLAN cannot reach other VLANs, check whether the SVI is up and routing is enabled.

---

## TCP

**Full name:** Transmission Control Protocol

TCP is a connection-oriented transport protocol.

It is used when reliability matters.

### Where it is used

- Web traffic
- SSH
- Email
- File transfer
- Databases
- Remote desktop
- APIs

### Why it matters

TCP provides reliable delivery, ordering, retransmission, and flow control.

### TCP 3-way handshake

```text
SYN
SYN-ACK
ACK
```

This handshake establishes a TCP connection.

### Common TCP flags

- SYN
- ACK
- FIN
- RST
- PSH
- URG

### TCP vs UDP

| TCP | UDP |
|---|---|
| Reliable | No built-in reliability |
| Connection-oriented | Connectionless |
| Ordered delivery | No guaranteed order |
| More overhead | Lower overhead |

---

## TLS

**Full name:** Transport Layer Security

TLS provides encryption and authentication for network communication.

It is best known as the security layer used by HTTPS.

### Where it is used

- HTTPS
- Secure email
- VPNs
- APIs
- Secure LDAP
- Secure management interfaces
- Modern application security

### Why it matters

TLS helps protect data from being read or modified while in transit.

### Simple example

When you open:

```text
https://example.com
```

TLS is used to secure the connection between your browser and the server.

### TLS vs SSL

SSL is older and obsolete.

People sometimes still say “SSL certificate,” but modern secure connections use TLS.

### Practical note

Certificate problems can break TLS connections even when the network path is working.

---

## UDP

**Full name:** User Datagram Protocol

UDP is a connectionless transport protocol.

It is simple and fast, but it does not provide built-in reliability like TCP.

### Where it is used

- DNS
- DHCP
- NTP
- SNMP
- VoIP media
- Video streaming
- Gaming
- Some VPNs

### Why it matters

UDP is useful when speed, low overhead, or real-time delivery matters.

### Simple comparison

TCP is like a phone call where both sides establish a session.

UDP is more like sending individual messages without setting up a full session first.

### Practical note

Because UDP has no handshake like TCP, troubleshooting UDP services may require packet captures or application-level checks.

---

## ULA

**Full name:** Unique Local Address

ULA is an IPv6 address range for local private use.

The range is:

```text
fc00::/7
```

In practice, many ULA addresses start with:

```text
fd
```

### Where it is used

- Internal IPv6 networks
- Labs
- Private services
- Enterprise internal addressing

### Why it matters

ULA gives IPv6 networks a private addressing option similar in concept to private IPv4 ranges.

### Simple example

```text
fd12:3456:789a::1
```

### Practical note

ULA addresses are not intended to be routed on the public Internet.

---

## VLAN

**Full name:** Virtual Local Area Network

A VLAN is a logical Layer 2 network segment.

It allows one physical switch infrastructure to be divided into multiple separate broadcast domains.

### Where it is used

- Enterprise LANs
- Campus networks
- Data centers
- Voice networks
- Guest networks
- Management networks

### Why it matters

VLANs improve organization, security, and traffic separation.

### Simple example

| VLAN | Purpose | Subnet |
|---:|---|---|
| 10 | Users | `192.168.10.0/24` |
| 20 | Voice | `192.168.20.0/24` |
| 30 | Guests | `192.168.30.0/24` |
| 99 | Management | `192.168.99.0/24` |

### Practical note

Devices in different VLANs need routing to communicate with each other.

---

## VPN

**Full name:** Virtual Private Network

A VPN creates a secure logical connection across another network, often the Internet.

### Where it is used

- Remote work
- Site-to-site connectivity
- Cloud connectivity
- Secure access to internal systems
- Encrypted tunnels

### Common VPN types

- Site-to-site VPN
- Remote access VPN
- IPsec VPN
- SSL VPN
- WireGuard-based VPN
- OpenVPN

### Why it matters

VPNs allow private communication over untrusted networks.

### Simple example

A remote employee connects from home to the company network:

```text
Laptop <--VPN--> Company firewall
```

After connecting, the employee can access internal systems based on policy.

### Practical note

VPN issues often involve routing, DNS, authentication, firewall rules, or split tunneling.

---

## VRRP

**Full name:** Virtual Router Redundancy Protocol

VRRP is an open standard first-hop redundancy protocol.

It provides a virtual default gateway shared by multiple routers or Layer 3 switches.

### Where it is used

- Multi-vendor networks
- Enterprise LANs
- Router redundancy
- Layer 3 switch gateway redundancy

### Why it matters

VRRP helps keep client connectivity working if one gateway fails.

### Simple example

```text
Virtual gateway: 192.168.1.1
Router A:        192.168.1.2
Router B:        192.168.1.3
```

Clients use `192.168.1.1`.

If the active router fails, another router takes over.

### Related term

HSRP is similar but Cisco-specific.

---

## WEP

**Full name:** Wired Equivalent Privacy

WEP is an old Wi-Fi security protocol.

It is considered insecure and should not be used.

### Where it was used

- Old wireless networks
- Legacy devices
- Early Wi-Fi deployments

### Why it matters

You may still see WEP mentioned in older documentation or legacy environments.

### Security status

WEP is broken and can be cracked easily with modern tools.

### Practical advice

Do not use WEP.

Use WPA2 or WPA3 instead.

---

## WPA

**Full name:** Wi-Fi Protected Access

WPA is a family of Wi-Fi security protocols created to improve wireless security after WEP.

### Main versions

- WPA
- WPA2
- WPA3

### Where it is used

- Home Wi-Fi
- Enterprise Wi-Fi
- Guest wireless networks
- Wireless security design

### Why it matters

WPA protects wireless communication with encryption and authentication.

### WPA2

WPA2 is still widely used and commonly supports AES-based encryption.

### WPA3

WPA3 is newer and improves security in several areas, including stronger protection for password-based networks.

### PSK vs Enterprise

| Mode | Meaning |
|---|---|
| WPA-Personal / PSK | Uses a shared Wi-Fi password |
| WPA-Enterprise | Uses 802.1X authentication, usually with RADIUS |

### Practical note

For home and small networks, WPA2/WPA3-Personal may be enough.

For companies, WPA2/WPA3-Enterprise is usually better because it supports per-user authentication.

---

## 4. Quick Review Table

| Term | Full Name | Simple Meaning |
|---|---|---|
| ACL | Access Control List | Rules that allow or deny traffic |
| AF | Assured Forwarding | QoS marking group |
| AH | Authentication Header | IPsec integrity/authentication header |
| APIPA | Automatic Private IP Addressing | IPv4 fallback when DHCP fails |
| ARP | Address Resolution Protocol | Maps IPv4 addresses to MAC addresses |
| AS | Autonomous System | Network group under one routing policy |
| BGP | Border Gateway Protocol | Routing protocol of the Internet |
| CAM | Content Addressable Memory | Switch MAC address table memory |
| CIDR | Classless Inter-Domain Routing | Prefix notation like `/24` |
| CRC | Cyclic Redundancy Check | Error detection method |
| DHCP | Dynamic Host Configuration Protocol | Automatic IP configuration |
| DNS | Domain Name System | Resolves names to IPs and records |
| ESP | Encapsulating Security Payload | IPsec encryption/protection component |
| FHRP | First Hop Redundancy Protocol | Default gateway redundancy family |
| HSRP | Hot Standby Router Protocol | Cisco gateway redundancy protocol |
| ICMP | Internet Control Message Protocol | Error and diagnostic messages |
| IKE | Internet Key Exchange | IPsec key negotiation |
| IPsec | Internet Protocol Security | Secure IP traffic suite |
| LACP | Link Aggregation Control Protocol | Bundles physical links |
| LDAP | Lightweight Directory Access Protocol | Directory lookup protocol |
| LLDP | Link Layer Discovery Protocol | Neighbor discovery between devices |
| MAC | Media Access Control | Layer 2 hardware address |
| MSS | Maximum Segment Size | Largest TCP payload segment |
| MTU | Maximum Transmission Unit | Largest packet size on a link |
| NAT | Network Address Translation | Translates IP addresses |
| NDP | Neighbor Discovery Protocol | IPv6 neighbor discovery |
| NTP | Network Time Protocol | Time synchronization |
| OSPF | Open Shortest Path First | Internal routing protocol |
| PAT | Port Address Translation | Many-to-one NAT using ports |
| PDU | Protocol Data Unit | Data unit at a network layer |
| PoE | Power over Ethernet | Power and data over Ethernet cable |
| QoS | Quality of Service | Traffic prioritization and control |
| RADIUS | Remote Authentication Dial-In User Service | Central authentication protocol |
| RSTP | Rapid Spanning Tree Protocol | Faster loop prevention protocol |
| SFP | Small Form-factor Pluggable | Network transceiver module |
| SIP | Session Initiation Protocol | Voice/video call signaling |
| SLAAC | Stateless Address Autoconfiguration | IPv6 automatic addressing |
| SMB | Server Message Block | File and printer sharing protocol |
| SNMP | Simple Network Management Protocol | Device monitoring protocol |
| SOA | Start of Authority | DNS zone authority record |
| SSH | Secure Shell | Secure remote CLI access |
| SSID | Service Set Identifier | Wi-Fi network name |
| STP | Spanning Tree Protocol | Layer 2 loop prevention |
| SVI | Switched Virtual Interface | VLAN Layer 3 interface |
| TCP | Transmission Control Protocol | Reliable transport protocol |
| TLS | Transport Layer Security | Encryption for network sessions |
| UDP | User Datagram Protocol | Fast connectionless transport |
| ULA | Unique Local Address | Private IPv6 addressing |
| VLAN | Virtual Local Area Network | Logical Layer 2 segment |
| VPN | Virtual Private Network | Secure tunnel over another network |
| VRRP | Virtual Router Redundancy Protocol | Open gateway redundancy protocol |
| WEP | Wired Equivalent Privacy | Old insecure Wi-Fi security |
| WPA | Wi-Fi Protected Access | Modern Wi-Fi security family |

---

## 5. Final Notes

This glossary gives quick explanations of many common networking terms, but each term can go much deeper.

For study and interviews, focus first on understanding these ideas clearly:

- What layer the term belongs to
- What problem it solves
- Where it appears in a real network
- What can go wrong with it
- Which commands or tools help troubleshoot it

The most important practical skill is not memorizing every abbreviation. It is understanding how the terms connect.

For example:

- DHCP gives a client an IP address.
- DNS lets the client resolve names.
- ARP helps the client find the gateway MAC address in IPv4.
- The gateway routes traffic to other networks.
- NAT may translate the traffic before it reaches the Internet.
- ACLs and firewalls may allow or block the traffic.
- TCP or UDP carries the application data.
- TLS may encrypt the session.

When you understand those connections, networking becomes much easier to troubleshoot.
