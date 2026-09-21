
---
title: "Wireshark ICMP Traffic Analysis"
date: 2026-09-21 11:00:00 +0300
categories: [Lab-Challenges]
tags: [Wireshark, Networking, ICMP, TCP-IP, ARP, LAN, Packet-Analysis]
description: "Capturing and analyzing local and remote ICMP traffic using Wireshark to understand MAC addressing, ARP, and network communication."
---

# Wireshark ICMP Traffic Analysis

## Overview

This lab explored the use of Wireshark to capture and analyze both local and remote ICMP (ping) traffic. The exercise focused on understanding how IP addressing and MAC addressing work together to facilitate communication across a network.

Two scenarios were investigated:

1. Capturing ICMP communication between a workstation and its default gateway.
2. Capturing ICMP communication between a workstation and remote internet hosts.

The lab provided practical insight into how Ethernet frames, routing, ARP, and ICMP operate within modern networks. 【1-51ebf2】

---

# Problem Statement

Network administrators often need to troubleshoot connectivity issues and understand how traffic flows within a network. This lab aimed to examine how ICMP traffic behaves when communicating with both local and remote devices and determine how MAC addressing differs between the two scenarios. 【1-51ebf2】

---

# Objectives

- Capture ICMP traffic using Wireshark.
- Analyze local network communication.
- Analyze communication with remote hosts.
- Understand the relationship between MAC and IP addresses.
- Examine the role of ARP in local communication.
- Compare local and remote traffic behavior. 【1-51ebf2】

---

# Tools Used

- Wireshark
- Windows Command Prompt
- ICMP (Ping)
- ARP
- IPv4
- Windows Operating System 【1-51ebf2】

---

# Lab Environment

### Device Information

| Parameter | Value |
|------------|--------|
| PC IP Address | 192.168.1.142 |
| PC MAC Address | 04-6C-59-D7-A0-DC |
| Default Gateway | 192.168.1.1 |
| DNS Server | 192.168.1.1 |

【1-51ebf2】

---

# Part 1: Capture and Analyze Local ICMP Data

## Step 1: Retrieve Network Information

The following command was used to obtain interface information:

```cmd
ipconfig /all
```

Key information recorded included:

```text
IPv4 Address: 192.168.1.142
MAC Address: 04-6C-59-D7-A0-DC
Default Gateway: 192.168.1.1
DNS Server: 192.168.1.1
```

【1-51ebf2】

### Screenshot

```text
/assets/img/labs/ipconfig-all.png
```

---

## Step 2: Start Packet Capture

Wireshark was opened on the active Wi-Fi adapter.

The following capture filter was applied:

```text
icmp
```

This ensured that only ICMP traffic appeared in the capture window. 【1-51ebf2】

### Screenshot

```text
/assets/img/labs/wireshark-filter.png
```

---

## Step 3: Generate Local ICMP Traffic

The default gateway was used as the target because another LAN workstation was not available.

```cmd
ping 192.168.1.1
```

Result:

```text
4 packets transmitted
4 packets received
0% packet loss
```

【1-51ebf2】

### Screenshot

```text
/assets/img/labs/local-ping.png
```

---

# Analysis of Local Traffic

Wireshark displayed ICMP Echo Requests and Echo Replies exchanged between the workstation and the router.

The Ethernet II header revealed both source and destination MAC addresses.

### Observation

Router MAC Address:

```text
0c:61:f9:1a:1a:18
```

PC MAC Address:

```text
04-6c-59-d7-a0-dc
```

The captured data showed that local devices communicate directly using their real MAC addresses. 【1-51ebf2】

---

# ARP Analysis

Before sending an ICMP request, the operating system needed to identify the MAC address associated with the destination IP.

This process was performed through ARP (Address Resolution Protocol).

### ARP Process

1. PC broadcasts ARP request.
2. Gateway responds with its MAC address.
3. MAC address is stored in the ARP cache.
4. ICMP packet is transmitted within an Ethernet frame.

