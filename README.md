# Jeremy's IT Lab CCNA Mega Lab

This is my work from the **Jeremy's IT Lab CCNA Mega Lab**. I used this lab to practise configuring a multi-switch network and to get more comfortable with the commands and troubleshooting I may need for the CCNA.

The lab covers routing, switching, redundancy, DHCP, network management, and Layer 2 security. I configured the devices in stages and kept rough command notes so I could refer back to what I had done.

> **Note:** This README describes the configuration work represented in my notes. Some items may still need verification against the lab's Check Results assessment.

<img width="1018" height="639" alt="image" src="https://github.com/user-attachments/assets/125fe664-6592-422c-bae4-17037f7cf4f1" />

## What I worked on

### 1. Basic device setup
- Set hostnames on network devices.
- Configured enable secrets and local user accounts.
- Set console access to use local login.
- Added an exec timeout and synchronous logging on console lines.
- Saved configurations during the lab.

### 2. VLANs and trunk links
- Worked with the Office A VLANs: **10, 20, 40 and 99**.
- Worked with the Office B VLANs: **10, 20, 30 and 99**.
- Configured switch trunks with static trunk mode and disabled DTP negotiation using `switchport nonegotiate`.
- Set **VLAN 1000** as the native VLAN on the relevant switch-to-switch trunks.
- Restricted trunk links to the VLANs needed for each office.
- Shut down unused switch interfaces where specified in my notes.

### 3. Layer 3 switching and addressing
- Configured routed interfaces between the core and distribution switches.
- Assigned point-to-point IPv4 subnets to the routed links.
- Configured loopback interfaces for device router IDs and management/testing.
- Enabled IP routing on the Layer 3 switches.

The addressing used in my notes includes:
- CSW1 loopback: `10.0.0.77/32`
- CSW2 loopback: `10.0.0.78/32`
- DSW-A1 loopback: `10.0.0.79/32`
- DSW-A2 loopback: `10.0.0.80/32`
- DSW-B1 loopback: `10.0.0.81/32`
- DSW-B2 loopback: `10.0.0.82/32`

### 4. OSPF routing
- Configured OSPF process 1 on the Layer 3 devices shown in my notes.
- Set unique router IDs.
- Advertised the routed links, loopbacks, and relevant VLAN interfaces into area 0.
- Made loopbacks and user VLAN interfaces passive where specified.
- Configured routed links as OSPF point-to-point links to avoid a broadcast network type on those connections.

I still use commands such as `show ip ospf neighbor` and `show ip route ospf` to verify adjacencies and learned routes.

### 5. HSRP gateway redundancy
- Configured HSRP version 2 on the distribution switches.
- Set virtual default-gateway addresses for the VLANs.
- Adjusted HSRP priority and preemption on selected switches so gateway roles could be distributed between the two switches.

The virtual gateway addresses in my notes are:

| Office | VLAN | Virtual IP |
|---|---:|---|
| A | 10 | `10.1.0.1` |
| A | 20 | `10.2.0.1` |
| A | 40 | `10.6.0.1` |
| A | 99 | `10.0.0.1` |
| B | 10 | `10.3.0.1` |
| B | 20 | `10.4.0.1` |
| B | 30 | `10.5.0.1` |
| B | 99 | `10.0.0.17` |

### 6. Spanning Tree and EtherChannel
- Used Rapid PVST+ for spanning-tree operation.
- Set different root bridge priorities for VLAN groups to distribute Layer 2 forwarding across the distribution switches.
- Configured a Port-Channel using PAgP on the distribution switches.
- Checked EtherChannel membership and trunk status with show commands.

### 7. DHCP and relay
- Created DHCP pools on R1 for management devices, PCs, phones, and Wi-Fi clients.
- Configured excluded address ranges to reserve addresses for gateways and infrastructure.
- Set default gateways, DNS server, and domain name in the pools.
- Configured helper addresses on the distribution-switch VLAN interfaces to relay DHCP requests to R1.
- Used DHCP Option 43 in the management pool configuration where specified in my notes.

### 8. Network services and remote management
- Configured the domain name `jeremysitlab.com` and DNS server `10.5.0.4` on devices where required.
- Configured NTP authentication and a trusted key.
- Added an SNMP read-only community and remote syslog settings.
- Configured SSH version 2, local user authentication, and a VTY access-class to limit SSH access to the specified source subnet.
- Disabled CDP and enabled LLDP on the devices specified in my notes.
- Disabled LLDP transmit on selected access ports.

### 9. Layer 2 security
- Configured port security on endpoint-facing access ports.
- Used sticky MAC learning and the `restrict` violation mode on selected ports.
- Enabled DHCP snooping on the specified VLANs.
- Trusted the switch uplinks for DHCP snooping and applied DHCP rate limits to endpoint ports.
- Disabled the DHCP snooping information option where specified.
- Enabled Dynamic ARP Inspection (DAI) on the relevant VLANs.
- Added ARP validation checks and trusted the uplinks for DAI.
- Configured PortFast and BPDU Guard on selected access ports.

## Commands I used for verification

These are some of the commands I used, or planned to use, to check the configuration and troubleshoot problems:

```text
show vlan brief
show interfaces trunk
show etherchannel summary
show spanning-tree
show standby brief
show ip interface brief
show ip ospf neighbor
show ip route
show ip route ospf
show ip dhcp binding
show ip dhcp snooping
show ip dhcp snooping binding
show ip arp inspection statistics
show port-security
show port-security address
show ip nat translations
show ip nat statistics
copy running-config startup-config
```

## What I learned

This lab helped me practise how different parts of a network depend on each other. For example, a DHCP pool can be configured correctly but clients may still fail to get an address if the VLAN, trunk, SVI, helper address, or routing is wrong. I also got practice with HSRP gateway redundancy, OSPF neighbor troubleshooting, trunk allowed VLANs, EtherChannel, and switch security features.

I kept command notes by lab section so I can revisit the configurations and understand why each command was used instead of just pasting commands without knowing what they do.

## Current status

This README is a summary of my lab work and notes, not a claim that every assessment item has passed. I still need to compare the final device configurations with the lab's **Check Results** output and verify end-to-end connectivity.

## Lab reference

- Lab: [Jeremy's IT Lab CCNA Mega Lab](https://www.jeremysitlab.com/)
- Main topics: VLANs, trunking, EtherChannel, Rapid PVST+, inter-VLAN routing, OSPF, HSRP, DHCP, SSH, NTP, SNMP, syslog, DHCP snooping, Dynamic ARP Inspection, and port security.
