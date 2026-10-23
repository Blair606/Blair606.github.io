---
title: "Configuring and Verifying a Site-to-Site IPsec VPN"
date: 2026-10-11 14:00:00 +0300
categories: [Lab-Challenges]
tags: [Cisco, IPsec, VPN, Networking, Routing, IKE, ISAKMP, Cybersecurity]
description: "Configured and verified a site-to-site IPsec VPN between two Cisco routers using IKE Phase 1 and Phase 2 policies in Cisco Packet Tracer."
---

# Configuring and Verifying a Site-to-Site IPsec VPN

## Challenge Overview

This lab involved configuring and verifying a site-to-site IPsec VPN between two Cisco routers using the Cisco IOS Command Line Interface (CLI). The VPN was established between Router R1 and Router R3 with Router R2 acting as a transit device that was completely unaware of the VPN.

The objective was to secure communication between two remote LANs while allowing unrelated traffic to continue routing normally without encryption.

---

# Problem Statement

Organizations with offices in different geographical locations often need a secure method of exchanging data over untrusted networks.

The challenge was to:

- Establish an IPsec VPN tunnel between two routers.
- Secure communication between two remote LANs.
- Allow only designated traffic to be encrypted.
- Verify successful tunnel establishment and operation.
- Confirm that unprotected traffic continued to route normally.

---

# Objectives

- Configure IKE Phase 1 policies.
- Configure IKE Phase 2 policies.
- Define interesting traffic.
- Create and apply crypto maps.
- Verify VPN establishment.
- Analyze encrypted and unencrypted traffic behavior.

---

# Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- IPsec VPN Technology
- ISAKMP (IKE Phase 1)
- Crypto Maps
- Access Control Lists (ACLs)

---

# Network Topology

## VPN Endpoints

```text
R1 (10.1.1.2) <----> R3 (10.2.2.2)
```

Router R2 operated as a normal intermediary router carrying encrypted traffic between the two VPN endpoints.

---

## Protected Networks

### Site A

```text
192.168.1.0/24
```

Connected to Router R1.

### Site B

```text
192.168.3.0/24
```

Connected to Router R3.

---

## Unprotected Network

```text
192.168.2.0/24
```

Connected to Router R2.

This network was not part of the VPN tunnel and all traffic flowed normally without encryption.

---

# Technical Configuration

## IKE Phase 1

Phase 1 was responsible for establishing trust between the VPN peers.

### Configuration Parameters

| Parameter | Value |
|-----------|---------|
| Key Distribution | ISAKMP |
| Encryption | AES 256 |
| Authentication | Pre-shared Key |
| Diffie-Hellman Group | 5 |
| Hashing | SHA-1 |
| Key Lifetime | 86400 Seconds |
| Shared Secret | vpnpa55 |

---

## IKE Phase 2

Phase 2 established the actual IPsec security association used to encrypt data.

### Transform Set

```text
VPN-SET
```

### Encryption

```text
ESP-AES
```

### Authentication

```text
ESP-SHA-HMAC
```

---

# Approach

## Step 1: Verify Connectivity

Before implementing the VPN, connectivity was verified between the end devices located behind each router.

### Test Performed

```text
PC-A ---> PC-C
```

The ping confirmed:

- Layer 3 connectivity existed
- Routing was configured correctly
- The VPN could be deployed on a functioning network

---

## Step 2: Enable Security Technology Package

The Cisco Security Technology Package was required to support IPsec functionality.

### Verification

```text
show version
```

Confirmed:

```text
securityk9
```

was active on the routers.

---

## Step 3: Define Interesting Traffic

Access Control List 110 was created to identify traffic that should trigger the VPN tunnel.

### R1 ACL

```text
access-list 110 permit ip
192.168.1.0 0.0.0.255
192.168.3.0 0.0.0.255
```

### R3 ACL

```text
access-list 110 permit ip
192.168.3.0 0.0.0.255
192.168.1.0 0.0.0.255
```

