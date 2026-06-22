# Enterprise Network Segmentation Lab

## Project Objective
To design, deploy, and troubleshoot a segmented enterprise network using Cisco enterprise hardware. The goal was to simulate a production environment where different departments (VLANs) require logical isolation but still need to communicate via Layer 3 routing.

## Equipment List
| Device | Model | Role |
| Router | Cisco 2901 | Gateway / Inter-VLAN Routing |
| Switch | Cisco SG-200 | Layer 2 Access & Trunking |
| Workstation | Dell OptiPlex | Management Console & Testing |

## Topology Overview
- **Router-on-a-Stick** configuration deployed to route traffic between multiple VLANs.
- Single trunk link carrying 802.1Q tagged traffic from the switch to the router.
- Multiple access ports assigned to distinct VLANs to simulate separate broadcast domains.

## Configuration Highlights

### 1. VLAN Creation (Layer 2)
Created and named VLANs on the Cisco SG-200 to logically segment the network:
- VLAN 10: Management
- VLAN 20: Operations
- VLAN 30: Guest

### 2. Trunking (802.1Q)
Configured the uplink interface between the switch and router as a trunk port to allow tagged frames from all VLANs to pass through a single physical connection.

### 3. Inter-VLAN Routing (Layer 3)
On the Cisco 2901 router, configured sub-interfaces with 802.1Q encapsulation and assigned IP addresses to serve as default gateways for each VLAN.
- *Example:* `interface GigabitEthernet0/0.10` with `encapsulation dot1Q 10` and an appropriate IP address.

### 4. Troubleshooting & Validation
- **Layer 1:** Verified physical connectivity using console cables and interface status lights.
- **Layer 2:** Utilized `show mac-address-table` to verify MAC learning and confirm trunks were passing frames correctly.
- **Layer 3:** Used `show arp` inspection to resolve IP-to-MAC mismatches and executed extended pings to confirm routing paths.
- **Resolution:** Identified and corrected a sub-interface VLAN mismatch that was dropping traffic, restoring full connectivity across all segments.

## Challenges Faced & Solutions
- **Issue:** Hosts on separate VLANs could not ping their respective gateways.
- **Diagnosis:** Used console access to check ARP tables; discovered the sub-interface was configured with the wrong VLAN ID.
- **Solution:** Corrected the `encapsulation dot1Q` command to match the switch's VLAN assignment, restoring full Layer 3 communication.

## Future Enhancements
- Implement DHCP pools on the router to dynamically assign IP addresses per VLAN.
- Apply basic Access Control Lists (ACLs) to restrict inter-VLAN traffic based on security policies.
- Integrate a syslog server for centralized logging and monitoring.

## Skills Demonstrated
- Cisco IOS CLI navigation & configuration
- VLAN segmentation & 802.1Q trunking
- Sub-interface & inter-VLAN routing
- Systematic network troubleshooting (OSI Model)
- Console-based hardware management
