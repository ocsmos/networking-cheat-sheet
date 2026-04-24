# 6. Core Network Protocols

Network protocols are the rules that devices follow when they communicate. Each protocol has a specific job. Some protocols move packets, some find neighbors, some assign addresses, some resolve names, and others carry application data.

This guide explains the core protocols you will see in real networks. The focus is practical: what each protocol does, where it fits, when you see it, and how it helps with troubleshooting.

---

## Table of Contents

- [1. What a Network Protocol Is](#1-what-a-network-protocol-is)
- [2. Why Protocols Are Layered](#2-why-protocols-are-layered)
- [3. Protocols by Layer](#3-protocols-by-layer)
- [4. Layer 2 and Neighboring Protocols](#4-layer-2-and-neighboring-protocols)
- [5. ARP](#5-arp)
- [6. NDP](#6-ndp)
- [7. STP and RSTP](#7-stp-and-rstp)
- [8. LLDP](#8-lldp)
- [9. LACP](#9-lacp)
- [10. Layer 3 and Routing Protocols](#10-layer-3-and-routing-protocols)
- [11. IPv4](#11-ipv4)
- [12. IPv6](#12-ipv6)
- [13. ICMP and ICMPv6](#13-icmp-and-icmpv6)
- [14. OSPF](#14-ospf)
- [15. RIP](#15-rip)
- [16. EIGRP](#16-eigrp)
- [17. BGP](#17-bgp)
- [18. Layer 4 Transport Protocols](#18-layer-4-transport-protocols)
- [19. TCP](#19-tcp)
- [20. UDP](#20-udp)
- [21. TCP vs UDP](#21-tcp-vs-udp)
- [22. TCP 3-Way Handshake](#22-tcp-3-way-handshake)
- [23. Common TCP Flags](#23-common-tcp-flags)
- [24. Application and Service Protocols](#24-application-and-service-protocols)
- [25. DNS](#25-dns)
- [26. DHCP and DHCPv6](#26-dhcp-and-dhcpv6)
- [27. HTTP and HTTPS](#27-http-and-https)
- [28. SMTP, IMAP, and POP3](#28-smtp-imap-and-pop3)
- [29. SNMP](#29-snmp)
- [30. NTP](#30-ntp)
- [31. SSH](#31-ssh)
- [32. LDAP](#32-ldap)
- [33. RADIUS and TACACS+](#33-radius-and-tacacs)
- [34. How Protocols Work Together](#34-how-protocols-work-together)
- [35. Example: Opening a Website](#35-example-opening-a-website)
- [36. Example: Getting an IP Address](#36-example-getting-an-ip-address)
- [37. Example: Sending an Email](#37-example-sending-an-email)
- [38. Troubleshooting by Protocol](#38-troubleshooting-by-protocol)
- [39. Useful Commands](#39-useful-commands)
- [40. Security Notes](#40-security-notes)
- [41. Quick Summary](#41-quick-summary)

---

## 1. What a Network Protocol Is

A network protocol is a set of rules for communication.

A protocol defines things like:

- How data is formatted
- How devices identify each other
- How errors are handled
- How connections start and end
- Which ports are used
- What messages mean
- How devices respond to those messages

### Simple example

When a browser opens a website, it does not just send random data to a server.

It follows several protocols, such as:

- DNS to find the server IP address
- TCP to create a reliable connection
- TLS to encrypt the session
- HTTP to request the web page
- IP to route packets
- Ethernet or Wi-Fi to move frames locally

Each protocol handles one part of the process.

---

## 2. Why Protocols Are Layered

Network protocols are usually explained in layers because each layer has a different responsibility.

This makes networks easier to design, understand, and troubleshoot.

### Example idea

If a website does not load, the problem could be:

- Physical cable issue
- Wi-Fi issue
- VLAN issue
- IP address issue
- Routing issue
- DNS issue
- TCP port issue
- TLS certificate issue
- Web server issue

Layering helps you narrow down the problem.

### Common model

A practical view is:

| Layer | Main Job | Example Protocols |
|---|---|---|
| Layer 2 | Local network delivery | Ethernet, ARP, STP, LLDP, LACP |
| Layer 3 | Logical addressing and routing | IPv4, IPv6, ICMP, OSPF, BGP |
| Layer 4 | Transport between applications | TCP, UDP |
| Application | User and service protocols | DNS, DHCP, HTTP, SSH, SMTP, SNMP |

---

## 3. Protocols by Layer

Here is a quick grouping of important protocols.

### Layer 2 / Neighboring

- ARP
- NDP
- STP
- RSTP
- LLDP
- LACP

### Layer 3 / Routing

- IPv4
- IPv6
- ICMP
- ICMPv6
- OSPF
- RIP
- EIGRP
- BGP

### Layer 4 / Transport

- TCP
- UDP

### Application / Services

- DNS
- DHCP
- DHCPv6
- HTTP
- HTTPS
- SMTP
- IMAP
- POP3
- SNMP
- NTP
- SSH
- LDAP
- RADIUS
- TACACS+

---

## 4. Layer 2 and Neighboring Protocols

Layer 2 protocols help devices communicate on the local network segment.

Layer 2 is where you deal with:

- MAC addresses
- Ethernet frames
- Switches
- VLANs
- Trunks
- Local neighbor discovery
- Loop prevention
- Link aggregation

Layer 2 does not route traffic between different IP networks. That is Layer 3's job.

### Common Layer 2 questions

When troubleshooting, ask:

- Is the cable connected?
- Is the switch port up?
- Is the device in the correct VLAN?
- Is the switch learning the MAC address?
- Is STP blocking the port?
- Is the trunk carrying the right VLANs?
- Is LACP working correctly?

---

## 5. ARP

ARP stands for **Address Resolution Protocol**.

ARP is used in IPv4 networks to map an IPv4 address to a MAC address.

### Why ARP is needed

IPv4 uses IP addresses for logical addressing, but Ethernet uses MAC addresses for local delivery.

Before a device can send an IPv4 packet to another device on the same local network, it needs the destination MAC address.

ARP solves that problem.

### Example

A computer has this IP address:

```text
192.168.1.10
```

It wants to send traffic to the default gateway:

```text
192.168.1.1
```

The computer asks:

```text
Who has 192.168.1.1? Tell 192.168.1.10.
```

The gateway replies:

```text
192.168.1.1 is at aa:bb:cc:dd:ee:ff.
```

Now the computer can send Ethernet frames to the gateway MAC address.

### ARP request

An ARP request is usually broadcast on the local network.

The destination MAC address is:

```text
ff:ff:ff:ff:ff:ff
```

That means every device in the broadcast domain receives it.

### ARP reply

An ARP reply is usually unicast back to the requester.

### ARP cache

Devices store ARP results in an ARP cache.

This avoids asking the same question repeatedly.

### Useful command

Windows, Linux, and macOS commonly support:

```bash
arp -a
```

This shows the local ARP cache.

### Common ARP problems

ARP problems can cause symptoms like:

- Cannot reach gateway
- Intermittent connectivity
- Duplicate IP address issues
- Wrong MAC address in ARP cache
- Local subnet communication failure

### Security note

ARP has no strong built-in authentication. Attackers can abuse ARP with techniques like ARP spoofing or ARP poisoning.

Common protections include:

- Dynamic ARP Inspection
- DHCP snooping
- Static ARP entries in special cases
- Network segmentation
- Monitoring for duplicate IP/MAC behavior

---

## 6. NDP

NDP stands for **Neighbor Discovery Protocol**.

NDP is used in IPv6 networks. It performs several jobs that ARP and other IPv4 mechanisms handled separately.

NDP uses ICMPv6 messages.

### What NDP does

NDP helps with:

- Finding neighboring devices
- Resolving IPv6 addresses to MAC addresses
- Finding routers
- Learning network prefixes
- Detecting duplicate IPv6 addresses
- Redirecting hosts to better next hops

### NDP vs ARP

| Feature | ARP | NDP |
|---|---|---|
| Used by | IPv4 | IPv6 |
| Transport | Directly over Layer 2 | ICMPv6 |
| Neighbor address resolution | Yes | Yes |
| Router discovery | No | Yes |
| Duplicate address detection | Not built into ARP itself | Yes |
| Broadcast use | Yes | No, uses multicast |

### Important NDP messages

Common NDP-related ICMPv6 messages include:

- Neighbor Solicitation
- Neighbor Advertisement
- Router Solicitation
- Router Advertisement
- Redirect

### Router Advertisement

Router Advertisements help hosts learn IPv6 network information.

They can tell hosts:

- The local prefix
- The default gateway
- Whether SLAAC is available
- Whether DHCPv6 should be used

### Common NDP problems

NDP problems may appear as:

- IPv6 gateway not reachable
- SLAAC not working
- Duplicate address detection failures
- IPv6 works locally but not beyond the router
- ICMPv6 blocked by firewall rules

### Important warning

Do not block ICMPv6 carelessly. IPv6 needs ICMPv6 for normal operation.

---

## 7. STP and RSTP

STP stands for **Spanning Tree Protocol**.

RSTP stands for **Rapid Spanning Tree Protocol**.

They are used to prevent switching loops in Layer 2 networks.

### Why loops are dangerous

Ethernet frames do not have a TTL field like IP packets.

If a Layer 2 loop exists, frames can circulate endlessly.

This can cause:

- Broadcast storms
- MAC table instability
- High CPU on switches
- Network outages
- Packet loss
- Very slow network behavior

### What STP does

STP builds a loop-free logical topology.

It allows some switch ports to forward traffic and blocks others to prevent loops.

If the active path fails, STP can unblock another path.

### Root bridge

STP elects a root bridge.

The root bridge is the central reference point for the spanning tree.

Switches calculate best paths toward the root bridge.

### Common RSTP states

RSTP uses these common states:

| State | Meaning |
|---|---|
| Discarding | Not forwarding normal traffic |
| Learning | Learning MAC addresses |
| Forwarding | Forwarding traffic |

### STP vs RSTP

RSTP is faster than classic STP.

In modern networks, RSTP or vendor variants are much more common than original STP.

### Common STP problems

STP issues may cause:

- A port is unexpectedly blocked
- VLAN connectivity is broken
- Slow convergence after link failure
- Root bridge is not where expected
- Layer 2 loops
- Broadcast storms

### Useful checks

On switches, check:

- Root bridge
- Port states
- Blocked ports
- BPDU activity
- VLAN spanning-tree state
- Recent topology changes

---

## 8. LLDP

LLDP stands for **Link Layer Discovery Protocol**.

It allows directly connected network devices to advertise information about themselves.

LLDP is vendor-neutral.

### What LLDP tells you

LLDP can show information such as:

- Neighbor device name
- Neighbor port ID
- Device description
- VLAN information
- Management IP address
- System capabilities

### Why LLDP is useful

LLDP is very useful for troubleshooting and documentation.

It can answer questions like:

```text
What device is connected to this switch port?
```

or:

```text
Which switch port is this server connected to?
```

### Example

A switch port may show:

```text
Local port: Gi1/0/10
Neighbor: access-switch-2
Neighbor port: Gi1/0/48
```

This helps trace physical and logical connections.

### LLDP vs CDP

CDP is Cisco Discovery Protocol.

LLDP is open standard and works across many vendors.

| Protocol | Type |
|---|---|
| LLDP | Vendor-neutral |
| CDP | Cisco-specific |

### Security note

LLDP can reveal network details. Some organizations disable LLDP on untrusted access ports.

---

## 9. LACP

LACP stands for **Link Aggregation Control Protocol**.

It is used to bundle multiple physical links into one logical link.

This is often called:

- Link aggregation
- Port channel
- EtherChannel
- LAG

### Why LACP is used

LACP helps provide:

- More bandwidth
- Redundancy
- Better link utilization
- Cleaner switch-to-switch or server-to-switch connections

### Example

Two 1 Gbps links can be bundled into one logical port channel.

```text
Link 1: 1 Gbps
Link 2: 1 Gbps
Logical bundle: Port-channel
```

The total available bandwidth can be higher, but a single flow may not always use both links at once. Load balancing depends on hashing behavior.

### LACP modes

Common modes include:

| Mode | Behavior |
|---|---|
| Active | Actively negotiates LACP |
| Passive | Waits for the other side to negotiate |
| On/static | Forces a bundle without LACP negotiation |

### Common LACP problems

LACP issues can happen when:

- One side is active and the other is not configured correctly
- VLAN trunk settings do not match
- Speed or duplex settings differ
- The wrong ports are bundled
- One side uses static bundling and the other uses LACP
- Switch stacking or MLAG/vPC settings are wrong

### Troubleshooting checks

Check:

- Are all member links up?
- Are both sides using compatible LACP mode?
- Are trunk/access settings identical?
- Are allowed VLANs consistent?
- Is the port-channel interface up?
- Are any member links suspended?

---

## 10. Layer 3 and Routing Protocols

Layer 3 protocols handle logical addressing and routing between networks.

Layer 3 is where you deal with:

- IPv4 addresses
- IPv6 addresses
- Routers
- Routing tables
- Default gateways
- ICMP
- Dynamic routing protocols

### Main Layer 3 job

Layer 3 answers:

```text
How do I get this packet from one IP network to another IP network?
```

### Common Layer 3 problems

Layer 3 problems may include:

- Wrong IP address
- Wrong subnet mask or prefix length
- Missing default gateway
- Missing route
- Wrong next hop
- ACL blocking traffic
- Routing loop
- Dynamic routing neighbor failure
- Asymmetric routing

---

## 11. IPv4

IPv4 stands for **Internet Protocol version 4**.

It is the most widely known IP version and uses 32-bit addresses.

### Example IPv4 address

```text
192.168.1.10
```

### What IPv4 does

IPv4 provides:

- Logical addressing
- Packet routing
- Source and destination IP fields
- Fragmentation behavior
- TTL field
- Protocol field to identify upper-layer protocol

### IPv4 packet delivery

When a device sends IPv4 traffic, it checks whether the destination is local or remote.

If the destination is local, the device sends directly to the local host.

If the destination is remote, the device sends traffic to its default gateway.

### Example

Host:

```text
IP: 192.168.1.10
Mask: 255.255.255.0
Gateway: 192.168.1.1
```

Destination:

```text
8.8.8.8
```

Because `8.8.8.8` is not in `192.168.1.0/24`, the host sends the packet to the default gateway.

### Common IPv4 issues

- Duplicate IP address
- Wrong subnet mask
- Wrong gateway
- Missing route
- NAT problems
- ACL or firewall blocks
- ARP failure

---

## 12. IPv6

IPv6 stands for **Internet Protocol version 6**.

It uses 128-bit addresses and was designed to solve the address shortage problem in IPv4.

### Example IPv6 address

```text
2001:db8::10
```

### What IPv6 does

IPv6 provides:

- Larger address space
- Simplified header design
- No broadcast
- Strong multicast usage
- Neighbor Discovery through ICMPv6
- SLAAC support
- Better address planning for large networks

### IPv6 default route

The IPv6 default route is:

```text
::/0
```

### Important IPv6 ideas

- Link-local addresses are important
- `/64` is the common subnet size
- ICMPv6 is required for many normal functions
- NDP replaces ARP
- IPv6 does not use broadcast

### Common IPv6 issues

- Missing router advertisement
- Bad prefix configuration
- ICMPv6 blocked
- No default route
- DNS has no AAAA record
- Device has link-local only but no global address

---

## 13. ICMP and ICMPv6

ICMP stands for **Internet Control Message Protocol**.

ICMP is used for control messages, error messages, and diagnostics.

### ICMP in IPv4

Common ICMP messages include:

- Echo request
- Echo reply
- Destination unreachable
- Time exceeded
- Redirect

### Ping

The `ping` command commonly uses ICMP echo request and echo reply.

Example:

```bash
ping 8.8.8.8
```

If the remote host replies, you know some basic Layer 3 connectivity exists.

### Traceroute

Traceroute uses TTL or hop-limit behavior to discover the path to a destination.

Linux/macOS:

```bash
traceroute example.com
```

Windows:

```powershell
tracert example.com
```

### ICMPv6

ICMPv6 is even more important than ICMP in IPv4.

IPv6 uses ICMPv6 for:

- Neighbor discovery
- Router discovery
- Router advertisements
- Router solicitations
- Path MTU discovery
- Error reporting
- Ping

### Important warning

Blocking all ICMP or ICMPv6 is a common mistake.

You should filter carefully, not blindly block everything.

### Common ICMP-related problems

- Ping blocked by firewall
- Traceroute incomplete
- Path MTU problems
- IPv6 neighbor discovery broken
- Misleading troubleshooting because ICMP is filtered

---

## 14. OSPF

OSPF stands for **Open Shortest Path First**.

It is a dynamic routing protocol commonly used inside enterprise networks.

OSPF is a link-state routing protocol.

### What OSPF does

OSPF allows routers to:

- Discover neighbors
- Exchange routing information
- Build a topology database
- Calculate best paths
- React to network changes

### OSPF metric

OSPF uses **cost** as its metric.

Cost is usually based on interface bandwidth, though exact behavior depends on configuration and platform.

### OSPF areas

OSPF supports areas.

The backbone area is:

```text
Area 0
```

Areas help scale larger OSPF networks.

### OSPF neighbor relationships

Routers must form neighbor relationships before exchanging routes.

Common requirements include matching:

- Area ID
- Authentication settings
- Hello/dead timers
- Network type
- Subnet
- MTU in many cases

### Common OSPF problems

- Neighbors stuck in INIT or EXSTART
- Area mismatch
- Authentication mismatch
- MTU mismatch
- Passive interface configured by mistake
- Missing network statement
- Route filtering
- Wrong cost

### Useful show commands

On many network devices, you check:

```text
show ip ospf neighbor
show ip route ospf
show ip ospf interface
```

Command syntax depends on vendor.

---

## 15. RIP

RIP stands for **Routing Information Protocol**.

RIP is an older distance-vector routing protocol.

### RIP metric

RIP uses hop count.

A route with fewer hops is preferred.

### RIP limit

RIP has a maximum useful hop count of 15.

A route with 16 hops is considered unreachable.

### Where RIP is used

RIP is mostly seen in:

- Simple labs
- Legacy networks
- Basic training
- Small environments

It is not common for modern large networks.

### Common RIP issues

- Hop count limit
- Slow convergence
- Wrong version
- Missing network configuration
- Passive interfaces
- Route loops in poor designs

### Practical note

RIP is easy to learn, but OSPF and BGP are much more important in many real environments.

---

## 16. EIGRP

EIGRP stands for **Enhanced Interior Gateway Routing Protocol**.

It is commonly associated with Cisco environments.

EIGRP is often described as an advanced distance-vector or hybrid routing protocol.

### What EIGRP does

EIGRP exchanges routing information between routers and calculates efficient paths.

It can converge quickly when designed correctly.

### EIGRP metric

EIGRP commonly considers:

- Bandwidth
- Delay

It can also include other values depending on configuration, but bandwidth and delay are the main practical ones.

### Important EIGRP terms

- Neighbor
- Successor
- Feasible successor
- Feasible distance
- Reported distance
- DUAL algorithm

### Common EIGRP problems

- Neighbor mismatch
- AS number mismatch
- K-value mismatch
- Authentication mismatch
- Passive interface
- Missing network statement
- Stub configuration issues

### Practical note

EIGRP is very useful in Cisco-heavy networks, but it is less universal than OSPF.

---

## 17. BGP

BGP stands for **Border Gateway Protocol**.

BGP is the main routing protocol of the Internet.

It is also used inside large data centers, cloud networks, and enterprise WANs.

BGP is a path-vector routing protocol.

### What BGP does

BGP exchanges routes between autonomous systems.

An autonomous system, or AS, is a network under one administrative control with a routing policy.

### BGP port

BGP uses:

```text
179/TCP
```

### eBGP vs iBGP

| Type | Meaning |
|---|---|
| eBGP | BGP between different autonomous systems |
| iBGP | BGP within the same autonomous system |

### Why BGP is important

BGP is used for:

- Internet routing
- ISP connectivity
- Multi-homing
- Cloud interconnects
- Data center fabrics
- Large enterprise routing policy

### BGP path selection

BGP does not simply choose the fastest path. It uses attributes and policy.

Common BGP attributes include:

- AS path
- Local preference
- MED
- Next hop
- Origin
- Communities

### Common BGP problems

- Neighbor session down
- TCP 179 blocked
- Wrong AS number
- Authentication mismatch
- Route not advertised
- Route filtered by policy
- Next-hop unreachable
- Prefix limit reached
- Bad route-map or policy

### Useful checks

Common things to check:

- Is the neighbor reachable?
- Is TCP 179 open?
- Is the peer AS correct?
- Are routes being advertised?
- Are routes being received?
- Is policy filtering the route?
- Is the next hop reachable?

---

## 18. Layer 4 Transport Protocols

Layer 4 protocols provide communication between applications running on hosts.

The two most important transport protocols are:

- TCP
- UDP

Layer 4 uses ports.

Example:

```text
192.0.2.10:443
```

Here:

- `192.0.2.10` is the IP address
- `443` is the port

The port identifies the service.

---

## 19. TCP

TCP stands for **Transmission Control Protocol**.

TCP is reliable and connection-oriented.

### TCP provides

- Connection setup
- Ordered delivery
- Retransmission of lost data
- Flow control
- Congestion control
- Session tracking through ports

### Common TCP applications

- HTTP
- HTTPS
- SSH
- SMTP
- IMAP
- POP3
- SMB
- RDP
- LDAP
- Many databases

### TCP example

When you connect to a website using HTTPS, your device usually opens a TCP connection to port 443.

```text
Client -> Server:443/TCP
```

Then TLS and HTTP traffic run over that TCP connection.

### Common TCP symptoms

TCP problems may appear as:

- Connection timeout
- Connection refused
- TCP reset
- Slow transfer
- Retransmissions
- High latency
- Window size issues

### Timeout vs refused

These two symptoms mean different things.

#### Timeout

A timeout often means the client received no useful response.

Possible causes:

- Firewall drop
- Routing issue
- Server down
- Packet loss

#### Connection refused

Connection refused often means the server responded, but nothing is listening on that port, or the host actively rejected the connection.

---

## 20. UDP

UDP stands for **User Datagram Protocol**.

UDP is connectionless and lightweight.

### UDP provides

- Simple transport
- Low overhead
- No connection setup
- No built-in retransmission
- No built-in ordering

### Common UDP applications

- DNS
- DHCP
- NTP
- SNMP
- VoIP media
- Video streaming
- Some VPNs
- Some games

### Why UDP is useful

UDP is useful when:

- Speed matters
- Small request/response messages are enough
- The application handles reliability itself
- Real-time traffic should not wait for retransmission

### UDP example

A DNS query usually uses UDP port 53.

```text
Client -> DNS Server:53/UDP
```

### Common UDP problems

UDP can be harder to troubleshoot than TCP because there is no handshake.

Problems may appear as:

- No response
- Intermittent response
- Application timeout
- One-way audio in VoIP
- VPN tunnel not forming
- DNS queries failing silently

---

## 21. TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Reliability | Built in | Not built in |
| Ordering | Built in | Not built in |
| Speed/overhead | More overhead | Lower overhead |
| Common use | Web, SSH, email, databases | DNS, DHCP, NTP, voice, video |
| Troubleshooting | Handshake visible | No handshake |

### Simple rule

Use TCP when reliable ordered delivery matters.

Use UDP when speed, simplicity, or real-time behavior matters.

This is a simplification, but it is useful when learning.

---

## 22. TCP 3-Way Handshake

Before two systems exchange normal TCP data, they create a TCP connection.

This is done with the TCP 3-way handshake.

### Steps

1. SYN
2. SYN-ACK
3. ACK

### Step 1: SYN

The client sends a SYN to the server.

```text
Client -> Server: SYN
```

This means the client wants to start a TCP connection.

### Step 2: SYN-ACK

The server replies with SYN-ACK.

```text
Server -> Client: SYN-ACK
```

This means the server accepts the connection request and sends its own synchronization.

### Step 3: ACK

The client replies with ACK.

```text
Client -> Server: ACK
```

Now the TCP connection is established.

### Why this matters

The handshake helps you troubleshoot.

If you see:

```text
SYN, SYN, SYN
```

with no SYN-ACK, the server is not replying or the reply is not getting back.

If you see:

```text
SYN -> RST
```

something is actively refusing the connection.

---

## 23. Common TCP Flags

TCP flags describe the state or purpose of a TCP segment.

| Flag | Meaning |
|---|---|
| SYN | Start a connection |
| ACK | Acknowledge received data |
| FIN | Finish a connection gracefully |
| RST | Reset or abort a connection |
| PSH | Push data to the application quickly |
| URG | Urgent pointer field is significant |
| ECE | ECN Echo |
| CWR | Congestion Window Reduced |

### SYN

Used to start a TCP connection.

### ACK

Used to acknowledge received data.

Most packets after the handshake have ACK set.

### FIN

Used to close a connection gracefully.

### RST

Used to reset a connection immediately.

RST often appears when:

- A port is closed
- A firewall or host rejects traffic
- An application aborts a connection
- A security device interrupts a session

### PSH

Tells the receiver to deliver data to the application promptly.

### ECE and CWR

Used with Explicit Congestion Notification.

These flags are more advanced and are related to congestion handling.

---

## 24. Application and Service Protocols

Application protocols sit above transport protocols like TCP and UDP.

They define how applications communicate.

Examples:

- DNS defines name lookups
- HTTP defines web requests and responses
- SMTP defines mail transfer
- SSH defines secure remote sessions
- SNMP defines network monitoring queries

Application protocols often have default ports, but they are not limited to those ports.

For example, HTTP commonly uses TCP 80, but a web application can also run on TCP 8080.

---

## 25. DNS

DNS stands for **Domain Name System**.

DNS maps names to IP addresses and other records.

### Common ports

```text
53/UDP
53/TCP
853/TCP for DNS over TLS
443/TCP for DNS over HTTPS
```

### Common DNS records

- A
- AAAA
- CNAME
- MX
- NS
- TXT
- PTR
- SOA

### Example

A client asks:

```text
What is the IP address for example.com?
```

DNS may answer:

```text
example.com -> 93.184.216.34
```

### Common DNS issues

- Wrong DNS server
- Missing record
- Old cached record
- Broken delegation
- DNSSEC validation failure
- Split DNS issue
- Firewall blocking UDP or TCP 53

---

## 26. DHCP and DHCPv6

DHCP stands for **Dynamic Host Configuration Protocol**.

DHCP automatically gives network settings to clients.

### DHCP for IPv4

DHCP commonly provides:

- IP address
- Subnet mask
- Default gateway
- DNS server
- Lease time

### DHCP ports

```text
67/UDP server
68/UDP client
```

### DHCP DORA process

DHCP often follows this process:

1. Discover
2. Offer
3. Request
4. Acknowledge

### DHCPv6

DHCPv6 is used in IPv6 networks.

Ports:

```text
546/UDP client
547/UDP server
```

IPv6 may use DHCPv6, SLAAC, or both depending on the network design.

### Common DHCP issues

- No available addresses in scope
- DHCP server down
- Relay/helper missing
- Wrong VLAN
- Wrong gateway option
- Wrong DNS option
- Client gets APIPA address in IPv4

---

## 27. HTTP and HTTPS

HTTP stands for **Hypertext Transfer Protocol**.

HTTPS is HTTP protected with TLS encryption.

### Common ports

```text
80/TCP  HTTP
443/TCP HTTPS
```

### What HTTP does

HTTP is request/response based.

A client sends a request:

```text
GET /index.html
```

The server sends a response:

```text
200 OK
```

### What HTTPS adds

HTTPS adds encryption, authentication, and integrity through TLS.

It helps protect:

- Login credentials
- Cookies
- Personal data
- API traffic
- Web content from tampering

### Common HTTP/HTTPS issues

- Web server down
- DNS wrong
- TCP 80/443 blocked
- TLS certificate expired
- Certificate name mismatch
- Wrong virtual host
- Reverse proxy issue
- Application error

---

## 28. SMTP, IMAP, and POP3

These are common email protocols.

### SMTP

SMTP stands for **Simple Mail Transfer Protocol**.

It is used to send email.

Common ports:

```text
25/TCP   server-to-server mail delivery
587/TCP  mail submission
465/TCP  SMTP over implicit TLS
```

### IMAP

IMAP is used by mail clients to access mailboxes.

Common ports:

```text
143/TCP  IMAP
993/TCP  IMAPS
```

IMAP is useful when users access mail from multiple devices.

### POP3

POP3 is an older mailbox access protocol.

Common ports:

```text
110/TCP  POP3
995/TCP  POP3S
```

### Common email protocol issues

- MX record missing or wrong
- SMTP port blocked
- Authentication failure
- TLS problem
- SPF/DKIM/DMARC issue
- Reverse DNS problem
- Mailbox access blocked

---

## 29. SNMP

SNMP stands for **Simple Network Management Protocol**.

It is used to monitor and manage network devices.

### Common ports

```text
161/UDP  SNMP queries
162/UDP  SNMP traps
```

### What SNMP monitors

SNMP can collect information such as:

- Interface status
- Interface bandwidth
- CPU usage
- Memory usage
- Device temperature
- Uptime
- Errors
- Packet counters

### SNMP versions

| Version | Notes |
|---|---|
| SNMPv1 | Old and weak |
| SNMPv2c | Common but community-string based |
| SNMPv3 | Better security with authentication and encryption |

### Security note

Use SNMPv3 where possible.

Avoid exposing SNMP to untrusted networks.

---

## 30. NTP

NTP stands for **Network Time Protocol**.

It synchronizes clocks across devices.

### Common port

```text
123/UDP
```

### Why time matters

Correct time is important for:

- Logs
- Authentication
- Kerberos
- Certificates
- Security investigations
- File timestamps
- Distributed systems

### Common NTP issues

- Firewall blocking UDP 123
- Wrong time source
- Large clock drift
- NTP server unreachable
- Time zone confusion
- Virtual machine clock issues

### Practical note

Many authentication systems are sensitive to time differences.

If logins fail strangely, checking time synchronization is a good step.

---

## 31. SSH

SSH stands for **Secure Shell**.

It provides secure remote access.

### Common port

```text
22/TCP
```

### What SSH is used for

- Remote shell access
- Network device administration
- Secure file transfer
- Git over SSH
- Tunneling
- Automation

### Why SSH is preferred over Telnet

SSH encrypts traffic.

Telnet sends data in clear text.

Use SSH instead of Telnet whenever possible.

### Common SSH issues

- TCP 22 blocked
- SSH service not running
- Wrong username
- Bad key permissions
- Authentication failure
- Host key warning
- Server allows only key-based login

---

## 32. LDAP

LDAP stands for **Lightweight Directory Access Protocol**.

It is used to access and manage directory information.

### Common ports

```text
389/TCP or UDP  LDAP
636/TCP         LDAPS
```

### What LDAP is used for

- User lookup
- Group lookup
- Directory authentication
- Application integration
- Address books
- Identity systems

### LDAP and Active Directory

Microsoft Active Directory uses LDAP as one of its important protocols.

Applications often use LDAP to check users and groups in a directory.

### Common LDAP issues

- Wrong server name
- Firewall blocking port 389 or 636
- TLS certificate problem
- Bind account failure
- Search base wrong
- User not in expected group
- DNS SRV record issue

---

## 33. RADIUS and TACACS+

RADIUS and TACACS+ are used for centralized authentication, authorization, and accounting.

They are common in network and security environments.

### RADIUS

RADIUS commonly uses:

```text
1812/UDP  Authentication
1813/UDP  Accounting
```

Common uses:

- VPN login
- Wi-Fi Enterprise authentication
- 802.1X
- Network device login
- Remote access authentication

### TACACS+

TACACS+ commonly uses:

```text
49/TCP
```

It is often used for network device administration.

### Difference in practical use

RADIUS is very common for user/network access.

TACACS+ is often preferred for detailed network device command authorization.

### Common AAA issues

- Shared secret mismatch
- Firewall blocking the port
- Wrong source IP
- User not in correct group
- Policy mismatch
- Time synchronization problem
- Certificate problem for 802.1X

---

## 34. How Protocols Work Together

Real communication usually requires many protocols at once.

A user might think they are simply opening a website, but behind the scenes many protocols are involved.

Example stack:

```text
Ethernet/Wi-Fi -> ARP/NDP -> IP -> TCP -> TLS -> HTTP -> DNS support
```

Each protocol solves one part of the full communication path.

### Why this matters

When troubleshooting, do not focus on only one protocol too early.

A web issue could actually be caused by:

- DNS failure
- ARP failure
- Routing failure
- TCP block
- TLS issue
- HTTP application error

Understanding protocol roles helps you isolate the real layer of failure.

---

## 35. Example: Opening a Website

Imagine a user opens:

```text
https://www.example.com
```

Here is a simplified flow.

### Step 1: DNS lookup

The client asks DNS:

```text
What is the IP address of www.example.com?
```

DNS returns an IP address.

### Step 2: Local delivery decision

The client checks if the destination IP is local or remote.

If remote, it sends traffic to the default gateway.

### Step 3: ARP or NDP

For IPv4, the client may use ARP to find the gateway MAC address.

For IPv6, the client uses NDP.

### Step 4: TCP connection

The client starts a TCP connection to port 443.

```text
SYN -> SYN-ACK -> ACK
```

### Step 5: TLS session

The client and server negotiate encryption.

The server presents a certificate.

### Step 6: HTTP request

The browser sends an HTTP request inside the encrypted TLS session.

### Step 7: Server response

The server sends back the web content.

### Troubleshooting lesson

If the site fails, the problem could be at any of those steps.

---

## 36. Example: Getting an IP Address

When a new IPv4 client joins a network, it may use DHCP.

### DHCP DORA flow

1. Discover
2. Offer
3. Request
4. Acknowledge

### Step 1: Discover

The client broadcasts a DHCP Discover message.

It is asking:

```text
Is there a DHCP server available?
```

### Step 2: Offer

The DHCP server offers an address.

Example:

```text
You can use 192.168.1.50.
```

### Step 3: Request

The client requests the offered address.

### Step 4: Acknowledge

The server confirms the lease.

### Information received

The client may receive:

- IP address
- Subnet mask
- Default gateway
- DNS server
- Lease time

### Common failure

If DHCP fails on Windows, the client may assign itself an APIPA address:

```text
169.254.x.x
```

That often means the client could not reach a DHCP server.

---

## 37. Example: Sending an Email

Sending email uses several protocols and DNS records.

### Step 1: Mail client submits message

A user sends mail through a mail submission server.

Common port:

```text
587/TCP
```

### Step 2: Sender server checks recipient domain

The sending mail server checks DNS MX records for the recipient domain.

Example:

```text
example.net MX 10 mail.example.net
```

### Step 3: Sender server delivers mail

The sending server connects to the recipient mail server, often over:

```text
25/TCP
```

### Step 4: Recipient reads mail

The recipient may read mail using:

```text
993/TCP IMAPS
```

or another mailbox access method.

### Other DNS checks

Mail systems often check:

- SPF
- DKIM
- DMARC
- PTR / reverse DNS

### Troubleshooting lesson

Email problems often involve both application protocols and DNS.

---

## 38. Troubleshooting by Protocol

A structured approach helps you avoid guessing.

### ARP troubleshooting

Check:

- Is the destination in the same subnet?
- Is the ARP entry present?
- Is the MAC address correct?
- Is there a duplicate IP?

Command:

```bash
arp -a
```

### NDP troubleshooting

Check:

- Does the host have a link-local address?
- Does it know the router?
- Are Router Advertisements present?
- Is ICMPv6 blocked?

Command examples:

```bash
ip -6 neigh
ip -6 route
```

### DNS troubleshooting

Check:

- Which DNS server is configured?
- Does the name resolve?
- Does another resolver give a different answer?
- Is the record type correct?

Commands:

```bash
dig example.com
nslookup example.com
```

### TCP troubleshooting

Check:

- Is the server reachable?
- Is the port open?
- Is the service listening?
- Is a firewall dropping or rejecting traffic?

Commands:

```bash
nc -vz example.com 443
curl -I https://example.com
```

### UDP troubleshooting

Check:

- Is the service reachable?
- Is the firewall allowing UDP?
- Does the application receive a response?
- Is NAT affecting the traffic?

Commands depend on the protocol. For DNS:

```bash
dig @8.8.8.8 example.com
```

### Routing protocol troubleshooting

Check:

- Are neighbors up?
- Are routes learned?
- Are routes advertised?
- Is policy filtering routes?
- Is the next hop reachable?

---

## 39. Useful Commands

### Windows

```powershell
ipconfig /all
ping 8.8.8.8
ping -6 google.com
tracert example.com
nslookup example.com
arp -a
route print
netstat -ano
Test-NetConnection example.com -Port 443
```

### Linux

```bash
ip addr
ip route
ip -6 route
ip neigh
ip -6 neigh
ping 8.8.8.8
ping -6 google.com
traceroute example.com
dig example.com
ss -tulpn
nc -vz example.com 443
tcpdump -i eth0
```

### macOS

```bash
ifconfig
netstat -rn
netstat -rn -f inet6
arp -a
ping 8.8.8.8
ping6 google.com
traceroute example.com
dig example.com
lsof -i -P -n
nc -vz example.com 443
```

### Packet capture examples

Capture DNS traffic:

```bash
tcpdump -i eth0 port 53
```

Capture HTTP/HTTPS connection attempts:

```bash
tcpdump -i eth0 host example.com
```

Capture ARP:

```bash
tcpdump -i eth0 arp
```

Capture ICMP:

```bash
tcpdump -i eth0 icmp
```

Capture ICMPv6:

```bash
tcpdump -i eth0 icmp6
```

---

## 40. Security Notes

Protocols can create security risks if they are exposed or configured poorly.

### Clear-text protocols

Avoid clear-text protocols when possible.

Examples:

| Protocol | Concern | Safer option |
|---|---|---|
| Telnet | Sends credentials in clear text | SSH |
| FTP | Sends data and credentials in clear text | SFTP, FTPS, SCP |
| HTTP | No encryption | HTTPS |
| LDAP | May be clear text | LDAPS or StartTLS |
| SNMPv1/v2c | Weak community strings | SNMPv3 |

### Management protocols

Management protocols should be restricted.

Examples:

- SSH
- RDP
- SNMP
- Web admin panels
- Database ports
- Hypervisor management
- Firewall management

Use controls such as:

- VPN
- MFA
- Admin subnet restrictions
- Strong authentication
- Logging
- Least privilege

### Routing protocol security

Routing protocols should be protected.

Consider:

- Neighbor authentication
- Prefix filtering
- Route maps / policies
- Maximum prefix limits
- Passive interfaces
- Control plane protection

### DNS security

DNS should be protected against:

- Open resolver abuse
- Cache poisoning
- Zone transfer exposure
- Bad delegation
- DNS tunneling
- Unauthorized record changes

### General advice

Every protocol you enable should have a reason.

If a protocol is not needed, disable it.

If it is needed, protect it.

---

## 41. Quick Summary

Core network protocols are the basic rules that make networks work.

The most important points to remember are:

- ARP maps IPv4 addresses to MAC addresses
- NDP performs neighbor discovery for IPv6
- STP and RSTP prevent Layer 2 loops
- LLDP helps identify directly connected devices
- LACP bundles physical links into logical links
- IPv4 and IPv6 provide logical addressing and routing
- ICMP and ICMPv6 provide control and diagnostic messages
- OSPF, RIP, EIGRP, and BGP are routing protocols
- TCP is reliable and connection-oriented
- UDP is lightweight and connectionless
- TCP uses a 3-way handshake
- Common TCP flags include SYN, ACK, FIN, and RST
- DNS resolves names to records and IP addresses
- DHCP assigns network settings automatically
- HTTP and HTTPS carry web traffic
- SMTP, IMAP, and POP3 support email
- SNMP monitors network devices
- NTP synchronizes time
- SSH provides secure remote access
- LDAP supports directory access
- RADIUS and TACACS+ support centralized authentication

If you understand what each protocol does and where it fits, troubleshooting becomes much easier. Instead of guessing, you can move layer by layer and protocol by protocol until you find the real problem.
