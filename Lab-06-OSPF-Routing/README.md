# Lab 06 – OSPF Routing

## Overview

This lab demonstrates the configuration and operation of OSPF (Open Shortest Path First) across a multi-router network using Cisco Packet Tracer.

OSPF was configured to dynamically exchange routing information between the routers. An ISP router was also simulated to demonstrate how a default route can be originated and propagated through the OSPF network.

## Lab Topology

The following topology was built in Cisco Packet Tracer.

<!-- Insert topology screenshot here -->

![OSPF Lab Topology](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-06-OSPF-Routing/OSPF-Routing-topolgy.png)

## OSPF Configuration

The routers were configured with OSPF to dynamically advertise their connected networks and establish neighbor relationships.

The configuration included:

* Enabling OSPF routing
* Configuring OSPF router IDs
* Advertising the required networks
* Using wildcard masks
* Establishing OSPF neighbor relationships
* Dynamically learning remote networks

Example:

```text
router ospf 1
 router-id X.X.X.X
 network X.X.X.X X.X.X.X area 0
```

## ISP Simulation & Default Route

**R5** was configured as the ISP router to simulate an external network.

A static default route was configured on **R1** pointing toward R5:

```text
ip route 0.0.0.0 0.0.0.0 10.0.11.2
```

The default route was then originated into OSPF from R1 using:

```text
router ospf 1
 default-information originate
```

This allows R1 to advertise the default route to the other OSPF routers, providing them with a route toward the simulated ISP.

## OSPF Verification

OSPF operation was verified using:

```text
show ip ospf neighbor
show ip ospf interface
show ip route ospf
show ip protocols
```

The routing tables were checked to confirm that OSPF routes and the default route were being learned correctly.

## Connectivity Testing

Connectivity was tested using ICMP ping in Cisco Packet Tracer Simulation Mode.

### Ping Test 1

The first ping successfully reached the destination through the OSPF-routed network.

<!-- Insert first ping screenshot here -->

![OSPF Ping Test 1](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-06-OSPF-Routing/Lab-06-OSPF-Routing-ping1.png)

### Ping Test 2

A second ping was performed between another source and destination to further verify end-to-end connectivity.

<!-- Insert second ping screenshot here -->

![OSPF Ping Test 2](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-06-OSPF-Routing/OSPF-Routing-ping2.png)

## Result

The lab was successfully completed.

* OSPF neighbor adjacencies were established.
* OSPF routes were exchanged dynamically.
* R5 was used to simulate an ISP.
* R1 was configured with a static default route toward R5.
* R1 originated the default route into OSPF using `default-information originate`.
* Other OSPF routers were able to learn the default route.
* End-to-end connectivity was verified using successful ICMP pings in Simulation Mode.

## Key Concepts Practiced

* OSPF
* OSPF neighbor adjacency
* Router IDs
* Wildcard masks
* Dynamic routing
* Default routes
* Static routes
* `default-information originate`
* ISP simulation
* OSPF route advertisement
* Routing table verification
* ICMP connectivity testing
* Cisco Packet Tracer Simulation Mode
