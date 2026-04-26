# 4. DNS Reference

DNS is one of the most important services in networking. Many network problems look like “the Internet is down,” but the real issue is often DNS.

This guide explains DNS in a practical and detailed way. The goal is to understand what DNS does, how DNS lookups work, what the most common DNS records mean, and how to troubleshoot DNS problems.

---

## Table of Contents

- [1. What DNS Is](#1-what-dns-is)
- [2. Why DNS Matters](#2-why-dns-matters)
- [3. The Basic Idea](#3-the-basic-idea)
- [4. Names vs IP Addresses](#4-names-vs-ip-addresses)
- [5. Important DNS Terms](#5-important-dns-terms)
- [6. DNS Lookup Process](#6-dns-lookup-process)
- [7. Recursive vs Authoritative DNS](#7-recursive-vs-authoritative-dns)
- [8. DNS Caching](#8-dns-caching)
- [9. TTL](#9-ttl)
- [10. Common Public DNS Resolvers](#10-common-public-dns-resolvers)
- [11. DNS Ports](#11-dns-ports)
- [12. Common DNS Record Types](#12-common-dns-record-types)
- [13. A Records](#13-a-records)
- [14. AAAA Records](#14-aaaa-records)
- [15. CNAME Records](#15-cname-records)
- [16. MX Records](#16-mx-records)
- [17. TXT Records](#17-txt-records)
- [18. NS Records](#18-ns-records)
- [19. SOA Records](#19-soa-records)
- [20. PTR Records and Reverse DNS](#20-ptr-records-and-reverse-dns)
- [21. SRV Records](#21-srv-records)
- [22. CAA Records](#22-caa-records)
- [23. DNSSEC Records](#23-dnssec-records)
- [24. DNS Zones](#24-dns-zones)
- [25. Root, TLD, and Authoritative Servers](#25-root-tld-and-authoritative-servers)
- [26. Forward Lookup vs Reverse Lookup](#26-forward-lookup-vs-reverse-lookup)
- [27. Split DNS](#27-split-dns)
- [28. Internal DNS vs Public DNS](#28-internal-dns-vs-public-dns)
- [29. DNS over UDP and TCP](#29-dns-over-udp-and-tcp)
- [30. DNS over TLS and DNS over HTTPS](#30-dns-over-tls-and-dns-over-https)
- [31. DNSSEC in Simple Terms](#31-dnssec-in-simple-terms)
- [32. Common DNS Problems](#32-common-dns-problems)
- [33. Troubleshooting DNS Step by Step](#33-troubleshooting-dns-step-by-step)
- [34. Useful DNS Commands](#34-useful-dns-commands)
- [35. DNS Design Best Practices](#35-dns-design-best-practices)
- [36. Quick Summary](#36-quick-summary)

---

## 1. What DNS Is

DNS stands for **Domain Name System**.

It is the system that translates human-friendly names into IP addresses.

For example, people prefer to type:

```text
example.com
```

But computers need an IP address, such as:

```text
93.184.216.34
```

DNS connects these two worlds.

In simple words:

> DNS is the phonebook of the network.

That comparison is not perfect, but it is useful. You search for a name, and DNS tells you where that name points.

---

## 2. Why DNS Matters

DNS is used almost everywhere.

When you open a website, send an email, join a domain, connect to a cloud service, or use many applications, DNS is usually involved.

Without DNS, users would need to remember IP addresses instead of names. That would be painful and unrealistic.

DNS matters because it makes networks easier to use and easier to manage.

### Examples

Instead of remembering:

```text
142.250.190.14
```

you can use:

```text
google.com
```

Instead of connecting to a mail server by IP address, mail systems can find the correct server through DNS records.

Instead of hardcoding one server IP into every client, an administrator can update DNS and redirect users to a new service.

---

## 3. The Basic Idea

DNS answers questions.

A client asks:

```text
What is the IP address for example.com?
```

DNS replies:

```text
example.com has this IP address.
```

The exact answer depends on the DNS record being requested.

For example:

- `A` record gives an IPv4 address
- `AAAA` record gives an IPv6 address
- `MX` record gives mail servers
- `NS` record gives name servers
- `TXT` record gives text information

DNS is not only for websites. It is used for many services.

---

## 4. Names vs IP Addresses

A domain name is easy for humans to read.

An IP address is what computers actually use to send packets.

### Domain name example

```text
www.example.com
```

### IPv4 address example

```text
192.0.2.10
```

### IPv6 address example

```text
2001:db8::10
```

The DNS system maps names to addresses and other information.

A single domain name can point to:

- One IP address
- Multiple IP addresses
- Another name
- Mail servers
- Verification text
- Service location information

---

## 5. Important DNS Terms

### Domain

A domain is a name in the DNS system.

Example:

```text
example.com
```

### Hostname

A hostname is usually the name of a specific device or service.

Example:

```text
server1.example.com
```

### FQDN

FQDN means **Fully Qualified Domain Name**.

It is the complete DNS name.

Example:

```text
www.example.com
```

Technically, a full DNS name ends with a root dot:

```text
www.example.com.
```

Most people do not write the final dot, but DNS understands it.

### Resolver

A resolver is the DNS server that a client asks for answers.

Your laptop, phone, or server usually sends DNS questions to a resolver.

### Recursive Resolver

A recursive resolver does the work of finding the final answer for the client.

Examples include public resolvers like Google DNS, Cloudflare DNS, Quad9, and ISP DNS resolvers.

### Authoritative Name Server

An authoritative name server stores the official DNS records for a domain.

If a server is authoritative for `example.com`, it is trusted to answer questions about that domain.

### Zone

A zone is a section of DNS namespace managed together.

For example, `example.com` can be a DNS zone.

### Record

A DNS record is one piece of information stored in DNS.

Examples:

- A record
- AAAA record
- MX record
- TXT record

---

## 6. DNS Lookup Process

A DNS lookup usually starts when a user or application tries to access a name.

Example:

```text
www.example.com
```

A simplified lookup looks like this:

```text
Client -> Recursive Resolver -> Root Server -> TLD Server -> Authoritative Server -> Answer
```

### Step-by-step example

1. The client asks its DNS resolver:
   ```text
   What is the IP address for www.example.com?
   ```

2. The resolver checks its cache.

3. If the answer is not cached, the resolver asks a root DNS server.

4. The root server points the resolver to the correct TLD server.

5. The TLD server points the resolver to the authoritative name server for the domain.

6. The authoritative server gives the final answer.

7. The resolver returns the answer to the client.

8. The client connects to the IP address.

### Important note

The client usually does not talk directly to every DNS server in the chain. The recursive resolver does most of the work.

---

## 7. Recursive vs Authoritative DNS

This is one of the most important DNS concepts.

### Recursive DNS Server

A recursive server finds answers for clients.

It accepts a query from a client and then searches for the answer if it does not already know it.

Examples:

- Home router DNS forwarder
- ISP DNS resolver
- Google Public DNS
- Cloudflare DNS
- Quad9 DNS
- Corporate internal resolver

### Authoritative DNS Server

An authoritative server stores the official DNS data for a zone.

It does not search the Internet for answers like a recursive resolver does. It answers for the zone it is responsible for.

### Simple comparison

| Type | Main Job |
|---|---|
| Recursive DNS | Finds answers for clients |
| Authoritative DNS | Stores official records for a domain |

### Example

If you own `example.com`, your authoritative DNS provider stores records like:

```text
www.example.com -> 192.0.2.10
mail.example.com -> 192.0.2.20
```

Your laptop probably asks a recursive resolver to find those records.

---

## 8. DNS Caching

DNS caching stores answers for a period of time.

Caching makes DNS faster and reduces load on DNS servers.

DNS answers can be cached in many places:

- Browser
- Operating system
- Local DNS service
- Home router
- Corporate DNS resolver
- ISP resolver
- Public DNS resolver

### Example

The first lookup for a domain may take longer because the resolver has to find the answer.

The next lookup may be faster because the answer is already cached.

### Good side of caching

- Faster responses
- Less repeated traffic
- Less load on authoritative servers

### Bad side of caching

- Old records may stay around until they expire
- DNS changes may not appear immediately everywhere
- Troubleshooting can be confusing if different systems have different cached answers

---

## 9. TTL

TTL stands for **Time To Live**.

In DNS, TTL controls how long a record may be cached.

### Example

If a DNS record has this TTL:

```text
3600
```

That means the record can be cached for 3600 seconds, or 1 hour.

### Common TTL values

| TTL | Meaning |
|---:|---|
| `60` | 1 minute |
| `300` | 5 minutes |
| `600` | 10 minutes |
| `1800` | 30 minutes |
| `3600` | 1 hour |
| `86400` | 24 hours |

### When to use a low TTL

A low TTL is useful when:

- You are preparing for a migration
- You may need to change an IP quickly
- You are testing a new DNS setup
- You are moving websites or mail systems

### When to use a higher TTL

A higher TTL is useful when:

- Records rarely change
- You want better caching
- You want to reduce DNS query load

### Practical warning

If a record had a high TTL before you changed it, old cached answers may continue to exist until the old TTL expires.

That is why administrators often lower TTL before planned DNS changes.

---

## 10. Common Public DNS Resolvers

Public DNS resolvers are DNS services anyone can use.

| Provider | Primary | Secondary |
|---|---|---|
| Google | `8.8.8.8` | `8.8.4.4` |
| Cloudflare | `1.1.1.1` | `1.0.0.1` |
| Quad9 | `9.9.9.9` | `149.112.112.112` |
| OpenDNS | `208.67.222.222` | `208.67.220.220` |
| AdGuard | `94.140.14.14` | `94.140.15.15` |

### When public resolvers are useful

They are useful for:

- Testing DNS issues
- Comparing answers
- Bypassing a broken local resolver temporarily
- Personal or lab use

### Enterprise warning

In a company network, do not randomly switch to public DNS unless you understand the environment.

Internal services may depend on internal DNS records that public resolvers cannot see.

For example, a company may use internal names like:

```text
fileserver.corp.local
intranet.company.example
```

Public DNS will not know those names.

---

## 11. DNS Ports

DNS commonly uses port `53`.

| Port | Protocol | Use |
|---:|---|---|
| `53` | UDP | Standard DNS queries |
| `53` | TCP | Large responses and zone transfers |
| `853` | TCP | DNS over TLS |
| `443` | TCP | DNS over HTTPS |

### UDP 53

Most normal DNS queries use UDP port 53 because it is simple and fast.

### TCP 53

DNS can use TCP port 53 when:

- The response is too large for UDP
- A zone transfer is being performed
- DNSSEC or large records cause larger replies
- The client retries over TCP

### TCP 853

Port 853 is used for DNS over TLS, often written as DoT.

### TCP 443

Port 443 is used for DNS over HTTPS, often written as DoH.

---

## 12. Common DNS Record Types

Here is a practical list of common DNS records.

| Record | Purpose |
|---|---|
| `A` | Maps a name to an IPv4 address |
| `AAAA` | Maps a name to an IPv6 address |
| `CNAME` | Creates an alias to another name |
| `MX` | Defines mail servers for a domain |
| `NS` | Lists authoritative name servers |
| `TXT` | Stores text data |
| `PTR` | Maps an IP address back to a name |
| `SOA` | Stores zone authority information |
| `SRV` | Defines service location |
| `CAA` | Controls which certificate authorities can issue certificates |
| `DS` | DNSSEC delegation signer |
| `DNSKEY` | DNSSEC public key |
| `RRSIG` | DNSSEC signature |
| `NAPTR` | Rewrite and service discovery record |

You do not need every record type every day, but you should understand the common ones well.

---

## 13. A Records

An `A` record maps a name to an IPv4 address.

### Example

```text
www.example.com.  3600  IN  A  192.0.2.10
```

This means:

```text
www.example.com points to 192.0.2.10
```

### Common use

A records are used for:

- Websites
- Servers
- APIs
- Internal hosts
- Network devices

### Multiple A records

A name can have more than one A record.

Example:

```text
app.example.com.  IN  A  192.0.2.10
app.example.com.  IN  A  192.0.2.11
```

This can be used for simple load distribution, although it is not the same as a full load balancer.

---

## 14. AAAA Records

An `AAAA` record maps a name to an IPv6 address.

The name is pronounced “quad-A.”

### Example

```text
www.example.com.  3600  IN  AAAA  2001:db8::10
```

This means:

```text
www.example.com points to IPv6 address 2001:db8::10
```

### Common use

AAAA records are used when a service is reachable over IPv6.

A domain can have both:

- A record for IPv4
- AAAA record for IPv6

### Example

```text
www.example.com.  IN  A     192.0.2.10
www.example.com.  IN  AAAA  2001:db8::10
```

A client with IPv6 connectivity may prefer the IPv6 address depending on system behavior.

---

## 15. CNAME Records

A `CNAME` record creates an alias.

It points one name to another name.

### Example

```text
blog.example.com.  IN  CNAME  example-hosting-provider.net.
```

This means:

```text
blog.example.com is an alias for example-hosting-provider.net
```

The DNS resolver then looks up the target name.

### Common use

CNAME records are often used for:

- Hosted websites
- CDN services
- SaaS services
- Cloud services
- Friendly aliases

### Important rule

A CNAME points to a name, not directly to an IP address.

Wrong idea:

```text
blog.example.com CNAME 192.0.2.10
```

Correct idea:

```text
blog.example.com CNAME server.example.net
```

### CNAME at the root domain

Using CNAME at the zone root can be problematic in traditional DNS because the root domain usually also needs records like SOA and NS.

For example:

```text
example.com
```

Many DNS providers offer special features like ALIAS, ANAME, or flattened CNAME to solve this. These are provider features, not normal DNS record types in the same basic way.

---

## 16. MX Records

An `MX` record tells other mail systems where to deliver email for a domain.

MX stands for **Mail Exchanger**.

### Example

```text
example.com.  3600  IN  MX  10 mail.example.com.
```

This means:

```text
For email to example.com, send mail to mail.example.com.
```

### MX priority

MX records have priority numbers.

Lower number = higher priority.

Example:

```text
example.com.  IN  MX  10 mail1.example.com.
example.com.  IN  MX  20 mail2.example.com.
```

Mail systems try `mail1` first. If it fails, they may try `mail2`.

### Important point

The MX target should normally resolve to an IP address through A or AAAA records.

Example:

```text
mail.example.com.  IN  A  192.0.2.25
```

---

## 17. TXT Records

A `TXT` record stores text data.

TXT records are very common for verification and email security.

### Common uses

TXT records are used for:

- SPF
- DKIM
- DMARC
- Domain ownership verification
- Service verification
- Security policies

### SPF example

SPF helps define which mail servers are allowed to send mail for a domain.

```text
example.com.  IN  TXT  "v=spf1 include:_spf.example.net -all"
```

### Domain verification example

A service may ask you to add something like:

```text
example.com.  IN  TXT  "service-verification=abc123"
```

This proves that you control the domain.

### Practical warning

TXT records are flexible, but they can become messy if many services require verification records. Keep them documented.

---

## 18. NS Records

An `NS` record lists the authoritative name servers for a domain or zone.

NS stands for **Name Server**.

### Example

```text
example.com.  IN  NS  ns1.example.net.
example.com.  IN  NS  ns2.example.net.
```

This means the authoritative servers for `example.com` are:

```text
ns1.example.net
ns2.example.net
```

### Why NS records matter

They tell the DNS system where to find official answers for a zone.

If NS records are wrong, the domain may stop resolving correctly.

### Registrar vs DNS provider

For public domains, name server information is usually configured at the domain registrar.

Your DNS provider hosts the actual zone records.

Sometimes the registrar and DNS provider are the same company, but not always.

---

## 19. SOA Records

SOA stands for **Start of Authority**.

An SOA record contains important information about a DNS zone.

### Common SOA fields

An SOA record usually includes:

- Primary name server
- Responsible contact
- Serial number
- Refresh interval
- Retry interval
- Expire time
- Negative caching TTL

### Example

```text
example.com. IN SOA ns1.example.com. admin.example.com. (
  2026010101 ; serial
  3600       ; refresh
  900        ; retry
  1209600    ; expire
  300        ; minimum
)
```

### Serial number

The serial number changes when the zone changes.

Secondary DNS servers use it to know if they need to update their copy of the zone.

A common serial format is:

```text
YYYYMMDDNN
```

Example:

```text
2026010101
```

This can mean the first change made on January 1, 2026.

---

## 20. PTR Records and Reverse DNS

A `PTR` record maps an IP address back to a name.

This is called **reverse DNS**.

### Forward DNS

Forward DNS maps name to IP.

Example:

```text
www.example.com -> 192.0.2.10
```

### Reverse DNS

Reverse DNS maps IP to name.

Example:

```text
192.0.2.10 -> www.example.com
```

### IPv4 reverse lookup

IPv4 reverse DNS uses the `in-addr.arpa` zone.

For IP:

```text
192.0.2.10
```

The reverse name is:

```text
10.2.0.192.in-addr.arpa
```

### IPv6 reverse lookup

IPv6 reverse DNS uses the `ip6.arpa` zone.

IPv6 reverse DNS is longer because IPv6 addresses are much longer.

### Common use

PTR records are commonly used for:

- Mail server reputation
- Logging
- Troubleshooting
- Some security checks

### Mail server note

For mail servers, reverse DNS should usually match or make sense with the mail server hostname.

Bad or missing reverse DNS can cause mail delivery problems.

---

## 21. SRV Records

An `SRV` record tells clients where to find a specific service.

SRV records include:

- Service name
- Protocol
- Priority
- Weight
- Port
- Target host

### Example

```text
_service._tcp.example.com.  IN  SRV  10 5 5060 sipserver.example.com.
```

This record says:

- Service: `_service`
- Protocol: `_tcp`
- Priority: `10`
- Weight: `5`
- Port: `5060`
- Target: `sipserver.example.com`

### Common uses

SRV records are used by systems like:

- SIP
- LDAP
- Kerberos
- Microsoft Active Directory
- Some service discovery systems

### Priority and weight

Priority works like MX priority:

- Lower priority number is preferred

Weight is used to distribute traffic among records with the same priority.

---

## 22. CAA Records

A `CAA` record controls which certificate authorities are allowed to issue TLS certificates for a domain.

CAA stands for **Certification Authority Authorization**.

### Example

```text
example.com.  IN  CAA  0 issue "letsencrypt.org"
```

This means Let's Encrypt is allowed to issue certificates for the domain.

### Why CAA matters

CAA can help reduce the risk of certificates being issued by an unauthorized certificate authority.

### Common CAA tags

| Tag | Meaning |
|---|---|
| `issue` | Allows a CA to issue normal certificates |
| `issuewild` | Allows a CA to issue wildcard certificates |
| `iodef` | Gives a reporting contact |

### Example with reporting

```text
example.com.  IN  CAA  0 iodef "mailto:security@example.com"
```

---

## 23. DNSSEC Records

DNSSEC adds cryptographic validation to DNS.

It helps prove that DNS answers have not been tampered with.

Common DNSSEC-related records include:

| Record | Purpose |
|---|---|
| `DNSKEY` | Stores a public key |
| `RRSIG` | Stores a signature for DNS data |
| `DS` | Connects a child zone to a parent zone |
| `NSEC` | Proves a name does not exist |
| `NSEC3` | Similar to NSEC with hashed names |

DNSSEC does not encrypt DNS traffic. It validates authenticity and integrity.

That means DNSSEC helps answer:

```text
Is this DNS answer authentic?
```

It does not mainly answer:

```text
Can someone see my DNS query?
```

For privacy, technologies like DoT or DoH are used.

---

## 24. DNS Zones

A DNS zone is a part of the DNS namespace that is managed together.

Example:

```text
example.com
```

The zone may include records like:

```text
example.com
www.example.com
mail.example.com
api.example.com
```

### Zone file

A zone file is a text-based representation of DNS records.

Not every DNS provider exposes raw zone files, but the concept is still useful.

### Example zone records

```text
example.com.       IN  SOA   ns1.example.com. admin.example.com. ...
example.com.       IN  NS    ns1.example.com.
example.com.       IN  NS    ns2.example.com.
www.example.com.   IN  A     192.0.2.10
mail.example.com.  IN  A     192.0.2.25
example.com.       IN  MX    10 mail.example.com.
```

### Delegation

A parent zone can delegate a child zone to different name servers.

Example:

```text
sub.example.com
```

could be managed separately from:

```text
example.com
```

This is useful in large organizations.

---

## 25. Root, TLD, and Authoritative Servers

DNS is hierarchical.

A simplified hierarchy looks like this:

```text
Root
└── com
    └── example.com
        └── www.example.com
```

### Root servers

Root servers know where to find TLD servers.

They do not usually store every domain record. Instead, they point resolvers in the right direction.

### TLD servers

TLD means **Top-Level Domain**.

Examples:

- `.com`
- `.org`
- `.net`
- `.edu`
- country-code TLDs like `.uk`, `.sa`, `.de`

TLD servers know where to find authoritative name servers for domains under that TLD.

### Authoritative servers

Authoritative servers store the actual DNS records for a domain.

For example, the authoritative servers for `example.com` answer questions about `www.example.com`, `mail.example.com`, and other records in that zone.

---

## 26. Forward Lookup vs Reverse Lookup

### Forward lookup

A forward lookup asks:

```text
What IP address belongs to this name?
```

Example:

```text
www.example.com -> 192.0.2.10
```

Common records:

- A
- AAAA

### Reverse lookup

A reverse lookup asks:

```text
What name belongs to this IP address?
```

Example:

```text
192.0.2.10 -> www.example.com
```

Common record:

- PTR

### Why reverse lookup is useful

Reverse DNS is useful for:

- Mail servers
- Logs
- Security analysis
- Troubleshooting
- Identifying systems

---

## 27. Split DNS

Split DNS means different users get different DNS answers depending on where they are.

For example:

- Internal users get internal IP addresses
- External users get public IP addresses

### Example

Internal answer:

```text
app.example.com -> 10.10.10.50
```

External answer:

```text
app.example.com -> 203.0.113.50
```

### Why split DNS is used

Split DNS is useful when:

- Internal systems use private IPs
- External users need public IPs
- The same name should work inside and outside the network
- VPN users need internal answers

### Common problem

If a client uses the wrong DNS resolver, it may get the wrong answer.

For example, a VPN user may fail to reach internal systems if their DNS queries go to public DNS instead of corporate DNS.

---

## 28. Internal DNS vs Public DNS

### Internal DNS

Internal DNS is used inside a private network.

It may contain records for:

- File servers
- Internal applications
- Domain controllers
- Printers
- Private cloud services
- Management interfaces

Example:

```text
dc1.corp.example.com
fileserver.corp.example.com
intranet.example.com
```

### Public DNS

Public DNS is visible on the Internet.

It contains records for public services like:

- Websites
- Public mail servers
- Public APIs
- Public verification records

Example:

```text
www.example.com
mail.example.com
api.example.com
```

### Important rule

Do not expose internal-only DNS records publicly unless you have a reason and understand the risk.

Internal names can reveal infrastructure details.

---

## 29. DNS over UDP and TCP

DNS normally uses UDP because it is lightweight.

But DNS also supports TCP.

### UDP DNS

UDP DNS is used for most normal queries.

Advantages:

- Fast
- Simple
- Low overhead

### TCP DNS

TCP DNS is used when needed.

Common reasons:

- Large DNS responses
- DNSSEC responses
- Zone transfers
- Retry after truncated UDP response

### Truncated responses

If a UDP DNS response is too large, the server may set the truncated flag. The client can then retry using TCP.

### Firewall note

Do not allow only UDP 53 and block all TCP 53 without understanding the impact. Some DNS behavior may break.

---

## 30. DNS over TLS and DNS over HTTPS

Traditional DNS is usually sent in clear text.

That means someone on the network path may be able to see DNS queries.

Two common privacy-focused options are:

- DNS over TLS
- DNS over HTTPS

### DNS over TLS

DNS over TLS is often called **DoT**.

It commonly uses:

```text
TCP 853
```

DoT encrypts DNS traffic using TLS.

### DNS over HTTPS

DNS over HTTPS is often called **DoH**.

It uses HTTPS, usually over:

```text
TCP 443
```

DoH sends DNS queries over HTTPS.

### Important difference

DoT and DoH can improve privacy from local network observers, but they can also make enterprise DNS visibility and filtering more difficult.

In corporate networks, DNS privacy settings should be planned carefully.

---

## 31. DNSSEC in Simple Terms

DNSSEC helps verify DNS answers.

Without DNSSEC, a client usually trusts that the DNS answer it receives is correct.

With DNSSEC, DNS data can be cryptographically signed. A validating resolver can check the signature.

### DNSSEC helps protect against

- DNS spoofing
- DNS cache poisoning
- Some forms of DNS tampering

### DNSSEC does not do everything

DNSSEC does not:

- Encrypt DNS queries
- Hide which domains are being queried
- Replace TLS certificates
- Stop all phishing
- Fix bad DNS records

### Simple idea

DNSSEC is about trust and integrity.

DoT and DoH are about privacy in transport.

They solve different problems.

---

## 32. Common DNS Problems

DNS problems can appear in many ways.

### Problem 1: Name does not resolve

Example:

```text
server not found
```

Possible causes:

- Missing DNS record
- Wrong resolver
- DNS server down
- Typo in name
- Domain expired
- Broken delegation

### Problem 2: DNS resolves to the wrong IP

Possible causes:

- Old cached record
- Wrong record configured
- Split DNS issue
- Client using public DNS instead of internal DNS
- Load balancing or CDN behavior

### Problem 3: Works for some users but not others

Possible causes:

- DNS caching
- Different resolvers
- GeoDNS
- Split DNS
- VPN DNS issue
- Propagation delay after change

### Problem 4: Website works by IP but not by name

This usually points to DNS.

If this works:

```text
ping 192.0.2.10
```

but this does not:

```text
ping example.com
```

then DNS resolution may be failing.

### Problem 5: Email delivery fails

Possible DNS causes:

- Missing MX record
- Wrong MX target
- Missing A or AAAA record for mail host
- Bad SPF
- Bad DKIM
- Bad DMARC
- Missing reverse DNS
- Broken DNSSEC

---

## 33. Troubleshooting DNS Step by Step

Here is a simple troubleshooting flow.

### Step 1: Check basic network connectivity

Before blaming DNS, check if the device has network access.

Try pinging an IP address:

```bash
ping 8.8.8.8
```

If IP connectivity does not work, the problem may not be DNS.

### Step 2: Check the configured DNS resolver

On Windows:

```powershell
ipconfig /all
```

On Linux:

```bash
resolvectl status
cat /etc/resolv.conf
```

On macOS:

```bash
scutil --dns
```

Check which DNS servers the client is using.

### Step 3: Query the default resolver

Use the normal resolver first.

```bash
nslookup example.com
```

or:

```bash
dig example.com
```

### Step 4: Query a specific resolver

Compare the answer from another resolver.

```bash
nslookup example.com 8.8.8.8
```

or:

```bash
dig @1.1.1.1 example.com
```

If two resolvers give different answers, you may be seeing caching, split DNS, or propagation differences.

### Step 5: Query the exact record type

Do not only ask for the default answer.

Check specific records:

```bash
dig example.com A
dig example.com AAAA
dig example.com MX
dig example.com TXT
dig example.com NS
```

### Step 6: Check authoritative servers

Ask the authoritative server directly.

```bash
dig NS example.com
dig @ns1.example.com www.example.com A
```

This helps separate authoritative DNS problems from resolver caching problems.

### Step 7: Check TTL and caching

Look at the TTL in DNS answers.

If the answer has a long TTL, old cached data may remain for a while.

### Step 8: Check DNSSEC if enabled

If DNSSEC is enabled and validation fails, the domain may fail to resolve for validating resolvers.

Useful command:

```bash
dig example.com DNSKEY
dig example.com +dnssec
```

### Step 9: Check firewall rules

Make sure DNS traffic is allowed.

Common ports:

```text
UDP 53
TCP 53
TCP 853 for DoT
TCP 443 for DoH
```

### Step 10: Check the application

Sometimes DNS is fine, but the application has its own cache or configuration.

Examples:

- Browser DNS cache
- Application-level cache
- Container DNS settings
- Kubernetes DNS behavior
- Proxy settings

---

## 34. Useful DNS Commands

### Windows

```powershell
nslookup example.com
nslookup -type=mx example.com
nslookup -type=txt example.com
ipconfig /displaydns
ipconfig /flushdns
Resolve-DnsName example.com
Resolve-DnsName example.com -Type MX
```

### Linux

```bash
dig example.com
dig example.com A
dig example.com AAAA
dig example.com MX
dig example.com TXT
dig example.com NS
dig @8.8.8.8 example.com
host example.com
nslookup example.com
resolvectl status
```

### macOS

```bash
dig example.com
host example.com
nslookup example.com
scutil --dns
dscacheutil -flushcache
```

### Useful dig examples

#### Query an A record

```bash
dig example.com A
```

#### Query an AAAA record

```bash
dig example.com AAAA
```

#### Query mail records

```bash
dig example.com MX
```

#### Query TXT records

```bash
dig example.com TXT
```

#### Query a specific DNS server

```bash
dig @1.1.1.1 example.com
```

#### Show the trace path

```bash
dig +trace example.com
```

#### Short answer only

```bash
dig +short example.com
```

#### Reverse DNS lookup

```bash
dig -x 192.0.2.10
```

---

## 35. DNS Design Best Practices

### Use at least two authoritative name servers

A public domain should normally have multiple authoritative name servers.

This improves availability.

### Keep records documented

Document important records such as:

- A records
- AAAA records
- MX records
- TXT records for email security
- CNAME records for third-party services
- DNSSEC settings

### Use reasonable TTL values

Do not set every TTL extremely low forever.

Low TTLs are useful during changes, but normal stable records can usually have higher TTLs.

### Lower TTL before planned migrations

Before changing important records, lower TTL ahead of time.

Example:

- Normal TTL: 3600 or 86400 seconds
- Migration TTL: 300 seconds

After the migration is stable, raise it again if appropriate.

### Be careful with public DNS records

Avoid exposing unnecessary internal names.

Bad example:

```text
firewall-mgmt.internal.example.com
backup-server-01.example.com
```

Public DNS records can reveal useful information to attackers.

### Keep email DNS records correct

For email, check:

- MX
- SPF
- DKIM
- DMARC
- PTR / reverse DNS
- TLS-related records if used

Email delivery often depends heavily on DNS correctness.

### Monitor DNS

DNS should be monitored like any other critical service.

Monitor:

- Resolver health
- Authoritative DNS availability
- Record correctness
- DNS response time
- DNSSEC validity if enabled

### Avoid random DNS changes

DNS changes can affect users, mail, certificates, applications, and security systems.

Change DNS carefully and document what changed.

---

## 36. Quick Summary

DNS translates names into IP addresses and other useful information.

The most important points to remember are:

- DNS stands for **Domain Name System**
- DNS maps names to IP addresses and other records
- Clients usually ask a recursive resolver
- Authoritative servers store official records for a domain
- DNS caching makes lookups faster
- TTL controls how long DNS answers are cached
- Normal DNS uses UDP port `53`
- DNS can also use TCP port `53`
- DoT uses TCP `853`
- DoH usually uses TCP `443`
- `A` records point to IPv4 addresses
- `AAAA` records point to IPv6 addresses
- `CNAME` records create aliases
- `MX` records define mail servers
- `TXT` records store text and verification data
- `NS` records list name servers
- `PTR` records are used for reverse DNS
- DNSSEC validates DNS data but does not encrypt DNS traffic
- Many “network problems” are actually DNS problems

If you can understand DNS lookup flow, common record types, TTL, caching, and troubleshooting commands, you will be able to solve a large number of real-world network issues.
