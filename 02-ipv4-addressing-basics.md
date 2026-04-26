# 2. IPv4 Addressing Basics

## Introduction

IPv4 stands for **Internet Protocol version 4**. It is the addressing system that has been used on most networks and on the internet for many years. Even though IPv6 exists and is growing, IPv4 is still very common in homes, offices, data centers, and cloud environments.

When a device sends traffic on an IPv4 network, it uses an IPv4 address to identify itself and to find the destination. Routers use these addresses to move packets from one network to another.

This section explains the most important IPv4 concepts in detail: how the address is built, how subnetting works, what private and public addresses mean, how to find the network and broadcast address, and how to understand host ranges.

---

## 1. What an IPv4 Address Is

An IPv4 address is a **32-bit number**.

Because 32-bit binary numbers are hard for humans to read, IPv4 addresses are usually written in **dotted decimal notation**.

Example:

```text
192.168.1.10
```

This address has four parts called **octets**:

- `192`
- `168`
- `1`
- `10`

Each octet represents **8 bits**, so:

- 4 octets × 8 bits = **32 bits total**

Each octet can have a decimal value from:

- `0` to `255`

That is because 8 bits can represent 256 values.

### Binary Example

The IPv4 address below:

```text
192.168.1.10
```

In binary looks like this:

```text
11000000.10101000.00000001.00001010
```

You do not need to convert every address by hand in real work, but understanding that IPv4 is really binary is very important for subnetting.

---

## 2. Why IPv4 Addresses Matter

IPv4 addresses are used to:

- identify devices on a network
- separate one network from another
- allow routing between networks
- define source and destination in packets
- support communication between clients, servers, routers, switches with Layer 3 functions, and many services

Without logical addressing, a network could not scale beyond simple local communication.

---

## 3. The Two Main Parts of an IPv4 Address

Every IPv4 address has two logical parts:

- the **network portion**
- the **host portion**

Which part is network and which part is host depends on the **subnet mask** or **prefix length**.

### Example

```text
192.168.1.10/24
```

In this example:

- `/24` means the first **24 bits** are the network part
- the last **8 bits** are the host part

So:

- Network = `192.168.1`
- Host = `.10`

This is why the prefix is so important. The same IP address with a different prefix can belong to a different subnet.

---

## 4. Prefix Length and CIDR Notation

Modern IPv4 networks usually use **CIDR notation**.

CIDR means **Classless Inter-Domain Routing**.

Instead of only thinking in old classes such as Class A, B, and C, we define the subnet size using a prefix.

### Example Prefixes

- `/8`
- `/16`
- `/24`
- `/27`
- `/30`
- `/32`

The number after the slash tells you how many bits belong to the network portion.

### Examples

- `/24` = 24 network bits, 8 host bits
- `/16` = 16 network bits, 16 host bits
- `/30` = 30 network bits, 2 host bits

### Why CIDR Matters

CIDR helps with:

- flexible subnet sizes
- better address planning
- route summarization
- efficient address usage

---

## 5. Subnet Masks

A **subnet mask** shows which part of the address is network and which part is host.

Common examples:

| Prefix | Subnet Mask |
|---|---|
| `/8` | `255.0.0.0` |
| `/16` | `255.255.0.0` |
| `/24` | `255.255.255.0` |
| `/25` | `255.255.255.128` |
| `/26` | `255.255.255.192` |
| `/27` | `255.255.255.224` |
| `/28` | `255.255.255.240` |
| `/29` | `255.255.255.248` |
| `/30` | `255.255.255.252` |
| `/31` | `255.255.255.254` |
| `/32` | `255.255.255.255` |

### Binary View of a Mask

For `/24`:

```text
11111111.11111111.11111111.00000000
```

In decimal:

```text
255.255.255.0
```

- `1` bits = network bits
- `0` bits = host bits

---

## 6. Address Types in IPv4

IPv4 supports different addressing types depending on how traffic is sent.

### Unicast

**Unicast** means one sender to one destination.

This is the most common type of communication.

Examples:

- a laptop connecting to a web server
- one PC pinging another PC
- an SSH client connecting to one router

### Broadcast

**Broadcast** means one sender to all devices in the same subnet.

Example uses:

- ARP requests
- some DHCP messages before the client knows its address

The limited broadcast address is:

```text
255.255.255.255
```

Each subnet also has its own directed broadcast address, which is the last address in that subnet.

### Multicast

**Multicast** means one sender to a group of receivers.

The multicast range is:

```text
224.0.0.0/4
```

Multicast is used when traffic should go to a selected group, not to every host.

### Anycast

**Anycast** means one sender reaches the nearest or best destination among multiple devices sharing the same address.

It is more commonly discussed with IPv6, but it also exists operationally in IPv4-based services.

