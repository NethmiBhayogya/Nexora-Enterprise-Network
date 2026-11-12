# Day 02 – VLAN Configuration and Inter-VLAN Routing

## 1. Overview

Day 2 focused on implementing departmental network segmentation and inter-VLAN communication across the three branches of Nexora Technologies: Colombo (Head Office), Kandy, and Galle.

The implementation involved configuring VLANs, assigning switch ports, establishing IEEE 802.1Q trunk links, implementing Router-on-a-Stick inter-VLAN routing, and performing connectivity tests.

## 2. Objectives

- Create departmental VLANs on all three branch switches.
- Assign switch access ports to the appropriate VLANs.
- Configure IEEE 802.1Q trunk links between switches and routers.
- Implement Router-on-a-Stick inter-VLAN routing.
- Configure static IP addresses and default gateways for all nine PCs.
- Verify gateway connectivity and inter-VLAN communication.

## 3. VLAN Configuration

Three departmental VLANs were created on each branch switch.

| VLAN ID | Department | Purpose |
|---|---|---|
| VLAN 10 | HR | Human Resources network |
| VLAN 20 | IT | Information Technology network |
| VLAN 30 | Finance | Finance department network |

The same VLAN IDs were used across all three branches to maintain a consistent network design.

### Switch Port Assignments

The following access port assignments were configured on each branch switch:

| Switch Port | Connected Device | VLAN |
|---|---|---|
| FastEthernet 0/2 | HR PC | VLAN 10 |
| FastEthernet 0/3 | IT PC | VLAN 20 |
| FastEthernet 0/4 | Finance PC | VLAN 30 |

### VLAN Creation Commands

The following commands were used on the Cisco 2960 switches:

```cisco
enable
configure terminal

vlan 10
name HR
exit

vlan 20
name IT
exit

vlan 30
name Finance
exit
```

Access ports were assigned to their respective VLANs.

Example:

```cisco
interface fastEthernet 0/2
switchport mode access
switchport access vlan 10
exit
```

The same process was followed for the remaining departmental ports and branch switches.

## 4. IEEE 802.1Q Trunk Configuration

Trunk links were configured between each branch switch and its corresponding router.

| Branch | Switch | Router | Connection |
|---|---|---|---|
| Colombo | SW1-HQ | R1-HQ | Gi0/1 ↔ Gi0/0 |
| Kandy | SW2-KANDY | R2-KANDY | Gi0/1 ↔ Gi0/0 |
| Galle | SW3-GALLE | R3-GALLE | Gi0/1 ↔ Gi0/0 |

The following configuration was applied to the switch trunk ports:

```cisco
enable
configure terminal

interface gigabitEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
no shutdown

end
```

IEEE 802.1Q trunking allows traffic from multiple VLANs to travel through a single physical connection between a switch and a router.

### Trunk Verification

The following command was used to verify trunk operation:

```cisco
show interfaces trunk
```

All three branch switches successfully displayed:

- 802.1Q trunk encapsulation.
- Operational trunking status.
- VLANs 10, 20 and 30 allowed on the trunk.
- VLANs 10, 20 and 30 active and forwarding.

## 5. Router-on-a-Stick Configuration

Inter-VLAN routing was implemented using the Router-on-a-Stick method.

Each branch router used one physical GigabitEthernet interface connected to its branch switch. Three logical subinterfaces were created to handle traffic from the three departmental VLANs.

### Colombo – R1-HQ

| Subinterface | VLAN | IP Address |
|---|---|---|
| G0/0.10 | VLAN 10 | 192.168.10.1 |
| G0/0.20 | VLAN 20 | 192.168.20.1 |
| G0/0.30 | VLAN 30 | 192.168.30.1 |

### Kandy – R2-KANDY

| Subinterface | VLAN | IP Address |
|---|---|---|
| G0/0.10 | VLAN 10 | 192.168.110.1 |
| G0/0.20 | VLAN 20 | 192.168.120.1 |
| G0/0.30 | VLAN 30 | 192.168.130.1 |

### Galle – R3-GALLE

