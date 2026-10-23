---
title: "Build a Switch and Router Network"
date: 2026-09-23 10:00:00 +0300
categories: [Lab-Challenges]
tags: [Cisco, Routing, Switching, IPv4, IPv6, Packet-Tracer, Networking, IOS]
description: "Built and configured a Cisco switch and router network using IPv4 and IPv6 addressing, device hardening, and connectivity verification."
---

# Build a Switch and Router Network

## Challenge Overview

This lab was completed using Cisco Packet Tracer as part of the Cloud and Network Security course. The objective was to build a functioning network consisting of a Cisco router, a Cisco switch, and two end-user devices connected across separate IPv4 and IPv6 networks.

The exercise reinforced key networking concepts including:

- Router configuration
- Switch configuration
- IPv4 addressing
- IPv6 addressing
- Default gateways
- Device security
- IOS navigation
- Connectivity verification
- Network troubleshooting

---

# Problem Statement

A small network consisting of a router, switch, and two hosts needed to be configured from a clean state.

The challenge required:

- Building the topology
- Assigning IPv4 and IPv6 addresses
- Configuring router interfaces
- Configuring the switch management interface
- Enabling device access security
- Verifying connectivity between different subnets
- Troubleshooting addressing and routing issues

---

# Objectives

- Configure network devices from a blank configuration.
- Implement IPv4 and IPv6 addressing.
- Configure switch management connectivity.
- Verify Layer 2 and Layer 3 communication.
- Apply basic device hardening and security.
- Troubleshoot connectivity issues.

---

# Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- Cisco ISR4331 Router
- Cisco Catalyst 2960 Switch

---

# Network Topology

```text
PC-A
  |
  |
  S1
  |
  |
 R1
  |
  |
PC-B
```

### Device Connections

```text
PC-A → S1

S1 → R1

R1 → PC-B
```

The topology placed each PC on a different subnet, requiring the router to perform Layer 3 forwarding.

---

# Addressing Scheme

| Device | Interface | IPv4 Address | IPv6 Address |
|----------|-----------|--------------|-------------|
| R1 | G0/0/0 | 192.168.0.1 | 2001:DB8:ACAD::1 |
| R1 | G0/0/1 | 192.168.1.1 | 2001:DB8:ACAD:1::1 |
| S1 | VLAN 1 | 192.168.1.2 | N/A |
| PC-A | FastEthernet0 | 192.168.1.3 | 2001:DB8:ACAD:1::3 |
| PC-B | FastEthernet0 | 192.168.0.3 | 2001:DB8:ACAD::3 |

---

# Part 1: Topology Setup and Initialization

## Device Initialization

The network was assembled according to the provided topology and all devices were confirmed to be operating from clean startup configurations.

---

## SDM Template Configuration

The Cisco Catalyst switch initially lacked IPv6 support through its default configuration template.

### Action Performed

The SDM template was changed to:

```text
dual-ipv4-and-ipv6
```

### Benefit

This enabled IPv6 functionality on the switch after a reload.

### Key Learning

Switch features can depend on the active SDM template, and configuration changes may require a reload before taking effect.

---

# Part 2: Device Configuration

## PC Configuration

### PC-A

```text
IPv4:
192.168.1.3/24

IPv6:
2001:DB8:ACAD:1::3/64

Gateway:
192.168.1.1
```

### PC-B

```text
IPv4:
192.168.0.3/24

IPv6:
2001:DB8:ACAD::3/64

Gateway:
192.168.0.1
```

---

# Router Configuration

## Basic Security

The router was configured with:

- Hostname
- Enable secret
- Console password
- VTY password
- Password encryption
- MOTD banner

### Security Features

```text
enable secret class

service password-encryption

banner motd
```

---

## Interface Configuration

### G0/0/0

```text
IPv4:
192.168.0.1/24

IPv6:
2001:DB8:ACAD::1/64
```

Connected to:

```text
PC-B
```

---

### G0/0/1

```text
IPv4:
192.168.1.1/24

IPv6:
2001:DB8:ACAD:1::1/64
```

Connected to:

```text
S1
```

---

## IPv6 Routing

IPv6 routing was enabled globally.

### Command

```text
ipv6 unicast-routing
```

### Benefit

Allowed the router to forward IPv6 traffic between networks.

---

# Initial Connectivity Test

Before router interfaces were fully configured, connectivity testing failed.

### Result

```text
100% Packet Loss
```

### Cause

The router interfaces responsible for routing traffic between subnets had not yet been activated.

### Key Learning

Communication between different networks requires functioning Layer 3 gateways.

---

# IPv4 Connectivity Verification

