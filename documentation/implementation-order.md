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

Why?

Pretty hard to configure stuff when its not on.

## 2. Baseline Device Access

Establish basic administrative access to each device.

Why?

Pretty hard to configure stuff when you cant log in.

## 3. Management Network

Building the first management network before adding the rest of the VLANs.

Why?

Building out the management network first will be fundamental for setting up the rest of the network more easily!

## 4. Catalyst Layer 3 Switching

Prepare the Catalyst switch to act as the internal Layer 3 routing device.

(Basically just setup the SVI's and default route so everything that should talk to each other can talk to each other.)

## 5. Inter-VLAN Routing

Verify that VLAN-to-VLAN routing works on the Catalyst switch.

## 6. Catalyst-to-MikroTik Uplink

Connect the Catalyst Layer 3 switch to the MikroTik router.

## 7. Internet Access for Lab VLANs

Configure upstream access so approved lab VLANs can reach the internet.

## 8. Proxmox Installation and Networking

Install and configure the virtualization host after the network foundation is stable.

## 9. Core Internal Services
Deploy basic services after routing and Proxmox networking are working.

Possible services:

DNS
DHCP, if not handled by the router
Monitoring
Docker host
Backup target
Linux administration VM
Test web service

## 10. Monitoring and Logging

Add monitoring after there are devices and services worth monitoring.

## 11. Backup Planning and Testing
Add backups before the lab becomes too complex.

Backup configs and important VM data

## 12. Security Tightening

Add stricter controls after basic connectivity has been proven.

## 13. Automation

Add automation only after the manual design is understood.

Possible tools:

Ansible
Backup scripts
Configuration collection scripts
Health check scripts
Update scripts
