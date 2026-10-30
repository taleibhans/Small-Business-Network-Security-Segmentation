# Network Implementation and Design Notes

## 1. Scenario

This project simulates the network of a small business with multiple user groups, internal services, wireless networks, security infrastructure, voice devices, guest access, and externally exposed decoy systems.

The network was designed around four main requirements:

1. Segmentation
2. Security
3. Availability
4. Manageability

The final design avoids a flat network and instead separates systems based on purpose and trust level.

---

## 2. Physical and Logical Architecture

The network contains several layers with different responsibilities.

### Managed Layer 2 Access Switch

The managed Layer 2 switch provides connectivity for endpoint devices.

Its responsibilities include:

- Connecting wired endpoints
- Assigning switch ports to VLANs
- Carrying multiple VLANs toward the Layer 3 infrastructure
- Supporting separate logical networks over shared physical infrastructure

### Layer 3 Switches

Two Layer 3 switches provide routing between VLANs.

The switches operate as a high-availability pair.

The primary device operates as the active switch and the secondary device operates as the standby switch.

If the primary Layer 3 switch becomes unavailable, the standby device can provide failover.

### Company Routers

Two company routers are included to reduce dependency on a single routing device.

They provide upstream routing between the internal network infrastructure and the perimeter network.

### Firewall

The firewall forms part of the perimeter security architecture.

Its purpose is to allow or deny traffic according to security policy.

### IDS/IPS

An inline IDS/IPS component provides additional inspection of network traffic.

The firewall answers:

"Should this traffic be allowed?"

The IPS adds another question:

"Even if this traffic is allowed, does it appear malicious?"

This creates an additional layer of protection.

---

## 3. VLAN Structure

### VLAN 10 - Servers

Network:

10.0.1.0/24

Systems include:

- DNS server
- FTP server
- HTTP/HTTPS server
- NAS
- Network printers

Separating servers from employee endpoints reduces direct exposure of infrastructure services.

### VLAN 20 - Security

Network:

10.0.2.0/24

Systems include:

- IP security cameras
- NVR / surveillance server

Security and IoT-style devices are separated from normal workstations.

### VLAN 30 - Management

Network:

10.0.3.0/24

Systems include:

- Management PCs
- Management laptops
- Management wireless clients

This network is treated as a higher-trust administrative VLAN.

### VLAN 40 - Staff

Network:

10.0.4.0/24

Systems include:

- Staff workstations
- Staff mobile devices
- Staff wireless clients

### VLAN 50 - Voice

Network:

10.0.5.0/24

Systems include:

- VoIP phones

Voice traffic is separated from general user traffic.

### VLAN 60 - Guest

Network:

10.0.6.0/24

Systems include:

- Guest wireless access point
- Guest phone
- Guest tablet

Guest devices are allowed internet connectivity but are restricted from internal business networks.

### VLAN 70 - DMZ

Network:

10.0.7.0/24

Systems include:

- Honeypot endpoint 1
- Honeypot endpoint 2
- Honeypot server

DMZ systems are treated as untrusted and cannot freely initiate connections into internal VLANs.

---

## 4. Wireless Design

The employee wireless infrastructure supports separate SSIDs.

Management SSID:
VLAN 30

Staff SSID:
VLAN 40

This demonstrates how multiple wireless networks can use the same access point while assigning clients to different VLANs.

The guest wireless environment uses a separate access point associated with VLAN 60.

Guest devices are isolated from corporate resources.

---

## 5. Inter-VLAN Routing

The Layer 3 switches provide routing between VLANs.

Without filtering, inter-VLAN routing could allow devices from one VLAN to communicate with another.

For that reason, routing is combined with ACL-based access control.

VLAN segmentation separates the networks.

ACLs determine whether communication between those networks is actually permitted.

---

## 6. ACL Policy

The network uses a deny-by-default policy.

If no ACL rule explicitly permits traffic, the traffic is blocked.

