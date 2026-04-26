# 14. Troubleshooting Flow

Troubleshooting is not about guessing randomly. Good troubleshooting is a structured process. You start with the simplest checks, collect evidence, isolate the problem, and then fix the real cause instead of only treating the symptom.

This guide explains a practical network troubleshooting flow. It follows a layer-by-layer method, starting from the physical layer and moving up toward the application layer.

---

## Table of Contents

- [1. What Troubleshooting Means](#1-what-troubleshooting-means)
- [2. Why a Troubleshooting Flow Matters](#2-why-a-troubleshooting-flow-matters)
- [3. Start With the Symptom](#3-start-with-the-symptom)
- [4. Ask the Right Questions First](#4-ask-the-right-questions-first)
- [5. Confirm the Scope](#5-confirm-the-scope)
- [6. Check What Changed](#6-check-what-changed)
- [7. Use the Layer-by-Layer Method](#7-use-the-layer-by-layer-method)
- [8. Layer 1: Physical](#8-layer-1-physical)
- [9. Layer 2: Data Link](#9-layer-2-data-link)
- [10. Layer 3: Network](#10-layer-3-network)
- [11. Layer 4: Transport](#11-layer-4-transport)
- [12. Layer 5 to 7: Session, Presentation, and Application](#12-layer-5-to-7-session-presentation-and-application)
- [13. Basic Connectivity Test Flow](#13-basic-connectivity-test-flow)
- [14. DNS Troubleshooting Flow](#14-dns-troubleshooting-flow)
- [15. DHCP Troubleshooting Flow](#15-dhcp-troubleshooting-flow)
- [16. VLAN and Switching Troubleshooting Flow](#16-vlan-and-switching-troubleshooting-flow)
- [17. Routing Troubleshooting Flow](#17-routing-troubleshooting-flow)
- [18. Firewall and ACL Troubleshooting Flow](#18-firewall-and-acl-troubleshooting-flow)
- [19. Wireless Troubleshooting Flow](#19-wireless-troubleshooting-flow)
- [20. VPN Troubleshooting Flow](#20-vpn-troubleshooting-flow)
- [21. Performance Troubleshooting Flow](#21-performance-troubleshooting-flow)
- [22. Common Symptoms and Likely Causes](#22-common-symptoms-and-likely-causes)
- [23. Useful Commands by Platform](#23-useful-commands-by-platform)
- [24. Packet Capture in Troubleshooting](#24-packet-capture-in-troubleshooting)
- [25. How to Avoid Making the Problem Worse](#25-how-to-avoid-making-the-problem-worse)
- [26. Documentation During Troubleshooting](#26-documentation-during-troubleshooting)
- [27. Example Troubleshooting Scenarios](#27-example-troubleshooting-scenarios)
- [28. Troubleshooting Checklist](#28-troubleshooting-checklist)
- [29. Best Practices](#29-best-practices)
- [30. Quick Summary](#30-quick-summary)

---

## 1. What Troubleshooting Means

Troubleshooting means finding the cause of a problem and fixing it in a controlled way.

In networking, the problem may be simple, such as a loose cable, or complex, such as a routing loop, firewall policy issue, DNS failure, wireless interference, or broken application dependency.

A good troubleshooter does not immediately jump to conclusions.

Instead, they follow a process:

1. Understand the symptom.
2. Confirm the scope.
3. Collect evidence.
4. Test one idea at a time.
5. Identify the real cause.
6. Apply a fix.
7. Verify that the fix worked.
8. Document what happened.

The goal is not just to make the issue disappear temporarily. The goal is to understand what failed and prevent the same issue from happening again when possible.

---

## 2. Why a Troubleshooting Flow Matters

A troubleshooting flow helps you stay organized.

Without a flow, it is easy to waste time checking random things. You may restart services, change firewall rules, replace cables, or edit DNS records without knowing if those changes are related to the actual problem.

That can make the situation worse.

A structured flow helps you:

- Avoid guessing
- Find the root cause faster
- Reduce unnecessary changes
- Communicate clearly with others
- Keep track of what was tested
- Prevent repeated incidents
- Troubleshoot under pressure

### Simple idea

Do not start with the most complicated explanation.

Start with the basics, then move upward.

---

## 3. Start With the Symptom

Before touching anything, define the symptom clearly.

Bad symptom description:

```text
The network is broken.
```

Better symptom description:

```text
Users in VLAN 20 can access internal servers, but they cannot browse external websites.
```

Even better:

```text
Users in VLAN 20 can ping 8.8.8.8, but DNS lookups fail. VLAN 10 users are not affected.
```

The better the symptom description, the faster the troubleshooting process becomes.

### Questions to clarify the symptom

Ask:

- What exactly is not working?
- Who is affected?
- When did it start?
- Is it constant or intermittent?
- Did it ever work before?
- What changed recently?
- Is there an error message?
- Can the problem be reproduced?

---

## 4. Ask the Right Questions First

Good questions save time.

### User-focused questions

Ask the affected user:

- What were you trying to do?
- What did you expect to happen?
- What actually happened?
- Did you see an error message?
- When did the problem start?
- Are others around you affected?
- Does it happen on another device?
- Does it happen on wired and wireless?

### Technical questions

Ask yourself:

- Is the device connected?
- Does it have an IP address?
- Does it have the correct gateway?
- Does DNS work?
- Can it reach the destination by IP?
- Can it reach the required port?
- Is a firewall blocking it?
- Is the application actually running?

### Important point

Do not assume the user's description is technically exact.

A user may say:

```text
The Internet is down.
```

But the real issue may be:

- DNS failure
- Captive portal issue
- Wi-Fi authentication failure
- Expired password
- Proxy problem
- Browser cache problem
- Only one website is down

---

## 5. Confirm the Scope

Scope tells you how big the problem is.

This is one of the most important troubleshooting steps.

### Possible scopes

The issue may affect:

- One user
- One device
- One switch port
- One room
- One access point
- One VLAN
- One subnet
- One site
- One application
- One server
- All users
- External users only
- VPN users only

### Why scope matters

If only one device is affected, check the device, cable, port, Wi-Fi connection, IP configuration, or local firewall.

If an entire VLAN is affected, check the gateway, VLAN configuration, DHCP scope, ACL, firewall policy, or routing.

If all users are affected, check shared services such as DNS, firewall, Internet circuit, core switch, routing, or authentication systems.

### Example

Symptom:

```text
Users cannot access the file server.
```

Scope questions:

- Can any user access it?
- Is it only Wi-Fi users?
- Is it only one VLAN?
- Can users reach other servers?
- Is the file server itself online?
- Did firewall rules change?

A few scope questions can quickly reduce the possible causes.

---

## 6. Check What Changed

Many outages happen after a change.

The change may be planned or accidental.

### Common changes that cause issues

- New firewall rule
- VLAN change
- Switch port change
- DHCP scope change
- DNS record change
- Routing update
- VPN change
- Certificate renewal
- Server patch
- Firmware upgrade
- ISP maintenance
- New access point
- New cable path
- Authentication policy change
- Cloud security group change

### Important question

Ask:

```text
What changed before the problem started?
```

This does not always reveal the cause, but it often gives a strong clue.

### Be careful

Do not blame the most recent change automatically.

Use the change as a lead, then verify with evidence.

---

## 7. Use the Layer-by-Layer Method

A practical troubleshooting method is to follow the network layers from bottom to top.

This is based on the idea that higher-layer services depend on lower layers working correctly.

### Simple layer flow

```text
1. Physical
2. Data Link
3. Network
4. Transport
5. Application
```

You do not always need to check every layer deeply, but this order helps you avoid missing basic problems.

### Why start low?

If the cable is unplugged, DNS does not matter.

If the device has no IP address, the application settings are not the first concern.

If routing is broken, changing browser settings will not help.

Start with the foundation.

---

## 8. Layer 1: Physical

Layer 1 is about the physical connection and signaling.

This includes:

- Cable
- Fiber
- Radio signal
- Transceiver
- NIC
- Switch port
- Link light
- Speed
- Duplex
- Power

### What to check

Check:

- Is the device powered on?
- Is the cable connected?
- Is the link light on?
- Is the port enabled?
- Is the correct cable used?
- Is the fiber polarity correct?
- Is the SFP supported?
- Is the interface flapping?
- Are there CRC errors?
- Is the wireless signal strong enough?

### Common Layer 1 problems

| Symptom | Possible Cause |
|---|---|
| No link light | Bad cable, disabled port, bad NIC, bad switch port |
| Link flapping | Bad cable, bad SFP, speed/duplex issue, power issue |
| Many CRC errors | Bad cable, dirty fiber, faulty optics, interference |
| Low wireless signal | Too far from AP, walls, interference, bad placement |
| PoE device not powering | PoE budget, bad cable, wrong port, unsupported device |

### Useful checks

On a switch:

```text
show interface status
show interfaces counters errors
show interfaces <interface>
```

On Linux:

```bash
ip link
ethtool eth0
```

On Windows:

```powershell
Get-NetAdapter
```

### Practical rule

If Layer 1 is broken, fix it before moving up.

---

## 9. Layer 2: Data Link

Layer 2 is about local network communication.

This includes:

- MAC addresses
- Ethernet frames
- VLANs
- Switching
- Trunks
- STP
- ARP for IPv4
- NDP for IPv6

### What to check

Check:

- Is the device in the correct VLAN?
- Is the switch port an access port or trunk?
- Is the VLAN allowed on the trunk?
- Is STP blocking the port?
- Is the MAC address learned on the expected port?
- Is there MAC flapping?
- Is port security blocking the device?
- Is ARP working?
- Are there duplicate IP addresses?

### Common Layer 2 problems

| Symptom | Possible Cause |
|---|---|
| Device gets wrong IP range | Wrong VLAN |
| Device cannot reach gateway | VLAN mismatch, ARP issue, blocked port |
| Intermittent traffic | Loop, MAC flapping, bad trunk, STP changes |
| Some VLANs work across trunk, others do not | VLAN not allowed on trunk |
| Port shuts down | Port security violation, BPDU guard, err-disable |

### Useful switch checks

```text
show vlan brief
show interfaces trunk
show mac address-table
show spanning-tree
show interface status
show port-security interface <interface>
```

### Practical example

If a client should be in VLAN 20 but receives an IP from VLAN 30, the problem is likely around:

- Access port VLAN
- Wireless SSID-to-VLAN mapping
- Trunk configuration
- Switchport profile
- NAC assignment

---

## 10. Layer 3: Network

Layer 3 is about IP addressing and routing.

This includes:

- IPv4
- IPv6
- Subnet mask
- Default gateway
- Routing table
- ICMP
- ARP/NDP interaction
- Static routes
- Dynamic routing

### What to check

Check:

- Does the device have a valid IP address?
- Is the subnet mask correct?
- Is the default gateway correct?
- Can the client ping itself?
- Can it ping the gateway?
- Can it ping a remote IP?
- Does the router have a route back?
- Is there asymmetric routing?
- Is NAT required?
- Is the route more specific or less specific than expected?

### Basic IP checks

On Windows:

```powershell
ipconfig /all
route print
ping <gateway>
tracert <destination>
```

On Linux/macOS:

```bash
ip addr
ip route
ping <gateway>
traceroute <destination>
```

### Common Layer 3 problems

| Symptom | Possible Cause |
|---|---|
| Can reach local subnet only | Missing or wrong default gateway |
| Can reach gateway but not remote network | Routing issue, firewall, NAT issue |
| One-way traffic | Missing return route, asymmetric path, firewall state issue |
| Wrong source IP | NAT or routing policy issue |
| IPv6 works but IPv4 fails | IPv4 addressing/routing issue |
| IPv4 works but IPv6 fails | IPv6 route, RA, DHCPv6, firewall, or DNS issue |

### Important test order

A good basic test order is:

```text
1. Ping loopback
2. Ping own IP
3. Ping default gateway
4. Ping remote IP
5. Test DNS
6. Test application port
```

---

## 11. Layer 4: Transport

Layer 4 is about TCP and UDP communication.

At this layer, IP connectivity may work, but the service port may still fail.

### What to check

Check:

- Is the required TCP or UDP port open?
- Is the service listening?
- Is a firewall blocking the port?
- Is the connection timing out?
- Is the connection being reset?
- Is UDP traffic being dropped?
- Is the server replying?

### TCP symptoms

| Symptom | Possible Meaning |
|---|---|
| Connection refused | Host reachable, but service not listening or rejecting |
| Timeout | Firewall drop, routing issue, host down, path issue |
| Reset | Service or firewall actively closed the connection |
| TLS error | Port reachable, but certificate/protocol problem |

### Test TCP ports

Windows:

```powershell
Test-NetConnection example.com -Port 443
```

Linux/macOS:

```bash
nc -vz example.com 443
curl -I https://example.com
```

### UDP troubleshooting

UDP is harder to troubleshoot because it has no handshake like TCP.

You may need:

- Application logs
- Packet capture
- Server logs
- Firewall logs
- Protocol-specific tools

Examples of UDP services:

- DNS
- DHCP
- NTP
- SNMP
- VoIP media
- Some VPNs

---

## 12. Layer 5 to 7: Session, Presentation, and Application

Many real problems are above basic connectivity.

At this level, the network may be working, but the application still fails.

### What to check

Check:

- Is the application running?
- Is the service healthy?
- Is DNS pointing to the correct server?
- Is authentication working?
- Is the certificate valid?
- Is the application listening on the right interface?
- Is the URL correct?
- Is the proxy configured correctly?
- Are application dependencies available?
- Are logs showing errors?

### Common application-layer problems

| Symptom | Possible Cause |
|---|---|
| Website shows certificate warning | Expired certificate, wrong hostname, missing chain |
| Login fails | Authentication issue, expired password, IdP problem |
| App loads slowly | Backend issue, database latency, DNS delay, packet loss |
| API returns 500 | Application/server-side issue |
| API returns 403 | Permission, WAF, policy, authentication issue |
| Browser works but app fails | Proxy, certificate store, app-specific DNS/cache issue |

### Useful tools

```bash
curl -I https://example.com
curl -v https://example.com
openssl s_client -connect example.com:443 -servername example.com
```

---

## 13. Basic Connectivity Test Flow

This is a simple flow for general network issues.

### Step 1: Check local interface

Windows:

```powershell
ipconfig /all
```

Linux:

```bash
ip addr
```

Look for:

- IP address
- Subnet mask/prefix
- Default gateway
- DNS server
- Interface state

### Step 2: Ping loopback

IPv4:

```bash
ping 127.0.0.1
```

IPv6:

```bash
ping ::1
```

If this fails, there may be a local TCP/IP stack problem.

### Step 3: Ping own IP address

Example:

```bash
ping 192.168.1.50
```

This checks local IP configuration.

### Step 4: Ping default gateway

Example:

```bash
ping 192.168.1.1
```

If this fails, check:

- Cable or Wi-Fi
- VLAN
- Subnet mask
- Gateway status
- ARP/NDP
- Local firewall

### Step 5: Ping remote IP

Example:

```bash
ping 8.8.8.8
```

If gateway works but remote IP fails, check:

- Routing
- Firewall
- NAT
- ISP
- WAN link

### Step 6: Test DNS

```bash
nslookup example.com
```

or:

```bash
dig example.com
```

If IP works but names fail, investigate DNS.

### Step 7: Test the application port

Example:

```bash
nc -vz example.com 443
```

or:

```powershell
Test-NetConnection example.com -Port 443
```

If ping works but the port fails, check firewalls and service status.

---

## 14. DNS Troubleshooting Flow

DNS issues are very common.

A user may say the Internet is down, but the actual problem may be name resolution.

### Step 1: Test IP connectivity

Try reaching an IP address:

```bash
ping 8.8.8.8
```

If IP connectivity fails, the problem may not be DNS.

### Step 2: Check configured DNS servers

Windows:

```powershell
ipconfig /all
```

Linux:

```bash
resolvectl status
cat /etc/resolv.conf
```

macOS:

```bash
scutil --dns
```

### Step 3: Query a domain

```bash
nslookup example.com
```

or:

```bash
dig example.com
```

### Step 4: Query a specific resolver

```bash
dig @8.8.8.8 example.com
```

or:

```bash
dig @1.1.1.1 example.com
```

If public DNS works but internal DNS does not, check the internal resolver.

If internal names fail using public DNS, that may be expected.

### Step 5: Check the exact record type

```bash
dig example.com A
dig example.com AAAA
dig example.com MX
dig example.com TXT
```

### Step 6: Check authoritative DNS

```bash
dig NS example.com
dig +trace example.com
```

### Common DNS causes

- Wrong DNS server
- Bad DNS record
- Expired domain
- Broken delegation
- DNSSEC validation failure
- Split DNS issue
- Stale cache
- Firewall blocking UDP/TCP 53

---

## 15. DHCP Troubleshooting Flow

DHCP problems usually appear when a device cannot get a valid IP address.

### Step 1: Check the client address

Windows:

```powershell
ipconfig /all
```

Linux:

```bash
ip addr
```

If the client has an address like this:

```text
169.254.x.x
```

then DHCP likely failed.

### Step 2: Check VLAN and link

Make sure the device is connected to the correct network.

Check:

- Switch port VLAN
- SSID-to-VLAN mapping
- Trunk allowed VLANs
- Authentication state
- Physical link

### Step 3: Check DHCP scope

Check:

- Is the scope active?
- Are addresses available?
- Are options correct?
- Is the gateway correct?
- Are DNS servers correct?

### Step 4: Check DHCP relay

If the DHCP server is on another subnet, check the relay or IP helper.

Cisco-style example:

```text
show running-config | include helper-address
```

### Step 5: Look for rogue DHCP

If clients receive wrong addresses or gateways, there may be an unauthorized DHCP server.

Use packet capture to identify the DHCP server identifier.

### Step 6: Capture DORA

Use Wireshark filter:

```text
bootp
```

Look for:

```text
Discover -> Offer -> Request -> ACK
```

---

## 16. VLAN and Switching Troubleshooting Flow

VLAN and switching issues often appear as local connectivity problems.

### Step 1: Check port status

```text
show interface status
```

Look for:

- Connected or not connected
- Disabled port
- Err-disabled port
- Speed/duplex mismatch

### Step 2: Check VLAN assignment

```text
show vlan brief
```

Make sure the access port is in the correct VLAN.

### Step 3: Check trunks

```text
show interfaces trunk
```

Check:

- Is the port trunking?
- Is the VLAN allowed?
- Is the native VLAN correct?
- Are both sides configured consistently?

### Step 4: Check MAC learning

```text
show mac address-table
```

If the switch does not learn the client's MAC address, check Layer 1, port security, VLAN, or client NIC.

### Step 5: Check STP

```text
show spanning-tree
```

Look for blocked ports, topology changes, or root bridge issues.

### Step 6: Check port security

```text
show port-security interface <interface>
```

A security violation may shut down or restrict the port.

### Common switching causes

- Wrong access VLAN
- VLAN not allowed on trunk
- Native VLAN mismatch
- STP blocking
- Port security violation
- MAC flapping
- Layer 2 loop
- Bad cable or interface

---

## 17. Routing Troubleshooting Flow

Routing issues happen when traffic cannot reach a remote network or cannot return from it.

### Step 1: Check the client's gateway

Make sure the default gateway is correct.

### Step 2: Ping the gateway

```bash
ping <gateway-ip>
```

If this fails, troubleshoot local network first.

### Step 3: Trace the path

Windows:

```powershell
tracert <destination>
```

Linux/macOS:

```bash
traceroute <destination>
```

### Step 4: Check routing table

On Linux:

```bash
ip route
```

On Windows:

```powershell
route print
```

On a router:

```text
show ip route
show ipv6 route
```

### Step 5: Check return path

Routing must work in both directions.

A common mistake is checking only the path from client to server, while the server or another router has no route back.

### Step 6: Check NAT if needed

If traffic leaves toward the Internet, NAT may be required.

Check:

- Is NAT configured?
- Is the source subnet included?
- Is the NAT rule order correct?
- Is traffic matching the correct rule?

### Common routing causes

- Missing route
- Wrong default route
- Wrong next hop
- Overlapping subnet
- Asymmetric routing
- Route filtering
- Dynamic routing neighbor down
- NAT missing or misconfigured

---

## 18. Firewall and ACL Troubleshooting Flow

Firewall issues often look like timeouts or blocked applications.

### Step 1: Identify the flow

Before checking firewall rules, define the traffic clearly.

You need:

- Source IP
- Destination IP
- Protocol
- Destination port
- Direction
- Application name

Example:

```text
Source: 10.10.20.55
Destination: 10.10.50.20
Protocol: TCP
Port: 443
```

### Step 2: Test basic reachability

Check if the destination IP is reachable.

```bash
ping <destination>
```

Remember that ping may be blocked even when the application port is open.

### Step 3: Test the port

```bash
nc -vz 10.10.50.20 443
```

or:

```powershell
Test-NetConnection 10.10.50.20 -Port 443
```

### Step 4: Check firewall policy

Look for:

- Matching allow rule
- Matching deny rule
- Rule order
- Source zone
- Destination zone
- NAT policy
- Security profile
- User identity policy
- Time-based rule

### Step 5: Check logs

Firewall logs can show whether traffic is allowed or denied.

Search by:

- Source IP
- Destination IP
- Port
- Time

### Step 6: Check both directions

Stateful firewalls usually allow return traffic automatically, but asymmetric routing can break state tracking.

### Common firewall causes

- No matching allow rule
- Rule order issue
- Wrong object/group
- NAT rule mismatch
- Security profile blocking traffic
- Geo/IP reputation block
- Expired policy
- Asymmetric routing
- Missing return route

---

## 19. Wireless Troubleshooting Flow

Wireless problems can be caused by RF, authentication, DHCP, VLANs, roaming, or client device issues.

### Step 1: Check if the client can see the SSID

If the SSID is not visible, check:

- AP status
- SSID broadcast setting
- WLAN enabled state
- Band support
- Client distance
- Regulatory/channel settings

### Step 2: Check association

Can the client connect to the access point?

If not, check:

- Password
- WPA/WPA2/WPA3 settings
- 802.1X authentication
- MAC filtering
- Client compatibility

### Step 3: Check signal quality

Look at:

- RSSI
- SNR
- Noise
- Channel utilization
- Interference

Poor signal can cause slow or unstable connections.

### Step 4: Check IP address

If the client connects to Wi-Fi but gets no IP, troubleshoot DHCP and VLAN mapping.

### Step 5: Check VLAN mapping

Many Wi-Fi issues are actually VLAN issues.

Check:

- SSID mapped to correct VLAN
- AP trunk allows the VLAN
- Controller policy
- NAC/RADIUS VLAN assignment

### Step 6: Check roaming

If the problem happens while moving around, check roaming behavior, AP coverage, power levels, and sticky clients.

### Common wireless causes

- Weak signal
- Interference
- Wrong password
- 802.1X failure
- DHCP failure
- Wrong VLAN
- Overloaded AP
- Bad channel plan
- Client driver issue
- Roaming issue

---

## 20. VPN Troubleshooting Flow

VPN issues can involve authentication, routing, firewall rules, DNS, certificates, and encryption settings.

### Step 1: Identify VPN type

Is it:

- Remote access VPN?
- Site-to-site VPN?
- SSL VPN?
- IPsec VPN?
- WireGuard/OpenVPN-based VPN?

The troubleshooting steps depend on the type.

### Step 2: Check authentication

For remote access VPN, check:

- Username/password
- MFA
- Account status
- Group membership
- Certificate
- Client profile
- License/session limit

### Step 3: Check tunnel status

For site-to-site VPN, check:

- Phase 1 / IKE status
- Phase 2 / IPsec status
- Encryption proposals
- Pre-shared key or certificates
- Peer IP address
- NAT-T

### Step 4: Check routing

After the VPN connects, traffic still needs routes.

Check:

- Client route table
- Split tunnel routes
- Remote subnet definitions
- Return routes
- Overlapping networks

### Step 5: Check firewall rules

VPN traffic may need explicit rules from VPN zone to internal zones.

### Step 6: Check DNS

Remote users may connect to VPN but fail to access internal resources because DNS is wrong.

Check:

- DNS server assigned by VPN
- DNS suffix
- Split DNS
- Internal domain resolution

### Common VPN causes

- Bad credentials
- MFA failure
- Certificate expired
- Wrong pre-shared key
- Phase 1/2 mismatch
- NAT-T blocked
- Missing route
- Overlapping subnet
- Firewall rule missing
- DNS not assigned

---

## 21. Performance Troubleshooting Flow

Performance issues are different from complete outages.

A service may work, but slowly.

### Step 1: Define “slow”

Ask:

- Slow compared to what?
- Slow for everyone or one user?
- Slow always or only at certain times?
- Slow for one app or all apps?
- Slow on wired and wireless?
- Slow locally or over VPN/WAN?

### Step 2: Check latency

```bash
ping <destination>
```

Look for average latency and spikes.

### Step 3: Check packet loss

Packet loss can make applications feel very slow.

Use:

```bash
ping
mtr
pathping
```

### Step 4: Check bandwidth

Use tools like:

```bash
iperf3
```

But remember that bandwidth is not the same as application performance.

### Step 5: Check interface errors

On switches and routers, check for:

- CRC errors
- Drops
- Input errors
- Output errors
- Collisions in old half-duplex environments
- Queue drops

### Step 6: Check congestion

Look at:

- WAN utilization
- Firewall CPU
- Router CPU
- Interface utilization
- Wi-Fi channel utilization
- Server load

### Step 7: Check application dependencies

The network may not be the bottleneck.

A slow app can be caused by:

- Database latency
- Slow disk
- CPU load
- Memory pressure
- Backend API delay
- TLS handshake delay
- Authentication delay

### Common performance causes

- Packet loss
- High latency
- Jitter
- Congestion
- Duplex mismatch
- Bad cable
- Wi-Fi interference
- Firewall inspection bottleneck
- Slow DNS
- Server-side bottleneck

---

## 22. Common Symptoms and Likely Causes

This table gives common symptoms and likely causes.

| Symptom | Likely Causes |
|---|---|
| No link light | Cable, NIC, switch port, power, SFP |
| APIPA address | DHCP failure |
| Can ping IP but not name | DNS issue |
| Can ping gateway only | Routing, firewall, NAT, WAN issue |
| Cannot ping gateway | VLAN, Layer 1, Layer 2, local firewall, wrong IP |
| One VLAN affected | Gateway, DHCP scope, ACL, VLAN, trunk issue |
| One user affected | Device, cable, port, Wi-Fi, local config |
| All users affected | Core, firewall, DNS, DHCP, Internet circuit, routing |
| Website certificate error | TLS certificate, date/time, hostname mismatch |
| Connection timeout | Firewall drop, routing issue, host down |
| Connection refused | Service not listening or rejecting |
| Slow network | Loss, congestion, Wi-Fi, errors, server load |
| Intermittent drops | Bad cable, RF issue, flapping interface, loop, congestion |
| Wrong IP range | Wrong VLAN, rogue DHCP, SSID mapping issue |
| VPN connects but internal names fail | VPN DNS issue |
| VPN connects but no internal access | Route/firewall issue |

Use this table as a starting point, not as proof.

Always verify with tests.

---

## 23. Useful Commands by Platform

### Windows

Show IP configuration:

```powershell
ipconfig /all
```

Release and renew DHCP:

```powershell
ipconfig /release
ipconfig /renew
```

Flush DNS cache:

```powershell
ipconfig /flushdns
```

Test connectivity:

```powershell
ping 8.8.8.8
tracert example.com
pathping example.com
```

Test DNS:

```powershell
nslookup example.com
Resolve-DnsName example.com
```

Test TCP port:

```powershell
Test-NetConnection example.com -Port 443
```

Show connections:

```powershell
netstat -ano
```

Show adapters:

```powershell
Get-NetAdapter
```

### Linux

Show IP addresses:

```bash
ip addr
```

Show routes:

```bash
ip route
```

Ping:

```bash
ping 8.8.8.8
```

Trace route:

```bash
traceroute example.com
```

DNS:

```bash
dig example.com
nslookup example.com
resolvectl status
```

Show ports:

```bash
ss -tulpn
```

Test TCP port:

```bash
nc -vz example.com 443
```

HTTP test:

```bash
curl -I https://example.com
curl -v https://example.com
```

Packet capture:

```bash
sudo tcpdump -i eth0
```

### macOS

Show interfaces:

```bash
ifconfig
```

Show routes:

```bash
netstat -rn
```

DNS settings:

```bash
scutil --dns
```

Ping:

```bash
ping 8.8.8.8
```

Trace route:

```bash
traceroute example.com
```

DNS lookup:

```bash
dig example.com
nslookup example.com
```

Test port:

```bash
nc -vz example.com 443
```

### Cisco-style network devices

Interface status:

```text
show interface status
show interfaces
show interfaces counters errors
```

VLANs and trunks:

```text
show vlan brief
show interfaces trunk
```

MAC table:

```text
show mac address-table
```

ARP table:

```text
show ip arp
```

Routing:

```text
show ip route
show ipv6 route
```

Neighbors:

```text
show cdp neighbors
show lldp neighbors
```

Spanning tree:

```text
show spanning-tree
```

---

## 24. Packet Capture in Troubleshooting

Packet capture is one of the strongest troubleshooting tools.

It shows what is actually happening on the wire or interface.

### When packet capture helps

Use packet capture when:

- Logs are unclear
- You need proof of packet flow
- You suspect firewall drops
- You suspect retransmissions
- DHCP is failing
- DNS replies are strange
- TCP handshakes fail
- An application vendor asks for evidence

### Common Wireshark filters

DNS:

```text
dns
```

DHCP:

```text
bootp
```

HTTP:

```text
http
```

TCP port 443:

```text
tcp.port == 443
```

Specific host:

```text
ip.addr == 192.0.2.10
```

TCP retransmissions:

```text
tcp.analysis.retransmission
```

### tcpdump examples

Capture traffic to or from a host:

```bash
sudo tcpdump -i eth0 host 192.0.2.10
```

Capture DNS:

```bash
sudo tcpdump -i eth0 port 53
```

Capture HTTPS traffic to a host:

```bash
sudo tcpdump -i eth0 host 203.0.113.10 and port 443
```

Write capture to file:

```bash
sudo tcpdump -i eth0 -w capture.pcap
```

### Important note

Packet capture can contain sensitive data.

Handle captures carefully and avoid sharing them publicly.

---

## 25. How to Avoid Making the Problem Worse

During troubleshooting, it is easy to create new problems.

### Avoid random changes

Do not change many things at once.

Bad approach:

```text
Restart firewall, change DNS, edit VLAN, change routes, reboot server.
```

Good approach:

```text
Change one thing, test, document the result.
```

### Make reversible changes

Before changing configuration, know how to roll back.

For network devices, consider:

- Saving current config
- Taking screenshots
- Copying relevant sections
- Using maintenance windows
- Having console access if remote change may break connectivity

### Be careful with remote changes

Changing routing, VLANs, firewall rules, or management access remotely can lock you out.

Before risky changes, ask:

- Do I have out-of-band access?
- Is someone onsite?
- Can I roll back automatically?
- Do I understand the current path?

### Communicate impact

If users are affected, communicate clearly:

- What is affected
- What is not affected
- What is being tested
- When the next update will be given

---

## 26. Documentation During Troubleshooting

Documentation is part of troubleshooting.

Even quick notes can help a lot.

### What to document

Record:

- Time problem started
- Affected users/systems
- Symptoms
- Error messages
- Tests performed
- Results of each test
- Changes made
- Logs reviewed
- Root cause
- Final fix
- Follow-up actions

### Simple troubleshooting note format

```text
Issue:
Users in VLAN 20 cannot access the Internet.

Scope:
Only VLAN 20 affected. VLAN 10 and VLAN 30 work.

Tests:
- Client has valid IP: yes
- Can ping gateway: yes
- Can ping 8.8.8.8: no
- DNS lookup: fails because no external connectivity
- Firewall logs: denies from VLAN 20 after policy change

Root cause:
Firewall rule for VLAN 20 was accidentally disabled.

Fix:
Re-enabled correct policy and verified Internet access.

Follow-up:
Add change review for firewall rule edits.
```

Good documentation makes future troubleshooting faster.

---

## 27. Example Troubleshooting Scenarios

### Scenario 1: User cannot access the Internet

Symptom:

```text
One user cannot access websites.
```

Flow:

1. Check if other users are affected.
2. Check user IP configuration.
3. Ping default gateway.
4. Ping public IP.
5. Test DNS.
6. Test HTTPS port.
7. Check browser/proxy settings.

Possible findings:

- If no IP address: DHCP issue
- If wrong VLAN: switch or Wi-Fi assignment issue
- If IP works but DNS fails: DNS issue
- If DNS works but HTTPS fails: firewall/proxy issue
- If only one browser fails: browser/application issue

### Scenario 2: Server cannot be reached on port 443

Symptom:

```text
Users cannot open https://app.example.com.
```

Flow:

1. Resolve DNS.
2. Ping server IP if allowed.
3. Test TCP 443.
4. Check firewall logs.
5. Check server listening ports.
6. Check web service status.
7. Check certificate and application logs.

Useful commands:

```bash
dig app.example.com
nc -vz app.example.com 443
curl -v https://app.example.com
```

Possible causes:

- Wrong DNS record
- Firewall blocking TCP 443
- Web service stopped
- Server listening on wrong interface
- TLS certificate problem
- Load balancer issue

### Scenario 3: Client gets 169.254 address

Symptom:

```text
Client IP is 169.254.45.20.
```

Flow:

1. Check physical or Wi-Fi connection.
2. Check VLAN assignment.
3. Check DHCP server scope.
4. Check DHCP relay.
5. Capture DHCP traffic.
6. Look for Discover, Offer, Request, ACK.

Likely causes:

- DHCP server unreachable
- DHCP scope exhausted
- Wrong VLAN
- Missing helper address
- Firewall blocking DHCP

### Scenario 4: Only one VLAN cannot reach servers

Symptom:

```text
VLAN 30 cannot access server subnet, but VLAN 20 can.
```

Flow:

1. Check gateway for VLAN 30.
2. Check route from VLAN 30 to server subnet.
3. Check firewall rules from VLAN 30.
4. Check return path from server subnet.
5. Compare with working VLAN 20.

Likely causes:

- Missing firewall policy
- Missing route
- Wrong ACL
- NAT issue
- VLAN interface down

### Scenario 5: Wireless is slow in one area

Symptom:

```text
Wi-Fi is slow in conference room.
```

Flow:

1. Check if wired network is normal.
2. Check signal strength and SNR.
3. Check AP load.
4. Check channel utilization.
5. Check interference.
6. Check roaming behavior.
7. Check client capabilities.

Likely causes:

- Weak signal
- Co-channel interference
- Too many clients on one AP
- Bad AP placement
- Wide channel in crowded environment
- Client driver issue

---

## 28. Troubleshooting Checklist

Use this checklist as a quick reference.

### First checks

- [ ] What exactly is failing?
- [ ] Who is affected?
- [ ] When did it start?
- [ ] Was there a recent change?
- [ ] Is it reproducible?
- [ ] Is the issue local, subnet-wide, site-wide, or global?

### Layer 1

- [ ] Power is on
- [ ] Cable connected
- [ ] Link light active
- [ ] Interface up
- [ ] No excessive errors
- [ ] Correct speed/duplex
- [ ] Wireless signal acceptable

### Layer 2

- [ ] Correct VLAN
- [ ] Trunk allows VLAN
- [ ] MAC address learned
- [ ] No STP blocking issue
- [ ] No port security violation
- [ ] No MAC flapping

### Layer 3

- [ ] Valid IP address
- [ ] Correct subnet mask/prefix
- [ ] Correct gateway
- [ ] Can ping gateway
- [ ] Correct route exists
- [ ] Return route exists
- [ ] NAT works if needed

### Layer 4

- [ ] Correct port
- [ ] Service listening
- [ ] Firewall allows traffic
- [ ] No timeout/reset issue
- [ ] UDP behavior verified if applicable

### Application

- [ ] DNS resolves correctly
- [ ] URL/hostname correct
- [ ] Certificate valid
- [ ] Authentication works
- [ ] Application service running
- [ ] Logs checked
- [ ] Dependencies healthy

---

## 29. Best Practices

### Start simple

Check the basic things first.

Many real outages are caused by simple issues:

- Unplugged cable
- Wrong VLAN
- Bad DNS server
- Expired certificate
- Firewall rule change
- DHCP scope exhaustion

### Change one thing at a time

If you change five things and the issue is fixed, you may not know which change actually fixed it.

### Use evidence

Good evidence includes:

- Command output
- Logs
- Packet captures
- Interface counters
- Monitoring graphs
- Error messages

### Compare working and broken systems

If one user works and another does not, compare:

- IP settings
- DNS settings
- VLAN
- Routes
- Firewall path
- Application version
- Authentication status

### Know when to escalate

Escalate when:

- The issue is outside your access level
- There is business-critical impact
- You need vendor support
- You need another team's system
- You do not have enough data to proceed safely

### Verify after fixing

After applying a fix, test again.

Do not assume the fix worked.

Verify:

- The original symptom is gone
- Related services still work
- No new issue was created
- Monitoring is clean

### Write the root cause clearly

A useful root cause is specific.

Weak root cause:

```text
Network issue.
```

Better root cause:

```text
Firewall policy for VLAN 20 to Internet was disabled during a rule cleanup, causing outbound traffic from VLAN 20 to be denied.
```

---

## 30. Quick Summary

Good troubleshooting is structured, not random.

The most important points to remember are:

- Start by defining the symptom clearly
- Confirm the scope of the issue
- Check what changed recently
- Use a layer-by-layer method
- Start with physical connectivity
- Verify VLAN and Layer 2 behavior
- Check IP addressing, gateway, and routes
- Test the required transport port
- Check DNS and application behavior
- Use logs and packet captures when needed
- Change one thing at a time
- Document what you tested and changed
- Always verify the fix

A strong troubleshooting flow saves time, reduces mistakes, and helps you find the real root cause instead of guessing.
