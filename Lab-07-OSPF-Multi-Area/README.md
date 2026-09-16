# OSPF Multi-Area Network Topology & Routing Lab

Cisco Packet Tracer lab implementing a **multi-area OSPF network** with four core routers, five OSPF areas, departmental LANs, and an external ISP connection.

## Network Overview

The topology uses **Area 0 as the OSPF backbone**, connecting four departmental areas through Area Border Routers (ABRs).

* **Area 0:** Backbone / Core
* **Area 1:** Sales — `192.168.1.0/24`
* **Area 2:** Engineering — `192.168.2.0/24`
* **Area 3:** Management — `192.168.3.0/24`
* **Area 4:** Services — `192.168.4.0/24`

The core consists of **R5, R6, R7, and R8**, with R5 also providing the connection to the external ISP router **R9**.

## Topology

![OSPF Multi-Area Topology](screenshots/topology.png)

### Main Router Links

| Network        | Purpose            |
| -------------- | ------------------ |
| `10.0.6.0/30`  | Core link          |
| `10.0.7.0/30`  | Core link          |
| `10.0.8.0/30`  | Core link          |
| `10.0.9.0/30`  | Core link          |
| `10.0.2.0/30`  | Sales uplink       |
| `10.0.3.0/30`  | Engineering uplink |
| `10.0.4.0/30`  | Management uplink  |
| `10.0.5.0/30`  | Services uplink    |
| `10.0.10.0/30` | ISP connection     |

## OSPF Configuration

OSPF is configured across all five areas, with ABRs connecting the departmental areas to Area 0.

Example ABR configuration:

```cisco
router ospf 1
 network 10.0.8.0 0.0.0.3 area 0
 network 10.0.9.0 0.0.0.3 area 0
 network 10.0.4.0 0.0.0.3 area 3
```

### Default Route

R5 acts as the edge router toward the ISP.

```cisco
ip route 0.0.0.0 0.0.0.0 10.0.10.2

router ospf 1
 default-information originate
```

This advertises the default route through OSPF so internal networks can reach external destinations through R5.

## Verification

The following commands were used to verify the OSPF operation:

### OSPF Neighbors

```cisco
show ip ospf neighbor
```

![OSPF Neighbors](screenshots/ospf-neighbors.png)

### OSPF Interfaces

```cisco
show ip ospf interface brief
```

![OSPF Interfaces](screenshots/ospf-interfaces.png)

### OSPF Routes

```cisco
show ip route ospf
```

![OSPF Routes](screenshots/ospf-routes.png)

Inter-area routes are identified by **`O IA`** in the routing table.

### End-to-End Connectivity

A connectivity test was performed between hosts in different OSPF areas.

**PC4 (Area 3) → PC1 (Area 1)**

```text
ping 192.168.1.10
```

![Successful Inter-Area Ping](screenshots/inter-area-ping.png)

## Skills Demonstrated

* Multi-area OSPF
* OSPF Area 0 / Backbone
* Area Border Routers (ABRs)
* Inter-area routing
* Default-route propagation
* IPv4 subnetting
* Point-to-point router links
* OSPF troubleshooting and verification
* End-to-end connectivity testing

## Packet Tracer File

The complete topology and configurations are included in the `.pkt` file.
