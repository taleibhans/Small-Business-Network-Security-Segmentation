# Small Business Network Security & High Availability Lab

## Overview

This project is a simulated small-business network designed around segmentation, access control, secure wireless networking, threat detection, and infrastructure availability.

The goal was to design a network that avoids placing every device into a single flat LAN. Systems were separated based on their purpose and level of trust using VLANs, while access control rules determine which networks are allowed to communicate.

The final topology also includes redundant routing and Layer 3 switching infrastructure to reduce single points of failure.

## Environment

This lab was built using NetworkSim.

The environment includes:

- Managed Layer 2 access switching
- Redundant Layer 3 switches
- Redundant company routers
- Firewall protection
- Inline IDS/IPS
- VLAN segmentation
- Inter-VLAN routing
- ACL-based traffic control
- Staff and management wireless networks
- Separate guest Wi-Fi
- Internal servers and storage
- IP security cameras and video recording
- VoIP devices
- DMZ
- Honeypot systems
- High-availability switching

## Network Topology

![Small Business Network Topology](Screenshots/network-topology.png)

## Network Architecture

Endpoint devices connect through a managed Layer 2 access switch.

The access switch carries the VLANs toward a redundant Layer 3 switching pair, which provides routing between internal networks.

The design also includes redundant company routers, perimeter firewalling, and an inline IPS before traffic reaches the external network.

At a high level:

Internet  
↓  
ISP / Modem  
↓  
Firewall and IPS  
↓  
Redundant Company Routers  
↓  
Redundant Layer 3 Switches  
↓  
Managed Layer 2 Access Switch  
↓  
Endpoints, servers, APs, cameras, phones, and DMZ systems

## VLAN Design

| VLAN | Purpose | Network |
|------|---------|---------|
| 10 | Servers | 10.0.1.0/24 |
| 20 | Security | 10.0.2.0/24 |
| 30 | Management | 10.0.3.0/24 |
| 40 | Staff | 10.0.4.0/24 |
| 50 | Voice | 10.0.5.0/24 |
| 60 | Guest | 10.0.6.0/24 |
| 70 | DMZ | 10.0.7.0/24 |

## Managed Access Layer

A managed Layer 2 switch is used as the access switch for endpoint connectivity.

Devices from different networks can connect to the same physical switch while remaining logically separated through VLAN assignments.

The access switch connects upstream to both Layer 3 switches.

This allows the access layer to reach either Layer 3 device rather than requiring every endpoint to connect directly to both switches.

## Layer 3 Switching and High Availability

Two Layer 3 switches are used in the core of the network.

They are configured as a high-availability pair using an active/passive design.

During normal operation, the active Layer 3 switch handles routing.

The standby switch provides failover capability if the primary Layer 3 switch becomes unavailable.

This reduces the Layer 3 switching infrastructure as a single point of failure.

## Router Redundancy

The network includes two company routers.

The secondary router provides redundant routing infrastructure so the topology is not dependent on a single company router.

This demonstrates the role of redundant network paths in improving availability.

## Firewall and Threat Prevention

A firewall is placed at the perimeter of the business network.

The firewall controls traffic crossing between trusted and untrusted environments.

An inline IDS/IPS component is also included in the network path.

The firewall determines whether traffic is permitted by policy, while the IPS provides additional inspection for potentially malicious traffic that may otherwise be allowed through the network.

## Server Infrastructure

The server environment includes services such as:

- DNS
- FTP
- HTTP/HTTPS
- NAS / shared storage
- Network printing

These systems are placed in VLAN 10 rather than sharing the normal employee network.

## Security Infrastructure

Security cameras are placed in VLAN 20.

The surveillance environment is separated from employee devices to reduce unnecessary communication between security/IoT equipment and user workstations.

Camera footage can be stored and managed using an NVR or video-surveillance server.

## Management Network

Management systems are placed in VLAN 30.

The management network is treated as a higher-trust administrative environment.

