# 7. Switching, VLANs, and Layer 2

Layer 2 is where Ethernet switching happens. It is the part of networking that deals with MAC addresses, frames, VLANs, trunks, switching loops, and local network segmentation.

This section explains switching and VLANs in a practical way. The goal is to understand how switches forward traffic, why VLANs exist, how trunk links work, and how to troubleshoot common Layer 2 problems.

---

## Table of Contents

- [1. What Layer 2 Means](#1-what-layer-2-means)
- [2. Layer 2 vs Layer 3](#2-layer-2-vs-layer-3)
- [3. Ethernet Frames](#3-ethernet-frames)
- [4. MAC Addresses](#4-mac-addresses)
- [5. How a Switch Learns MAC Addresses](#5-how-a-switch-learns-mac-addresses)
- [6. CAM Table / MAC Address Table](#6-cam-table--mac-address-table)
- [7. Unknown Unicast, Broadcast, and Multicast](#7-unknown-unicast-broadcast-and-multicast)
- [8. Broadcast Domains](#8-broadcast-domains)
- [9. Collision Domains](#9-collision-domains)
- [10. What a VLAN Is](#10-what-a-vlan-is)
- [11. Why VLANs Are Used](#11-why-vlans-are-used)
- [12. Access Ports](#12-access-ports)
- [13. Trunk Ports](#13-trunk-ports)
- [14. 802.1Q Tagging](#14-8021q-tagging)
- [15. Native VLAN](#15-native-vlan)
- [16. Inter-VLAN Routing](#16-inter-vlan-routing)
- [17. SVIs](#17-svis)
- [18. Router-on-a-Stick](#18-router-on-a-stick)
- [19. VLAN Design Example](#19-vlan-design-example)
- [20. STP: Spanning Tree Protocol](#20-stp-spanning-tree-protocol)
- [21. Why Switching Loops Are Dangerous](#21-why-switching-loops-are-dangerous)
- [22. RSTP States](#22-rstp-states)
- [23. Root Bridge Basics](#23-root-bridge-basics)
- [24. Common STP Protection Features](#24-common-stp-protection-features)
- [25. Link Aggregation and LACP](#25-link-aggregation-and-lacp)
- [26. PoE Basics](#26-poe-basics)
- [27. Port Security](#27-port-security)
- [28. Common Layer 2 Problems](#28-common-layer-2-problems)
- [29. Troubleshooting Layer 2 Step by Step](#29-troubleshooting-layer-2-step-by-step)
- [30. Useful Commands](#30-useful-commands)
- [31. Best Practices](#31-best-practices)
- [32. Quick Summary](#32-quick-summary)

---

## 1. What Layer 2 Means

Layer 2 is the **Data Link Layer** of the OSI model.

It is responsible for communication on the local network segment.

At Layer 2, the main addressing system is the **MAC address**.

Common Layer 2 topics include:

- Ethernet frames
- MAC addresses
- Switches
- VLANs
- Trunks
- STP
- Link aggregation
- Port security
- Broadcast domains

Layer 2 does not route traffic between IP networks. That is Layer 3.

Layer 2 is mostly about moving frames inside the same local network.

---

## 2. Layer 2 vs Layer 3

Layer 2 and Layer 3 are closely related, but they do different jobs.

| Layer | Main Address | Main Device | Main Job |
|---|---|---|---|
| Layer 2 | MAC address | Switch | Forward frames inside a local network |
| Layer 3 | IP address | Router / Layer 3 switch | Route packets between networks |

### Simple explanation

Layer 2 asks:

```text
Which local device should receive this frame?
```

Layer 3 asks:

```text
Which network should this packet go to next?
```

### Example

Two devices in the same VLAN can communicate through a switch at Layer 2.

Two devices in different VLANs need Layer 3 routing.

---

## 3. Ethernet Frames

Ethernet uses frames at Layer 2.

A frame is the Layer 2 unit of data.

A simplified Ethernet frame contains:

- Destination MAC address
- Source MAC address
- Optional VLAN tag
- EtherType / length field
- Payload
- Frame Check Sequence (FCS)

### Simplified view

```text
[Destination MAC][Source MAC][VLAN Tag][Type][Payload][FCS]
```

### Important point

Switches forward frames based mainly on the **destination MAC address**.

Routers forward packets based mainly on the **destination IP address**.

---

## 4. MAC Addresses

A MAC address is a Layer 2 hardware address.

It is usually written in hexadecimal.

Example:

```text
00:1A:2B:3C:4D:5E
```

Another common format:

```text
001a.2b3c.4d5e
```

A MAC address is 48 bits long.

### What MAC addresses are used for

MAC addresses are used for local delivery on Ethernet networks.

When a device sends traffic to another device on the same LAN, it sends an Ethernet frame to the destination MAC address.

### MAC address parts

A MAC address often has two conceptual parts:

- Vendor portion
- Device-specific portion

The vendor portion is called the **OUI**, or Organizationally Unique Identifier.

---

## 5. How a Switch Learns MAC Addresses

Switches learn MAC addresses automatically.

When a frame enters a switch port, the switch looks at the **source MAC address**.

It then records:

```text
This MAC address is reachable through this port.
```

### Example

A switch receives a frame on port `Gi0/1` with this source MAC:

```text
00:11:22:33:44:55
```

The switch learns:

```text
00:11:22:33:44:55 -> Gi0/1
```

Later, if another device sends traffic to `00:11:22:33:44:55`, the switch knows to forward it out `Gi0/1`.

---

## 6. CAM Table / MAC Address Table

The MAC address table is also called the **CAM table**.

CAM stands for **Content Addressable Memory**.

The table maps MAC addresses to switch ports.

### Example MAC table

| VLAN | MAC Address | Port |
|---:|---|---|
| 10 | `00:11:22:33:44:55` | `Gi0/1` |
| 10 | `00:11:22:33:44:66` | `Gi0/2` |
| 20 | `00:11:22:33:44:77` | `Gi0/3` |

### Important point

The same MAC address table is usually VLAN-aware.

That means the switch tracks MAC addresses per VLAN.

A MAC address learned in VLAN 10 is not the same forwarding context as a MAC address learned in VLAN 20.

---

## 7. Unknown Unicast, Broadcast, and Multicast

A switch handles different frame types differently.

### Known unicast

A known unicast frame has a destination MAC address already in the MAC table.

The switch forwards it only out the correct port.

### Unknown unicast

An unknown unicast frame has a destination MAC address that is not in the MAC table.

The switch floods it out all ports in the same VLAN except the port it came from.

### Broadcast

A broadcast frame is sent to all devices in the same broadcast domain.

The broadcast MAC address is:

```text
ff:ff:ff:ff:ff:ff
```

Broadcasts are flooded within the VLAN.

### Multicast

Multicast is one-to-many communication.

Switches may flood multicast traffic or handle it more intelligently if features like IGMP snooping are enabled.

---

## 8. Broadcast Domains

A broadcast domain is the area where a Layer 2 broadcast can travel.

In a switched network, each VLAN is usually its own broadcast domain.

### Example

If a switch has these VLANs:

- VLAN 10
- VLAN 20
- VLAN 30

Then it usually has three separate broadcast domains.

A broadcast in VLAN 10 does not automatically go to VLAN 20.

### Why this matters

Broadcast domains affect:

- ARP traffic
- DHCP traffic
- Network noise
- Segmentation
- Security boundaries
- Troubleshooting scope

---

## 9. Collision Domains

A collision domain is the area where Ethernet collisions can occur.

In old hub-based networks, many devices shared the same collision domain.

In modern switched full-duplex Ethernet, collisions are usually not a normal issue.

### Modern switch behavior

Each switch port is usually its own collision domain.

Full-duplex links do not use CSMA/CD in the same practical way old half-duplex Ethernet did.

### Why you still need to know this

Collision domain is a common exam and interview term.

It is also useful for understanding older Ethernet behavior.

---

## 10. What a VLAN Is

A VLAN is a **Virtual Local Area Network**.

A VLAN creates a logical Layer 2 network.

Devices in the same VLAN behave like they are in the same local network, even if they are connected to different physical switch ports.

Devices in different VLANs are separated at Layer 2.

### Simple idea

A VLAN is like a separate switch created inside the same physical switch.

### Example

One physical switch can carry:

```text
VLAN 10 -> Users
VLAN 20 -> Servers
VLAN 30 -> Voice
VLAN 40 -> Guest Wi-Fi
VLAN 99 -> Management
```

Each VLAN is a separate broadcast domain.

---

## 11. Why VLANs Are Used

VLANs are used to organize and segment networks.

### Common reasons to use VLANs

- Separate users from servers
- Separate guests from internal devices
- Separate voice traffic from data traffic
- Separate management interfaces
- Reduce broadcast scope
- Improve security policy design
- Make troubleshooting easier
- Support multi-tenant or multi-department networks

### Example design

| VLAN | Purpose | Subnet |
|---:|---|---|
| 10 | Users | `10.10.10.0/24` |
| 20 | Servers | `10.10.20.0/24` |
| 30 | Voice | `10.10.30.0/24` |
| 40 | Guest | `10.10.40.0/24` |
| 99 | Management | `10.10.99.0/24` |

### Important point

A VLAN is Layer 2.

An IP subnet is Layer 3.

In many designs, each VLAN gets its own IP subnet.

---

## 12. Access Ports

An access port carries traffic for one VLAN.

It is commonly used for end-user devices.

Examples:

- Desktop PC
- Printer
- IP phone
- Camera
- Access point management port
- Server connected to a simple network

### Example

A port assigned to VLAN 10:

```text
interface GigabitEthernet0/1
 switchport mode access
 switchport access vlan 10
```

A device connected to this port belongs to VLAN 10.

### Untagged traffic

Most access ports send and receive untagged Ethernet frames from the endpoint.

The switch internally associates that traffic with the configured access VLAN.

---

## 13. Trunk Ports

A trunk port carries traffic for multiple VLANs.

Trunks are commonly used between:

- Switch to switch
- Switch to router
- Switch to firewall
- Switch to hypervisor host
- Switch to wireless access point

### Example

A trunk between two switches may carry:

```text
VLAN 10
VLAN 20
VLAN 30
VLAN 99
```

### Cisco-style example

```text
interface GigabitEthernet0/24
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,99
```

### Important point

If a VLAN is not allowed on the trunk, traffic for that VLAN cannot cross that trunk.

This is a very common troubleshooting issue.

---

## 14. 802.1Q Tagging

802.1Q is the standard method for VLAN tagging on Ethernet trunks.

It adds a VLAN tag into the Ethernet frame.

The VLAN tag tells the receiving switch which VLAN the frame belongs to.

### Simple view

Without VLAN tag:

```text
[Destination MAC][Source MAC][Type][Payload][FCS]
```

With 802.1Q tag:

```text
[Destination MAC][Source MAC][802.1Q Tag][Type][Payload][FCS]
```

### VLAN ID range

VLAN IDs are usually from:

```text
1 to 4094
```

VLAN `0` and `4095` are reserved and not used as normal VLANs.

---

## 15. Native VLAN

The native VLAN is the VLAN that carries untagged traffic on an 802.1Q trunk.

On many Cisco switches, the default native VLAN is VLAN 1.

### Example

If the native VLAN is 99:

```text
interface GigabitEthernet0/24
 switchport mode trunk
 switchport trunk native vlan 99
```

Then untagged frames received on that trunk are treated as VLAN 99.

### Important warning

Native VLAN mismatches can cause confusing Layer 2 problems.

Example:

- Switch A native VLAN: 99
- Switch B native VLAN: 1

This mismatch can cause traffic to be placed into the wrong VLAN.

### Best practice

Many environments use an unused VLAN as the native VLAN and avoid carrying user traffic untagged over trunks.

---

## 16. Inter-VLAN Routing

Devices in different VLANs need routing to communicate.

A switch can forward frames inside a VLAN, but it cannot route between VLANs unless it has Layer 3 capability.

### Example

Device A:

```text
VLAN 10
IP: 10.10.10.25
```

Device B:

```text
VLAN 20
IP: 10.10.20.25
```

These are different subnets.

They need a router, firewall, or Layer 3 switch to communicate.

### Common inter-VLAN routing methods

- Layer 3 switch with SVIs
- Router-on-a-stick
- Firewall as default gateway
- Dedicated router interfaces

---

## 17. SVIs

SVI stands for **Switched Virtual Interface**.

An SVI is a Layer 3 interface for a VLAN on a Layer 3 switch.

### Example

```text
interface Vlan10
 ip address 10.10.10.1 255.255.255.0
```

This interface can be the default gateway for devices in VLAN 10.

### Example VLAN gateway design

| VLAN | SVI IP |
|---:|---|
| 10 | `10.10.10.1` |
| 20 | `10.10.20.1` |
| 30 | `10.10.30.1` |
| 99 | `10.10.99.1` |

### Important point

An SVI needs the VLAN to exist, and the VLAN usually needs at least one active port or trunk carrying that VLAN for the SVI to come up, depending on the platform.

---

## 18. Router-on-a-Stick

Router-on-a-stick is an inter-VLAN routing design where one physical router interface carries multiple VLANs using subinterfaces.

The switch port to the router is a trunk.

The router has subinterfaces for each VLAN.

### Example idea

```text
Switch trunk -> Router physical interface
```

Router subinterfaces:

```text
G0/0.10 -> VLAN 10 -> 10.10.10.1/24
G0/0.20 -> VLAN 20 -> 10.10.20.1/24
G0/0.30 -> VLAN 30 -> 10.10.30.1/24
```

### When it is used

Router-on-a-stick is common in:

- Labs
- Small networks
- Training environments
- Simple branch networks

In larger networks, Layer 3 switches are often preferred for inter-VLAN routing.

---

## 19. VLAN Design Example

Here is a simple office VLAN design.

| VLAN | Name | Subnet | Gateway |
|---:|---|---|---|
| 10 | Users | `10.10.10.0/24` | `10.10.10.1` |
| 20 | Servers | `10.10.20.0/24` | `10.10.20.1` |
| 30 | Voice | `10.10.30.0/24` | `10.10.30.1` |
| 40 | Guest | `10.10.40.0/24` | `10.10.40.1` |
| 99 | Management | `10.10.99.0/24` | `10.10.99.1` |

### Access port examples

A user PC port:

```text
switchport mode access
switchport access vlan 10
```

An IP phone port may use a voice VLAN:

```text
switchport mode access
switchport access vlan 10
switchport voice vlan 30
```

A switch uplink:

```text
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,99
```

### Security policy example

- Users can access servers only on required ports
- Guest VLAN can access the Internet only
- Management VLAN can access switch and firewall admin interfaces
- Voice VLAN is prioritized for QoS

---

## 20. STP: Spanning Tree Protocol

STP stands for **Spanning Tree Protocol**.

Its job is to prevent Layer 2 switching loops.

Switching loops are dangerous because Ethernet frames do not have a TTL like IP packets.

If a loop exists, frames can circulate endlessly.

### What STP does

STP builds a loop-free logical topology by blocking some redundant paths.

This allows physical redundancy without creating active Layer 2 loops.

### Simple example

If three switches are connected in a triangle, there is a loop.

STP blocks one path so traffic has only one active Layer 2 path.

If an active path fails, STP can unblock a backup path.

---

## 21. Why Switching Loops Are Dangerous

Layer 2 loops can cause serious problems very quickly.

### Common symptoms

- Broadcast storms
- High switch CPU
- MAC address table instability
- Network-wide slowness
- Intermittent connectivity
- DHCP failures
- ARP problems
- Devices disconnecting randomly

### Broadcast storm

A broadcast storm happens when broadcast frames loop and multiply through the network.

Because broadcasts are flooded, a loop can make traffic grow extremely fast.

### MAC flapping

MAC flapping happens when a switch sees the same MAC address moving rapidly between ports.

This often suggests a loop or incorrect cabling.

---

## 22. RSTP States

RSTP stands for **Rapid Spanning Tree Protocol**.

It is a faster version of STP.

Common RSTP port states are:

- Discarding
- Learning
- Forwarding

### Discarding

The port does not forward user traffic.

It may still participate in STP control traffic.

### Learning

The port learns MAC addresses but does not forward user traffic yet.

### Forwarding

The port forwards normal traffic.

### Why RSTP matters

RSTP usually converges faster than traditional STP, which means the network recovers faster after topology changes.

---

## 23. Root Bridge Basics

STP elects a root bridge.

The root bridge is the reference point for the spanning tree.

Switches calculate the best path toward the root bridge.

### Bridge ID

The root bridge is selected based on the lowest Bridge ID.

Bridge ID includes:

- Bridge priority
- MAC address

### Root bridge design

You should not leave root bridge selection to chance.

In a well-designed network, the root bridge should usually be a core or distribution switch.

### Example idea

Primary core switch:

```text
Lowest STP priority
```

Secondary core switch:

```text
Second-lowest STP priority
```

Access switches should usually not become the STP root.

---

## 24. Common STP Protection Features

Switches often support protection features to make STP safer.

The names vary by vendor, but the ideas are common.

### PortFast / Edge Port

Used on ports connected to end devices.

It lets the port move to forwarding quickly.

Use it on access ports, not switch-to-switch links.

### BPDU Guard

If a port receives a BPDU when it should not, the switch can disable the port.

This helps prevent accidental switch connections on edge ports.

### Root Guard

Prevents a port from becoming a path to a better root bridge.

Useful to protect the intended STP topology.

### Loop Guard

Helps protect against unidirectional link problems that could cause loops.

### BPDU Filter

Can suppress BPDUs in certain cases.

Use carefully. Misuse can create dangerous situations.

---

## 25. Link Aggregation and LACP

Link aggregation combines multiple physical links into one logical link.

This can improve:

- Redundancy
- Bandwidth
- Load distribution

### LACP

LACP stands for **Link Aggregation Control Protocol**.

It is the IEEE standard for negotiating link bundles.

The standard is:

```text
802.3ad / 802.1AX
```

### Common names

Depending on vendor, link aggregation may be called:

- EtherChannel
- Port-channel
- LAG
- Bond
- Team

### Example

Four 1 Gbps links can be bundled into one logical port-channel.

```text
Gi0/1 + Gi0/2 + Gi0/3 + Gi0/4 -> Port-channel1
```

### Important note

A single traffic flow usually does not use all links at the same time.

Traffic is distributed based on a hashing algorithm, often using fields like:

- Source MAC
- Destination MAC
- Source IP
- Destination IP
- TCP/UDP ports

---

## 26. PoE Basics

PoE stands for **Power over Ethernet**.

It allows a switch to provide electrical power through the Ethernet cable.

Common PoE devices include:

- IP phones
- Wireless access points
- Security cameras
- Door access devices
- IoT devices

### Common PoE standards

| Standard | Common Name | Notes |
|---|---|---|
| `802.3af` | PoE | Basic PoE |
| `802.3at` | PoE+ | Higher power than PoE |
| `802.3bt` | PoE++ | Higher power for newer devices |

### Troubleshooting PoE

Check:

- Does the switch support PoE?
- Is PoE enabled on the port?
- Does the device need more power than the switch can provide?
- Is the switch PoE budget full?
- Is the cable good?

---

## 27. Port Security

Port security limits which MAC addresses can use a switch port.

It is commonly used on access ports.

### What port security can do

- Limit number of MAC addresses
- Allow only specific MAC addresses
- Learn MAC addresses dynamically
- Disable or restrict a port after violation

### Example use

A port should allow only one user device.

If another device or mini-switch is connected, the port can block or shut down.

### Common violation actions

Vendor terms vary, but typical actions include:

- Protect
- Restrict
- Shutdown

### Important warning

Port security can cause user issues if phones, docks, virtual machines, or multiple devices legitimately use the same port.

Plan before enabling it widely.

---

## 28. Common Layer 2 Problems

Layer 2 issues can be very disruptive.

### Wrong VLAN

A device is connected to a port in the wrong VLAN.

Symptoms:

- Gets wrong IP subnet
- Cannot reach expected gateway
- Cannot access expected resources

### VLAN not allowed on trunk

A VLAN exists, but it is not allowed across an uplink trunk.

Symptoms:

- Works on one switch but not another
- Devices in same VLAN cannot communicate across switches

### Native VLAN mismatch

Two trunk ports have different native VLANs.

Symptoms:

- Strange connectivity
- CDP/LLDP warnings on some platforms
- Traffic appears in wrong VLAN

### STP blocking

A port may be blocked by STP.

Symptoms:

- Link is physically up but not forwarding
- Redundant path not active

### MAC flapping

The same MAC address appears on different ports repeatedly.

Symptoms:

- Intermittent traffic
- Switch logs show MAC move events
- Possible loop

### Duplex mismatch

One side is full-duplex and the other side is half-duplex.

Symptoms:

- Slow performance
- CRC errors
- Late collisions on old platforms
- Poor throughput

### Bad cable or transceiver

Physical problems can appear as Layer 2 instability.

Symptoms:

- Interface flaps
- CRC errors
- Link drops
- Speed negotiation issues

---

## 29. Troubleshooting Layer 2 Step by Step

Use a structured approach.

### Step 1: Check physical link

Ask:

- Is the cable connected?
- Is the link light on?
- Is the interface up?
- Are speed and duplex correct?
- Are there errors on the interface?

### Step 2: Check the port mode

Is the port an access port or trunk port?

A user device usually needs an access port.

A switch uplink usually needs a trunk port.

### Step 3: Check VLAN assignment

For access ports:

- Is the correct access VLAN configured?
- Does the VLAN exist on the switch?

For trunk ports:

- Is the VLAN allowed on the trunk?
- Is the native VLAN correct?

### Step 4: Check MAC address learning

Look for the device MAC address in the MAC table.

If the switch does not learn the MAC, check:

- Cable
- NIC
- Port state
- VLAN
- STP
- Port security

### Step 5: Check STP

Check if the port is forwarding or blocking.

If the port is blocked, find out whether that is expected.

### Step 6: Check for loops

Look for:

- MAC flapping
- Broadcast storms
- High CPU
- Many topology changes
- Unexpected cables between switches

### Step 7: Check Layer 3 only after Layer 2 looks correct

If Layer 2 is working, then move to:

- IP address
- Subnet mask
- Default gateway
- ARP
- Routing
- Firewall rules

---

## 30. Useful Commands

Command syntax changes by vendor, but these examples show common Cisco-style commands and general host commands.

### Switch commands

Show VLANs:

```text
show vlan brief
```

Show trunk links:

```text
show interfaces trunk
```

Show MAC address table:

```text
show mac address-table
```

Show MACs on a specific interface:

```text
show mac address-table interface gi0/1
```

Show interface status:

```text
show interfaces status
```

Show interface details and errors:

```text
show interfaces gi0/1
```

Show STP information:

```text
show spanning-tree
```

Show STP for a VLAN:

```text
show spanning-tree vlan 10
```

Show port-channel summary:

```text
show etherchannel summary
```

Show CDP neighbors:

```text
show cdp neighbors
```

Show LLDP neighbors:

```text
show lldp neighbors
```

### Windows host commands

Show IP and MAC information:

```powershell
ipconfig /all
```

Show ARP table:

```powershell
arp -a
```

Test gateway reachability:

```powershell
ping 10.10.10.1
```

### Linux host commands

Show interfaces:

```bash
ip link
ip addr
```

Show ARP / neighbor table:

```bash
ip neigh
```

Check link details:

```bash
ethtool eth0
```

Capture Ethernet traffic:

```bash
tcpdump -i eth0
```

---

## 31. Best Practices

### Use clear VLAN names

Good names help operations.

Example:

```text
VLAN 10  USERS
VLAN 20  SERVERS
VLAN 30  VOICE
VLAN 40  GUEST
VLAN 99  MGMT
```

### Avoid using VLAN 1 for important traffic

VLAN 1 is often the default VLAN.

Many environments avoid using it for user, management, or native VLAN traffic.

### Restrict trunks

Do not allow every VLAN on every trunk unless needed.

Better:

```text
Allowed VLANs: 10,20,30,99
```

Instead of:

```text
Allowed VLANs: all
```

### Set an unused native VLAN

Use an unused VLAN as the native VLAN where appropriate.

Make sure both sides match.

### Disable unused ports

Unused access ports should usually be disabled or placed in an unused VLAN.

### Use STP protection

On edge ports, consider:

- PortFast / edge port
- BPDU Guard

On important topology boundaries, consider:

- Root Guard
- Loop Guard

### Plan the STP root bridge

Do not let the network randomly choose the root bridge.

Set root bridge priority intentionally.

### Document uplinks and VLANs

Document:

- Access VLANs
- Trunk allowed VLANs
- Native VLANs
- Port-channel members
- STP root switches
- Management VLANs

### Monitor Layer 2 errors

Watch for:

- CRC errors
- Interface flaps
- MAC flapping
- STP topology changes
- High broadcast traffic
- Port-security violations

---

## 32. Quick Summary

Layer 2 is where Ethernet switching happens.

The most important points to remember are:

- Layer 2 uses MAC addresses
- Switches forward Ethernet frames
- Switches learn source MAC addresses
- The MAC address table maps MAC addresses to ports
- Unknown unicast and broadcast traffic are flooded within the VLAN
- Each VLAN is a separate broadcast domain
- Access ports carry one VLAN
- Trunk ports carry multiple VLANs
- 802.1Q adds VLAN tags to frames
- The native VLAN carries untagged trunk traffic
- Devices in different VLANs need Layer 3 routing
- SVIs are Layer 3 interfaces for VLANs
- STP prevents switching loops
- RSTP converges faster than traditional STP
- LACP bundles multiple physical links into one logical link
- PoE powers devices over Ethernet
- Wrong VLANs, trunk issues, STP blocking, and loops are common Layer 2 problems

If you understand MAC learning, VLAN separation, trunks, native VLANs, and STP, you have a strong foundation for switching and Layer 2 troubleshooting.
