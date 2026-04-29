# 📘 Experiment 3: Inter-VLAN Routing Using Multilayer Switch

**Date:** 20/02/2026  
**Course:** Computer Networks Lab  

---

## 🎯 Aim

To configure **Inter-VLAN Routing using a Multilayer Switch** and enable communication between different VLAN networks.

---

## 🧠 Theory

A VLAN (Virtual Local Area Network) is a logical segmentation of a network into different broadcast domains. Devices within the same VLAN can communicate directly, while communication between different VLANs requires routing, which can be performed using a multilayer switch.

---

## ⚙️ Procedure

### 🔹 Step 1: Create Network Topology

- Place **1 Switch** and **1 Multilayer Switch**
- Add **4 PCs**
- Connect all PCs to the switch using straight-through cables
- Connect switch to multilayer switch using Ethernet cable

---

### 🔹 Step 2: Assign IP Addresses

| Device | IP Address     | VLAN  |
|--------|---------------|-------|
| PC0    | 192.168.1.10  | VLAN 10 |
| PC1    | 192.168.1.20  | VLAN 10 |
| PC2    | 192.168.2.10  | VLAN 20 |
| PC3    | 192.168.2.20  | VLAN 20 |

**Subnet Mask (All):** `255.255.255.0`

---

### 🔹 Step 3: Set Default Gateway

- VLAN 10 → `192.168.1.1`  
- VLAN 20 → `192.168.2.1`  

---

### 🔹 Step 4: Configure VLAN on Switch

enable
configure terminal

vlan 10
name HR
exit

vlan 20
name IT
exit

interface range fa0/1-2
switchport mode access
switchport access vlan 10
exit

interface range fa0/3-4
switchport mode access
switchport access vlan 20
exit

interface fa0/5
switchport mode trunk
exit

🔹 Step 5: Configure Multilayer Switch (Inter-VLAN Routing)
enable
configure terminal

vlan 10
name HR
exit

vlan 20
name IT
exit

ip routing

interface vlan 10
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

interface vlan 20
ip address 192.168.2.1 255.255.255.0
no shutdown
exit

interface fa0/24
switchport mode trunk
no shutdown
exit

end