Management devices can access resources required for administration while lower-trust networks are restricted from accessing the management VLAN.

## Staff Network

Employee systems are placed in VLAN 40.

This includes wired staff workstations and employee wireless devices.

Staff users can access approved business resources and the internet but are restricted from accessing higher-trust administrative systems where not required.

## Wireless Segmentation

The employee wireless access point supports separate wireless networks.

The Management SSID is associated with VLAN 30.

The Staff SSID is associated with VLAN 40.

This allows different users to use the same physical wireless infrastructure while remaining on separate logical networks.

Guest Wi-Fi uses a separate guest access point and VLAN 60.

Guest devices are permitted internet access but are isolated from internal corporate resources.

## Voice Network

VoIP devices are placed in VLAN 50.

Separating voice traffic from general employee traffic provides cleaner segmentation and allows voice services to be controlled independently.

## DMZ and Honeypots

The DMZ uses VLAN 70.

The environment contains honeypot endpoints and a honeypot server designed to act as decoy systems.

Because honeypots may intentionally receive suspicious or malicious traffic, they are treated as untrusted.

ACL rules prevent the DMZ from initiating connections into trusted corporate networks.

This limits lateral movement if one of the honeypot systems becomes compromised.

## Access Control Strategy

The network uses a deny-by-default ACL policy.

Traffic that is not explicitly permitted is blocked.

Examples include:

- Management → approved internal systems: Permit
- Staff → Internet: Permit
- Staff → Management: Deny
- Guest → Internet: Permit
- Guest → internal VLANs: Deny
- Security → Staff: Deny
- DMZ → internal VLANs: Deny

The ACL uses top-down, first-match processing, so rule order is important.

## Security Principles Demonstrated

This lab applies several security concepts:

- Network segmentation
- Least privilege
- Deny-by-default access control
- Guest isolation
- DMZ isolation
- Management-plane separation
- IoT/security-device isolation
- Wireless SSID-to-VLAN mapping
- Threat detection and prevention
- Defense in depth
- Reduction of lateral movement

## Availability and Fault Tolerance

Security was not the only design goal.

The network also includes redundant infrastructure to reduce the impact of device failure.

The design includes:

- Primary and backup Layer 3 switches
- Active/passive Layer 3 failover
- Primary and backup company routers
- Multiple upstream network paths

The managed Layer 2 access switch remains a potential single point of failure in this version of the design.

In a production environment with higher availability requirements, a second access switch and dual-homed critical systems could be added.

## What I Learned

This lab helped me understand that physical connectivity and logical network membership are not the same thing.

Devices can use the same managed switching infrastructure while remaining separated through VLANs.

I also learned that VLANs alone do not provide complete security. Once inter-VLAN routing exists, ACLs or firewall policies are still required to control communication between networks.

Designing the guest and DMZ networks also reinforced the importance of separating lower-trust systems from internal business resources.

Adding the firewall and IPS helped demonstrate the difference between traffic filtering and threat inspection.

The final stage of the lab focused on availability. Adding redundant Layer 3 switches and routers showed how network design must account not only for security incidents but also for equipment failure.

I also identified that the single managed access switch remains a single point of failure, demonstrating the importance of recognizing the limitations of a design rather than assuming redundancy at one layer makes the entire network fault tolerant.

## Skills Demonstrated

- Small-business network design
- Layer 2 switching
- Managed switching
- Layer 3 switching
- VLAN configuration
- VLAN segmentation
- Inter-VLAN routing
- Access control lists
- Firewall architecture
- IDS/IPS concepts
- Defense in depth
- DMZ architecture
- Honeypot isolation
- Guest network isolation
- Wireless SSID segmentation
- DNS
- FTP
- HTTP/HTTPS
- NAS
- Network printing
- IP camera networking
- NVR concepts
- VoIP networking
- Least privilege
- Deny-by-default access control
- Network redundancy
- High availability
- Active/passive failover
- Fault tolerance
- Identification of single points of failure
