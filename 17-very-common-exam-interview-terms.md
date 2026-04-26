# 17. Very Common Exam / Interview Terms

Networking interviews and certification exams often repeat the same core terms. The words may look simple, but interviewers usually want to know if you understand the idea behind the term, not just the definition.

This guide explains common networking exam and interview terms in a practical way. Each topic includes a clear meaning, why it matters, and what kind of answer is usually expected in an interview.

---

## Table of Contents

- [1. How to Use This Guide](#1-how-to-use-this-guide)
- [2. Quick Interview Advice](#2-quick-interview-advice)
- [3. Layer 2 and Switching Terms](#3-layer-2-and-switching-terms)
  - [Broadcast Domain](#broadcast-domain)
  - [Collision Domain](#collision-domain)
  - [MAC Address](#mac-address)
  - [ARP](#arp)
  - [VLAN](#vlan)
  - [Access Port](#access-port)
  - [Trunk Port](#trunk-port)
  - [Native VLAN](#native-vlan)
  - [STP / RSTP](#stp--rstp)
  - [SVI](#svi)
- [4. IPv4 and Subnetting Terms](#4-ipv4-and-subnetting-terms)
  - [CIDR](#cidr)
  - [Subnet Mask](#subnet-mask)
  - [Wildcard Mask](#wildcard-mask)
  - [Default Gateway](#default-gateway)
  - [Network Address](#network-address)
  - [Broadcast Address](#broadcast-address)
  - [Usable Host Range](#usable-host-range)
  - [Private IP Address](#private-ip-address)
  - [Public IP Address](#public-ip-address)
  - [APIPA](#apipa)
- [5. IPv6 Terms](#5-ipv6-terms)
  - [IPv6](#ipv6)
  - [SLAAC](#slaac)
  - [DHCPv6](#dhcpv6)
  - [NDP](#ndp)
  - [Link-Local Address](#link-local-address)
  - [ULA](#ula)
- [6. Routing Terms](#6-routing-terms)
  - [Routing](#routing)
  - [Route Table](#route-table)
  - [Static Route](#static-route)
  - [Default Route](#default-route)
  - [Longest Prefix Match](#longest-prefix-match)
  - [Next Hop](#next-hop)
  - [Metric](#metric)
  - [Administrative Distance](#administrative-distance)
  - [OSPF](#ospf)
  - [BGP](#bgp)
  - [HSRP / VRRP](#hsrp--vrrp)
- [7. Transport Layer Terms](#7-transport-layer-terms)
  - [TCP](#tcp)
  - [UDP](#udp)
  - [TCP vs UDP](#tcp-vs-udp)
  - [Port Number](#port-number)
  - [TCP 3-Way Handshake](#tcp-3-way-handshake)
  - [MTU](#mtu)
  - [MSS](#mss)
- [8. Name Resolution and Address Assignment Terms](#8-name-resolution-and-address-assignment-terms)
  - [DNS](#dns)
  - [A Record](#a-record)
  - [AAAA Record](#aaaa-record)
  - [CNAME Record](#cname-record)
  - [MX Record](#mx-record)
  - [TXT Record](#txt-record)
  - [DHCP](#dhcp)
  - [DHCP DORA](#dhcp-dora)
- [9. Security, NAT, Firewall, and VPN Terms](#9-security-nat-firewall-and-vpn-terms)
  - [NAT](#nat)
  - [PAT](#pat)
  - [ACL](#acl)
  - [Firewall](#firewall)
  - [Stateful Firewall](#stateful-firewall)
  - [VPN](#vpn)
  - [IPsec](#ipsec)
  - [Load Balancer](#load-balancer)
  - [Reverse Proxy](#reverse-proxy)
- [10. Wireless Terms](#10-wireless-terms)
  - [SSID](#ssid)
  - [BSSID](#bssid)
  - [WPA2 / WPA3](#wpa2--wpa3)
  - [802.1X](#8021x)
  - [RSSI](#rssi)
- [11. QoS and Performance Terms](#11-qos-and-performance-terms)
  - [QoS](#qos)
  - [Latency](#latency)
  - [Jitter](#jitter)
  - [Packet Loss](#packet-loss)
  - [Bandwidth](#bandwidth)
  - [Throughput](#throughput)
- [12. Troubleshooting Terms](#12-troubleshooting-terms)
  - [Ping](#ping)
  - [Traceroute / Tracert](#traceroute--tracert)
  - [Packet Capture](#packet-capture)
  - [DNS Resolution Failure](#dns-resolution-failure)
  - [Asymmetric Routing](#asymmetric-routing)
- [13. Quick Interview Questions and Answers](#13-quick-interview-questions-and-answers)
- [14. Final Cheat Sheet](#14-final-cheat-sheet)

---

## 1. How to Use This Guide

Use this file as a revision guide before exams, interviews, labs, or troubleshooting practice.

For each term, try to answer four questions:

1. **What does it mean?**
2. **Which layer does it belong to?**
3. **Why does it matter?**
4. **What can go wrong with it?**

If you can answer those four questions clearly, you probably understand the term well enough for most beginner and intermediate networking interviews.

---

## 2. Quick Interview Advice

In networking interviews, a good answer is usually short, clear, and practical.

A weak answer sounds like this:

```text
A VLAN is a virtual LAN.
```

That is technically true, but it is too shallow.

A better answer sounds like this:

```text
A VLAN is a logical Layer 2 network. It separates devices into different broadcast domains even if they are connected to the same physical switch. VLANs are commonly used to separate users, servers, voice, guests, and management traffic.
```

That answer shows understanding.

### Good interview answer structure

Use this simple structure:

```text
Definition -> Purpose -> Example -> Common issue
```

Example for DNS:

```text
DNS translates names into IP addresses. It lets users access services using names like example.com instead of remembering IP addresses. For example, an A record maps a hostname to an IPv4 address. A common issue is when DNS fails, the network may work by IP but not by name.
```

---

# 3. Layer 2 and Switching Terms

Layer 2 terms are very common in interviews because they explain how local networks work.

Layer 2 is mainly about:

- Ethernet
- MAC addresses
- Switching
- VLANs
- Trunks
- STP
- Broadcast domains

---

## Broadcast Domain

A **broadcast domain** is the group of devices that receive the same Layer 2 broadcast traffic.

In IPv4, a broadcast frame may be sent to all devices in the same subnet or VLAN.

### Simple explanation

If one device sends a broadcast, every device in the same broadcast domain can receive it.

### What creates a broadcast domain?

Usually, a VLAN creates a broadcast domain.

Example:

```text
VLAN 10 = one broadcast domain
VLAN 20 = another broadcast domain
```

Devices in VLAN 10 do not receive broadcasts from VLAN 20.

### Why it matters

Broadcast domains matter because too many devices in one broadcast domain can create unnecessary traffic and make troubleshooting harder.

### Interview answer

```text
A broadcast domain is the area of a network where a broadcast frame can reach. VLANs usually define broadcast domains, and routers or Layer 3 interfaces separate them.
```

### Common issue

If a device is in the wrong VLAN, it may be in the wrong broadcast domain and fail to get DHCP or reach local services.

---

## Collision Domain

A **collision domain** is a network segment where packet collisions can occur.

This term was more important in older Ethernet networks that used hubs and half-duplex communication.

### Simple explanation

A collision happens when two devices transmit at the same time on shared media.

### Modern switching

On modern switches, each switch port is usually its own collision domain.

With full-duplex Ethernet, collisions are normally not expected.

### Why it matters

Collision domains are still useful for understanding older networks and Ethernet basics.

### Interview answer

```text
A collision domain is an area where Ethernet collisions can happen. With hubs, many devices share one collision domain. With switches, each port is usually its own collision domain, and full-duplex links should not have collisions.
```

### Common issue

If you see collisions on a modern full-duplex switch port, check for duplex mismatch or old/incorrect equipment.

---

## MAC Address

A **MAC address** is a Layer 2 hardware address used by Ethernet.

Example:

```text
00:1A:2B:3C:4D:5E
```

### Simple explanation

A MAC address identifies a network interface on the local network.

### Why it matters

Switches use MAC addresses to forward frames.

A switch learns which MAC addresses are reachable on which ports and stores that information in a MAC address table.

### Interview answer

```text
A MAC address is a Layer 2 address used for local Ethernet communication. Switches learn MAC addresses and use them to forward frames inside a LAN.
```

### Common issue

If a switch sees the same MAC address moving between ports quickly, it may indicate a loop or misconfiguration. This is often called MAC flapping.

---

## ARP

ARP stands for **Address Resolution Protocol**.

It is used in IPv4 to map an IP address to a MAC address.

### Simple explanation

A device knows the destination IP address, but it needs the destination MAC address to send a frame on the local network.

ARP answers this question:

```text
Who has this IP address?
```

### Example

A computer wants to reach its gateway:

```text
Gateway IP: 192.168.1.1
```

The computer sends an ARP request:

```text
Who has 192.168.1.1?
```

The router replies with its MAC address.

### Why it matters

If ARP fails, local communication fails.

### Interview answer

```text
ARP maps IPv4 addresses to MAC addresses on a local network. A host uses ARP when it needs to send traffic to another local host or to its default gateway.
```

### Common issue

ARP problems can happen because of duplicate IP addresses, VLAN issues, security features, or ARP spoofing.

---

## VLAN

VLAN stands for **Virtual Local Area Network**.

A VLAN is a logical Layer 2 network.

### Simple explanation

VLANs let you separate devices into different logical networks even if they use the same physical switch.

### Example

```text
VLAN 10 = Users
VLAN 20 = Servers
VLAN 30 = Voice
VLAN 40 = Guests
```

Each VLAN is usually its own broadcast domain and often has its own IP subnet.

### Why it matters

VLANs improve:

- Segmentation
- Security
- Organization
- Broadcast control
- Network design

### Interview answer

```text
A VLAN is a logical Layer 2 segment. It separates devices into different broadcast domains and is commonly used to separate users, servers, voice, guests, and management networks.
```

### Common issue

If a device is assigned to the wrong VLAN, it may get the wrong IP address or lose access to expected resources.

---

## Access Port

An **access port** is a switch port that belongs to one VLAN.

### Simple explanation

Access ports are usually used for end devices.

Examples:

- PC
- Printer
- IP phone
- Camera
- Access point in some designs

### Example

```text
Switch port Fa0/10
Mode: access
VLAN: 20
```

A device connected to that port is placed into VLAN 20.

### Interview answer

```text
An access port carries traffic for one VLAN and is usually used to connect end-user devices such as PCs or printers.
```

### Common issue

If an access port is configured for the wrong VLAN, the connected device may receive the wrong DHCP scope or fail to reach expected systems.

---

## Trunk Port

A **trunk port** carries traffic for multiple VLANs.

### Simple explanation

Trunks are used between network devices.

Examples:

- Switch to switch
- Switch to router
- Switch to firewall
- Switch to wireless access point
- Switch to hypervisor host

### 802.1Q tagging

Trunk ports usually use 802.1Q tags to identify which VLAN each frame belongs to.

### Interview answer

```text
A trunk port carries multiple VLANs across one physical link. It usually uses 802.1Q tagging so the receiving device knows which VLAN each frame belongs to.
```

### Common issue

If a VLAN is not allowed on a trunk, devices in that VLAN may fail to communicate across switches.

---

## Native VLAN

The **native VLAN** is the VLAN that is sent untagged on an 802.1Q trunk.

### Simple explanation

Most VLANs on a trunk are tagged, but the native VLAN is usually untagged.

### Why it matters

Native VLAN mismatches can cause confusing Layer 2 issues.

### Interview answer

```text
The native VLAN is the untagged VLAN on an 802.1Q trunk. Both sides of a trunk should normally agree on the native VLAN to avoid misconfiguration and security issues.
```

### Best practice

In many enterprise networks, the native VLAN is changed from VLAN 1 to an unused VLAN.

---

## STP / RSTP

STP stands for **Spanning Tree Protocol**.

RSTP stands for **Rapid Spanning Tree Protocol**.

They prevent Layer 2 switching loops.

### Why loops are dangerous

A Layer 2 loop can cause:

- Broadcast storms
- MAC address flapping
- High CPU usage
- Network outage
- Unstable forwarding

### Simple explanation

STP blocks some redundant paths so there is no loop.

If the active path fails, STP can unblock another path.

### Interview answer

```text
STP prevents Layer 2 loops by blocking redundant paths. RSTP is a faster version of STP that converges more quickly after topology changes.
```

### Common issue

If STP is disabled or misconfigured, a simple cabling mistake can cause a major network outage.

---

## SVI

SVI stands for **Switched Virtual Interface**.

It is a Layer 3 interface for a VLAN on a switch.

### Simple explanation

An SVI lets a Layer 3 switch route traffic for a VLAN.

### Example

```text
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
```

This interface can act as the default gateway for VLAN 10.

### Interview answer

```text
An SVI is a virtual Layer 3 interface for a VLAN. It is commonly used on Layer 3 switches to provide default gateway and inter-VLAN routing.
```

### Common issue

If an SVI is down, the VLAN may not have a working gateway on that switch.

---

# 4. IPv4 and Subnetting Terms

IPv4 and subnetting terms are extremely common in exams and interviews.

You should be able to explain them clearly and do simple subnet calculations.

---

## CIDR

CIDR stands for **Classless Inter-Domain Routing**.

It uses prefix length notation like:

```text
192.168.1.0/24
```

### Simple explanation

The `/24` tells you how many bits are used for the network part.

### Example

```text
192.168.1.0/24
```

means:

```text
Subnet mask: 255.255.255.0
Total addresses: 256
Usable hosts: 254
```

### Why it matters

CIDR replaced older classful addressing and allows more flexible subnetting.

### Interview answer

```text
CIDR is a way to write IP networks using a prefix length, such as /24. It tells how many bits identify the network portion of the address.
```

---

## Subnet Mask

A **subnet mask** separates the network part from the host part of an IPv4 address.

### Example

```text
IP address: 192.168.1.10
Subnet mask: 255.255.255.0
```

This means the network is:

```text
192.168.1.0/24
```

### Why it matters

A host uses the subnet mask to decide whether a destination is local or remote.

### Interview answer

```text
A subnet mask defines which part of an IPv4 address is the network portion and which part is the host portion.
```

### Common issue

If the subnet mask is wrong, a device may think remote devices are local or local devices are remote.

---

## Wildcard Mask

A **wildcard mask** is often used in Cisco ACLs and routing configurations.

It is the inverse of a subnet mask.

### Example

Subnet mask:

```text
255.255.255.0
```

Wildcard mask:

```text
0.0.0.255
```

### Simple explanation

In wildcard masks:

- `0` means “match exactly”
- `255` means “ignore this part”

### Interview answer

```text
A wildcard mask is the inverse of a subnet mask. It is commonly used in Cisco ACLs and routing protocols to define address ranges.
```

### Common example

```text
192.168.1.0 0.0.0.255
```

matches:

```text
192.168.1.0 - 192.168.1.255
```

---

## Default Gateway

A **default gateway** is the router a device uses to reach networks outside its local subnet.

### Example

Client:

```text
IP: 192.168.1.50
Mask: 255.255.255.0
Gateway: 192.168.1.1
```

If the client wants to reach `8.8.8.8`, it sends the traffic to `192.168.1.1`.

### Interview answer

```text
A default gateway is the router a host sends traffic to when the destination is outside the local subnet.
```

### Common issue

If the gateway is missing or wrong, the host may communicate locally but not reach the Internet or other networks.

---

## Network Address

The **network address** identifies the subnet itself.

### Example

For:

```text
192.168.1.10/24
```

the network address is:

```text
192.168.1.0
```

### Interview answer

```text
The network address is the first address in a subnet and identifies the subnet itself.
```

### Common note

The network address is usually not assigned to a normal host.

---

## Broadcast Address

The **broadcast address** is the last address in an IPv4 subnet.

### Example

For:

```text
192.168.1.0/24
```

the broadcast address is:

```text
192.168.1.255
```

### Interview answer

```text
The broadcast address is the last address in an IPv4 subnet and is used to send traffic to all hosts in that subnet.
```

### Important note

IPv6 does not use broadcast.

---

## Usable Host Range

The **usable host range** is the range of IP addresses that can normally be assigned to devices.

### Example

For:

```text
192.168.1.0/24
```

the usable range is:

```text
192.168.1.1 - 192.168.1.254
```

Because:

```text
192.168.1.0   = network address
192.168.1.255 = broadcast address
```

### Interview answer

```text
The usable host range excludes the network address and broadcast address in normal IPv4 subnets.
```

---

## Private IP Address

Private IP addresses are used inside internal networks.

### Private IPv4 ranges

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

### Why they matter

Private addresses are not routed on the public Internet.

They are commonly used with NAT.

### Interview answer

```text
Private IP addresses are reserved for internal networks and are not routed publicly on the Internet. Common ranges are 10/8, 172.16/12, and 192.168/16.
```

---

## Public IP Address

A **public IP address** is globally routable on the Internet.

### Simple explanation

Public IPs are used for devices or services that need to be reachable across the Internet.

### Interview answer

```text
A public IP address is globally unique and routable on the Internet. It is usually assigned by an ISP or cloud provider.
```

### Common issue

Public IPs should be protected with firewalls and good security controls.

---

## APIPA

APIPA stands for **Automatic Private IP Addressing**.

It uses the IPv4 range:

```text
169.254.0.0/16
```

### Simple explanation

If a Windows client cannot get an IP address from DHCP, it may assign itself a `169.254.x.x` address.

### Interview answer

```text
APIPA is a link-local IPv4 address range used when a client fails to get an address from DHCP. Seeing 169.254.x.x usually means DHCP failed.
```

### Common troubleshooting clue

If a client has an APIPA address, check:

- DHCP server
- DHCP relay
- VLAN
- Switch port
- Wi-Fi connection
- Scope exhaustion

---

# 5. IPv6 Terms

IPv6 is common in exams because it is different from IPv4 in several important ways.

---

## IPv6

IPv6 is the newer version of IP.

It uses 128-bit addresses.

### Example

```text
2001:db8::1
```

### Key differences from IPv4

- 128-bit addresses
- Hexadecimal notation
- No broadcast
- Uses NDP instead of ARP
- Uses multicast heavily
- Common subnet size is `/64`

### Interview answer

```text
IPv6 is the newer IP protocol designed to provide a much larger address space than IPv4. It uses 128-bit addresses and does not use broadcast.
```

---

## SLAAC

SLAAC stands for **Stateless Address Autoconfiguration**.

It allows IPv6 hosts to create their own addresses using router advertisements.

### Interview answer

```text
SLAAC lets an IPv6 host automatically configure its own address using information advertised by a router.
```

### Common note

SLAAC can work without a DHCPv6 server.

---

## DHCPv6

DHCPv6 is DHCP for IPv6.

It can provide IPv6 addresses and other configuration options.

### Interview answer

```text
DHCPv6 provides IPv6 configuration to clients. Depending on the network design, it may assign addresses or only provide options like DNS servers.
```

### Common note

IPv6 networks may use SLAAC, DHCPv6, both, or static addressing.

---

## NDP

NDP stands for **Neighbor Discovery Protocol**.

It is used in IPv6 for neighbor discovery and address resolution.

### Simple explanation

NDP is often compared to ARP, but it does more than ARP.

### Interview answer

```text
NDP is an IPv6 protocol that handles neighbor discovery, address resolution, router discovery, and duplicate address detection. It replaces ARP in IPv6.
```

---

## Link-Local Address

A link-local IPv6 address is used only on the local network link.

The common range is:

```text
fe80::/10
```

### Interview answer

```text
A link-local address is an IPv6 address used for communication on the local link only. It is not routed across the Internet.
```

### Common use

Link-local addresses are used for:

- NDP
- Router discovery
- Routing protocol next hops
- Local communication

---

## ULA

ULA stands for **Unique Local Address**.

The range is:

```text
fc00::/7
```

In practice, many ULA addresses start with:

```text
fd
```

### Interview answer

```text
A ULA is an IPv6 address intended for local private use, similar in purpose to private IPv4 addressing, but designed differently.
```

---

# 6. Routing Terms

Routing terms explain how traffic moves between networks.

---

## Routing

Routing is the process of forwarding packets between different networks.

### Simple explanation

Switching moves frames inside a local network.

Routing moves packets between networks.

### Interview answer

```text
Routing is the process of choosing a path and forwarding packets between different IP networks.
```

---

## Route Table

A route table contains the routes a device uses to forward traffic.

### Common route information

A route usually includes:

- Destination network
- Prefix length
- Next hop
- Exit interface
- Metric
- Route source

### Interview answer

```text
A route table stores the known paths to networks. Routers use it to decide where to forward packets.
```

---

## Static Route

A static route is manually configured by an administrator.

### Example

```text
ip route 10.20.0.0 255.255.0.0 192.168.1.1
```

### Interview answer

```text
A static route is a manually configured route. It is simple and predictable, but it does not automatically adapt to network changes unless tracking or automation is used.
```

---

## Default Route

A default route is used when no more specific route exists.

### IPv4 default route

```text
0.0.0.0/0
```

### IPv6 default route

```text
::/0
```

### Interview answer

```text
A default route is the route used when there is no more specific route in the routing table.
```

---

## Longest Prefix Match

Longest prefix match means the most specific route wins.

### Example

Routes:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
```

Destination:

```text
10.10.10.50
```

The router chooses:

```text
10.10.10.0/24
```

because it is the most specific match.

### Interview answer

```text
Longest prefix match means a router chooses the most specific matching route for a destination.
```

---

## Next Hop

The next hop is the next router or gateway where traffic should be sent.

### Example

```text
Destination: 10.20.0.0/16
Next hop: 192.168.1.1
```

### Interview answer

```text
The next hop is the next Layer 3 device a packet is sent to on its way to the destination network.
```

---

## Metric

A metric is a value used by a routing protocol to choose the best path.

### Examples

Different protocols use different metrics:

- RIP uses hop count
- OSPF uses cost
- EIGRP uses a composite metric
- BGP uses path attributes

### Interview answer

```text
A metric is a value used by routing protocols to compare paths and choose the best route.
```

---

## Administrative Distance

Administrative Distance, or AD, is used to choose between routes learned from different sources.

### Simple explanation

If a router learns the same destination from multiple routing sources, it uses administrative distance to decide which source is more trusted.

Lower AD is preferred.

### Common Cisco AD values

| Route Source | AD |
|---|---:|
| Connected | `0` |
| Static | `1` |
| EIGRP internal | `90` |
| OSPF | `110` |
| RIP | `120` |

### Interview answer

```text
Administrative Distance is a trust value used to choose between routes learned from different sources. Lower AD is preferred.
```

---

## OSPF

OSPF stands for **Open Shortest Path First**.

It is a link-state interior routing protocol.

### Common use

OSPF is often used inside enterprise networks.

### Interview answer

```text
OSPF is a link-state dynamic routing protocol used inside an organization. It uses areas and calculates best paths based on cost.
```

### Important terms

- Area
- Neighbor
- LSDB
- Cost
- Router ID
- DR/BDR
- SPF calculation

---

## BGP

BGP stands for **Border Gateway Protocol**.

It is a path-vector routing protocol.

### Common use

BGP is used for Internet routing and large-scale routing between autonomous systems.

### Interview answer

```text
BGP is a path-vector routing protocol used to exchange routes between autonomous systems. It is the main routing protocol of the Internet.
```

### Important terms

- AS number
- eBGP
- iBGP
- Prefix
- AS path
- Local preference
- MED
- Communities

---

## HSRP / VRRP

HSRP and VRRP are first-hop redundancy protocols.

They provide a redundant default gateway for hosts.

### Simple explanation

Two routers share a virtual gateway IP address.

If one router fails, another router takes over.

### Interview answer

```text
HSRP and VRRP provide default gateway redundancy. Hosts use a virtual gateway IP, and if the active router fails, another router takes over.
```

### Difference

- HSRP is Cisco proprietary
- VRRP is an open standard

---

# 7. Transport Layer Terms

Transport layer terms explain how applications communicate across the network.

---

## TCP

TCP stands for **Transmission Control Protocol**.

It is reliable and connection-oriented.

### TCP provides

- Connection setup
- Ordered delivery
- Retransmission
- Flow control
- Error recovery

### Interview answer

```text
TCP is a reliable, connection-oriented transport protocol. It uses acknowledgments and retransmissions to make sure data is delivered correctly and in order.
```

### Common uses

- HTTP/HTTPS
- SSH
- SMTP
- IMAP
- RDP
- SMB
- Databases

---

## UDP

UDP stands for **User Datagram Protocol**.

It is connectionless and lightweight.

### UDP does not provide

- Connection setup
- Guaranteed delivery
- Ordered delivery
- Retransmission by itself

### Interview answer

```text
UDP is a connectionless transport protocol. It has less overhead than TCP but does not guarantee delivery or ordering by itself.
```

### Common uses

- DNS
- DHCP
- NTP
- VoIP media
- Streaming
- Gaming
- Some VPNs

---

## TCP vs UDP

This is one of the most common interview questions.

### Simple comparison

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Reliability | Built in | Not built in |
| Ordering | Yes | No |
| Overhead | Higher | Lower |
| Speed/latency | Usually heavier | Usually lighter |
| Common use | Web, SSH, email | DNS, voice, video, DHCP |

### Interview answer

```text
TCP is reliable and connection-oriented, while UDP is connectionless and has lower overhead. TCP is used when delivery and order matter. UDP is used when speed, simplicity, or real-time behavior is more important.
```

---

## Port Number

A port number identifies a service or application on a host.

### Example

```text
192.0.2.10:443
```

means host `192.0.2.10`, service port `443`.

### Interview answer

```text
A port number identifies a specific service or application on a device. For example, HTTPS commonly uses TCP port 443.
```

---

## TCP 3-Way Handshake

The TCP 3-way handshake establishes a TCP connection.

### Steps

```text
SYN
SYN-ACK
ACK
```

### Interview answer

```text
The TCP 3-way handshake establishes a TCP connection. The client sends SYN, the server replies with SYN-ACK, and the client responds with ACK.
```

### Common issue

If SYN packets go out but no SYN-ACK comes back, the server may be down, the port may be closed, or a firewall may be blocking traffic.

---

## MTU

MTU stands for **Maximum Transmission Unit**.

It is the largest packet size that can be sent on a link without fragmentation.

### Common Ethernet MTU

```text
1500 bytes
```

### Interview answer

```text
MTU is the maximum packet size that can be transmitted on a link. Ethernet commonly uses an MTU of 1500 bytes.
```

### Common issue

MTU problems can cause strange behavior, especially with VPNs, tunnels, and large packets.

---

## MSS

MSS stands for **Maximum Segment Size**.

It is the largest TCP payload size inside a packet.

### Common value

For Ethernet MTU 1500:

```text
TCP MSS is commonly 1460 bytes for IPv4
```

because:

```text
1500 - 20 byte IPv4 header - 20 byte TCP header = 1460
```

### Interview answer

```text
MSS is the maximum TCP segment payload size. It is related to MTU but applies to TCP data.
```

---

# 8. Name Resolution and Address Assignment Terms

These terms are important because many network problems involve DNS or DHCP.

---

## DNS

DNS stands for **Domain Name System**.

It translates names to IP addresses and other records.

### Interview answer

```text
DNS resolves human-readable names like example.com into IP addresses and other records used by network services.
```

### Common issue

If DNS fails, users may not access services by name even if IP connectivity still works.

---

## A Record

An A record maps a name to an IPv4 address.

### Example

```text
www.example.com -> 192.0.2.10
```

### Interview answer

```text
An A record maps a hostname to an IPv4 address.
```

---

## AAAA Record

An AAAA record maps a name to an IPv6 address.

### Example

```text
www.example.com -> 2001:db8::10
```

### Interview answer

```text
An AAAA record maps a hostname to an IPv6 address.
```

---

## CNAME Record

A CNAME record creates an alias from one name to another.

### Example

```text
blog.example.com -> example-hosting-provider.net
```

### Interview answer

```text
A CNAME record points one DNS name to another DNS name. It is commonly used for aliases and third-party services.
```

---

## MX Record

An MX record identifies mail servers for a domain.

### Example

```text
example.com MX 10 mail.example.com
```

### Interview answer

```text
An MX record tells mail systems which mail servers receive email for a domain.
```

---

## TXT Record

A TXT record stores text information in DNS.

### Common uses

- SPF
- DKIM
- DMARC
- Domain verification
- Security policies

### Interview answer

```text
A TXT record stores text data in DNS and is commonly used for email security records and domain verification.
```

---

## DHCP

DHCP stands for **Dynamic Host Configuration Protocol**.

It automatically assigns IP configuration to clients.

### Interview answer

```text
DHCP automatically provides network settings such as IP address, subnet mask, default gateway, DNS servers, and lease time.
```

---

## DHCP DORA

DORA is the classic DHCP process.

### Steps

```text
Discover
Offer
Request
Acknowledge
```

### Interview answer

```text
DORA is the DHCP process where a client discovers DHCP servers, receives an offer, requests an address, and receives an acknowledgment.
```

---

# 9. Security, NAT, Firewall, and VPN Terms

These terms are common in both networking and cybersecurity interviews.

---

## NAT

NAT stands for **Network Address Translation**.

It changes IP address information in packets.

### Common use

NAT is commonly used to let private IP addresses access the Internet through a public IP address.

### Interview answer

```text
NAT translates IP addresses, commonly private internal addresses to public addresses for Internet access.
```

---

## PAT

PAT stands for **Port Address Translation**.

It is a form of NAT where many private hosts share one public IP address by using different port numbers.

### Interview answer

```text
PAT allows many internal devices to share one public IP address by translating both IP addresses and port numbers.
```

### Common name

PAT is often called NAT overload.

---

## ACL

ACL stands for **Access Control List**.

It is a set of rules that permit or deny traffic.

### Simple explanation

ACLs can match traffic based on:

- Source IP
- Destination IP
- Protocol
- Port
- Direction

### Interview answer

```text
An ACL is a rule list used to permit or deny traffic based on criteria like source, destination, protocol, and port.
```

### Common issue

ACL order matters. Rules are checked from top to bottom.

---

## Firewall

A firewall controls traffic between networks or hosts based on security policy.

### Interview answer

```text
A firewall filters traffic based on rules and security policies. It can control which traffic is allowed or denied between networks.
```

### Common types

- Host firewall
- Network firewall
- Stateful firewall
- Next-generation firewall
- Cloud security group

---

## Stateful Firewall

A stateful firewall tracks connection state.

### Simple explanation

If an internal client opens a connection to a web server, the firewall remembers that session and allows the return traffic.

### Interview answer

```text
A stateful firewall tracks active connections and allows return traffic for sessions that were already permitted.
```

---

## VPN

VPN stands for **Virtual Private Network**.

It creates an encrypted tunnel over another network.

### Common types

- Remote access VPN
- Site-to-site VPN
- SSL VPN
- IPsec VPN

### Interview answer

```text
A VPN creates a secure tunnel over an untrusted network, commonly used for remote access or connecting sites together.
```

---

## IPsec

IPsec is a suite of protocols used to secure IP traffic.

### Common IPsec terms

- IKE
- ESP
- AH
- Security Association
- NAT-T

### Interview answer

```text
IPsec secures IP traffic using encryption and authentication. It is commonly used for site-to-site and remote access VPNs.
```

---

## Load Balancer

A load balancer distributes traffic across multiple backend servers.

### Why it matters

Load balancers improve:

- Availability
- Scalability
- Maintenance flexibility
- Performance distribution

### Interview answer

```text
A load balancer distributes client traffic across multiple servers to improve availability and scalability.
```

---

## Reverse Proxy

A reverse proxy sits in front of servers and handles client requests on their behalf.

### Common uses

- TLS termination
- Web routing
- Security filtering
- Caching
- Hiding backend servers
- Central access control

### Interview answer

```text
A reverse proxy receives client requests and forwards them to backend servers. It is commonly used for web applications, TLS termination, and traffic control.
```

---

# 10. Wireless Terms

Wireless terms are common because Wi-Fi problems are very common in real networks.

---

## SSID

SSID stands for **Service Set Identifier**.

It is the Wi-Fi network name users see.

### Example

```text
Company-WiFi
Guest-WiFi
```

### Interview answer

```text
An SSID is the wireless network name that clients see and connect to.
```

---

## BSSID

BSSID is usually the MAC address of the wireless access point radio serving a specific SSID.

### Interview answer

```text
A BSSID identifies a specific access point radio for a wireless network. It is usually based on the AP radio MAC address.
```

### Why it matters

Multiple access points can broadcast the same SSID, but each AP radio has a different BSSID.

---

## WPA2 / WPA3

WPA2 and WPA3 are Wi-Fi security standards.

### WPA2

WPA2 is widely used and much stronger than older WEP or WPA.

### WPA3

WPA3 is newer and improves security.

### Interview answer

```text
WPA2 and WPA3 are wireless security standards used to protect Wi-Fi networks. WPA3 is newer and provides stronger protections than WPA2.
```

---

## 802.1X

802.1X is a network access control standard.

It is often used with enterprise Wi-Fi.

### Components

- Supplicant: client device
- Authenticator: switch or access point
- Authentication server: usually RADIUS

### Interview answer

```text
802.1X is used for port-based or wireless network access control. It commonly uses RADIUS to authenticate users or devices.
```

---

## RSSI

RSSI stands for **Received Signal Strength Indicator**.

It measures received wireless signal strength.

### Interview answer

```text
RSSI is a measurement of wireless signal strength. It helps determine how strong a client's Wi-Fi signal is.
```

### Important note

Signal strength alone is not enough. Noise, interference, SNR, and capacity also matter.

---

# 11. QoS and Performance Terms

Performance terms help explain why a network may feel slow or unstable.

---

## QoS

QoS stands for **Quality of Service**.

It is used to classify, mark, prioritize, or shape traffic.

### Common use

QoS is often used to prioritize voice, video, and critical applications.

### Interview answer

```text
QoS is used to manage and prioritize network traffic so important or delay-sensitive traffic gets better treatment during congestion.
```

---

## Latency

Latency is delay.

It is the time it takes for traffic to travel from source to destination.

### Interview answer

```text
Latency is the delay in network communication, often measured in milliseconds.
```

### Common causes

- Long distance
- Congestion
- Slow links
- Queuing
- Firewall inspection
- Wireless issues

---

## Jitter

Jitter is variation in delay.

### Why it matters

Jitter is especially bad for voice and video.

### Interview answer

```text
Jitter is variation in packet delay. It can cause poor voice and video quality.
```

---

## Packet Loss

Packet loss happens when packets do not reach their destination.

### Interview answer

```text
Packet loss means packets are dropped or fail to arrive. It can cause retransmissions, slow performance, and poor real-time communication.
```

### Common causes

- Congestion
- Bad cable
- Wireless interference
- Interface errors
- Firewall drops
- Overloaded devices

---

## Bandwidth

Bandwidth is the maximum capacity of a link.

### Example

```text
1 Gbps link
```

### Interview answer

```text
Bandwidth is the theoretical or configured capacity of a network link.
```

---

## Throughput

Throughput is the actual amount of data successfully transferred.

### Simple difference

Bandwidth is capacity.

Throughput is real achieved performance.

### Interview answer

```text
Throughput is the actual data transfer rate achieved across a network, which may be lower than the link bandwidth.
```

---

# 12. Troubleshooting Terms

These terms are useful in practical interviews and real troubleshooting.

---

## Ping

Ping uses ICMP echo request and echo reply to test reachability.

### Interview answer

```text
Ping tests basic IP reachability by sending ICMP echo requests and waiting for replies.
```

### Important note

A failed ping does not always mean the host is down. ICMP may be blocked.

---

## Traceroute / Tracert

Traceroute shows the path packets take to a destination.

### Names

- `traceroute` on Linux/macOS
- `tracert` on Windows

### Interview answer

```text
Traceroute shows the Layer 3 path to a destination by displaying each router hop along the way.
```

### Common use

Traceroute helps find where traffic stops or takes an unexpected path.

---

## Packet Capture

A packet capture records network packets for analysis.

### Common tools

- Wireshark
- tcpdump

### Interview answer

```text
A packet capture lets you inspect actual network traffic and is useful for troubleshooting protocols, handshakes, retransmissions, DNS, DHCP, and application issues.
```

---

## DNS Resolution Failure

DNS resolution failure means a name cannot be resolved to the needed record.

### Example

A user can ping:

```text
8.8.8.8
```

but cannot open:

```text
example.com
```

This may indicate DNS failure.

### Interview answer

```text
DNS resolution failure happens when a client cannot translate a hostname into an IP address. The network may still work by IP, but names fail.
```

---

## Asymmetric Routing

Asymmetric routing happens when traffic goes out one path and returns through a different path.

### Why it matters

Asymmetric routing can break stateful firewalls because the firewall may only see one direction of the session.

### Interview answer

```text
Asymmetric routing occurs when outbound and return traffic use different paths. It can cause issues with stateful firewalls and troubleshooting.
```

---

# 13. Quick Interview Questions and Answers

## What is the difference between a switch and a router?

A switch usually forwards frames inside a local network using MAC addresses.

A router forwards packets between networks using IP addresses.

## What is the difference between TCP and UDP?

TCP is reliable and connection-oriented. UDP is connectionless and lower overhead.

## What is a VLAN?

A VLAN is a logical Layer 2 network that separates devices into different broadcast domains.

## What is a default gateway?

A default gateway is the router a host uses to reach networks outside its local subnet.

## What is DNS?

DNS translates names like `example.com` into IP addresses and other records.

## What is DHCP?

DHCP automatically assigns IP configuration to clients.

## What is NAT?

NAT translates IP addresses, often private internal addresses to public addresses.

## What is PAT?

PAT lets many private hosts share one public IP address by translating ports.

## What is STP?

STP prevents Layer 2 loops by blocking redundant paths.

## What is ARP?

ARP maps an IPv4 address to a MAC address on a local network.

## What is OSPF?

OSPF is a link-state dynamic routing protocol commonly used inside enterprise networks.

## What is BGP?

BGP is a path-vector routing protocol used between autonomous systems and across the Internet.

## What is MTU?

MTU is the maximum packet size that can be sent on a link without fragmentation.

## What does APIPA usually indicate?

It usually means the client failed to get an address from DHCP.

## What is the difference between bandwidth and throughput?

Bandwidth is the link capacity. Throughput is the actual achieved data transfer rate.

---

# 14. Final Cheat Sheet

| Term | Short Meaning |
|---|---|
| Broadcast domain | Area where broadcasts can reach |
| Collision domain | Area where Ethernet collisions can happen |
| MAC address | Layer 2 hardware address |
| ARP | Maps IPv4 address to MAC address |
| VLAN | Logical Layer 2 network |
| Access port | Switch port for one VLAN |
| Trunk port | Switch port carrying multiple VLANs |
| Native VLAN | Untagged VLAN on a trunk |
| STP | Prevents Layer 2 loops |
| SVI | Layer 3 interface for a VLAN |
| CIDR | Prefix notation like `/24` |
| Subnet mask | Separates network and host bits |
| Wildcard mask | Inverse mask used in ACLs/routing |
| Default gateway | Router used to reach remote networks |
| APIPA | `169.254.0.0/16`, often DHCP failure |
| SLAAC | IPv6 automatic address configuration |
| NDP | IPv6 neighbor discovery protocol |
| Static route | Manually configured route |
| Default route | Route used when no specific route exists |
| Longest prefix match | Most specific route wins |
| Metric | Routing path cost/value |
| Administrative Distance | Trust value for route sources |
| OSPF | Link-state internal routing protocol |
| BGP | Internet-scale path-vector routing protocol |
| TCP | Reliable connection-oriented transport |
| UDP | Lightweight connectionless transport |
| Port | Identifies a service on a host |
| MTU | Maximum packet size on a link |
| MSS | Maximum TCP payload size |
| DNS | Resolves names to addresses/records |
| DHCP | Automatically assigns IP settings |
| NAT | Translates IP addresses |
| PAT | Many-to-one NAT using ports |
| ACL | Permit/deny rule list |
| Firewall | Enforces traffic policy |
| VPN | Secure tunnel over another network |
| QoS | Traffic prioritization/management |
| Latency | Delay |
| Jitter | Delay variation |
| Packet loss | Packets failing to arrive |
| Ping | Basic reachability test |
| Traceroute | Shows path to destination |
| Packet capture | Records packets for analysis |

---

## Final Note

The best way to learn these terms is not only to memorize them. Try to connect each term to a real troubleshooting situation.

For example:

- No IP address? Think DHCP, VLAN, relay, scope.
- Can ping IP but not name? Think DNS.
- Same VLAN works but other VLANs fail? Think routing, gateway, ACL, firewall.
- Network suddenly unstable? Think loop, STP, MAC flapping, bad cable.
- Website opens slowly? Think DNS delay, latency, packet loss, MTU, firewall inspection.

If you can connect the term to a real problem, you are much more prepared for both exams and interviews.
