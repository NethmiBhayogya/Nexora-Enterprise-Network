# Day 03 – OSPF Dynamic Routing and WAN Redundancy

## 1. Overview

Day 3 focused on implementing dynamic routing and WAN redundancy for the Nexora Technologies multi-branch enterprise network.

Open Shortest Path First (OSPF) was configured as the primary routing protocol through ISP1. Floating static routes were configured through ISP2 to provide backup connectivity when the primary WAN connection becomes unavailable.

Failover and recovery tests were performed to verify network resilience and automatic route selection.

## 2. Objectives

- Configure OSPF dynamic routing between Colombo, Kandy, and Galle.
- Establish OSPF neighbor relationships.
- Advertise branch LAN networks through OSPF.
- Configure floating static routes through the backup ISP.
- Verify automatic route selection during primary WAN failure.
- Test end-to-end connectivity during failover.
- Verify automatic recovery to the primary WAN connection.

## 3. OSPF Dynamic Routing Configuration

OSPF Process ID 1 was configured across the three branch routers and ISP1.

### Router IDs

| Device | OSPF Router ID |
|---|---|
| R1-HQ | 1.1.1.1 |
| R2-KANDY | 2.2.2.2 |
| R3-GALLE | 3.3.3.3 |
| ISP1 | 4.4.4.4 |

### OSPF Configuration – R1-HQ

```cisco
router ospf 1
router-id 1.1.1.1
passive-interface default
no passive-interface gigabitEthernet 0/1
network 10.0.1.4 0.0.0.3 area 0
network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0
network 192.168.30.0 0.0.0.255 area 0
```

### OSPF Configuration – R2-KANDY

```cisco
router ospf 1
router-id 2.2.2.2
passive-interface default
no passive-interface gigabitEthernet 0/1
network 10.0.1.0 0.0.0.3 area 0
network 192.168.110.0 0.0.0.255 area 0
network 192.168.120.0 0.0.0.255 area 0
network 192.168.130.0 0.0.0.255 area 0
```

### OSPF Configuration – R3-GALLE

```cisco
router ospf 1
router-id 3.3.3.3
passive-interface default
no passive-interface gigabitEthernet 0/1
network 10.0.1.8 0.0.0.3 area 0
network 192.168.210.0 0.0.0.255 area 0
network 192.168.220.0 0.0.0.255 area 0
network 192.168.230.0 0.0.0.255 area 0
```

### OSPF Configuration – ISP1

```cisco
router ospf 1
router-id 4.4.4.4
network 10.0.1.0 0.0.0.3 area 0
network 10.0.1.4 0.0.0.3 area 0
network 10.0.1.8 0.0.0.3 area 0
```

Area 0 was used as the OSPF backbone area. Passive interfaces were configured on the branch routers to advertise LAN networks without forming OSPF neighbor relationships with end devices.

## 4. OSPF Verification

The following commands were used to verify OSPF operation:

```cisco
show ip ospf neighbor
show ip route ospf
show ip protocols
```

OSPF neighbor relationships were established between the branch routers and ISP1.

The routing table confirmed that remote branch networks were learned dynamically through OSPF.

Example route on R1-HQ:

```text
O 192.168.110.0 [110/3] via 10.0.1.5
```

This confirmed that the Kandy HR network was reachable through the primary ISP1 connection.

## 5. WAN Redundancy – Floating Static Routes

ISP1 was designated as the primary WAN connection, while ISP2 was configured as the backup WAN connection.

Floating static routes were configured with an administrative distance of 200.

Since OSPF has an administrative distance of 110, OSPF routes are preferred during normal operation. Floating static routes become active when the corresponding OSPF routes are no longer available.

### Backup Routing Configuration

Static routes were configured on ISP2 to reach all nine departmental LAN networks through the appropriate branch routers.

Floating static routes were also configured on R1-HQ, R2-KANDY, and R3-GALLE to provide backup paths to remote branch networks through ISP2.

Example – R1-HQ:

