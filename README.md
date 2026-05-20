# VLAN Design

## Overview
Designed and implemented a VLAN-segmented network for a multi-department organization (CS, EE, Math) using Cisco Packet Tracer. Configured trunking between switches and inter-VLAN routing through a router, then traced ARP and ICMP traffic to understand frame-level behavior.

## Tools Used
- Cisco Packet Tracer
- Cisco 3650-24PS Multilayer Switches (Switch 0 and Switch 1)
- Cisco ISR4331 Router
- PC and Server end hosts

## Network Topology
```
                        Router0
                       /       \
              Switch0           Switch1
             /       \         /       \
          CS(VLAN10) EE(VLAN20) CS(VLAN10) Math(VLAN30)
          PC0,PC1   PC2,Laptop0 Server0,PC3  PC4,PC5,PC6
```

## Subnetting
Each department maps to one VLAN and one /24 subnet carved from `10.z.0.0/16` (~200 hosts each):

| Department | VLAN ID | Subnet         |
|------------|---------|----------------|
| CS         | 10      | 10.z.0.0/24    |
| EE         | 20      | 10.z.1.0/24    |
| Math       | 30      | 10.z.2.0/24    |

*(z derived from student ID)*

## What I Did

### Part 1: VLAN and Trunk Configuration (no router)
- Assigned access ports on each switch to the appropriate VLAN for each department
- Configured an 802.1Q trunk link between Switch 0 and Switch 1 to carry CS department traffic across switches (one trunk link handles all tagged CS frames)
- Verified intra-department connectivity with ICMP PDUs between hosts on the same VLAN, including cross-switch pings
- Analyzed when VLAN tags are added and stripped: tags present only on trunk links, stripped at access ports before delivery to end hosts

### Part 2: Inter-VLAN Routing
- Connected Router0 to both switches using router-on-a-stick (subinterfaces per VLAN with 802.1Q encapsulation)
- Configured each subinterface as the default gateway for its respective subnet
- Verified the following ping paths:
  - CS ↔ CS (same switch)
  - CS ↔ CS (different switches)
  - CS (Switch 0) ↔ EE
  - CS (Switch 0) ↔ Math
- Traced all ARP and ICMP packets for each scenario, identifying which pings triggered new ARP requests and which reused cached entries
- Identified cases where a PDU crosses a router interface twice (router-on-a-stick behavior) and explained why this is correct

## Key Concepts Demonstrated
- VLAN creation and access port assignment
- 802.1Q trunking between switches
- Router-on-a-stick inter-VLAN routing with subinterfaces
- VLAN tag lifecycle (added on trunk, stripped at access port)
- ARP behavior within and across VLANs
- Relationship between subnets and VLANs (1:1 mapping)

## Files
- `\.pkt` — Packet Tracer file with completed VLAN and routing configuration
