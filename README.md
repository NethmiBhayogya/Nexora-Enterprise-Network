# Nexora Technologies – Resilient Multi-Branch Enterprise Network

A multi-branch enterprise network designed and simulated using **Cisco Packet Tracer**, focusing on network infrastructure design, VLAN segmentation, dynamic routing, WAN redundancy, and network security.

## Project Overview

This project simulates the network infrastructure of **Nexora Technologies**, a fictional company operating across three locations in Sri Lanka:

- Colombo – Head Office
- Kandy – Branch Office
- Galle – Branch Office

The objective is to design a scalable, reliable, and secure enterprise network that enables communication between departments and branch offices while providing redundant WAN connectivity.

## Project Objectives

- Design a multi-branch enterprise network topology.
- Implement VLAN-based network segmentation.
- Configure inter-VLAN routing using Router-on-a-Stick.
- Establish communication between geographically separated branches.
- Implement dynamic routing using OSPF.
- Design primary and backup WAN connectivity.
- Configure automatic WAN failover.
- Implement DHCP and Access Control Lists (ACLs).
- Test network connectivity, redundancy, and security.

## Network Topology

The network consists of three branch locations connected through two simulated ISP routers.

**WAN Design:**
- ISP1 – Primary WAN connection.
- ISP2 – Backup WAN connection.

Each branch router has a connection to both ISP routers, providing the foundation for redundant WAN connectivity.

<!-- Add your Packet Tracer topology screenshot here -->
<!-- Example: ![Network Topology](Screenshots/Topology/network-topology.png) -->

## Network Infrastructure

| Device | Quantity |
|---|---:|
| Cisco Routers | 5 |
| Cisco 2960 Switches | 3 |
| End Devices (PCs) | 9 |
| Simulated ISP Routers | 2 |

### Router Configuration Names
- R1-HQ
- R2-KANDY
- R3-GALLE
- ISP1
- ISP2

### Switch Configuration Names
- SW1-HQ
- SW2-KANDY
- SW3-GALLE

## VLAN and IP Addressing Plan

Each branch is designed with three departments.

| Department | VLAN ID | Colombo | Kandy | Galle |
|---|---:|---|---|---|
| HR | 10 | 192.168.10.0/24 | 192.168.110.0/24 | 192.168.210.0/24 |
| IT | 20 | 192.168.20.0/24 | 192.168.120.0/24 | 192.168.220.0/24 |
| Finance | 30 | 192.168.30.0/24 | 192.168.130.0/24 | 192.168.230.0/24 |

Each department will use the first usable IP address in its subnet as its default gateway.

## Technologies and Concepts

| Technology | Purpose |
|---|---|
| Cisco Packet Tracer | Network simulation |
| VLANs | Departmental network segmentation |
| IEEE 802.1Q | VLAN trunking |
| Router-on-a-Stick | Inter-VLAN routing |
| OSPF | Dynamic routing |
| Static Routing | Backup path selection |
| Floating Static Routes | WAN failover |
| DHCP | Automatic IP allocation |
| ACLs | Traffic filtering and access control |

## Implementation Progress

| Project Stage | Tasks | Status |
|---|---|---|
| Day 1 | Physical topology and WAN interface configuration | Completed |
| Day 2 | VLANs, trunking and inter-VLAN routing | Planned |
| Day 3 | OSPF, WAN failover, DHCP and ACLs | Planned |
| Day 4 | Connectivity testing, documentation and verification | Planned |

## Repository Structure

```text
Nexora-Enterprise-Network/
│
├── README.md
│
├── Packet-Tracer/
│   └── Nexora_Enterprise_Network.pkt
│
├── Documentation/
│   ├── Day-01/
│   │   └── DAY-01.md
│   ├── Day-02/
│   ├── Day-03/
│   └── Day-04/
│
└── Screenshots/
    ├── Topology/
    ├── Configuration/
    └── Testing/
```

## Tools Used

- Cisco Packet Tracer
- GitHub
- Cisco IOS CLI

## Project Deliverables

- Cisco Packet Tracer network simulation file.
- Network topology diagram.
- IP addressing documentation.
- Router and switch configuration documentation.
- Connectivity and failover testing evidence.

## Learning Outcomes

Through this project, I aim to develop practical experience in:

- Enterprise network design and implementation.
- Cisco router and switch configuration.
- Network segmentation and routing.
- WAN redundancy and fault tolerance.
- Network troubleshooting and verification.
- Technical documentation using GitHub.

## Author

**Nethmi Bhayogya**

Bachelor of ICT (Hons) – Specialized in Networking Technology  
Faculty of Technology  
University of Sri Jayewardenepura

---

*This project is developed as a practical networking portfolio project to strengthen enterprise networking and network administration skills.*