---

## 7. Public and Private IPv4 Addresses

One of the first things to learn in IPv4 is the difference between **public** and **private** addressing.

### Public IPv4 Addresses

Public addresses are globally routable on the internet.

If a server is directly reachable from the internet, it usually has a public IP address or is reached through NAT using a public IP.

Because IPv4 address space is limited, public IPv4 addresses are valuable.

### Private IPv4 Addresses

Private addresses are used inside local networks and are **not directly routable on the public internet**.

The private IPv4 ranges are:

| Range | Size |
|---|---|
| `10.0.0.0/8` | very large private range |
| `172.16.0.0/12` | medium private range |
| `192.168.0.0/16` | common in home and small office networks |

Examples of private addresses:

- `10.1.20.5`
- `172.16.50.10`
- `192.168.1.10`

### Why Private Addresses Are Used

Private addressing helps because:

- it reduces public IPv4 usage
- it allows internal addressing plans
- it works well with NAT
- it keeps internal address design separate from public internet exposure

---

## 8. Special IPv4 Ranges

Some IPv4 addresses have special uses.

| Range / Address | Purpose |
|---|---|
| `0.0.0.0` | unspecified address or default route context |
| `127.0.0.0/8` | loopback |
| `169.254.0.0/16` | link-local / APIPA |
| `224.0.0.0/4` | multicast |
| `240.0.0.0/4` | reserved |
| `255.255.255.255` | limited broadcast |
| `100.64.0.0/10` | carrier-grade NAT space |

### Loopback

`127.0.0.1` is the most famous loopback address.

It refers to the local machine itself.

Common use:

- testing the TCP/IP stack
- checking whether local services are running

### APIPA / Link-Local

If a device cannot reach a DHCP server, some operating systems assign an address from:

```text
169.254.0.0/16
```

This usually means:

- DHCP failed
- the host gave itself a temporary local address

### 0.0.0.0

`0.0.0.0` can mean different things depending on context.

Examples:

- a host that does not yet know its own address
- the default route `0.0.0.0/0`
- a service listening on all interfaces

---

## 9. Network Address, Broadcast Address, and Host Range

When working with any subnet, you should be able to identify:

- the **network address**
- the **broadcast address**
- the **usable host range**

### Network Address

The network address is the **first address in the subnet**.

It identifies the subnet itself, not a host.

### Broadcast Address

The broadcast address is the **last address in the subnet**.

It sends traffic to all devices in that subnet.

### Usable Host Range

These are the addresses between the network address and the broadcast address.

In a normal subnet:

- first usable = network + 1
- last usable = broadcast - 1

### Example: `192.168.1.0/24`

- Network address: `192.168.1.0`
- First usable: `192.168.1.1`
- Last usable: `192.168.1.254`
- Broadcast address: `192.168.1.255`

---

## 10. How to Calculate the Number of Addresses

The total number of addresses in a subnet depends on how many host bits remain.

### Formula

```text
Total addresses = 2^(32 - prefix)
```

### Usable Host Formula

For most normal subnets:

```text
Usable hosts = 2^(32 - prefix) - 2
```

Why subtract 2?

Because usually:

- one address is reserved for the network address
- one address is reserved for the broadcast address

### Example: `/24`

```text
32 - 24 = 8 host bits
2^8 = 256 total addresses
256 - 2 = 254 usable hosts
```

### Example: `/26`

```text
32 - 26 = 6 host bits
2^6 = 64 total addresses
64 - 2 = 62 usable hosts
```

---

## 11. Important Exceptions: /31 and /32

### /31

A `/31` has only 2 addresses.

Traditionally, this would leave no usable hosts after subtracting network and broadcast. But in modern practice, `/31` is commonly used on **point-to-point links**.

Why it works:

- there are only two devices on the link
- no need for a broadcast address in the traditional sense

Example:

- `10.0.0.0/31`
- usable endpoints: `10.0.0.0` and `10.0.0.1`

### /32

A `/32` represents a **single host route**.

Common uses:

- loopback interfaces on routers
- host-specific routes
- security policies
- route advertisements for one exact address

---

## 12. Common Prefix Sizes and What They Mean

| Prefix | Subnet Mask | Total Addresses | Usable Hosts | Common Use |
|---|---|---:|---:|---|
| `/8` | `255.0.0.0` | 16777216 | 16777214 | very large networks |
| `/16` | `255.255.0.0` | 65536 | 65534 | large internal subnets |
| `/24` | `255.255.255.0` | 256 | 254 | common LAN subnet |
| `/25` | `255.255.255.128` | 128 | 126 | split a /24 into 2 parts |
| `/26` | `255.255.255.192` | 64 | 62 | smaller segments |
| `/27` | `255.255.255.224` | 32 | 30 | small VLANs |
| `/28` | `255.255.255.240` | 16 | 14 | very small segments |
| `/29` | `255.255.255.248` | 8 | 6 | tiny networks or device groups |
| `/30` | `255.255.255.252` | 4 | 2 | point-to-point links |
| `/31` | `255.255.255.254` | 2 | special case | point-to-point modern use |
| `/32` | `255.255.255.255` | 1 | 1 | single host route |

