---
title: "DNS in Detail - TryHackMe"
date: 2026-09-29 09:00:00 +0300
categories: [Lab-Challenges]
tags: [DNS, TryHackMe, Networking, Cybersecurity, Domain-Names, nslookup]
description: "Completed the TryHackMe DNS in Detail room and explored DNS hierarchy, DNS records, DNS resolution, and practical DNS enumeration using nslookup."
---

# DNS in Detail - TryHackMe

## Challenge Overview

This lab was completed through the TryHackMe **DNS in Detail** room. The module introduced the Domain Name System (DNS), how internet devices locate resources, and how DNS queries travel across the internet.

The room combined theory with practical exercises and demonstrated the role DNS plays in modern networking and cybersecurity.

### Room Information

| Item | Value |
|--------|--------|
| Platform | TryHackMe |
| Room | DNS in Detail |
| Difficulty | Easy |
| Duration | Approximately 45 Minutes |
| Topics | DNS Fundamentals, Record Types, DNS Resolution, DNS Enumeration |

---

# Problem Statement

Computers communicate using numerical IP addresses, but humans find domain names much easier to remember.

For example:

```text
104.26.10.229
```

is much harder to remember than:

```text
tryhackme.com
```

DNS solves this challenge by translating human-readable domain names into machine-readable IP addresses.

This room explored:

- How DNS works
- Domain hierarchy
- DNS record types
- DNS resolution
- DNS request flow
- Practical DNS interrogation techniques

---

# Objectives

- Understand the purpose of DNS.
- Learn how domain names are structured.
- Identify common DNS record types.
- Understand the DNS lookup process.
- Query DNS records using command-line tools.
- Explore the security implications of DNS.

---

# Tools Used

### Learning Platform

- TryHackMe

### Utilities

- nslookup
- Browser-based terminal

---

# Task 1: What is DNS?

DNS stands for:

```text
Domain Name System
```

DNS functions as the internet's directory service by converting domain names into IP addresses.

Without DNS, users would have to remember numerical IP addresses for every website they visit.

### Answer

```text
Domain Name System
```

---

# Task 2: Domain Hierarchy

This task explored the structure of internet domain names.

Example:

```text
www.tryhackme.com
```

can be broken into:

```text
.com         → Top-Level Domain (TLD)

tryhackme    → Second-Level Domain

www          → Subdomain
```

### Questions and Answers

#### Maximum Length of a Subdomain

```text
63
```

#### Invalid Character for a Subdomain

```text
_
```

#### Maximum Length of a Domain Name

```text
253
```

#### What Type of TLD Is .co.uk?

```text
ccTLD
```

(Country Code Top-Level Domain)

### Key Learning

Subdomains may contain:

- Letters (a-z)
- Numbers (0-9)
- Hyphens (-)

Underscores are not permitted.

---

# Task 3: DNS Record Types

DNS uses different record types depending on the service being provided.

---

## A Record

Maps:

```text
Hostname → IPv4 Address
```

Example:

```text
example.com
```

↓

```text
192.168.1.1
```

---

## AAAA Record

Maps:

```text
Hostname → IPv6 Address
```

---

## CNAME Record

Creates an alias for another domain.

Example:

```text
shop.example.com
```

↓

```text
shops.myshopify.com
```

---

## MX Record

Specifies the mail servers responsible for receiving email.

---

## TXT Record

Stores text information associated with a domain.

Common uses include:

- SPF Records
- DKIM Records
- DMARC Records
- Domain Ownership Verification

---

### Questions and Answers

#### Which Record Type Directs Email?

```text
MX
```

#### Which Record Type Handles IPv6 Addresses?

```text
AAAA
```

---

# Task 4: Making a DNS Request

This task followed the complete lifecycle of a DNS query.

### DNS Resolution Process

#### Step 1

The computer checks:

```text
Local DNS Cache
```

#### Step 2

If no answer exists, it contacts a:

```text
Recursive DNS Server
```

#### Step 3

The recursive server may query:

```text
Root DNS Server
```

#### Step 4

The root server directs the query to the correct:

```text
Top-Level Domain Server
```

#### Step 5

The TLD server identifies the:

