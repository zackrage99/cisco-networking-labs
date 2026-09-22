# Lab 08 — Router-on-a-Stick with HSRP

This lab demonstrates **Inter-VLAN Routing** using **Router-on-a-Stick (ROAS)** with **HSRP** for default gateway redundancy.

## Network Topology

The topology uses two routers and multiple switches. Both routers provide routing for VLAN 10 and VLAN 20, while HSRP provides a virtual default gateway for each VLAN.

* **Router 1:** Primary HSRP router
* **Router 2:** Standby HSRP router
* **VLAN 10:** `192.168.1.0/24`
* **VLAN 20:** `192.168.2.0/24`
* Router-to-switch links use **802.1Q trunking**
* HSRP uses **version 2**

### Topology

![HSRP Topology](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-08-Router-on-a-Stick-HSRP/HSRP-Topology.png)

## IP Addressing

| Device   | Interface | VLAN | IP Address       |
| -------- | --------- | ---: | ---------------- |
| Router 1 | `G0/0.10` |   10 | `192.168.1.2/24` |
| Router 1 | `G0/0.20` |   20 | `192.168.2.2/24` |
| Router 2 | `G0/0.10` |   10 | `192.168.1.3/24` |
| Router 2 | `G0/0.20` |   20 | `192.168.2.3/24` |

### HSRP Virtual Gateways

| VLAN | HSRP Group | Virtual IP      | Router 1 Priority | Router 2 Priority |
| ---: | ---------: | --------------- | ----------------: | ----------------: |
|   10 |         10 | `192.168.1.254` |               200 |               100 |
|   20 |         20 | `192.168.2.254` |               200 |               100 |

Router 1 has the higher priority and is configured with `preempt`, allowing it to regain the Active role after recovering.

## Router Configuration

### Router 1

```cisco
interface GigabitEthernet0/0
 no shutdown

interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.1.2 255.255.255.0
 standby version 2
 standby 10 ip 192.168.1.254
 standby 10 priority 200
 standby 10 preempt

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.2.2 255.255.255.0
 standby version 2
 standby 20 ip 192.168.2.254
 standby 20 priority 200
 standby 20 preempt
```

### Router 2

```cisco
interface GigabitEthernet0/0
 no shutdown

interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.1.3 255.255.255.0
 standby version 2
 standby 10 ip 192.168.1.254
 standby 10 priority 100

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.2.3 255.255.255.0
 standby version 2
 standby 20 ip 192.168.2.254
 standby 20 priority 100
```

## Switch Configuration

The switch interfaces connected to the routers are configured as trunks and allow VLAN 10 and VLAN 20.

```cisco
interface <interface-id>
 switchport mode trunk
 switchport trunk allowed vlan 10,20
```

### VLAN Verification

The VLAN configuration was verified using `show vlan brief`.

![Switch VLAN Brief](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-08-Router-on-a-Stick-HSRP/Switch-Show-VLAN-Brief.png)

## HSRP Verification

### Router 1 — HSRP Status

The `show standby` command confirms the HSRP configuration. Router 1 has a priority of **200** and is the local Active router.

![R1 Show Standby](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-08-Router-on-a-Stick-HSRP/R1-Show-Standby-Brief.png)

### Router 1 — HSRP Brief

Router 1 is shown as the **Active** router for the configured HSRP groups.

![R1 Show Standby Brief](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-08-Router-on-a-Stick-HSRP/R1-Show-Standby-Brief.png)

### Router 2 — HSRP Brief

Router 2 is shown as the **Standby** router for the configured HSRP groups.

![R2 Show Standby Brief](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-08-Router-on-a-Stick-HSRP/R2-Show-Standby-Brief.png)

## Connectivity Testing

Connectivity between the VLANs was tested using Cisco Packet Tracer **Simulation Mode**.

### Ping — Sent

![Ping Simulation Send](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-08-Router-on-a-Stick-HSRP/Ping-Simulation-Send.png)

### Ping — Received

![Ping Simulation Receive](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-08-Router-on-a-Stick-HSRP/Ping-Simulation-Receive.png)

The successful packet flow demonstrates communication between the VLANs through the HSRP virtual gateway and Router-on-a-Stick configuration.

## Verification Commands

### HSRP

```cisco
show standby
show standby brief
```

### VLANs

```cisco
show vlan brief
```

### Trunks

```cisco
show interfaces trunk
```

## HSRP Failover

Router 1 was configured as the preferred HSRP router using a higher priority and `preempt`.

The HSRP configuration provides gateway redundancy:

1. Router 1 operates as the **Active** router.
2. Router 2 operates as the **Standby** router.
3. If Router 1 fails, Router 2 can take over the Active role.
4. Hosts continue using the same HSRP virtual IP as their default gateway.

## Key Concepts Practiced

* Router-on-a-Stick
* 802.1Q trunking
* Inter-VLAN Routing
* HSRP Version 2
* HSRP Active/Standby roles
* HSRP priority and preemption
* Default Gateway Redundancy
* VLAN verification
* Trunk verification
* Packet Tracer Simulation Mode
* HSRP failover
