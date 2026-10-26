# 🌐 DNS Server — Concept Inside

A **DNS (Domain Name System) server** works like the **contact list in your phone**. You remember a person's name, and your phone finds their number. Similarly, you type a website name, and DNS helps your computer find the IP address needed to reach that website.

For example, when you enter `www.google.com`, your computer needs to discover the IP address associated with that domain.

Let's understand how it works, step by step, with real-world examples.

## 1. What problem does DNS solve?

Computers communicate over networks using IP addresses, but humans prefer domain names.

| Domain name | Example IP address |
|---|---|
| `www.google.com` | `142.250.183.68`* |
| `www.microsoft.com` | An IP address returned by DNS |
| `www.amazon.com` | An IP address returned by DNS |

*The Google IP above is an example; actual addresses can vary by location and time.*

Without DNS, you would often need to remember IP addresses instead of website names.

**DNS translates domain names into the records computers need to locate services.**

---

## 2. How does a DNS server work?

Imagine you open your browser and enter `www.google.com`.



Here is the process:

**Step 1: You enter the domain name**

Your browser requests `www.google.com`.

**Step 2: Your computer checks its cache**

Your operating system or browser may already have the DNS result stored.

- If a valid result exists, it can use that result.
- Otherwise, it sends a DNS query to a configured DNS resolver.

**Step 3: The recursive DNS resolver checks its cache**

Your resolver might be provided by your ISP, your organization, or a public DNS service.

If it has a valid cached answer, it returns it immediately.

Otherwise, it finds the answer by querying other DNS servers.

**Step 4: The root DNS server directs the resolver**

The root server helps identify which DNS servers handle the `.com` domain.

It does not normally provide Google's final IP address.

**Step 5: The TLD DNS server responds**

The `.com` top-level domain server directs the resolver to the authoritative DNS servers for `google.com`.

**Step 6: The authoritative DNS server returns the record**

The authoritative DNS server provides the requested DNS record, such as an A record containing an IPv4 address.

**Step 7: The resolver returns the answer**

The recursive resolver sends the result back to your computer and may cache it.

**Step 8: Your browser connects to the destination**

Your browser uses the returned IP address to establish a connection to the website.

HTTPS connections also involve TLS security checks.

### Remember this flow

```text
User enters www.google.com
          |
          v
Browser / OS DNS Cache
          |
          v
Recursive DNS Resolver
          |
          v
Root DNS Server
          |
          v
.com TLD DNS Server
          |
          v
Authoritative DNS Server
          |
          v
IP address returned
          |
          v
Browser connects to website
```

**Important:** The root, TLD, and authoritative servers are not necessarily contacted for every request. Caching often eliminates some or all of these queries.

---

## 3. Types of DNS servers

There are four important roles to understand.

| DNS server | Main responsibility | Simple analogy |
|---|---|---|
| Recursive resolver | Finds the answer on behalf of the client | A person who looks up a number for you |
| Root DNS server | Directs queries to the appropriate TLD servers | Directs you to the correct department |
| TLD DNS server | Directs queries to the domain's authoritative DNS servers | Directs you to the correct office |
| Authoritative DNS server | Stores or serves the definitive DNS records for a domain | The official contact directory |

A recursive resolver is often called a *DNS server* in everyday conversation, but it has a different role from an authoritative DNS server.

### Example: Public DNS resolvers

Two popular services are:

- Google Public DNS — `8.8.8.8`
- Cloudflare DNS — `1.1.1.1`

These are examples of recursive DNS resolvers. They help clients find DNS records.

---

## 4. Important DNS record types

DNS does more than translate a name into an IP address.

| Record | Purpose | Example |
|---|---|---|
| **A** | Maps a name to an IPv4 address | `app.example.com → 192.0.2.10` |
| **AAAA** | Maps a name to an IPv6 address | `app.example.com → IPv6 address` |
| **CNAME** | Makes one name an alias of another name | `www.example.com → example.com` |
| **MX** | Identifies mail servers for a domain | Email delivery |
| **TXT** | Stores text information used for verification and email security | SPF verification |
| **NS** | Identifies authoritative name servers for a domain | DNS delegation |
| **PTR** | Supports reverse DNS lookup | IP address → domain name |
| **SRV** | Identifies servers providing particular services | Service discovery |

### A vs. CNAME

Suppose you have this DNS configuration:

```text
app.example.com      A       192.0.2.10
www.example.com      CNAME   app.example.com
```

When a client looks up `www.example.com`, DNS follows the CNAME to `app.example.com` and ultimately obtains the IP address `192.0.2.10`.

The IP address above is reserved for documentation and is not a real public website address.

---

## 5. What happens when DNS fails?

Consider this scenario:

You have an internet connection, but `www.example.com` does not open.

Several things could be wrong:

- The DNS resolver is unavailable.
- The DNS record is incorrect or missing.
- A DNS query is being blocked by a firewall.
- The DNS cache contains an outdated answer.
- The domain's authoritative DNS service is unavailable.

The problem may be DNS even when your network connection itself is working.

### Useful DNS troubleshooting commands

**Windows**

Open Command Prompt.

Check DNS resolution:

```cmd
nslookup google.com
```

Check which DNS server your computer is using:

```cmd
ipconfig /all
```

Clear the local DNS cache:

```cmd
ipconfig /flushdns
```

**Linux**

Check name resolution:

```bash
dig google.com
```

Or:

```bash
nslookup google.com
```

These commands help you investigate DNS resolution, although the exact commands available depend on your operating system and installed tools.

---

