# Lab: Remote Switch Management Using Telnet and SSH

## 1. Overview

In this lab, I configured remote management on a Cisco Layer 2 switch using **Telnet and SSH**.

The objective was to enable remote access to Switch2 from PC1, configure local user authentication, secure privileged EXEC access, and establish SSH connections using RSA encryption.

The routers are configured with OSPF to provide end-to-end connectivity between the two LANs.

## 2. Network Topology

![Network Topology](Screenshots/Topology.png)

### IP Addressing

| Device | Interface | IP Address | Subnet Mask |
|---|---|---|---|
| PC1 | FastEthernet0 | 192.168.2.1 | 255.255.255.0 |
| Router4 | Gig0/0 | 192.168.2.254 | 255.255.255.0 |
| Router4 | Gig0/1 | 10.0.1.1 | 255.255.255.252 |
| Router5 | Gig0/1 | 10.0.1.2 | 255.255.255.252 |
| Router5 | Gig0/0 | 192.168.1.254 | 255.255.255.0 |
| Switch2 | VLAN 1 | 192.168.1.253 | 255.255.255.0 |
| PC2 | FastEthernet0 | 192.168.1.1 | 255.255.255.0 |

**Switch2 Default Gateway:** `192.168.1.254`

## 3. Switch Configuration

### 3.1 Management SVI and Default Gateway

Configured VLAN 1 with a management IP address to allow remote management of Switch2.

```cisco
interface vlan 1
 ip address 192.168.1.253 255.255.255.0
 no shutdown
exit
ip default-gateway 192.168.1.254
```

### 3.2 Privileged EXEC Password

Configured an enable secret to protect privileged EXEC mode.

```cisco
enable secret zack
```

### 3.3 Local Username and Password

Created a local user account for authentication when accessing the switch remotely.

```cisco
username zack password zack
```

## 4. SSH Configuration

### 4.1 Domain Name and RSA Key Generation

Configured a domain name and generated a 2048-bit RSA key pair required for SSH.

```cisco
ip domain name zacklab.com
crypto key generate rsa 2048
```

### 4.2 SSH Version

Enabled SSH version 2.

```cisco
ip ssh version 2
```

## 5. VTY Line Configuration

Configured all 16 VTY lines (0–15) to allow remote access using local authentication.

```cisco
line vty 0 15
 login local
 exec-timeout 5 0
 transport input all
```

**Configuration explanations:**

- `line vty 0 15`: Configures all 16 Virtual Terminal lines.
- `login local`: Uses the locally configured username and password for authentication.
- `exec-timeout 5 0`: Automatically terminates inactive remote sessions after 5 minutes.
- `transport input all`: Allows all remote access protocols supported by the device.

**Note:** I used `transport input all` because Packet Tracer did not support the more restrictive `transport input telnet ssh` command in my environment.

## 6. Telnet Configuration and Testing

Telnet remote access was enabled through the VTY lines.

```cisco
line vty 0 15
 login local
 transport input all
```

**Configuration explanations:**

- `line vty 0 15`: Enables configuration of all VTY lines.
- `login local`: Requires the locally configured username and password.
- `transport input all`: Allows Telnet and other supported remote access protocols, including SSH.

### Telnet Connection Test

From PC1, I opened Command Prompt and established a Telnet connection to Switch2 using its management IP address.

```text
telnet 192.168.1.253
```

I successfully authenticated using the configured local username and password and gained remote CLI access to Switch2.

## 7. SSH Connection and Testing

From PC1, I opened Command Prompt and established an SSH connection to Switch2.

```text
ssh -l zack 192.168.1.253
```

I successfully authenticated using the local username and password and established a secure remote CLI session.

Unlike Telnet, SSH encrypts the communication between the client and the switch.

## 8. Verification Commands

The following commands were used to verify the switch configuration and remote connections.

```cisco
show ip interface brief
show running-config
show users
show ssh
```

- `show ip interface brief`: Verifies the management SVI status and IP address.
- `show running-config`: Displays the active switch configuration.
- `show users`: Displays active terminal sessions.
- `show ssh`: Displays SSH connection and session information.

## 9. Screenshots

| Screenshot | Description |
|---|---|
| `Topology.png` | Complete network topology and IP addressing |
| `Telnet-Connection.png` | Successful Telnet connection from PC1 to Switch2 |
| `SSH-Connection.png` | Successful SSH connection from PC1 to Switch2 |
| `SSH-Verification.png` | SSH session verification using `show ssh` on Switch2 |

## 10. Skills Practiced

- Cisco IOS switch configuration
- Switch management using SVI
- Default gateway configuration
- Privileged EXEC password protection
- Local user authentication
- SSH configuration and RSA key generation
- Telnet remote access
- VTY line configuration
- Session timeout configuration
- OSPF-based network connectivity
- Remote management troubleshooting

## 11. Lab Environment

- **Software:** Cisco Packet Tracer
- **Device:** Cisco Layer 2 Switch
- **Routing Protocol:** OSPF
- **Remote Access Protocols:** Telnet and SSH
- **SSH Version:** SSHv2
- **RSA Key Size:** 2048 bits
