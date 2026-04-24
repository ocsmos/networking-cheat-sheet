# 8. Routing Basics

Routing is the process of moving packets from one network to another. If switching is mainly about moving frames inside the same local network, routing is about moving packets between different networks.

This guide explains routing in a practical and detailed way. The goal is to understand how routers make forwarding decisions, what route tables contain, how default routes work, and how common routing protocols fit into real networks.

---

## Table of Contents

- [1. What Routing Is](#1-what-routing-is)
- [2. Why Routing Matters](#2-why-routing-matters)
- [3. Switching vs Routing](#3-switching-vs-routing)
- [4. What a Router Does](#4-what-a-router-does)
- [5. Routing Table Basics](#5-routing-table-basics)
- [6. Connected Routes](#6-connected-routes)
- [7. Static Routes](#7-static-routes)
- [8. Default Routes](#8-default-routes)
- [9. Dynamic Routing](#9-dynamic-routing)
- [10. Longest Prefix Match](#10-longest-prefix-match)
- [11. Next Hop](#11-next-hop)
- [12. Metric](#12-metric)
- [13. Administrative Distance](#13-administrative-distance)
- [14. Route Preference](#14-route-preference)
- [15. Routing Convergence](#15-routing-convergence)
- [16. Route Summarization](#16-route-summarization)
- [17. Route Redistribution](#17-route-redistribution)
- [18. Common Routing Protocols](#18-common-routing-protocols)
- [19. RIP](#19-rip)
- [20. OSPF](#20-ospf)
- [21. EIGRP](#21-eigrp)
- [22. BGP](#22-bgp)
- [23. First Hop Redundancy](#23-first-hop-redundancy)
- [24. Inter-VLAN Routing](#24-inter-vlan-routing)
- [25. IPv4 Routing Example](#25-ipv4-routing-example)
- [26. IPv6 Routing Example](#26-ipv6-routing-example)
- [27. Common Routing Problems](#27-common-routing-problems)
- [28. Troubleshooting Routing Step by Step](#28-troubleshooting-routing-step-by-step)
- [29. Useful Routing Commands](#29-useful-routing-commands)
- [30. Routing Best Practices](#30-routing-best-practices)
- [31. Quick Summary](#31-quick-summary)

---

## 1. What Routing Is

Routing is the process of choosing a path for network traffic.

When a device wants to send traffic to a different network, it usually sends that traffic to a router. The router checks the destination IP address and decides where to send the packet next.

### Simple example

A computer is on this network:

```text
192.168.1.0/24
```

It wants to reach a server on this network:

```text
10.10.20.0/24
```

Those are different networks, so the computer sends the packet to its default gateway. The gateway then routes the packet toward the destination network.

---

## 2. Why Routing Matters

Routing is what makes large networks possible.

Without routing, devices could only communicate inside their own local network. Routing allows communication between:

- VLANs
- Branch offices
- Data centers
- Cloud networks
- Remote sites
- The Internet
- Partner networks
- VPN networks

Routing is used every time traffic moves from one IP network to another.

### Practical examples

Routing is involved when:

- A laptop reaches the Internet
- A user in VLAN 10 accesses a server in VLAN 20
- A branch office connects to headquarters
- A server reaches a database in another subnet
- A company connects to a cloud provider
- A router forwards traffic to an ISP

---

## 3. Switching vs Routing

Switching and routing are related, but they work at different layers.

| Topic | Switching | Routing |
|---|---|---|
| Main layer | Layer 2 | Layer 3 |
| Main address | MAC address | IP address |
| Main device | Switch | Router / Layer 3 switch |
| Main scope | Same network / VLAN | Between networks |
| Data unit | Frame | Packet |

### Switching

Switching moves Ethernet frames inside the same Layer 2 network.

A switch looks at MAC addresses.

### Routing

Routing moves IP packets between Layer 3 networks.

A router looks at IP addresses.

### Simple rule

If the destination is in the same subnet, the device uses local Layer 2 delivery.

If the destination is in a different subnet, the device sends the packet to a router.

---

## 4. What a Router Does

A router forwards packets between networks.

The router looks at the destination IP address, checks its routing table, and sends the packet out of the correct interface.

### Router responsibilities

A router can:

- Connect different IP networks
- Choose forwarding paths
- Act as a default gateway
- Run routing protocols
- Apply access control policies
- Perform NAT in some designs
- Connect LANs to WANs
- Connect private networks to the Internet

### Important point

A router does not forward traffic randomly. It follows the routing table.

---

## 5. Routing Table Basics

A routing table is a list of known networks and how to reach them.

A router uses this table to decide where to send packets.

### Basic routing table fields

A route usually contains:

- Destination network
- Prefix length or subnet mask
- Next hop
- Exit interface
- Metric
- Route source

### Example route

```text
Destination: 10.10.20.0/24
Next hop:    192.168.1.2
Interface:   GigabitEthernet0/1
```

This means:

```text
To reach 10.10.20.0/24, send traffic to 192.168.1.2 through interface GigabitEthernet0/1.
```

### Route sources

Routes can come from different sources:

- Connected interfaces
- Static configuration
- Dynamic routing protocols
- Default routes

---

## 6. Connected Routes

A connected route is created automatically when a router interface has an IP address and the interface is up.

### Example

If a router interface is configured as:

```text
192.168.10.1/24
```

The router knows that this network is directly connected:

```text
192.168.10.0/24
```

The router does not need a next hop for that network because it is directly attached.

### Why connected routes matter

Connected routes are usually the simplest and most trusted routes.

They tell the router:

```text
This network is directly reachable from one of my interfaces.
```

---

## 7. Static Routes

A static route is manually configured by an administrator.

Static routes are useful when the path is simple and does not change often.

### Example static route

```text
ip route 10.10.20.0 255.255.255.0 192.168.1.2
```

This means:

```text
To reach 10.10.20.0/24, send traffic to 192.168.1.2.
```

### When static routes are useful

Static routes are useful for:

- Small networks
- Default routes to an ISP
- Simple branch connections
- Backup routes
- Lab environments
- Point-to-point links
- Routes that should not change automatically

### Advantages

- Simple
- Predictable
- Low overhead
- No routing protocol required

### Disadvantages

- Manual work
- Does not adapt automatically to failures unless configured with tracking
- Hard to manage at large scale
- Easy to misconfigure in complex networks

---

## 8. Default Routes

A default route is used when no more specific route matches the destination.

It is sometimes called the “route of last resort.”

### IPv4 default route

```text
0.0.0.0/0
```

### IPv6 default route

```text
::/0
```

### Example

A home router usually has local routes for the home network and a default route toward the ISP.

If a device wants to reach:

```text
8.8.8.8
```

and the router does not have a specific route for it, the router sends it to the default route.

### Static IPv4 default route example

```text
ip route 0.0.0.0 0.0.0.0 203.0.113.1
```

This means:

```text
For all unknown IPv4 destinations, send traffic to 203.0.113.1.
```

### Static IPv6 default route example

```text
ipv6 route ::/0 2001:db8::1
```

---

## 9. Dynamic Routing

Dynamic routing uses routing protocols to learn routes automatically.

Instead of manually configuring every route, routers exchange routing information with each other.

### Why dynamic routing is useful

Dynamic routing helps networks adapt when links fail or topology changes.

It is useful for:

- Medium networks
- Large networks
- Enterprise networks
- ISP networks
- Data centers
- Multi-site networks
- Networks with redundant paths

### Common dynamic routing protocols

- RIP
- OSPF
- EIGRP
- BGP

Each protocol has its own behavior, strengths, and use cases.

---

## 10. Longest Prefix Match

Longest prefix match is one of the most important routing rules.

When more than one route matches a destination, the router chooses the most specific route.

### Example

A router has these routes:

```text
10.0.0.0/8
10.10.0.0/16
10.10.20.0/24
```

A packet is going to:

```text
10.10.20.55
```

All three routes match, but this route is the most specific:

```text
10.10.20.0/24
```

So the router uses the `/24` route.

### Why this matters

Longest prefix match is more important than many other route preferences.

A more specific route usually wins over a less specific route.

### Simple memory rule

The bigger the prefix length, the more specific the route.

```text
/24 is more specific than /16
/16 is more specific than /8
/32 is a single IPv4 host route
/128 is a single IPv6 host route
```

---

## 11. Next Hop

The next hop is the next router or gateway that should receive the packet.

### Example

```text
Destination network: 10.10.20.0/24
Next hop: 192.168.1.2
```

This means the router does not send the packet directly to `10.10.20.0/24`. It sends it to `192.168.1.2`, which is closer to the destination.

### Next hop vs exit interface

Some routes specify a next hop.

Some routes specify an exit interface.

Some routes specify both.

Example:

```text
ip route 10.10.20.0 255.255.255.0 192.168.1.2
```

or:

```text
ip route 10.10.20.0 255.255.255.0 GigabitEthernet0/1
```

or:

```text
ip route 10.10.20.0 255.255.255.0 GigabitEthernet0/1 192.168.1.2
```

The exact syntax depends on the platform.

---

## 12. Metric

A metric is a value used by a routing protocol to choose the best path.

Lower metric usually means better path, but the exact meaning depends on the routing protocol.

### Examples

| Protocol | Common Metric Idea |
|---|---|
| RIP | Hop count |
| OSPF | Cost |
| EIGRP | Composite metric |
| BGP | Path attributes, not a simple metric like OSPF |

### RIP metric

RIP uses hop count.

A path with 2 router hops is better than a path with 5 router hops.

### OSPF metric

OSPF uses cost, often based on bandwidth.

A faster link often has a better cost than a slower link, depending on configuration.

### Important point

Metrics are usually compared inside the same routing protocol.

If routes come from different sources, administrative distance is usually checked before metric.

---

## 13. Administrative Distance

Administrative distance, often called AD, is used to choose between routes learned from different sources.

It tells the router how trusted a route source is.

Lower administrative distance is preferred.

### Common Cisco-style administrative distances

| Route Source | Administrative Distance |
|---|---:|
| Connected | `0` |
| Static | `1` |
| EIGRP summary | `5` |
| External BGP | `20` |
| Internal EIGRP | `90` |
| OSPF | `110` |
| RIP | `120` |
| External EIGRP | `170` |
| Internal BGP | `200` |

### Example

A router learns the same network from OSPF and RIP:

```text
OSPF AD: 110
RIP AD: 120
```

The router prefers OSPF because `110` is lower than `120`.

### Important note

Administrative distance values are vendor-specific. The table above is common in Cisco environments, but other platforms may use different route preference systems.

---

## 14. Route Preference

When a router chooses a route, it usually follows this general logic:

1. Find all matching routes
2. Choose the longest prefix match
3. If there are multiple routes with the same prefix length, compare route source preference or administrative distance
4. If still tied, compare metric
5. If still tied, use equal-cost multipath if supported and configured

### Example

Routes:

```text
10.10.20.0/24 via OSPF
10.10.20.0/24 via static route
```

Same prefix length.

Static route usually has lower AD than OSPF, so the static route is preferred.

### Another example

Routes:

```text
10.10.0.0/16 via static route
10.10.20.0/24 via OSPF
```

A packet to `10.10.20.55` uses the `/24` OSPF route because it is more specific.

Longest prefix match wins first.

---

## 15. Routing Convergence

Convergence is the process of routers updating their routing tables after a network change.

A network is converged when all routers have a consistent and correct view of the network.

### Example

A link fails between two routers.

The routers must:

1. Detect the failure
2. Remove the bad route
3. Share the change
4. Calculate new best paths
5. Install new routes

The time this takes is convergence time.

### Why convergence matters

Slow convergence can cause:

- Packet loss
- Temporary routing loops
- Application timeouts
- Voice/video disruption
- Unstable user experience

### Protocol differences

Different protocols converge differently.

In general:

- RIP is slower and simpler
- OSPF is usually faster and more scalable
- EIGRP can converge quickly in many designs
- BGP convergence depends heavily on policy, scale, and timers

---

## 16. Route Summarization

Route summarization means advertising one larger route that represents several smaller routes.

### Example

Instead of advertising these routes:

```text
10.10.0.0/24
10.10.1.0/24
10.10.2.0/24
10.10.3.0/24
```

A router may advertise:

```text
10.10.0.0/22
```

This summary covers all four `/24` networks.

### Why summarization is useful

Summarization can:

- Reduce routing table size
- Improve stability
- Hide internal topology details
- Reduce routing updates
- Make large networks easier to manage

### Warning

Bad summarization can cause routing problems.

A summary route may send traffic toward a router that does not actually have a path to all parts of the summary.

Always summarize carefully.

---

## 17. Route Redistribution

Redistribution means taking routes learned from one routing source and injecting them into another routing protocol.

### Example

A router may redistribute:

```text
Static routes into OSPF
OSPF routes into BGP
EIGRP routes into OSPF
```

### Why redistribution is used

Redistribution is used when different parts of a network use different routing protocols.

Example:

- Internal network uses OSPF
- Edge network uses BGP
- Some legacy area uses EIGRP
- Static routes exist for special networks

### Risk

Redistribution can create routing loops or unexpected paths if it is done carelessly.

Good redistribution design usually needs:

- Filtering
- Metrics
- Tags
- Clear route control
- Documentation

---

## 18. Common Routing Protocols

Common routing protocols include:

| Protocol | Type | Common Use |
|---|---|---|
| RIP | Distance vector | Small or legacy networks |
| OSPF | Link-state | Enterprise internal routing |
| EIGRP | Advanced distance vector / hybrid | Cisco-oriented internal routing |
| BGP | Path vector | Internet and large-scale routing |

Each protocol solves routing differently.

A good engineer does not just memorize the names. They understand where each protocol fits.

---

## 19. RIP

RIP stands for **Routing Information Protocol**.

It is one of the older dynamic routing protocols.

### Main characteristics

- Distance vector protocol
- Uses hop count as metric
- Maximum hop count is 15
- 16 hops means unreachable
- Simple to configure
- Not ideal for large modern networks

### Metric

RIP chooses the path with the lowest hop count.

Example:

```text
Path A: 2 hops
Path B: 5 hops
```

RIP chooses Path A.

### Limitations

RIP has several limitations:

- Slow convergence
- Small maximum network diameter
- Does not consider bandwidth
- Not suitable for complex enterprise networks

### Where RIP may appear

You may still see RIP in:

- Labs
- Exam examples
- Legacy networks
- Very small networks
- Simple routing demonstrations

---

## 20. OSPF

OSPF stands for **Open Shortest Path First**.

It is a very common internal routing protocol.

### Main characteristics

- Link-state protocol
- Uses cost as metric
- Supports areas
- Scales better than RIP
- Common in enterprise networks
- Runs inside an autonomous system

### OSPF cost

OSPF uses cost to choose paths.

Cost is often based on interface bandwidth, but it can be manually adjusted.

Lower cost is preferred.

### OSPF areas

OSPF uses areas to organize the network.

The backbone area is:

```text
Area 0
```

Other areas usually connect to Area 0.

### Why areas matter

Areas help reduce the amount of routing information that every router must process.

They improve scalability and stability.

### Common OSPF terms

- Area
- Router ID
- Neighbor
- Adjacency
- LSA
- LSDB
- DR
- BDR
- Cost

### When OSPF is useful

OSPF is useful for:

- Enterprise LANs
- Campus networks
- Data center internal routing
- Multi-router internal networks
- Networks that need fast convergence and good scalability

---

## 21. EIGRP

EIGRP stands for **Enhanced Interior Gateway Routing Protocol**.

It is commonly associated with Cisco networks, although there has been some broader availability over time.

### Main characteristics

- Advanced distance vector / hybrid behavior
- Uses a composite metric
- Can converge quickly
- Supports unequal-cost load balancing in some cases
- Common in Cisco-oriented environments

### EIGRP metric

EIGRP can consider multiple factors, commonly including:

- Bandwidth
- Delay

Other values can exist, but bandwidth and delay are the most important in normal discussions.

### Common EIGRP terms

- Neighbor
- Successor
- Feasible successor
- Feasible distance
- Reported distance
- DUAL

### When EIGRP is useful

EIGRP can be useful in:

- Cisco-heavy networks
- Enterprise routing
- Networks that need fast convergence
- Designs where unequal-cost load balancing is useful

---

## 22. BGP

BGP stands for **Border Gateway Protocol**.

It is the routing protocol used to exchange routes between autonomous systems on the Internet.

### Main characteristics

- Path vector protocol
- Uses TCP port `179`
- Used between autonomous systems
- Very policy-driven
- Internet-scale
- Also used inside large private and cloud networks

### Autonomous System

An autonomous system, or AS, is a network or group of networks under one administrative control.

Each AS has an AS number.

### BGP is different

BGP is not mainly about choosing the fastest path.

It is about routing policy.

BGP can choose paths based on attributes such as:

- AS path
- Local preference
- MED
- Origin
- Next hop
- Communities
- Weight on some platforms

### Common BGP uses

BGP is used for:

- Internet routing
- ISP connectivity
- Multi-homing
- Cloud connectivity
- Data center routing
- Large enterprise WANs
- Route control between organizations

### eBGP vs iBGP

| Type | Meaning |
|---|---|
| eBGP | BGP between different autonomous systems |
| iBGP | BGP inside the same autonomous system |

### Security note

BGP should be carefully filtered.

Bad BGP advertisements can cause serious routing problems.

---

## 23. First Hop Redundancy

First hop redundancy provides a backup default gateway for hosts.

Common protocols include:

- HSRP
- VRRP
- GLBP

### The problem

A host usually has one default gateway.

If that gateway fails, the host may lose access to other networks.

### The solution

First hop redundancy protocols create a virtual gateway IP address.

Two or more routers participate.

One router actively forwards traffic. Another router can take over if the active router fails.

### Example

Hosts use this default gateway:

```text
192.168.1.1
```

But behind the scenes, two routers share that virtual IP.

```text
Router A: active
Router B: standby
Virtual gateway: 192.168.1.1
```

If Router A fails, Router B takes over the virtual gateway.

---

## 24. Inter-VLAN Routing

VLANs separate Layer 2 networks.

To communicate between VLANs, routing is required.

### Example

```text
VLAN 10: 192.168.10.0/24
VLAN 20: 192.168.20.0/24
```

A host in VLAN 10 cannot directly communicate with VLAN 20 at Layer 2.

Traffic must go through a Layer 3 device.

### Common inter-VLAN routing methods

- Router-on-a-stick
- Layer 3 switch with SVIs
- Firewall as default gateway

### SVI example

An SVI is a switched virtual interface.

Example:

```text
interface vlan 10
 ip address 192.168.10.1 255.255.255.0

interface vlan 20
 ip address 192.168.20.1 255.255.255.0
```

The Layer 3 switch can route between VLAN 10 and VLAN 20.

### Security note

Just because routing exists between VLANs does not mean all traffic should be allowed.

Use ACLs or firewall policies where segmentation matters.

---

## 25. IPv4 Routing Example

Consider this network:

```text
PC-A:      192.168.1.10/24
Gateway:   192.168.1.1
Router R1: 192.168.1.1 and 10.10.10.1
Server:    10.10.10.50/24
```

PC-A wants to reach:

```text
10.10.10.50
```

### Step-by-step

1. PC-A checks its own subnet.
2. `10.10.10.50` is not in `192.168.1.0/24`.
3. PC-A sends the packet to its default gateway `192.168.1.1`.
4. R1 receives the packet.
5. R1 checks its routing table.
6. R1 sees that `10.10.10.0/24` is directly connected.
7. R1 forwards the packet toward the server.
8. The server replies through its own gateway if needed.

### Key lesson

Routing must work in both directions.

If the return route is missing, the connection can fail even if the first packet reaches the destination.

---

## 26. IPv6 Routing Example

IPv6 routing uses the same basic idea as IPv4 routing.

Example networks:

```text
LAN A: 2001:db8:10::/64
LAN B: 2001:db8:20::/64
```

A host in LAN A wants to reach a server in LAN B.

The host sends traffic to its IPv6 default gateway.

The router checks the destination IPv6 prefix and forwards the packet.

### IPv6 default route

```text
::/0
```

### IPv6 specific route example

```text
2001:db8:20::/64 via 2001:db8:10::1
```

### Key lesson

The routing logic is similar, but the addresses and neighbor discovery behavior are different.

IPv6 uses NDP instead of ARP.

---

## 27. Common Routing Problems

Routing problems can happen for many reasons.

### Missing route

A router does not know where to send traffic.

Symptom:

```text
Destination unreachable
```

or traffic simply times out.

### Wrong default gateway

A host uses the wrong gateway.

Symptom:

- Can reach local subnet
- Cannot reach remote networks

### Asymmetric routing

Traffic goes out one path and returns through another path.

This is not always bad, but it can break stateful firewalls or troubleshooting assumptions.

### Route loop

Routers send packets in a loop.

Symptoms:

- TTL expires
- Traceroute shows repeated hops
- Packet loss

### Bad summarization

A summary route sends traffic toward a router that does not actually know all detailed routes.

### Administrative distance issue

A less desirable route source is preferred because of route preference settings.

### Metric issue

A routing protocol chooses a path that is technically valid but not the desired path.

### Redistribution issue

Routes are injected incorrectly between protocols.

Symptoms:

- Loops
- Duplicate routes
- Unexpected path selection
- Missing routes

---

## 28. Troubleshooting Routing Step by Step

Routing troubleshooting should be methodical.

### Step 1: Confirm the IP configuration

On the client, check:

- IP address
- Subnet mask or prefix
- Default gateway
- DNS servers if name resolution is involved

### Step 2: Check local connectivity

Can the host reach its default gateway?

```bash
ping 192.168.1.1
```

If not, the issue may be local switching, VLAN, ARP, NDP, firewall, or host configuration.

### Step 3: Check the routing table on the host

Windows:

```powershell
route print
```

Linux:

```bash
ip route
```

macOS:

```bash
netstat -rn
```

### Step 4: Check the router routing table

Look for a route to the destination network.

Cisco-style example:

```text
show ip route
show ipv6 route
```

Linux router example:

```bash
ip route
ip -6 route
```

### Step 5: Check the return path

Make sure the destination network also has a route back to the source network.

Routing must work both ways.

### Step 6: Use traceroute

Traceroute shows the path traffic takes.

Windows:

```powershell
tracert 10.10.20.50
```

Linux/macOS:

```bash
traceroute 10.10.20.50
```

### Step 7: Check firewall and ACLs

Sometimes routing is correct, but traffic is blocked.

Check:

- Host firewall
- Network firewall
- Router ACLs
- Cloud security groups
- Security appliances

### Step 8: Check dynamic routing neighbors

If using a routing protocol, verify neighbor relationships.

Examples:

```text
show ip ospf neighbor
show ip bgp summary
show ip eigrp neighbors
```

### Step 9: Check route preference

If the wrong route is being used, check:

- Prefix length
- Administrative distance
- Metric
- Policy
- Redistribution
- Route filters

### Step 10: Check recent changes

Routing issues are often caused by changes.

Ask:

- Was a VLAN added?
- Was a route removed?
- Was a firewall rule changed?
- Was a routing protocol changed?
- Was an interface shut down?
- Was a VPN tunnel changed?

---

## 29. Useful Routing Commands

### Windows

Show routing table:

```powershell
route print
```

Trace route:

```powershell
tracert example.com
```

Test connectivity:

```powershell
ping 8.8.8.8
Test-NetConnection example.com -Port 443
```

Show IP configuration:

```powershell
ipconfig /all
```

### Linux

Show IPv4 routes:

```bash
ip route
```

Show IPv6 routes:

```bash
ip -6 route
```

Add a temporary IPv4 route:

```bash
sudo ip route add 10.10.20.0/24 via 192.168.1.1
```

Delete a route:

```bash
sudo ip route delete 10.10.20.0/24
```

Trace route:

```bash
traceroute 8.8.8.8
```

Show interfaces:

```bash
ip addr
```

### macOS

Show routing table:

```bash
netstat -rn
```

Trace route:

```bash
traceroute 8.8.8.8
```

Show interfaces:

```bash
ifconfig
```

Add a route:

```bash
sudo route add 10.10.20.0/24 192.168.1.1
```

### Cisco-style commands

Show IPv4 routing table:

```text
show ip route
```

Show IPv6 routing table:

```text
show ipv6 route
```

Show interfaces:

```text
show ip interface brief
```

Show OSPF neighbors:

```text
show ip ospf neighbor
```

Show BGP summary:

```text
show ip bgp summary
```

Show route to a specific destination:

```text
show ip route 10.10.20.0
```

---

## 30. Routing Best Practices

### Keep addressing organized

Good IP addressing makes routing easier.

Use logical blocks for:

- Sites
- VLANs
- Departments
- Data centers
- Cloud environments
- VPNs

### Use summarization where appropriate

Summarization reduces routing table size and improves stability.

But do not summarize carelessly.

### Document static routes

Static routes are easy to forget.

Document:

- Destination
- Next hop
- Reason
- Owner
- Date added
- Change ticket if applicable

### Avoid unnecessary redistribution

Redistribution can be powerful, but it can also be dangerous.

Use filtering and route tags when needed.

### Filter routes at network boundaries

Do not accept or advertise routes blindly.

This is especially important with BGP.

### Use default routes carefully

A default route is useful, but it can hide missing specific routes.

Make sure it points in the correct direction.

### Monitor routing neighbors

For dynamic routing, monitor neighbor status.

A lost neighbor can cause traffic disruption.

### Plan redundancy

Avoid single points of failure.

Use:

- Multiple links
- Multiple routers
- Dynamic routing
- First hop redundancy
- Proper failover testing

### Test failover

A backup route is only useful if it works.

Test it before you need it during an outage.

---

## 31. Quick Summary

Routing moves packets between networks.

The most important points to remember are:

- Routing works at Layer 3
- Routers forward packets based on destination IP address
- A routing table tells the router where to send traffic
- Connected routes come from active interfaces
- Static routes are manually configured
- Default routes are used when no specific route matches
- Dynamic routing protocols learn routes automatically
- Longest prefix match chooses the most specific route
- Next hop is the next router toward the destination
- Metric helps choose the best path inside a routing protocol
- Administrative distance helps choose between different route sources
- Convergence is the process of routing stabilizing after a change
- Summarization can reduce routing table size
- Redistribution shares routes between routing sources
- RIP is simple and older
- OSPF is common for enterprise internal routing
- EIGRP is common in Cisco-oriented environments
- BGP is used for Internet-scale and policy-based routing
- First hop redundancy protects the default gateway
- Routing must work in both directions

If you understand routing tables, default routes, longest prefix match, next hop, metric, and administrative distance, you have a strong foundation for understanding real network routing.