Only traffic between these networks was encrypted.

---

## Step 4: Configure IKE Phase 1

The ISAKMP policy established secure communication between both routers.

### Key Elements

```text
Encryption: AES 256
Authentication: Pre-share
DH Group: 5
```

### Shared Key

```text
vpnpa55
```

---

## Step 5: Configure IPsec Transform Set

The transform set defined the encryption and integrity mechanisms used by the VPN tunnel.

### Configuration

```text
crypto ipsec transform-set VPN-SET
esp-aes
esp-sha-hmac
```

---

## Step 6: Configure Crypto Maps

Crypto maps tied together:

- Remote Peer
- Transform Set
- Interesting Traffic ACL

### Crypto Map

```text
crypto map VPN-MAP
```

The map was then applied to the serial interfaces facing the WAN.

---

# Challenges Encountered

## Configuration Order Error

While configuring R3, the crypto map was applied before it had been created.

### Error Message

```text
ERROR:
Crypto Map with tag VPN-MAP does not exist.
```

### Resolution

The transform set and crypto map were created first, after which the interface binding succeeded without issues.

### Lesson Learned

Cisco IOS enforces configuration dependencies, making configuration order critical during VPN deployment.

---

# Verification

## Before Tunnel Activation

Verification using:

```text
show crypto ipsec sa
```

showed:

```text
0 encrypted packets
0 decrypted packets
```

This confirmed no interesting traffic had yet passed through the VPN.

---

## Generating Interesting Traffic

Traffic was generated by pinging:

```text
192.168.3.3
```

from PC-A.

The first ping experienced delays while:

- Phase 1 negotiation occurred
- Phase 2 negotiation occurred
- Security Associations were established

Subsequent pings completed successfully.

---

## Tunnel Validation

Successful verification demonstrated:

- Tunnel establishment
- Encrypted packet transmission
- Proper security association creation
- Successful communication between protected networks

---

# Key Findings

## IPsec Operates in Two Stages

### Phase 1

Establishes trust and creates a secure management channel between peers.

### Phase 2

Negotiates the encryption mechanisms used to protect actual user traffic.

---

## ACLs Define Encryption Boundaries

The VPN did not encrypt all traffic.

Only traffic matching ACL 110 was protected.

This demonstrated the role of ACLs in determining what qualifies as interesting traffic.

---

## Transit Devices Require No VPN Awareness

Router R2 was never configured for IPsec.

Despite this:

- VPN traffic passed successfully.
- Communications remained encrypted.
- R2 simply forwarded packets as normal IP traffic.

This highlighted how IPsec functions independently of intermediary devices.

---

# Skills Demonstrated

- Cisco IOS Administration
- VPN Configuration
- IPsec Deployment
- ISAKMP Configuration
- Routing Verification
- Network Troubleshooting
- ACL Design
- Security Policy Implementation

---

# Evidence of Completion

Lab activity successfully completed within Cisco Packet Tracer.

Configuration included:

- IKE Phase 1
- IKE Phase 2
- IPsec Transform Sets
- Crypto Maps
- Tunnel Verification
- Traffic Validation

---

# Key Lessons Learned

- IPsec VPNs rely on both IKE Phase 1 and Phase 2 negotiations.
- Correct configuration sequencing is critical when deploying Cisco VPNs.
- ACLs control which traffic is encrypted.
- Transit routers do not need awareness of VPN operations.
- Verification commands are essential for troubleshooting tunnel establishment.
- Successful VPN deployment requires both connectivity validation and security validation.

---

# Conclusion

This lab provided practical experience configuring a site-to-site IPsec VPN using Cisco IOS CLI. The activity reinforced core networking and cybersecurity concepts including secure communications, encryption, authentication, key exchange, traffic selection, and VPN verification.

Successfully deploying and testing the VPN demonstrated how organizations can securely connect remote networks across untrusted infrastructure while maintaining confidentiality, integrity, and secure routing operations.