【1-51ebf2】

### Key Finding

The MAC address of the destination device was learned dynamically using ARP before communication occurred. 【1-51ebf2】

---

# Part 2: Capture and Analyze Remote ICMP Data

A second Wireshark capture session was started to analyze communication with remote internet hosts. 【1-51ebf2】

## Hosts Tested

```text
www.google.com
www.cisco.com
www.yahoo.com
```

【1-51ebf2】

---

# Remote Host Analysis

## Google

### IP Address

```text
142.251.154.119
```

### Destination MAC

```text
0c:61:f9:1a:1a:18
```

【1-51ebf2】

---

## Cisco

### IP Address

```text
2.22.192.103
```

### Destination MAC

```text
0c:61:f9:1a:1a:18
```

【1-51ebf2】

---

## Yahoo

### IP Address

```text
69.147.82.61
```

### Destination MAC

```text
0c:61:f9:1a:1a:18
```

【1-51ebf2】

---

# Significant Observation

Although each website had a different IP address, all captured packets used the same destination MAC address.

### Why?

The destination MAC address belonged to the default gateway rather than the remote website.

```text
0c:61:f9:1a:1a:18
```

This demonstrates that MAC addresses only have significance within the local network segment. When traffic is destined for a remote network, the workstation forwards the frame to its default gateway, which then routes the packet toward its destination. 【1-51ebf2】

---

# Local vs Remote Communication

## Local Host Communication

```text
PC → Local Device
```

- ARP discovers the actual MAC address.
- Ethernet frame is delivered directly.
- Real destination MAC is visible.

【1-51ebf2】

## Remote Host Communication

```text
PC → Default Gateway → Internet
```

- Actual web server MAC is never visible.
- Ethernet frame is sent to the router.
- Router forwards the packet through multiple hops.

【1-51ebf2】

---

# Reflection

## Why does Wireshark show the MAC address of local hosts but not remote hosts?

Ethernet communication only functions within a local broadcast domain.

When communicating with devices on the same LAN, ARP can resolve and obtain the destination device's actual MAC address.

For remote devices, the workstation only needs the MAC address of its default gateway. The gateway then removes the Ethernet frame and forwards the packet toward the next hop using a new Ethernet frame. This process is repeated by routers throughout the communication path.

As a result, the actual MAC address of a remote web server is never visible to the originating workstation or Wireshark capture session. 【1-51ebf2】

---

# Screenshots

Replace the placeholders below with your screenshots.

## Interface Information

```text
/assets/img/labs/ipconfig-all.png
```

## ICMP Capture Filter

```text
/assets/img/labs/icmp-filter.png
```

## Local Ping Analysis

```text
/assets/img/labs/local-ping-analysis.png
```

## Google Ping Analysis

```text
/assets/img/labs/google-ping-analysis.png
```

## Cisco Ping Analysis

```text
/assets/img/labs/cisco-ping-analysis.png
```

## Yahoo Ping Analysis

```text
/assets/img/labs/yahoo-ping-analysis.png
```

---

# Key Lessons Learned

- ICMP is commonly used for connectivity testing.
- Wireshark is an effective packet analysis tool.
- ARP is required to resolve local MAC addresses.
- Ethernet addressing only applies to devices on the local network.
- Remote communication always relies on the default gateway.
- Routers re-encapsulate frames at every hop.
- MAC addresses and IP addresses serve different purposes in network communication.
- Understanding packet flow is essential for troubleshooting and network administration. 【1-51ebf2】

---

# Conclusion

This lab successfully demonstrated how Wireshark can be used to analyze network traffic and visualize communication between devices. The exercise highlighted the relationship between IP addressing, MAC addressing, ICMP communication, and ARP resolution.

The most important takeaway was understanding that local communication reveals the actual MAC address of destination devices, while communication with remote hosts only reveals the MAC address of the default gateway. This distinction is fundamental to understanding how Ethernet and routing operate in modern networks. 【1-51ebf2】
