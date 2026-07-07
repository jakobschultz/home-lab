# SVI Configuration Design

This document explains the planned switched virtual interface, or SVI, configuration for the homelab.

SVIs will be configured on the Cisco Catalyst Layer 3 switch and used as the default gateways for internal lab VLANs.

## Purpose

The purpose of this document is to define how SVIs will be used in the homelab network design.

An SVI allows a Layer 3 switch to provide routing for a VLAN. Instead of each VLAN sending traffic to the router for inter-VLAN routing, each VLAN will use an SVI on the Catalyst switch as its default gateway.

This allows the Catalyst switch to route traffic between internal lab VLANs.

## Scope

This document covers:

- What SVIs are used for
- Which device will host the SVIs
- How VLAN gateway addresses will be assigned
- How SVIs fit into inter-VLAN routing
- Design standards for SVI naming and addressing
- Basic validation steps

This document does not include:

- Full Cisco switch configuration
- Passwords or secrets
- Final ACL rules
- Detailed step-by-step implementation commands

Actual switch configuration should be stored in `configs/cisco-switch/`.

Detailed step-by-step setup procedures should be stored in `runbooks/`.

## Design Goals

The SVI design should accomplish the following goals:

- Use the Cisco Catalyst Layer 3 switch as the internal routing point
- Provide a default gateway for each routed VLAN
- Keep internal VLAN-to-VLAN traffic on the switch
- Reduce unnecessary traffic through the MikroTik router
- Practice enterprise-style Layer 3 switching
- Keep VLAN gateway addressing consistent and easy to understand
- Support future ACLs for VLAN-to-VLAN access control

## Design Overview

The Cisco Catalyst switch will host one SVI for each internal lab VLAN that requires Layer 3 routing.

Each SVI will use the `.1` address of its assigned subnet.

Example:

```text
VLAN 10 subnet: 10.10.10.0/24
VLAN 10 SVI:    10.10.10.1
```

Devices in VLAN 10 will use `10.10.10.1` as their default gateway.

When a device in one VLAN needs to communicate with a device in another VLAN, traffic will be sent to the local VLAN SVI. The Catalyst switch will then route the traffic internally.

## Device Role

| Device | Responsibility |
|---|---|
| Cisco Catalyst Layer 3 Switch | Hosts SVIs and performs internal inter-VLAN routing |
| MikroTik Router | Handles upstream routing, NAT, firewall boundaries, and internet access |
| Ubiquiti Express 7 | Existing upstream home network |
| Proxmox Server | Hosts lab VMs and services |
| Management Workstation | Used to administer lab devices |

## Planned SVI Layout

| VLAN | Purpose | Subnet | SVI / Default Gateway |
|---|---|---|---|
| VLAN 10 | Management | 10.10.10.0/24 | 10.10.10.1 |
| VLAN 20 | Servers | 10.10.20.0/24 | 10.10.20.1 |
| VLAN 30 | Lab / Testing | 10.10.30.0/24 | 10.10.30.1 |
| VLAN 40 | Monitoring | 10.10.40.0/24 | 10.10.40.1 |
| VLAN 50 | Clients | 10.10.50.0/24 | 10.10.50.1 |
| VLAN 60 | Guest / Untrusted | 10.10.60.0/24 | 10.10.60.1 |

The final VLAN list may change as the lab design develops.

## Addressing Standard

SVI gateway addresses should follow a consistent pattern.

Standard:

```text
VLAN subnet:         10.10.<VLAN ID>.0/24
SVI gateway address: 10.10.<VLAN ID>.1
```

Examples:

```text
VLAN 20 subnet: 10.10.20.0/24
VLAN 20 SVI:    10.10.20.1

VLAN 40 subnet: 10.10.40.0/24
VLAN 40 SVI:    10.10.40.1
```

This pattern makes the network easier to understand and troubleshoot.

## SVI Naming Standard

Each SVI should include a clear description.

Recommended format:

```text
interface Vlan<VLAN_ID>
 description <VLAN Purpose> Gateway
```

Example:

```text
interface Vlan20
 description Server VLAN Gateway
```

The description should match the VLAN purpose documented in `vlan-plan.md`.

## Inter-VLAN Routing

The Catalyst switch must have Layer 3 routing enabled in order to route between SVIs.

Conceptual command:

```text
ip routing
```

Once routing is enabled, the switch can route traffic between directly connected VLAN interfaces.

Example traffic flow:

```text
Server in VLAN 20
    ↓
Default gateway 10.10.20.1
    ↓
Catalyst Layer 3 switch
    ↓
SVI for destination VLAN
    ↓
Destination host in VLAN 40
```

Internal VLAN-to-VLAN traffic should stay on the Catalyst switch unless the destination is outside the lab VLANs.

## Default Route

