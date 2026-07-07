# First Implementation Order

This document defines the recommended order for implementing the homelab network and core infrastructure.

The purpose of this implementation order is to build the lab in a safe, logical sequence. Each step creates the foundation needed for the next step. This helps avoid troubleshooting too many problems at once.

## Purpose

The first implementation order explains what should be configured first, second, third, and so on when moving from planning into the actual build.

This is not a detailed step-by-step runbook. Instead, it is a high-level sequence for the initial lab deployment.

Detailed procedures should be placed in `runbooks/`.

Actual configuration files should be placed in `configs/`.

## Implementation Principles

The lab should be built in layers.

Core network access should come before virtualization.  
Routing should come before services.  
Basic connectivity should come before security tightening.  
Documentation should be updated throughout the process.

The main goal is to confirm each layer works before building the next one.

## Implementation Order

## 1. Physical Setup

Set up the physical lab equipment before configuring services or advanced network features.

Tasks:

- Place or rack lab devices
- Connect power
- Connect console cables if needed
- Connect Ethernet cables
- Label important cables
- Confirm devices power on
- Confirm fans, ports, and link lights work

Expected outcome:

The physical lab hardware is connected, powered, and ready for baseline configuration.

## 2. Baseline Device Access

Establish basic administrative access to each core device.

Tasks:

- Console into the MikroTik router
- Console into the Cisco Catalyst switch
- Set device hostnames
- Set local admin credentials
- Enable SSH where appropriate
- Disable insecure or unnecessary management access
- Assign temporary or planned management IP addresses
- Save baseline configurations

Expected outcome:

The router and switch can be safely accessed and managed.

## 3. Management Network Foundation

Build the first management network before adding the rest of the VLANs.

Tasks:

- Create the Management VLAN
- Assign the Catalyst switch management SVI or management interface
- Assign router management access as planned
- Confirm the management workstation can reach core devices
- Confirm SSH or web management access works only from approved locations

Expected outcome:

Core lab devices are reachable from the intended management network.

## 4. Catalyst Layer 3 Switching Foundation

Prepare the Catalyst switch to act as the internal Layer 3 routing device.

Tasks:

- Create planned VLANs
- Create SVIs for routed VLANs
- Enable Layer 3 routing with `ip routing`
- Assign gateway IP addresses to VLAN SVIs
- Configure access ports
- Configure trunk ports if needed
- Confirm VLAN interfaces come up

Expected outcome:

The Catalyst switch is ready to route between internal lab VLANs.

## 5. Inter-VLAN Routing

Verify that VLAN-to-VLAN routing works on the Catalyst switch.

Tasks:

- Place test devices or test VMs into different VLANs
- Confirm each host can reach its default gateway
- Confirm VLANs can route through their SVIs
- Test basic connectivity between approved VLANs
- Document any VLANs that should be restricted later

Expected outcome:

Internal VLAN routing works through the Catalyst Layer 3 switch.

## 6. Catalyst-to-MikroTik Uplink

Connect the Catalyst Layer 3 switch to the MikroTik router.

Preferred design:

- Use a routed Layer 3 transit link between the Catalyst switch and MikroTik router

Example:

```text
MikroTik transit IP: 10.10.254.1/30
Catalyst transit IP: 10.10.254.2/30
```

Tasks:

- Configure the transit link
- Add a default route on the Catalyst pointing to the MikroTik
- Add return routes on the MikroTik pointing back to the Catalyst
- Confirm the MikroTik can reach the VLAN gateway IPs
- Confirm the Catalyst can reach the MikroTik transit IP

Expected outcome:

The Catalyst and MikroTik can route traffic between each other.

## 7. Internet Access for Lab VLANs

Configure upstream access so approved lab VLANs can reach the internet.

Tasks:

- Configure NAT on the MikroTik if required
- Configure firewall rules for outbound lab traffic
- Allow DNS and NTP as needed
- Test internet access from each approved VLAN
- Confirm blocked VLANs cannot reach networks they should not access

Expected outcome:

Approved lab VLANs can reach the internet through the MikroTik router.

## 8. Proxmox Installation and Networking

Install and configure the virtualization host after the network foundation is stable.

Tasks:

- Install Proxmox VE
- Assign Proxmox management IP
- Configure Linux bridges
- Connect Proxmox to the correct switch port or trunk
- Confirm Proxmox web access from the management network
- Create an initial test VM
- Confirm VM network connectivity

Expected outcome:

Proxmox is reachable, stable, and able to host virtual machines on the correct network.

## 9. Core Internal Services

Deploy basic services after routing and Proxmox networking are working.

Possible services:

- DNS
- DHCP, if not handled by the router
- Monitoring
- Docker host
- Backup target
- Linux administration VM
- Test web service

Tasks:

- Create required VMs
- Assign static IP addresses or DHCP reservations
- Document service purpose and network placement
- Confirm each service is reachable from approved VLANs
- Confirm services are not reachable from unapproved VLANs

Expected outcome:

The first internal lab services are running and reachable only where intended.

## 10. Monitoring and Logging

Add monitoring after there are devices and services worth monitoring.

Tasks:

- Monitor router reachability
- Monitor switch reachability
- Monitor Proxmox host
- Monitor core VMs
- Configure SNMP, agents, or exporters as needed
- Create basic dashboards
- Document what is monitored and why

Expected outcome:

Core infrastructure visibility is in place.

## 11. Backup Planning and Testing

Add backups before the lab becomes too complex.

Tasks:

- Decide what needs to be backed up
- Back up router configuration
- Back up switch configuration
- Back up important VM data
- Back up Proxmox configuration notes
- Test at least one restore process
- Document backup locations and restore procedures

Expected outcome:

Important configurations and services can be recovered if something breaks.

## 12. Security Tightening

Add stricter controls after basic connectivity has been proven.

Tasks:

- Restrict management access to the Management VLAN
- Add ACLs between VLANs if using the Catalyst for internal filtering
- Add MikroTik firewall rules for lab-to-home and lab-to-internet boundaries
- Block guest or untrusted networks from internal access
- Limit access to Proxmox management
- Disable unused services
- Review exposed ports
- Confirm denied traffic is actually blocked

Expected outcome:

The lab is segmented and management access is restricted.

## 13. Automation

Add automation only after the manual design is understood.

Possible tools:

- Ansible
- Backup scripts
- Configuration collection scripts
- Health check scripts
- Update scripts

Tasks:

- Create an inventory of managed hosts
- Automate simple repeatable tasks first
- Avoid automating broken or unclear processes
- Document what each automation does

Expected outcome:

Common tasks can be repeated consistently and safely.

## 14. Documentation Cleanup

Update documentation throughout the build, then review it after the first full implementation.

Tasks:

- Update network diagrams
- Update VLAN plan
- Update IP addressing plan
- Update hardware inventory
- Add sanitized configs
- Add runbooks for common tasks
- Add changelog entries
- Remove outdated planning notes
- Verify links in the README

Expected outcome:

The repository accurately reflects the current design and implementation.

## Summary

The lab should be implemented in this order:

```text
1. Physical setup
2. Baseline device access
3. Management network foundation
4. Catalyst Layer 3 switching foundation
5. Inter-VLAN routing
6. Catalyst-to-MikroTik uplink
7. Internet access for lab VLANs
8. Proxmox installation and networking
9. Core internal services
10. Monitoring and logging
11. Backup planning and testing
12. Security tightening
13. Automation
14. Documentation cleanup
```

This order keeps the build controlled and easier to troubleshoot.

The most important rule is to validate each layer before moving to the next one.