The ACL is processed from top to bottom using first-match behavior.

This means more specific rules must appear before broader rules that would otherwise match the same traffic.

Examples of security policy include:

Management → approved internal resources:
Permit

Staff → Internet:
Permit

Staff → Management:
Deny

Security → Staff:
Deny

Guest → Internet:
Permit

Guest → internal networks:
Deny

DMZ → trusted internal networks:
Deny

This reduces unnecessary communication and limits lateral movement between networks.

---

## 7. DMZ and Honeypot Security

The honeypot environment is placed in VLAN 70.

Honeypots are intentionally designed to attract or observe suspicious activity and therefore must not be treated as trusted internal systems.

The DMZ is isolated from:

- Server VLAN
- Security VLAN
- Management VLAN
- Staff VLAN
- Voice VLAN

If a honeypot becomes compromised, the attacker should not be able to use that system as a direct path into trusted networks.

---

## 8. Threat Detection and Prevention

The network includes an IDS/IPS component.

The firewall handles policy-based traffic filtering.

The IDS/IPS provides deeper inspection for suspicious or malicious activity.

Using both demonstrates defense in depth.

A connection being permitted through the firewall does not automatically mean the traffic itself is safe.

---

## 9. High Availability

The network includes redundancy at both the Layer 3 switching and routing layers.

### Layer 3 Switch Redundancy

Two Layer 3 switches are configured as an HA pair.

Primary:
L3-1

Secondary:
L3 Backup

Operating mode:
Active / Passive

The active switch handles normal Layer 3 operations.

The standby device provides failover if the active switch becomes unavailable.

### Router Redundancy

Two company routers are included in the topology.

The second router provides an alternate infrastructure path if the primary routing device fails.

---

## 10. Access Layer Design

Endpoint devices do not need separate physical connections to both Layer 3 switches.

Instead, endpoints connect to the managed Layer 2 access switch.

The access switch connects upstream to the redundant Layer 3 infrastructure.

Conceptually:

Endpoints
   |
Managed L2 Access Switch
   |               |
L3 Primary      L3 Backup

This keeps endpoint connectivity manageable while still allowing redundancy upstream.

---

## 11. Remaining Single Point of Failure

The current design uses one managed Layer 2 access switch.

Although the Layer 3 switches and routers include redundancy, failure of the access switch could still disconnect endpoint devices.

This is an intentional limitation identified during the design review.

A production implementation requiring greater availability could add:

- A second managed access switch
- Redundant uplinks
- Dual-homed critical servers
- NIC teaming or bonding where appropriate

Recognizing this limitation is important because redundancy at one layer does not automatically make the entire network fault tolerant.

---

## 12. Security and Availability Goals

The final network demonstrates both security and resilience.

Security controls focus on:

- Segmentation
- Least privilege
- Access control
- Guest isolation
- DMZ isolation
- Management protection
- Threat detection
- Reduction of lateral movement

Availability controls focus on:

- Redundant Layer 3 switches
- Active/passive failover
- Redundant company routers
- Alternate network paths

Together, these provide a more complete small-business network architecture.

---

## 13. Key Takeaways

This project reinforced several important networking concepts.

A physical switch can carry devices from multiple logical networks without placing those devices in the same VLAN.

Layer 2 switching provides endpoint connectivity while Layer 3 switching provides routing between networks.

VLANs provide segmentation, but ACLs or firewall policies are required to control routed communication.

Wireless SSIDs can be mapped to different VLANs to separate users even when they share wireless infrastructure.

DMZ and guest networks should be treated as lower-trust environments and restricted from internal systems.

Firewalls and IPS technologies perform related but different security functions.

Finally, high availability must be designed at multiple layers. Adding a backup core switch does not automatically eliminate every possible single point of failure.

The final topology combines network segmentation, security policy, threat prevention, routing, wireless networking, server infrastructure, and high availability into one cohesive small-business design.
