# Network Projects Portfolio — Daniel Ntumba

Cybersecurity student at Metropolitan Community College, Omaha, Nebraska  
A.A.S. Cybersecurity | Career Certificate: IT Technician | Graduating May 2026  
Contact: danielntumbatshibangu@gmail.com

---

## About This Repository

This repository contains two network simulation projects built in Cisco Packet Tracer as part of my cybersecurity and networking coursework. Each project includes the `.pkt` simulation file and the running configuration files exported from each device. 
*Specification for all routers and switches: 
- Encrypted privileged EXEC password: class
- Console access password: cisco


---

## Project 1 — Secure LAN: VLAN Segmentation & Layer 2 Hardening

**Folder:** `network-1-secure-LAN/`

### Overview
A fully configured LAN environment built with 1 router, 2 switches, and 2 PCs. The focus of this project was network segmentation, Layer 2 security, and router hardening using industry best practices.

### What Was Configured

**Router (R1)**
- Full security hardening: SSH-only remote access, encrypted passwords, local user database authentication, RSA crypto key generation, MOTD banner, DNS lookup disabled
- Router-on-a-Stick: subinterfaces on G0/0/1 for inter-VLAN routing
- IPv4 DHCP pools for VLAN 2 (ccna-a.net) and VLAN 3 (ccna-b.net) — last 10 addresses per scope
- Default routes for IPv4 and IPv6 directed to Loopback0
- IPv6 routing enabled; static IPv6 GUA and link-local addresses on host PCs
- Loopback0 interface configured

**Both Switches (S1 & S2)**
- VLANs created and named: VLAN 2 (PCs), VLAN 3 (Laptops), VLAN 4 (Management), VLAN 5 (Tablets), VLAN 6 (Native)
- Layer 2 EtherChannel using PAgP protocol (Port-channel 1) across G1/0/1 and G1/0/2
- 802.1Q trunking with Native VLAN 6
- SVI configuration + default gateway assignment
- Port-security on access ports: maximum 2 MAC addresses allowed
- Unused ports secured: assigned to VLAN 5, set to access mode, shutdown

**Switch-specific differences**
- S1: host access port on G1/0/6 → VLAN 2; EtherChannel on Po1 and G1/0/5
- S2: host access port on G1/0/18 → VLAN 3

### Files
| File | Description |
|---|---|
| `secure-lan.pkt` | Full Packet Tracer simulation |
| `router-config.txt` | R1 running configuration |
| `switch1-config.txt` | S1 running configuration |
| `switch2-config.txt` | S2 running configuration |

---

## Project 2 — Enterprise Network: OSPFv2, NAT, ACL & TFTP Backup

**Folder:** `network-2-enterprise/`

### Overview
An enterprise-grade network simulation built with 2 Cisco 4321 routers, 2 Cisco 3650 switches, and 2 PCs. This project covers dynamic routing, address translation, access control, device discovery protocols, network time synchronization, and configuration backup — skills used in real enterprise environments.

### What Was Configured

**Router R1**
- Security hardening: SSH-only VTY access, encrypted passwords, local user database, MOTD banner, DNS lookup disabled, domain name (ccna-lab.com)
- Single-Area OSPFv2 (Router ID 1.1.1.1): advertised networks in Area 0, passive interface on G0/0/1, reference bandwidth set to 1 Gbps, Loopback1 configured as point-to-point for OSPF
- NAT Overload (PAT): ACL 1 matches 192.168.1.0/24 network, translated through G0/0/0 (outside interface)
- NTP Master (stratum 1): acts as the time source for the entire network
- Loopback1: 10.52.0.1/29

**Router R2**
- Security hardening: SSH-only VTY access, encrypted passwords, local user database, MOTD banner
- Single-Area OSPFv2 (Router ID 2.2.2.2): advertised networks in Area 0, passive interfaces on G0/0/1 and Loopback1, `default-information originate` to propagate default route
- Static default route: 0.0.0.0/0 via Loopback1 (simulating ISP connection at 209.165.201.1/27)
- Extended ACL (R2-SECURITY): blocks SSH access to R2's loopback from any source, permits all other IP traffic — applied inbound on G0/0/0
- OSPF DR priority set to 50 on G0/0/0; hello interval tuned to 20 seconds
- NTP Client: synchronized to R1 (10.67.254.2)

**Switches (S1 & S2)**
- Security hardening: SSH-only VTY access, encrypted passwords, local user database, MOTD banner
- SVI (VLAN 1) configured as management interface with default gateway
- S1: management IP 192.168.1.2/24, gateway 192.168.1.1, NTP client to R1
- S2: management IP 10.67.1.2/24, gateway 10.67.1.1, NTP client to R2
- CDP disabled on S1; LLDP enabled on R1 and S1 for neighbor discovery
- Unused ports shutdown for security

**Additional Features**
- TFTP backup: running configurations backed up to TFTP server
- CDP/LLDP: neighbor discovery configured and verified across devices
- NTP: full time synchronization hierarchy — R1 as master, all other devices as clients
- Host PCs: statically assigned IPv4 addresses (PC-A: 192.168.1.50, PC-B: 10.67.1.50)

### Network Addressing
| Device | Interface | IP Address | Subnet Mask |
|---|---|---|---|
| R1 | G0/0/0 | 10.67.254.2 | 255.255.255.252 |
| R1 | G0/0/1 | 192.168.1.1 | 255.255.255.0 |
| R1 | Lo1 | 10.52.0.1 | 255.255.255.248 |
| R2 | G0/0/0 | 10.67.254.1 | 255.255.255.252 |
| R2 | G0/0/1 | 10.67.1.1 | 255.255.255.0 |
| R2 | Lo1 | 209.165.201.1 | 255.255.255.224 |
| S1 | VLAN 1 | 192.168.1.2 | 255.255.255.0 |
| S2 | VLAN 1 | 10.67.1.2 | 255.255.255.0 |

### Files
| File | Description |
|---|---|
| `enterprise-network.pkt` | Full Packet Tracer simulation |
| `R1-config.txt` | R1 running configuration |
| `R2-config.txt` | R2 running configuration |
| `S1-config.txt` | S1 running configuration |
| `S2-config.txt` | S2 running configuration |

---

## Tools & Technologies
Cisco Packet Tracer · Cisco IOS · OSPFv2 · NAT/PAT · ACL · VLANs · EtherChannel (PAgP) · DHCP · SSH · NTP · TFTP · CDP · LLDP · IPv4 · IPv6

---

## Security Capstone (In Progress — May 2026)
Currently building a standalone cybersecurity lab environment featuring pfSense firewall, 2 servers, and 2 host machines. Vulnerability assessment using Nessus, Nmap, and Wireshark. Full documentation and findings report to be added upon completion.
