# Network Models

A clear, publication-ready reference to the two most important networking models: **OSI** and **TCP/IP**. This guide explains what each layer does, how the layers map to real protocols, how data moves through the stack, and where each layer helps during troubleshooting.

---

## Table of Contents

- [Why Network Models Matter](#why-network-models-matter)
- [The OSI Model](#the-osi-model)
  - [Layer 1: Physical](#layer-1-physical)
  - [Layer 2: Data Link](#layer-2-data-link)
  - [Layer 3: Network](#layer-3-network)
  - [Layer 4: Transport](#layer-4-transport)
  - [Layer 5: Session](#layer-5-session)
  - [Layer 6: Presentation](#layer-6-presentation)
  - [Layer 7: Application](#layer-7-application)
- [The TCP/IP Model](#the-tcpip-model)
  - [Link Layer](#link-layer)
  - [Internet Layer](#internet-layer)
  - [Transport Layer](#transport-layer)
  - [Application Layer](#application-layer)
- [OSI vs TCP/IP](#osi-vs-tcpip)
- [Protocol Data Units (PDU)](#protocol-data-units-pdu)
- [How Encapsulation Works](#how-encapsulation-works)
- [How to Use the Models in Troubleshooting](#how-to-use-the-models-in-troubleshooting)
- [Common Interview and Exam Points](#common-interview-and-exam-points)
- [Quick Summary](#quick-summary)

---

## Why Network Models Matter

Network models provide a structured way to understand how communication happens between devices. Instead of treating a network connection as one large black box, they break communication into layers, where each layer has a defined role.

This helps with:

- understanding protocols and where they operate
- isolating faults faster
- designing cleaner networks
- explaining packet flow clearly in documentation, interviews, and incident reports

The two models used most often are:

- **OSI Model**: a conceptual 7-layer reference model
- **TCP/IP Model**: the practical model used by real-world IP networks

---

## The OSI Model

The **Open Systems Interconnection (OSI)** model divides communication into **seven layers**. It is mainly used as a learning, design, and troubleshooting framework.

| Layer | Name | Main Purpose | Common Examples |
|---|---|---|---|
| 7 | Application | User-facing network services | HTTP, HTTPS, DNS, DHCP, SMTP, SSH |
| 6 | Presentation | Data formatting, encryption, encoding | TLS, encryption, compression, character encoding |
| 5 | Session | Session setup, control, teardown | Session coordination, authentication flow support |
| 4 | Transport | End-to-end delivery | TCP, UDP |
| 3 | Network | Logical addressing and routing | IPv4, IPv6, ICMP, routing |
| 2 | Data Link | Local delivery on the same segment | Ethernet, MAC, VLAN, STP, switching |
| 1 | Physical | Signals and media | Copper, fiber, radio, connectors, optics |

### Layer 1: Physical

The **Physical layer** is responsible for transmitting raw bits across a medium.

It deals with:

- cables
- fiber optics
- radio waves
- connectors
- electrical or optical signaling
- link speed and physical transmission standards

Typical issues at this layer:

- bad cable
- damaged transceiver
- no link light
- incorrect speed settings
- signal loss
- interface flaps

Examples:

- Cat6 cable
- fiber patch cable
- SFP/QSFP transceivers
- wireless radio frequency transmission

### Layer 2: Data Link

The **Data Link layer** handles communication on the local network segment. It uses hardware addressing and frame-based delivery.

Key functions:

- MAC addressing
- framing
- VLAN tagging
- switching
- loop prevention
- error detection at frame level

Common technologies and concepts:

- Ethernet
- MAC addresses
- VLANs
- 802.1Q
- STP / RSTP
- LACP
- CAM table

Typical problems:

- wrong VLAN assignment
- switch loop
- STP blocking state
- MAC learning issues
- duplex mismatch

### Layer 3: Network

The **Network layer** provides logical addressing and path selection between networks.

Key functions:

- IP addressing
- routing between subnets
- packet forwarding
- path selection
- TTL / Hop Limit handling

Common protocols:

- IPv4
- IPv6
- ICMP
- ICMPv6
- OSPF
- RIP
- EIGRP
- BGP

Typical problems:

- wrong IP address
- wrong subnet mask or prefix
- missing default gateway
- bad route
- routing loop
- ACL blocking traffic

### Layer 4: Transport

The **Transport layer** provides end-to-end communication between hosts.

Its main role is to decide **how** data is delivered.

#### TCP

**Transmission Control Protocol (TCP)** is connection-oriented and reliable.

It provides:

- sequencing
- acknowledgments
- retransmission
- flow control
- session reliability

Used by protocols and services that need reliable delivery, such as:

- HTTPS
- SSH
- SMTP
- IMAP

#### UDP

**User Datagram Protocol (UDP)** is connectionless and lightweight.

It provides:

- fast delivery
- lower overhead
- no built-in retransmission
- no connection establishment

Used where low latency matters or the application can tolerate some loss, such as:

- DNS queries
- VoIP
- streaming
- NTP

Typical Layer 4 issues:

- blocked TCP/UDP port
- TCP handshake failure
- connection reset
- timeout
- packet loss affecting sessions

### Layer 5: Session

The **Session layer** manages the creation, maintenance, and termination of communication sessions.

In real-world modern networking, this layer is not always isolated as a separate protocol layer, but the concept remains useful.

It covers ideas such as:

- session establishment
- session persistence
- coordination between applications
- dialog control

In practice, session-related behavior is often handled by application logic, authentication systems, or protocol implementations.

### Layer 6: Presentation

The **Presentation layer** is responsible for how data is represented.

It handles:

- data format conversion
- encryption and decryption
- compression
- character encoding
- serialization

Examples:

- TLS encryption
- UTF-8 encoding
- compression formats
- data representation between systems

This layer ensures that data created by one system can be understood by another.

### Layer 7: Application

The **Application layer** is the closest layer to the user.

It provides network services directly to applications and users.

Examples:

- HTTP / HTTPS
- DNS
- DHCP
- SMTP
- SSH
- FTP
- LDAP

Important note: this does **not** mean the user interface itself is Layer 7. It means the network service the application uses operates here.

Typical Layer 7 issues:

- DNS record problem
- certificate error
- service misconfiguration
- authentication failure
- application bug

---

## The TCP/IP Model

The **TCP/IP model** is the practical model behind modern IP networking. It is simpler than OSI and is more closely aligned with how real networks are implemented.

It usually has **four layers**.

| Layer | Main Purpose | Common Examples |
|---|---|---|
| Application | Application services and data formatting handled together | HTTP, HTTPS, DNS, DHCP, SMTP, SSH |
| Transport | End-to-end host communication | TCP, UDP |
| Internet | Logical addressing and routing | IP, ICMP, routing |
| Link | Local network access and physical delivery | Ethernet, Wi-Fi, ARP, switching |

### Link Layer

The **Link layer** combines functions that the OSI model separates into **Physical** and **Data Link**.

It includes:

- cabling and wireless signaling
- frame delivery
- MAC addressing
- switching
- local link access

Examples:

- Ethernet
- Wi-Fi
- ARP for IPv4 neighbor resolution
- switching on a LAN

### Internet Layer

The **Internet layer** corresponds mainly to OSI Layer 3.

It handles:

- IP addressing
- routing
- inter-network forwarding
- error and control messaging via ICMP

Examples:

- IPv4
- IPv6
- ICMP
- routing protocols and route-based forwarding

### Transport Layer

The **Transport layer** is the same general concept as OSI Layer 4.

It provides host-to-host communication using:

- TCP
- UDP

It decides whether communication is reliable, connection-oriented, or lightweight and connectionless.

### Application Layer

The **Application layer** in TCP/IP is broader than OSI Layer 7.

It typically includes the responsibilities of OSI Layers **5, 6, and 7** together.

That means this layer may include:

- application protocols
- data representation
- encryption behavior
- session behavior handled by software

---

## OSI vs TCP/IP

The two models describe similar communication processes, but they do so at different levels of abstraction.

| OSI Layer | TCP/IP Equivalent |
|---|---|
| Application (7) | Application |
| Presentation (6) | Application |
| Session (5) | Application |
| Transport (4) | Transport |
| Network (3) | Internet |
| Data Link (2) | Link |
| Physical (1) | Link |

### Main Differences

| Topic | OSI | TCP/IP |
|---|---|---|
| Number of layers | 7 | 4 |
| Purpose | Conceptual reference model | Practical implementation model |
| Real-world use | Training, design, troubleshooting | Actual Internet and enterprise networking |
| Session/Presentation separation | Explicit | Usually merged into Application |
| Physical/Data Link separation | Explicit | Usually merged into Link |

### Which One Should You Use?

Use **OSI** when:

- teaching or studying networking
- explaining where a problem exists
- documenting behavior layer by layer

Use **TCP/IP** when:

- describing real protocol stacks
- discussing production network behavior
- mapping how Internet protocols actually operate

---

## Protocol Data Units (PDU)

A **Protocol Data Unit (PDU)** is the unit of data associated with a specific layer.

| Layer | PDU Name |
|---|---|
| Layer 1 | Bits |
| Layer 2 | Frame |
| Layer 3 | Packet |
| Layer 4 | Segment (TCP) / Datagram (UDP) |

This terminology is useful in packet analysis, troubleshooting, and interviews.

Example:

- an HTTP request is application data
- TCP wraps it into a **segment**
- IP wraps that into a **packet**
- Ethernet wraps that into a **frame**
- the frame becomes **bits** on the medium

---

## How Encapsulation Works

**Encapsulation** means each layer adds its own header, and sometimes a trailer, to the data it receives from the layer above.

### Sending Data

When a client sends traffic:

1. the application creates the data
2. the transport layer adds TCP or UDP information
3. the network layer adds IP addressing
4. the data link layer adds MAC addressing and frame details
5. the physical layer transmits the bits

### Receiving Data

On the receiving side, the process is reversed. This is called **de-encapsulation**.

1. bits are received at the physical layer
2. the frame is processed at the data link layer
3. the packet is processed at the network layer
4. the segment or datagram is processed at the transport layer
5. the application receives the final data

### Simple Encapsulation View

```text
Application Data
  ↓
[TCP Header][Application Data]
  ↓
[IP Header][TCP Header][Application Data]
  ↓
[Ethernet Header][IP Header][TCP Header][Application Data][Trailer]
  ↓
Bits on the wire
```

---

## How to Use the Models in Troubleshooting

One of the best uses of network models is structured troubleshooting.

A clean method is to move **from lower layers to higher layers**.

### Layer-Based Example

| Layer | Questions to Ask |
|---|---|
| Physical | Is the cable connected? Is there link light? Is the optic or radio working? |
| Data Link | Is the device in the correct VLAN? Is the switch learning the MAC address? |
| Network | Does the host have the correct IP, mask, and gateway? Is routing correct? |
| Transport | Is the required TCP or UDP port open and reachable? |
| Application | Is DNS correct? Is the service running? Is TLS or authentication failing? |

### Example Scenario

A user cannot open a website.

A structured approach would be:

1. verify physical connectivity
2. verify VLAN and local switch connectivity
3. verify IP address, subnet, and gateway
4. test remote reachability with `ping` or `traceroute`
5. verify DNS resolution
6. test TCP port `443`
7. inspect certificates, web service state, or reverse proxy configuration

This method reduces guesswork and helps isolate the failure domain quickly.

---

## Common Interview and Exam Points

### 1. OSI is mostly conceptual

The OSI model is important, but real production networks usually follow the TCP/IP model more closely.

### 2. TCP/IP is the practical model

When people describe how the Internet works, they are usually thinking in TCP/IP terms.

### 3. Layers do not always map perfectly in software

Real implementations often blur boundaries. For example, encryption may be discussed as Presentation-layer behavior in OSI, but in practice it may be tightly integrated into application or transport behavior.

### 4. Layer 2 and Layer 3 are especially important

A large portion of networking work depends on understanding the difference between:

- local forwarding using MAC addresses
- routed forwarding using IP addresses

### 5. PDU names matter

Interviewers may ask:

- What is the PDU at Layer 2?
- What is the difference between a packet and a frame?
- Is UDP data called a segment or a datagram?

These are common baseline knowledge checks.

---

## Quick Summary

- **OSI** is a 7-layer conceptual model used for learning and troubleshooting.
- **TCP/IP** is a 4-layer practical model used by real IP networks.
- **Layer 2** handles local delivery using MAC addresses and frames.
- **Layer 3** handles routing using IP addresses and packets.
- **Layer 4** handles end-to-end delivery using TCP or UDP.
- **Application-related functions** sit at the top of the stack.
- **PDU names** change by layer: bits, frame, packet, segment/datagram.
- **Encapsulation** explains how data is wrapped as it moves down the stack.
- These models are most useful when diagnosing problems layer by layer.

---
