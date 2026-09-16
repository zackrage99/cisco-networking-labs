# Lab 07 — OSPF Multi-Area Routing

This lab demonstrates a **multi-area OSPF network** in Cisco Packet Tracer, using four backbone routers and five OSPF areas.

## Network Topology

The network consists of:

* **4 backbone routers:** R5, R6, R7, R8
* **5 OSPF areas:** Area 0, Area 1, Area 2, Area 3, Area 4
* Multiple departmental LANs
* Area Border Routers (ABRs)
* An external ISP connection through R9

### Topology

![Network Topology](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-07-OSPF-Multi-Area/topology.png)

## OSPF Area Design

| Area   | Network / Department |
| ------ | -------------------- |
| Area 0 | OSPF Backbone        |
| Area 1 | Sales                |
| Area 2 | Engineering          |
| Area 3 | Management           |
| Area 4 | Services             |

The backbone routers provide connectivity between the different OSPF areas through the ABRs.

## OSPF Verification

### OSPF Neighbors

The OSPF neighbor table was checked on a backbone router to verify that OSPF adjacencies were successfully established.

```cisco
show ip ospf neighbor
```

![OSPF Neighbors](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-07-OSPF-Multi-Area/ospf-neighbors.png)

### OSPF Interfaces

OSPF-enabled interfaces and their associated areas were verified using:

```cisco
show ip ospf interface brief
```

![OSPF Interface Brief](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-07-OSPF-Multi-Area/ospf-interface-brief.png)

### ABR Verification

The backbone router connecting Area 0 to another OSPF area was verified as an **Area Border Router (ABR)**.

```cisco
show ip protocols
```

![ABR Verification](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-07-OSPF-Multi-Area/ospf-abr.png)

## Key Concepts Demonstrated

* Multi-area OSPF
* OSPF Area 0 backbone
* Area Border Routers (ABRs)
* OSPF neighbor adjacencies
* Inter-area routing
* IPv4 subnetting
* Point-to-point router links
* OSPF verification and troubleshooting

## Lab File

The complete Cisco Packet Tracer topology and configuration are included in the `.pkt` file.
