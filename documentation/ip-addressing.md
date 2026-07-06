# IP Addressing Plan

## Overview

This document contains the IP addressing scheme for the homelab network.

The goal is to keep devices organized and allow for future expansion, VLAN segmentation, and easier troubleshooting.

---

# Network Ranges

| Network | Purpose | Subnet |
|---|---|---|
| Management | Network infrastructure devices | 192.168.10.0/24 |
| Servers | Virtual machines and services | 192.168.20.0/24 |
| Clients | Personal devices | 192.168.30.0/24 |
| IoT | Smart devices | 192.168.40.0/24 |

---

# Infrastructure Devices

| Device | IP Address | Notes |
|---|---|---|
| Router/Firewall | 192.168.10.1 | Default gateway |
| Managed Switch | 192.168.10.2 | Network management |
| Proxmox Host | 192.168.10.10 | Hypervisor |

---

# Virtual Machines

| VM Name | IP Address | Purpose |
|---|---|---|


---

# DHCP

## DHCP Scope

Network:
192.168.30.0/24

Range:
192.168.30.3 - 192.168.30.254

Reserved addresses:

.1 Gateway

.2 Switch

