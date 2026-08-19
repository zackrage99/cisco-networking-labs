# EtherChannel – Layer 2 & Layer 3 Switches

## Lab Overview

This lab demonstrates the configuration of **EtherChannel** between a Layer 2 switch and a Layer 3 switch to increase link capacity and provide redundancy by bundling multiple physical interfaces into a single logical **Port-Channel**.

![EtherChannel](https://github.com/zackrage99/cisco-networking-labs/blob/main/Lab-04-EtherChannel/EtherChannel.png)

### Configuration

* Created the required VLANs on both switches.
* Configured **EtherChannel** on the Layer 2 switch to increase the available bandwidth and address the oversubscription issue.
* Configured the resulting **Port-Channel as a trunk** so traffic from multiple VLANs could traverse the EtherChannel link.
* Manually configured the Layer 2 switch interfaces to **100 Mbps, full-duplex** to match the available interface speed on the Layer 3 switch.
* Configured EtherChannel on the Layer 3 switch using multiple physical interfaces.
* Configured the Layer 3 switch **Port-Channel as a trunk** to carry traffic for multiple VLANs.
* Configured the Layer 3 switch interfaces to **100 Mbps, full-duplex** to match the Layer 2 switch.
* Configured **SVIs (Switched Virtual Interfaces)** on the Layer 3 switch to provide inter-VLAN routing.

### Additional Configuration

* Enabled **Rapid PVST+** on both switches.
* Enabled **PortFast by default** on access ports.
* Enabled **BPDU Guard by default** on PortFast-enabled interfaces.
* Shut down unused interfaces as a basic security measure.

## Technologies Used

* EtherChannel
* Port-Channel
* VLANs
* 802.1Q Trunking
* Layer 3 Switching
* SVI
* Inter-VLAN Routing
* Rapid PVST+
* PortFast
* BPDU Guard
* Speed & Duplex Configuration
