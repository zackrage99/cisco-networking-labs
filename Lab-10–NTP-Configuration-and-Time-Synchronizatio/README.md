# Lab 09 – NTP Configuration and Time Synchronization

## Overview
This lab demonstrates NTP configuration and time synchronization in a three-router network using Cisco Packet Tracer. OSPF provides IP connectivity between the routers, allowing NTP communication across the network.

## Objectives
- Configure OSPF for IP connectivity.
- Manually set the clock on R1.
- Configure R1 as an NTP master.
- Configure R2 as an NTP client.
- Verify NTP status and associations on R1 and R3.

## Topology

![NTP Topology](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-10%E2%80%93NTP-Configuration-and-Time-Synchronizatio/NTP-Topology.png)

**Devices:** R1, R2, R3  
**Routing Protocol:** OSPF  
**Time Synchronization:** NTP

## Configuration

### R1 – NTP Master

R1's clock was set manually before configuring it as the NTP master.

```cisco
clock set HH:MM:SS DAY MONTH YEAR
ntp master
```

### R2 – NTP Client

R2 was configured to synchronize with R1.

```cisco
ntp server 10.0.1.1
```

## Verification

### 1. Network Topology

![NTP Topology](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-10%E2%80%93NTP-Configuration-and-Time-Synchronizatio/NTP-Topology.png)

### 2. R1 – NTP Associations

```cisco
show ntp associations
```

![R1 NTP Associations](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-10%E2%80%93NTP-Configuration-and-Time-Synchronizatio/R1-NTP-Associations.png)

### 3. R3 – NTP Status

```cisco
show ntp status
```

![R3 NTP Status](R3-NTP-Status.png)

### 4. R3 – NTP Associations

```cisco
show ntp associations
```

![R3 NTP Associations](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-10%E2%80%93NTP-Configuration-and-Time-Synchronizatio/R3-NTP-Associations.png)

## Key Takeaways
- NTP synchronizes clocks between network devices.
- OSPF provides IP reachability but does not synchronize time.
- `ntp master` configures a router to provide an NTP time reference.
- `ntp server` configures a router to synchronize with an NTP server.
- `show ntp associations` displays NTP association information.
- `show ntp status` displays the router's synchronization state and stratum.

