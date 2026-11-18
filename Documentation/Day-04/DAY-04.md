# Day 04 – DHCP, Access Control Lists & Final Verification

## 1. Overview

Day 04 focused on implementing dynamic IP address allocation, improving departmental network security, and verifying the recovery of the primary WAN connection in the Nexora Technologies multi-branch enterprise network.

The configurations were implemented across the Colombo, Kandy, and Galle branches using Cisco Packet Tracer.

## 2. Objectives

- Configure DHCP on all three branch routers.
- Automate IP address allocation for all nine PCs.
- Implement Extended Named Access Control Lists (ACLs).
- Restrict communication between HR and Finance departments.
- Allow IT department communication with other departments.
- Verify OSPF operation and primary WAN recovery.

## 3. DHCP Configuration

Configured DHCP services on all three branch routers:

- R1-HQ – Colombo
- R2-KANDY – Kandy
- R3-GALLE – Galle

Created three DHCP pools per router to provide IP configurations for the HR, IT, and Finance VLANs.

### DHCP Pools

| Branch | Department | Network | Default Gateway |
|---|---|---|---|
| Colombo | HR | 192.168.10.0/24 | 192.168.10.1 |
| Colombo | IT | 192.168.20.0/24 | 192.168.20.1 |
| Colombo | Finance | 192.168.30.0/24 | 192.168.30.1 |
| Kandy | HR | 192.168.110.0/24 | 192.168.110.1 |
| Kandy | IT | 192.168.120.0/24 | 192.168.120.1 |
| Kandy | Finance | 192.168.130.0/24 | 192.168.130.1 |
| Galle | HR | 192.168.210.0/24 | 192.168.210.1 |
| Galle | IT | 192.168.220.0/24 | 192.168.220.1 |
| Galle | Finance | 192.168.230.0/24 | 192.168.230.1 |

### DHCP Configuration Commands

Example configuration on R1-HQ:

```cisco
ip dhcp excluded-address 192.168.10.1 192.168.10.20

ip dhcp pool HQ-HR
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
```

Similar DHCP pools were configured for the remaining VLANs across all three branches.

### DHCP Verification

Used the following commands:

```cisco
show ip dhcp pool
show ip dhcp binding
```

All nine PCs were successfully configured to obtain their IP addresses automatically through DHCP.

## 4. Network Security – Access Control Lists

Implemented Extended Named ACLs to control communication between departmental VLANs.

### Security Policy

| Source Department | Destination Department | Policy |
|---|---|---|
| HR | Finance | Deny |
| Finance | HR | Deny |
| HR | IT | Permit |
| IT | HR | Permit |
| IT | Finance | Permit |
| Finance | IT | Permit |

The same departmental access policy was implemented across Colombo, Kandy, and Galle.

### ACL Configuration

Example ACL configuration:

```cisco
ip access-list extended HQ-HR-IN
deny ip 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255
deny ip 192.168.10.0 0.0.0.255 192.168.130.0 0.0.0.255
deny ip 192.168.10.0 0.0.0.255 192.168.230.0 0.0.0.255
permit ip any any
```

ACLs were applied inbound on the HR and Finance router subinterfaces.

### ACL Verification

Used:

```cisco
show access-lists
```

Connectivity tests were performed to verify permitted and restricted communication between departments.

## 5. OSPF Verification

Verified the OSPF neighbor relationship between R1-HQ and ISP1.

Command:

```cisco
show ip ospf neighbor
```

Result:

- Neighbor Router ID: 4.4.4.4
- Neighbor State: FULL/DR
- Neighbor Address: 10.0.1.5
- Interface: GigabitEthernet0/1

The OSPF adjacency was successfully established.

## 6. Primary WAN Recovery Verification

During the final verification, the primary ISP1 interfaces were restored after the WAN failover test.

OSPF successfully re-established its neighbor relationship and the primary routing path became active again.

Verification command:

```cisco
show ip route 192.168.110.0
```

Verified routing information:

- Network: 192.168.110.0/24
- Routing Protocol: OSPF
- Administrative Distance: 110
- Next Hop: 10.0.1.5
- Outgoing Interface: GigabitEthernet0/1

This confirmed that the OSPF route through ISP1 replaced the floating static backup route through ISP2, which has an administrative distance of 200.

## 7. Troubleshooting

### OSPF Recovery

During primary WAN restoration, the branch router initially displayed a `2WAY/DROTHER` neighbor state.

After allowing time for OSPF convergence, the neighbor relationship reached `FULL/DR`.

The routing table was subsequently verified to confirm that the primary OSPF route was active again.

### WAN Interface Status

The ISP1 interfaces were initially administratively down following the failover test.

The interfaces were restored using the `no shutdown` command, allowing OSPF adjacencies and primary routing to recover.

## 8. Learning Outcomes

Through this implementation, I gained practical experience in:

- Configuring DHCP pools across multiple VLANs.
- Automating IP address allocation in an enterprise network.
- Implementing Extended Named ACLs.
- Applying departmental network access policies.
- Verifying OSPF neighbor relationships and routing tables.
- Troubleshooting WAN connectivity and routing convergence.
- Understanding primary and backup WAN route selection.

## 9. Conclusion

Completed the implementation of DHCP and departmental access control policies across the three branches of the Nexora Technologies enterprise network.

Verified successful DHCP address allocation for all nine PCs, tested departmental access restrictions, and confirmed recovery of the primary OSPF routing path after WAN restoration.

The project demonstrates practical implementation of enterprise network segmentation, dynamic routing, WAN redundancy, automated IP addressing, and basic network security using Cisco Packet Tracer.
