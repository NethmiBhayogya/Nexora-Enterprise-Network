# Day 01 – Network Topology and WAN Configuration

## 1. Overview

Day 1 focused on designing and implementing the physical topology of the Nexora Technologies Multi-Branch Enterprise Network using Cisco Packet Tracer.

The simulated company has three locations:
- Colombo – Head Office
- Kandy – Branch Office
- Galle – Branch Office

## 2. Network Devices

| Device | Quantity |
|---|---:|
| Cisco Routers | 5 |
| Cisco 2960 Switches | 3 |
| PCs | 9 |

### Router Names
- R1-HQ
- R2-KANDY
- R3-GALLE
- ISP1
- ISP2

### Switch Names
- SW1-HQ
- SW2-KANDY
- SW3-GALLE

## 3. Network Design

The network uses two separate WAN connections:

- ISP1 – Primary WAN connection
- ISP2 – Backup WAN connection

Each branch router connects to both ISP routers to provide a foundation for WAN redundancy.

## 4. IP Addressing – WAN Links

All WAN point-to-point links use a `/30` subnet mask (`255.255.255.252`).

| Connection | Device Interface | IP Address |
|---|---|---|
| ISP1 – HQ | ISP1 G0/0 | 10.0.1.5 |
| ISP1 – HQ | R1-HQ G0/1 | 10.0.1.6 |
| ISP1 – Kandy | ISP1 G0/1 | 10.0.1.1 |
| ISP1 – Kandy | R2-KANDY G0/1 | 10.0.1.2 |
| ISP1 – Galle | ISP1 G0/2 | 10.0.1.9 |
| ISP1 – Galle | R3-GALLE G0/1 | 10.0.1.10 |
| ISP2 – Kandy | ISP2 G0/0 | 10.0.2.1 |
| ISP2 – Kandy | R2-KANDY G0/2 | 10.0.2.2 |
| ISP2 – HQ | ISP2 G0/1 | 10.0.2.5 |
| ISP2 – HQ | R1-HQ G0/2 | 10.0.2.6 |
| ISP2 – Galle | ISP2 G0/2 | 10.0.2.9 |
| ISP2 – Galle | R3-GALLE G0/2 | 10.0.2.10 |

## 5. Configuration and Verification

The following tasks were completed:

- Configured IP addresses on ISP1 interfaces.
- Configured IP addresses on ISP2 interfaces.
- Configured primary WAN interfaces on all three branch routers.
- Configured backup WAN interfaces on all three branch routers.
- Enabled the interfaces using `no shutdown`.
- Verified interface status using `show ip interface brief`.

All configured WAN interfaces were verified as `up/up`.

## 6. Day 1 Outcome

The physical network topology and WAN interface addressing were completed successfully.

The primary and backup WAN connections are physically connected and their interfaces are operational.

**Note:** End-to-end routing, automatic WAN failover, VLAN configuration, DHCP and ACL testing are planned for subsequent project stages. These features have not yet been verified.

## 7. Next Steps – Day 2

- Create VLAN 10 – HR.
- Create VLAN 20 – IT.
- Create VLAN 30 – Finance.
- Configure switch access ports.
- Configure trunk links.
- Implement Router-on-a-Stick inter-VLAN routing.
- Verify connectivity between devices and their default gateways.
