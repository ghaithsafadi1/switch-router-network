# Lab: Build a Switch and Router Network

**Tool:** Cisco Packet Tracer  
**Topic:** Router and switch configuration, SSH, end-to-end connectivity  
**Status:** ✅ Completed

---

## Scenario

A small office network with two PCs, one switch (S1), and one router (R1). The goal is to cable the devices, apply a full baseline configuration on both the router and switch, verify connectivity, and secure remote access via SSH.

---

---

## Addressing Table

| Device | Interface | IP Address     | Subnet Mask     | Default Gateway |
|--------|-----------|----------------|-----------------|-----------------|
| R1     | G0/0/0    | 192.168.0.1    | 255.255.255.0   | —               |
| R1     | G0/0/1    | 192.168.1.1    | 255.255.255.0   | —               |
| S1     | VLAN 1    | 192.168.1.2    | 255.255.255.0   | 192.168.1.1     |
| PCA    | NIC       | 192.168.1.10   | 255.255.255.0   | 192.168.1.1     |
| PCB    | NIC       | 192.168.0.10   | 255.255.255.0   | 192.168.0.1     |

> *Replace with your actual values from the lab's Addressing Table.*

---

## Configuration Steps

### 1. Cabling
- R1 G0/0/1 → S1 (Copper Straight-Through)
- PCA → S1 (Copper Straight-Through)
- PCB → R1 G0/0/0 (Copper Straight-Through)

### 2. PC Static Addressing
Configured IPv4 address, subnet mask, and default gateway on both PCA and PCB.

### 3. Router R1 Configuration

```ios
hostname R1

enable secret class

line console 0
 password cisco
 login

service password-encryption

banner motd # Unauthorized access is prohibited. #

interface GigabitEthernet0/0/0
 ip address 192.168.0.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/0/1
 ip address 192.168.1.1 255.255.255.0
 no shutdown

copy running-config startup-config
```

### 4. Switch S1 Configuration

```ios
hostname S1

enable secret class

line console 0
 password cisco
 login

service password-encryption

banner motd # Unauthorized access is prohibited. #

interface vlan 1
 ip address 192.168.1.2 255.255.255.0
 no shutdown

ip default-gateway 192.168.1.1

copy running-config startup-config
```

### 5. SSH Configuration on R1

```ios
ip domain-name academy.net

crypto key generate rsa
! Key size: 1024

username SSHuser secret cisco

line vty 0 4
 login local
 transport input ssh
```

---

## Verification

| Test | Result |
|------|--------|
| PCA → PCB ping (before router config) | ❌ Fail (expected) |
| PCA → PCB ping (after router config)  | ✅ Success |
| SSH from PCA to R1                    | ✅ Success |
---

## Key Concepts Practiced

- Static IPv4 addressing
- Router inter-VLAN routing (separate subnets per interface)
- Switch SVI (VLAN 1) management IP
- IOS security baseline: encrypted passwords, MOTD banner, console login
- SSH setup: domain name, RSA key generation, local user, VTY restriction

---

## Part of My CCST Study Portfolio

This lab is one in a series of Packet Tracer exercises I'm completing as I work toward the CCST certification.

