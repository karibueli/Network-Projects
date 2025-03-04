# Routing & Switching LAB
Objective
Configure OSPF, EIGRP, ACLs, and VLANs using your Cisco vIOS router, NXOSv9k switch, and 3 virtual PCs in EVE-NG.

 ## Topology Overview:
- **Cisco NXOS Switch**: Acts as a core switch with VLANs configured for segmentation.
- **Cisco vIOS Router**: Provides routing between VLANs and runs both OSPF and EIGRP to demonstrate routing protocols.
- **3 Virtual PCs**: Placed in separate VLANs to simulate devices in different network segments.

# The Topology Diagram
  
![R S Topology 1](https://github.com/user-attachments/assets/f095af1b-15c4-4dc3-9bc7-712d750a5c70)

# Tasks Objectives

1. **VLAN Configuration:**
    
    - VLAN 10 (PC1): 192.168.10.0/24
    - VLAN 20 (PC2): 192.168.20.0/24
    - VLAN 30 (PC3): 192.168.30.0/24
2. **Inter-VLAN Routing:**
    
    - Router subinterfaces for inter-VLAN routing.
3. **Routing Protocols:**
    - Configure OSPF for VLAN 10 and VLAN 20.
    - Configure EIGRP for VLAN 30.
    - Redistribute routes between OSPF and EIGRP.
4. **Access Control Lists (ACLs):**
    - Block/allow traffic between specific VLANs.
    - Allow HTTP/HTTPS traffic while blocking other protocols.