## 6. DNS in Azure and DevOps

Since DNS is also important in cloud and DevOps, let's connect it to a practical Azure example.

Imagine you deploy an application in Azure:

```text
User
  |
  v
app.company.com
  |
  v
DNS resolution
  |
  v
Application Gateway / Load Balancer IP
  |
  v
Backend application
```

You might create an A record:

```text
app.company.com → Application Gateway public IP
```

When users open `app.company.com`, DNS resolves the name to the gateway's IP address. The gateway then routes the request to the appropriate backend.

**DNS does not itself route the HTTP request to your application.** It resolves the name; the network and application infrastructure handle the subsequent connection and routing.

### What about Azure Private DNS?

Suppose you deploy a virtual machine inside an Azure Virtual Network (VNet).

You may want an internal name such as:

```text
vm01.internal.company.com → 10.0.1.4
```

An Azure Private DNS zone can provide name resolution for resources within linked VNets.

This is useful for:
- Internal applications.
- Private endpoints.
- Communication between VMs.
- Internal service discovery.

For example, when you configure an Azure Storage account with a private endpoint, DNS must resolve the storage hostname to the appropriate private IP address for the intended private-access path.

If DNS is misconfigured, the storage hostname might resolve to a public address instead of the expected private address.

---

## 7. DNS over UDP and TCP

DNS traditionally uses **UDP port 53** for most standard queries because UDP has low overhead.

DNS also uses **TCP port 53**, including for responses that require TCP and for certain DNS operations, such as zone transfers.

| Protocol | Port | Typical use |
|---|---:|---|
| UDP | 53 | Most conventional DNS queries |
| TCP | 53 | Larger responses, retries, and zone transfers |

Modern DNS technologies also include DNS over HTTPS (DoH) and DNS over TLS (DoT), which protect DNS traffic between supported clients and resolvers.

---

## 8. Interview questions to test your understanding

**Q1. What is DNS?**

DNS is a distributed naming system that translates domain names into DNS records, including IP addresses.

**Q2. What is the difference between a recursive resolver and an authoritative DNS server?**

A recursive resolver finds answers on behalf of clients. An authoritative DNS server provides the definitive records for the DNS zones it serves.

**Q3. Does a root DNS server know the IP address of every website?**

No. Root servers direct queries to the appropriate top-level domain servers.

**Q4. What is DNS caching?**

DNS caching temporarily stores DNS answers to reduce lookup time and network traffic. Cached records are generally retained according to their TTL (Time to Live).

**Q5. What is the difference between public DNS and private DNS?**

Public DNS names and records can be resolved through public DNS infrastructure, depending on their configuration. Private DNS is used for internal name resolution and restricted environments.

**Q6. What happens if DNS works but the website is down?**

DNS may return the correct IP address, but the application, server, firewall, or network path may still be unavailable.

---

### The most important concept to remember

**DNS is the internet's naming system.** It helps your computer discover where a service can be reached, but it does not itself guarantee that the service is available.

For DevOps, focus on these five topics first:

1. Recursive vs. authoritative DNS.
2. A, AAAA, CNAME, and MX records.
3. DNS caching and TTL.
4. DNS troubleshooting using `nslookup` and `dig`.
5. Public DNS vs. private DNS, especially Azure Private DNS and private endpoints.


## What is `nslookup -type=NS`?

`nslookup -type=NS` is a DNS command used to **find the Name Servers (NS) responsible for a domain**.

In simple terms, it tells you which DNS servers manage the DNS records for a domain.

### 1. Syntax

```bash
nslookup -type=NS example.com
```

For your example, you can run:

```bash
nslookup -type=NS devopsinsiders.com
```

### 2. Example output

Suppose the output is:

```text
devopsinsiders.com    nameserver = elinore.ns.cloudflare.com
devopsinsiders.com    nameserver = another.ns.cloudflare.com
```

This tells you that the domain uses these name servers.

- `devopsinsiders.com` — the domain you queried.
- `elinore.ns.cloudflare.com` — one of its authoritative name servers.
- The other name server — typically provides redundancy.

*The output above is illustrative; actual results depend on the domain's current DNS configuration.*

### 3. How does this relate to your DNS diagram?

Your diagram shows:

`elinore.ns.cloudflare.com`

When you execute:

```bash
nslookup -type=NS devopsinsiders.com
```

You are asking DNS:

**"Which name servers are authoritative for `devopsinsiders.com`?"**

The answer identifies the name servers, not necessarily the IP address of `netflix.devopsinsiders.com`.

### 4. Difference between `A`, `NS`, and `MX`

| Command | What it finds | Example purpose |
|---|---|---|
| `nslookup -type=A example.com` | IPv4 address | Find a website's IPv4 address |
| `nslookup -type=AAAA example.com` | IPv6 address | Find a website's IPv6 address |
| `nslookup -type=NS example.com` | Name servers | Find which DNS servers manage the domain |
| `nslookup -type=MX example.com` | Mail servers | Find where domain email is handled |
| `nslookup -type=TXT example.com` | TXT records | Check verification or SPF records |

### 5. DevOps interview tip

**Question:** Why do we use `nslookup -type=NS`?

**Answer:** We use it to identify the name servers responsible for a domain's DNS zone. It is useful for verifying DNS delegation, checking whether a domain uses Cloudflare or another DNS provider, and troubleshooting DNS configuration issues.

**Remember:**
- `A` record → Where does this hostname point?
- `NS` record → Which name servers manage this domain?
- `MX` record → Which mail servers handle this domain's email?
