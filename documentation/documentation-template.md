# USE THIS AS ROUGH TEMPLATE FOR DOCUMENTATION

# Document Title

## Purpose

Briefly explain what this document covers and why it exists.

Example:

This document defines the VLAN design for the homelab, including VLAN IDs, purposes, subnet assignments, and intended access boundaries.

## Scope

Explain what is included and what is not included.

Included:

* Item 1
* Item 2
* Item 3

Not included:

* Actual device configuration
* Passwords, secrets, or private keys
* Step-by-step implementation procedures

## Design Goals

List the goals this design is trying to accomplish.

* Keep the lab network organized
* Separate trusted and untrusted traffic
* Make the design easy to troubleshoot
* Keep documentation understandable for future review

## Design Overview

Describe the design at a high level.

This section should explain how the system is supposed to work without getting too deep into commands or configuration syntax.

## Standards / Rules

List the standards, rules, or conventions that should be followed.

Examples:

* Use consistent naming
* Avoid exposing internal services directly to the internet
* Management access should only come from the management VLAN
* All major changes should be documented

## Planned Design

Document the actual planned design.

Use tables when possible.

| Item    | Value   | Notes   |
| ------- | ------- | ------- |
| Example | Example | Example |

## Dependencies

List anything this design depends on.

Examples:

* Router configuration
* Switch VLAN support
* Proxmox networking
* DNS
* DHCP
* Firewall rules

## Security Considerations

Explain the security impact of this design.

Include things like:

* What should be restricted
* What should be allowed
* What should never be exposed
* What needs to be reviewed later

## Implementation Notes

Briefly describe how this design will eventually be implemented.

Do not put full configs here. Actual configuration files should go in `configs/`.

For step-by-step procedures, use `runbooks/`.

## Validation / Testing

List how you will confirm the design works.

Examples:

* Ping default gateway from each VLAN
* Confirm DNS resolution
* Confirm internet access where allowed
* Confirm blocked traffic is actually blocked
* Confirm management access only works from approved networks

## Related Files

Link to related documentation, configs, diagrams, or runbooks.

Examples:

* [Network Diagram](../diagrams/network-diagram.png)
* [VLAN Plan](vlan-plan.md)
* [IP Addressing Plan](ip-addressing.md)
* [Inter-VLAN Routing](inter-vlan-routing.md)
* [Add New VLAN Runbook](../runbooks/add-new-vlan.md)

## Change Notes

Track major updates to this document.

| Date       | Change        |
| ---------- | ------------- |
| YYYY-MM-DD | Initial draft |