After interface activation and addressing, connectivity testing was repeated.

### Result

Successful communication between:

```text
PC-A
```

and

```text
PC-B
```

### Observation

The first ping experienced a timeout due to ARP resolution.

Subsequent packets completed successfully.

---

# IPv6 Troubleshooting

## Problem Encountered

IPv4 connectivity worked correctly, but IPv6 communication failed.

### Symptom

```text
Destination Host Unreachable
```

messages appeared when attempting to reach remote IPv6 hosts.

---

## Root Cause

The IPv6 address configured on:

```text
G0/0/1
```

contained a typographical error.

Configured:

```text
200:DB8:ACAD:1::1
```

Expected:

```text
2001:DB8:ACAD:1::1
```

---

## Why It Was Difficult to Detect

The incorrect address remained syntactically valid.

Because the command itself was legal, Cisco IOS generated no error message.

### Key Learning

Successful command execution does not guarantee correct configuration.

Verification is always required.

---

## Resolution

The incorrect IPv6 address was removed and replaced with:

```text
2001:DB8:ACAD:1::1/64
```

---

## Result

IPv6 connectivity immediately succeeded.

Tests confirmed successful communication between:

```text
2001:DB8:ACAD::3
```

and

```text
2001:DB8:ACAD:1::3
```

---

# Switch Configuration

## Basic Configuration

The switch was configured with:

- Hostname
- Default Gateway
- Management Interface
- IPv4 Addressing

### VLAN 1 Management Interface

```text
192.168.1.2/24
```

### Default Gateway

```text
192.168.1.1
```

---

## Remote Management

The switch successfully communicated beyond its local subnet by using the router as its default gateway.

### Verification

The switch successfully pinged:

```text
PC-B
```

through the router.

---

# Connectivity Verification

## IPv4 Connectivity

Successful communication was verified between:

```text
PC-A ↔ PC-B
```

### Result

```text
Successful
```

---

## IPv6 Connectivity

Successful communication was verified between:

```text
PC-A ↔ PC-B
```

using IPv6 addresses.

### Result

```text
Successful
```

---

## Switch Verification

The switch management interface successfully communicated with:

```text
PC-B
```

through the router.

---

# Displaying Device Information

Several show commands were used to verify operational status.

---

## Routing Table Verification

### Command

```text
show ip route
```

### Observation

Two directly connected networks were present.

```text
192.168.0.0/24

192.168.1.0/24
```

### Routing Code

```text
C
```

indicates a directly connected network.

---

## IPv6 Routing Verification

### Command

```text
show ipv6 route
```

### Observation

The routing table displayed the correct IPv6 networks after correcting the addressing error.

---

## Interface Verification

### Command

```text
show ip interface brief
```

### Result

All critical interfaces reported:

```text
up/up
```

confirming full connectivity.

---

# Challenges Encountered

## IPv6 Address Typo

A single missing digit caused complete IPv6 communication failure.

### Problem

```text
200:DB8:ACAD:1::1
```

instead of:

```text
2001:DB8:ACAD:1::1
```

### Resolution

The interface was reconfigured and verified using:

```text
show ipv6 interface
```

### Key Lesson

Verification is just as important as configuration.

---

# Skills Demonstrated

- Cisco IOS Administration
- Router Configuration
- Switch Configuration
- IPv4 Addressing
- IPv6 Addressing
- Routing Fundamentals
- Layer 3 Troubleshooting
- Device Security
- Network Verification
- CLI Navigation

---

# Key Lessons Learned

- Always verify configurations after implementation.
- IPv6 syntax errors may not generate error messages.
- Layer 3 devices are required for inter-subnet communication.
- Routing tables provide valuable troubleshooting information.
- The first failed ping often occurs because of ARP resolution.
- IPv6 routing must be explicitly enabled on Cisco routers.
- Management interfaces require proper gateway configuration for remote communication.

---

# Skills Gained

- Dual-stack network configuration
- IPv4/IPv6 troubleshooting
- Cisco IOS command-line administration
- Network verification techniques
- Routing analysis
- Infrastructure deployment

---

# Conclusion

This lab provided practical experience configuring and troubleshooting both IPv4 and IPv6 networks using Cisco routers and switches. By building the topology from scratch, implementing device security, configuring addressing, enabling routing, and troubleshooting a real IPv6 configuration error, I reinforced fundamental networking skills required for network administration and cybersecurity careers.

The exercise demonstrated the importance of validation and troubleshooting, showing that successfully entering commands is only one part of network configuration. Reliable operation depends on careful verification, testing, and analysis of device behavior after implementation.
