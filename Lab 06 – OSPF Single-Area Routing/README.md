# Lab 06 – OSPF Routing

## Overview

This lab demonstrates the configuration of OSPF (Open Shortest Path First) across a multi-router network using Cisco Packet Tracer.

OSPF was configured on the routers to dynamically advertise the connected networks and establish routing between the different parts of the topology.

## Lab Topology

The following topology was built in Cisco Packet Tracer.

<!-- Insert topology screenshot here -->

![OSPF Lab Topology](images/ospf-topology.png)

## Configuration

The routers were configured with OSPF and assigned to the appropriate OSPF area.

The configuration included:

* Enabling OSPF routing
* Configuring OSPF router IDs
* Advertising the required networks
* Using wildcard masks to match the appropriate interfaces
* Establishing OSPF neighbor relationships
* Allowing routers to dynamically learn remote networks

Example OSPF configuration:

```text
router ospf 1
 router-id X.X.X.X
 network X.X.X.X X.X.X.X area 0
```

## OSPF Verification

OSPF neighbor relationships and routing information were verified using Cisco IOS commands such as:

```text
show ip ospf neighbor
show ip ospf interface
show ip route ospf
show ip protocols
```

## Connectivity Testing

Connectivity was tested using ICMP ping in Cisco Packet Tracer Simulation Mode.

### Ping Test 1

The first ping successfully reached the destination through the OSPF-routed network.

<!-- Insert first ping screenshot here -->

![OSPF Ping Test 1](images/ospf-ping-1.png)

### Ping Test 2

A second ping was performed between another source and destination to further verify end-to-end connectivity.

<!-- Insert second ping screenshot here -->

![OSPF Ping Test 2](images/ospf-ping-2.png)

## Result

The OSPF configuration was successful.

* OSPF neighbor adjacencies were established.
* Routes were exchanged dynamically between routers.
* Remote networks were successfully learned through OSPF.
* ICMP packets successfully reached their destinations.
* End-to-end connectivity was verified in Simulation Mode.

## Key Concepts Practiced

* OSPF
* Dynamic routing
* OSPF areas
* Router IDs
* Wildcard masks
* OSPF neighbor adjacency
* OSPF route advertisement
* OSPF route verification
* ICMP troubleshooting
* Cisco Packet Tracer Simulation Mode
