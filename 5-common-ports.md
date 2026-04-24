# 5. Common Ports

Network ports help computers separate different services and conversations. When a device receives network traffic, the destination port helps the operating system decide which application or service should handle that traffic.

This guide explains common TCP and UDP ports in a practical way. It is useful for troubleshooting, firewall rules, security reviews, interview preparation, and general network administration.

---

## Table of Contents

- [1. What a Port Is](#1-what-a-port-is)
- [2. Why Ports Matter](#2-why-ports-matter)
- [3. TCP vs UDP Ports](#3-tcp-vs-udp-ports)
- [4. Port Number Ranges](#4-port-number-ranges)
- [5. Common Port Table](#5-common-port-table)
- [6. Web and Remote Access Ports](#6-web-and-remote-access-ports)
- [7. Email Ports](#7-email-ports)
- [8. DNS and DHCP Ports](#8-dns-and-dhcp-ports)
- [9. File Transfer and File Sharing Ports](#9-file-transfer-and-file-sharing-ports)
- [10. Authentication and Directory Service Ports](#10-authentication-and-directory-service-ports)
- [11. Routing and Network Service Ports](#11-routing-and-network-service-ports)
- [12. Monitoring and Logging Ports](#12-monitoring-and-logging-ports)
- [13. VPN and Tunneling Ports](#13-vpn-and-tunneling-ports)
- [14. Database Ports](#14-database-ports)
- [15. Voice and Real-Time Communication Ports](#15-voice-and-real-time-communication-ports)
- [16. Common Alternative Ports](#16-common-alternative-ports)
- [17. Source Ports vs Destination Ports](#17-source-ports-vs-destination-ports)
- [18. Listening Ports](#18-listening-ports)
- [19. Firewall Rules and Ports](#19-firewall-rules-and-ports)
- [20. Port Scanning Basics](#20-port-scanning-basics)
- [21. Common Troubleshooting Examples](#21-common-troubleshooting-examples)
- [22. Useful Commands](#22-useful-commands)
- [23. Security Notes](#23-security-notes)
- [24. Quick Summary](#24-quick-summary)

---

## 1. What a Port Is

A port is a logical number used by TCP and UDP to identify a specific service or application on a device.

An IP address identifies the device.

A port identifies the service on that device.

### Simple example

If a server has this IP address:

```text
192.0.2.10
```

It may run several services at the same time:

```text
192.0.2.10:22    SSH
192.0.2.10:80    HTTP
192.0.2.10:443   HTTPS
192.0.2.10:3306  MySQL
```

The IP address tells the client which host to reach.

The port tells the client which service to use.

---

## 2. Why Ports Matter

Ports are important because many services can run on the same device.

Without ports, the system would not know which application should receive incoming traffic.

Ports are used in:

- Web browsing
- Remote access
- Email
- DNS
- File sharing
- Databases
- VPNs
- Monitoring
- Firewalls
- Security tools
- Troubleshooting

### Practical example

When you open:

```text
https://example.com
```

your browser usually connects to:

```text
example.com:443
```

Port `443` tells the remote server that this is HTTPS traffic.

---

## 3. TCP vs UDP Ports

TCP and UDP both use port numbers, but they behave differently.

### TCP

TCP is connection-oriented.

It is used when reliability matters.

TCP provides:

- Connection setup
- Ordered delivery
- Retransmission
- Flow control
- Error recovery

Common TCP services:

- HTTP
- HTTPS
- SSH
- SMTP
- IMAP
- RDP
- SMB
- Databases

### UDP

UDP is connectionless.

It is used when speed or simplicity is preferred.

UDP does not provide the same built-in reliability as TCP.

Common UDP services:

- DNS queries
- DHCP
- NTP
- SNMP
- VoIP media
- Some VPN traffic
- Some gaming traffic

### Important point

The same port number can exist in both TCP and UDP.

For example:

```text
53/TCP
53/UDP
```

Both are DNS-related, but they are not the same flow.

---

## 4. Port Number Ranges

Port numbers go from:

```text
0 to 65535
```

They are usually divided into three groups.

| Range | Name | Common Use |
|---:|---|---|
| `0 - 1023` | Well-known ports | Common system services |
| `1024 - 49151` | Registered ports | Vendor and application services |
| `49152 - 65535` | Dynamic / ephemeral ports | Temporary client-side ports |

### Well-known ports

These are assigned to very common services.

Examples:

- `22` SSH
- `53` DNS
- `80` HTTP
- `443` HTTPS

### Registered ports

These are often used by specific applications or platforms.

Examples:

- `1433` Microsoft SQL Server
- `3306` MySQL
- `3389` RDP
- `5432` PostgreSQL

### Dynamic or ephemeral ports

These are usually temporary ports used by clients.

Example:

A laptop may connect from a random high source port to a web server on port `443`.

```text
Client: 192.0.2.50:51544 -> Server: 203.0.113.10:443
```

The server uses destination port `443`, while the client uses a temporary source port.

---

## 5. Common Port Table

This table lists many common ports that are useful to know.

| Port | Protocol | Service |
|---:|---|---|
| `20/21` | TCP | FTP |
| `22` | TCP | SSH |
| `23` | TCP | Telnet |
| `25` | TCP | SMTP |
| `53` | TCP/UDP | DNS |
| `67/68` | UDP | DHCP |
| `69` | UDP | TFTP |
| `80` | TCP | HTTP |
| `88` | TCP/UDP | Kerberos |
| `110` | TCP | POP3 |
| `123` | UDP | NTP |
| `135` | TCP | RPC Endpoint Mapper |
| `137-139` | TCP/UDP | NetBIOS |
| `143` | TCP | IMAP |
| `161/162` | UDP | SNMP / SNMP Traps |
| `179` | TCP | BGP |
| `389` | TCP/UDP | LDAP |
| `443` | TCP | HTTPS |
| `445` | TCP | SMB / CIFS |
| `500` | UDP | IKE / IPsec |
| `514` | UDP | Syslog |
| `520` | UDP | RIP |
| `546/547` | UDP | DHCPv6 |
| `636` | TCP | LDAPS |
| `853` | TCP | DNS over TLS |
| `993` | TCP | IMAPS |
| `995` | TCP | POP3S |
| `1194` | UDP/TCP | OpenVPN |
| `1433` | TCP | Microsoft SQL Server |
| `1521` | TCP | Oracle Database |
| `1701` | UDP | L2TP |
| `1723` | TCP | PPTP |
| `1812/1813` | UDP | RADIUS Authentication / Accounting |
| `2049` | TCP/UDP | NFS |
| `3306` | TCP | MySQL / MariaDB |
| `3389` | TCP | RDP |
| `4500` | UDP | IPsec NAT-T |
| `5060` | UDP/TCP | SIP |
| `5061` | TCP | SIP over TLS |
| `5432` | TCP | PostgreSQL |
| `5900` | TCP | VNC |
| `8080` | TCP | HTTP Alternate |
| `8443` | TCP | HTTPS Alternate |

---

## 6. Web and Remote Access Ports

These ports are very common in daily network work.

### HTTP - TCP 80

HTTP is the basic web protocol.

```text
Port: 80/TCP
```

It is used for unencrypted web traffic.

Example:

```text
http://example.com
```

### HTTPS - TCP 443

HTTPS is HTTP over TLS.

```text
Port: 443/TCP
```

It is used for encrypted web traffic.

Example:

```text
https://example.com
```

In modern networks, HTTPS is much more common than plain HTTP.

### SSH - TCP 22

SSH stands for **Secure Shell**.

```text
Port: 22/TCP
```

It is used for secure remote administration of Linux, Unix, network devices, and many cloud systems.

Common uses:

- Remote shell access
- Secure file transfer
- Tunneling
- Git over SSH
- Network device administration

### Telnet - TCP 23

Telnet is an older remote access protocol.

```text
Port: 23/TCP
```

Telnet sends data in clear text, including usernames and passwords.

For that reason, Telnet should usually be avoided on modern networks.

Use SSH instead.

### RDP - TCP 3389

RDP stands for **Remote Desktop Protocol**.

```text
Port: 3389/TCP
```

It is commonly used for remote access to Windows systems.

Security note:

RDP should not be exposed directly to the public Internet unless there is a very strong security design around it, such as VPN, MFA, restricted IP access, or a secure remote access gateway.

### VNC - TCP 5900

VNC is used for graphical remote control.

```text
Port: 5900/TCP
```

It is common in labs, remote support, and some server environments.

VNC security depends heavily on implementation and configuration, so it should be protected carefully.

---

## 7. Email Ports

Email uses several different protocols and ports.

### SMTP - TCP 25

SMTP stands for **Simple Mail Transfer Protocol**.

```text
Port: 25/TCP
```

It is mainly used for mail server to mail server delivery.

Example:

```text
Mail server A sends mail to Mail server B over TCP 25.
```

### SMTP Submission - TCP 587

Port `587` is commonly used by mail clients to submit outgoing email to a mail server.

```text
Port: 587/TCP
```

This is usually preferred over port 25 for client mail submission.

### SMTPS - TCP 465

Port `465` is commonly used for SMTP over implicit TLS.

```text
Port: 465/TCP
```

You may still see it in many mail configurations.

### POP3 - TCP 110

POP3 is used by clients to retrieve email.

```text
Port: 110/TCP
```

POP3 is older and less flexible than IMAP in many environments.

### POP3S - TCP 995

POP3S is POP3 over TLS.

```text
Port: 995/TCP
```

It is the encrypted version commonly used when POP3 is required.

### IMAP - TCP 143

IMAP is used by clients to access mailboxes.

```text
Port: 143/TCP
```

IMAP is generally better than POP3 for users who access mail from multiple devices.

### IMAPS - TCP 993

IMAPS is IMAP over TLS.

```text
Port: 993/TCP
```

This is commonly used for secure email access.

### Quick email table

| Port | Protocol | Use |
|---:|---|---|
| `25` | TCP | SMTP server-to-server |
| `465` | TCP | SMTPS |
| `587` | TCP | SMTP submission |
| `110` | TCP | POP3 |
| `995` | TCP | POP3S |
| `143` | TCP | IMAP |
| `993` | TCP | IMAPS |

---

## 8. DNS and DHCP Ports

### DNS - TCP/UDP 53

DNS uses:

```text
53/UDP
53/TCP
```

UDP is used for most normal DNS queries.

TCP is used for larger responses, zone transfers, and some DNSSEC-related behavior.

### DNS over TLS - TCP 853

DNS over TLS, or DoT, commonly uses:

```text
853/TCP
```

It encrypts DNS traffic using TLS.

### DNS over HTTPS - TCP 443

DNS over HTTPS, or DoH, uses HTTPS.

```text
443/TCP
```

This makes DNS queries look like normal HTTPS traffic.

### DHCP - UDP 67 and 68

DHCP uses UDP.

```text
67/UDP  DHCP server
68/UDP  DHCP client
```

The client starts without an IP address, so DHCP has special behavior during the address assignment process.

### DHCPv6 - UDP 546 and 547

DHCPv6 uses:

```text
546/UDP  DHCPv6 client
547/UDP  DHCPv6 server
```

DHCPv6 is used in IPv6 environments, often alongside or instead of SLAAC depending on the design.

---

## 9. File Transfer and File Sharing Ports

### FTP - TCP 20 and 21

FTP uses:

```text
20/TCP
21/TCP
```

Port `21` is used for FTP control.

Port `20` may be used for FTP data in active mode.

FTP can be confusing because it uses separate control and data connections.

Security note:

Traditional FTP is not encrypted. Use SFTP, FTPS, or another secure option when possible.

### TFTP - UDP 69

TFTP stands for **Trivial File Transfer Protocol**.

```text
69/UDP
```

It is simple and has no built-in authentication.

Common uses:

- Network device booting
- Firmware transfers
- PXE-related environments
- Simple lab file transfers

Security note:

TFTP should be restricted and not exposed broadly.

### SMB / CIFS - TCP 445

SMB is used for Windows file sharing and many file server environments.

```text
445/TCP
```

Common uses:

- File shares
- Printer sharing
- Windows domain environments
- Network storage access

Security note:

SMB should not be exposed directly to the public Internet.

### NetBIOS - TCP/UDP 137-139

NetBIOS ports are older Windows networking ports.

```text
137/UDP
138/UDP
139/TCP
```

They are still seen in some legacy networks.

Modern SMB usually uses port `445/TCP`.

### NFS - TCP/UDP 2049

NFS stands for **Network File System**.

```text
2049/TCP
2049/UDP
```

It is commonly used in Unix and Linux environments for file sharing.

---

## 10. Authentication and Directory Service Ports

### Kerberos - TCP/UDP 88

Kerberos is an authentication protocol.

```text
88/TCP
88/UDP
```

It is heavily used in Microsoft Active Directory environments.

Common use:

- Domain authentication
- Single sign-on
- Service tickets

### LDAP - TCP/UDP 389

LDAP stands for **Lightweight Directory Access Protocol**.

```text
389/TCP
389/UDP
```

It is used to query and manage directory information.

Common use:

- User lookup
- Group lookup
- Directory authentication
- Application integration with directories

### LDAPS - TCP 636

LDAPS is LDAP over TLS.

```text
636/TCP
```

It encrypts LDAP communication.

### RADIUS - UDP 1812 and 1813

RADIUS is used for centralized authentication, authorization, and accounting.

```text
1812/UDP  Authentication
1813/UDP  Accounting
```

Common use:

- VPN authentication
- Wi-Fi Enterprise authentication
- Network device login
- 802.1X environments

### TACACS+

TACACS+ is also used for network device authentication and authorization.

A common port is:

```text
49/TCP
```

Although it was not in the original compact table, it is useful to know for network administration.

---

## 11. Routing and Network Service Ports

### BGP - TCP 179

BGP stands for **Border Gateway Protocol**.

```text
179/TCP
```

It is used for routing between autonomous systems and in many large network designs.

Common use:

- Internet routing
- ISP routing
- Data center routing
- Cloud connectivity
- Large enterprise routing

### RIP - UDP 520

RIP stands for **Routing Information Protocol**.

```text
520/UDP
```

It is an older routing protocol and is mostly used in simple or legacy environments.

### NTP - UDP 123

NTP stands for **Network Time Protocol**.

```text
123/UDP
```

It is used to synchronize time.

Time synchronization is important for:

- Logs
- Authentication
- Certificates
- Security investigations
- Distributed systems
- Domain environments

### RPC Endpoint Mapper - TCP 135

RPC Endpoint Mapper is common in Windows environments.

```text
135/TCP
```

It helps clients find services that use Microsoft RPC.

You may see it during Windows administration, Active Directory operations, and enterprise troubleshooting.

---

## 12. Monitoring and Logging Ports

### SNMP - UDP 161

SNMP stands for **Simple Network Management Protocol**.

```text
161/UDP
```

It is used to query device information.

Common use:

- Interface status
- Bandwidth counters
- CPU usage
- Memory usage
- Device health
- Printer status
- UPS monitoring

### SNMP Traps - UDP 162

SNMP traps are alerts sent from devices to a monitoring system.

```text
162/UDP
```

Example:

A switch may send an SNMP trap when an interface goes down.

### Syslog - UDP 514

Syslog is used to send logs to a central log server.

```text
514/UDP
```

Some environments also use TCP or TLS-based syslog, but UDP 514 is the classic default.

Common use:

- Firewall logs
- Router logs
- Switch logs
- Linux system logs
- Security logs

---

## 13. VPN and Tunneling Ports

### IKE / IPsec - UDP 500

IKE is used by IPsec to negotiate security associations.

```text
500/UDP
```

Common use:

- Site-to-site VPN
- Remote access VPN
- IPsec tunnel setup

### IPsec NAT-T - UDP 4500

NAT-T stands for NAT Traversal.

```text
4500/UDP
```

It allows IPsec to work through NAT.

### L2TP - UDP 1701

L2TP stands for **Layer 2 Tunneling Protocol**.

```text
1701/UDP
```

It is often paired with IPsec for security.

### PPTP - TCP 1723

PPTP is an older VPN protocol.

```text
1723/TCP
```

Security note:

PPTP is considered outdated and should generally be avoided in modern secure networks.

### OpenVPN - UDP/TCP 1194

OpenVPN commonly uses:

```text
1194/UDP
1194/TCP
```

UDP is common for performance, but TCP can also be used.

### GRE

GRE is a tunneling protocol, but it is not TCP or UDP.

It uses IP protocol number `47`.

This is different from a port number.

This distinction matters when creating firewall rules.

---

## 14. Database Ports

Database ports are very important in application and security work.

### Microsoft SQL Server - TCP 1433

```text
1433/TCP
```

Used by Microsoft SQL Server.

Security note:

Do not expose database ports directly to the Internet unless there is a strong, intentional design.

### Oracle Database - TCP 1521

```text
1521/TCP
```

Common default listener port for Oracle Database.

### MySQL / MariaDB - TCP 3306

```text
3306/TCP
```

Common port for MySQL and MariaDB.

### PostgreSQL - TCP 5432

```text
5432/TCP
```

Common port for PostgreSQL.

### Database security note

Database ports should usually be limited to:

- Application servers
- Admin jump hosts
- Private networks
- VPN networks
- Specific trusted IPs

A database should not be open to everyone.

---

## 15. Voice and Real-Time Communication Ports

### SIP - TCP/UDP 5060

SIP stands for **Session Initiation Protocol**.

```text
5060/TCP
5060/UDP
```

It is used to set up and manage voice or video sessions.

### SIP over TLS - TCP 5061

```text
5061/TCP
```

This is SIP protected with TLS.

### RTP

RTP is commonly used to carry the actual voice or video media.

RTP usually uses dynamic UDP port ranges, depending on the system.

Example:

A PBX may use a range like:

```text
10000-20000/UDP
```

The exact range depends on the vendor and configuration.

### Important voice note

For VoIP, opening only SIP may not be enough.

You often also need to allow the media ports used by RTP.

---

## 16. Common Alternative Ports

Some services use alternative ports.

### HTTP alternate - TCP 8080

```text
8080/TCP
```

Common use:

- Web development
- Proxy servers
- Application servers
- Admin interfaces
- Test services

### HTTPS alternate - TCP 8443

```text
8443/TCP
```

Common use:

- Web admin panels
- Development services
- Application servers
- Alternative TLS services

### Why alternative ports are used

Alternative ports are used when:

- The default port is already used
- A service is for testing
- A service is behind a proxy
- The application server has its own default port
- Admin interfaces are separated from public services

### Security warning

Using a non-standard port does not make a service secure by itself.

Attackers can scan all ports.

Security should come from proper authentication, encryption, patching, network restrictions, and monitoring.

---

## 17. Source Ports vs Destination Ports

When troubleshooting, it is important to understand source and destination ports.

### Destination port

The destination port usually identifies the service being accessed.

Example:

```text
Client -> Server:443
```

The client is trying to reach HTTPS.

### Source port

The source port is often a temporary high-numbered port chosen by the client.

Example:

```text
Client:51544 -> Server:443
```

The client source port is `51544`.

The server destination port is `443`.

### Return traffic

The server replies back to the client source port.

Example:

```text
Server:443 -> Client:51544
```

Stateful firewalls track this connection and allow the return traffic automatically if the original connection was allowed.

---

## 18. Listening Ports

A listening port means a service is waiting for incoming connections.

Example:

If a web server is running, it may listen on:

```text
0.0.0.0:80
0.0.0.0:443
```

This means it accepts traffic on ports 80 and 443.

### Listening on all addresses

```text
0.0.0.0:443
```

means the service listens on all IPv4 interfaces.

For IPv6, you may see:

```text
:::443
```

### Listening on localhost only

```text
127.0.0.1:5432
```

means the service is available only locally on the same machine.

That is common for databases used by local applications.

### Why listening ports matter

If a service is not listening, clients cannot connect even if the firewall allows the port.

Troubleshooting should check both:

- Is the port allowed by the firewall?
- Is the service actually listening?

---

## 19. Firewall Rules and Ports

Firewalls often use ports to allow or block traffic.

A firewall rule may specify:

- Source IP
- Destination IP
- Protocol
- Destination port
- Direction
- Action
- Logging

### Example rule

Allow internal users to access HTTPS on the Internet:

```text
Source: Internal network
Destination: Any
Protocol: TCP
Destination port: 443
Action: Allow
```

### Another example

Allow only an admin subnet to SSH into a server:

```text
Source: 10.10.50.0/24
Destination: 10.10.10.20
Protocol: TCP
Destination port: 22
Action: Allow
```

### Best practice

Do not open ports broadly unless needed.

Prefer specific rules:

- Specific source
- Specific destination
- Specific protocol
- Specific port
- Logging where useful

---

## 20. Port Scanning Basics

Port scanning is the process of checking which ports are open on a host.

It is useful for:

- Troubleshooting
- Inventory
- Security audits
- Finding unexpected services
- Verifying firewall rules

### Common tool

`nmap` is one of the most common tools.

Example:

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

### Be careful

Only scan networks and systems you own or have permission to test.

Port scanning unknown public systems can cause alerts and may violate policy or law.

---

## 21. Common Troubleshooting Examples

### Example 1: Website does not load

Check:

- Does DNS resolve?
- Can you ping the server IP?
- Is TCP 80 or 443 open?
- Is the web server running?
- Is a firewall blocking access?

Useful command:

```bash
curl -I https://example.com
```

### Example 2: SSH does not work

Check:

- Is the server reachable?
- Is SSH listening on port 22?
- Is a firewall blocking TCP 22?
- Is the SSH service running?
- Is the server using a custom SSH port?

Useful command:

```bash
nc -vz server.example.com 22
```

### Example 3: DNS lookup fails

Check:

- Is UDP 53 allowed?
- Is TCP 53 allowed when needed?
- Is the DNS resolver reachable?
- Is the domain configured correctly?
- Is the client using the right resolver?

Useful command:

```bash
dig example.com
```

### Example 4: RDP does not connect

Check:

- Is TCP 3389 open?
- Is Remote Desktop enabled?
- Is Windows Firewall allowing RDP?
- Is Network Level Authentication required?
- Is access limited by VPN or security group?

Useful command:

```powershell
Test-NetConnection server.example.com -Port 3389
```

### Example 5: Application cannot connect to database

Check:

- Is the database listening?
- Is the database bound to the correct interface?
- Is the firewall allowing the database port?
- Is the client allowed by database authentication rules?
- Is the application using the right hostname and port?

Example ports:

```text
1433 SQL Server
3306 MySQL
5432 PostgreSQL
1521 Oracle
```

---

## 22. Useful Commands

### Windows

Show active connections:

```powershell
netstat -ano
```

Show listening ports:

```powershell
netstat -ano | findstr LISTENING
```

Test a TCP port:

```powershell
Test-NetConnection example.com -Port 443
```

Find the process using a port:

```powershell
netstat -ano | findstr :443
tasklist /FI "PID eq 1234"
```

### Linux

Show listening TCP and UDP ports:

```bash
ss -tulpn
```

Show listening TCP ports:

```bash
ss -lntp
```

Test a TCP connection:

```bash
nc -vz example.com 443
```

Use curl for HTTP/HTTPS:

```bash
curl -I https://example.com
```

Scan ports with nmap:

```bash
nmap -p 22,80,443 example.com
```

### macOS

Show listening ports:

```bash
lsof -i -P -n
```

Test a TCP connection:

```bash
nc -vz example.com 443
```

Use curl:

```bash
curl -I https://example.com
```

Use netstat:

```bash
netstat -an
```

---

## 23. Security Notes

Ports are important for security because open ports expose services.

An open port is not automatically bad, but every exposed service should have a reason.

### Good practices

- Close ports that are not needed
- Restrict management ports
- Use firewalls
- Use VPNs for admin access
- Keep services updated
- Use strong authentication
- Use encryption where possible
- Monitor logs
- Scan your own systems regularly
- Avoid exposing databases publicly
- Avoid exposing SMB publicly
- Avoid exposing RDP publicly without strong controls

### Risky ports when exposed publicly

These ports are often dangerous if exposed carelessly:

| Port | Why it is risky |
|---:|---|
| `22` | SSH brute-force attempts |
| `23` | Telnet is clear text |
| `445` | SMB exposure risk |
| `3389` | RDP brute-force and exploitation risk |
| `3306` | MySQL database exposure |
| `5432` | PostgreSQL database exposure |
| `1433` | SQL Server exposure |
| `5900` | VNC exposure risk |

### Important idea

Security is not only about closing ports.

A secure service also needs:

- Good patching
- Good configuration
- Strong authentication
- Least privilege
- Logging
- Monitoring
- Network segmentation

---

## 24. Quick Summary

Ports identify services on a device.

The most important points to remember are:

- An IP address identifies a host
- A port identifies a service on that host
- TCP is connection-oriented
- UDP is connectionless
- Ports range from `0` to `65535`
- `0-1023` are well-known ports
- `1024-49151` are registered ports
- `49152-65535` are dynamic or ephemeral ports
- HTTP uses `80/TCP`
- HTTPS uses `443/TCP`
- SSH uses `22/TCP`
- DNS uses `53/UDP` and `53/TCP`
- DHCP uses `67/68 UDP`
- RDP uses `3389/TCP`
- SMB uses `445/TCP`
- SNMP uses `161/UDP`
- NTP uses `123/UDP`
- BGP uses `179/TCP`
- MySQL uses `3306/TCP`
- PostgreSQL uses `5432/TCP`
- SQL Server uses `1433/TCP`
- Firewall rules often depend on ports
- Open ports should be reviewed and protected

If you understand common ports, you can troubleshoot faster, write better firewall rules, and recognize many network services quickly.
