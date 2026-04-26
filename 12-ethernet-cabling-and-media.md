# 12. Ethernet, Cabling, and Media

Ethernet, cabling, and physical media are the foundation of wired networking. Many problems that look like routing, DNS, firewall, or application issues can actually start at Layer 1 or Layer 2.

This guide explains Ethernet media in a practical way. It covers copper cabling, fiber, connectors, transceivers, speed, duplex, distance limits, common physical problems, and troubleshooting steps.

---

## Table of Contents

- [1. What Ethernet Is](#1-what-ethernet-is)
- [2. Why Cabling and Media Matter](#2-why-cabling-and-media-matter)
- [3. Ethernet and the OSI Model](#3-ethernet-and-the-osi-model)
- [4. Common Ethernet Speeds](#4-common-ethernet-speeds)
- [5. How Ethernet Names Work](#5-how-ethernet-names-work)
- [6. Copper Ethernet](#6-copper-ethernet)
- [7. Twisted Pair Cabling](#7-twisted-pair-cabling)
- [8. Cable Categories](#8-cable-categories)
- [9. RJ45 Connectors](#9-rj45-connectors)
- [10. T568A and T568B Wiring](#10-t568a-and-t568b-wiring)
- [11. Straight-Through and Crossover Cables](#11-straight-through-and-crossover-cables)
- [12. Auto-MDI/MDIX](#12-auto-mdimdix)
- [13. Copper Distance Limits](#13-copper-distance-limits)
- [14. Shielded vs Unshielded Cable](#14-shielded-vs-unshielded-cable)
- [15. Fiber Optic Cabling](#15-fiber-optic-cabling)
- [16. Multimode Fiber](#16-multimode-fiber)
- [17. Single-Mode Fiber](#17-single-mode-fiber)
- [18. Fiber Connectors](#18-fiber-connectors)
- [19. Fiber Polarity](#19-fiber-polarity)
- [20. Transceivers](#20-transceivers)
- [21. Common Transceiver Types](#21-common-transceiver-types)
- [22. DAC and AOC Cables](#22-dac-and-aoc-cables)
- [23. Speed and Duplex](#23-speed-and-duplex)
- [24. Auto-Negotiation](#24-auto-negotiation)
- [25. Half-Duplex vs Full-Duplex](#25-half-duplex-vs-full-duplex)
- [26. MTU and Jumbo Frames](#26-mtu-and-jumbo-frames)
- [27. PoE Basics](#27-poe-basics)
- [28. Common Layer 1 and Layer 2 Issues](#28-common-layer-1-and-layer-2-issues)
- [29. Interface Counters](#29-interface-counters)
- [30. Cable Testing](#30-cable-testing)
- [31. Fiber Testing](#31-fiber-testing)
- [32. Troubleshooting Flow](#32-troubleshooting-flow)
- [33. Useful Commands](#33-useful-commands)
- [34. Best Practices](#34-best-practices)
- [35. Quick Summary](#35-quick-summary)

---

## 1. What Ethernet Is

Ethernet is the most common technology used for wired local area networks.

It defines how devices send frames over physical media such as copper cables, fiber optic cables, and some direct attach cables.

Ethernet is used in:

- Home networks
- Office networks
- Data centers
- Campus networks
- Industrial networks
- Cloud infrastructure
- Server rooms
- Network backbones

When people say “wired network,” they usually mean Ethernet.

---

## 2. Why Cabling and Media Matter

Cabling is easy to ignore until something breaks.

A bad cable, wrong transceiver, dirty fiber connector, or duplex mismatch can cause serious problems.

Common symptoms include:

- No link
- Slow network
- Packet loss
- Random disconnects
- Interface flapping
- CRC errors
- Poor voice or video quality
- Storage traffic problems
- Unstable uplinks

A network can have perfect IP addressing, routing, DNS, and firewall rules, but if the physical media is bad, traffic will still fail.

---

## 3. Ethernet and the OSI Model

Ethernet mainly works at:

- **Layer 1: Physical Layer**
- **Layer 2: Data Link Layer**

### Layer 1

Layer 1 includes:

- Cables
- Connectors
- Electrical signals
- Optical signals
- Radio signals in wireless networks
- Transceivers
- Link speed
- Physical distance limits

### Layer 2

Layer 2 includes:

- Ethernet frames
- MAC addresses
- VLAN tags
- Switching
- Error detection

Cabling and media are mostly Layer 1 topics, but they directly affect Layer 2 behavior.

---

## 4. Common Ethernet Speeds

Ethernet has evolved through many speeds over time.

| Name | Speed | Common Medium |
|---|---:|---|
| `10BASE-T` | 10 Mbps | Copper twisted pair |
| `100BASE-TX` | 100 Mbps | Copper twisted pair |
| `1000BASE-T` | 1 Gbps | Copper twisted pair |
| `2.5GBASE-T` | 2.5 Gbps | Copper twisted pair |
| `5GBASE-T` | 5 Gbps | Copper twisted pair |
| `10GBASE-T` | 10 Gbps | Copper twisted pair |
| `10GBASE-SR` | 10 Gbps | Multimode fiber |
| `10GBASE-LR` | 10 Gbps | Single-mode fiber |
| `25GBASE-SR` | 25 Gbps | Multimode fiber |
| `40GBASE-SR4` | 40 Gbps | Multimode fiber |
| `100GBASE-SR4` | 100 Gbps | Multimode fiber |
| `100GBASE-LR4` | 100 Gbps | Single-mode fiber |
| `400G Ethernet` | 400 Gbps | Data center / carrier media |

### Practical note

In everyday office networks, the most common wired speeds are:

- 1 Gbps
- 2.5 Gbps
- 10 Gbps

In data centers, speeds like 10G, 25G, 40G, 100G, and 400G are common depending on the environment.

---

## 5. How Ethernet Names Work

Ethernet names often follow a pattern.

Example:

```text
1000BASE-T
```

This can be read as:

- `1000` = speed in Mbps
- `BASE` = baseband signaling
- `T` = twisted pair copper

Another example:

```text
10GBASE-SR
```

This means:

- `10G` = 10 Gigabit Ethernet
- `BASE` = baseband signaling
- `SR` = short reach fiber

Common suffixes:

| Suffix | Meaning |
|---|---|
| `T` | Twisted pair copper |
| `SR` | Short reach, usually multimode fiber |
| `LR` | Long reach, usually single-mode fiber |
| `ER` | Extended reach |
| `ZR` | Very long reach, often carrier-style use |
| `CR` | Copper direct attach cable |

You do not need to memorize every Ethernet name, but understanding the naming pattern helps a lot.

---

## 6. Copper Ethernet

Copper Ethernet uses electrical signals over copper wires.

The most common copper Ethernet cable is twisted pair cable with RJ45-style connectors.

Copper is common because it is:

- Cheap
- Easy to install
- Easy to terminate
- Good for endpoint connections
- Common for office devices
- Supported by almost all switches and computers

### Common copper uses

Copper is often used for:

- Desktops
- Laptops through docks
- Printers
- IP phones
- Wireless access points
- Cameras
- Small servers
- Home routers
- Office switch ports

### Copper limitations

Copper has limits:

- Shorter distance than fiber
- More affected by electrical interference
- Can be heavier and thicker at higher categories
- 10G over copper may use more power and generate more heat than fiber or DAC in some environments

---

## 7. Twisted Pair Cabling

Twisted pair cable contains pairs of wires twisted together.

The twists help reduce interference and crosstalk.

A standard Ethernet cable usually has 4 twisted pairs, meaning 8 wires total.

### Why the wires are twisted

Twisting helps reduce:

- Electromagnetic interference
- Crosstalk between pairs
- Noise from nearby cables
- Signal degradation

### Common twisted pair cable types

- UTP: Unshielded Twisted Pair
- STP: Shielded Twisted Pair
- FTP: Foiled Twisted Pair
- S/FTP: Shielded and foiled twisted pair

Most office cabling is UTP unless the environment requires shielding.

---

## 8. Cable Categories

Copper Ethernet cables are grouped into categories.

| Cable Category | Common Use |
|---|---|
| Cat5 | Older networks, mostly legacy |
| Cat5e | Common for 1G Ethernet |
| Cat6 | Better performance, often used for 1G and some 10G at shorter distances |
| Cat6a | Better for 10G up to standard horizontal cabling distances |
| Cat7 | Shielded cable, less common in typical office Ethernet |
| Cat8 | Short high-speed runs, mostly data center style use |

### Cat5e

Cat5e is very common and supports 1G Ethernet in many normal installations.

### Cat6

Cat6 provides better performance than Cat5e and is common in modern building cabling.

It can support 10G in some conditions at shorter distances.

### Cat6a

Cat6a is commonly used when 10GBASE-T is required over normal structured cabling distances.

It has better crosstalk control than Cat6.

### Practical recommendation

For new office cabling, Cat6 or Cat6a is commonly preferred depending on budget and speed requirements.

---

## 9. RJ45 Connectors

Most copper Ethernet connections use an 8-position modular connector commonly called RJ45.

Technically, the exact naming can be more specific, but in normal networking conversation people say RJ45.

### Common RJ45 uses

RJ45 connectors are used for:

- Switch ports
- Router ports
- Patch panels
- Wall jacks
- Ethernet cables
- IP phones
- Access points
- Printers

### Common problems

RJ45-related problems include:

- Broken locking tab
- Poor termination
- Bent contacts
- Loose connection
- Incorrect wiring order
- Cable not fully seated
- Damaged patch panel port

A cable can look fine from the outside and still be badly terminated.

---

## 10. T568A and T568B Wiring

T568A and T568B are wiring standards for Ethernet cable termination.

They define the order of the wire colors inside the connector.

### T568B pin order

A common T568B order is:

```text
1 white/orange
2 orange
3 white/green
4 blue
5 white/blue
6 green
7 white/brown
8 brown
```

### T568A pin order

A common T568A order is:

```text
1 white/green
2 green
3 white/orange
4 blue
5 white/blue
6 orange
7 white/brown
8 brown
```

### Important rule

Use the same standard consistently in a structured cabling environment.

If both ends are wired the same way, it is a straight-through cable.

If one end is T568A and the other is T568B, it is a crossover cable.

---

## 11. Straight-Through and Crossover Cables

### Straight-through cable

A straight-through cable has the same wiring standard on both ends.

Example:

```text
End 1: T568B
End 2: T568B
```

or:

```text
End 1: T568A
End 2: T568A
```

Straight-through cables are the normal cables used in most modern networks.

### Crossover cable

A crossover cable has one standard on one end and another standard on the other end.

Example:

```text
End 1: T568A
End 2: T568B
```

Historically, crossover cables were used to connect similar devices directly, such as switch-to-switch or PC-to-PC.

### Modern note

Most modern Ethernet devices support Auto-MDI/MDIX, so crossover cables are rarely needed in normal situations.

---

## 12. Auto-MDI/MDIX

Auto-MDI/MDIX allows network ports to automatically detect and adjust transmit and receive pairs.

This means modern devices can usually work with either straight-through or crossover cables.

### Why it matters

In older networks, using the wrong cable type could prevent link from coming up.

With Auto-MDI/MDIX, the device can usually correct this automatically.

### Practical note

Even though Auto-MDI/MDIX is common, it is still useful to understand crossover cables when working with older equipment, labs, or exam material.

---

## 13. Copper Distance Limits

A common Ethernet copper distance limit is:

```text
100 meters
```

This usually means:

- Up to 90 meters of permanent horizontal cabling
- Up to 10 meters of patch cables total

### Why distance matters

If copper cable is too long, you may see:

- No link
- Speed downgrade
- CRC errors
- Packet loss
- Intermittent drops
- Poor performance

### Practical warning

A cable that works at 100 Mbps may not work reliably at 1G or 10G if the cable quality, termination, or distance is poor.

Higher speeds need better signal quality.

---

## 14. Shielded vs Unshielded Cable

### UTP

UTP means **Unshielded Twisted Pair**.

It is the most common Ethernet cabling type in offices.

Advantages:

- Flexible
- Affordable
- Easy to install
- Good enough for many environments

### Shielded cable

Shielded cable includes extra protection around pairs or the whole cable.

It can help in environments with high electromagnetic interference.

Examples:

- Factories
- Industrial areas
- Near motors
- Near heavy electrical equipment
- Data centers with specific requirements

### Important warning

Shielded cable must be grounded correctly.

Bad grounding can create problems instead of solving them.

Do not use shielded cable randomly without understanding the installation requirements.

---

## 15. Fiber Optic Cabling

Fiber optic cabling uses light instead of electrical signals.

Fiber is commonly used for:

- Long-distance links
- Building uplinks
- Campus backbones
- Data center connections
- High-speed switch uplinks
- Carrier networks
- Environments with electrical interference

### Advantages of fiber

Fiber has several advantages:

- Longer distance than copper
- Higher speeds
- Immune to electromagnetic interference
- Useful between buildings
- Lower signal loss over long distances
- Good for high-density uplinks

### Disadvantages of fiber

Fiber also has challenges:

- More fragile than copper
- Connectors must be clean
- Optics can be expensive
- Requires correct transceiver type
- Requires correct fiber type
- Can be harder to terminate in the field

---

## 16. Multimode Fiber

Multimode fiber is usually used for shorter distances.

It has a larger core than single-mode fiber and allows multiple light paths.

### Common multimode types

| Type | Common Notes |
|---|---|
| OM1 | Older multimode fiber |
| OM2 | Older / moderate performance |
| OM3 | Common for 10G and higher short links |
| OM4 | Better performance than OM3 |
| OM5 | Wideband multimode fiber |

### Common uses

Multimode fiber is common in:

- Data centers
- Same-building uplinks
- Short switch-to-switch links
- Server connections
- Storage networks

### Common transceiver examples

- 10GBASE-SR
- 25GBASE-SR
- 40GBASE-SR4
- 100GBASE-SR4

The `SR` usually means short reach.

---

## 17. Single-Mode Fiber

Single-mode fiber is usually used for longer distances.

It has a smaller core and carries light in a more focused path.

### Common uses

Single-mode fiber is common for:

- Long-distance links
- Campus links
- Building-to-building links
- ISP connections
- WAN links
- Carrier circuits
- Long data center interconnects

### Common transceiver examples

- 1GBASE-LX
- 10GBASE-LR
- 10GBASE-ER
- 100GBASE-LR4

The `LR` usually means long reach.

### Practical note

Single-mode fiber can support very long distances with the correct optics, but the exact distance depends on the optic, fiber quality, and optical budget.

---

## 18. Fiber Connectors

Fiber cables use different connector types.

Common connector names include:

| Connector | Common Use |
|---|---|
| LC | Very common in modern switches and transceivers |
| SC | Common in older or carrier-style environments |
| ST | Older environments |
| MPO / MTP | High-density multi-fiber links |

### LC connector

LC is very common on SFP and SFP+ transceivers.

It is small and works well in high-density network equipment.

### SC connector

SC is larger than LC and is still seen in some fiber panels and service provider handoffs.

### ST connector

ST is older and less common in modern enterprise switching.

### MPO / MTP connector

MPO and MTP connectors carry multiple fibers in one connector.

They are common for high-speed links such as 40G, 100G, and structured data center fiber systems.

---

## 19. Fiber Polarity

Fiber links often need one side's transmit path to connect to the other side's receive path.

In simple terms:

```text
TX on one side must go to RX on the other side.
```

If polarity is wrong, the link may not come up.

### Common symptom

You plug in the fiber and optics, but there is no link.

Possible causes:

- TX/RX reversed incorrectly
- Wrong patch cable
- Wrong cassette polarity
- Wrong MPO polarity method
- Transmit light not reaching receive side

### Simple troubleshooting step

For a basic duplex LC fiber link, swapping the two fiber strands on one side may help confirm a polarity issue.

Do this carefully and only when appropriate for the environment.

---

## 20. Transceivers

A transceiver is a module that converts electrical signals from a switch or router into optical or electrical signals for a cable.

Common pluggable transceiver families include:

- SFP
- SFP+
- SFP28
- QSFP+
- QSFP28
- QSFP-DD

### Why transceivers matter

A link may fail if the transceiver does not match:

- Speed
- Fiber type
- Wavelength
- Distance
- Connector type
- Switch compatibility
- Cable type

### Example

You cannot expect a short-reach multimode optic to work correctly over a long single-mode fiber path.

The media and optics must match the design.

---

## 21. Common Transceiver Types

### SFP

SFP is commonly used for 1G links.

Examples:

- 1000BASE-SX
- 1000BASE-LX
- 1000BASE-T SFP

### SFP+

SFP+ is commonly used for 10G links.

Examples:

- 10GBASE-SR
- 10GBASE-LR
- 10GBASE-T SFP+

### SFP28

SFP28 is commonly used for 25G links.

It looks similar to SFP+, but it supports higher speed.

### QSFP+

QSFP+ is commonly used for 40G links.

### QSFP28

QSFP28 is commonly used for 100G links.

### QSFP-DD

QSFP-DD is used for higher-speed data center links such as 400G in many modern environments.

### Compatibility note

Some switches restrict which optics are supported.

Always check vendor compatibility when designing production links.

---

## 22. DAC and AOC Cables

### DAC

DAC stands for **Direct Attach Copper**.

A DAC cable is a short cable with transceiver-style ends built in.

It is commonly used for short data center connections, such as:

- Switch to server
- Switch to switch in the same rack
- Top-of-rack links

Advantages:

- Low cost
- Low latency
- Low power
- Simple for short distances

Limitations:

- Short reach
- Thicker and less flexible than fiber
- Compatibility can be vendor-specific

### AOC

AOC stands for **Active Optical Cable**.

It is similar in idea to DAC, but uses fiber inside the cable.

Advantages:

- Longer reach than DAC
- Lighter cable
- Useful for high-speed links

Limitations:

- More expensive than DAC
- Still has fixed ends
- Must match device requirements

---

## 23. Speed and Duplex

Speed is the data rate of the link.

Duplex describes whether traffic can move in both directions at the same time.

### Common speeds

- 10 Mbps
- 100 Mbps
- 1 Gbps
- 2.5 Gbps
- 5 Gbps
- 10 Gbps
- 25 Gbps
- 40 Gbps
- 100 Gbps
- 400 Gbps

### Duplex modes

- Half-duplex
- Full-duplex

Most modern switched Ethernet links use full-duplex.

---

## 24. Auto-Negotiation

Auto-negotiation lets two connected devices agree on link settings.

They may negotiate:

- Speed
- Duplex
- Flow control
- Some physical capabilities

### Common behavior

Most modern devices should be set to auto-negotiate unless there is a specific reason not to.

### Common problem

If one side is hardcoded and the other side is auto, you may get a duplex mismatch.

Example:

```text
Switch side: hardcoded 100 Mbps full-duplex
Server side: auto
```

This can cause poor performance and errors.

### Practical advice

For most modern networks:

```text
Auto / Auto is usually best.
```

If you hardcode settings, make sure both sides match exactly.

---

## 25. Half-Duplex vs Full-Duplex

### Half-duplex

Half-duplex means a device cannot send and receive at the same time.

This was more common in older Ethernet environments with hubs.

### Full-duplex

Full-duplex means a device can send and receive at the same time.

Modern switched Ethernet is normally full-duplex.

### Duplex mismatch

A duplex mismatch happens when one side thinks the link is full-duplex and the other side thinks it is half-duplex.

Common symptoms:

- Very slow performance
- Late collisions
- CRC errors
- Retransmissions
- Poor application performance
- Works a little, but badly

A duplex mismatch is one of the classic Ethernet troubleshooting issues.

---

## 26. MTU and Jumbo Frames

MTU stands for **Maximum Transmission Unit**.

It defines the largest packet size that can be sent on a link without fragmentation at Layer 3.

### Common Ethernet MTU

A normal Ethernet MTU is:

```text
1500 bytes
```

### Jumbo frames

Jumbo frames are larger Ethernet frames, commonly around:

```text
9000 bytes
```

The exact value depends on the device and environment.

### Where jumbo frames are used

Jumbo frames are often used in:

- Storage networks
- Data centers
- Backup networks
- High-throughput server networks

### Warning

Jumbo frames must be configured consistently across the path.

If one device supports jumbo frames and another device does not, you can get connectivity problems or performance issues.

---

## 27. PoE Basics

PoE stands for **Power over Ethernet**.

It allows network cables to carry both data and electrical power.

### Common PoE devices

PoE is commonly used for:

- IP phones
- Wireless access points
- IP cameras
- Door access devices
- Some thin clients
- IoT devices

### Common PoE standards

| Standard | Common Name |
|---|---|
| `802.3af` | PoE |
| `802.3at` | PoE+ |
| `802.3bt` | PoE++ |

### PoE budget

A switch has a total PoE power budget.

Example:

```text
Switch PoE budget: 370W
```

If connected devices need more total power than the switch can provide, some devices may not power on.

### Common PoE problems

- Device needs more power than port can provide
- Switch PoE budget is exhausted
- Bad cable
- Unsupported PoE standard
- PoE disabled on port
- Device negotiates wrong power class

---

## 28. Common Layer 1 and Layer 2 Issues

### No link

Possible causes:

- Cable unplugged
- Bad cable
- Wrong cable type
- Bad port
- Bad transceiver
- Wrong transceiver
- Fiber polarity issue
- Speed mismatch
- Disabled interface

### Interface flapping

Interface flapping means the link goes up and down repeatedly.

Possible causes:

- Loose cable
- Bad patch cable
- Bad optics
- Dirty fiber
- Power issue
- Failing NIC
- Failing switch port
- Bad cabling path

### CRC errors

CRC errors indicate that frames are arriving corrupted.

Possible causes:

- Bad cable
- Electrical interference
- Bad fiber
- Dirty connector
- Duplex mismatch
- Bad transceiver
- Hardware issue

### Speed mismatch

A link may come up at a lower speed than expected.

Example:

```text
Expected: 1 Gbps
Actual:   100 Mbps
```

Possible causes:

- Bad cable pairs
- Old cable category
- Poor termination
- Device limitation
- Auto-negotiation issue

### Duplex mismatch

Symptoms include:

- Slow transfers
- Retransmissions
- Collisions on one side
- CRC or frame errors
- Poor voice/video quality

### Bad cable

A cable can be bad even if link lights are on.

Always test or replace the patch cable when troubleshooting unexplained physical errors.

---

## 29. Interface Counters

Network devices track interface counters.

These counters help identify physical and Layer 2 problems.

Common counters include:

- Input errors
- Output errors
- CRC errors
- Frame errors
- Runts
- Giants
- Collisions
- Late collisions
- Drops
- Discards
- Interface resets
- Link state changes

### CRC errors

CRC errors usually point to corrupted frames.

Check:

- Cable
- Connector
- Transceiver
- Fiber cleanliness
- Interference
- Duplex settings

### Runts

Runts are frames that are smaller than the minimum Ethernet frame size.

Possible causes:

- Collisions
- Bad NIC
- Duplex issues
- Physical layer problems

### Giants

Giants are frames larger than expected.

Possible causes:

- MTU mismatch
- Jumbo frames
- Misconfiguration

### Link state changes

Frequent link state changes can indicate flapping.

Check physical stability first.

---

## 30. Cable Testing

Cable testing helps verify copper cabling.

### Basic cable tester

A simple cable tester can check:

- Continuity
- Wire map
- Open wires
- Short circuits
- Reversed pairs
- Split pairs

### Certification tester

A certification tester is more advanced.

It can test whether a cable meets a specific category standard such as Cat6 or Cat6a.

It may measure:

- Length
- Crosstalk
- Return loss
- Signal quality
- Insertion loss
- Wire map

### When to test cables

Test cables when:

- Link does not come up
- Speed is lower than expected
- CRC errors appear
- A new cabling run is installed
- A port has intermittent drops
- A device works on one cable but not another

---

## 31. Fiber Testing

Fiber testing is used to verify optical links.

### Common fiber checks

- Is the fiber type correct?
- Are connectors clean?
- Is polarity correct?
- Is the transceiver correct?
- Is receive power within range?
- Is transmit power within range?
- Is the distance supported?

### Optical power levels

Many switches can show optical transmit and receive levels if the optic supports DOM or DDM.

These values may appear as:

- Tx power
- Rx power
- Temperature
- Voltage
- Bias current

### Dirty fiber connectors

Dirty connectors are a very common fiber problem.

Dust, oil, or small particles can cause:

- Link failure
- High loss
- CRC errors
- Intermittent errors
- Poor optical power

### Best practice

Always inspect and clean fiber connectors before connecting them in important environments.

---

## 32. Troubleshooting Flow

Use a simple physical-to-logical flow.

### Step 1: Check link status

Is the interface up?

If not, check:

- Cable connected
- Port enabled
- Correct transceiver
- Correct media
- Device powered on

### Step 2: Check speed and duplex

Make sure both sides agree on speed and duplex.

Look for:

- Wrong negotiated speed
- Half-duplex on one side
- Hardcoded mismatch
- Auto-negotiation problems

### Step 3: Check cable or fiber

Try:

- Replacing patch cable
- Moving to another switch port
- Testing cable
- Cleaning fiber
- Checking fiber polarity
- Replacing optics

### Step 4: Check interface counters

Look for:

- CRC errors
- Input errors
- Output errors
- Drops
- Link flaps
- Collisions

### Step 5: Check VLAN and Layer 2 settings

After physical link is stable, check:

- Access VLAN
- Trunk allowed VLANs
- Native VLAN
- Port security
- STP state
- MAC address learning

### Step 6: Check Layer 3

If Layer 1 and Layer 2 look good, check:

- IP address
- Subnet mask
- Default gateway
- ARP or NDP
- Routing

### Step 7: Check application behavior

Only after lower layers are working should you focus on:

- DNS
- Firewall
- Server process
- Application logs
- Authentication

---

## 33. Useful Commands

### Windows

Show network adapter information:

```powershell
Get-NetAdapter
```

Show detailed adapter information:

```powershell
Get-NetAdapter | Format-List *
```

Show IP configuration:

```powershell
ipconfig /all
```

Test connectivity:

```powershell
ping 192.168.1.1
Test-NetConnection example.com -Port 443
```

### Linux

Show interfaces:

```bash
ip link
```

Show IP addresses:

```bash
ip addr
```

Show interface statistics:

```bash
ip -s link
```

Show driver and link details:

```bash
ethtool eth0
```

Show interface errors:

```bash
ethtool -S eth0
```

### macOS

Show interfaces:

```bash
ifconfig
```

Show hardware ports:

```bash
networksetup -listallhardwareports
```

Test connectivity:

```bash
ping 192.168.1.1
```

### Cisco-style commands

Show interface status:

```text
show interfaces status
```

Show interface details:

```text
show interfaces gigabitEthernet 1/0/1
```

Show interface counters:

```text
show interfaces counters errors
```

Show transceiver information:

```text
show interfaces transceiver
```

Show optical details when supported:

```text
show interfaces transceiver detail
```

Show running interface configuration:

```text
show running-config interface gigabitEthernet 1/0/1
```

---

## 34. Best Practices

### Label cables clearly

Good labeling saves time during outages and maintenance.

Label:

- Patch panel ports
- Switch ports
- Cable ends
- Fiber pairs
- Uplink paths
- Rack connections

### Use the right cable category

Choose cable based on expected speed, distance, and environment.

Do not expect old or poor-quality cable to perform well at high speeds.

### Keep fiber clean

Fiber cleanliness is very important.

Use proper inspection and cleaning tools.

Do not touch fiber ends with fingers.

### Avoid sharp bends

Copper and fiber cables both have bend limits.

Sharp bends can damage the cable or degrade signal quality.

### Do not overload cable trays

Too much cable weight or pressure can damage cabling.

Plan cable management carefully.

### Match optics and fiber type

Make sure transceivers match:

- Speed
- Fiber type
- Distance
- Wavelength
- Connector
- Device compatibility

### Use auto-negotiation normally

For most modern Ethernet links, auto-negotiation on both sides is best.

Hardcode only when there is a clear reason.

### Monitor errors

Do not ignore interface errors.

Small error counts over a long time may not be urgent, but rapidly increasing errors usually need investigation.

### Keep spare parts

Useful spare parts include:

- Patch cables
- Fiber jumpers
- SFP modules
- SFP+ modules
- DAC cables
- Cable tester
- Fiber cleaning kit

### Document uplinks

Document important links clearly:

- Source device
- Source port
- Destination device
- Destination port
- Cable type
- Speed
- VLAN/trunk details
- Transceiver type

Good documentation makes troubleshooting much faster.

---

## 35. Quick Summary

Ethernet and physical media are the base of wired networking.

The most important points to remember are:

- Ethernet mainly works at Layer 1 and Layer 2
- Copper Ethernet commonly uses twisted pair cabling
- RJ45 is the common connector for copper Ethernet
- Cat5e is common for 1G
- Cat6 and Cat6a are common in modern installations
- Standard copper Ethernet distance is usually up to 100 meters
- Fiber is used for longer distance, higher speed, and uplinks
- Multimode fiber is usually used for shorter fiber runs
- Single-mode fiber is usually used for longer fiber runs
- LC, SC, ST, and MPO/MTP are common fiber connector types
- SFP, SFP+, SFP28, QSFP+, and QSFP28 are common transceiver families
- Speed and duplex must match correctly
- Auto-negotiation is normally preferred on modern links
- CRC errors often point to physical problems
- Interface flapping usually means a physical or hardware issue
- Dirty fiber connectors can cause serious problems
- PoE provides power over Ethernet for phones, APs, cameras, and similar devices
- Always troubleshoot from physical layer upward

If you understand cables, media types, speed, duplex, distance, and interface errors, you can solve many real-world network problems much faster.
