
# Lab 11 – DNS Name Resolution with OSPF Routing

## Overview
This lab demonstrates how to configure a DNS server to resolve hostnames into IP addresses while using OSPF to provide network connectivity between three different subnets.

## Network Topology
The network consists of one router, two PCs, and one DNS server. OSPF is configured on the router to route traffic between the connected networks.

![DNS and OSPF Topology](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-11-DNS/DNS-OSPF-Topology.png)

## IP Addressing

| Device | Interface / Role | IP Address | Subnet |
|---|---|---|---|
| PC3 | Host | 192.168.1.2 | 192.168.1.0/24 |
| PC4 | Host | 192.168.2.2 | 192.168.2.0/24 |
| DNS Server | DNS Service | 192.168.3.2 | 192.168.3.0/24 |
| R1 | Routing | Connected to all three subnets | Multiple subnets |

## Configuration

### 1. OSPF Routing
Configured OSPF on R1 to provide routing between the PC3, PC4, and DNS server subnets.

### 2. DNS Server
Enabled the DNS service and created the following A records:

| Hostname | IP Address |
|---|---|
| pc3 | 192.168.1.2 |
| pc4 | 192.168.2.2 |

![DNS A Records](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-11-DNS/DNS-A-Records-Configuration.png)

### 3. PC DNS Configuration
Configured both PCs to use the DNS server at `192.168.3.2`.

## Verification

From PC4, pinged PC3 using its hostname instead of its IP address:

```text
ping pc3
```

The ping succeeded, demonstrating that DNS resolved `pc3` to `192.168.1.2` and that OSPF provided the necessary network connectivity.

![DNS Name Resolution Ping Test](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-11-DNS/DNS-Name-Resolution-with-OSPF-Routing.pkt)

