# RIP and EIGRP Route Redistribution Lab

## Overview

In this lab, I built a network consisting of **8 routers**, divided into two routing domains.

Each routing domain contains **4 routers running RIPv2** for internal route exchange. Two routers from each domain participate in **EIGRP**, creating the connection between the two routing domains.

To connect the EIGRP routers, I used **single-mode fiber (SMF)** with the **HWIC-1GE-SFP module** and **GLC-LH-SMD transceiver** in Cisco Packet Tracer.

The topology includes labeled IP addresses, interfaces, routing protocols, and network connections.

![Lab Topology](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-05-RIP-EIGRP-Route-Redistribution/RIP-EIGRP-Route-Redistribution-topology.png)

---

## Network Design

* **8 routers** in total
* **4 routers in the first routing domain** running RIPv2
* **4 routers in the second routing domain** running RIPv2
* **2 routers from each domain** participate in EIGRP
* **RIPv2** is used for internal routing within each domain
* **EIGRP** is used to connect the two routing domains
* **Route redistribution** is used to allow RIP routes to be advertised into EIGRP
* `/30` subnets are used for the point-to-point EIGRP connections

---

## RIP Configuration

RIP version 2 was configured on the routers within each routing domain:

```cisco
router rip
 version 2
 no auto-summary
 network 10.0.0.0
```

### Configuration Explanation

* `version 2` enables RIPv2.
* `no auto-summary` disables automatic classful route summarization.
* `network 10.0.0.0` enables RIP on interfaces belonging to the specified network.

---

## EIGRP Configuration

EIGRP autonomous system **1** was configured between the routers connecting the two routing domains.

For the first `/30` connection:

```cisco
router eigrp 1
 no auto-summary
 network 172.16.0.0 0.0.0.3
```

The neighboring router uses the same network statement because both interfaces belong to the same `/30` subnet:

```cisco
router eigrp 1
 no auto-summary
 network 172.16.0.0 0.0.0.3
```

The second EIGRP connection uses the `172.17.0.0/30` network:

```cisco
router eigrp 1
 no auto-summary
 network 172.17.0.0 0.0.0.3
```

The opposite router uses the same `/30` network statement:

```cisco
router eigrp 1
 no auto-summary
 network 172.17.0.0 0.0.0.3
```

### Wildcard Masks

The EIGRP network statements use wildcard masks to match the `/30` point-to-point networks.

A `/30` subnet uses:

```text
Subnet Mask:   255.255.255.252
Wildcard Mask: 0.0.0.3
```

For example:

```cisco
network 172.16.0.0 0.0.0.3
```

The wildcard mask `0.0.0.3` allows the EIGRP network statement to match the `172.16.0.0/30` subnet.

---

## Route Redistribution

RIP and EIGRP are separate routing protocols, so routes learned through RIP are not automatically advertised into EIGRP.

To allow the EIGRP routers to learn routes from the RIP routing domain, **RIP routes were redistributed into EIGRP**.

The following configuration was used:

```cisco
router eigrp 1
 redistribute rip metric 10000 100 255 1 1500
```

### EIGRP Redistribution Metric

EIGRP requires a metric when redistributing routes from another routing protocol.

The configured values are:

```text
Bandwidth:   10000 Kbps
Delay:       100
Reliability: 255
Load:        1
MTU:         1500
```

This allows RIP-learned routes to be imported into EIGRP.

Once redistributed, routers running EIGRP can learn destinations that originally came from the RIP routing domain.

---

## Verifying the Routing Table

The routing table can be inspected using:

```cisco
do show ip route
```

The routing table contains routes marked:

```text
D EX
```

![Routing Table](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-05-RIP-EIGRP-Route-Redistribution/RIP-EIGRP-Route-Redistribution-route.png)

### Understanding `D EX`

* `D` = EIGRP
* `EX` = EIGRP External

An **EIGRP External** route is a route that was learned from another routing protocol and redistributed into EIGRP.

In this lab, the `D EX` routes represent **RIP routes that were redistributed into EIGRP**.

The presence of these routes confirms that the redistribution process is working.

---

## Key Concepts Demonstrated

This lab provided hands-on practice with:

* RIPv2
* EIGRP
* Route redistribution
* EIGRP external routes
* EIGRP redistribution metrics
* Wildcard masks
* `/30` point-to-point networks
* Single-mode fiber
* HWIC-1GE-SFP modules
* GLC-LH-SMD transceivers
* Routing-table verification
* `show ip route`

---

## Conclusion

This lab demonstrates how **different dynamic routing protocols can coexist within the same network** and how route redistribution can be used to exchange routing information between them.

RIPv2 was used for internal routing within each routing domain, while EIGRP provided connectivity between the two domains. RIP routes were then redistributed into EIGRP, allowing EIGRP routers to learn destinations that originated from RIP.

The lab also provided practical experience with **EIGRP metrics, external EIGRP routes, wildcard masks, `/30` point-to-point networks, and fiber-based router connections**.
