# Routing & Switching LAB
Objective
Configure OSPF, EIGRP, ACLs, and VLANs using your Cisco vIOS router, NXOSv9k switch, and 3 virtual PCs in EVE-NG.

 # **1. Topology** 
 ## Overview:
- **Cisco NXOS Switch**: Acts as a core switch with VLANs configured for segmentation.
- **Cisco vIOS Router**: Provides routing between VLANs and runs both OSPF and EIGRP to demonstrate routing protocols.
- **3 Virtual PCs**: Placed in separate VLANs to simulate devices in different network segments.

 ## The Topology Diagram
  
![R S Topology 1](https://github.com/user-attachments/assets/f095af1b-15c4-4dc3-9bc7-712d750a5c70)

# **2. Tasks Objectives**

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


# **3. IP Addressing Scheme**

| **Device**            | **Interface**         | **IP Address** | **Subnet Mask** | **Purpose**               |
| --------------------- | --------------------- | -------------- | --------------- | ------------------------- |
| **vIOS Router**       | GE1.10 (Subinterface) | 192.168.10.1   | 255.255.255.0   | VLAN 10 Gateway           |
|                       | GE1.20 (Subinterface) | 192.168.20.1   | 255.255.255.0   | VLAN 20 Gateway           |
|                       | GE1.30 (Subinterface) | 192.168.30.1   | 255.255.255.0   | VLAN 30 Gateway           |
|                       | GE1                   | 10.0.0.1       | 255.255.255.252 | Connection to NXOS Switch |
| **Cisco NXOS Switch** | VLAN 1                | 10.0.0.2       | 255.255.255.252 | Router connection         |
|                       | VLAN 10               | 192.168.10.2   | 255.255.255.0   | Host for vPC1             |
|                       | VLAN 20               | 192.168.20.2   | 255.255.255.0   | Host for vPC2             |
|                       | VLAN 30               | 192.168.30.2   | 255.255.255.0   | Host for vPC3             |
| **vPC1**              | Ethernet0             | 192.168.10.10  | 255.255.255.0   | Device in VLAN 10         |
| **vPC2**              | Ethernet0             | 192.168.20.10  | 255.255.255.0   | Device in VLAN 20         |
| **vPC3**              | Ethernet0             | 192.168.30.10  | 255.255.255.0   | Device in VLAN 30         |


# **4. Configuration Setup**

#### **Cisco NXOS Switch:**

1. **VLAN Configuration:**
    
    ```bash
    conf t
    vlan 10
    name VLAN10
    vlan 20
    name VLAN20
    vlan 30
    name VLAN30
    ```
    
2. **Interface Configuration:** Assign interfaces to VLANs for the PCs:
    
    ```bash
    interface Ethernet1/1
    switchport mode access
    switchport access vlan 10
    
    interface Ethernet1/2
    switchport mode access
    switchport access vlan 20
    
    interface Ethernet1/3
    switchport mode access
    switchport access vlan 30
    ```
    
3. **Router Connection (Trunk Port):**
    
    ```bash
    interface Ethernet1/4
    switchport mode trunk
    switchport trunk allowed vlan 10,20,30


4. **Management IP Address:**
    
    ```bash
    interface vlan 1
    ip address 10.0.0.2 255.255.255.252
    no shutdown
    ```  

---
#### **Cisco vIOS Router:**

1. **Subinterface Configuration for Inter-VLAN Routing:**
    
    ```bash
    conf t
    interface GigabitEthernet0/0.10
    encapsulation dot1Q 10
    ip address 192.168.10.1 255.255.255.0
    
    interface GigabitEtherne0/0.20
    encapsulation dot1Q 20
    ip address 192.168.20.1 255.255.255.0
    
    interface GigabitEthernet0/0.30
    encapsulation dot1Q 30
    ip address 192.168.30.1 255.255.255.0


2. **Routing Configuration:**
    
    - Configure OSPF:
        
        ```bash
        router ospf 1
        network 192.168.10.0 0.0.0.255 area 0
        network 192.168.20.0 0.0.0.255 area 0
        ```
        
    - Configure EIGRP:
        
        ```bash
        router eigrp 1
        network 192.168.30.0
        ```

3. **Route Redistribution:**
    
    ```bash
    router ospf 1
    redistribute eigrp 1
    
    router eigrp 1
    redistribute ospf 1 metric 1000 1 255 1 1500


# **5. Connectivity & Protocol Test w/o security Restrictions**

**The general test**

As screenshot below showing combination of PING Tests from vPC1, vPC2 & vPC3 across VLAN 10,20&30 responded as required. Screenshot also indicated packet capture from wireshark on port E1/4 to test various protocols including OSPF, EIGRP and ICMP

![Pasted image 20241212102308](https://github.com/user-attachments/assets/e68738dc-525b-41a1-a1e3-98ec26e73e7f)
