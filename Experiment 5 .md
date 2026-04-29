# 📘 Experiment 5: DHCP Server Configuration

**Course:** Computer Networks Lab  

---

## 🎯 Aim

To configure a **DHCP (Dynamic Host Configuration Protocol) Server** to automatically assign IP addresses to devices in a network.

---

## 🧠 Theory

DHCP is used to dynamically assign IP addresses to devices in a network. It reduces manual configuration and ensures efficient IP management. A DHCP server assigns IP address, subnet mask, default gateway, and other parameters automatically.

---

## ⚙️ Configuration Steps

### 🔹 Step 1: Configure Router Interface

enable
configure terminal

interface fa0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

###🔹 Step 2: Configure DHCP Server on Router
ip dhcp pool GUEST
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
exit

🔹 Step 3: (Optional) Exclude IP Addresses
ip dhcp excluded-address 192.168.1.1 192.168.1.10
