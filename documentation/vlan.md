# VLAN Plan

## Overview

This document defines the VLAN segmentation strategy for the homelab environment.

The goal is to improve security, isolate network traffic, and simulate enterprise network design.

---

## VLAN Structure

| VLAN ID | Name        | Purpose                          | Subnet            |
|--------|------------|----------------------------------|-------------------|
| 10     | MGMT       | Network device management        | 192.168.10.0/24   |
| 20     | SERVERS    | Proxmox, VMs, self-hosted apps   | 192.168.20.0/24   |
| 30     | CLIENTS    | Personal laptops/desktops        | 192.168.30.0/24   |
| 40     | IOT        | Smart devices (TVs, plugs, etc.) | 192.168.40.0/24   |
| 50     | GUEST      | Guest Wi-Fi access               | 192.168.50.0/24   |

---

## VLAN Details

### VLAN 10 - Management
- Used for network infrastructure only
- Devices:
  - Router / Firewall
  - Managed switch
  - Proxmox host (management interface)
- Highly restricted access

---

### VLAN 20 - Servers
- Internal services and virtual machines
- Examples:
  - Pi-hole DNS
  - Docker containers
  - Monitoring stack (Grafana/Prometheus)
- Accessible from CLIENTS VLAN with rules

---

### VLAN 30 - Clients
- Personal devices (PCs, laptops)
- Can access:
  - Internet
  - Selected server services
- Cannot directly access management VLAN

---

### VLAN 40 - IoT
- Smart home devices
- Strict outbound-only internet access
- Blocked from internal network access

---

### VLAN 50 - Guest
- Temporary access for visitors
- Internet-only access
- Fully isolated from internal VLANs

---

## Inter-VLAN Routing Rules

| Source VLAN | Destination VLAN | Access |
|-------------|------------------|--------|
| CLIENTS     | SERVERS          | Allowed (limited ports) |
| CLIENTS     | MGMT             | Denied |
| IOT         | SERVERS          | Denied |
| GUEST       | Any internal     | Denied |
| MGMT        | All              | Allowed |

---

## Firewall Philosophy


---

## Future Expansion

---

## Notes

This VLAN design is intended to simulate a small enterprise network environment and may evolve as services and infrastructure grow.
