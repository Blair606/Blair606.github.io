---
title: "WLAN Configuration: Home Router and Enterprise WLC"
date: 2026-10-06 09:00:00 +0300
categories: [Lab-Challenges]
tags: [Wireless, WLAN, Cisco, WLC, WPA2, RADIUS, Packet-Tracer, Networking]
description: "Configured a home wireless router and an enterprise Wireless LAN Controller using WPA2-Personal and WPA2-Enterprise security models."
---

# WLAN Configuration: Home Router and Enterprise WLC

## Challenge Overview

This lab focused on the deployment and security of wireless networks in two different environments:

1. A home wireless network using WPA2-Personal security.
2. An enterprise wireless network managed through a Cisco Wireless LAN Controller (WLC) using both WPA2-Personal and WPA2-Enterprise authentication.

The objective was to configure wireless connectivity, implement secure authentication methods, and verify connectivity between clients and network services.

---

# Problem Statement

Wireless networks are widely used in both home and enterprise environments, but each environment has different security and management requirements.

The challenge was to:

- Configure a secure home wireless network.
- Configure enterprise WLANs through a Wireless LAN Controller.
- Implement WPA2-Personal and WPA2-Enterprise authentication.
- Integrate RADIUS-based authentication.
- Verify connectivity between wireless clients and network resources.

---

# Objectives

- Configure a home wireless router.
- Configure DHCP services for wireless clients.
- Secure wireless communications using WPA2.
- Configure enterprise WLANs on a Cisco WLC.
- Deploy WPA2-Enterprise authentication with RADIUS.
- Validate wireless connectivity and user authentication.

---

# Tools Used

- Cisco Packet Tracer
- Cisco Wireless LAN Controller (WLC)
- RADIUS Server
- Wireless Access Point (LAP)
- Wireless Router
- DHCP Services
- WPA2-Personal Security
- WPA2-Enterprise Security
- 802.1X Authentication

---

# Network Overview

## Home Wireless Network

### Wireless Router

```text
192.168.6.1/27
```

### Wireless Devices

- Laptop
- Tablet PC
- Smartphone

### Security Model

```text
WPA2-Personal
```

SSID:

```text
HomeSSID
```

Passphrase:

```text
Cisco123
```

---

## Enterprise Wireless Network

### Core Components

- RTR-1
- SW1
- WLC-1
- LAP-1
- RADIUS Server

### Wireless VLANs

#### Wireless VLAN 2

```text
SSID-2
```

Authentication:

```text
WPA2-Personal
```

#### Wireless VLAN 5

```text
SSID-5
```

Authentication:

```text
WPA2-Enterprise (802.1X)
```

RADIUS User:

```text
userWLAN5
```

---

# Part 1: Home Wireless Router Configuration

## DHCP Configuration

The wireless router was configured with a new addressing scheme to replace the default configuration.

### Configuration

```text
Router IP Address:
192.168.6.1

DHCP Enabled

Start Address:
192.168.6.3

Maximum Users:
20
```

This provided an available DHCP range of:

```text
192.168.6.3 – 192.168.6.22
```

for client devices.

---

## Wireless Configuration

The 2.4 GHz radio was configured with:

```text
SSID:
HomeSSID

Channel:
6

SSID Broadcast:
Enabled
```

The 5 GHz radios remained disabled because they were not required for this deployment.

---

## Wireless Security

The wireless network was secured using:

```text
Security Mode:
WPA2-Personal

Encryption:
AES

Passphrase:
Cisco123
```

---

## Administrative Hardening

The default administrator credentials were replaced with a custom administrator password.

This prevents unauthorized access through commonly known factory-default credentials.

---

## Client Connectivity

The following devices successfully connected:

- Laptop
- Tablet PC
- Smartphone

All devices received IP addresses automatically through DHCP.

---

## Verification

### Host-to-Host Communication

Successful ping tests confirmed:

```text
Laptop ↔ Tablet PC
```

communication.

### Internet Access

The wireless hosts successfully accessed:

```text
www.netacad.pt
```

demonstrating external connectivity.

---

# Part 2: Enterprise Wireless LAN Controller (WLC)

## Enterprise Architecture

The enterprise deployment used:

```text
Wireless LAN Controller
(WLC-1)
```

to centrally manage wireless access.

This architecture provides:

- Centralized management
- Improved scalability
- Enterprise authentication
- VLAN segmentation

---

## VLAN Interfaces

Two dynamic VLAN interfaces were configured.

### VLAN 2

```text
Network:
192.168.2.0/24

Gateway:
192.168.2.1
```

### VLAN 5

```text
Network:
192.168.5.0/24

Gateway:
192.168.5.1
```

---

## DHCP Scope Configuration

An internal DHCP scope was configured on the controller.

### Scope Configuration

```text
Pool:
192.168.100.235
-
192.168.100.245

Default Router:
192.168.100.1
```

This provided dynamic addressing for wireless management services.

---

## RADIUS Configuration

The WLC was integrated with an external RADIUS server.

### RADIUS Server

```text
10.6.0.254
```

### Authentication Port

```text
1812
```

Purpose:

- User authentication
- Centralized identity management
- WPA2-Enterprise support

---

## SNMP Monitoring

An SNMP trap receiver was configured.

### SNMP Server

```text
10.6.0.254
```

Community:

```text
WLAN
```

This enables centralized monitoring and event reporting.

---

# WLAN Deployment

## Wireless VLAN 2

### Configuration

```text
SSID:
SSID-2

Security:
WPA2-Personal

Encryption:
AES

Authentication:
Pre-Shared Key (PSK)
```

### Use Case

A simple wireless network requiring all users to share the same passphrase.

---

## Wireless VLAN 5

### Configuration

```text
SSID:
SSID-5

Security:
WPA2-Enterprise

Authentication:
802.1X

Backend:
RADIUS Server
```

### Credentials

```text
Username:
userWLAN5

Password:
userW5pass
```

### Use Case

Enterprise environments where user access is validated individually.

---

# Client Configuration

## Wireless Host 1

Connected to:

```text
SSID-2
```

Security:

```text
WPA2-Personal
```

---

## Wireless Host 2

Connected to:

```text
SSID-5
```

Security:

```text
WPA2-Enterprise
```

Authentication:

```text
RADIUS
```

---

# Verification

## Wireless Connectivity

Both wireless hosts successfully:

- Obtained IP addresses
- Connected to their assigned WLANs
- Authenticated successfully

---

## Inter-VLAN Communication

Successful ping results confirmed communication between:

```text
Wireless Host 1
```

and

```text
Wireless Host 2
```

despite existing on separate VLANs.

---

## Web Server Access

Both hosts successfully accessed:

```text
www.netacad.pt
```

confirming proper routing, DHCP functionality, authentication, and internet connectivity.

---

# Key Findings

## Home Wireless Security

WPA2-Personal offers:

- Easy deployment
- Low management overhead
- Simple shared authentication

However:

- Every user shares the same password.
- Revoking individual users is difficult.

---

## Enterprise Wireless Security

WPA2-Enterprise offers:

- Individual user authentication
- Centralized access control
- RADIUS integration
- Better scalability
- Stronger security

If one user leaves the organization, only that account must be disabled rather than changing the entire wireless password.

---

## Wireless LAN Controllers Simplify Management

The WLC provided centralized management for:

- WLANs
- Access Points
- Authentication Policies
- DHCP Services
- Monitoring Services

This significantly reduces administrative effort in large wireless environments.

---

# Challenges Encountered

## VLAN Interface Misconfiguration

During deployment, VLAN 5 addressing was 
