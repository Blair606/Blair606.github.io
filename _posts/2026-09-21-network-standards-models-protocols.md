---
title: "Network Standards, Models & Protocols: Packet Tracer Analysis"
date: 2026-09-21 10:00:00 +0300
categories: [Lab-Challenges]
tags: [Networking, Cisco, Packet-Tracer, TCP-IP, OSI, HTTP, DNS, ARP]
description: "A practical analysis of an HTTP session using Cisco Packet Tracer to explore the OSI Model, TCP/IP Suite, DNS, ARP, and TCP communication."
---

# Network Standards, Models & Protocols: Packet Tracer Analysis

## Introduction

This lab activity involved using Cisco Packet Tracer to capture and analyze a live HTTP session between a client workstation and a web server on a local area network (LAN). The exercise provided a practical understanding of how the OSI Reference Model and the TCP/IP protocol suite work together to facilitate communication across a network. Additionally, supporting protocols such as DNS and ARP were examined to understand how they contribute to successful web communication. 

The primary objective was to observe the encapsulation and de-encapsulation process as data moves through the Application, Transport, Network, and Data Link layers during an HTTP request and response cycle. 

---

## Lab Objectives

- Understand the interaction between the OSI and TCP/IP models.
- Observe network communication using Cisco Packet Tracer Simulation Mode.
- Examine how HTTP, TCP, IP, ARP, and DNS protocols operate.
- Analyze packet encapsulation and de-encapsulation.
- Understand how network devices exchange information during a web session. 

---

## Tools and Technologies Used

- Cisco Packet Tracer
- HTTP Protocol
- TCP Protocol
- IP Protocol
- ARP Protocol
- DNS Protocol
- Web Client
- Web Server 

---

## Problem Statement

The challenge was to investigate how a client accesses a web server by generating an HTTP request and examining every stage of the communication process. The activity required identifying the protocols involved, understanding the information exchanged at each layer, and analyzing how data travels from source to destination. 

---

## Methodology

### Step 1: Switching to Simulation Mode

The Packet Tracer environment was switched from Realtime Mode to Simulation Mode to enable packet-level analysis. Event filters were configured to capture HTTP traffic and observe protocol interactions step by step. 

### Step 2: Generating HTTP Traffic

The Web Client initiated a connection by navigating to:

```text
www.osi.local
```

The Capture/Forward function was then used to examine the movement of packets between devices. Successful communication resulted in the Web Client displaying the Web Server homepage. 

### Step 3: Examining Packet Details

The OSI Model and PDU Details windows were used to analyze:

- Layer-specific operations
- Source and destination addressing
- TCP port assignments
- HTTP request and response behavior
- DNS name resolution
- MAC address identification via ARP 

---

## Protocol Analysis

### HTTP Protocol

The Application Layer generated an HTTP request from the client to the server.

The request was successfully processed and a response was returned containing the web page hosted on the server. The browser displayed:

> You have successfully accessed the home page for Web Server. 

#### HTTP Port

```text
80
```

Port 80 was identified as the destination port responsible for accepting the web request on the server. 

---

### TCP Protocol

TCP provided reliable communication between the client and server.

Observed values included:

```text
Source Port: 1030
Destination Port: 80
```

The protocol established a connection, maintained reliable transmission, and eventually terminated the session after communication was complete. 

Key observations:

- TCP connection established successfully.
- Connection state changed to ESTABLISHED.
- Connection was later reset and closed. 

---

### IP Protocol

The Network Layer handled logical addressing.

#### Client

```text
192.168.1.1
```

#### Web Server

```text
192.168.1.254
```

These addresses enabled communication between devices across the network. 

---

### ARP Protocol

Before communication could occur, ARP was used to determine the destination MAC address associated with the server's IP address.

ARP bridged Layer 3 addressing and Layer 2 communication by mapping IP addresses to physical hardware addresses. 

---

### DNS Protocol

DNS was responsible for translating the hostname:

```text
www.osi.local
```

into its corresponding IP address:

```text
192.168.1.254
```

This translation occurred before the HTTP request was sent. 

#### DNS Port

```text
53
```

Port 53 was used to process DNS requests and responses. 

---

## OSI Model Analysis

### Layer 7 – Application

Responsible for generating the HTTP request and interacting with DNS services. The packet details indicated that the HTTP client initiated communication by sending an HTTP request to the server. 

### Layer 4 – Transport

TCP was responsible for reliable communication and port management.

Observed values:

```text
Src Port: 1030
Dst Port: 80
```



### Layer 3 – Network

IP addressing was used to identify source and destination devices.

```text
Source IP: 192.168.1.1
Destination IP: 192.168.1.254
```



### Layer 2 – Data Link

Ethernet II frames transported the packet between devices using MAC addresses.

Example:

```text
Source MAC → Destination MAC
0060.47CA.4DEE → 0001.96A9.401D
```



### Layer 1 – Physical

Responsible for transmitting bits across the network media. Packet Tracer displayed Layer 1 activity when frames were physically transmitted between devices. 

---

## Key Findings

1. DNS resolved the hostname before communication could begin. 

2. ARP resolved the destination MAC address required for local delivery. 

3. TCP established a reliable communication channel before data exchange occurred. 

4. HTTP successfully delivered web content from the server to the client. 

5. Packet Tracer provided clear visibility into packet encapsulation and de-encapsulation across OSI layers. 

---

## Screenshots

Replace the paths below with your actual screenshots.

### Simulation Mode

```markdown
/assets/img/labs/simulation-mode.png
```

### HTTP Request Analysis

```markdown
![HTTP Request Analysisequest-analysis.png
```

### DNS Query Analysis

```markdown
/assets/img/labs/dns-query.png
```

### TCP Connection Establishment

```markdown
/assets/img/labs/tcp-connection.png
```

---

## Lessons Learned

This exercise reinforced several important networking concepts:

- The OSI Model provides a structured framework for understanding communication processes.
- The TCP/IP model forms the foundation of modern networking.
- DNS and ARP play critical supporting roles in network communication.
- TCP ensures reliable and ordered data delivery.
- HTTP relies on lower-layer protocols to successfully exchange information.
- Encapsulation and de-encapsulation are essential processes in network communication.
- Packet Tracer is an effective learning platform for visualizing protocol operations and network behavior. 

---

## Conclusion

The lab successfully demonstrated how multiple networking protocols work together to facilitate communication between devices. Through the analysis of DNS queries, ARP requests, TCP connections, and HTTP traffic, it became evident that network communication is a coordinated process involving several layers of the OSI and TCP/IP models.

Observing packet flow in Cisco Packet Tracer transformed theoretical networking concepts into practical understanding and strengthened troubleshooting and protocol analysis skills. 