```text
Authoritative DNS Server
```

#### Step 6

The authoritative server returns the requested DNS record.

---

### Questions and Answers

#### Which Field Defines Cache Duration?

```text
TTL
```

(Time To Live)

---

#### Which DNS Server Is Usually Provided by an ISP?

```text
Recursive
```

---

#### Which DNS Server Holds the Official DNS Records?

```text
Authoritative
```

---

# Task 5: Practical DNS Enumeration

The final task used:

```bash
nslookup
```

to query DNS records for:

```text
website.thm
```

---

## CNAME Record Lookup

Command:

```bash
nslookup --type=CNAME shop.website.thm
```

Result:

```text
shops.myshopify.com
```

---

## TXT Record Lookup

Result:

```text
THM{7012BBA60997F35A9516C2E16D2944FF}
```

---

## MX Record Lookup

Priority Value:

```text
30
```

---

## A Record Lookup

Result:

```text
10.10.10.10
```

---

# Practical Skills Gained

This lab provided practical experience in:

- DNS Interrogation
- Record Enumeration
- Domain Analysis
- Troubleshooting DNS Issues
- Security Reconnaissance

During the practical activity I learned how to:

- Query A records
- Query MX records
- Query TXT records
- Query CNAME records
- Interpret nslookup output
- Gather information about internet-facing services

---

# Security Relevance

DNS plays a critical role in:

- Network Troubleshooting
- Security Assessments
- Threat Intelligence
- Reconnaissance
- Incident Response

Poorly configured DNS records may expose:

- Internal infrastructure
- Mail servers
- Third-party services
- Verification tokens
- Misconfigured applications

DNS-based security technologies such as SPF, DKIM, and DMARC help protect organizations against email spoofing and phishing attacks.

---

# Evidence of Completion

The completion of this room can be verified using the official TryHackMe share link below:

🔗 **Completed Room:**

[DNS in Detail - TryHackMe](https://tryhackme.com/room/dnsindetail?utm_campaign=social_share&utm_medium=social&utm_content=share-completed-room&utm_source=copy&sharerId=6a5e7e8f4a7687db5607ff1b)

### Completion Summary

- Platform: TryHackMe
- Room: DNS in Detail
- Difficulty: Easy
- Tasks Completed: 5/5
- Status: Completed

---

# Key Findings

## DNS Is Hierarchical

A domain consists of:

- Top-Level Domain (TLD)
- Second-Level Domain
- Subdomains

---

## DNS Uses Multiple Record Types

| Record Type | Purpose |
|------------|----------|
| A | IPv4 Address |
| AAAA | IPv6 Address |
| CNAME | Alias |
| MX | Mail Routing |
| TXT | Text Information |

---

## DNS Uses Caching

DNS responses remain cached according to their:

```text
TTL (Time To Live)
```

value.

This improves performance and reduces unnecessary queries.

---

## DNS Resolution Is Distributed

Multiple servers participate in a typical lookup:

- Recursive Servers
- Root Servers
- TLD Servers
- Authoritative Servers

---

# Skills Demonstrated

- DNS Fundamentals
- DNS Enumeration
- Network Troubleshooting
- Domain Analysis
- DNS Record Investigation
- nslookup Usage
- Security Reconnaissance
- Information Gathering

---

# Key Lessons Learned

- DNS functions as the directory service of the internet.
- Domain names follow a structured hierarchy.
- Different DNS record types support different services.
- Recursive DNS servers cache responses to improve efficiency.
- Authoritative servers provide the final source of truth.
- DNS information can reveal valuable infrastructure details.
- Tools like nslookup enable quick and effective DNS analysis.
- DNS security controls such as SPF and DMARC help defend against email spoofing.

---

# Conclusion

The DNS in Detail room provided a strong foundation in understanding how internet name resolution works. Through theoretical explanations and practical exercises, I learned how DNS maps domain names to IP addresses, how DNS requests move through the global DNS infrastructure, and how various DNS record types support internet services.

The practical use of `nslookup` transformed the theory into real-world experience and demonstrated how DNS information can be gathered and analyzed during troubleshooting and security assessments. These concepts are fundamental for networking, cybersecurity, penetration testing, and systems administration.
