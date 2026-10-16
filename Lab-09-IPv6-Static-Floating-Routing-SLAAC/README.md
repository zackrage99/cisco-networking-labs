# IPv6 Static Routing, Floating Static Routes & SLAAC

## Overview

This Cisco Packet Tracer lab explores IPv6 static routing, link-local next-hop addresses, SLAAC, and floating static routes.

The topology contains two paths between PC2 and PC3:

- **Primary path:** R2 → R4 → R3, using normal static routes with global unicast next-hop addresses.
- **Backup path:** R2 → R5 → R3, using link-local next-hop addresses and floating static routes with an administrative distance (AD) of 10.

Both PCs obtain their IPv6 addresses automatically using SLAAC.

## Network Topology

![IPv6 Network Topology](screenshots/ipv6-topology.png)

### IPv6 Addressing

| Network | IPv6 Prefix |
|---|---|
| PC2 LAN | `2001:DB8:FFE:1::/64` |
| R2–R4 | `2001:DB8:FFE:2::/64` |
| R4–R3 | `2001:DB8:FFE:3::/64` |
| PC3 LAN | `2001:DB8:FFE:4::/64` |

The R2–R5 and R5–R3 links use link-local addresses for next-hop routing.

## 1. Normal IPv6 Static Routing

The primary route forwards traffic through R4 using global unicast next-hop addresses.

Example route on R2:

```cisco
ipv6 route 2001:DB8:FFE:4::/64 <next-hop-global-address>
```

The corresponding routes must also be configured on R4 and R3 to provide connectivity in both directions.

These are recursive static routes because the router resolves the global unicast next-hop address using its routing table.

## 2. Link-Local Static Routing

The backup path uses R5 instead of R4.

Example route using a link-local next hop:

```cisco
ipv6 route 2001:DB8:FFE:4::/64 <outgoing-interface> <link-local-address>
```

When configuring a static route with a link-local next hop, the outgoing interface must be specified because link-local addresses are only valid on their directly connected link.

The next-hop address must belong to the neighboring router's interface on that link.

## 3. Floating Static Routes

The backup route is configured with an administrative distance of 10.

```cisco
ipv6 route 2001:DB8:FFE:4::/64 <outgoing-interface> <link-local-address> 10
```

The default administrative distance of a static route is 1. Therefore, the route with AD 10 is less preferred while the primary route is available.

When the primary route becomes unavailable and is removed from the routing table, the floating static route can be installed, provided the backup route remains valid.

## 4. SLAAC Configuration

Both PCs use Stateless Address Autoconfiguration (SLAAC) to obtain their IPv6 addresses automatically.

The routers advertise their LAN prefixes through Router Advertisements, allowing the PCs to generate their own IPv6 addresses and learn their default gateways.

The router interfaces must have correctly configured IPv6 addresses and IPv6 routing enabled.

## 5. Connectivity Testing

### Default Static Route

A ping test from PC2 to PC3 verifies connectivity through the primary path:

**PC2 → R2 → R4 → R3 → PC3**

![Ping Test - Default Static Route](screenshots/default-static-route-ping.png)

### Floating Static Route — AD 10

To test the backup route, I removed the primary-path links between R2, R4, and R3, then repeated the ping test from PC2 to PC3.

Traffic used the alternative path:

**PC2 → R2 → R5 → R3 → PC3**

![Ping Test - Floating Static Route](screenshots/floating-static-route-ping.png)

This test demonstrates backup connectivity when the primary path is unavailable.

## Verification Commands

```cisco
show ipv6 interface brief
show ipv6 route
show ipv6 route static
show ipv6 neighbors
show running-config
```

These commands help verify interface status, installed routes, IPv6 neighbors, and static route configuration.

