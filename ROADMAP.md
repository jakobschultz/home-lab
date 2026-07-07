# Homelab Roadmap

This roadmap tracks the planned buildout of my homelab from design, to physical deployment, to network implementation, to services and automation.

The lab is currently in the planning phase. Hardware has been purchased, but the environment has not been powered on, configured, or tested yet.

## Phase 1 - Planning and Design

Goal: define the intended network design before powering on hardware.

* [x] Create GitHub repository
* [x] Create initial README
* [x] Create initial VLAN plan
* [x] Create initial IP addressing plan
* [x] Create initial network diagram
* [ ] Finalize hardware inventory
* [ ] Finalize physical topology
* [ ] Create port map
* [ ] Decide how the lab connects to the home network
* [ ] Decide where inter-VLAN routing will happen
* [ ] Decide where DHCP will run
* [ ] Decide what device acts as the lab firewall
* [ ] Decide which VLAN is used for management access
* [ ] Define basic firewall philosophy
* [ ] Define which VLANs can communicate with each other
* [ ] Define which VLANs are blocked from management access
* [ ] Define recovery plan if management access breaks
* [ ] Define first implementation order

### Phase 1 Design Decisions

| Decision               | Current Plan                                            | Status   |
| ---------------------- | ------------------------------------------------------- | -------- |
| Lab router/firewall    | MikroTik router                                         | Planned  |
| Switching              | Cisco Catalyst switch                                   | Planned  |
| Hypervisor             | Proxmox                                                 | Planned  |
| VLAN design            | MGMT, SERVERS, CLIENTS, IOT, GUEST                      | Planned  |
| IP addressing          | 192.168.10.0/24 through 192.168.50.0/24                 | Planned  |
| Lab-to-home connection | TBD                                                     | Open     |
| Inter-VLAN routing     | TBD                                                     | Open     |
| DHCP location          | TBD                                                     | Open     |
| Firewall policy        | Default deny between VLANs, allow only required traffic | Proposed |
| Management access      | TBD                                                     | Open     |

### Phase 1 Success Criteria

Phase 1 is complete when:

* Hardware inventory is accurate
* Physical topology is documented
* Port map is documented
* VLAN plan is complete
* IP addressing plan is complete
* Routing design is chosen
* DHCP design is chosen
* Firewall philosophy is written
* Management access plan is written
* Initial implementation order is clear

---

## Phase 2 - Physical Deployment

Goal: power on the hardware, verify access, and document the real device details.

* [ ] Power on MikroTik router
* [ ] Verify access to MikroTik
* [ ] Record MikroTik model and RouterOS version
* [ ] Power on Cisco switch
* [ ] Verify console access to Cisco switch
* [ ] Record Cisco switch model and IOS version
* [ ] Power on Proxmox server
* [ ] Verify BIOS/UEFI access
* [ ] Record CPU, RAM, storage, and NIC details
* [ ] Install Proxmox
* [ ] Assign Proxmox management IP
* [ ] Label physical cables
* [ ] Update hardware inventory with real device information

---

## Phase 3 - Network Baseline

Goal: configure the basic network foundation.

* [ ] Configure MikroTik baseline settings
* [ ] Configure Cisco switch baseline settings
* [ ] Configure management VLAN
* [ ] Configure server VLAN
* [ ] Configure client VLAN
* [ ] Configure IoT VLAN
* [ ] Configure guest VLAN
* [ ] Configure trunk links
* [ ] Configure access ports
* [ ] Configure DHCP scopes
* [ ] Configure DNS forwarding or DNS servers
* [ ] Verify each VLAN gateway
* [ ] Verify DHCP assignment
* [ ] Verify internet access from allowed VLANs
* [ ] Document verification commands

---

## Phase 4 - Routing and Firewall Rules

Goal: control traffic between VLANs and protect the home network.

* [ ] Configure inter-VLAN routing
* [ ] Allow MGMT VLAN to access network devices
* [ ] Allow CLIENTS VLAN to access approved services
* [ ] Allow SERVERS VLAN to reach required destinations
* [ ] Block CLIENTS from MGMT
* [ ] Block IOT from MGMT
* [ ] Block GUEST from internal networks
* [ ] Decide whether lab networks can access home network
* [ ] Decide whether home network can access lab networks
* [ ] Test allowed traffic
* [ ] Test blocked traffic
* [ ] Document firewall rules and reasoning

---

## Phase 5 - Proxmox and Virtualization

Goal: build the virtualization layer for lab services.

* [ ] Configure Proxmox networking
* [ ] Decide whether Proxmox uses access port or VLAN trunk
* [ ] Create VLAN-aware bridge if needed
* [ ] Create first Linux VM
* [ ] Test VM network access
* [ ] Place VM in correct VLAN
* [ ] Create VM naming convention
* [ ] Create snapshot/backup plan
* [ ] Document Proxmox network design
* [ ] Document VM deployment process

---

## Phase 6 - Core Services

Goal: deploy basic internal services.

* [ ] Deploy Linux test VM
* [ ] Deploy DNS service or DNS filtering
* [ ] Deploy monitoring platform
* [ ] Deploy Docker host
* [ ] Deploy internal dashboard or documentation service
* [ ] Assign static IPs or DHCP reservations
* [ ] Create DNS records
* [ ] Document service ports
* [ ] Document service dependencies
* [ ] Document backup needs

---

## Phase 7 - Monitoring and Troubleshooting

Goal: add visibility into the network and document problems.

* [ ] Monitor router
* [ ] Monitor switch
* [ ] Monitor Proxmox host
* [ ] Monitor key VMs
* [ ] Configure logging where practical
* [ ] Document common troubleshooting commands
* [ ] Create troubleshooting log
* [ ] Record issues, causes, fixes, and lessons learned

---

## Phase 8 - Security Improvements

Goal: harden the lab after the basic environment works.

* [ ] Disable unused switch ports
* [ ] Change default passwords
* [ ] Use SSH instead of insecure management methods where possible
* [ ] Restrict management access to MGMT VLAN
* [ ] Review firewall rules
* [ ] Review exposed services
* [ ] Review user accounts
* [ ] Document security decisions

---

## Phase 9 - Automation and Advanced Labs

Goal: practice higher-level network engineering and systems administration skills.

* [ ] Back up device configurations
* [ ] Create configuration templates
* [ ] Use Ansible for Linux configuration
* [ ] Practice Cisco configuration backups
* [ ] Practice MikroTik export backups
* [ ] Build Layer 3 switching lab
* [ ] Build OSPF lab
* [ ] Build VPN lab
* [ ] Build disaster recovery test
* [ ] Document lessons learned

---

## Long-Term Ideas

These are future ideas and are not required for the initial build.

* [ ] Active Directory lab
* [ ] WireGuard remote access
* [ ] Site-to-site VPN simulation
* [ ] Network automation
* [ ] Centralized logging
* [ ] SIEM-style security monitoring
* [ ] NAS or shared storage
* [ ] More advanced routing labs
* [ ] Public portfolio write-ups