---

## 13. Subnetting Step by Step

Subnetting is the process of dividing a larger network into smaller networks.

This is useful for:

- reducing broadcast domains
- improving management
- improving security
- matching subnet size to actual need
- organizing departments, VLANs, sites, or services

### Example: Splitting a /24 into Four /26 Networks

Start with:

```text
192.168.1.0/24
```

A `/24` has 256 addresses.

If you split it into `/26` subnets, each subnet has 64 addresses.

The four `/26` networks are:

1. `192.168.1.0/26`
2. `192.168.1.64/26`
3. `192.168.1.128/26`
4. `192.168.1.192/26`

### First Subnet: `192.168.1.0/26`

- Network: `192.168.1.0`
- Usable: `192.168.1.1 - 192.168.1.62`
- Broadcast: `192.168.1.63`

### Second Subnet: `192.168.1.64/26`

- Network: `192.168.1.64`
- Usable: `192.168.1.65 - 192.168.1.126`
- Broadcast: `192.168.1.127`

### Third Subnet: `192.168.1.128/26`

- Network: `192.168.1.128`
- Usable: `192.168.1.129 - 192.168.1.190`
- Broadcast: `192.168.1.191`

### Fourth Subnet: `192.168.1.192/26`

- Network: `192.168.1.192`
- Usable: `192.168.1.193 - 192.168.1.254`
- Broadcast: `192.168.1.255`

This is a core subnetting skill.

---

## 14. A Practical Way to Find Subnet Boundaries

For common subnetting tasks, people often use the **block size method**.

### Formula

```text
Block size = 256 - interesting octet in subnet mask
```

### Example: `/26`

Subnet mask:

```text
255.255.255.192
```

Interesting octet = `192`

```text
256 - 192 = 64
```

So subnet boundaries increase by 64:

- 0
- 64
- 128
- 192

That is why the `/26` networks in a /24 are:

- `.0`
- `.64`
- `.128`
- `.192`

### Example: `/27`

Mask:

```text
255.255.255.224
```

```text
256 - 224 = 32
```

Subnets start every 32:

- `.0`
- `.32`
- `.64`
- `.96`
- `.128`
- `.160`
- `.192`
- `.224`

---

## 15. Wildcard Masks

A **wildcard mask** is often used in network configuration, especially with ACLs and some routing protocols.

It is the inverse of the subnet mask.

### Example

Subnet mask:

```text
255.255.255.0
```

Wildcard mask:

```text
0.0.0.255
```

Another example:

- Subnet mask: `255.255.255.192`
- Wildcard: `0.0.0.63`

### Why Wildcard Masks Matter

They are common in:

- ACL entries
- routing protocol matching
- some older network configuration styles

---

## 16. Classful Addressing vs Modern CIDR

Historically, IPv4 used address classes.

### Old Classes

| Class | First Octet Range | Default Mask |
|---|---|---|
| A | `1-126` | `255.0.0.0` |
| B | `128-191` | `255.255.0.0` |
| C | `192-223` | `255.255.255.0` |
| D | `224-239` | multicast |
| E | `240-255` | experimental / reserved |

Today, classful design is mostly historical knowledge.

Modern networks use:

- CIDR
- VLSM
- flexible prefix lengths

Still, classful ideas appear in older study materials and interviews, so it is worth recognizing them.

---

## 17. VLSM: Variable Length Subnet Masking

**VLSM** means using different subnet sizes in the same overall address plan.

This is much more efficient than forcing every subnet to have the same size.

### Example

You may need:

- one subnet for 200 hosts
- one subnet for 50 hosts
- one subnet for 10 hosts
- one point-to-point link

With VLSM, you can assign:

- `/24` or `/25` for larger needs
- `/26` for medium needs
- `/28` for small needs
- `/30` or `/31` for point-to-point links

### Why VLSM Is Good

- saves address space
- gives better design flexibility
- avoids wasting large numbers of IPs

---

## 18. Default Gateway in IPv4

A host can directly reach devices in its own subnet.

If it needs to reach another subnet, it sends traffic to the **default gateway**.

The default gateway is usually the router interface or Layer 3 switch interface in the same subnet.

### Example

Host:

```text
192.168.10.25/24
```

Gateway:

```text
192.168.10.1
```

If the host wants to reach `192.168.20.50`, it sees that the destination is outside its local subnet and forwards the packet to the gateway.

