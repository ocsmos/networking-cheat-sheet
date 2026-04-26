# 16. Network Design Concepts

Network design is the process of planning how a network should be built, organized, secured, scaled, and maintained. A good design is not only about making devices communicate. It is about making the network reliable, secure, understandable, and ready for growth.

This guide explains the most important network design concepts in a practical way. The goal is to understand how engineers think when they design real networks for homes, labs, offices, data centers, campuses, and cloud-connected environments.

---

## Table of Contents

- [1. What Network Design Means](#1-what-network-design-means)
- [2. Why Network Design Matters](#2-why-network-design-matters)
- [3. Design Goals](#3-design-goals)
- [4. Understanding Requirements](#4-understanding-requirements)
- [5. Physical Design vs Logical Design](#5-physical-design-vs-logical-design)
- [6. Network Topologies](#6-network-topologies)
- [7. Hierarchical Network Design](#7-hierarchical-network-design)
- [8. Core, Distribution, and Access Layers](#8-core-distribution-and-access-layers)
- [9. Small Network Design](#9-small-network-design)
- [10. Campus Network Design](#10-campus-network-design)
- [11. Data Center Network Design](#11-data-center-network-design)
- [12. Segmentation](#12-segmentation)
- [13. VLAN Design](#13-vlan-design)
- [14. Subnet Design](#14-subnet-design)
- [15. IP Address Planning](#15-ip-address-planning)
- [16. Routing Design](#16-routing-design)
- [17. Default Gateway Design](#17-default-gateway-design)
- [18. Redundancy](#18-redundancy)
- [19. High Availability](#19-high-availability)
- [20. Load Balancing](#20-load-balancing)
- [21. Failure Domains](#21-failure-domains)
- [22. Security Zones](#22-security-zones)
- [23. Firewall Placement](#23-firewall-placement)
- [24. Management Network Design](#24-management-network-design)
- [25. Wireless Design](#25-wireless-design)
- [26. Internet Edge Design](#26-internet-edge-design)
- [27. VPN and Remote Access Design](#27-vpn-and-remote-access-design)
- [28. Cloud and Hybrid Network Design](#28-cloud-and-hybrid-network-design)
- [29. DNS, DHCP, and Core Services](#29-dns-dhcp-and-core-services)
- [30. Monitoring and Logging](#30-monitoring-and-logging)
- [31. Capacity Planning](#31-capacity-planning)
- [32. Scalability](#32-scalability)
- [33. Performance Design](#33-performance-design)
- [34. QoS Design](#34-qos-design)
- [35. Documentation](#35-documentation)
- [36. Change Management](#36-change-management)
- [37. Common Design Mistakes](#37-common-design-mistakes)
- [38. Example Design: Small Office](#38-example-design-small-office)
- [39. Example Design: Medium Business](#39-example-design-medium-business)
- [40. Network Design Checklist](#40-network-design-checklist)
- [41. Quick Summary](#41-quick-summary)

---

## 1. What Network Design Means

Network design means planning how the network will work before or while it is being built.

It includes decisions such as:

- Which devices are needed
- Where switches, routers, firewalls, and access points should be placed
- How VLANs and subnets should be organized
- How traffic should move between networks
- How users should access internal and external services
- How the network should stay online during failures
- How the network should be secured
- How the network can grow later

A network can work without good design, but it usually becomes harder to troubleshoot, scale, and secure.

---

## 2. Why Network Design Matters

A good network design helps prevent problems before they happen.

Good design improves:

- Reliability
- Security
- Performance
- Troubleshooting
- Scalability
- Visibility
- Manageability
- User experience

### Bad design usually causes problems like:

- Random outages
- Hard-to-find loops
- Confusing VLANs
- Poor Wi-Fi coverage
- Weak firewall rules
- IP conflicts
- Routing problems
- No redundancy
- Poor documentation
- Slow troubleshooting

### Simple idea

A good network is not only one that works today.

A good network is one that can still be understood, repaired, secured, and expanded later.

---

## 3. Design Goals

Before designing a network, define the goals.

Common design goals include:

- Availability
- Security
- Performance
- Simplicity
- Scalability
- Cost control
- Ease of management
- Compliance
- Redundancy
- Fast troubleshooting

### Example

A home lab may prioritize learning and flexibility.

A hospital network may prioritize uptime, segmentation, and security.

A small office may prioritize cost and simplicity.

A data center may prioritize performance, redundancy, automation, and predictable traffic flow.

The best design depends on the environment.

---

## 4. Understanding Requirements

Network design should start with requirements.

Do not begin by choosing hardware first. Begin by understanding what the network must support.

### Questions to ask

- How many users will use the network?
- How many devices are expected?
- What applications are critical?
- Are there servers on-site?
- Is cloud access important?
- Is Wi-Fi the main access method?
- Are there voice or video systems?
- Are there guest users?
- Is remote access needed?
- What level of downtime is acceptable?
- Is compliance required?
- How much growth is expected?
- What is the budget?

### Example

A network for 20 users is very different from a network for 2,000 users.

A network that supports only web browsing is very different from a network that supports VoIP, video meetings, file servers, cameras, and production systems.

---

## 5. Physical Design vs Logical Design

Network design usually has two sides:

- Physical design
- Logical design

### Physical design

Physical design describes how the network is physically connected.

It includes:

- Switch locations
- Rack layout
- Cable paths
- Fiber links
- Patch panels
- Power
- UPS systems
- Device placement
- Access point locations
- Uplink connections

### Logical design

Logical design describes how the network behaves.

It includes:

- VLANs
- Subnets
- IP addressing
- Routing
- Firewall zones
- NAT
- VPNs
- DNS
- DHCP
- Access policies
- Management networks

### Example

Two switches may be physically connected with one cable, but logically they may carry many VLANs through a trunk.

Physical design shows the cable.

Logical design shows the VLANs and traffic paths.

---

## 6. Network Topologies

A topology describes how devices are connected.

### Star topology

In a star topology, devices connect to a central switch.

```text
PC1  \
PC2   \
PC3 ---- Switch ---- Router
PC4   /
PC5  /
```

This is common in small networks.

### Extended star topology

Multiple access switches connect back to a central or distribution switch.

```text
Access Switch 1 ----\
Access Switch 2 ----- Distribution/Core ---- Router/Firewall
Access Switch 3 ----/
```

This is common in offices and campuses.

### Mesh topology

In a mesh, devices have multiple paths between them.

Full mesh means every device connects to every other device.

Partial mesh means only some devices have multiple paths.

Mesh designs improve redundancy but increase complexity and cost.

### Ring topology

In a ring, devices are connected in a circular path.

This can provide redundancy, but it requires loop prevention or ring protection mechanisms.

### Point-to-point topology

A point-to-point link connects two devices directly.

Common examples:

- Router to router
- Switch to switch
- Firewall to router
- Site-to-site WAN link

---

## 7. Hierarchical Network Design

Hierarchical design means dividing the network into clear layers.

The classic model uses:

- Access layer
- Distribution layer
- Core layer

This model is useful because it creates structure.

Each layer has a specific job.

### Why hierarchical design is useful

It helps with:

- Scalability
- Easier troubleshooting
- Clear traffic flow
- Redundancy planning
- Policy enforcement
- Simpler documentation
- Better operational control

Not every network needs all three layers physically separated. Small networks may combine layers.

---

## 8. Core, Distribution, and Access Layers

### Access layer

The access layer is where end devices connect.

Examples:

- User PCs
- Phones
- Printers
- Cameras
- Wireless access points
- IoT devices

Access layer switches usually handle:

- VLAN assignment
- Port security
- PoE
- 802.1X
- Basic access control
- Connection to users and devices

### Distribution layer

The distribution layer sits between access and core.

It often handles:

- Inter-VLAN routing
- Policy enforcement
- ACLs
- Route summarization
- Redundancy
- Aggregation of access switches

### Core layer

The core layer is the fast backbone of the network.

It should be:

- Fast
- Reliable
- Simple
- Highly available

The core should avoid unnecessary complexity. Its main job is to move traffic quickly between major parts of the network.

### Simple diagram

```text
          Core
        /      \
Distribution  Distribution
    /   \        /   \
Access Access  Access Access
```

---

## 9. Small Network Design

A small network may not need separate access, distribution, and core layers.

A simple design may include:

- One router or firewall
- One switch
- One or more access points
- One Internet connection
- A few VLANs

### Example

```text
Internet
   |
Firewall/Router
   |
Switch
   |---- PCs
   |---- Printer
   |---- Access Point
   |---- NAS
```

### Good small network design still includes:

- Clear IP addressing
- Separate guest Wi-Fi
- Basic firewall rules
- Strong Wi-Fi security
- Backups for important devices
- Documentation
- Firmware updates

Small does not mean careless.

---

## 10. Campus Network Design

A campus network connects multiple areas in one organization.

Examples:

- Office buildings
- Schools
- Universities
- Hospitals
- Industrial sites

Campus design usually includes:

- Many access switches
- Multiple VLANs
- Wireless coverage
- Redundant uplinks
- Layer 3 routing between areas
- Central firewalls
- Network access control
- Monitoring and logging

### Common campus design goals

- Reliable user connectivity
- Good Wi-Fi coverage
- Simple VLAN structure
- Secure guest access
- Segmentation for sensitive systems
- Redundant paths
- Fast troubleshooting

### Common campus VLANs

| VLAN Type | Example Purpose |
|---|---|
| User VLAN | Employee devices |
| Voice VLAN | IP phones |
| Guest VLAN | Guest Wi-Fi |
| Server VLAN | Internal servers |
| Printer VLAN | Printers |
| Camera VLAN | Security cameras |
| Management VLAN | Network device management |
| IoT VLAN | IoT and building systems |

---

## 11. Data Center Network Design

Data center networks are designed for servers, storage, virtualization, and application traffic.

They often need:

- High bandwidth
- Low latency
- High redundancy
- Predictable paths
- Strong segmentation
- Automation
- Monitoring
- Scalable addressing

### Traditional design

Older data centers often used:

- Access layer
- Aggregation layer
- Core layer

### Spine-leaf design

Modern data centers often use spine-leaf design.

```text
        Spine 1      Spine 2
        /  |  \      /  |  \
     Leaf Leaf Leaf Leaf Leaf
      |    |    |    |    |
    Servers and workloads
```

### Why spine-leaf is popular

Spine-leaf provides:

- Predictable latency
- Equal-cost paths
- Horizontal scalability
- Better east-west traffic handling
- Easier expansion

### East-west vs north-south traffic

North-south traffic moves in and out of the data center.

Example:

```text
User -> Application -> Internet
```

East-west traffic moves between systems inside the data center.

Example:

```text
Web server -> App server -> Database server
```

Modern applications often create a lot of east-west traffic.

---

## 12. Segmentation

Segmentation means dividing the network into smaller parts.

The goal is to control traffic, reduce risk, and improve organization.

### Common segmentation methods

- VLANs
- Subnets
- Firewalls
- VRFs
- ACLs
- Security groups
- Zero trust policies
- Microsegmentation

### Why segmentation is important

Segmentation helps with:

- Security
- Broadcast control
- Compliance
- Troubleshooting
- Performance management
- Limiting damage during an incident

### Example

A printer does not need direct access to every server.

A guest device should not access internal file shares.

A camera should not have the same network access as an employee laptop.

Segmentation helps enforce those boundaries.

---

## 13. VLAN Design

VLANs separate Layer 2 broadcast domains.

A VLAN is often mapped to one subnet.

### Example VLAN design

| VLAN | Name | Subnet |
|---:|---|---|
| 10 | Users | `192.168.10.0/24` |
| 20 | Voice | `192.168.20.0/24` |
| 30 | Printers | `192.168.30.0/24` |
| 40 | Guests | `192.168.40.0/24` |
| 50 | Servers | `192.168.50.0/24` |
| 99 | Management | `192.168.99.0/24` |

### Good VLAN design principles

- Use clear names
- Keep numbering consistent
- Avoid using VLAN 1 for normal user traffic
- Separate guests from internal users
- Separate management traffic
- Separate voice if needed
- Avoid too many unnecessary VLANs
- Document every VLAN

### Bad VLAN design example

```text
VLAN 2: random users
VLAN 3: some printers
VLAN 4: test maybe
VLAN 5: old stuff
```

This becomes hard to manage quickly.

### Better design example

```text
VLAN 10: Users
VLAN 20: Voice
VLAN 30: Printers
VLAN 40: Guests
VLAN 50: Servers
VLAN 99: Network Management
```

Clear design helps future troubleshooting.

---

## 14. Subnet Design

A subnet is a Layer 3 IP network.

Each VLAN usually has its own subnet.

### Example

```text
VLAN 10 -> 192.168.10.0/24
VLAN 20 -> 192.168.20.0/24
VLAN 30 -> 192.168.30.0/24
```

### Why subnet design matters

Good subnet design helps with:

- Routing
- Summarization
- Troubleshooting
- Security rules
- DHCP scopes
- Growth planning
- Documentation

### Avoid random addressing

Bad example:

```text
Users:   192.168.7.0/24
Voice:   10.55.2.0/24
Guests:  172.21.99.0/24
Printers:192.168.200.0/24
```

This may work, but it is harder to understand.

Better example:

```text
Site 1 Users:    10.1.10.0/24
Site 1 Voice:    10.1.20.0/24
Site 1 Printers: 10.1.30.0/24
Site 1 Guests:   10.1.40.0/24

Site 2 Users:    10.2.10.0/24
Site 2 Voice:    10.2.20.0/24
Site 2 Printers: 10.2.30.0/24
Site 2 Guests:   10.2.40.0/24
```

This is easier to read and scale.

---

## 15. IP Address Planning

IP address planning means choosing address ranges in a way that supports the network design.

### Common private IPv4 ranges

| Range | Size |
|---|---|
| `10.0.0.0/8` | Very large |
| `172.16.0.0/12` | Medium-large |
| `192.168.0.0/16` | Common for small networks |

### Good IP planning should consider:

- Number of sites
- Number of VLANs
- Expected growth
- Routing summarization
- VPN connections
- Cloud networks
- Overlapping networks
- Guest networks
- Lab networks

### Avoid overlapping IP ranges

Overlapping IP ranges cause problems with VPNs, mergers, cloud connections, and routing.

Example problem:

```text
Company network: 192.168.1.0/24
Remote user home network: 192.168.1.0/24
```

This can create routing confusion when the user connects through VPN.

### Practical advice

For business networks, avoid using only `192.168.1.0/24` everywhere.

Use a planned structure such as:

```text
10.<site>.<vlan>.0/24
```

Example:

```text
10.5.10.0/24  Site 5 Users
10.5.20.0/24  Site 5 Voice
10.5.30.0/24  Site 5 Printers
```

---

## 16. Routing Design

Routing design controls how traffic moves between networks.

### Common routing choices

- Static routes
- Default routes
- OSPF
- EIGRP
- BGP
- Route redistribution
- Route summarization

### Static routing

Static routes are manually configured.

They are simple and predictable, but they do not scale well in large networks.

Good for:

- Small networks
- Simple default routes
- Specific fixed paths
- Lab environments

### Dynamic routing

Dynamic routing protocols allow routers to exchange route information.

Good for:

- Larger networks
- Redundant paths
- Faster failover
- Multi-site networks
- Data centers

### Route summarization

Route summarization combines multiple smaller routes into one larger route.

Example:

```text
10.1.10.0/24
10.1.20.0/24
10.1.30.0/24
10.1.40.0/24
```

may be summarized as:

```text
10.1.0.0/16
```

Summarization keeps routing tables cleaner and improves stability.

---

## 17. Default Gateway Design

The default gateway is the router address that clients use to reach other networks.

### Example

Client:

```text
IP address:      192.168.10.50
Subnet mask:     255.255.255.0
Default gateway: 192.168.10.1
```

The client sends non-local traffic to `192.168.10.1`.

### Gateway placement options

Default gateways can live on:

- Router interfaces
- Layer 3 switch SVIs
- Firewall interfaces
- Redundant gateway pairs

### First-hop redundancy

To avoid one gateway becoming a single point of failure, networks can use first-hop redundancy protocols.

Examples:

- HSRP
- VRRP
- GLBP

### Simple redundant gateway idea

```text
Client default gateway: 192.168.10.1

Switch A real IP:       192.168.10.2
Switch B real IP:       192.168.10.3
Virtual gateway IP:     192.168.10.1
```

Clients use the virtual gateway. If one switch fails, the other can take over.

---

## 18. Redundancy

Redundancy means having backup components or paths.

The goal is to avoid a single failure taking down the network.

### Common redundancy examples

- Dual power supplies
- UPS systems
- Dual switches
- Dual uplinks
- Redundant firewalls
- Multiple Internet circuits
- Multiple DNS servers
- Multiple DHCP servers
- Dynamic routing failover
- Link aggregation

### Example without redundancy

```text
Internet -> Firewall -> Switch -> Users
```

If the firewall fails, users lose Internet access.

### Example with redundancy

```text
ISP 1 ---- Firewall A ---- Switch A
ISP 2 ---- Firewall B ---- Switch B
              |              |
            Redundant links and failover
```

This design can survive more failures.

### Important point

Redundancy adds complexity. It must be designed and tested properly.

A badly designed redundant network can create loops, asymmetric routing, or failover problems.

---

## 19. High Availability

High availability means designing systems so they stay available even during failures.

Redundancy is part of high availability, but high availability also includes:

- Failover behavior
- Monitoring
- Testing
- Maintenance planning
- Backup power
- Configuration consistency
- Operational procedures

### Availability example

If a firewall pair is configured in high availability mode, one firewall can take over when the other fails.

### Active-passive

One device is active, the other waits.

If the active device fails, the passive device takes over.

### Active-active

Both devices actively pass traffic.

This can improve resource use, but it may be more complex.

### Important question

Do not only ask:

```text
Do we have two devices?
```

Ask:

```text
Have we tested failover?
```

Redundancy that has never been tested is only an assumption.

---

## 20. Load Balancing

Load balancing spreads traffic across multiple systems or paths.

### Common load balancing examples

- Web load balancers
- Multiple Internet links
- Equal-cost routing paths
- DNS-based distribution
- Server clusters

### Load balancing vs redundancy

They are related but not identical.

Redundancy focuses on backup during failure.

Load balancing focuses on distributing traffic.

Some systems do both.

### Example

A website may have three web servers behind a load balancer:

```text
Users -> Load Balancer -> Web1
                       -> Web2
                       -> Web3
```

If one server fails, the load balancer can stop sending traffic to it.

---

## 21. Failure Domains

A failure domain is the part of the network affected by a failure.

Good design tries to keep failure domains small.

### Example

If one access switch fails, only users on that switch should be affected.

If one VLAN has a broadcast storm, it should not take down the whole network.

If one Internet circuit fails, another circuit should carry traffic.

### Why this matters

Small failure domains make networks easier to troubleshoot and more resilient.

Bad designs allow one small issue to affect many unrelated systems.

---

## 22. Security Zones

A security zone is a group of systems with similar trust level or security requirements.

### Common zones

| Zone | Purpose |
|---|---|
| Inside | Trusted internal users and systems |
| Outside | Internet or untrusted networks |
| DMZ | Public-facing services |
| Guest | Guest users |
| Management | Admin access to infrastructure |
| Server | Internal servers |
| Voice | IP phones and voice systems |
| IoT | Cameras, sensors, smart devices |

### Why zones matter

Security zones help define firewall policy.

Example:

```text
Guest -> Internet: Allow
Guest -> Internal servers: Deny
Users -> Printers: Allow selected ports
Users -> Management network: Deny
Admins -> Management network: Allow
```

This is much better than allowing everything everywhere.

---

## 23. Firewall Placement

Firewalls control traffic between zones or networks.

### Common firewall locations

- Internet edge
- Between users and servers
- Between guest and internal networks
- Between data center zones
- Between cloud and on-premises networks
- Around sensitive systems

### Internet edge firewall

This firewall separates the internal network from the Internet.

It commonly handles:

- NAT
- VPN
- Internet access control
- Inbound publishing rules
- Threat filtering
- Logging

### Internal segmentation firewall

This firewall controls traffic between internal zones.

Example:

```text
Users -> Server VLAN
IoT -> Internal network
Guest -> Internet
Admin -> Management network
```

### Important design idea

Not all internal traffic should be trusted automatically.

Modern designs often limit east-west movement inside the network.

---

## 24. Management Network Design

A management network is used to manage infrastructure devices.

Examples of devices managed through it:

- Switches
- Routers
- Firewalls
- Servers
- Hypervisors
- Storage systems
- Wireless controllers
- UPS systems
- Out-of-band management cards

### Why separate management traffic?

A separate management network improves security and control.

It helps ensure that normal users cannot directly access device management interfaces.

### Good practices

- Use a dedicated management VLAN or network
- Restrict access to admin workstations or jump hosts
- Use SSH or HTTPS, not Telnet or HTTP
- Use MFA where possible
- Log administrative access
- Use AAA systems like RADIUS or TACACS+
- Disable unused management services

### Out-of-band management

Out-of-band management uses a separate path for emergency access.

Example:

If the production network is broken, administrators can still access devices through a separate management network.

---

## 25. Wireless Design

Wireless design is not only about installing access points.

Good Wi-Fi design considers:

- Coverage
- Capacity
- Interference
- Channel planning
- Roaming
- Security
- Client density
- Building materials
- Application requirements

### Coverage vs capacity

Coverage means the signal reaches the area.

Capacity means the network can handle the number of users and traffic.

A space may have good coverage but poor capacity if too many users share too few access points.

### Common Wi-Fi design choices

- Separate guest SSID
- WPA2/WPA3 security
- Enterprise authentication for employees
- Proper channel width
- Avoid too many SSIDs
- Use 5 GHz and 6 GHz where possible
- Survey before large deployments

### Guest Wi-Fi

Guest Wi-Fi should usually be isolated from internal networks.

Common design:

```text
Guest Wi-Fi -> Guest VLAN -> Firewall -> Internet only
```

---

## 26. Internet Edge Design

The Internet edge is where the internal network connects to the Internet.

Common components:

- ISP circuit
- Modem or handoff
- Edge router
- Firewall
- VPN gateway
- NAT
- Public IP addresses
- DNS records
- DDoS protection in some environments

### Single ISP design

Simple but less redundant.

```text
Internet -> ISP -> Firewall -> Internal Network
```

### Dual ISP design

More resilient.

```text
ISP 1 ----\
           Firewall/Edge ---- Internal Network
ISP 2 ----/
```

### Important considerations

- NAT policy
- Inbound services
- VPN access
- DNS records
- Failover behavior
- Public IP management
- Monitoring
- Security filtering

---

## 27. VPN and Remote Access Design

VPN design controls how remote users or remote sites connect.

### Site-to-site VPN

Connects two networks.

Example:

```text
Branch Office <---- VPN Tunnel ----> Headquarters
```

Used for:

- Branch connectivity
- Partner connectivity
- Cloud connectivity
- Backup links

### Remote access VPN

Connects individual users.

Example:

```text
Remote laptop -> VPN -> Company network
```

Used for:

- Work from home
- Admin access
- Traveling users

### Full tunnel vs split tunnel

Full tunnel sends all user traffic through the VPN.

Split tunnel sends only company traffic through the VPN, while Internet traffic goes directly out.

| Mode | Benefit | Tradeoff |
|---|---|---|
| Full tunnel | More control and visibility | More bandwidth load |
| Split tunnel | Better performance | Less centralized control |

### VPN design should consider:

- Authentication
- MFA
- Device posture
- DNS behavior
- Route injection
- Split tunneling
- Logging
- Access control
- Capacity

---

## 28. Cloud and Hybrid Network Design

Many modern networks connect on-premises environments to cloud platforms.

Examples:

- AWS
- Azure
- Google Cloud
- Oracle Cloud
- Private cloud

### Hybrid network

A hybrid network connects local infrastructure with cloud infrastructure.

Example:

```text
Office/Data Center <---- VPN or Direct Connect ----> Cloud VPC/VNet
```

### Design concerns

- IP address overlap
- Routing
- Security groups
- Cloud firewalls
- DNS resolution
- Latency
- Bandwidth
- High availability
- Identity integration
- Monitoring

### Avoid overlapping cloud networks

If your on-premises network uses:

```text
10.0.0.0/8
```

and your cloud also uses overlapping ranges, routing becomes difficult.

Plan cloud address ranges carefully.

---

## 29. DNS, DHCP, and Core Services

Core services are essential for daily network operation.

Important services include:

- DNS
- DHCP
- NTP
- Directory services
- Authentication systems
- Logging systems
- Monitoring systems

### DNS design

DNS should be reliable and reachable.

Good design uses:

- Multiple DNS servers
- Correct internal and external zones
- Clear split DNS design if needed
- Proper TTL values
- Secure DNS updates where required

### DHCP design

DHCP should match VLAN and subnet design.

Good DHCP design includes:

- Correct scopes
- Correct gateways
- Correct DNS options
- Reservations where needed
- Exclusions for static devices
- Failover or redundancy for important networks

### NTP design

Time synchronization is important for:

- Logs
- Kerberos
- Certificates
- Security investigations
- Distributed systems

Every network should have a clear time source.

---

## 30. Monitoring and Logging

A network should be monitored before users report problems.

### What to monitor

- Device uptime
- Interface status
- Bandwidth usage
- CPU and memory
- Packet loss
- Latency
- Wireless health
- VPN tunnels
- Firewall logs
- DHCP scope usage
- DNS health
- Environmental sensors

### Logging

Logs help answer what happened.

Important log sources:

- Firewalls
- Switches
- Routers
- Wireless controllers
- Servers
- Authentication systems
- VPN gateways
- Cloud platforms

### Good monitoring answers questions like:

- Is the Internet link down?
- Which switch port is flapping?
- Is the DHCP scope full?
- Is a firewall blocking traffic?
- Did a device reboot?
- When did the problem start?

---

## 31. Capacity Planning

Capacity planning means making sure the network can handle current and future demand.

### Things to measure

- Number of users
- Number of devices
- Internet bandwidth usage
- Wi-Fi client density
- Switch port usage
- Uplink utilization
- Firewall throughput
- VPN user count
- Server traffic
- Storage traffic

### Example

A 1 Gbps uplink may be fine today, but if many users start using video calls, backups, or cloud storage, it may become a bottleneck.

### Good capacity planning includes growth

Plan for:

- New employees
- More wireless devices
- More cloud services
- More cameras
- More voice/video traffic
- More remote users

Do not design only for the exact number of devices you have today.

---

## 32. Scalability

Scalability means the network can grow without needing a complete redesign.

### Scalable design uses:

- Structured IP addressing
- Consistent VLAN numbering
- Modular topology
- Dynamic routing when appropriate
- Route summarization
- Clear naming conventions
- Repeatable site templates
- Good documentation

### Example of scalable site design

```text
Site 1: 10.1.0.0/16
Site 2: 10.2.0.0/16
Site 3: 10.3.0.0/16
```

Within each site:

```text
Users:    10.x.10.0/24
Voice:    10.x.20.0/24
Printers: 10.x.30.0/24
Guests:   10.x.40.0/24
Servers:  10.x.50.0/24
```

This pattern is easy to repeat.

---

## 33. Performance Design

Performance design focuses on making traffic move efficiently.

### Important performance factors

- Bandwidth
- Latency
- Jitter
- Packet loss
- Duplex settings
- Interface errors
- Routing paths
- Firewall inspection load
- Wi-Fi interference
- Server performance
- Application behavior

### Bandwidth

Bandwidth is the amount of data that can be sent per second.

Example:

```text
1 Gbps
10 Gbps
40 Gbps
100 Gbps
```

### Latency

Latency is delay.

It matters for:

- Voice
- Video
- Gaming
- Remote desktops
- Financial systems
- Interactive applications

### Jitter

Jitter is variation in delay.

It is especially important for voice and video.

### Packet loss

Packet loss can cause:

- Slow transfers
- Bad calls
- Video freezing
- Application timeouts
- TCP retransmissions

Good design reduces unnecessary bottlenecks and avoids overloaded links.

---

## 34. QoS Design

QoS stands for **Quality of Service**.

It helps prioritize important traffic when there is congestion.

### Common traffic classes

- Voice
- Video
- Business-critical applications
- Best effort traffic
- Bulk transfers
- Guest traffic

### Why QoS matters

Voice traffic is sensitive to delay, jitter, and packet loss.

A file download can slow down, but a voice call may become unusable if delayed too much.

QoS helps protect time-sensitive traffic.

### Common QoS concepts

- Classification
- Marking
- Queuing
- Policing
- Shaping
- DSCP
- CoS

### Important note

QoS does not create more bandwidth.

It decides which traffic gets better treatment when bandwidth is limited.

---

## 35. Documentation

Documentation is part of network design, not an optional extra.

Good documentation helps people understand and fix the network.

### Useful documentation includes:

- Network diagrams
- IP address plan
- VLAN list
- Device inventory
- Switch port mapping
- Firewall rules summary
- Routing design
- DNS and DHCP design
- VPN information
- Wireless design
- Circuit information
- Admin access process
- Backup and recovery process

### Good diagram types

- Physical diagram
- Logical diagram
- Rack diagram
- Wireless coverage map
- Data flow diagram
- Security zone diagram

### Bad documentation problem

If only one person understands the network, the network has operational risk.

---

## 36. Change Management

Change management means controlling how changes are made.

Network changes can affect many users quickly.

### Good change process includes:

- What will change
- Why it is needed
- Who approved it
- Expected impact
- Maintenance window
- Rollback plan
- Testing plan
- Communication plan
- Documentation update

### Example change

Changing a firewall rule may seem small, but it can break an application or expose a service.

Changing a VLAN trunk may disconnect many users.

Changing a routing protocol setting may affect multiple sites.

### Rollback plan

Always know how to undo the change.

A good rollback plan is specific, not vague.

Bad rollback plan:

```text
Revert if needed.
```

Better rollback plan:

```text
Restore previous firewall policy version from backup taken at 10:00.
Verify users can access app.example.com after rollback.
```

---

## 37. Common Design Mistakes

### Mistake 1: No IP addressing plan

Random IP addressing makes routing, troubleshooting, and growth harder.

### Mistake 2: Flat network

A flat network with everything in one subnet is simple at first but risky later.

Problems include:

- Large broadcast domain
- Poor security
- No segmentation
- Harder troubleshooting

### Mistake 3: No guest isolation

Guest users should not be placed on the same network as internal users.

### Mistake 4: Single points of failure

One switch, one firewall, one Internet link, or one power source can become a major risk.

### Mistake 5: Poor VLAN naming

Names like `VLAN 12`, `VLAN 13`, and `VLAN 14` without documentation are not useful.

Use clear names.

### Mistake 6: Overly complex design

Complexity creates operational risk.

Do not build a large enterprise-style design for a tiny network unless there is a real reason.

### Mistake 7: No monitoring

Without monitoring, you usually discover problems from users.

### Mistake 8: No documentation

Undocumented networks become difficult to operate.

### Mistake 9: Exposing management interfaces

Management access should be restricted.

Do not expose switch, router, firewall, hypervisor, or server management to untrusted networks.

### Mistake 10: Ignoring future growth

A design that barely fits today may fail soon.

Leave room for growth.

---

## 38. Example Design: Small Office

### Scenario

A small office has:

- 20 employees
- 1 Internet connection
- 1 firewall/router
- 1 managed switch
- 2 access points
- Some printers
- Guest Wi-Fi

### Simple VLAN design

| VLAN | Name | Subnet |
|---:|---|---|
| 10 | Users | `192.168.10.0/24` |
| 20 | Printers | `192.168.20.0/24` |
| 30 | Guests | `192.168.30.0/24` |
| 99 | Management | `192.168.99.0/24` |

### Traffic policy

| Source | Destination | Action |
|---|---|---|
| Users | Internet | Allow |
| Users | Printers | Allow printing ports |
| Guests | Internet | Allow |
| Guests | Internal networks | Deny |
| Users | Management | Deny |
| Admin PC | Management | Allow |

### Design notes

- Guest Wi-Fi is isolated
- Printers are separate from users
- Management interfaces are restricted
- DHCP scopes match VLANs
- DNS points to trusted resolvers
- Firewall handles inter-VLAN policy

This design is simple but much better than placing everything in one flat network.

---

## 39. Example Design: Medium Business

### Scenario

A medium business has:

- 250 users
- 2 floors
- Multiple access switches
- IP phones
- Internal servers
- Guest Wi-Fi
- VPN users
- Two Internet circuits
- Firewall pair

### Example VLAN design

| VLAN | Name | Subnet |
|---:|---|---|
| 10 | Users Floor 1 | `10.1.10.0/24` |
| 11 | Users Floor 2 | `10.1.11.0/24` |
| 20 | Voice | `10.1.20.0/24` |
| 30 | Printers | `10.1.30.0/24` |
| 40 | Guests | `10.1.40.0/24` |
| 50 | Servers | `10.1.50.0/24` |
| 60 | Cameras | `10.1.60.0/24` |
| 99 | Management | `10.1.99.0/24` |

### Core ideas

- Access switches connect to redundant distribution switches
- Distribution switches handle inter-VLAN routing
- Firewalls enforce traffic to sensitive zones
- Guest traffic goes only to the Internet
- Voice traffic gets QoS treatment
- VPN users have controlled access
- Management network is restricted
- Monitoring watches links, devices, DHCP, DNS, and VPNs

### Example layout

```text
             ISP 1          ISP 2
               |              |
          Firewall A ---- Firewall B
               |              |
        Distribution A ---- Distribution B
          /       \          /       \
     Access 1   Access 2  Access 3  Access 4
       |           |         |         |
     Users       Phones     APs     Printers
```

This design provides segmentation, redundancy, and room for growth.

---

## 40. Network Design Checklist

Use this checklist when planning or reviewing a network.

### Requirements

- [ ] Number of users is known
- [ ] Number of devices is estimated
- [ ] Critical applications are identified
- [ ] Security requirements are understood
- [ ] Growth expectations are documented
- [ ] Budget and constraints are known

### Physical design

- [ ] Device locations are planned
- [ ] Cable paths are planned
- [ ] Uplinks are sized correctly
- [ ] Power and UPS needs are considered
- [ ] Rack layout is documented
- [ ] Fiber/copper requirements are known

### Logical design

- [ ] VLANs are defined
- [ ] Subnets are assigned
- [ ] IP addressing plan is consistent
- [ ] Default gateways are planned
- [ ] Routing design is documented
- [ ] DHCP scopes are planned
- [ ] DNS design is documented

### Security

- [ ] Guest network is isolated
- [ ] Management network is restricted
- [ ] Firewall zones are defined
- [ ] Inbound Internet access is limited
- [ ] VPN access is controlled
- [ ] Unused ports are disabled where appropriate
- [ ] Authentication and logging are planned

### Availability

- [ ] Single points of failure are identified
- [ ] Redundant links are planned where needed
- [ ] Critical devices have backup power
- [ ] Firewall/router redundancy is considered
- [ ] DHCP/DNS redundancy is considered
- [ ] Failover has a test plan

### Operations

- [ ] Monitoring is planned
- [ ] Logging is centralized where needed
- [ ] Backups are configured
- [ ] Change management process exists
- [ ] Diagrams are created
- [ ] Device inventory is maintained

---

## 41. Quick Summary

Network design is about building a network that is reliable, secure, scalable, and manageable.

The most important points to remember are:

- Good design starts with requirements
- Physical and logical design are different but connected
- Hierarchical design helps create structure
- Access, distribution, and core layers have different roles
- Segmentation improves security and control
- VLANs separate Layer 2 domains
- Subnets organize Layer 3 addressing
- A clear IP plan makes troubleshooting and growth easier
- Routing design controls how traffic moves
- Redundancy reduces single points of failure
- High availability requires testing, not just extra hardware
- Security zones help define firewall policy
- Management networks should be protected
- Wireless design must consider coverage and capacity
- Monitoring and logging are essential
- Documentation is part of the design
- Simplicity is valuable

A strong network design should be easy to understand, easy to operate, and ready for future growth.