The Catalyst switch will need a default route pointing to the MikroTik router for traffic outside the lab VLANs.

Example:

```text
ip route 0.0.0.0 0.0.0.0 <MikroTik transit IP>
```

This allows internal lab VLANs to reach upstream networks and the internet.

## Router Return Routes

The MikroTik router must have routes back to the VLAN subnets hosted behind the Catalyst switch.

Without return routes, traffic may leave the VLAN correctly but fail on the return path.

Example route concept:

```text
10.10.20.0/24 via Catalyst transit IP
10.10.30.0/24 via Catalyst transit IP
10.10.40.0/24 via Catalyst transit IP
```

## Access Control Considerations

SVIs make inter-VLAN routing possible, but they do not automatically enforce strong security boundaries.

Access between VLANs should be controlled using one or more of the following:

- Catalyst ACLs
- MikroTik firewall rules
- Host-based firewalls
- Service-level access controls

Initial implementation may allow basic connectivity for testing, but the final design should restrict traffic based on purpose.

Recommended policy:

```text
Management VLAN can administer infrastructure.
Monitoring VLAN can observe approved devices and services.
Server VLAN provides internal services.
Lab / Testing VLAN is restricted from management networks.
Guest / Untrusted VLAN is internet-only.
```

## Security Considerations

Important security rules:

- Do not allow all VLANs to manage the switch
- Do not allow guest or untrusted VLANs to access internal services
- Restrict Proxmox management access to trusted networks
- Restrict router and switch administration to the Management VLAN
- Use ACLs carefully and document all allowed traffic
- Avoid exposing internal management interfaces to upstream or internet networks

SVIs should be treated as critical infrastructure because they are the gateways for each VLAN.

## Dependencies

SVI configuration depends on:

- VLANs being created on the Catalyst switch
- Access ports being assigned to the correct VLANs
- Trunks carrying the required VLANs
- `ip routing` being enabled on the Catalyst switch
- Correct IP addressing
- Correct default gateway settings on hosts
- Correct routing between the Catalyst and MikroTik
- Return routes existing on the MikroTik router

## Implementation Notes

SVI implementation should happen after the basic switch baseline configuration is complete.

Recommended order:

1. Create VLANs
2. Assign VLAN names
3. Create SVIs
4. Assign SVI IP addresses
5. Enable SVIs with `no shutdown`
6. Enable Layer 3 routing
7. Configure access and trunk ports
8. Configure default route to the MikroTik
9. Configure return routes on the MikroTik
10. Test connectivity

Full switch configuration should be saved separately in `configs/cisco-switch/`.

## Validation / Testing

After configuring SVIs, validate the design with these tests.

### Local VLAN Gateway Test

From a host in each VLAN, ping the VLAN gateway.

Example:

```text
Host in VLAN 20 → ping 10.10.20.1
```

Expected result:

The host can reach its local SVI.

### Inter-VLAN Routing Test

From a host in one VLAN, ping a host or gateway in another VLAN.

Example:

```text
Host in VLAN 20 → ping 10.10.40.1
```

Expected result:

The Catalyst switch routes traffic between VLANs if allowed by policy.

### Internet Access Test

From an approved VLAN, test internet access.

Example:

```text
Host in VLAN 20 → ping 1.1.1.1
```

Expected result:

Traffic routes from the host to the Catalyst switch, then to the MikroTik router, then upstream.

### DNS Test

From an approved VLAN, test name resolution.

Example:

```text
nslookup example.com
```

Expected result:

DNS works through the planned DNS server or upstream resolver.

### Restricted Access Test

Test that blocked VLANs cannot reach restricted networks.

Example:

```text
Guest VLAN → Management VLAN
```

Expected result:

Traffic should be denied once ACLs or firewall rules are implemented.

## Troubleshooting Notes

Common SVI issues:

| Issue | Possible Cause |
|---|---|
| SVI is down | No active port in that VLAN or VLAN not created |
| Host cannot ping gateway | Wrong VLAN, wrong subnet, bad cable, or SVI down |
| Inter-VLAN routing fails | `ip routing` not enabled or ACL blocking traffic |
| Internet access fails | Missing default route, NAT, firewall rule, or MikroTik return route |
| One-way traffic | Missing return route on the MikroTik |
| Wrong gateway behavior | Host default gateway set incorrectly |

## Related Files

- [VLAN Plan](vlan-plan.md)
- [IP Addressing Plan](ip-addressing.md)
- [Inter-VLAN Routing](inter-vlan-routing.md)
- [Network Design](network-design.md)
- [Network Diagram](../diagrams/network-diagram.png)
- [Cisco Switch Configs](../configs/cisco-switch/)

## Change Notes

| Date | Change |
|---|---|
| 2026-07-06 | Initial draft |
