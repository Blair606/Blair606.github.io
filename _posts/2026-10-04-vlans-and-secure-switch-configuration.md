---
title: "VLANs and Secure Switch Configuration"
date: 2026-10-04 10:00:00 +0300
categories: [Lab-Challenges]
tags: [Cisco, VLAN, Switching, Port-Security, DHCP-Snooping, STP, BPDU-Guard, Networking]
description: "Configured VLANs, trunking, port security, DHCP snooping, PortFast, and BPDU Guard to secure a Layer 2 network."
---

# VLANs and Secure Switch Configuration

## Challenge Overview

This lab focused on building and securing a Layer 2 switching environment from a clean configuration. A router and two switches were configured to support VLAN segmentation, management access, DHCP services, and multiple switch security features.

The exercise progressed from basic connectivity configuration to implementing enterprise-grade Layer 2 security controls such as:

- VLAN segmentation
- 802.1Q trunking
- Port Security
- DHCP Snooping
- PortFast
- BPDU Guard
- Secure management VLANs

---

# Problem Statement

Modern switched networks require more than connectivity. Unsecured switchports, rogue DHCP servers, unauthorized devices, and accidental topology changes can introduce significant security and operational risks.

The goal of this lab was to:

- Build a functional VLAN environment.
- Secure access ports.
- Secure trunk links.
- Prevent rogue DHCP attacks.
- Reduce spanning-tree convergence delays.
- Protect the network from unauthorized connections.

---

# Objectives

- Configure VLANs.
- Configure Switch Virtual Interfaces (SVIs).
- Implement 802.1Q trunking.
- Configure management interfaces.
- Secure unused switchports.
- Deploy port security.
- Implement DHCP snooping.
- Configure PortFast and BPDU Guard.
- Verify end-to-end connectivity.

---

# Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- VLAN Technology
- Spanning Tree Protocol (STP)
- DHCP Snooping
- Port Security
- 802.1Q Trunking

---

# Network Topology

## Devices

### Router

```text
R1
```

### Switches

```text
S1
S2
```

### End Devices

```text
PC-A
PC-B
```

---

## Physical Connections

```text
R1 G0/0/1  --> S1 Fa0/5

S1 Fa0/1   --> S2 Fa0/1

PC-A       --> S1 Fa0/6

PC-B       --> S2 Fa0/18
```

---

# Addressing Scheme

| Device | Interface | IP Address |
|----------|------------|-------------|
| R1 | G0/0/1 | 192.168.10.1 |
| R1 | Loopback0 | 10.10.1.1 |
| S1 | VLAN 10 | 192.168.10.201 |
| S2 | VLAN 10 | 192.168.10.202 |
| PC-A | DHCP | Dynamic |
| PC-B | DHCP | Dynamic |

---

# Part 1: Device Configuration

## Router Configuration

Router R1 was configured with:

- Hostname
- DHCP Services
- Loopback Interface
- Routed Interface Connectivity

### DHCP Configuration

The DHCP server provided addresses for end-user devices while excluding critical infrastructure addresses.

### Interfaces Configured

```text
G0/0/1
192.168.10.1
```

```text
Loopback0
10.10.1.1
```

---

## Switch Configuration

Basic switch settings included:

### Hostname Assignment

```text
S1
S2
```

### DNS Lookup Disabled

```text
no ip domain-lookup
```

### Interface Documentation

Descriptions were added to all active interfaces for easier administration and troubleshooting.

### Default Gateway

```text
ip default-gateway 192.168.10.1
```

This enabled remote management of the switches.

---

# Part 2: VLAN Configuration

## VLAN 10 – Management VLAN

VLAN 10 was created on both switches.

### Purpose

- Switch Management
- Device Administration
- Inter-switch Communication

### SVI Addresses

#### S1

```text
192.168.10.201
```

#### S2

```text
192.168.10.202
```

---

## VLAN 333 – Native VLAN

Created to serve as the trunk's native VLAN.

### Benefits

- Avoids use of VLAN 1
- Improves trunk security
- Follows security best practices

---

## VLAN 999 – Parking Lot VLAN

Created to isolate unused switchports.

### Benefits

- Prevents unauthorized access
- Reduces attack surface
- Provides administrative control of unused interfaces

---

# Part 3: Switch Security

## 802.1Q Trunking

A trunk link was established between:

```text
S1 Fa0/1
```

and

```text
S2 Fa0/1
```

### Configuration

```text
switchport mode trunk
switchport trunk native vlan 333
```

### Security Enhancement