| Subinterface | VLAN | IP Address |
|---|---|---|
| G0/0.10 | VLAN 10 | 192.168.210.1 |
| G0/0.20 | VLAN 20 | 192.168.220.1 |
| G0/0.30 | VLAN 30 | 192.168.230.1 |

### Example Router Configuration

The following configuration was applied to the Colombo router:

```cisco
enable
configure terminal

interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

interface gigabitEthernet 0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit

end
copy running-config startup-config
```

Equivalent configurations were implemented on R2-KANDY and R3-GALLE using their respective gateway addresses.

### Router Interface Verification

The following command was used:

```cisco
show ip interface brief
```

All nine subinterfaces were verified with their correct IP addresses and operational status of `up/up`.

## 6. End-Device IP Address Configuration

Static IPv4 addresses were assigned to all nine PCs through the Desktop → IP Configuration interface in Cisco Packet Tracer.

### Colombo – Head Office

| PC | Department | IP Address | Default Gateway |
|---|---|---|---|
| PC0 | HR | 192.168.10.10 | 192.168.10.1 |
| PC1 | IT | 192.168.20.10 | 192.168.20.1 |
| PC2 | Finance | 192.168.30.10 | 192.168.30.1 |

### Kandy – Branch Office

| PC | Department | IP Address | Default Gateway |
|---|---|---|---|
| PC3 | HR | 192.168.110.10 | 192.168.110.1 |
| PC4 | IT | 192.168.120.10 | 192.168.120.1 |
| PC5 | Finance | 192.168.130.10 | 192.168.130.1 |

### Galle – Branch Office

| PC | Department | IP Address | Default Gateway |
|---|---|---|---|
| PC6 | HR | 192.168.210.10 | 192.168.210.1 |
| PC7 | IT | 192.168.220.10 | 192.168.220.1 |
| PC8 | Finance | 192.168.230.10 | 192.168.230.1 |

Subnet mask for all PCs: `255.255.255.0`.

## 7. Connectivity Testing and Verification

Connectivity tests were performed using the `ping` command from the PC Command Prompt in Cisco Packet Tracer.

The following tests were conducted:

- Communication between each PC and its respective default gateway.
- Communication between different departmental VLANs within each branch.
- Verification of HR-to-IT and HR-to-Finance communication at the three branches.

### Example Testing Commands

Colombo:

```bash
ping 192.168.10.1
ping 192.168.20.10
ping 192.168.30.10
```

Kandy:

```bash
ping 192.168.110.1
ping 192.168.120.10
ping 192.168.130.10
```

Galle:

```bash
ping 192.168.210.1
ping 192.168.220.10
ping 192.168.230.10
```

The tests confirmed successful gateway connectivity and inter-VLAN communication within the three branches. Some initial ping attempts experienced packet loss, but repeated tests returned successful replies.

## 8. Challenges and Troubleshooting

### Trunk Interface Initially Not Operational

During trunk configuration, the `show interfaces trunk` command initially displayed no active trunk ports on SW1-HQ.

Further investigation using:

```cisco
show interfaces gigabitEthernet 0/1 switchport
```

showed that the switch port was configured for trunking but its operational status was down.

The issue was traced to the router's connected GigabitEthernet 0/0 interface being administratively down.

The issue was resolved by enabling the router interface:

```cisco
configure terminal
interface gigabitEthernet 0/0
no shutdown
```

The trunk was subsequently verified as operational.

## 9. Outcome

Successfully implemented departmental VLAN segmentation and Router-on-a-Stick inter-VLAN routing across the Colombo, Kandy and Galle branches.

All three branch switches were configured with operational IEEE 802.1Q trunk links. Nine PCs were assigned static IPv4 configurations, and gateway connectivity and inter-VLAN communication were verified through ping testing.

The network is now prepared for the next implementation stage, which will focus on dynamic routing, inter-branch connectivity, WAN redundancy and additional network services.

## 10. Technologies and Concepts Practiced

- Cisco Packet Tracer
- Cisco IOS CLI
- VLAN Configuration
- IEEE 802.1Q Trunking
- IPv4 Addressing and Subnetting
- Router-on-a-Stick
- Inter-VLAN Routing
- Static IP Configuration
- ICMP Ping Testing
- Network Troubleshooting
