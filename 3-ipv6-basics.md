# 3. IPv6 Basics

IPv6 is the newer version of the Internet Protocol. It was designed mainly to solve the address shortage problem in IPv4, but it also brings other improvements in routing, address design, and network behavior.

This guide explains IPv6 in a practical and detailed way. The goal is not just to memorize facts, but to understand how IPv6 works, how addresses are written, and what makes it different from IPv4.

---

## Table of Contents

- [1. What IPv6 Is](#1-what-ipv6-is)
- [2. Why IPv6 Exists](#2-why-ipv6-exists)
- [3. IPv6 Address Size](#3-ipv6-address-size)
- [4. How IPv6 Addresses Are Written](#4-how-ipv6-addresses-are-written)
- [5. Rules for Shortening IPv6 Addresses](#5-rules-for-shortening-ipv6-addresses)
- [6. Address Structure](#6-address-structure)
- [7. Common IPv6 Address Types](#7-common-ipv6-address-types)
- [8. Important IPv6 Ranges](#8-important-ipv6-ranges)
- [9. Prefix Lengths in IPv6](#9-prefix-lengths-in-ipv6)
- [10. Subnetting in IPv6](#10-subnetting-in-ipv6)
- [11. Link-Local Addresses](#11-link-local-addresses)
- [12. Unique Local Addresses](#12-unique-local-addresses)
- [13. Global Unicast Addresses](#13-global-unicast-addresses)
- [14. Multicast in IPv6](#14-multicast-in-ipv6)
- [15. Anycast in IPv6](#15-anycast-in-ipv6)
- [16. No Broadcast in IPv6](#16-no-broadcast-in-ipv6)
- [17. Neighbor Discovery Protocol (NDP)](#17-neighbor-discovery-protocol-ndp)
- [18. SLAAC and DHCPv6](#18-slaac-and-dhcpv6)
- [19. ICMPv6 and Why It Matters](#19-icmpv6-and-why-it-matters)
- [20. IPv6 Routing Basics](#20-ipv6-routing-basics)
- [21. IPv4 vs IPv6](#21-ipv4-vs-ipv6)
- [22. Common IPv6 Examples](#22-common-ipv6-examples)
- [23. Common Mistakes and Confusing Points](#23-common-mistakes-and-confusing-points)
- [24. Useful IPv6 Commands](#24-useful-ipv6-commands)
- [25. Quick Summary](#25-quick-summary)

---

## 1. What IPv6 Is

IPv6 stands for **Internet Protocol version 6**.

It is the Layer 3 protocol used for logical addressing and routing in modern IP networks. Just like IPv4, it tells devices how to identify themselves and how to send packets across networks.

IPv6 does the same basic job as IPv4, but with a much larger address space and a cleaner design.

---

## 2. Why IPv6 Exists

The biggest reason IPv6 was created is simple: **IPv4 does not have enough addresses for the modern Internet**.

IPv4 uses 32-bit addresses, which gives about 4.3 billion total addresses. That sounded huge many years ago, but it is not enough for the number of devices, users, servers, phones, cloud systems, and IoT devices in the world today.

IPv6 solves this problem by using 128-bit addresses.

IPv6 was also designed to improve other areas:

- More efficient address allocation
- Better support for hierarchical routing
- Simpler end-to-end addressing
- Less need for NAT in many designs
- Cleaner support for multicast
- Built-in support for automatic configuration
- More modern neighbor discovery behavior

IPv6 does **not** automatically make a network faster or more secure by itself, but it gives network engineers a much larger and more flexible addressing system.

---

## 3. IPv6 Address Size

An IPv6 address is **128 bits** long.

That is much larger than an IPv4 address, which is only 32 bits.

### Example IPv6 Address

```text
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

Because 128 bits is such a large space, IPv6 can provide an enormous number of unique addresses.

You do not need to memorize the exact number, but it is often described as:

- **2^128 total addresses**

That number is so large that practical address exhaustion is not a normal concern in the same way it is with IPv4.

---

## 4. How IPv6 Addresses Are Written

IPv6 addresses are written in **hexadecimal**.

Hexadecimal uses:

- `0 1 2 3 4 5 6 7 8 9`
- `a b c d e f`

An IPv6 address is divided into **8 groups**, and each group contains **4 hexadecimal characters**.

Groups are separated by colons.

### Full Example

```text
2001:0db8:0000:0000:0000:ff00:0042:8329
```

Each group represents 16 bits.

So:

- 8 groups
- 16 bits per group
- Total = 128 bits

---

## 5. Rules for Shortening IPv6 Addresses

IPv6 addresses are long, so there are standard rules to shorten them.

### Rule 1: Remove leading zeros in any group

You can remove zeros at the **start** of a group.

Example:

```text
2001:0db8:0000:0000:0000:ff00:0042:8329
```

becomes:

```text
2001:db8:0:0:0:ff00:42:8329
```

### Rule 2: Replace one consecutive sequence of all-zero groups with `::`

You can compress **one** continuous run of zero groups using a double colon.

So:

```text
2001:db8:0:0:0:ff00:42:8329
```

becomes:

```text
2001:db8::ff00:42:8329
```

### Important Rule

You can use `::` **only once in one address**.

Why? Because if you used it twice, the address would become ambiguous and the reader would not know how many zero groups were removed in each place.

### Examples

#### Full form
```text
fe80:0000:0000:0000:021c:7eff:fe11:2233
```

#### Shortened form
```text
fe80::21c:7eff:fe11:2233
```

#### Another example
```text
2001:0000:0000:0000:0000:0000:0000:0001
```

becomes:

```text
2001::1
```

---

## 6. Address Structure

IPv6 addresses are usually thought of in two main parts:

- **Network prefix**
- **Interface identifier**

A very common subnet size in IPv6 is **/64**.

That means:

- The first 64 bits identify the network
- The last 64 bits identify the interface

### Example

```text
2001:db8:abcd:0012::/64
```

In that subnet:

- `2001:db8:abcd:0012` is the network part
- The remaining 64 bits are used for hosts or interfaces

This structure makes IPv6 easy to organize and scale in large networks.

---

## 7. Common IPv6 Address Types

IPv6 has several address types. The most important ones are:

### Unicast

A unicast address identifies **one interface**.

This is the normal one-to-one communication model.

### Multicast

A multicast address identifies a **group of interfaces**.

Packets sent to a multicast address go to all members of that group.

### Anycast

An anycast address is assigned to **multiple devices**, but traffic goes to the **nearest one**, based on routing.

### Broadcast

IPv6 does **not** use broadcast.

Instead, it uses multicast for cases where IPv4 would often use broadcast.

---

## 8. Important IPv6 Ranges

Here are some of the most important IPv6 ranges you should know.

| Prefix | Purpose |
|---|---|
| `::1/128` | Loopback |
| `::/0` | Default route |
| `fe80::/10` | Link-local |
| `fc00::/7` | Unique Local Address (ULA) |
| `2000::/3` | Global unicast |
| `ff00::/8` | Multicast |

### `::1/128`
This is the IPv6 loopback address. It is the IPv6 version of `127.0.0.1` in IPv4.

### `::/0`
This is the default route in IPv6, similar to `0.0.0.0/0` in IPv4.

### `fe80::/10`
This range is used for link-local addresses. These are very important in IPv6 and are used on the local network segment.

### `fc00::/7`
This is the Unique Local Address space, which is somewhat similar in idea to private IPv4 addressing.

### `2000::/3`
This is the main global unicast space used for public IPv6 addressing.

### `ff00::/8`
This is the multicast range.

---

## 9. Prefix Lengths in IPv6

IPv6 uses prefix length notation just like CIDR in IPv4.

Examples:

- `/64`
- `/56`
- `/48`
- `/128`

### Very Common Prefixes

#### `/64`
This is the standard subnet size in IPv6.

In normal network design, a single IPv6 LAN is usually a `/64`.

#### `/128`
This represents one specific interface or one single address.

#### `/48`
A common allocation size for a site or organization.

#### `/56`
A common size that an ISP may delegate to a home or small business customer.

The exact allocation depends on the provider and environment, but `/64` is the most important one to remember for ordinary subnets.

---

## 10. Subnetting in IPv6

IPv6 subnetting is different in feeling from IPv4 subnetting.

In IPv4, people often try to save address space and count usable hosts carefully.

In IPv6, address space is so large that engineers usually do **not** spend time trying to conserve host addresses inside each subnet.

Instead, subnetting is more about:

- Clean design
- Logical structure
- Aggregation
- Simpler routing
- Easier management

### Example

Suppose you receive this prefix:

```text
2001:db8:1200::/48
```

You can create many `/64` networks from it, such as:

```text
2001:db8:1200:0001::/64
2001:db8:1200:0002::/64
2001:db8:1200:0003::/64
2001:db8:1200:0004::/64
```

This is one reason IPv6 is much easier to scale in larger environments.

---

## 11. Link-Local Addresses

Every IPv6-enabled interface typically has a **link-local address**.

These addresses come from the range:

```text
fe80::/10
```

### What link-local addresses are used for

They are used for communication on the same local Layer 2 segment.

Typical uses include:

- Neighbor discovery
- Router discovery
- Local communication on a LAN
- Next-hop communication between routers

### Important point

A link-local address is **not routable across the Internet**.

It only works on the local link.

### Example

```text
fe80::1
```

In practice, systems often generate a longer link-local address automatically.

---

## 12. Unique Local Addresses

Unique Local Addresses, or **ULA**, are local-use IPv6 addresses.

Their range is:

```text
fc00::/7
```

In practice, many local deployments use addresses that start with `fd`.

### Why ULAs are used

They are useful for:

- Internal-only networks
- Lab networks
- Enterprise internal systems
- Environments that do not need public Internet routing for every device

ULAs are similar in concept to private IPv4 ranges like `10.0.0.0/8` or `192.168.0.0/16`, although the design is not exactly the same.

### Important point

ULAs are **not globally routed on the public Internet**.

---

## 13. Global Unicast Addresses

A **global unicast address** is a normal public IPv6 address.

The main range is:

```text
2000::/3
```

These addresses are globally routable, which means they can be used across the public Internet when routing and policy allow it.

### Example

```text
2001:db8::1
```

Note that `2001:db8::/32` is commonly used in documentation and examples. It is reserved for documentation, so you should use it in notes, labs, and diagrams rather than on real public networks.

---

## 14. Multicast in IPv6

IPv6 makes heavy use of multicast.

Multicast addresses are in:

```text
ff00::/8
```

Instead of sending packets to every device with broadcast, IPv6 often sends traffic only to devices that joined a particular multicast group.

### Why multicast matters in IPv6

It is used for:

- Neighbor discovery
- Router advertisements
- Some service discovery functions
- Group communication

### Example multicast ideas

- All nodes on a link
- All routers on a link
- Solicited-node multicast groups

You do not need to memorize every multicast group at the start, but you should understand that multicast is a core part of IPv6 behavior.

---

## 15. Anycast in IPv6

Anycast means that the **same address is assigned to multiple devices**, and routing sends traffic to the nearest or best one.

This is useful for:

- Distributed services
- Resilient infrastructure
- DNS services
- Load distribution at a routing level

From the user perspective, it looks like one destination address. In reality, multiple systems may share it.

---

## 16. No Broadcast in IPv6

IPv6 does **not** have broadcast addresses.

That is one of the most important differences from IPv4.

In IPv4, broadcast is used to send traffic to all hosts in a subnet. In IPv6, this behavior is replaced mostly by multicast.

### Why this matters

Using multicast instead of broadcast can reduce unnecessary traffic and gives the protocol a cleaner design.

So if you ask, “What is the IPv6 broadcast address?” the correct answer is:

**There is no broadcast in IPv6.**

---

## 17. Neighbor Discovery Protocol (NDP)

IPv6 does not use ARP.

Instead, it uses **Neighbor Discovery Protocol**, usually called **NDP**.

NDP works through ICMPv6 and handles several jobs that are very important on IPv6 networks.

### NDP helps with:

- Discovering neighboring devices
- Learning link-layer addresses
- Finding routers
- Detecting duplicate addresses
- Discovering prefixes and network settings

### Compare with IPv4

In IPv4, ARP resolves an IP address to a MAC address.

In IPv6, NDP handles similar neighbor functions, but with more features and a cleaner design model.

---

## 18. SLAAC and DHCPv6

IPv6 supports multiple ways for a device to get its address and settings.

### SLAAC

**SLAAC** stands for **Stateless Address Autoconfiguration**.

With SLAAC, a device can automatically form its own IPv6 address based on information learned from router advertisements.

This is one of the most famous IPv6 features.

### DHCPv6

**DHCPv6** can also be used to provide:

- IPv6 addresses
- DNS servers
- Other configuration information

### Important point

IPv6 does not always use DHCPv6 the same way IPv4 uses DHCP.

A network may use:

- SLAAC only
- DHCPv6 only
- SLAAC plus DHCPv6
- Static addressing

It depends on the design and the operating environment.

---

## 19. ICMPv6 and Why It Matters

ICMPv6 is not just optional troubleshooting traffic.

It is a very important part of IPv6 operation.

It is used for:

- Neighbor discovery
- Router solicitation
- Router advertisement
- Path MTU discovery
- Error reporting
- Echo request and echo reply

### Very important lesson

Blocking ICMPv6 carelessly can break IPv6.

In IPv4, people sometimes think of ICMP only as “ping traffic.” In IPv6, ICMPv6 is much more central to how the network works.

---

## 20. IPv6 Routing Basics

IPv6 routing works in the same basic idea as IPv4 routing:

- Hosts send packets to local destinations directly
- Remote traffic goes to a router
- Routers examine the destination prefix
- Routers use the routing table to forward traffic

### Default route

The IPv6 default route is:

```text
::/0
```

### Longest prefix match

Just like IPv4, IPv6 routing uses **longest prefix match**.

That means the most specific route wins.

### Example

A router may have:

- `2001:db8:1::/48`
- `2001:db8:1:20::/64`

Traffic for `2001:db8:1:20::5` will match the `/64` route because it is more specific.

---

## 21. IPv4 vs IPv6

Here is a simple comparison.

| Feature | IPv4 | IPv6 |
|---|---|---|
| Address length | 32-bit | 128-bit |
| Notation | Decimal | Hexadecimal |
| Broadcast | Yes | No |
| Multicast | Supported | Core behavior |
| Address shortage | Major issue | Not a practical issue |
| ARP | Yes | No |
| NDP | No | Yes |
| NAT dependency | Very common | Often less necessary |
| Standard subnet size | Varies | Usually `/64` |

### Important idea

IPv6 is not just “IPv4 with longer addresses.”

Some ideas are similar, but many operational details are different.

---

## 22. Common IPv6 Examples

### Loopback
```text
::1
```

### Link-local example
```text
fe80::1234:5678:abcd:1
```

### ULA example
```text
fd12:3456:789a::10
```

### Global unicast example
```text
2001:db8:abcd:1::25
```

### Default route
```text
::/0
```

### Single address
```text
2001:db8::1/128
```

### Standard LAN prefix
```text
2001:db8:100:20::/64
```

---

## 23. Common Mistakes and Confusing Points

### Mistake 1: Thinking IPv6 has broadcast
It does not.

### Mistake 2: Treating link-local as a public address
Link-local addresses stay on the local link.

### Mistake 3: Forgetting that `/64` is standard
Many IPv6 features assume normal subnetting practices around `/64`.

### Mistake 4: Blocking ICMPv6 too aggressively
That can break core IPv6 functions.

### Mistake 5: Assuming DHCPv6 is always required
It is not. SLAAC may be enough, depending on the design.

### Mistake 6: Using `::` more than once
That is not valid notation.

### Mistake 7: Reading shortened addresses incorrectly
When you see `::`, you must mentally expand it to the correct number of zero groups.

---

## 24. Useful IPv6 Commands

These commands help you inspect and test IPv6.

### Windows

```powershell
ipconfig /all
ping -6 google.com
tracert -6 google.com
nslookup
route print
netsh interface ipv6 show addresses
```

### Linux

```bash
ip -6 addr
ip -6 route
ping -6 google.com
traceroute -6 google.com
ss -tulpn
```

### macOS

```bash
ifconfig
netstat -rn -f inet6
ping6 google.com
traceroute6 google.com
```

### Common things to check

- Does the interface have a link-local address?
- Does it have a global unicast address?
- Is there a default IPv6 route?
- Can it reach the local router?
- Can it resolve AAAA DNS records?
- Is ICMPv6 being blocked?

---

## 25. Quick Summary

IPv6 is the modern IP version used to solve IPv4 address exhaustion and improve address design for large-scale networks.

The most important points to remember are:

- IPv6 addresses are **128 bits**
- They are written in **hexadecimal**
- Leading zeros can be removed
- One run of zero groups can be replaced with `::`
- IPv6 uses **unicast, multicast, and anycast**
- IPv6 does **not** use broadcast
- `fe80::/10` is link-local
- `fc00::/7` is ULA
- `2000::/3` is global unicast
- `ff00::/8` is multicast
- `/64` is the standard subnet size
- IPv6 uses **NDP** instead of ARP
- IPv6 may use **SLAAC**, **DHCPv6**, or both
- **ICMPv6 is essential**

If you understand those points clearly, you already have a strong foundation in IPv6 basics.
