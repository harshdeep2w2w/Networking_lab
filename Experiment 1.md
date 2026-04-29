# Experiment 1: Network Configuration on Cisco Design

**Date:** 13/02/2026  
**Course:** Computer Networks Lab  

---

# Theory

Every host in a network requires a unique IP address for communication.  
Routers operate at **Layer 3 (Network Layer)** of the OSI model, while switches operate at **Layer 2 (Data Link Layer)**.

---

# Procedure

## 1. Setup Devices
- Place **1 Switch** and **4 PCs** in Cisco Packet Tracer.

---

## 2. Establish Connections
- Connect each PC to the switch using Ethernet cables.
- Use different switch ports for each connection.

---

## 3. Assign IP Addresses

| Device | IP Address     | Subnet Mask     |
|--------|---------------|-----------------|
| PC1    | 192.168.1.1   | 255.255.255.0   |
| PC2    | 192.168.1.2   | 255.255.255.0   |
| PC3    | 192.168.1.3   | 255.255.255.0   |
| PC4    | 192.168.1.4   | 255.255.255.0   |

---

## 4. Verify Connection
- Ensure switch port lights are **green**, indicating active connection.

---

## 5. Test Connectivity

Open terminal on each PC and run:

```bash
ping 192.168.1.2
ping 192.168.1.3
ping 192.168.1.4
