# 9. NAT, ACLs, Firewalls, and VPNs

NAT, ACLs, firewalls, and VPNs are core parts of real network operations. They are used to control traffic, protect systems, connect private networks, and allow users to reach internal resources safely.

This guide explains these topics in a detailed but practical way. The goal is to understand what each technology does, why it exists, how it behaves in real networks, and what to check when troubleshooting.

---

## Table of Contents

- [1. Big Picture](#1-big-picture)
- [2. Why These Topics Are Grouped Together](#2-why-these-topics-are-grouped-together)
- [3. What NAT Is](#3-what-nat-is)
- [4. Why NAT Is Used](#4-why-nat-is-used)
- [5. Private and Public IP Addresses](#5-private-and-public-ip-addresses)
- [6. Static NAT](#6-static-nat)
- [7. Dynamic NAT](#7-dynamic-nat)
- [8. PAT / NAPT](#8-pat--napt)
- [9. SNAT and DNAT](#9-snat-and-dnat)
- [10. NAT Table and Translation Entries](#10-nat-table-and-translation-entries)
- [11. NAT Example: Home Internet](#11-nat-example-home-internet)
- [12. NAT Example: Publishing an Internal Server](#12-nat-example-publishing-an-internal-server)
- [13. NAT Advantages](#13-nat-advantages)
- [14. NAT Limitations](#14-nat-limitations)
- [15. Carrier-Grade NAT](#15-carrier-grade-nat)
- [16. What ACLs Are](#16-what-acls-are)
- [17. Why ACLs Are Used](#17-why-acls-are-used)
- [18. Standard ACLs](#18-standard-acls)
- [19. Extended ACLs](#19-extended-acls)
- [20. ACL Direction: Inbound vs Outbound](#20-acl-direction-inbound-vs-outbound)
- [21. ACL Rule Order](#21-acl-rule-order)
- [22. Implicit Deny](#22-implicit-deny)
- [23. Wildcard Masks in ACLs](#23-wildcard-masks-in-acls)
- [24. Common ACL Examples](#24-common-acl-examples)
- [25. What a Firewall Is](#25-what-a-firewall-is)
- [26. Stateless Firewalls](#26-stateless-firewalls)
- [27. Stateful Firewalls](#27-stateful-firewalls)
- [28. Next-Generation Firewalls](#28-next-generation-firewalls)
- [29. Firewall Policies](#29-firewall-policies)
- [30. Security Zones](#30-security-zones)
- [31. Default Deny vs Default Allow](#31-default-deny-vs-default-allow)
- [32. Firewall Logging](#32-firewall-logging)
- [33. What a VPN Is](#33-what-a-vpn-is)
- [34. Why VPNs Are Used](#34-why-vpns-are-used)
- [35. Site-to-Site VPN](#35-site-to-site-vpn)
- [36. Remote Access VPN](#36-remote-access-vpn)
- [37. SSL VPN](#37-ssl-vpn)
- [38. IPsec VPN](#38-ipsec-vpn)
- [39. GRE Tunnels](#39-gre-tunnels)
- [40. IPsec Terms](#40-ipsec-terms)
- [41. VPN Full Tunnel vs Split Tunnel](#41-vpn-full-tunnel-vs-split-tunnel)
- [42. NAT and VPN Together](#42-nat-and-vpn-together)
- [43. Common Ports and Protocols](#43-common-ports-and-protocols)
- [44. Troubleshooting NAT](#44-troubleshooting-nat)
- [45. Troubleshooting ACLs](#45-troubleshooting-acls)
- [46. Troubleshooting Firewalls](#46-troubleshooting-firewalls)
- [47. Troubleshooting VPNs](#47-troubleshooting-vpns)
- [48. Useful Commands](#48-useful-commands)
- [49. Best Practices](#49-best-practices)
- [50. Quick Summary](#50-quick-summary)

---

## 1. Big Picture

These four topics are closely related, but they do different jobs.

| Topic | Main Purpose |
|---|---|
| NAT | Changes IP address or port information in packets |
| ACL | Allows or denies traffic based on rules |
| Firewall | Enforces security policy between networks or zones |
| VPN | Creates a secure tunnel over an untrusted network |

A simple way to remember them:

- **NAT** changes addresses.
- **ACLs** filter traffic.
- **Firewalls** enforce security decisions.
- **VPNs** securely connect users or networks.

---

## 2. Why These Topics Are Grouped Together

In real networks, these technologies often work together.

Example:

A remote user connects to a company VPN. The VPN gateway authenticates the user. A firewall policy decides what the user can access. NAT may or may not translate the user's traffic. ACLs may filter traffic at routers, firewalls, or interfaces.

Another example:

An internal web server needs to be reachable from the Internet. DNAT or port forwarding sends public traffic to the internal server. Firewall rules allow only the required ports. ACLs may block unwanted sources.

Because these features affect traffic flow, troubleshooting often requires checking all of them together.

---

## 3. What NAT Is

NAT stands for **Network Address Translation**.

NAT changes IP address information in packets as they pass through a router, firewall, or gateway.

Most commonly, NAT is used to let private IP addresses access the Internet through one or more public IP addresses.

### Simple example

A laptop has a private IP:

```text
192.168.1.50
```

It accesses the Internet through a router with a public IP:

```text
203.0.113.10
```

The router changes the source address from:

```text
192.168.1.50
```

to:

```text
203.0.113.10
```

The Internet server sees the public IP, not the private IP.

---

## 4. Why NAT Is Used

NAT is used for several reasons.

### IPv4 address conservation

There are not enough public IPv4 addresses for every device. NAT allows many private devices to share one public IPv4 address.

### Hiding internal address structure

NAT hides internal private addresses from external networks.

This is not a complete security solution, but it does avoid exposing the internal addressing plan directly.

### Internet access for private networks

Private IP ranges cannot be routed on the public Internet. NAT lets private hosts communicate with Internet services.

### Publishing internal services

NAT can make an internal service reachable through a public address.

Example:

```text
Public IP: 203.0.113.20:443
Internal server: 10.10.10.50:443
```

### Overlapping networks

NAT can sometimes help when two networks use overlapping IP ranges, although this can become complex.

---

## 5. Private and Public IP Addresses

NAT is most commonly used with private IPv4 ranges.

### Private IPv4 ranges

| Range | CIDR |
|---|---|
| `10.0.0.0 - 10.255.255.255` | `10.0.0.0/8` |
| `172.16.0.0 - 172.31.255.255` | `172.16.0.0/12` |
| `192.168.0.0 - 192.168.255.255` | `192.168.0.0/16` |

These addresses are used inside private networks.

They are not routed globally on the public Internet.

### Public IP addresses

Public IP addresses are globally routable on the Internet.

A public IP address can be assigned by:

- ISP
- Cloud provider
- Hosting provider
- Organization with its own IP allocation

---

## 6. Static NAT

Static NAT maps one private IP address to one public IP address.

It is a one-to-one translation.

### Example

```text
Inside private IP: 10.10.10.10
Outside public IP: 203.0.113.10
```

Traffic from `10.10.10.10` appears on the Internet as `203.0.113.10`.

Traffic sent to `203.0.113.10` can be translated back to `10.10.10.10` if the firewall policy allows it.

### When static NAT is used

Static NAT is often used for:

- Public-facing servers
- Mail servers
- VPN gateways
- Systems that need a fixed public identity
- Partner connectivity

### Advantage

The mapping is predictable.

### Disadvantage

It consumes one public IP address per internal host.

---

## 7. Dynamic NAT

Dynamic NAT maps private addresses to a pool of public addresses.

Unlike static NAT, the mapping is not permanently tied to one internal host.

### Example

Private network:

```text
10.10.10.0/24
```

Public NAT pool:

```text
203.0.113.100 - 203.0.113.110
```

When an internal host starts a connection, the NAT device assigns one available public IP from the pool.

### When dynamic NAT is used

Dynamic NAT is used when:

- You have multiple public IPs
- You do not need fixed mappings for every internal host
- You want outbound translation without port overload

### Limitation

If the pool runs out of public IP addresses, new connections may fail.

For that reason, PAT is more common in many networks.

---

## 8. PAT / NAPT

PAT stands for **Port Address Translation**.

It is also called **NAPT**, which means **Network Address and Port Translation**.

PAT allows many internal devices to share one public IP address by using different port numbers.

### Example

| Internal Host | Internal Source | Translated Source |
|---|---|---|
| Laptop | `192.168.1.10:50100` | `203.0.113.10:40001` |
| Phone | `192.168.1.11:50101` | `203.0.113.10:40002` |
| Printer | `192.168.1.12:50102` | `203.0.113.10:40003` |

All devices appear to use the same public IP:

```text
203.0.113.10
```

But the NAT device tracks each connection using port numbers.

### Where PAT is used

PAT is used in:

- Home routers
- Small business firewalls
- Enterprise Internet edges
- Cloud NAT gateways
- Guest Wi-Fi networks

### Why PAT is so common

PAT saves public IPv4 addresses very efficiently.

One public IP can support many internal connections.

---

## 9. SNAT and DNAT

SNAT and DNAT describe the direction of the translation.

### SNAT

SNAT means **Source NAT**.

It changes the source address of a packet.

This is common for outbound Internet access.

Example before SNAT:

```text
Source: 10.10.10.50
Destination: 8.8.8.8
```

Example after SNAT:

```text
Source: 203.0.113.10
Destination: 8.8.8.8
```

### DNAT

DNAT means **Destination NAT**.

It changes the destination address of a packet.

This is common when publishing an internal service to external users.

Example before DNAT:

```text
Source: Internet client
Destination: 203.0.113.20:443
```

Example after DNAT:

```text
Source: Internet client
Destination: 10.10.10.50:443
```

### Simple memory trick

- **SNAT** changes where traffic appears to come from.
- **DNAT** changes where traffic is going.

---

## 10. NAT Table and Translation Entries

A NAT device keeps a translation table.

This table tracks active translations.

### Example NAT table

| Inside Local | Inside Global | Destination |
|---|---|---|
| `192.168.1.10:50100` | `203.0.113.10:40001` | `93.184.216.34:443` |
| `192.168.1.11:50101` | `203.0.113.10:40002` | `142.250.190.14:443` |

The NAT device uses this table to send return traffic back to the correct internal device.

Without this tracking, return traffic would not know which private host should receive the response.

---

## 11. NAT Example: Home Internet

A home network usually looks like this:

```text
Laptop: 192.168.1.10
Phone: 192.168.1.11
TV: 192.168.1.12
Router public IP: 203.0.113.10
```

When all devices access the Internet, the router translates their private addresses to the public IP.

From the Internet's perspective, all traffic appears to come from:

```text
203.0.113.10
```

The router uses port translations to keep each connection separate.

This is PAT.

---

## 12. NAT Example: Publishing an Internal Server

Suppose a company has an internal web server:

```text
10.10.10.50
```

The company wants external users to access it through:

```text
203.0.113.50
```

A firewall can translate:

```text
203.0.113.50:443 -> 10.10.10.50:443
```

This is DNAT or port forwarding.

However, NAT alone is not enough. The firewall policy must also allow the traffic.

A complete setup usually needs:

- Public DNS record
- DNAT rule
- Firewall allow rule
- Server listening on the correct port
- Correct return route
- Valid TLS certificate if HTTPS is used

---

## 13. NAT Advantages

NAT has several advantages.

### Saves IPv4 addresses

This is the biggest reason NAT became common.

### Allows private addressing internally

Organizations can use private IP ranges inside their networks.

### Hides internal IP addresses

External systems usually do not see the private source IP.

### Helps with migrations

NAT can help during network migrations, cloud moves, mergers, and overlapping address situations.

### Enables simple Internet sharing

Home and small business networks depend heavily on NAT for shared Internet access.

---

## 14. NAT Limitations

NAT is useful, but it also creates problems.

### Breaks pure end-to-end addressing

The original source or destination address is changed.

This can make some protocols harder to use.

### Makes troubleshooting harder

Logs may show translated addresses instead of original addresses.

You may need NAT logs to identify the real internal host.

### Can affect applications

Some protocols embed IP address information inside the application payload. NAT may not handle that cleanly without extra help.

### Requires state tracking

PAT depends on connection tracking. If the NAT table is full or times out too quickly, sessions may fail.

### Not a security solution by itself

NAT hides internal addresses, but it should not be treated as a full firewall.

Security policy still needs to be enforced with firewall rules, segmentation, patching, and monitoring.

---

## 15. Carrier-Grade NAT

Carrier-Grade NAT, or **CGNAT**, is used by ISPs to share public IPv4 addresses among many customers.

A common CGNAT range is:

```text
100.64.0.0/10
```

### Why CGNAT exists

ISPs use CGNAT because public IPv4 addresses are limited.

### Common effects of CGNAT

CGNAT can affect:

- Port forwarding
- Hosting services at home
- Some gaming services
- Peer-to-peer applications
- Remote access
- VPN behavior in some cases

If your router WAN address is in `100.64.0.0/10`, you may be behind CGNAT.

---

## 16. What ACLs Are

ACL stands for **Access Control List**.

An ACL is a list of rules used to permit or deny traffic.

ACLs are often used on routers, switches, firewalls, and operating systems.

### Simple ACL idea

```text
Allow traffic from trusted network
Deny traffic from untrusted network
```

ACLs usually inspect packet information such as:

- Source IP
- Destination IP
- Protocol
- Source port
- Destination port
- Direction

The exact fields depend on the ACL type and platform.

---

## 17. Why ACLs Are Used

ACLs are used to control traffic.

Common uses:

- Block unwanted traffic
- Permit only required traffic
- Limit management access
- Restrict inter-VLAN communication
- Control routing updates
- Define interesting traffic for VPNs
- Match traffic for NAT rules
- Protect infrastructure devices

ACLs can be used for security, traffic selection, and network control.

---

## 18. Standard ACLs

A standard ACL filters mainly by source IP address.

### Example idea

```text
Permit source network 10.10.10.0/24
Deny all other sources
```

Standard ACLs are simple.

They are useful when you only care about where traffic comes from.

### Limitation

A standard ACL does not normally match destination ports or application protocols.

Because of that, it is less precise than an extended ACL.

---

## 19. Extended ACLs

An extended ACL can match more fields.

It can filter based on:

- Source IP
- Destination IP
- Protocol
- Source port
- Destination port
- ICMP type

### Example idea

```text
Allow 10.10.10.0/24 to access 10.20.20.10 on TCP 443
Deny all other traffic to 10.20.20.10
```

Extended ACLs are more flexible and more common for detailed traffic control.

### Common use cases

- Allow web traffic only
- Block SSH except from admin subnet
- Permit DNS only to approved DNS servers
- Restrict user VLANs from server VLANs
- Protect management interfaces

---

## 20. ACL Direction: Inbound vs Outbound

ACLs are usually applied in a direction.

### Inbound

Inbound means the ACL is checked as traffic enters an interface.

### Outbound

Outbound means the ACL is checked as traffic leaves an interface.

### Why direction matters

If the ACL is placed in the wrong direction, it may not match the traffic you expect.

Example:

A packet enters a router from VLAN 10 and leaves toward VLAN 20.

You could filter it:

- Inbound on the VLAN 10 interface
- Outbound on the VLAN 20 interface

Both may work, but the best choice depends on design.

### General design idea

Filtering closer to the source can stop unwanted traffic earlier.

Filtering closer to the destination can centralize protection for a sensitive network.

---

## 21. ACL Rule Order

ACLs are usually processed from top to bottom.

The first matching rule wins.

### Example

```text
1. Deny 10.10.10.50 to 10.20.20.10 TCP 443
2. Permit 10.10.10.0/24 to 10.20.20.10 TCP 443
3. Deny all
```

Traffic from `10.10.10.50` is denied because it matches rule 1 first.

Other hosts in `10.10.10.0/24` are allowed by rule 2.

### Important point

Put specific rules before general rules.

Bad order can cause unexpected results.

---

## 22. Implicit Deny

Many ACL systems have an implicit deny at the end.

That means if traffic does not match any permit rule, it is denied.

### Example

If the ACL says:

```text
Permit 10.10.10.0/24 to 10.20.20.10 TCP 443
```

but does not explicitly permit anything else, other traffic may be denied automatically.

### Practical warning

When applying ACLs, always think about what happens to traffic that is not listed.

Many outages happen because someone forgets the implicit deny.

---

## 23. Wildcard Masks in ACLs

Some network devices, especially Cisco-style configurations, use wildcard masks in ACLs.

A wildcard mask is the inverse of a subnet mask.

### Example

Subnet mask:

```text
255.255.255.0
```

Wildcard mask:

```text
0.0.0.255
```

### Common examples

| Network | Subnet Mask | Wildcard Mask |
|---|---|---|
| `/24` | `255.255.255.0` | `0.0.0.255` |
| `/25` | `255.255.255.128` | `0.0.0.127` |
| `/26` | `255.255.255.192` | `0.0.0.63` |
| `/30` | `255.255.255.252` | `0.0.0.3` |
| Single host | `255.255.255.255` | `0.0.0.0` |

### Simple meaning

In a wildcard mask:

- `0` means match exactly
- `255` means ignore this part

---

## 24. Common ACL Examples

These are conceptual examples, not tied to one vendor syntax.

### Allow HTTPS to a web server

```text
Permit TCP from any source to 10.10.10.50 destination port 443
Deny all other inbound traffic to 10.10.10.50
```

### Allow SSH only from admin subnet

```text
Permit TCP from 10.10.99.0/24 to 10.10.10.10 destination port 22
Deny TCP from any source to 10.10.10.10 destination port 22
```

### Block guest network from internal servers

```text
Deny IP from 10.50.50.0/24 to 10.10.0.0/16
Permit IP from 10.50.50.0/24 to Internet
```

### Allow DNS only to approved DNS servers

```text
Permit UDP from user VLAN to 10.10.1.53 destination port 53
Permit TCP from user VLAN to 10.10.1.53 destination port 53
Deny UDP from user VLAN to any destination port 53
Deny TCP from user VLAN to any destination port 53
```

---

## 25. What a Firewall Is

A firewall is a device or software system that enforces security policy for network traffic.

Firewalls can exist as:

- Physical appliances
- Virtual appliances
- Cloud firewalls
- Host-based firewalls
- Container or workload firewalls
- Firewall features inside routers

### Firewall purpose

A firewall decides whether traffic should be allowed or blocked.

It may inspect:

- Source IP
- Destination IP
- Protocol
- Port
- Connection state
- User identity
- Application
- URL category
- Threat signatures
- TLS information

The exact capabilities depend on the firewall type.

---

## 26. Stateless Firewalls

A stateless firewall checks each packet independently.

It does not remember previous packets in the connection.

### Example

A stateless rule may allow:

```text
Source: 10.10.10.0/24
Destination: Any
Protocol: TCP
Destination port: 443
```

But it may also need a separate rule for return traffic because it does not track the connection state.

### Advantage

Stateless filtering can be fast and simple.

### Disadvantage

It is less intelligent than stateful filtering.

It can be harder to manage for complex traffic flows.

---

## 27. Stateful Firewalls

A stateful firewall tracks connection state.

It understands whether traffic is part of an existing connection.

### Example

If an internal client opens an HTTPS connection to the Internet, the firewall allows the return traffic automatically because it belongs to an established session.

### Stateful inspection tracks things like:

- New sessions
- Established sessions
- Return traffic
- TCP state
- UDP pseudo-sessions
- Session timeouts

### Advantage

Stateful firewalls are easier and safer for most modern network designs.

### Disadvantage

They require resources to track sessions.

If the session table is overloaded, traffic may be affected.

---

## 28. Next-Generation Firewalls

A next-generation firewall, or **NGFW**, goes beyond basic IP and port filtering.

An NGFW may include:

- Application identification
- User-based policy
- URL filtering
- Intrusion prevention
- Malware detection
- SSL/TLS inspection
- Threat intelligence
- Geo-IP controls
- Advanced logging

### Example

A traditional firewall might allow:

```text
TCP 443 to the Internet
```

An NGFW may allow:

```text
Microsoft 365 and GitHub over HTTPS, but block unknown file-sharing applications.
```

### Important point

NGFW features are powerful, but they need careful tuning. Bad policy design can block legitimate applications or create blind spots.

---

## 29. Firewall Policies

A firewall policy is a rule or set of rules that controls traffic.

A firewall rule usually includes:

- Source zone
- Destination zone
- Source address
- Destination address
- Application or service
- Action
- Logging setting
- Security profile

### Example policy

```text
Source zone: Users
Destination zone: Internet
Source: 10.10.20.0/24
Destination: Any
Service: HTTPS
Action: Allow
Log: Yes
```

### Policy order

Like ACLs, firewall rules are often processed from top to bottom.

The first matching rule may be used.

Always understand how your specific firewall platform processes rules.

---

## 30. Security Zones

Security zones group interfaces or networks by trust level or function.

Common zones:

- Inside
- Outside
- DMZ
- Guest
- VPN
- Management
- Servers
- Users

### Example

```text
Inside -> Outside: Allow web browsing
Outside -> Inside: Deny by default
Outside -> DMZ: Allow HTTPS to public web server
Users -> Servers: Allow only required application ports
Guest -> Inside: Deny
```

Zones make firewall policy easier to understand and manage.

---

## 31. Default Deny vs Default Allow

### Default allow

A default allow policy permits traffic unless it is blocked.

This is easier at first but less secure.

### Default deny

A default deny policy blocks traffic unless it is specifically allowed.

This is more secure and is common in mature environments.

### Best practice

For sensitive networks, use default deny and allow only what is required.

This is also called least privilege.

---

## 32. Firewall Logging

Firewall logs are essential for troubleshooting and security.

Useful log fields include:

- Timestamp
- Source IP
- Destination IP
- Source port
- Destination port
- Protocol
- Action
- Rule name
- Interface or zone
- Application
- User identity
- NAT translation details

### Why logs matter

Logs help answer questions like:

- Was the traffic allowed or denied?
- Which rule matched?
- Did NAT happen?
- Which user or host generated the traffic?
- Is the application using the expected port?

### Practical advice

Log denies and important allows.

But avoid logging too much unnecessary traffic because excessive logs can become noisy and expensive.

---

## 33. What a VPN Is

VPN stands for **Virtual Private Network**.

A VPN creates a secure tunnel over another network, usually the Internet.

VPNs are used to connect private networks or remote users safely.

### Simple idea

Instead of sending private traffic directly over the Internet, a VPN encrypts the traffic and sends it through a tunnel.

### VPNs commonly provide:

- Encryption
- Authentication
- Integrity checking
- Secure remote access
- Private network connectivity

---

## 34. Why VPNs Are Used

VPNs are used for several reasons.

### Remote access

Employees can connect to company resources from outside the office.

### Site-to-site connectivity

Two offices can connect securely over the Internet.

### Cloud connectivity

A company can connect its on-premises network to a cloud network.

### Partner connectivity

Two organizations can share selected services through a controlled tunnel.

### Secure traffic over untrusted networks

VPNs protect traffic from being read or modified while crossing untrusted networks.

---

## 35. Site-to-Site VPN

A site-to-site VPN connects two networks.

Example:

```text
Office A network: 10.10.0.0/16
Office B network: 10.20.0.0/16
```

A VPN tunnel allows hosts in Office A to communicate with hosts in Office B securely.

### Common use cases

- Branch office to headquarters
- Data center to cloud
- Company to partner network
- Disaster recovery site

### Typical devices

Site-to-site VPNs are often built between:

- Firewalls
- Routers
- VPN gateways
- Cloud VPN services

---

## 36. Remote Access VPN

A remote access VPN connects an individual user to a private network.

Example:

A laptop at home connects to the company VPN.

After connecting, the laptop may access internal resources such as:

- File servers
- Internal websites
- Admin systems
- Databases
- Remote desktop gateways

### Authentication

Remote access VPNs often use:

- Username and password
- Multi-factor authentication
- Certificates
- Device posture checks
- Identity provider integration

### Security note

Remote access VPNs should be protected strongly because they provide access into the internal environment.

---

## 37. SSL VPN

An SSL VPN uses TLS to secure remote access.

It often works through a browser or VPN client.

### Common use

SSL VPNs are common for remote users because they can work over HTTPS-like traffic.

### Advantages

- Often easier for users
- Can work through many networks
- Supports portal-based access or full tunnel clients

### Things to check

- Certificate validity
- User authentication
- MFA
- Allowed resources
- Client compatibility
- Firewall policies after connection

---

## 38. IPsec VPN

IPsec is a suite of protocols used to secure IP traffic.

IPsec is common for site-to-site VPNs and also used for some remote access VPNs.

### IPsec can provide:

- Encryption
- Authentication
- Integrity
- Anti-replay protection

### Common IPsec uses

- Site-to-site VPN
- Cloud VPN
- Branch connectivity
- Router-to-router tunnels
- Firewall-to-firewall tunnels

### IPsec phases

Many IPsec VPNs are discussed in two major parts:

1. IKE negotiation
2. IPsec tunnel establishment

The exact terms vary by version and vendor, but the basic idea is that peers first agree on how to protect the tunnel, then they send encrypted traffic.

---

## 39. GRE Tunnels

GRE stands for **Generic Routing Encapsulation**.

GRE creates a tunnel between two endpoints.

### Important point

GRE by itself does not encrypt traffic.

It encapsulates traffic, but it is not a security protocol by itself.

### Common use

GRE is often used when you need to carry routing protocols or non-standard traffic over a tunnel.

GRE may be combined with IPsec to add encryption.

### GRE protocol number

GRE uses IP protocol number:

```text
47
```

This is not TCP port 47 or UDP port 47. It is an IP protocol number.

---

## 40. IPsec Terms

Here are common IPsec terms.

| Term | Meaning |
|---|---|
| AH | Authentication Header |
| ESP | Encapsulating Security Payload |
| IKE | Internet Key Exchange |
| NAT-T | NAT Traversal |
| SA | Security Association |
| PSK | Pre-Shared Key |
| PFS | Perfect Forward Secrecy |
| Proposal | Set of crypto parameters |
| Transform set | Encryption and integrity settings |
| Tunnel mode | Protects the full original IP packet |
| Transport mode | Protects the payload of the IP packet |

### AH

AH provides authentication and integrity, but it does not encrypt the payload.

It is less common than ESP in many modern VPN designs.

### ESP

ESP provides encryption and can also provide authentication and integrity.

ESP is very common in IPsec VPNs.

### IKE

IKE negotiates keys and security parameters between VPN peers.

Common versions:

- IKEv1
- IKEv2

IKEv2 is generally preferred in modern deployments when supported.

### NAT-T

NAT-T allows IPsec to work through NAT.

It commonly uses:

```text
UDP 4500
```

---

## 41. VPN Full Tunnel vs Split Tunnel

Remote access VPNs can use full tunnel or split tunnel designs.

### Full tunnel

In a full tunnel design, all user traffic goes through the VPN.

This includes:

- Internal company traffic
- Internet browsing
- SaaS traffic

### Advantages

- Centralized security inspection
- Consistent policy enforcement
- Easier traffic monitoring

### Disadvantages

- More load on VPN infrastructure
- More bandwidth usage
- May add latency for Internet traffic

### Split tunnel

In a split tunnel design, only selected traffic goes through the VPN.

Example:

```text
10.0.0.0/8 goes through VPN
Internet traffic goes directly out local connection
```

### Advantages

- Less VPN bandwidth usage
- Better performance for Internet traffic
- Lower load on VPN gateways

### Disadvantages

- Less centralized visibility
- More complex endpoint security considerations
- Policy must be designed carefully

---

## 42. NAT and VPN Together

NAT and VPN can interact in important ways.

### NAT exemption

In many site-to-site VPNs, traffic between internal networks should not be NATed.

Example:

```text
Local network: 10.10.0.0/16
Remote network: 10.20.0.0/16
```

Traffic from `10.10.0.0/16` to `10.20.0.0/16` may need a NAT exemption rule.

### NAT before VPN problem

If traffic is translated before it enters the VPN, the remote side may see the wrong source network.

This can break tunnel policy matching.

### Overlapping networks

If both sides use the same IP range, NAT may be required.

Example:

```text
Company A: 10.10.0.0/16
Company B: 10.10.0.0/16
```

This creates overlapping addressing.

A NAT design may be needed so each side sees a translated range.

---

## 43. Common Ports and Protocols

These ports and protocols are commonly related to NAT, firewalls, and VPNs.

| Port / Protocol | Use |
|---|---|
| `500/UDP` | IKE / IPsec negotiation |
| `4500/UDP` | IPsec NAT-T |
| `ESP protocol 50` | IPsec ESP |
| `AH protocol 51` | IPsec AH |
| `443/TCP` | HTTPS / SSL VPN |
| `1194/UDP or TCP` | OpenVPN default |
| `1701/UDP` | L2TP |
| `1723/TCP` | PPTP control |
| `GRE protocol 47` | GRE / PPTP data |
| `22/TCP` | SSH management |
| `3389/TCP` | RDP |
| `53/UDP and TCP` | DNS |

### Important distinction

Some items are ports, and some are IP protocol numbers.

Examples:

- `UDP 500` is a port.
- `ESP protocol 50` is not a port.
- `GRE protocol 47` is not a port.

This matters when creating firewall rules.

---

## 44. Troubleshooting NAT

When NAT does not work, check the full traffic path.

### Step 1: Confirm the original traffic

Check the source, destination, and port before NAT.

Example:

```text
Source: 10.10.10.50
Destination: 8.8.8.8
Port: 443
```

### Step 2: Confirm the expected translation

What should the translated packet look like?

Example:

```text
Source after NAT: 203.0.113.10
Destination: 8.8.8.8
Port: 443
```

### Step 3: Check NAT rule order

Like firewall policies, NAT rules may be order-sensitive.

A more general NAT rule may match traffic before a specific rule.

### Step 4: Check NAT table

Look for active translations.

If no translation appears, the NAT rule may not be matching.

### Step 5: Check firewall policy

NAT and firewall policy are separate on many platforms.

A NAT rule may translate traffic, but a firewall rule must still allow it.

### Step 6: Check return routing

Return traffic must come back through the NAT device.

Asymmetric routing can break NAT.

### Step 7: Check overlapping addresses

If the same subnet exists on both sides, traffic may route incorrectly.

---

## 45. Troubleshooting ACLs

ACL issues are common and can be subtle.

### Step 1: Identify the traffic

Know the exact:

- Source IP
- Destination IP
- Protocol
- Source port
- Destination port
- Direction

### Step 2: Check ACL placement

Ask:

- Is the ACL applied to the correct interface?
- Is it inbound or outbound?
- Is the traffic actually passing that interface?

### Step 3: Check rule order

A deny rule above a permit rule may block traffic unexpectedly.

### Step 4: Check implicit deny

If traffic is not explicitly allowed, it may be denied.

### Step 5: Check counters or logs

Many platforms show hit counts for ACL rules.

If a rule counter increases, traffic is matching that rule.

### Step 6: Watch for object mistakes

In firewall-style ACLs, address objects and service objects may be wrong.

Example:

- Wrong subnet mask
- Wrong port
- Wrong host object
- Old IP address

---

## 46. Troubleshooting Firewalls

Firewall troubleshooting should be methodical.

### Step 1: Define the flow

Write the flow clearly:

```text
Source: 10.10.20.15
Destination: 10.30.40.25
Protocol: TCP
Destination port: 443
```

### Step 2: Check routing

The firewall must know how to reach both sides.

### Step 3: Check NAT

Determine whether NAT should happen.

Ask:

- Is source NAT required?
- Is destination NAT required?
- Is NAT exemption required?

### Step 4: Check security policy

Find the rule that should match.

Check:

- Source zone
- Destination zone
- Source address
- Destination address
- Application
- Service
- User
- Action

### Step 5: Check logs

Firewall logs usually show allow or deny decisions.

Look for:

- Rule name
- Deny reason
- NAT translation
- Application detected
- Session end reason

### Step 6: Check return path

Traffic must return through the expected path.

Asymmetric routing can cause stateful firewalls to drop traffic.

---

## 47. Troubleshooting VPNs

VPN troubleshooting depends on the VPN type, but the same general method helps.

### Step 1: Check peer reachability

Can the VPN peers reach each other over the Internet or transport network?

### Step 2: Check ports and protocols

For IPsec, check:

```text
500/UDP
4500/UDP
ESP protocol 50 if NAT-T is not used
```

For SSL VPN, check:

```text
443/TCP
```

### Step 3: Check authentication

Check:

- Pre-shared key
- Certificates
- User credentials
- MFA
- Identity provider
- Clock/time synchronization

### Step 4: Check crypto settings

For IPsec, both sides must agree on crypto parameters.

Common mismatches:

- IKE version
- Encryption algorithm
- Integrity algorithm
- Diffie-Hellman group
- Lifetime
- PFS setting

### Step 5: Check local and remote networks

The tunnel policy must match the traffic.

Example:

```text
Local: 10.10.0.0/16
Remote: 10.20.0.0/16
```

If one side defines `/24` and the other defines `/16`, the tunnel may not work as expected.

### Step 6: Check NAT exemption

Make sure VPN traffic is not accidentally NATed before entering the tunnel.

### Step 7: Check firewall rules after tunnel setup

A tunnel can be up, but traffic can still be blocked by firewall policy.

### Step 8: Check routing

The device must route remote network traffic into the VPN.

### Step 9: Check logs

VPN logs usually show negotiation errors and policy mismatches.

---

## 48. Useful Commands

Commands vary by vendor and operating system, but these are useful ideas.

### Windows

Check IP configuration:

```powershell
ipconfig /all
```

Test a TCP port:

```powershell
Test-NetConnection example.com -Port 443
```

Show routes:

```powershell
route print
```

Check VPN adapter information:

```powershell
Get-NetAdapter
Get-NetIPConfiguration
```

### Linux

Show addresses:

```bash
ip addr
```

Show routes:

```bash
ip route
```

Test TCP connectivity:

```bash
nc -vz example.com 443
```

Capture traffic:

```bash
tcpdump -i eth0 host 10.10.10.50
```

Show listening ports:

```bash
ss -tulpn
```

### Cisco-style examples

Show access lists:

```text
show access-lists
```

Show NAT translations:

```text
show ip nat translations
```

Show NAT statistics:

```text
show ip nat statistics
```

Show crypto IPsec security associations:

```text
show crypto ipsec sa
```

Show crypto IKE security associations:

```text
show crypto ikev2 sa
```

### Firewall platform checks

On most firewalls, look for:

- Session table
- Traffic log
- NAT log
- Packet capture tool
- Policy match tool
- Route lookup tool
- VPN logs

---

## 49. Best Practices

### NAT best practices

- Document NAT rules clearly.
- Keep NAT rule order clean.
- Avoid unnecessary NAT.
- Use NAT exemption for VPN traffic when needed.
- Monitor NAT table usage.
- Log important translations when possible.

### ACL best practices

- Put specific rules before general rules.
- Use clear descriptions.
- Avoid overly broad permits.
- Remember the implicit deny.
- Remove old unused rules.
- Test changes carefully.

### Firewall best practices

- Use default deny for sensitive networks.
- Allow only required traffic.
- Use zones to organize policy.
- Log important allows and denies.
- Review rules regularly.
- Avoid exposing management services publicly.
- Use strong authentication for admin access.
- Keep firewall firmware and software updated.

### VPN best practices

- Use strong encryption settings.
- Prefer modern VPN protocols and configurations.
- Use MFA for remote access VPNs.
- Avoid weak legacy VPN protocols.
- Document local and remote networks.
- Monitor tunnel health.
- Keep certificates valid.
- Review user access regularly.

### General security advice

- Segment networks by function and trust level.
- Use least privilege.
- Keep logs and review them.
- Monitor for unusual traffic.
- Test backup remote access methods.
- Keep diagrams updated.

---

## 50. Quick Summary

NAT, ACLs, firewalls, and VPNs are key tools for controlling and securing network traffic.

The most important points to remember are:

- **NAT** translates IP addresses or ports.
- **Static NAT** is one-to-one.
- **Dynamic NAT** uses a pool of addresses.
- **PAT / NAPT** lets many devices share one public IP using ports.
- **SNAT** changes the source address.
- **DNAT** changes the destination address.
- **ACLs** permit or deny traffic based on rules.
- **Standard ACLs** usually match source IP.
- **Extended ACLs** can match source, destination, protocol, and ports.
- ACL order matters.
- Many ACLs end with an implicit deny.
- **Firewalls** enforce security policy between networks or zones.
- **Stateless firewalls** check packets independently.
- **Stateful firewalls** track sessions.
- **NGFWs** can inspect applications, users, URLs, and threats.
- **VPNs** create secure tunnels over untrusted networks.
- **Site-to-site VPNs** connect networks.
- **Remote access VPNs** connect users.
- **IPsec** commonly uses IKE, ESP, and NAT-T.
- **NAT-T** commonly uses UDP `4500`.
- Troubleshooting should always check routing, NAT, policy, logs, and return traffic.

If you understand these topics well, you can troubleshoot many real-world connectivity and security problems much faster.
