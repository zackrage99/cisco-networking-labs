# RIP and EIGRP Route Redistribution Lab

In this lab, I built a network consisting of **8 routers**, divided into two routing areas.

Each area contains **4 routers running RIP** as the internal routing protocol. The two areas are connected using **EIGRP**, allowing the routers in both areas to exchange routes.

For the inter-area connection, I used **single-mode fiber (SMF)** with the **HWIC-1GE-SFP module** and **GLC-LH-SMD transceiver** in Cisco Packet Tracer.

All IP addresses, interfaces, routing protocols, and connections are clearly labeled in the topology.

## Network Design

* **8 routers** in total
* **4 routers in Area 1** running RIP
* **4 routers in Area 2** running RIP
* **2 routers from each area** participate in EIGRP
* RIP is used for internal routing within each area
* EIGRP is used to connect the two routing domains
* Route redistribution is configured so that RIP and EIGRP routes can be exchanged

## RIP Configuration

RIP version 2 was configured on the routers inside each area:

```cisco
router rip
 version 2
 no auto-summary
 network 10.0.0.0
```

`no auto-summary` was used to prevent classful route summarization.

## EIGRP Configuration

EIGRP AS **1** was configured between the routers connecting the two areas.

For example, on one router:

```cisco
router eigrp 1
 no auto-summary
 network 172.16.0.0 0.0.0.3
```

On the opposite router:

```cisco
router eigrp 1
 no auto-summary
 network 172.16.0.0 0.0.0.3
```

The second EIGRP connection was configured similarly using the `172.17.0.0/30` network.

The `/30` networks were configured using **wildcard masks**. A `/30` subnet uses the subnet mask `255.255.255.252`, which corresponds to the wildcard mask:

```text
0.0.0.3
```

This allows EIGRP to match only the interfaces belonging to the specified `/30` network.

## Route Redistribution

Since RIP and EIGRP are separate routing protocols, the routers do not automatically exchange routes learned from the other protocol.

To allow them to learn routes from each other, **route redistribution** was configured.

For example, EIGRP was configured to redistribute RIP routes:

```cisco
router eigrp 1
 redistribute rip metric 10000 100 255 1 1500
```

The metric values are required because EIGRP needs an appropriate metric when importing routes from another routing protocol.

Redistribution allows routes learned through RIP to be advertised into EIGRP, making those destinations reachable from the other routing domain.

The same concept is applied in the opposite direction where necessary, allowing EIGRP-learned routes to be redistributed into RIP.

## Verifying the Routing Table

The routing table can be inspected using:

```cisco
do show ip route
```

Routes marked with:

```text
D EX
```

represent **EIGRP external routes**.

`D` indicates that the route was learned through EIGRP, while `EX` indicates that the route is an **external EIGRP route**, meaning it was redistributed into EIGRP from another routing protocol such as RIP.

This confirms that route redistribution is working and that the routers are successfully learning destinations from the other routing domain.

## Conclusion

This lab demonstrates how multiple routing protocols can operate within the same network and how **route redistribution** can be used to exchange routing information between them.

The lab also provided practical experience with:

* RIP version 2
* EIGRP
* EIGRP external routes
* Route redistribution
* EIGRP metrics
* `/30` point-to-point networks
* Wildcard masks
* Single-mode fiber connections
* HWIC-1GE-SFP modules
* GLC-LH-SMD transceivers
* Verifying routes using `show ip route`
