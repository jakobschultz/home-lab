# Inter-VLAN Routing Design

This document explains the planned inter-VLAN routing design for the homelab.

## Overview

Inter-VLAN routing will be handled by the Cisco Catalyst Layer 3 switch.

Instead of sending all VLAN-to-VLAN traffic up to the router, the Catalyst switch will provide the default gateways for the internal lab VLANs using switched virtual interfaces, also known as SVIs.

The router will still be used for upstream routing, internet access, firewall boundaries, and traffic leaving the lab network.

## Design Goal

The goal of this design is to make the Catalyst switch function as the core Layer 3 device for internal lab traffic.

This provides practice with an enterprise-style network design where internal VLAN routing is handled at the switching layer, while the router/firewall controls traffic going outside the internal network.

## Device Roles

| Device | Role |
|---|---|
| Cisco Catalyst Layer 3 Switch | Handles VLANs, SVIs, and inter-VLAN routing |
| MikroTik Router | Handles upstream routing, firewall policy, NAT, and lab-to-home/internet access |
| Ubiquiti Express 7 | Existing home network / upstream network |
| Proxmox Server | Hosts virtual machines and lab services |
| Management Workstation | Used to administer lab devices and services |

## Routing Responsibility

The Catalyst switch will handle routing between internal lab VLANs.

Examples:

- Management VLAN to Server VLAN
- Server VLAN to Lab VLAN
- Lab VLAN to Monitoring VLAN
- Management VLAN to Proxmox management interface

The MikroTik router will handle traffic that needs to leave the lab switching environment.

Examples:

- Lab VLANs accessing the internet
- Lab network reaching the home network, if allowed
- VPN access into the lab
- Firewall enforcement between lab and upstream networks

## Planned Traffic Flow

### Internal VLAN-to-VLAN Traffic

```text
Host in VLAN 10
    ↓
Default gateway SVI on Catalyst switch
    ↓
Catalyst routes traffic internally
    ↓
Destination host in VLAN 20
```

This traffic stays on the Catalyst switch and does not need to go to the router.

### Internet-Bound Traffic

```text
Host in VLAN 20
    ↓
Default gateway SVI on Catalyst switch
    ↓
Default route from Catalyst to MikroTik router
    ↓
MikroTik firewall/NAT
    ↓
Upstream home network / internet
```

The Catalyst switch routes the traffic to the MikroTik only when the destination is outside the internal lab networks.

## SVI Design Example (Not actual VLAN's)

Each routed VLAN will have an SVI on the Catalyst switch.

Example:

| VLAN | Purpose | Subnet | Default Gateway |
|---|---|---|---|
| VLAN 10 | Management | 10.10.10.0/24 | 10.10.10.1 |
| VLAN 20 | Servers | 10.10.20.0/24 | 10.10.20.1 |
| VLAN 30 | Lab / Testing | 10.10.30.0/24 | 10.10.30.1 |
| VLAN 40 | Monitoring | 10.10.40.0/24 | 10.10.40.1 |

The `.1` address in each subnet will be assigned to the Catalyst switch SVI and used as the default gateway for devices in that VLAN.

## Example Cisco SVI Layout

```text
ip routing

vlan 10
 name MANAGEMENT

vlan 20
 name SERVERS

vlan 30
 name LAB

vlan 40
 name MONITORING

interface Vlan10
 description Management VLAN Gateway
 ip address 10.10.10.1 255.255.255.0
 no shutdown

interface Vlan20
 description Server VLAN Gateway
 ip address 10.10.20.1 255.255.255.0
 no shutdown

interface Vlan30
 description Lab VLAN Gateway
 ip address 10.10.30.1 255.255.255.0
 no shutdown

interface Vlan40
 description Monitoring VLAN Gateway
 ip address 10.10.40.1 255.255.255.0
 no shutdown
```

## Uplink to Router

The Catalyst switch will have an uplink to the MikroTik router.

This uplink may be configured as either:

1. A routed Layer 3 link
2. A trunk link carrying one or more VLANs

The preferred design is a routed Layer 3 link if supported by the final hardware and configuration plan.

Example routed link:

```text
Catalyst switch routed port: 10.10.254.2/30
MikroTik router interface:   10.10.254.1/30
```

Example Catalyst default route:

```text
ip route 0.0.0.0 0.0.0.0 10.10.254.1
```

This tells the Catalyst switch to send unknown or internet-bound traffic to the MikroTik router.

## Router Return Routes

The MikroTik router must know how to reach the VLAN subnets behind the Catalyst switch.

Example static routes on the MikroTik:

```text
10.10.10.0/24 via 10.10.254.2
10.10.20.0/24 via 10.10.254.2
10.10.30.0/24 via 10.10.254.2
10.10.40.0/24 via 10.10.254.2
```

Without these return routes, traffic may leave the VLANs correctly but fail on the return path.

## Firewall and Access Control

The Catalyst switch will handle basic internal routing, but access control still needs to be planned carefully.

Possible options:

- Use ACLs on the Catalyst switch to restrict traffic between VLANs
- Use the MikroTik router as the main firewall boundary
- Keep internal VLAN routing open at first, then restrict it later
- Allow Management VLAN access to all infrastructure devices
- Deny unnecessary access from lab/testing VLANs to management interfaces

Initial implementation should prioritize safe basic connectivity before adding complex ACLs.

## Design Benefits

This design provides several learning benefits:

- Practice configuring VLANs and SVIs
- Practice enabling Layer 3 routing on a switch
- Practice separating internal routing from upstream firewalling
- Practice planning default routes and return routes
- Practice enterprise-style switching design
- Reduce unnecessary traffic through the router
- Build a cleaner foundation for future services

## Design Considerations

This design adds more complexity than routing all VLANs on the router.

Important considerations:

- The Catalyst switch must support the required Layer 3 features
- `ip routing` must be enabled on the switch
- Each VLAN needs a working SVI
- Trunk and access ports must be configured correctly
- The router needs routes back to the internal VLANs
- Firewall policy must be planned intentionally
- Documentation should be updated whenever VLANs or subnets change

## Summary

Inter-VLAN routing will be handled by the Cisco Catalyst Layer 3 switch using SVIs.

The Catalyst switch will act as the internal routing point for lab VLANs. The MikroTik router will act as the upstream router and firewall boundary for traffic leaving the lab network.

This design better reflects how many enterprise networks separate internal Layer 3 switching from edge routing and firewalling.
