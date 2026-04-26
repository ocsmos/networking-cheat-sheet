# 11. Wireless Networking

Wireless networking lets devices connect to a network without physical Ethernet cables. It is one of the most common parts of modern networking because laptops, phones, tablets, IoT devices, cameras, printers, and many office devices often depend on Wi-Fi.

This guide explains wireless networking in a practical and detailed way. The goal is to understand how Wi-Fi works, what the common standards mean, how wireless security works, and how to troubleshoot real wireless problems.

---

## Table of Contents

- [1. What Wireless Networking Is](#1-what-wireless-networking-is)
- [2. Why Wireless Networking Matters](#2-why-wireless-networking-matters)
- [3. Wi-Fi vs Wireless](#3-wi-fi-vs-wireless)
- [4. Basic Wireless Components](#4-basic-wireless-components)
- [5. Access Points](#5-access-points)
- [6. Wireless Clients](#6-wireless-clients)
- [7. SSID](#7-ssid)
- [8. BSSID](#8-bssid)
- [9. ESSID](#9-essid)
- [10. Wi-Fi Standards](#10-wi-fi-standards)
- [11. 2.4 GHz Band](#11-24-ghz-band)
- [12. 5 GHz Band](#12-5-ghz-band)
- [13. 6 GHz Band](#13-6-ghz-band)
- [14. Channels](#14-channels)
- [15. Channel Width](#15-channel-width)
- [16. Signal Strength and RSSI](#16-signal-strength-and-rssi)
- [17. SNR](#17-snr)
- [18. Noise and Interference](#18-noise-and-interference)
- [19. MIMO and Spatial Streams](#19-mimo-and-spatial-streams)
- [20. MU-MIMO and OFDMA](#20-mu-mimo-and-ofdma)
- [21. Roaming](#21-roaming)
- [22. Wireless Security Basics](#22-wireless-security-basics)
- [23. WEP](#23-wep)
- [24. WPA](#24-wpa)
- [25. WPA2](#25-wpa2)
- [26. WPA3](#26-wpa3)
- [27. PSK vs Enterprise Authentication](#27-psk-vs-enterprise-authentication)
- [28. 802.1X and RADIUS](#28-8021x-and-radius)
- [29. Guest Wi-Fi](#29-guest-wi-fi)
- [30. Captive Portals](#30-captive-portals)
- [31. Wireless Network Design](#31-wireless-network-design)
- [32. AP Placement](#32-ap-placement)
- [33. Coverage vs Capacity](#33-coverage-vs-capacity)
- [34. Common Wireless Problems](#34-common-wireless-problems)
- [35. Wireless Troubleshooting Flow](#35-wireless-troubleshooting-flow)
- [36. Useful Wireless Commands](#36-useful-wireless-commands)
- [37. Wireless Best Practices](#37-wireless-best-practices)
- [38. Quick Summary](#38-quick-summary)

---

## 1. What Wireless Networking Is

Wireless networking allows devices to communicate using radio signals instead of physical cables.

In most modern environments, wireless networking usually means Wi-Fi. A wireless client sends and receives radio signals to an access point. The access point connects that wireless traffic to the wired network.

### Simple example

```text
Laptop -> Wi-Fi -> Access Point -> Switch -> Router -> Internet
```

The user does not see all those steps. They simply connect to a Wi-Fi name and start using the network.

---

## 2. Why Wireless Networking Matters

Wireless networking is important because users expect mobility.

People move around with devices such as:

- Laptops
- Phones
- Tablets
- Barcode scanners
- Printers
- IP cameras
- Smart TVs
- IoT devices
- VoIP handsets
- Medical or industrial devices

Wireless makes networking more flexible, but it also introduces challenges that wired networks usually do not have.

### Wireless challenges

Wireless networks must deal with:

- Signal strength
- Interference
- Distance
- Walls and obstacles
- Shared airtime
- Client roaming
- Security
- Channel planning
- Device compatibility
- Band selection

A cable is predictable. Radio is not always predictable. That is why wireless troubleshooting often needs both networking knowledge and radio-frequency awareness.

---

## 3. Wi-Fi vs Wireless

The word **wireless** is broad.

It can include many technologies:

- Wi-Fi
- Bluetooth
- Cellular
- Zigbee
- NFC
- Satellite
- Microwave links

**Wi-Fi** is a specific family of wireless networking technologies based on IEEE 802.11 standards.

In normal conversation, people often say “wireless network” when they mean Wi-Fi. In this guide, wireless mostly means Wi-Fi.

---

## 4. Basic Wireless Components

A typical Wi-Fi network includes several parts.

### Access Point

The access point provides wireless connectivity. It bridges wireless clients into the wired network.

### Wireless Client

The client is the device connecting to Wi-Fi.

Examples:

- Phone
- Laptop
- Tablet
- Printer
- Camera

### SSID

The SSID is the Wi-Fi network name users see.

Example:

```text
Company-WiFi
Guest-WiFi
```

### Wireless Controller

Some environments use a wireless LAN controller or cloud dashboard to manage many access points.

The controller can handle:

- AP configuration
- SSID settings
- Roaming behavior
- Security policies
- Channel and power management
- Monitoring

### Authentication Server

Enterprise Wi-Fi often uses a RADIUS server for user authentication.

Example:

```text
Client -> AP -> RADIUS Server -> Authentication decision
```

---

## 5. Access Points

An access point, or AP, is the device that provides Wi-Fi coverage.

It has radios that transmit and receive wireless signals. The AP is usually connected to the wired network using Ethernet.

### Main jobs of an AP

An AP handles:

- Broadcasting SSIDs
- Accepting wireless clients
- Encrypting and decrypting Wi-Fi traffic
- Bridging wireless traffic to wired networks
- Managing radio settings
- Supporting roaming
- Enforcing wireless security policies

### Standalone AP

A standalone AP is managed individually. This is common in homes and small offices.

### Controller-managed AP

A controller-managed AP gets its configuration from a wireless controller or cloud management system. This is common in enterprise networks.

### AP vs wireless router

A home wireless router usually combines several functions:

- Router
- Firewall
- Switch
- Access point
- DHCP server
- NAT device

An enterprise AP is usually only the wireless access part and depends on other network devices for routing, firewalling, and DHCP.

---

## 6. Wireless Clients

A wireless client is any device that connects to Wi-Fi.

Examples:

- Laptop
- Phone
- Tablet
- Printer
- Smart device
- Camera
- Scanner
- IoT device

### Client behavior matters

In wireless networking, the client makes many important decisions.

For example, the client often decides:

- Which AP to join
- When to roam
- Whether to use 2.4 GHz, 5 GHz, or 6 GHz
- Which supported data rate to use
- Whether a signal is good enough

This is important because a wireless network can be designed well, but a bad or old client can still behave poorly.

---

## 7. SSID

SSID stands for **Service Set Identifier**.

It is the Wi-Fi network name that users see.

Example:

```text
Office-WiFi
Guest-WiFi
Warehouse-WiFi
```

### One SSID, many APs

In a large building, the same SSID may be broadcast by many access points.

Example:

```text
Company-WiFi
```

A user can walk through the building and stay connected to the same SSID while the device roams between APs.

### Hidden SSID

Some networks hide the SSID, but this is not strong security.

A hidden SSID can still be discovered with wireless analysis tools. Better security comes from strong encryption and authentication, not hiding the name.

### Too many SSIDs

Broadcasting too many SSIDs can reduce wireless efficiency. Each SSID creates management overhead.

In enterprise environments, it is usually better to keep the number of SSIDs limited.

---

## 8. BSSID

BSSID stands for **Basic Service Set Identifier**.

In practice, it is usually the MAC address of the AP radio for a specific SSID.

### Example

A user sees one SSID:

```text
Company-WiFi
```

But behind the scenes, multiple APs may broadcast it using different BSSIDs:

```text
AP1 radio BSSID: aa:bb:cc:00:11:22
AP2 radio BSSID: aa:bb:cc:00:11:33
AP3 radio BSSID: aa:bb:cc:00:11:44
```

The client uses BSSID information to know which actual AP/radio it is connected to.

### Why BSSID matters

BSSID is useful when troubleshooting roaming or weak signal issues.

A user may say:

```text
I am connected to Company-WiFi.
```

But the engineer needs to know:

```text
Which AP and which radio are you connected to?
```

That is where BSSID helps.

---

## 9. ESSID

ESSID stands for **Extended Service Set Identifier**.

It refers to a wireless network made of multiple APs using the same SSID.

### Example

A campus has 50 access points, all broadcasting:

```text
Campus-WiFi
```

Together, they form an extended wireless network. Users can move from one area to another while staying on the same network name.

---

## 10. Wi-Fi Standards

Wi-Fi standards are based on the IEEE 802.11 family.

Different standards support different speeds, bands, and features.

| Standard | Common Name | Band |
|---|---|---|
| `802.11a` | Legacy Wi-Fi | 5 GHz |
| `802.11b` | Legacy Wi-Fi | 2.4 GHz |
| `802.11g` | Legacy Wi-Fi | 2.4 GHz |
| `802.11n` | Wi-Fi 4 | 2.4 / 5 GHz |
| `802.11ac` | Wi-Fi 5 | 5 GHz |
| `802.11ax` | Wi-Fi 6 / 6E | 2.4 / 5 / 6 GHz |
| `802.11be` | Wi-Fi 7 | 2.4 / 5 / 6 GHz |

### 802.11a

An older 5 GHz standard. It is mostly legacy today.

### 802.11b

An old 2.4 GHz standard. It is very slow by modern standards.

Many enterprise networks disable very old data rates related to legacy standards.

### 802.11g

An older 2.4 GHz standard. It improved speed over 802.11b but is still legacy.

### 802.11n / Wi-Fi 4

Works on 2.4 GHz and 5 GHz. It introduced major improvements like MIMO.

### 802.11ac / Wi-Fi 5

Mainly 5 GHz. Common in many modern networks.

### 802.11ax / Wi-Fi 6 and Wi-Fi 6E

Works on 2.4 GHz and 5 GHz. Wi-Fi 6E extends Wi-Fi 6 into the 6 GHz band.

Wi-Fi 6 improves efficiency, especially in busy environments.

### 802.11be / Wi-Fi 7

A newer generation that improves throughput, latency, and channel usage.

As with any new standard, real benefits depend on AP support, client support, and the design of the network.

---

## 11. 2.4 GHz Band

The 2.4 GHz band is one of the oldest and most widely supported Wi-Fi bands.

### Advantages

2.4 GHz usually has:

- Longer range
- Better wall penetration
- Support from almost all Wi-Fi devices

### Disadvantages

2.4 GHz also has:

- More interference
- Fewer usable channels
- More congestion
- Lower typical throughput

### Common sources of interference

2.4 GHz can be affected by:

- Bluetooth devices
- Microwave ovens
- Wireless cameras
- Older cordless phones
- IoT devices
- Neighboring Wi-Fi networks

### Non-overlapping channels

In many countries, the common non-overlapping 2.4 GHz channels are:

```text
1, 6, 11
```

These channels are commonly used because they do not overlap much when using 20 MHz channel width.

### Practical advice

Use 2.4 GHz for:

- Legacy devices
- Longer-range low-speed needs
- IoT devices that do not support 5 GHz or 6 GHz

Do not expect 2.4 GHz to provide the best performance in dense environments.

---

## 12. 5 GHz Band

The 5 GHz band is widely used in modern Wi-Fi networks.

### Advantages

5 GHz usually offers:

- Higher throughput
- More channels than 2.4 GHz
- Less interference in many environments
- Better performance for modern devices

### Disadvantages

5 GHz usually has:

- Shorter range than 2.4 GHz
- Weaker wall penetration
- More sensitivity to obstacles

### Practical use

For most modern laptops and phones, 5 GHz is often preferred over 2.4 GHz because it provides better performance.

### DFS channels

Some 5 GHz channels are DFS channels.

DFS stands for **Dynamic Frequency Selection**.

These channels may need to avoid radar systems. If an AP detects radar, it may move away from the DFS channel.

### Practical warning

DFS channels can provide more spectrum, but they may cause unexpected channel changes in some locations.

---

## 13. 6 GHz Band

The 6 GHz band is used by Wi-Fi 6E and Wi-Fi 7 capable devices.

### Advantages

6 GHz can offer:

- Cleaner spectrum
- More available channels
- Less legacy device interference
- Better performance for supported devices

### Disadvantages

6 GHz has:

- Shorter range
- Weaker wall penetration
- Device compatibility limits
- More dependency on newer hardware

### Practical use

6 GHz is useful in modern high-density environments where both APs and clients support it.

It is not a full replacement for 2.4 GHz and 5 GHz. It is another band that can improve capacity and performance when designed well.

---

## 14. Channels

A wireless channel is a smaller frequency range inside a Wi-Fi band.

APs use channels to send and receive traffic.

### Why channels matter

If nearby APs use overlapping or crowded channels, performance can drop.

Wireless clients share airtime, so bad channel planning can cause:

- Slower speeds
- Higher latency
- Packet loss
- Roaming issues
- Unstable connections

### 2.4 GHz channels

The most common non-overlapping channels are:

```text
1, 6, 11
```

Using channels like 2, 3, 4, 5, 7, 8, 9, or 10 can create overlap with other channels.

### 5 GHz channels

5 GHz has more channels than 2.4 GHz. This makes it easier to design networks with less co-channel interference.

### 6 GHz channels

6 GHz provides even more channel space for supported devices. This is one of the reasons Wi-Fi 6E and Wi-Fi 7 can perform well in dense environments.

---

## 15. Channel Width

Channel width controls how much frequency space a Wi-Fi channel uses.

Common channel widths include:

```text
20 MHz
40 MHz
80 MHz
160 MHz
320 MHz in newer Wi-Fi 7 environments
```

### Wider channels

Wider channels can provide higher throughput.

But they also consume more spectrum. This can increase interference and reduce the number of separate channels available.

### Narrower channels

Narrower channels provide less maximum speed, but they are often better for dense networks.

### Practical guidance

For 2.4 GHz:

```text
Use 20 MHz in most cases.
```

For 5 GHz:

```text
40 MHz or 80 MHz may be used depending on density and design.
```

For high-density enterprise networks:

```text
20 MHz or 40 MHz may be better than very wide channels.
```

### Important idea

Maximum speed on paper is not the same as real performance.

A stable, clean 40 MHz channel can be better than a noisy 80 MHz channel.

---

## 16. Signal Strength and RSSI

RSSI stands for **Received Signal Strength Indicator**.

It is a measurement of how strong the received wireless signal is. RSSI is often shown in dBm.

### Example values

| RSSI | Meaning |
|---:|---|
| `-30 dBm` | Excellent signal |
| `-50 dBm` | Very good signal |
| `-67 dBm` | Common target for voice/video quality |
| `-70 dBm` | Usable but not ideal |
| `-80 dBm` | Weak signal |
| `-90 dBm` | Very poor signal |

### Important point

RSSI values are negative numbers.

Closer to zero is stronger.

So:

```text
-50 dBm is stronger than -80 dBm
```

### Practical note

Signal strength alone does not guarantee good performance.

You also need to consider noise, interference, channel utilization, client capability, and AP load.

---

## 17. SNR

SNR stands for **Signal-to-Noise Ratio**.

It compares the useful wireless signal to background noise.

### Simple idea

```text
Good signal + low noise = better connection
Good signal + high noise = worse connection
```

### Why SNR matters

A client can have decent RSSI but still perform badly if there is too much noise or interference.

### Example

Signal:

```text
-60 dBm
```

Noise:

```text
-90 dBm
```

SNR:

```text
30 dB
```

That is generally much better than:

Signal:

```text
-60 dBm
```

Noise:

```text
-70 dBm
```

SNR:

```text
10 dB
```

### Practical use

When troubleshooting Wi-Fi quality, check both:

- RSSI
- SNR

---

## 18. Noise and Interference

Wireless networks use shared radio space.

Other devices and networks can interfere with Wi-Fi.

### Types of interference

#### Wi-Fi interference

This comes from other Wi-Fi networks or APs.

Examples:

- Neighboring offices
- Nearby apartments
- Too many APs on the same channel
- Overlapping channels

#### Non-Wi-Fi interference

This comes from devices that are not Wi-Fi but use similar frequencies.

Examples:

- Microwave ovens
- Bluetooth devices
- Wireless cameras
- Baby monitors
- Some industrial equipment

### Co-channel interference

Co-channel interference happens when multiple APs or clients use the same channel.

Devices must share airtime. This is not always bad if managed well, but too much co-channel usage reduces performance.

### Adjacent-channel interference

Adjacent-channel interference happens when overlapping channels interfere with each other.

This is often worse than co-channel interference because devices may not coordinate airtime cleanly.

### Practical lesson

More APs do not automatically mean better Wi-Fi.

Too many APs with poor channel and power design can make Wi-Fi worse.

---

## 19. MIMO and Spatial Streams

MIMO stands for **Multiple Input Multiple Output**.

It uses multiple antennas to send and receive more data.

### Simple idea

Instead of using one radio path, MIMO can use multiple paths.

This can improve:

- Throughput
- Reliability
- Performance in complex indoor environments

### Spatial streams

A spatial stream is an independent data stream sent over the wireless link.

Examples:

```text
1x1
2x2
3x3
4x4
```

A 2x2 client can usually support two spatial streams.

A 4x4 AP may support more streams, but the actual connection depends on both AP and client capability.

### Practical point

A powerful AP does not force an old client to be fast.

The client must also support the features.

---

## 20. MU-MIMO and OFDMA

Modern Wi-Fi standards include features designed to improve efficiency.

### MU-MIMO

MU-MIMO stands for **Multi-User MIMO**.

It allows an AP to communicate with multiple clients more efficiently.

This is useful when many devices are connected.

### OFDMA

OFDMA stands for **Orthogonal Frequency-Division Multiple Access**.

It allows Wi-Fi channels to be divided into smaller resource units.

This improves efficiency, especially when many clients send small amounts of data.

### Why these features matter

Modern networks often have many devices.

The problem is not only maximum speed. The problem is efficient airtime sharing.

MU-MIMO and OFDMA help improve performance in busy environments.

---

## 21. Roaming

Roaming happens when a wireless client moves from one AP to another while staying connected to the same network.

### Example

A user walks from one side of an office to another:

```text
AP1 -> AP2 -> AP3
```

The device should move between APs without the user noticing much interruption.

### Important point

The client usually decides when to roam.

The AP can encourage roaming, but the final decision is often made by the client.

### Roaming problems

Common roaming issues include:

- Client stays connected to a weak AP
- Client roams too often
- Voice calls drop while moving
- Authentication takes too long
- Coverage gaps between APs

### Sticky clients

A sticky client stays connected to an AP even when a better AP is nearby.

This can happen when the client does not roam aggressively enough.

### Fast roaming

Enterprise Wi-Fi may use features like:

- 802.11k
- 802.11v
- 802.11r

These can improve roaming behavior when supported by both infrastructure and clients.

---

## 22. Wireless Security Basics

Wireless security protects the network from unauthorized access and protects traffic over the air.

Unlike wired networks, wireless signals can leave the building. That means someone nearby may be able to see the wireless network.

### Wireless security goals

Wireless security should provide:

- Authentication
- Encryption
- Access control
- Client isolation where needed
- Protection from weak legacy protocols

### Main Wi-Fi security types

Common Wi-Fi security types include:

- Open
- WEP
- WPA
- WPA2
- WPA3
- WPA/WPA2 mixed mode
- WPA2/WPA3 mixed mode
- Enterprise authentication with 802.1X

---

## 23. WEP

WEP stands for **Wired Equivalent Privacy**.

It is an old wireless security protocol.

### Important point

WEP is broken and should not be used.

It can be cracked easily with common tools.

### Practical advice

If you find WEP in a network, treat it as a serious security problem.

Replace it with WPA2 or WPA3 where possible.

---

## 24. WPA

WPA stands for **Wi-Fi Protected Access**.

It was created as an improvement over WEP. WPA is also old now.

### Practical advice

WPA is better than WEP, but it should generally be replaced with WPA2 or WPA3.

---

## 25. WPA2

WPA2 has been the common Wi-Fi security standard for many years.

It commonly uses AES-based encryption.

### WPA2-Personal

WPA2-Personal uses a shared password.

This is common in homes and small offices.

Example:

```text
SSID: Office-WiFi
Password: shared-secret-password
```

### WPA2-Enterprise

WPA2-Enterprise uses 802.1X authentication.

Users authenticate individually, often through RADIUS. This is common in enterprise networks.

### Practical note

WPA2 is still widely used, but passwords must be strong if using PSK.

Weak Wi-Fi passwords can be guessed or attacked.

---

## 26. WPA3

WPA3 is a newer Wi-Fi security standard.

It improves security compared to WPA2.

### WPA3-Personal

WPA3-Personal uses SAE, which improves protection against offline password guessing compared to older PSK methods.

### WPA3-Enterprise

WPA3-Enterprise provides stronger security options for enterprise environments.

### Practical note

WPA3 support depends on both the AP and the client.

Some older devices may not support WPA3. Because of this, some networks use transition mode, but transition modes can reduce the security benefit.

---

## 27. PSK vs Enterprise Authentication

Wi-Fi authentication is commonly divided into Personal and Enterprise modes.

### PSK / Personal

PSK stands for **Pre-Shared Key**.

Everyone uses the same Wi-Fi password.

Common in:

- Homes
- Small offices
- Simple guest networks
- Small labs

### Advantages

- Simple to set up
- Easy for small networks
- No RADIUS server required

### Disadvantages

- Everyone shares the same password
- Hard to revoke one user without changing the password for everyone
- Password may spread
- Not ideal for large organizations

### Enterprise

Enterprise Wi-Fi uses individual authentication.

Commonly based on:

```text
802.1X + RADIUS
```

### Advantages

- Each user/device can authenticate separately
- Easier to revoke access
- Better logging
- Stronger access control
- Better for organizations

### Disadvantages

- More complex to configure
- Requires identity infrastructure
- Certificate and authentication issues can happen

---

## 28. 802.1X and RADIUS

802.1X is a network access control standard.

RADIUS is commonly used as the authentication server protocol.

### Basic flow

```text
Client -> Access Point -> RADIUS Server -> Accept or Reject
```

### Components

| Component | Role |
|---|---|
| Supplicant | The client device |
| Authenticator | The AP or switch |
| Authentication server | Usually RADIUS |

### Common EAP methods

Enterprise Wi-Fi may use EAP methods such as:

- PEAP
- EAP-TLS
- EAP-TTLS

### EAP-TLS

EAP-TLS uses certificates and is considered strong when deployed correctly.

### Common 802.1X problems

- Wrong username or password
- Expired certificate
- Untrusted certificate authority
- Wrong RADIUS shared secret
- Firewall blocking RADIUS
- Time mismatch
- Incorrect client profile

---

## 29. Guest Wi-Fi

Guest Wi-Fi is a separate wireless network for visitors or unmanaged devices.

### Goals of guest Wi-Fi

Guest Wi-Fi should usually:

- Provide Internet access
- Keep guests away from internal systems
- Use separate VLANs
- Apply firewall restrictions
- Limit bandwidth if needed
- Avoid access to corporate resources

### Example design

```text
Guest SSID -> Guest VLAN -> Firewall -> Internet
```

Guests should not be able to access:

- Internal servers
- Management interfaces
- Printers unless intentionally allowed
- Employee devices

### Client isolation

Guest networks often use client isolation.

This prevents guest devices from communicating directly with each other.

---

## 30. Captive Portals

A captive portal is a web page shown to users before they can access the network.

Common uses:

- Guest Wi-Fi login
- Terms and conditions page
- Voucher-based access
- Hotel Wi-Fi
- Cafe Wi-Fi
- Airport Wi-Fi

### How it works

A user connects to Wi-Fi, opens a browser, and is redirected to a portal page.

After accepting or authenticating, access is allowed.

### Common captive portal problems

- Portal page does not open
- HTTPS redirection issues
- DNS problems
- User device uses private DNS or DoH
- VPN blocks portal detection
- Client isolation or firewall blocks required traffic

### Practical note

Captive portals are useful for guest access, but they are not a replacement for strong wireless encryption on trusted networks.

---

## 31. Wireless Network Design

Good wireless design is more than just installing APs.

It requires planning for coverage, capacity, security, and user behavior.

### Design questions

Before deploying Wi-Fi, ask:

- How many users will connect?
- What devices will connect?
- What applications will they use?
- Is voice or video important?
- Are there walls, metal shelves, or high ceilings?
- Is this office, warehouse, school, hotel, hospital, or outdoor space?
- Is guest access required?
- Is roaming important?
- Are there security or compliance requirements?

### Site survey

A wireless site survey helps determine AP placement and expected coverage.

There are different types:

- Predictive survey
- Passive survey
- Active survey
- Post-deployment validation survey

### Why design matters

Poor wireless design can cause problems even if the equipment is expensive.

Common results of poor design:

- Dead zones
- Weak signal
- Too much interference
- Slow speeds
- Bad roaming
- Unstable voice calls
- Overloaded APs

---

## 32. AP Placement

AP placement affects signal quality and performance.

### Good AP placement considers:

- Building layout
- Walls and materials
- Ceiling height
- User density
- Device types
- Expected applications
- Power levels
- Channel plan

### Avoid poor placement

Avoid placing APs:

- Inside metal cabinets
- Above thick ceiling barriers
- Near heavy interference sources
- Too close to many other APs
- In random locations only because cabling is easy

### Wall and ceiling materials

Different materials affect wireless signals differently.

Common obstacles:

- Concrete walls
- Metal shelves
- Glass with coating
- Elevator shafts
- Fire doors
- Large appliances
- Water pipes
- Human bodies in crowded spaces

### Practical lesson

AP placement should follow wireless design needs, not only cable convenience.

---

## 33. Coverage vs Capacity

Coverage and capacity are different.

### Coverage

Coverage means the signal reaches an area.

Example question:

```text
Can users connect here?
```

### Capacity

Capacity means the network can handle the number of users and traffic load.

Example question:

```text
Can 80 users in this room have good performance at the same time?
```

### Important point

A network can have good coverage but poor capacity.

Example:

A meeting room may have strong signal from one AP, but if 100 people connect at once, performance may be poor.

### High-density areas

High-density areas include:

- Classrooms
- Lecture halls
- Conference rooms
- Auditoriums
- Stadiums
- Event spaces
- Open offices

These areas need capacity planning, not just coverage.

---

## 34. Common Wireless Problems

### Problem 1: Weak signal

Possible causes:

- Too far from AP
- Walls or obstacles
- Low AP power
- Bad AP placement
- Client antenna limitations
- Wrong band selection

### Problem 2: Slow Wi-Fi

Possible causes:

- Too many clients
- Channel congestion
- Interference
- Weak signal
- Old client device
- Low data rates enabled
- WAN or Internet bottleneck
- AP uplink issue

### Problem 3: Frequent disconnects

Possible causes:

- Poor signal
- Roaming problems
- Driver issues
- Interference
- Authentication problems
- AP rebooting
- Power or cabling issue

### Problem 4: Connected but no Internet

Possible causes:

- DHCP failure
- Wrong VLAN
- DNS issue
- Firewall issue
- Gateway issue
- Captive portal issue
- Internet uplink issue

### Problem 5: Some devices work, others do not

Possible causes:

- Client does not support WPA3
- Client does not support 5 GHz or 6 GHz
- Driver or firmware issue
- Authentication profile issue
- MAC filtering
- Band steering behavior
- Certificate issue with enterprise Wi-Fi

### Problem 6: Bad roaming

Possible causes:

- APs too far apart
- APs too close together
- Power levels too high
- Missing fast roaming support
- Sticky client behavior
- Poor channel design
- Authentication delay

### Problem 7: Guest portal does not appear

Possible causes:

- DNS issue
- Browser using HTTPS-only behavior
- Device using private DNS
- VPN enabled
- Captive portal detection blocked
- Firewall rule issue

---

## 35. Wireless Troubleshooting Flow

Wireless troubleshooting should be structured.

Do not immediately blame the AP.

### Step 1: Identify the scope

Ask:

- Is it one user or many users?
- Is it one device type or all devices?
- Is it one area or the whole site?
- Is it one SSID or all SSIDs?
- Is it one band or all bands?
- Did it start after a change?

### Step 2: Check client connection details

Check:

- SSID
- BSSID
- Band
- Channel
- RSSI
- SNR
- Data rate
- IP address
- Default gateway
- DNS servers

### Step 3: Check whether the client has a valid IP

If the client has an APIPA address like:

```text
169.254.x.x
```

then DHCP likely failed.

### Step 4: Check DNS

If the client can reach IP addresses but not names, check DNS.

Example:

```bash
ping 8.8.8.8
nslookup example.com
```

### Step 5: Check signal quality

Look at RSSI and SNR.

A weak signal or poor SNR can cause slow speeds and disconnects.

### Step 6: Check channel utilization

High channel utilization means the airtime is busy.

This can happen even if the signal is strong.

### Step 7: Check AP health

Check:

- AP online status
- AP uptime
- Radio status
- Channel
- Transmit power
- Connected client count
- AP uplink speed
- PoE status

### Step 8: Check authentication

For enterprise Wi-Fi, check:

- RADIUS logs
- User credentials
- Certificates
- EAP method
- Client profile
- Time synchronization

### Step 9: Check interference

Use controller tools, spectrum analysis, or survey tools when available.

Look for:

- High noise
- Non-Wi-Fi interference
- Overlapping channels
- Nearby APs
- DFS events

### Step 10: Compare wired and wireless

If wired works fine but wireless is slow, the issue may be wireless-specific.

If both wired and wireless are slow, the issue may be upstream:

- DNS
- Firewall
- WAN
- Server
- Internet provider
- Routing

---

## 36. Useful Wireless Commands

### Windows

Show wireless interfaces:

```powershell
netsh wlan show interfaces
```

Show wireless profiles:

```powershell
netsh wlan show profiles
```

Show available networks:

```powershell
netsh wlan show networks mode=bssid
```

Show IP configuration:

```powershell
ipconfig /all
```

Test connectivity:

```powershell
ping 8.8.8.8
Test-NetConnection example.com -Port 443
```

### Linux

Show wireless interfaces:

```bash
iw dev
```

Show link details:

```bash
iw dev wlan0 link
```

Scan nearby networks:

```bash
sudo iw dev wlan0 scan
```

Show IP information:

```bash
ip addr
ip route
```

NetworkManager status:

```bash
nmcli device wifi list
nmcli connection show
```

### macOS

Show Wi-Fi details from the menu:

```text
Hold Option and click the Wi-Fi icon
```

Useful command:

```bash
/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -I
```

Scan networks:

```bash
/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -s
```

Show IP information:

```bash
ifconfig
netstat -rn
```

### General tools

Useful wireless tools include:

- Wireshark
- tcpdump
- iperf3
- Ekahau
- NetSpot
- Acrylic WiFi
- inSSIDer
- Controller dashboards
- Spectrum analyzers

---

## 37. Wireless Best Practices

### Use strong security

Avoid:

- Open Wi-Fi for trusted networks
- WEP
- Weak WPA/WPA2 passwords
- Shared passwords in large organizations

Prefer:

- WPA2-Enterprise
- WPA3 where supported
- Strong PSKs for small networks
- 802.1X for enterprise networks

### Keep SSIDs limited

Do not create too many SSIDs.

A simple design may use:

- Corporate Wi-Fi
- Guest Wi-Fi
- IoT Wi-Fi if needed

Too many SSIDs create overhead.

### Use proper VLAN segmentation

Separate different types of wireless traffic.

Example:

| SSID | VLAN | Purpose |
|---|---:|---|
| Company-WiFi | 20 | Employees |
| Guest-WiFi | 40 | Guests |
| IoT-WiFi | 60 | IoT devices |
| Voice-WiFi | 70 | Wireless voice |

### Plan channels carefully

Avoid random channel selection in complex environments.

Use proper channel planning and controller automatic radio management when appropriate.

### Avoid too much transmit power

High transmit power is not always good.

If AP power is too high, clients may hear APs from too far away and roaming may become worse.

Also, the AP may hear clients poorly because client devices transmit at lower power.

### Prefer 5 GHz and 6 GHz for modern clients

For performance, modern clients usually do better on 5 GHz or 6 GHz.

Keep 2.4 GHz available only where needed.

### Use 20 MHz on 2.4 GHz

In most environments, 2.4 GHz should use 20 MHz channels.

Wider 2.4 GHz channels usually cause more overlap and interference.

### Validate after deployment

After installing APs, test the environment.

Check:

- Coverage
- Roaming
- Throughput
- Channel utilization
- Authentication
- Guest access
- Voice/video quality if needed

### Keep firmware updated

Wireless bugs can affect stability, roaming, security, and performance.

Keep APs, controllers, and client drivers updated carefully.

### Monitor the wireless network

Monitor:

- AP status
- Client count
- Channel utilization
- Interference
- Authentication failures
- DHCP failures
- Roaming events
- Client experience

### Design for users, not only signal

Good Wi-Fi is not just about showing full bars.

It should support the actual applications users need:

- Voice
- Video calls
- Cloud apps
- File access
- POS systems
- Scanners
- Real-time systems

---

## 38. Quick Summary

Wireless networking allows devices to connect without cables.

The most important points to remember are:

- Wi-Fi is based on IEEE 802.11 standards
- SSID is the wireless network name
- BSSID identifies the actual AP radio/interface
- 2.4 GHz has longer range but more interference
- 5 GHz usually provides better performance
- 6 GHz offers cleaner spectrum for newer devices
- Common 2.4 GHz non-overlapping channels are `1`, `6`, and `11`
- Channel width affects speed and interference
- RSSI measures signal strength
- SNR compares signal to noise
- MIMO improves wireless performance using multiple antennas
- Roaming is mostly client-driven
- WEP is insecure and should not be used
- WPA2 is common
- WPA3 is newer and stronger when supported
- Enterprise Wi-Fi often uses 802.1X and RADIUS
- Guest Wi-Fi should be isolated from internal networks
- More APs do not automatically mean better Wi-Fi
- Good wireless design must consider coverage, capacity, channels, power, security, and client behavior

If you understand SSIDs, bands, channels, signal quality, security, and roaming, you will have a strong foundation for working with wireless networks.
