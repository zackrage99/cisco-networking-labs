# STP VLAN-Based Port Roles

## Initial STP Configuration

First, I configured all switches to use **Rapid PVST+**:

```text
spanning-tree mode rapid-pvst
```

Then, on all switches, I enabled:

```text
spanning-tree portfast default
spanning-tree bpduguard default
```

PortFast is enabled globally for access ports, while BPDU Guard protects those PortFast-enabled ports from unexpected BPDUs.

No external or unauthorized switch is connected to the access ports in this topology.

### Root Bridge Design

For each VLAN, I intentionally selected the switch **closest to the router** as the root bridge:

* **VLAN 10 → SW5**
* **VLAN 20 → SW6**
* **VLAN 30 → SW4**

The goal is to keep the Layer 2 path toward the router as short as possible and **reduce unnecessary hops**, providing a more efficient path for traffic from the VLANs toward the router.

---

# VLAN 10

SW5 was configured as the root bridge:

```text
spanning-tree vlan 10 root primary
```

### Port Roles

**SW5 — Root Bridge**

* Fa0/1: Designated (D)
* Fa0/2: Designated (D)

All active ports on the root bridge are designated ports.

**SW6**

* Fa0/1: Designated (D)
* Fa0/2: Root (R)

The root port is selected because it provides the lowest-cost path toward the root bridge.

**SW7**

* Fa0/1: Designated (D)
* Fa0/2: Root (R)

**SW4**

* Fa0/1: Alternate/Blocking (B)
* Fa0/2: Root (R)
* Fa0/5: Alternate/Blocking (B)

---

# VLAN 20

SW6 was configured as the root bridge:

```text
spanning-tree vlan 20 root primary
```

### Port Roles

**SW6 — Root Bridge**

* Fa0/1: Designated (D)
* Fa0/2: Designated (D)
* Fa0/5: Designated (D)

**SW7**

* Fa0/1: Designated (D)
* Fa0/2: Root (R)

**SW4**

* Fa0/1: Alternate/Blocking (B)
* Fa0/2: Designated (D)

**SW5**

* Fa0/1: Alternate/Blocking (B)
* Fa0/2: Root (R)

---

# VLAN 30

SW4 was configured as the root bridge:

```text
spanning-tree vlan 30 root primary
```

### Port Roles

**SW4 — Root Bridge**

* Fa0/1: Designated (D)
* Fa0/2: Designated (D)
* Fa0/5: Designated (D)

**SW7**

* Fa0/1: Root (R)
* Fa0/2: Designated (D)

**SW6**

* Fa0/1: Alternate/Blocking (B)
* Fa0/2: Designated (D)

**SW5**

* Fa0/1: Root (R)
* Fa0/2: Root (R)

---

# STP Port-Role Selection

The **Root Bridge** is elected first using the lowest Bridge ID (BID), which consists of the bridge priority and MAC address.

After the root bridge is elected:

1. Each non-root switch selects its **Root Port** based on the best BPDU received, primarily using the **lowest Root Path Cost**.
2. On each network segment, a **Designated Port** is selected based on the best BPDU for that segment.
3. If the path costs are equal, STP uses additional tie-breakers, including the **Bridge ID (BID)** and then the **Port ID** when necessary.
4. A port that does not become a Root Port or Designated Port can become an **Alternate Port** and enter the blocking/discarding state.

Because I did not manually change the bridge priorities for these VLANs (apart from making a specific switch the root bridge), MAC addresses become important when STP needs to break a tie between equal-cost paths.

### Important Note

The **lowest MAC address does not automatically determine every Designated or Blocking port**. STP first considers the quality of the path to the root (Root Path Cost). The BID/MAC address is used as a tie-breaker when the relevant path costs are equal.