```cisco
ip route 192.168.110.0 255.255.255.0 10.0.2.5 200
ip route 192.168.120.0 255.255.255.0 10.0.2.5 200
ip route 192.168.130.0 255.255.255.0 10.0.2.5 200

ip route 192.168.210.0 255.255.255.0 10.0.2.5 200
ip route 192.168.220.0 255.255.255.0 10.0.2.5 200
ip route 192.168.230.0 255.255.255.0 10.0.2.5 200
```

Equivalent floating static routes were configured on the Kandy and Galle routers using their respective ISP2 next-hop addresses.

## 6. WAN Failover Testing

To simulate a primary WAN failure, all three ISP1 interfaces were administratively shut down.

```cisco
interface gigabitEthernet 0/0
shutdown

interface gigabitEthernet 0/1
shutdown

interface gigabitEthernet 0/2
shutdown
```

The OSPF routes through ISP1 were withdrawn, allowing the floating static routes through ISP2 to become active.

### Route Verification

On R1-HQ:

```cisco
show ip route 192.168.110.0
```

The route was verified as:

```text
Known via "static", distance 200
* 10.0.2.5
```

This confirmed that the backup route through ISP2 was selected.

### Backup WAN Connectivity Tests

From PC0 (Colombo HR), connectivity was tested to all six departmental PCs in Kandy and Galle.

| Destination | Department | Result |
|---|---|---|
| 192.168.110.10 | Kandy HR | Successful |
| 192.168.120.10 | Kandy IT | Successful |
| 192.168.130.10 | Kandy Finance | Successful |
| 192.168.210.10 | Galle HR | Successful |
| 192.168.220.10 | Galle IT | Successful |
| 192.168.230.10 | Galle Finance | Successful |

**Test summary:**

- Total packets sent: 24
- Total packets received: 24
- Packet loss: 0%

These results confirmed successful end-to-end connectivity through the backup WAN network while ISP1 was unavailable.

## 7. Primary WAN Recovery Testing

After successful failover testing, all three ISP1 interfaces were restored using the `no shutdown` command.

OSPF neighbor relationships were re-established, and the primary OSPF routes were automatically preferred over the floating static routes.

### Verification Commands

```cisco
show ip ospf neighbor
show ip route ospf
show ip route 192.168.110.0
```

The final route verification showed:

```text
Known via "ospf 1", distance 110, metric 3
* 10.0.1.5
```

This confirmed that traffic returned to the primary ISP1 connection after recovery.

## 8. Troubleshooting

### Incorrect Backup WAN Cable Connections

During failover testing, communication between some branch routers and ISP2 initially failed despite the interfaces showing an up/up status.

The issue was traced to incorrectly connected backup WAN cables between the Colombo and Kandy links.

The physical connections were corrected to match the configured IP addressing scheme.

After correcting the connections, connectivity to the ISP2 next-hop addresses was successfully verified.

### OSPF Route Recovery

Following restoration of ISP1, the OSPF neighbor state initially required additional convergence time.

After convergence, the neighbor relationship reached the FULL state and OSPF routes replaced the floating static routes.

## 9. Key Learning Outcomes

- Configuring OSPF dynamic routing in a multi-branch enterprise network.
- Understanding OSPF router IDs, areas, neighbor relationships, and route selection.
- Using passive interfaces to control OSPF neighbor formation.
- Understanding administrative distance and route preference.
- Implementing floating static routes for WAN redundancy.
- Testing primary WAN failure and backup route activation.
- Verifying end-to-end connectivity during failover.
- Troubleshooting physical connectivity and routing issues.
- Understanding automatic recovery to the primary routing path.

## 10. Day 3 Conclusion

Successfully implemented and tested OSPF dynamic routing and WAN redundancy for the Nexora Technologies enterprise network.

The project demonstrated successful failover from ISP1 to ISP2, continued connectivity between all three branch offices during the simulated primary WAN outage, and automatic recovery to the primary OSPF routes after ISP1 was restored.
