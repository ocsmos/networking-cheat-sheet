# 10. DHCP Reference

DHCP is one of the services that makes modern networks easier to use. Without DHCP, every device would need to be configured manually with an IP address, subnet mask, default gateway, DNS servers, and other network settings.

This guide explains DHCP in a practical and detailed way. The goal is to understand what DHCP does, how the DHCP process works, what can go wrong, and how to troubleshoot DHCP problems.

---

## Table of Contents

- [1. What DHCP Is](#1-what-dhcp-is)
- [2. Why DHCP Matters](#2-why-dhcp-matters)
- [3. What DHCP Can Assign](#3-what-dhcp-can-assign)
- [4. DHCP Client and DHCP Server](#4-dhcp-client-and-dhcp-server)
- [5. DHCP Ports](#5-dhcp-ports)
- [6. The DHCP DORA Process](#6-the-dhcp-dora-process)
- [7. DHCP Discover](#7-dhcp-discover)
- [8. DHCP Offer](#8-dhcp-offer)
- [9. DHCP Request](#9-dhcp-request)
- [10. DHCP Acknowledge](#10-dhcp-acknowledge)
- [11. DHCP Lease](#11-dhcp-lease)
- [12. Lease Renewal](#12-lease-renewal)
- [13. DHCP Scope](#13-dhcp-scope)
- [14. DHCP Pool](#14-dhcp-pool)
- [15. Exclusions](#15-exclusions)
- [16. Reservations](#16-reservations)
- [17. Static IP vs DHCP Reservation](#17-static-ip-vs-dhcp-reservation)
- [18. DHCP Options](#18-dhcp-options)
- [19. Common DHCP Options](#19-common-dhcp-options)
- [20. DHCP Relay / IP Helper](#20-dhcp-relay--ip-helper)
- [21. DHCP Across VLANs](#21-dhcp-across-vlans)
- [22. DHCP and Routers](#22-dhcp-and-routers)
- [23. DHCP and DNS](#23-dhcp-and-dns)
- [24. DHCPv6](#24-dhcpv6)
- [25. SLAAC vs DHCPv6](#25-slaac-vs-dhcpv6)
- [26. APIPA / Link-Local IPv4](#26-apipa--link-local-ipv4)
- [27. Rogue DHCP Servers](#27-rogue-dhcp-servers)
- [28. DHCP Snooping](#28-dhcp-snooping)
- [29. Common DHCP Problems](#29-common-dhcp-problems)
- [30. DHCP Troubleshooting Flow](#30-dhcp-troubleshooting-flow)
- [31. Useful DHCP Commands](#31-useful-dhcp-commands)
- [32. Packet Capture and DHCP](#32-packet-capture-and-dhcp)
- [33. DHCP Best Practices](#33-dhcp-best-practices)
- [34. Quick Summary](#34-quick-summary)

---

## 1. What DHCP Is

DHCP stands for **Dynamic Host Configuration Protocol**.

It is used to automatically give network settings to devices.

When a laptop, phone, printer, server, or virtual machine joins a network, it usually needs several settings before it can communicate properly.

The most important settings are:

- IP address
- Subnet mask
- Default gateway
- DNS server

DHCP provides these settings automatically.

Without DHCP, an administrator would need to configure every device manually.

---

## 2. Why DHCP Matters

DHCP saves time and reduces mistakes.

In a small network, manually configuring IP addresses may seem possible. In a larger network, it quickly becomes difficult and error-prone.

DHCP helps with:

- Automatic IP assignment
- Easier network management
- Fewer duplicate IP conflicts
- Centralized configuration
- Faster device onboarding
- Easier changes to DNS or gateway settings
- Support for mobile devices moving between networks

### Simple example

A user connects a laptop to Wi-Fi.

The laptop does not know:

- Which IP address to use
- Which subnet it belongs to
- Which gateway to send traffic to
- Which DNS server to use

DHCP gives the laptop that information.

---

## 3. What DHCP Can Assign

DHCP can assign more than just an IP address.

Common DHCP-provided settings include:

- IP address
- Subnet mask
- Default gateway
- DNS servers
- Domain name
- Lease time
- NTP servers
- TFTP server
- PXE boot information
- WINS servers in legacy environments
- Vendor-specific options

### Most common settings

For most client networks, the most important DHCP settings are:

```text
IP address
Subnet mask
Default gateway
DNS server
```

If these are correct, the client can usually communicate on the network and access names through DNS.

---

## 4. DHCP Client and DHCP Server

DHCP uses a client-server model.

### DHCP Client

The DHCP client is the device asking for network settings.

Examples:

- Laptop
- Desktop
- Phone
- Printer
- IP camera
- Server
- Virtual machine
- IoT device

### DHCP Server

The DHCP server gives network settings to clients.

Examples:

- Windows Server DHCP role
- Linux DHCP server
- Router
- Firewall
- Wireless controller
- Cloud network service
- Home router

### Basic relationship

```text
Client: I need network settings.
Server: Here is an IP address and configuration.
```

---

## 5. DHCP Ports

DHCP uses UDP.

For IPv4 DHCP:

| Role | Port |
|---|---:|
| DHCP Server | UDP `67` |
| DHCP Client | UDP `68` |

### Why UDP?

A DHCP client may not have an IP address yet, so DHCP needs a simple way to communicate during early network setup.

The client often starts by broadcasting because it does not know where the DHCP server is.

### DHCPv6 ports

For IPv6 DHCP:

| Role | Port |
|---|---:|
| DHCPv6 Client | UDP `546` |
| DHCPv6 Server | UDP `547` |

---

## 6. The DHCP DORA Process

The classic DHCP process is called **DORA**.

DORA stands for:

```text
Discover
Offer
Request
Acknowledge
```

This is the normal process a client uses to get an IPv4 address.

### Simple flow

```text
Client  -> Discover    -> Broadcast: Is there any DHCP server?
Server  -> Offer       -> I can offer this IP address.
Client  -> Request     -> I want to use that offered IP address.
Server  -> Acknowledge -> Approved. Use this configuration.
```

### Why DORA is important

DORA is one of the most important DHCP concepts for troubleshooting.

If DHCP fails, you can often identify where the process stopped:

- Did the client send Discover?
- Did the server send Offer?
- Did the client send Request?
- Did the server send Acknowledge?

---

## 7. DHCP Discover

The DHCP Discover message is sent by the client.

The client sends it when it needs an IP address.

At this point, the client usually does not have a valid IP address yet.

### Typical source and destination

```text
Source IP:      0.0.0.0
Destination IP: 255.255.255.255
Source port:    UDP 68
Destination:    UDP 67
```

### Meaning

The client is basically saying:

```text
Is there any DHCP server on this network?
```

### Important point

The Discover message is usually broadcast, so it stays within the local broadcast domain unless a DHCP relay forwards it.

---

## 8. DHCP Offer

The DHCP Offer message is sent by a DHCP server.

It offers an IP address and configuration to the client.

### The offer may include:

- Offered IP address
- Subnet mask
- Default gateway
- DNS servers
- Lease time
- DHCP server identifier
- Other DHCP options

### Meaning

The server is saying:

```text
You can use this IP address and these settings.
```

### Multiple offers

If more than one DHCP server hears the Discover, the client may receive multiple Offers.

The client normally chooses one offer and continues with the Request step.

---

## 9. DHCP Request

The DHCP Request message is sent by the client.

It tells the network which DHCP offer the client wants to accept.

### Meaning

The client is saying:

```text
I want to use this offered IP address from this DHCP server.
```

### Why this step matters

If multiple DHCP servers responded, the Request helps make clear which server and which offer the client selected.

Other DHCP servers understand that their offers were not chosen.

---

## 10. DHCP Acknowledge

The DHCP Acknowledge message is sent by the server.

It confirms that the client can use the assigned address and settings.

### Meaning

The server is saying:

```text
Approved. You can use this IP address for this lease time.
```

After receiving the ACK, the client configures the interface and starts using the network.

### DHCP NAK

Sometimes the server sends a DHCP NAK instead of an ACK.

NAK means negative acknowledgment.

This can happen when:

- The requested address is no longer valid
- The client moved to another subnet
- The server cannot approve the requested configuration
- The lease information is wrong or expired

When a client receives a NAK, it usually starts the DHCP process again.

---

## 11. DHCP Lease

A DHCP lease is the amount of time a client is allowed to use an assigned IP address.

The client does not own the address forever.

It is borrowing the address from the DHCP server.

### Example

A DHCP server may give this lease:

```text
IP address: 192.168.1.50
Lease time: 24 hours
```

The client can use `192.168.1.50` for 24 hours unless it renews the lease.

### Why leases exist

Leases help DHCP manage address space.

If a device leaves the network and never comes back, the address can eventually return to the pool and be used by another device.

---

## 12. Lease Renewal

Clients try to renew their leases before they expire.

A common behavior is:

- At 50% of lease time, the client tries to renew with the original DHCP server
- At 87.5% of lease time, the client may try rebinding more broadly
- If the lease expires, the client must stop using the address

### Example

If the lease time is 8 hours:

- Around 4 hours, the client tries to renew
- Around 7 hours, the client becomes more aggressive
- At 8 hours, the lease expires if not renewed

### Why renewal matters

Renewal lets clients keep the same IP address over time without repeating the full broadcast process every few minutes.

---

## 13. DHCP Scope

A DHCP scope is a range of IP addresses that a DHCP server can assign.

### Example scope

```text
Network:       192.168.10.0/24
DHCP range:    192.168.10.100 - 192.168.10.200
Subnet mask:   255.255.255.0
Gateway:       192.168.10.1
DNS servers:   192.168.10.10, 192.168.10.11
Lease time:    8 hours
```

This means DHCP can assign addresses from `.100` to `.200` to clients on that subnet.

### Scope settings usually include:

- Start IP
- End IP
- Subnet mask
- Default gateway
- DNS servers
- Lease duration
- Excluded addresses
- Reservations
- DHCP options

---

## 14. DHCP Pool

The DHCP pool is the group of addresses available for assignment.

The terms **scope** and **pool** are sometimes used differently depending on platform, but in normal conversation they are closely related.

### Example

If the DHCP range is:

```text
192.168.10.100 - 192.168.10.200
```

then that is the address pool available to clients.

### Pool exhaustion

Pool exhaustion happens when there are no available addresses left.

Possible causes:

- Too many clients
- Lease time too long
- Scope too small
- Rogue or misbehaving clients
- Devices not releasing addresses
- Attack or DHCP starvation attempt

When a pool is exhausted, new clients cannot get an address.

---

## 15. Exclusions

An exclusion is an address or range that the DHCP server should not assign.

### Example

You may have this subnet:

```text
192.168.10.0/24
```

You want DHCP to use:

```text
192.168.10.1 - 192.168.10.254
```

But some addresses are used manually:

```text
192.168.10.1    Gateway
192.168.10.10   DNS server
192.168.10.20   Printer
```

You should exclude those addresses from DHCP assignment.

### Why exclusions matter

Exclusions prevent DHCP from assigning addresses that are already used by static devices.

Without exclusions, you can get duplicate IP conflicts.

---

## 16. Reservations

A DHCP reservation gives a specific device the same IP address every time.

The reservation is usually based on the client MAC address or client identifier.

### Example

A printer has this MAC address:

```text
AA:BB:CC:11:22:33
```

You reserve this IP for it:

```text
192.168.10.50
```

Whenever that printer asks DHCP for an address, the server gives it `192.168.10.50`.

### Common uses

Reservations are useful for:

- Printers
- IP cameras
- Servers
- Network devices
- VoIP phones
- Important workstations
- Lab machines

### Benefit

The device keeps a predictable IP address, but the configuration stays centralized on the DHCP server.

---

## 17. Static IP vs DHCP Reservation

A static IP is configured directly on the device.

A DHCP reservation is configured on the DHCP server.

### Static IP

The device itself is manually configured.

Example:

```text
IP address: 192.168.10.20
Subnet mask: 255.255.255.0
Gateway: 192.168.10.1
DNS: 192.168.10.10
```

### DHCP reservation

The device asks DHCP, and DHCP always gives it the same address.

### Comparison

| Feature | Static IP | DHCP Reservation |
|---|---|---|
| Configured on | Device | DHCP server |
| Central management | No | Yes |
| Good for servers | Sometimes | Often |
| Easy to change DNS/gateway | No | Yes |
| Risk of local typo | Higher | Lower |
| Works without DHCP server | Yes | No |

### Practical advice

For many devices, DHCP reservations are easier to manage than static IPs.

But for critical infrastructure, some administrators still prefer static addressing or a carefully designed mix.

---

## 18. DHCP Options

DHCP options are extra settings provided by the DHCP server.

The IP address alone is not enough for most devices.

DHCP options tell clients how to use the network.

### Examples

A DHCP server may provide:

```text
Option 1:   Subnet mask
Option 3:   Default gateway / router
Option 6:   DNS servers
Option 15:  Domain name
Option 42:  NTP servers
Option 66:  TFTP server
Option 67:  Boot file name
```

Different vendors and systems may use different options for special features.

---

## 19. Common DHCP Options

Here are some common DHCP options.

| Option | Name | Purpose |
|---:|---|---|
| `1` | Subnet Mask | Gives the subnet mask |
| `3` | Router | Gives the default gateway |
| `6` | DNS Servers | Gives DNS resolver addresses |
| `12` | Host Name | Client hostname |
| `15` | Domain Name | DNS suffix/domain |
| `42` | NTP Servers | Time servers |
| `51` | Lease Time | Lease duration |
| `53` | DHCP Message Type | Discover, Offer, Request, ACK, etc. |
| `54` | DHCP Server Identifier | Identifies the DHCP server |
| `58` | Renewal Time | T1 renewal timer |
| `59` | Rebinding Time | T2 rebinding timer |
| `66` | TFTP Server Name | Often used for PXE or phones |
| `67` | Bootfile Name | PXE boot file |
| `150` | TFTP Server Address | Common with Cisco IP phones |

### Option 3: Router

This tells the client the default gateway.

Without a correct gateway, the client may communicate locally but fail to reach other networks.

### Option 6: DNS Servers

This tells the client which DNS servers to use.

If DNS is wrong, users may say “the Internet is down” even when IP connectivity works.

### Option 15: Domain Name

This gives the client a DNS suffix.

Example:

```text
company.local
```

This helps clients resolve short names.

### Options 66 and 67

These are common in PXE boot environments.

They help clients find boot servers and boot files.

### Option 150

This is often seen in Cisco voice environments to tell IP phones where the TFTP server is.

---

## 20. DHCP Relay / IP Helper

DHCP broadcast traffic normally stays inside the local broadcast domain.

That means a DHCP server on another subnet will not hear the client Discover message by default.

A DHCP relay solves this problem.

### What DHCP relay does

A DHCP relay receives DHCP broadcasts from clients and forwards them to a DHCP server on another network.

On Cisco devices, this is often configured with:

```text
ip helper-address
```

### Example

A client is in VLAN 20:

```text
Client subnet: 192.168.20.0/24
```

The DHCP server is in VLAN 10:

```text
DHCP server: 192.168.10.5
```

The router or Layer 3 switch interface for VLAN 20 can forward DHCP requests to `192.168.10.5`.

### Simple flow

```text
Client -> Broadcast DHCP Discover
Relay  -> Forwards request to DHCP server
Server -> Sends response back to relay
Relay  -> Sends response to client
```

### Why relay is useful

With DHCP relay, you do not need one DHCP server inside every VLAN.

One central DHCP server can serve many subnets.

---

## 21. DHCP Across VLANs

Each VLAN is usually a separate broadcast domain and often a separate IP subnet.

Because DHCP Discover uses broadcast, a client in one VLAN cannot directly discover a DHCP server in another VLAN unless relay is configured.

### Example

| VLAN | Subnet | Purpose |
|---:|---|---|
| 10 | `192.168.10.0/24` | Servers |
| 20 | `192.168.20.0/24` | Users |
| 30 | `192.168.30.0/24` | Voice |
| 40 | `192.168.40.0/24` | Guests |

If the DHCP server is in VLAN 10, then VLANs 20, 30, and 40 need DHCP relay configured on their Layer 3 gateway interfaces.

### Common issue

If clients in one VLAN get DHCP but clients in another VLAN do not, check the DHCP relay configuration for the failing VLAN.

---

## 22. DHCP and Routers

The default gateway given by DHCP is usually a router or Layer 3 interface.

### Example

Client configuration from DHCP:

```text
IP address:      192.168.20.55
Subnet mask:     255.255.255.0
Default gateway: 192.168.20.1
DNS server:      192.168.10.10
```

The client uses the default gateway to reach anything outside its local subnet.

### If gateway is wrong

The client may be able to reach devices in the same subnet but fail to reach:

- Internet
- Other VLANs
- Servers in other networks
- Cloud services
- VPN destinations

So when troubleshooting DHCP, always check whether the default gateway option is correct.

---

## 23. DHCP and DNS

DHCP commonly tells clients which DNS servers to use.

This is usually done with DHCP option 6.

### Example

```text
DNS servers: 192.168.10.10, 192.168.10.11
```

### DNS suffix

DHCP may also provide a DNS suffix using option 15.

Example:

```text
Domain name: corp.example.com
```

This lets users type short names like:

```text
fileserver1
```

and the system may try:

```text
fileserver1.corp.example.com
```

### Dynamic DNS updates

Some DHCP servers can update DNS records automatically.

Example:

A client gets this IP:

```text
192.168.20.55
```

The DHCP server may create or update a DNS record:

```text
laptop123.corp.example.com -> 192.168.20.55
```

This is common in Microsoft Active Directory environments.

---

## 24. DHCPv6

DHCPv6 is DHCP for IPv6.

It uses:

```text
UDP 546 client
UDP 547 server
```

DHCPv6 can provide IPv6 configuration to clients.

### DHCPv6 can provide:

- IPv6 addresses
- DNS servers
- Domain search list
- Other network options

### Important difference from IPv4

IPv6 networks may not always use DHCPv6 for address assignment.

IPv6 can also use SLAAC, where clients form their own address based on router advertisements.

DHCPv6 is often used for extra information, even when SLAAC provides the address.

---

## 25. SLAAC vs DHCPv6

IPv6 has multiple address configuration methods.

### SLAAC

SLAAC stands for **Stateless Address Autoconfiguration**.

With SLAAC, a client can create its own IPv6 address using information from router advertisements.

### DHCPv6

DHCPv6 can assign addresses or provide extra settings.

### Common modes

| Mode | Description |
|---|---|
| SLAAC only | Client creates address from router advertisement |
| SLAAC + DHCPv6 options | SLAAC gives address, DHCPv6 gives DNS or other options |
| Stateful DHCPv6 | DHCPv6 assigns the IPv6 address |
| Static IPv6 | Address is configured manually |

### Practical note

IPv6 address assignment depends heavily on router advertisement flags and operating system behavior.

Do not assume IPv6 DHCP works exactly like IPv4 DHCP.

---

## 26. APIPA / Link-Local IPv4

APIPA stands for **Automatic Private IP Addressing**.

If a Windows client cannot get an IPv4 address from DHCP, it may assign itself an address in this range:

```text
169.254.0.0/16
```

### Example APIPA address

```text
169.254.23.88
```

### What APIPA means

An APIPA address usually means:

```text
The client did not successfully receive a DHCP address.
```

The client may communicate with other devices on the same link-local network, but it usually will not have normal network access.

### Common causes

- DHCP server down
- DHCP relay missing
- VLAN mismatch
- Cable or Wi-Fi problem
- Firewall blocking DHCP
- DHCP scope exhausted
- Client isolated from network

### Important point

Seeing a `169.254.x.x` address is a strong clue that DHCP failed.

---

## 27. Rogue DHCP Servers

A rogue DHCP server is an unauthorized DHCP server on the network.

It may accidentally or intentionally give wrong network settings to clients.

### Examples of rogue DHCP sources

- Home router plugged into office network
- Misconfigured lab server
- Virtual machine running DHCP
- Malicious device
- Internet sharing feature on a laptop

### Problems caused by rogue DHCP

Clients may receive:

- Wrong IP address
- Wrong subnet mask
- Wrong gateway
- Wrong DNS server
- Attacker-controlled DNS server
- No Internet access
- Access to the wrong network

### Security risk

A malicious rogue DHCP server can direct clients to a fake gateway or DNS server, allowing traffic interception or redirection.

---

## 28. DHCP Snooping

DHCP snooping is a Layer 2 security feature used on switches.

It helps protect against rogue DHCP servers.

### Basic idea

The switch marks ports as:

- Trusted
- Untrusted

DHCP server responses are allowed only from trusted ports.

Client-facing ports are usually untrusted.

### Example

| Port Type | Trust Setting |
|---|---|
| Uplink to DHCP server | Trusted |
| Uplink to router / distribution switch | Trusted |
| User access port | Untrusted |

If a rogue DHCP server sends Offer messages from an untrusted port, the switch can block them.

### Benefits

DHCP snooping helps prevent:

- Rogue DHCP servers
- DHCP spoofing
- Some man-in-the-middle attacks
- Bad address assignment from unauthorized devices

### Related features

DHCP snooping can also support:

- Dynamic ARP Inspection
- IP Source Guard

These features use DHCP snooping data to improve Layer 2 security.

---

## 29. Common DHCP Problems

### Problem 1: Client gets no IP address

Possible causes:

- DHCP server is down
- DHCP scope is exhausted
- Client is in wrong VLAN
- DHCP relay is missing
- Firewall blocks DHCP
- Switch port issue
- Wireless authentication issue

### Problem 2: Client gets APIPA address

Example:

```text
169.254.10.20
```

This usually means DHCP failed.

### Problem 3: Client gets wrong IP range

Possible causes:

- Wrong VLAN
- Rogue DHCP server
- Incorrect DHCP scope
- Wrong SSID mapping
- Misconfigured trunk
- Incorrect relay address

### Problem 4: Client gets IP but no Internet

Possible causes:

- Wrong default gateway
- Wrong subnet mask
- DNS issue
- Firewall issue
- Routing issue
- NAT issue

### Problem 5: Client gets IP but cannot resolve names

Possible causes:

- Wrong DNS servers from DHCP
- DNS server down
- DNS suffix issue
- Client using stale DNS settings
- Split DNS or VPN DNS issue

### Problem 6: Some clients work, others fail

Possible causes:

- Scope exhaustion
- DHCP reservations conflict
- MAC filtering
- Wireless segmentation
- Lease issues
- Intermittent relay or network problem

### Problem 7: Duplicate IP address

Possible causes:

- Static IP inside DHCP pool
- Missing exclusion
- Bad reservation
- Rogue DHCP server
- Manual misconfiguration
- Restored VM with old settings

---

## 30. DHCP Troubleshooting Flow

Use a clear flow when troubleshooting DHCP.

### Step 1: Check the client IP

On Windows:

```powershell
ipconfig /all
```

On Linux:

```bash
ip addr
```

Check:

- IP address
- Subnet mask
- Default gateway
- DNS servers
- DHCP enabled status
- DHCP server address
- Lease obtained time
- Lease expiration time

### Step 2: Check for APIPA

If the client has:

```text
169.254.x.x
```

then DHCP likely failed.

### Step 3: Check physical or wireless connectivity

Make sure the client is actually connected.

Check:

- Cable
- Link light
- Wi-Fi association
- Switch port status
- VLAN assignment
- Authentication status

### Step 4: Check VLAN

Make sure the client is in the correct VLAN.

A wrong VLAN can cause the client to reach the wrong DHCP scope or no DHCP server at all.

### Step 5: Check DHCP server health

Check that the DHCP service is running.

Also check:

- Scope active
- Available addresses
- Correct options
- Reservations
- Exclusions
- Event logs

### Step 6: Check scope exhaustion

If the scope has no free addresses, new clients cannot get a lease.

Possible fixes:

- Expand the scope
- Reduce lease time
- Remove stale leases
- Create a larger subnet
- Investigate abnormal client behavior

### Step 7: Check DHCP relay

If the DHCP server is on another subnet, verify relay configuration.

On Cisco-style devices, look for:

```text
ip helper-address
```

Make sure it points to the correct DHCP server.

### Step 8: Check firewalls and ACLs

DHCP needs UDP 67 and 68 for IPv4.

Make sure traffic is not blocked between:

- Client and relay
- Relay and server
- Server and relay
- Relay and client

### Step 9: Check for rogue DHCP servers

If clients get strange addresses or wrong gateways, look for unauthorized DHCP servers.

Tools and methods:

- Packet capture
- Switch DHCP snooping logs
- Compare DHCP server identifier
- Check client lease details

### Step 10: Capture packets

A packet capture can show exactly where DORA fails.

Look for:

```text
Discover
Offer
Request
ACK
```

If you see Discover but no Offer, the server or relay may not be responding.

If you see Offer but no Request, the client may not accept the offer or may be receiving multiple bad offers.

---

## 31. Useful DHCP Commands

### Windows

Show IP configuration:

```powershell
ipconfig /all
```

Release DHCP lease:

```powershell
ipconfig /release
```

Renew DHCP lease:

```powershell
ipconfig /renew
```

Flush DNS cache:

```powershell
ipconfig /flushdns
```

Show routing table:

```powershell
route print
```

Test connectivity:

```powershell
ping 192.168.1.1
Test-NetConnection example.com -Port 443
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

Renew DHCP lease with NetworkManager:

```bash
nmcli networking off
nmcli networking on
```

Using dhclient on systems that use it:

```bash
sudo dhclient -r
sudo dhclient
```

Check systemd network status:

```bash
networkctl status
```

Check resolver status:

```bash
resolvectl status
```

### macOS

Show interface configuration:

```bash
ifconfig
```

Show routes:

```bash
netstat -rn
```

Renew DHCP lease from GUI:

```text
System Settings -> Network -> Details -> TCP/IP -> Renew DHCP Lease
```

Renew DHCP lease from command line may depend on interface name.

Example:

```bash
sudo ipconfig set en0 DHCP
```

### Cisco-style commands

Show interface configuration:

```text
show running-config interface vlan 20
```

Check helper address:

```text
show running-config | include helper-address
```

Show DHCP bindings if the device is the DHCP server:

```text
show ip dhcp binding
```

Show DHCP pool:

```text
show ip dhcp pool
```

Debug DHCP carefully in a lab or maintenance window:

```text
debug ip dhcp server events
```

---

## 32. Packet Capture and DHCP

Packet capture is very useful for DHCP.

Tools like Wireshark or tcpdump can show the DHCP conversation.

### Wireshark filter

```text
bootp
```

DHCP is based on BOOTP, so Wireshark uses the `bootp` filter for DHCP.

### tcpdump examples

Capture DHCP traffic:

```bash
sudo tcpdump -i eth0 port 67 or port 68
```

More detailed output:

```bash
sudo tcpdump -i eth0 -n -vvv port 67 or port 68
```

### What to look for

Look for the DORA sequence:

```text
DHCP Discover
DHCP Offer
DHCP Request
DHCP ACK
```

### Interpretation

| What you see | Possible meaning |
|---|---|
| Discover only | No server response, relay issue, VLAN issue, firewall issue |
| Discover + Offer | Server is responding |
| Offer from unknown server | Possible rogue DHCP |
| Request + no ACK | Server rejected or response blocked |
| ACK with wrong options | DHCP scope/options misconfigured |

---

## 33. DHCP Best Practices

### Use clear scope design

Create DHCP scopes that match your VLANs and subnets clearly.

Example:

| VLAN | Subnet | Scope |
|---:|---|---|
| 10 | `192.168.10.0/24` | Users |
| 20 | `192.168.20.0/24` | Voice |
| 30 | `192.168.30.0/24` | Printers |
| 40 | `192.168.40.0/24` | Guests |

### Exclude infrastructure addresses

Do not allow DHCP to assign addresses used by:

- Gateways
- Servers
- Firewalls
- Switch management interfaces
- Printers with static IPs
- Wireless controllers
- Monitoring systems

### Use reservations where useful

Reservations are useful for devices that should keep the same IP but still be centrally managed.

Good candidates:

- Printers
- VoIP phones
- Cameras
- Lab devices
- Special workstations

### Monitor scope usage

Watch for scopes nearing exhaustion.

A good warning threshold may be around 80% usage, depending on environment.

### Use DHCP snooping

On managed switches, DHCP snooping helps prevent rogue DHCP servers.

### Keep lease times reasonable

Lease time depends on environment.

Shorter leases can be useful for:

- Guest Wi-Fi
- Classrooms
- Labs
- High-turnover networks

Longer leases can be useful for:

- Stable office networks
- Devices that rarely move
- Small networks with enough address space

### Document DHCP options

Document non-obvious options such as:

- PXE boot options
- Voice VLAN options
- TFTP server options
- Vendor-specific options
- Special DNS settings

### Use redundancy

In larger environments, avoid having only one DHCP server with no backup.

Common approaches:

- DHCP failover
- Split scopes
- Redundant DHCP servers
- Highly available firewall/router DHCP service

### Protect management access

Only authorized administrators should be able to change DHCP settings.

A small DHCP mistake can break an entire subnet.

---

## 34. Quick Summary

DHCP automatically gives network settings to clients.

The most important points to remember are:

- DHCP stands for **Dynamic Host Configuration Protocol**
- DHCP uses UDP `67` and `68` for IPv4
- DHCPv6 uses UDP `546` and `547`
- DHCP commonly provides IP address, subnet mask, gateway, and DNS servers
- The classic DHCP process is **DORA**
- DORA means Discover, Offer, Request, Acknowledge
- A DHCP lease is temporary
- Clients renew leases before they expire
- A DHCP scope defines the assignable address range
- Exclusions prevent DHCP from assigning protected addresses
- Reservations give the same device the same IP through DHCP
- DHCP relay allows a central DHCP server to serve multiple VLANs
- APIPA addresses usually mean DHCP failed
- Rogue DHCP servers can break or compromise networks
- DHCP snooping helps protect against rogue DHCP
- Packet capture is one of the best ways to troubleshoot DHCP

If you understand DORA, scopes, leases, options, relay, and common failure points, you can solve most DHCP problems quickly.