---

## 19. How Hosts Decide Whether a Destination Is Local or Remote

This is one of the most important practical ideas in IP networking.

A host uses:

- its own IP address
- its subnet mask
- the destination IP address

It applies the subnet mask to decide whether the destination is:

- in the same local subnet
- on a remote network

### If Local

The host tries to find the destination MAC address using ARP and sends the frame directly.

### If Remote

The host sends the packet to the default gateway.

This logic happens constantly on every IP network.

---

## 20. Simple Subnetting Examples

### Example 1: `10.10.10.55/24`

Mask = `255.255.255.0`

- Network: `10.10.10.0`
- Broadcast: `10.10.10.255`
- Usable range: `10.10.10.1 - 10.10.10.254`

### Example 2: `172.16.5.130/25`

Mask = `255.255.255.128`

A `/25` splits the /24 into two ranges:

- `172.16.5.0 - 172.16.5.127`
- `172.16.5.128 - 172.16.5.255`

So this address is in the second subnet.

- Network: `172.16.5.128`
- Broadcast: `172.16.5.255`
- Usable range: `172.16.5.129 - 172.16.5.254`

### Example 3: `192.168.50.14/28`

Mask = `255.255.255.240`

Block size:

```text
256 - 240 = 16
```

Subnet ranges:

- `.0 - .15`
- `.16 - .31`
- `.32 - .47`
- and so on

Address `.14` is in the first subnet.

- Network: `192.168.50.0`
- Broadcast: `192.168.50.15`
- Usable range: `192.168.50.1 - 192.168.50.14`

---

## 21. Common Mistakes in IPv4 Addressing

### 1. Wrong Subnet Mask

A wrong mask can make a host think a remote device is local, or the opposite.

### 2. Duplicate IP Addresses

Two devices using the same IP address can cause unstable communication and hard-to-find problems.

### 3. Incorrect Default Gateway

The device may communicate locally but fail to reach outside networks.

### 4. Using the Network Address on a Host

The first address in a subnet is reserved for the subnet itself.

### 5. Using the Broadcast Address on a Host

The last address in a subnet is reserved for broadcast in normal subnetting.

### 6. Poor Address Planning

Subnets that are too small or badly organized make growth harder.

### 7. Confusing Public and Private Space

Private addresses are not directly internet-routable.

---

## 22. IPv4 Exhaustion and Why NAT Became So Common

IPv4 has a limited address space.

A 32-bit system can represent about 4.29 billion addresses in total, but many ranges are reserved or used for special purposes. Because the internet grew much larger than early planners expected, IPv4 address exhaustion became a major issue.

This is one reason why networks heavily use:

- private addressing
- PAT / NAT overload
- carrier-grade NAT in some provider environments
- IPv6 adoption

Even today, many internal enterprise networks still rely heavily on private IPv4 plus NAT.

---

## 23. Useful Mental Models for Learning IPv4

Here are a few simple ways to think about IPv4.

### Think in Network and Host Bits

Do not only memorize dotted decimal masks. Always remember that the real logic is binary.

### Prefix Length Tells the Size

A bigger prefix means:

- more network bits
- fewer host bits
- smaller subnet

A smaller prefix means:

- fewer network bits
- more host bits
- larger subnet

### Every Subnet Has Boundaries

You should always be able to answer:

- where does this subnet start?
- where does it end?
- what is the broadcast address?
- what is the usable range?

### The Gateway Connects You to Other Networks

Local subnet traffic stays local.
Remote traffic goes to the gateway.

---

## 24. Quick Review Table

| Topic | Key Idea |
|---|---|
| IPv4 size | 32 bits |
| Normal format | dotted decimal |
| Private ranges | `10/8`, `172.16/12`, `192.168/16` |
| Loopback | `127.0.0.0/8` |
| APIPA | `169.254.0.0/16` |
| Broadcast | last address in a subnet |
| Network address | first address in a subnet |
| CIDR | slash notation such as `/24` |
| Default route | `0.0.0.0/0` |
| Multicast | `224.0.0.0/4` |
| Point-to-point | often `/30` or `/31` |
| Single host route | `/32` |

---

## 25. Final Notes

IPv4 addressing is one of the foundations of networking. If you understand IPv4 well, many other topics become easier:

- routing
- NAT
- ACLs
- firewall rules
- VLAN design
- troubleshooting
- DHCP scopes
- DNS behavior in real networks

For most learners, the most important practical skills are:

1. recognize common private and special ranges
2. understand subnet masks and prefix lengths
3. calculate network, broadcast, and host ranges
4. know when traffic stays local and when it goes to the default gateway
5. become comfortable with subnetting small networks by hand

Once these skills are solid, the rest of IP networking becomes much easier to learn.
