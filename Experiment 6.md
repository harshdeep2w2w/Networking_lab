# Experiment 6

## Aim:
Configure Network Address Translation (NAT) to allow internal devices to access external networks.

---

## Theory:
Static NAT maps a single private IP address to a single public IP address.  
In this configuration, it serves two purposes:

(i) **Translation:**  
It allows internal hosts (inside) to be represented by a public IP when communicating with the external network (outside).

(ii) **Security:**  
By default, NAT on a Cisco router allows outgoing traffic but drops unsolicited incoming traffic to protect private devices.

---

## Procedure:

### Step 1: Topology Design

- Outside Network:  
  Connect PC0 & PC1 to Switch0, linked to Router0  
  (Network: 10.10.10.0)

- Inside Network:  
  Connect Server0 & PC2 to Switch1, linked to Router1  
  (Network: 20.20.20.0)

- Connection:  
  Establish a serial link between Router0 & Router1  
  (Network: 30.30.30.0)

---

### Step 2: Routing Configuration

Configure a static route on Router0 so it knows how to reach the internal network via Router1:

ip route 20.20.20.0 255.255.255.0 30.30.30.2

#Step 3: Static NAT Setup (On Router0)
Designate NAT roles for interfaces:
interface fa0/0
ip nat inside

interface serial 2/0
ip nat outside

## Define static mapping:
ip nat inside source static 10.10.10.1 30.30.30.1
ip nat inside source static 10.10.10.2 30.30.30.2

# Step 4: Testing & Results
Internal to External (PC2 → PC1):
Successful. Packet is initiated from inside and allowed through gateway.
External to Internal (PC1 → PC2):
Failed. Packet reaches Router0 but is blocked since unsolicited external traffic is not allowed.
Ping Test:
ping 30.30.30.1

Result:

Static NAT was successfully configured on the router.
The experiment demonstrated that NAT allows internal hosts to communicate with external networks while securing the internal network by blocking unauthorized incoming traffic.
