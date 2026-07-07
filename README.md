# Jakob's Homelab Infrastructure

This repository documents the design and buildout of a small enterprise-style homelab focused on networking, virtualization, Linux administration, monitoring, security, and automation.

The goal of this project is to practice real infrastructure design habits in a controlled lab environment. This includes planning the network, documenting design decisions, segmenting services, configuring routing and switching, deploying virtualized systems, and eventually adding monitoring, backups, and automation.

## Project Goals

This homelab is being built to develop hands-on experience with:

* Network segmentation using VLANs
* IP addressing and subnet planning
* Router, firewall, and switch configuration
* Proxmox virtualization
* Linux server administration
* Self-hosted infrastructure services
* Monitoring and logging
* Secure remote access
* Backup planning
* Infrastructure documentation
* Configuration automation

## Lab Scope

The lab is designed around a small physical network with dedicated routing, switching, and virtualization hardware. The environment is intended to simulate common infrastructure concepts used in business networks while staying practical for a home setup.

The design prioritizes:

* Clear network separation
* Documented IP and VLAN planning
* Safe integration with the existing home network
* Repeatable configuration practices
* Long-term documentation quality
* Learning value over unnecessary complexity

## Core Technologies

| Technology        | Purpose                                              |
| ----------------- | ---------------------------------------------------- |
| MikroTik RouterOS | Lab routing, firewalling, and network segmentation   |
| Cisco IOS         | Switching, VLANs, trunking, and access port practice |
| Proxmox VE        | Virtualization platform                              |
| Linux             | Server administration and service hosting            |
| Docker            | Application hosting inside Linux virtual machines    |
| WireGuard         | Secure remote access                                 |
| Grafana           | Monitoring dashboards                                |
| Ansible           | Future configuration automation                      |

## Design Principles

This lab is intentionally documented like a small production environment.

Key principles:

* Plan before building
* Keep the home network and lab network separated
* Use VLANs for logical segmentation
* Avoid exposing internal services directly to the internet
* Track design decisions and changes
* Build in phases
* Document both the final configuration and the reasoning behind it

## Roadmap

The project is organized into phases covering planning, network foundation, virtualization, core services, monitoring, security, and automation.

See [ROADMAP.md](ROADMAP.md) for the current phase, next steps, and future improvements.


## Network Diagram

![Network Diagram](diagrams/network-diagram.png)