Dynamic Trunking Protocol (DTP) was disabled:

```text
switchport nonegotiate
```

### Benefit

Prevents unauthorized trunk negotiation.

---

# Access Port Configuration

Access ports were assigned to VLAN 10.

### S1

```text
Fa0/5
Fa0/6
```

### S2

```text
Fa0/18
```

---

# Unused Port Security

All unused ports were:

1. Assigned to VLAN 999
2. Administratively disabled

### Configuration

```text
switchport access vlan 999
shutdown
```

### Benefit

Prevents unused ports from being exploited by unauthorized users.

---

# Port Security

## S1 Access Port

Port:

```text
Fa0/6
```

### Settings

```text
Maximum MAC Addresses: 3
Violation Mode: Restrict
Aging Time: 60 Minutes
Aging Type: Inactivity
```

### Benefit

Limits the number of devices that may connect to the port.

---

## S2 Access Port

Port:

```text
Fa0/18
```

### Settings

```text
Maximum MAC Addresses: 2
Violation Mode: Protect
Sticky Learning Enabled
```

### Benefit

Automatically learns and secures known client MAC addresses.

---

# DHCP Snooping

DHCP snooping was enabled to protect against rogue DHCP servers.

### Features Implemented

#### Global Snooping

```text
ip dhcp snooping
```

#### VLAN Protection

```text
ip dhcp snooping vlan 10
```

#### Trusted Interface

```text
Fa0/1
```

The trunk toward legitimate DHCP services was trusted.

#### Rate Limiting

```text
Fa0/18
```

limited to:

```text
5 DHCP packets per second
```

### Benefit

Prevents rogue DHCP servers from distributing incorrect network configurations.

---

# PortFast Configuration

PortFast was enabled on access ports.

### Ports

```text
S1 Fa0/5
S1 Fa0/6
S2 Fa0/18
```

### Benefit

Allows end devices to immediately enter forwarding mode without waiting for spanning-tree convergence.

---

# BPDU Guard Configuration

BPDU Guard was enabled on ports connected directly to end-user devices.

### Ports Protected

```text
S1 Fa0/6
S2 Fa0/18
```

### Benefit

Prevents accidental or malicious connection of switches to user-facing ports.

---

# Verification

## DHCP Verification

PC-B successfully:

```text
ipconfig /release
ipconfig /renew
```

and received a valid DHCP address from the configured DHCP pool.

---

## Connectivity Testing

Successful connectivity was verified between:

```text
PC-A
```

and

```text
PC-B
```

---

## Trunk Verification

The following command confirmed proper trunk operation:

```text
show interfaces trunk
```

Results confirmed:

- Native VLAN 333
- Operational trunking
- VLAN propagation between switches

---

## Port Security Verification

Commands used:

```text
show port-security interface
```

```text
show port-security address
```

Results confirmed:

- Learned MAC addresses
- Security status active
- Violation modes configured correctly

---

## DHCP Snooping Verification

Command:

```text
show ip dhcp snooping
```

confirmed:

- VLAN protection active
- Trusted trunk configured
- Rate limiting operational

---

# Key Findings

## VLAN Segmentation Improves Security

Separating management traffic from user traffic reduces exposure and simplifies administration.

---

## VLAN 1 Should Be Avoided

Using VLAN 333 as the native VLAN reduces the risk associated with default VLAN exploitation.

---

## Disabled Ports Increase Security

Assigning unused ports to a parking-lot VLAN and shutting them down prevents unauthorized access.

---

## Port Security Restricts Unauthorized Devices

Port Security limits which devices can access the network and reduces MAC flooding risks.

---

## DHCP Snooping Protects Against Rogue DHCP Servers

Only trusted interfaces can deliver DHCP responses to clients.

---

## BPDU Guard Protects Layer 2 Stability

BPDU Guard prevents unauthorized switches from affecting spanning-tree operations.

---

# Challenges Encountered

## Native VLAN Mismatch

A temporary native VLAN mismatch warning appeared during trunk implementation because one side of the trunk was configured before the other.

### Resolution

The native VLAN was configured identically on both ends:

```text
VLAN 333
```

The warning immediately cleared.

### Lesson Learned

Always configure both sides of a trunk consistently.

---

# Skills Demonstrated

- VLAN Configuration
- Switch Administration
- Layer 2 Security
- 802.1Q Trunking
- Port Security
- DHCP Snooping
- Spanning Tree Security
- Cisco IOS CLI Administration
- Troubleshooting
- Network Hardening
