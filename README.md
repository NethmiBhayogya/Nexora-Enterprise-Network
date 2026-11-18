# Nexora Technologies – Resilient Multi-Branch Enterprise Network

A multi-branch enterprise network designed and simulated using **Cisco Packet Tracer**, demonstrating network infrastructure design, VLAN segmentation, dynamic routing, WAN redundancy, DHCP, and network access control.

## Project Overview

This project simulates the network infrastructure of **Nexora Technologies**, a fictional company operating across three locations in Sri Lanka:

- **Colombo** – Head Office
- **Kandy** – Branch Office
- **Galle** – Branch Office

The network was designed to provide reliable communication between departments and branch offices while incorporating redundant WAN connectivity and basic network security policies.

## Project Objectives

- Design and implement a multi-branch enterprise network topology.
- Implement VLAN-based departmental network segmentation.
- Configure inter-VLAN routing using Router-on-a-Stick.
- Establish communication between geographically separated branches.
- Implement dynamic routing using OSPF.
- Configure primary and backup WAN connectivity.
- Implement WAN failover using floating static routes.
- Configure DHCP for automatic IP address allocation.
- Apply Extended Access Control Lists (ACLs) to control interdepartmental traffic.
- Perform connectivity, routing, and WAN failover verification.

## Network Topology

The network consists of three branch locations connected through two simulated ISP routers.

**WAN Design:**

- **ISP1:** Primary WAN connection using OSPF.
- **ISP2:** Backup WAN connection using static routing and floating static routes.

Each branch router connects to both ISP routers, providing redundant WAN paths between the branch offices.

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

Each branch is divided into three departments using separate VLANs and IP subnets.

| Department | VLAN ID | Colombo | Kandy | Galle |
|---|---:|---|---|---|
| HR | 10 | 192.168.10.0/24 | 192.168.110.0/24 | 192.168.210.0/24 |
| IT | 20 | 192.168.20.0/24 | 192.168.120.0/24 | 192.168.220.0/24 |
| Finance | 30 | 192.168.30.0/24 | 192.168.130.0/24 | 192.168.230.0/24 |

Each department uses the first usable IP address in its subnet as the default gateway.

## Technologies and Networking Concepts

| Technology | Purpose |
|---|---|
| Cisco Packet Tracer | Network design and simulation |
| VLANs | Departmental network segmentation |
| IEEE 802.1Q | VLAN trunking |
| Router-on-a-Stick | Inter-VLAN routing |
| OSPF | Dynamic routing between branches |
| Static Routing | Backup path configuration |
| Floating Static Routes | WAN failover |
| DHCP | Automatic IP address allocation |
| Extended ACLs | Traffic filtering and access control |

## Implementation Progress

| Project Stage | Implementation | Status |
|---|---|---|
| Day 1 | Physical topology and WAN interface configuration | Completed |
| Day 2 | VLANs, trunking and inter-VLAN routing | Completed |
| Day 3 | OSPF, floating static routes and WAN failover | Completed |
| Day 4 | DHCP, ACL configuration and final verification | Completed |

### Key Implementation and Testing Results

- Configured three departmental VLANs across all branch offices.
- Implemented Router-on-a-Stick inter-VLAN routing.
- Established OSPF routing through the primary ISP.
- Configured floating static routes through the backup ISP.
- Verified backup WAN connectivity by successfully pinging all six remote branch PCs from Colombo during the primary WAN outage (24/24 packets received).
- Restored the primary WAN and verified OSPF neighbor formation and route selection.
- Configured DHCP pools for all nine departmental networks and verified automatic IP assignment on all nine PCs.
- Applied extended ACLs to restrict HR-to-Finance and Finance-to-HR traffic.


## Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- GitHub

## Project Deliverables

- Cisco Packet Tracer network simulation files.
- Multi-branch network topology.
- IP addressing and VLAN documentation.
- Router and switch configuration documentation.
- DHCP and ACL implementation.
- WAN failover and routing verification evidence.
- Project documentation and screenshots.

## Learning Outcomes

Through this project, I gained practical experience in:

- Designing and implementing a multi-branch enterprise network.
- Configuring Cisco routers and switches using IOS commands.
- Implementing VLANs, trunking and inter-VLAN routing.
- Configuring and troubleshooting OSPF.
- Implementing WAN redundancy using floating static routes.
- Configuring DHCP and extended ACLs.
- Performing network connectivity and failover testing.
- Documenting network configurations and troubleshooting activities using GitHub.

## Author

**Nethmi Bhayogya**

Bachelor of ICT (Hons) – Specialized in Networking Technology  
Faculty of Technology  
University of Sri Jayewardenepura

---
