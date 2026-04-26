# 13. Essential Network Commands

Network commands are the fastest way to understand what is happening on a device or across a network. A good command-line workflow can quickly tell you whether the problem is related to IP addressing, routing, DNS, ports, firewalls, latency, packet loss, or the application itself.

This guide explains the most useful network commands for Windows, Linux, and macOS in a practical way. The goal is not only to list commands, but to show what each command is for, when to use it, and how to read the output.

---

## Table of Contents

- [1. Why Network Commands Matter](#1-why-network-commands-matter)
- [2. Basic Troubleshooting Order](#2-basic-troubleshooting-order)
- [3. Windows Commands](#3-windows-commands)
- [4. Linux Commands](#4-linux-commands)
- [5. macOS Commands](#5-macos-commands)
- [6. Cross-Platform Tools](#6-cross-platform-tools)
- [7. IP Address Commands](#7-ip-address-commands)
- [8. Routing Commands](#8-routing-commands)
- [9. Ping Commands](#9-ping-commands)
- [10. Trace Route Commands](#10-trace-route-commands)
- [11. DNS Lookup Commands](#11-dns-lookup-commands)
- [12. ARP and Neighbor Table Commands](#12-arp-and-neighbor-table-commands)
- [13. Port and Socket Commands](#13-port-and-socket-commands)
- [14. HTTP and Web Testing Commands](#14-http-and-web-testing-commands)
- [15. Packet Capture Commands](#15-packet-capture-commands)
- [16. Throughput Testing Commands](#16-throughput-testing-commands)
- [17. Wireless Commands](#17-wireless-commands)
- [18. Firewall and Security-Related Commands](#18-firewall-and-security-related-commands)
- [19. Practical Troubleshooting Examples](#19-practical-troubleshooting-examples)
- [20. Command Cheat Sheet](#20-command-cheat-sheet)
- [21. Best Practices](#21-best-practices)
- [22. Quick Summary](#22-quick-summary)

---

## 1. Why Network Commands Matter

Network commands help you answer simple but important questions:

- Does this device have an IP address?
- Is the default gateway correct?
- Can the device reach another host?
- Is DNS working?
- Is a port open?
- Is traffic leaving the device?
- Is the route correct?
- Is there packet loss?
- Is latency high?
- Is the service listening?
- Is the firewall blocking traffic?

In real troubleshooting, guessing wastes time. Commands give you evidence.

### Example

A user says:

```text
The Internet is not working.
```

That could mean many things:

- No IP address
- Wrong gateway
- DNS failure
- Wi-Fi issue
- Firewall block
- Proxy issue
- Browser issue
- ISP issue
- Website outage

Network commands help narrow the problem down quickly.

---

## 2. Basic Troubleshooting Order

A good command workflow usually starts simple and moves upward.

### Recommended order

1. Check IP configuration
2. Check the default gateway
3. Ping the local gateway
4. Ping a remote IP address
5. Test DNS resolution
6. Trace the route
7. Test the target port
8. Check listening services
9. Capture packets if needed

### Simple example flow

```text
1. Do I have an IP address?
2. Do I have a gateway?
3. Can I ping the gateway?
4. Can I ping 8.8.8.8?
5. Can I resolve example.com?
6. Can I connect to port 443?
```

This simple order can solve many common network issues.

---

## 3. Windows Commands

Windows has many useful built-in network commands.

### `ipconfig`

Shows IP configuration.

```powershell
ipconfig
```

More detailed output:

```powershell
ipconfig /all
```

Use it to check:

- IPv4 address
- IPv6 address
- Subnet mask
- Default gateway
- DNS servers
- DHCP status
- DHCP server
- MAC address
- Lease time

### `ping`

Tests reachability using ICMP Echo.

```powershell
ping 8.8.8.8
ping example.com
```

Useful options:

```powershell
ping -t 8.8.8.8
ping -n 10 8.8.8.8
ping -4 example.com
ping -6 example.com
```

### `tracert`

Shows the path packets take to a destination.

```powershell
tracert example.com
```

Use it when you need to see where traffic may be stopping.

### `pathping`

Combines ping and traceroute-style information.

```powershell
pathping example.com
```

It can help detect packet loss along the path.

### `nslookup`

Queries DNS.

```powershell
nslookup example.com
nslookup google.com 8.8.8.8
```

Query a specific record type:

```powershell
nslookup -type=mx example.com
nslookup -type=txt example.com
```

### `Resolve-DnsName`

A modern PowerShell DNS lookup tool.

```powershell
Resolve-DnsName example.com
Resolve-DnsName example.com -Type MX
Resolve-DnsName example.com -Server 1.1.1.1
```

### `arp`

Shows the ARP table.

```powershell
arp -a
```

Use it to see IP-to-MAC mappings on the local network.

### `route`

Shows or changes the routing table.

```powershell
route print
```

Add a temporary route:

```powershell
route add 10.10.20.0 mask 255.255.255.0 192.168.1.1
```

Delete a route:

```powershell
route delete 10.10.20.0
```

### `netstat`

Shows network connections and listening ports.

```powershell
netstat -ano
```

Show listening ports:

```powershell
netstat -ano | findstr LISTENING
```

Find a specific port:

```powershell
netstat -ano | findstr :443
```

### `Test-NetConnection`

PowerShell tool for connectivity testing.

```powershell
Test-NetConnection example.com
Test-NetConnection example.com -Port 443
```

This is very useful for testing TCP ports.

### `Get-NetIPConfiguration`

Shows IP configuration in PowerShell format.

```powershell
Get-NetIPConfiguration
```

### `Get-NetRoute`

Shows routes.

```powershell
Get-NetRoute
```

### `Get-NetTCPConnection`

Shows TCP connections.

```powershell
Get-NetTCPConnection
```

Filter for listening ports:

```powershell
Get-NetTCPConnection -State Listen
```

### `netsh`

Older but powerful Windows network configuration tool.

Show interface IP configuration:

```powershell
netsh interface ip show config
```

Show IPv6 addresses:

```powershell
netsh interface ipv6 show addresses
```

Show WLAN profiles:

```powershell
netsh wlan show profiles
```

Show Wi-Fi interface details:

```powershell
netsh wlan show interfaces
```

---

## 4. Linux Commands

Linux has a strong set of networking tools. Some older commands still exist, but modern Linux usually prefers the `ip` and `ss` commands.

### `ip addr`

Shows IP addresses on interfaces.

```bash
ip addr
```

Shorter version:

```bash
ip a
```

Show one interface:

```bash
ip addr show eth0
```

### `ip route`

Shows the routing table.

```bash
ip route
```

Show IPv6 routes:

```bash
ip -6 route
```

### `ping`

Tests reachability.

```bash
ping 8.8.8.8
ping example.com
```

Limit the number of packets:

```bash
ping -c 4 8.8.8.8
```

Use IPv6:

```bash
ping -6 google.com
```

### `traceroute`

Shows the path to a destination.

```bash
traceroute example.com
```

If not installed, install it using your package manager.

### `tracepath`

A simpler path tracing tool available on many Linux systems.

```bash
tracepath example.com
```

### `dig`

Powerful DNS lookup tool.

```bash
dig example.com
dig example.com A
dig example.com AAAA
dig example.com MX
dig @1.1.1.1 example.com
```

Short output:

```bash
dig +short example.com
```

Trace DNS delegation:

```bash
dig +trace example.com
```

### `nslookup`

Older DNS lookup tool, still common.

```bash
nslookup example.com
```

### `host`

Simple DNS lookup tool.

```bash
host example.com
host -t mx example.com
```

### `ss`

Modern replacement for many `netstat` use cases.

Show listening TCP and UDP ports:

```bash
ss -tulpn
```

Show listening TCP ports:

```bash
ss -lntp
```

Show established TCP connections:

```bash
ss -ant state established
```

### `netstat`

Older tool, still useful if installed.

```bash
netstat -tulpn
netstat -rn
```

### `tcpdump`

Command-line packet capture tool.

```bash
sudo tcpdump -i eth0
```

Capture DNS:

```bash
sudo tcpdump -i eth0 port 53
```

Capture DHCP:

```bash
sudo tcpdump -i eth0 port 67 or port 68
```

Capture traffic to a host:

```bash
sudo tcpdump -i eth0 host 192.0.2.10
```

### `curl`

Tests HTTP, HTTPS, APIs, headers, and connectivity.

```bash
curl https://example.com
curl -I https://example.com
curl -v https://example.com
```

### `wget`

Downloads files and can test web access.

```bash
wget https://example.com/file.txt
```

### `nc`

Netcat is useful for testing TCP and UDP connectivity.

Test TCP port:

```bash
nc -vz example.com 443
```

Listen on a port:

```bash
nc -l -p 9000
```

### `mtr`

Combines ping and traceroute in a live view.

```bash
mtr example.com
```

Report mode:

```bash
mtr -r -c 20 example.com
```

### `ethtool`

Shows and changes Ethernet interface settings.

```bash
sudo ethtool eth0
```

Use it to check:

- Speed
- Duplex
- Link detected
- Auto-negotiation
- Driver information

### `nmcli`

NetworkManager command-line tool.

Show devices:

```bash
nmcli device status
```

Show connections:

```bash
nmcli connection show
```

Restart networking:

```bash
nmcli networking off
nmcli networking on
```

### `resolvectl`

Useful on systemd-based systems for DNS status.

```bash
resolvectl status
resolvectl query example.com
```

---

## 5. macOS Commands

macOS is Unix-based, so many Linux-style commands also work, but some commands are different.

### `ifconfig`

Shows interface configuration.

```bash
ifconfig
```

Show a specific interface:

```bash
ifconfig en0
```

### `netstat`

Shows routing and network information.

Show routing table:

```bash
netstat -rn
```

Show IPv6 routing table:

```bash
netstat -rn -f inet6
```

### `ping`

Tests reachability.

```bash
ping 8.8.8.8
ping example.com
```

Stop with `Ctrl+C`.

### `ping6`

Tests IPv6 reachability.

```bash
ping6 google.com
```

### `traceroute`

Shows path to destination.

```bash
traceroute example.com
```

### `traceroute6`

IPv6 traceroute.

```bash
traceroute6 google.com
```

### `dig`

DNS lookup tool.

```bash
dig example.com
dig example.com MX
dig @1.1.1.1 example.com
```

### `nslookup`

DNS lookup tool.

```bash
nslookup example.com
```

### `host`

Simple DNS lookup tool.

```bash
host example.com
```

### `scutil --dns`

Shows DNS configuration.

```bash
scutil --dns
```

This is very useful on macOS because DNS behavior may depend on interface, VPN, or search domain settings.

### `networksetup`

macOS network configuration tool.

List network services:

```bash
networksetup -listallnetworkservices
```

Show DNS servers for Wi-Fi:

```bash
networksetup -getdnsservers Wi-Fi
```

Set DNS servers:

```bash
sudo networksetup -setdnsservers Wi-Fi 1.1.1.1 8.8.8.8
```

Reset DNS servers to automatic:

```bash
sudo networksetup -setdnsservers Wi-Fi Empty
```

### `lsof`

Shows open files and network sockets.

Show network connections:

```bash
lsof -i
```

Show listening ports:

```bash
lsof -i -P -n | grep LISTEN
```

### `nc`

Tests TCP connectivity.

```bash
nc -vz example.com 443
```

### `curl`

Tests HTTP and HTTPS.

```bash
curl -I https://example.com
curl -v https://example.com
```

---

## 6. Cross-Platform Tools

Some tools are useful across many systems.

### Wireshark

Wireshark is a graphical packet analyzer.

Use it to inspect:

- DNS queries
- TCP handshakes
- TLS negotiation
- DHCP DORA process
- ARP traffic
- ICMP errors
- Retransmissions
- Packet loss behavior

Wireshark is very useful when command output is not enough.

### Nmap

Nmap is used for port scanning and service discovery.

Basic scan:

```bash
nmap 192.0.2.10
```

Scan specific ports:

```bash
nmap -p 22,80,443 192.0.2.10
```

Scan all TCP ports:

```bash
nmap -p- 192.0.2.10
```

Service detection:

```bash
nmap -sV 192.0.2.10
```

Security note: scan only systems you own or have permission to test.

### iperf3

iperf3 tests network throughput.

On server:

```bash
iperf3 -s
```

On client:

```bash
iperf3 -c server-ip
```

Test for 30 seconds:

```bash
iperf3 -c server-ip -t 30
```

Test UDP:

```bash
iperf3 -c server-ip -u -b 10M
```

### curl

curl is useful on Windows, Linux, and macOS.

```bash
curl -I https://example.com
curl -v https://example.com
```

Use it to test:

- HTTP response headers
- TLS behavior
- Redirects
- Proxy issues
- API responses

### nc / netcat

Netcat tests raw TCP or UDP connectivity.

```bash
nc -vz example.com 443
```

It is especially useful when you want to know if a TCP port is reachable.

---

## 7. IP Address Commands

Checking IP configuration is usually the first step.

### Windows

```powershell
ipconfig /all
Get-NetIPConfiguration
```

### Linux

```bash
ip addr
ip -br addr
```

### macOS

```bash
ifconfig
```

### What to check

Look for:

- Correct IP address
- Correct subnet mask or prefix
- Correct default gateway
- Correct DNS servers
- DHCP or static configuration
- Duplicate address warnings
- Link-local address

### Red flags

| Symptom | Possible Meaning |
|---|---|
| `169.254.x.x` address | DHCP failed on IPv4 |
| No default gateway | Cannot reach remote networks |
| Wrong subnet mask | Local/remote traffic may fail |
| Wrong DNS server | Name resolution problems |
| No IPv6 link-local | IPv6 may not be enabled correctly |

---

## 8. Routing Commands

Routing commands show where traffic will go.

### Windows

```powershell
route print
Get-NetRoute
```

### Linux

```bash
ip route
ip -6 route
```

### macOS

```bash
netstat -rn
netstat -rn -f inet6
```

### What to check

- Is there a default route?
- Is the next hop correct?
- Is the route using the right interface?
- Is there a more specific route overriding the expected path?
- Are VPN routes present?
- Are duplicate or conflicting routes present?

### Default route examples

IPv4 default route:

```text
0.0.0.0/0
```

IPv6 default route:

```text
::/0
```

If there is no default route, the host usually cannot reach outside its local subnet.

---

## 9. Ping Commands

Ping uses ICMP to test basic reachability.

### Common tests

Ping localhost:

```bash
ping 127.0.0.1
```

Ping your own IP:

```bash
ping 192.168.1.50
```

Ping the gateway:

```bash
ping 192.168.1.1
```

Ping a remote IP:

```bash
ping 8.8.8.8
```

Ping a DNS name:

```bash
ping example.com
```

### How to read ping output

Look for:

- Reply or timeout
- Latency
- Packet loss
- Consistency
- Name resolution behavior

### Important note

Ping failure does not always mean the host is down.

ICMP may be blocked by:

- Firewall
- Security policy
- Cloud security group
- Router ACL
- Host firewall

So ping is useful, but it is not the final truth.

---

## 10. Trace Route Commands

Traceroute-style tools show the path to a destination.

### Windows

```powershell
tracert example.com
```

### Linux

```bash
traceroute example.com
tracepath example.com
```

### macOS

```bash
traceroute example.com
```

### What traceroute helps with

- Finding where traffic stops
- Seeing path changes
- Detecting routing loops
- Identifying high-latency hops
- Understanding ISP or WAN paths

### Important note

Some routers do not respond to traceroute probes. Asterisks do not always mean the path is broken.

Example:

```text
* * *
```

This may mean the router is filtering or rate-limiting responses.

If later hops still respond, traffic is passing through.

---

## 11. DNS Lookup Commands

DNS commands help you check whether names resolve correctly.

### Windows

```powershell
nslookup example.com
Resolve-DnsName example.com
Resolve-DnsName example.com -Type MX
```

### Linux

```bash
dig example.com
dig example.com A
dig example.com AAAA
dig example.com MX
host example.com
nslookup example.com
```

### macOS

```bash
dig example.com
host example.com
nslookup example.com
scutil --dns
```

### Query a specific DNS server

```bash
dig @1.1.1.1 example.com
nslookup example.com 8.8.8.8
```

### Useful record checks

A record:

```bash
dig example.com A
```

AAAA record:

```bash
dig example.com AAAA
```

MX record:

```bash
dig example.com MX
```

TXT record:

```bash
dig example.com TXT
```

NS record:

```bash
dig example.com NS
```

Reverse lookup:

```bash
dig -x 8.8.8.8
```

### What to check

- Does the name resolve?
- Does it return the expected IP?
- Are different DNS servers giving different answers?
- Is the TTL high or low?
- Are internal users getting internal answers?
- Are VPN users using the correct DNS server?

---

## 12. ARP and Neighbor Table Commands

ARP maps IPv4 addresses to MAC addresses on the local network.

IPv6 uses Neighbor Discovery instead of ARP.

### Windows

Show ARP table:

```powershell
arp -a
```

Show neighbor table in PowerShell:

```powershell
Get-NetNeighbor
```

### Linux

Show neighbors:

```bash
ip neigh
```

Older ARP command:

```bash
arp -a
```

### macOS

```bash
arp -a
```

### When this is useful

Use ARP/neighbor tables when checking:

- Local gateway reachability
- Duplicate IP problems
- MAC address changes
- Layer 2 issues
- Whether the host can resolve a local IP to MAC

### Example problem

If a host cannot ping its gateway, check whether it has an ARP entry for the gateway.

If there is no ARP entry, the problem may be Layer 2, VLAN, cabling, wireless, or gateway availability.

---

## 13. Port and Socket Commands

Port commands help you see what is listening and what connections exist.

### Windows

```powershell
netstat -ano
netstat -ano | findstr LISTENING
Get-NetTCPConnection
Test-NetConnection example.com -Port 443
```

### Linux

```bash
ss -tulpn
ss -lntp
nc -vz example.com 443
```

### macOS

```bash
lsof -i -P -n
lsof -i -P -n | grep LISTEN
nc -vz example.com 443
```

### What to check

- Is the service listening?
- Is it listening on the expected port?
- Is it listening on all interfaces or only localhost?
- Is the client reaching the port?
- Is the firewall blocking the connection?

### Localhost-only example

If a database listens on:

```text
127.0.0.1:5432
```

then remote clients cannot connect to it.

It is only listening locally.

---

## 14. HTTP and Web Testing Commands

HTTP and HTTPS are commonly tested with curl.

### Basic request

```bash
curl https://example.com
```

### Show response headers

```bash
curl -I https://example.com
```

### Verbose mode

```bash
curl -v https://example.com
```

Verbose mode can show:

- DNS resolution
- TCP connection
- TLS handshake
- HTTP request
- HTTP response headers

### Follow redirects

```bash
curl -L https://example.com
```

### Test a specific Host header

```bash
curl -H "Host: example.com" http://192.0.2.10
```

This is useful when testing web servers, virtual hosts, load balancers, and reverse proxies.

### Ignore certificate validation for testing

```bash
curl -k https://example.com
```

Use this only for testing. Do not ignore certificate validation as a normal habit.

---

## 15. Packet Capture Commands

Packet capture is used when normal commands do not show enough detail.

### Linux tcpdump examples

Capture all traffic on an interface:

```bash
sudo tcpdump -i eth0
```

Capture DNS:

```bash
sudo tcpdump -i eth0 port 53
```

Capture ICMP:

```bash
sudo tcpdump -i eth0 icmp
```

Capture traffic to or from a host:

```bash
sudo tcpdump -i eth0 host 192.0.2.10
```

Capture TCP port 443:

```bash
sudo tcpdump -i eth0 tcp port 443
```

Write capture to file:

```bash
sudo tcpdump -i eth0 -w capture.pcap
```

Read capture from file:

```bash
tcpdump -r capture.pcap
```

### Wireshark filters

DNS:

```text
dns
```

DHCP:

```text
bootp
```

TCP SYN:

```text
tcp.flags.syn == 1
```

HTTP:

```text
http
```

ICMP:

```text
icmp
```

Specific IP:

```text
ip.addr == 192.0.2.10
```

### When to capture packets

Packet capture is useful when:

- DHCP fails
- DNS gives strange behavior
- TCP handshakes fail
- TLS negotiation fails
- Packets leave but replies do not return
- Firewall behavior is unclear
- Application logs are not enough

---

## 16. Throughput Testing Commands

Throughput testing checks how much traffic a network path can carry.

### iperf3 server

On one machine:

```bash
iperf3 -s
```

### iperf3 client

On another machine:

```bash
iperf3 -c server-ip
```

### Longer test

```bash
iperf3 -c server-ip -t 30
```

### Reverse direction test

```bash
iperf3 -c server-ip -R
```

### Parallel streams

```bash
iperf3 -c server-ip -P 4
```

### UDP test

```bash
iperf3 -c server-ip -u -b 50M
```

### What to check

- Throughput
- Retransmissions
- Jitter for UDP
- Packet loss for UDP
- Directional differences
- Performance difference between wired and wireless

### Important warning

Throughput tests can consume real bandwidth. Be careful on production networks.

---

## 17. Wireless Commands

Wireless troubleshooting often needs signal and association information.

### Windows

Show Wi-Fi interface:

```powershell
netsh wlan show interfaces
```

Show saved profiles:

```powershell
netsh wlan show profiles
```

Show nearby networks:

```powershell
netsh wlan show networks mode=bssid
```

### Linux

Show wireless interfaces:

```bash
iw dev
```

Show link information:

```bash
iw dev wlan0 link
```

Scan nearby networks:

```bash
sudo iw dev wlan0 scan
```

NetworkManager status:

```bash
nmcli device wifi list
nmcli device status
```

### macOS

Airport utility path may vary by macOS version, but commonly:

```bash
/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -I
```

Scan networks:

```bash
/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -s
```

### What to check

- SSID
- BSSID
- Signal strength
- Channel
- Band
- Noise
- Transmit rate
- Security mode
- Roaming behavior

---

## 18. Firewall and Security-Related Commands

Firewall commands depend on the operating system.

### Windows Defender Firewall

Show firewall profiles:

```powershell
Get-NetFirewallProfile
```

Show firewall rules:

```powershell
Get-NetFirewallRule
```

Test a port:

```powershell
Test-NetConnection example.com -Port 443
```

### Linux firewalld

Show status:

```bash
sudo firewall-cmd --state
```

List rules:

```bash
sudo firewall-cmd --list-all
```

### Linux ufw

Show status:

```bash
sudo ufw status verbose
```

Allow SSH:

```bash
sudo ufw allow 22/tcp
```

### Linux iptables

List rules:

```bash
sudo iptables -L -n -v
```

### Linux nftables

List ruleset:

```bash
sudo nft list ruleset
```

### Security reminder

Do not change firewall rules blindly. A small mistake can expose a service or disconnect remote access.

---

## 19. Practical Troubleshooting Examples

### Example 1: User cannot access the Internet

Start with IP configuration:

```powershell
ipconfig /all
```

or:

```bash
ip addr
ip route
```

Check:

- Does the device have a valid IP?
- Is there a default gateway?
- Can it ping the gateway?
- Can it ping a public IP?
- Can it resolve DNS?

Commands:

```bash
ping 192.168.1.1
ping 8.8.8.8
nslookup example.com
```

### Example 2: DNS is not working

Check direct IP connectivity:

```bash
ping 8.8.8.8
```

Then test DNS:

```bash
nslookup example.com
dig example.com
```

Query a specific resolver:

```bash
dig @1.1.1.1 example.com
```

If IP works but DNS fails, focus on DNS resolver settings, firewall rules, or DNS server health.

### Example 3: Website is down for one user

Test DNS:

```bash
dig example.com
```

Test HTTPS:

```bash
curl -I https://example.com
```

Test port 443:

```bash
nc -vz example.com 443
```

Trace route:

```bash
traceroute example.com
```

On Windows:

```powershell
Test-NetConnection example.com -Port 443
tracert example.com
```

### Example 4: SSH to a server fails

Test port 22:

```bash
nc -vz server.example.com 22
```

Check route:

```bash
ip route
```

Check DNS:

```bash
dig server.example.com
```

On the server, check listening ports:

```bash
ss -lntp | grep :22
```

Possible causes:

- SSH service stopped
- Firewall blocking port 22
- Wrong IP address
- Wrong route
- Server listening on a custom port
- Security group blocking access

### Example 5: DHCP is failing

On client:

```powershell
ipconfig /all
ipconfig /release
ipconfig /renew
```

On Linux:

```bash
ip addr
sudo dhclient -r
sudo dhclient
```

Capture DHCP:

```bash
sudo tcpdump -i eth0 port 67 or port 68
```

Look for:

```text
Discover -> Offer -> Request -> ACK
```

### Example 6: Port is open locally but not remotely

On server:

```bash
ss -tulpn
```

Check if the service is listening on all interfaces or localhost only.

Localhost only:

```text
127.0.0.1:8080
```

Remote clients cannot connect to that.

All interfaces:

```text
0.0.0.0:8080
```

Then check firewall and routing.

---

## 20. Command Cheat Sheet

### Windows quick list

```powershell
ipconfig /all
ping 8.8.8.8
ping example.com
tracert example.com
pathping example.com
nslookup example.com
Resolve-DnsName example.com
arp -a
route print
netstat -ano
Test-NetConnection example.com -Port 443
Get-NetIPConfiguration
Get-NetRoute
Get-NetTCPConnection
netsh wlan show interfaces
```

### Linux quick list

```bash
ip addr
ip route
ip -6 route
ping 8.8.8.8
ping -c 4 example.com
traceroute example.com
tracepath example.com
dig example.com
dig +short example.com
nslookup example.com
host example.com
ip neigh
ss -tulpn
ss -lntp
curl -I https://example.com
curl -v https://example.com
nc -vz host port
sudo tcpdump -i eth0
mtr example.com
iperf3 -s
iperf3 -c server-ip
sudo ethtool eth0
resolvectl status
```

### macOS quick list

```bash
ifconfig
netstat -rn
ping 8.8.8.8
ping6 google.com
traceroute example.com
traceroute6 google.com
dig example.com
nslookup example.com
host example.com
scutil --dns
networksetup -listallnetworkservices
lsof -i -P -n
nc -vz example.com 443
curl -I https://example.com
```

---

## 21. Best Practices

### Start with the basics

Do not start with packet captures unless needed.

First check:

- IP address
- Gateway
- DNS
- Route
- Basic reachability

### Compare working and broken devices

If one device works and another does not, compare:

- IP address
- Subnet mask
- Gateway
- DNS servers
- Routes
- Firewall state
- Proxy settings
- VLAN or Wi-Fi network

### Test by IP and by name

If IP works but name fails, suspect DNS.

Example:

```bash
ping 8.8.8.8
ping google.com
```

### Test the actual port

Ping is not enough for application testing.

A website needs TCP 80 or 443.

An SSH server needs TCP 22.

A database may need TCP 3306 or 5432.

Use:

```bash
nc -vz example.com 443
```

or:

```powershell
Test-NetConnection example.com -Port 443
```

### Be careful with destructive commands

Some commands change system state.

Examples:

```powershell
ipconfig /release
route delete
netsh reset
```

```bash
sudo ip route del
sudo systemctl restart networking
sudo ufw enable
```

Understand the impact before running them, especially on remote systems.

### Save outputs when troubleshooting

For longer incidents, save command outputs.

This helps with:

- Comparing before and after
- Sharing with teammates
- Writing incident notes
- Proving what changed

### Use timestamps

When testing intermittent issues, record the time.

Example:

```text
10:05 - ping loss started
10:07 - DNS lookup failed
10:10 - route changed
```

This makes logs easier to compare.

---

## 22. Quick Summary

Essential network commands help you troubleshoot quickly and accurately.

The most important points to remember are:

- Start by checking IP configuration
- Confirm the default gateway
- Test local and remote reachability
- Test DNS separately from IP connectivity
- Use traceroute tools to inspect the path
- Use port testing tools for real application checks
- Use socket commands to see listening services
- Use packet capture when normal tools are not enough
- Use iperf3 for throughput testing
- Use Wireshark or tcpdump for deeper analysis
- Compare working and broken devices
- Be careful with commands that change configuration

A strong troubleshooting workflow is not about memorizing every command. It is about knowing which question to ask next, then choosing the command that answers it.
