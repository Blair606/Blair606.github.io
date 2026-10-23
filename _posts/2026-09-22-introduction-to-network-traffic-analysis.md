---
title: "Introduction to Network Traffic Analysis - HTB Academy"
date: 2026-09-22 09:00:00 +0300
categories: [Lab-Challenges]
tags: [Wireshark, Traffic-Analysis, Network-Security, HTB-Academy, RDP, Packet-Analysis, Cybersecurity]
description: "Completed the HTB Academy Introduction to Network Traffic Analysis module, analyzing HTTP and RDP traffic using Wireshark and protocol inspection techniques."
---

# Introduction to Network Traffic Analysis - HTB Academy

## Challenge Overview

This lab was completed through the HTB Academy **Introduction to Network Traffic Analysis** module.

The module introduced practical techniques for examining network traffic, identifying communication patterns, recovering information from packet captures, and understanding how protocols behave during real-world communication.

The practical exercises focused on:

- Traffic Analysis Workflows
- Wireshark Fundamentals
- RDP Traffic Analysis
- HTTP Traffic Analysis
- Packet Inspection
- Protocol Investigation

---

# Problem Statement

Security analysts frequently need to determine:

- What happened on a network.
- Which systems communicated.
- What data was exchanged.
- Whether sensitive information was exposed.

The challenge was to investigate live and recorded network traffic to identify communications, recover information from packet captures, and understand how different protocols behave during operation.

---

# Objectives

- Capture network traffic using Wireshark.
- Analyze communication between hosts.
- Follow TCP streams.
- Identify plaintext credentials.
- Investigate encrypted RDP traffic.
- Understand protocol-level information disclosure.
- Develop practical packet analysis skills.

---

# Tools Used

### Analysis Tools

- Wireshark
- tcpdump

### Remote Access

- xfreerdp
- Remote Desktop Protocol (RDP)

### Platform

- HTB Academy

---

# Guided Lab: Traffic Analysis Workflow

## Environment Setup

The HTB Academy VPN was initialized and connectivity was verified before beginning the analysis.

### VPN Interface

```text
tun0
```

### Target System

```text
10.129.43.4
```

Connectivity testing confirmed successful communication between the attack VM and the target system.

### Verification

```bash
ping 10.129.43.4
```

Result:

```text
0% Packet Loss
```

---

# Establishing an RDP Session

The target was accessed using Remote Desktop Protocol.

### Command Used

```bash
xfreerdp
```

### Connection Target

```text
10.129.43.4
```

The RDP session provided access to the target machine, allowing direct observation of internal network traffic.

### Key Learning

Traffic analysis often requires access to systems positioned closer to the traffic source.

---

# Capturing Network Traffic

Once connected to the target system, Wireshark was launched.

### Interface Selected

```text
ens224
```

This interface carried internal network traffic generated within the lab environment.

### Observed Protocols

- HTTP
- TCP
- ARP

### Source Hosts

The initial capture revealed communication between:

```text
172.16.10.20
```

and

```text
172.16.10.2
```

---

# Traffic Analysis

## Identifying Interesting Traffic

The captured packets immediately revealed unencrypted HTTP communications alongside routine ARP traffic.

### Why This Matters

HTTP transmits information without encryption, making captured traffic potentially valuable from an investigative and security perspective.

---

# Following TCP Streams

The HTTP communication was isolated by following the associated TCP stream.

### Stream Characteristics

The connection included:

- TCP Handshake
- HTTP POST Request
- HTTP Response
- Connection Termination

The captured session represented a complete client-server interaction.

---

# Recovering Plaintext Credentials

The next stage involved examining the contents of the TCP stream.

### HTTP POST Request

Reviewing the stream revealed login credentials transmitted without encryption.

### Username

```text
bob
```

### Password

```text
B0b_hardW0rker!
```

---

# Why Were the Credentials Visible?

The application used:

```text
HTTP
```

instead of:

```text
HTTPS
```

Because no TLS encryption was present, all submitted form data remained visible within the captured packets.

### Key Learning

Data transmitted using HTTP should always be considered exposed to anyone capable of intercepting the traffic.

---

# Decrypting RDP Connections

The second exercise analyzed a pre-captured network session containing Remote Desktop traffic.

### Capture File

```text
guided-rdp.pcapng
```

---

# RDP Analysis

The traffic was filtered using:

```text
tcp.port == 3389
```

to isolate Remote Desktop Protocol communications.

### Observed Activity

The capture contained:

- TLS Handshake
- CredSSP Negotiation
- Authentication Traffic
- Session Establishment

---

# TLS Handshake Analysis

The capture demonstrated how RDP secures communications using TLS.

### Observed Components

- Client Hello
- Server Hello
- Certificate Exchange
- Client Key Exchange
- Change Cipher Spec
- Finished Messages

These stages established encrypted communications between the client and server.

---

# CredSSP Analysis

Within the authentication sequence, CredSSP traffic became visible.

### Components Observed

```text
NTLMSSP_NEGOTIATE
NTLMSSP_CHALLENGE
NTLMSSP_AUTH
```

These packets assisted in user authentication before full session establishment.

---

# Identifying the Connected User

Although the session itself was encrypted, the username remained visible.

### User Account

```text
bucky
```

The name appeared within:

```text
NTLMSSP_AUTH
```

authentication traffic.

---

# mstshash Cookie Discovery

An additional discovery occurred before TLS encryption began.

The initial RDP connection request contained:

```text
mstshash
```

information that included the username.

### Revealed Value

```text
bucky
```

This information was transmitted before encryption was negotiated.

---

# Security Findings

## HTTP Credentials Were Recoverable

The HTTP login submitted credentials in plaintext.

This allowed direct recovery of:

- Username
- Password

without any decryption effort.

---

## Encryption Does Not Prevent Metadata Exposure

Although RDP protects session contents, metadata present during initial negotiation can still leak information.

### Examples

- Usernames
- Hostnames
- Session Identifiers

---

## Packet Analysis Requires Context

Traffic that appears secure should still be analyzed carefully.

Even encrypted protocols may expose useful information during setup and negotiation phases.

---

# Practical Skills Gained

This module provided hands-on experience with:

- Wireshark
- Packet Analysis
- RDP Traffic Inspection
- HTTP Analysis
- Traffic Capture
- Session Investigation
- Protocol Research
- Stream Reconstruction

---

# Evidence of Completion

The module completion can be verified using the HTB Academy achievement page.

🔗 **Achievement Badge**

https://academy.hackthebox.com/achievement/2870830/34

### Completion Summary

- Platform: HTB Academy
- Module: Introduction to Network Traffic Analysis
- Difficulty: Tier 0
- Status: Completed

---

# Key Findings

## Plaintext Traffic Is Dangerous

Applications that transmit credentials over HTTP expose users to credential theft and interception attacks.

---

## Follow TCP Stream Is Extremely Valuable

The ability to reconstruct an entire conversation from packet captures simplifies investigations and exposes hidden details.

---

## Encryption Has Limits

While encrypted protocols protect content, identity and metadata may still be exposed during connection setup.

---

## Interface Selection Matters

Capturing traffic on the correct interface is essential.

Analyzing the wrong interface may result in missing the relevant communications entirely.

---

# Skills Demonstrated

- Traffic Analysis
- Wireshark Usage
- Protocol Inspection
- Packet Capture Investigation
- Credential Discovery
- RDP Analysis
- TCP Stream Reconstruction
- Security Monitoring
- Threat Investigation
- Network Troubleshooting

---

# Key Lessons Learned

- Always verify whether traffic is encrypted
