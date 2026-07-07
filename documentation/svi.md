# SVI Configuration Design

This document explains the planned switched virtual interface, or SVI, configuration for the homelab.

SVIs will be configured on the Cisco Catalyst Layer 3 switch and used as the default gateways for internal lab VLANs.

## Basic Rules to follow
1. Subnets will follow VLAN nomenclature  
Example:  
VLAN 10 will be in subnet 192.168.10.X  
VLAN 20 will be in subnet 192.168.20.X  

2. Uplink will be routed link to Mikrotik router

3. If another Layer 3 switch is added later it will use a different but similar subnet  
Example:  
VLAN 10 on the new switch will be 192.168.110.X  
Why?  
I want to treat this as if I am setting up a new layer 3 switch that will be providing connectivity on a different floor of an office than the first one.
