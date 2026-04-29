# 📘 Experiment 2: VLAN Configuration & Inter-VLAN Routing

**Date:** 17/02/2026  
**Course:** Computer Networks Lab  

---

## 🎯 Aim

To implement VLANs on a switch and configure **Inter-VLAN Routing** using one router, one switch, and four PCs in order to segment network traffic.

---

## 🧠 Theory

VLAN (Virtual Local Area Network) is used to divide a network into smaller logical networks. Devices in different VLANs cannot communicate directly unless Inter-VLAN Routing is configured using a router or multilayer switch.

---

## ⚙️ Procedure

### 🔹 Step 1: Create Network Topology

- Place **1 Router** and **1 Switch**
- Add **4 PCs**
- Connect all PCs to the switch using Ethernet cables
- Connect switch to router

---

### 🔹 Step 2: Configure IP Addresses

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

### 🔹 Step 4: Configure VLANs on Switch

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
spanning-tree portfast
exit

interface range fa0/3-4
switchport mode access
switchport access vlan 20
spanning-tree portfast
exit

🔹 Step 5: Inter-VLAN Routing (Router-on-a-Stick)
enable
configure terminal

interface g0/0
no shutdown

interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.1.1 255.255.255.0

interface g0/0.20
encapsulation dot1Q 20
ip address 192.168.2.1 255.255.255.0

end
