 # 📘 Experiment 4: Access Control List (ACL) Configuration

**Course:** Computer Networks Lab  

---

## 🎯 Aim

To configure **Access Control Lists (ACLs)** to control network traffic and restrict access based on IP addresses.

---

## 🧠 Theory

ACLs (Access Control Lists) are used to control network traffic by permitting or denying packets based on conditions such as source IP address, destination IP, protocol, and port numbers.

---

## 🔢 Types of ACL

1. **Standard ACL (1–99)**
   - Filters traffic based only on **source IP address**

2. **Extended ACL (100–199)**
   - Filters traffic based on **source IP, destination IP, protocols, and ports**

---

## 🌐 Network Setup

| Device     | Interface | IP Address     |
|------------|----------|---------------|
| Router0    | Fa0/0    | 192.168.1.1   |
| Router1    | Fa0/0    | 192.168.2.1   |
| PC0        | -        | 192.168.1.2   |
| Server0    | -        | 192.168.2.2   |

---

## ⚙️ Configuration

### 🔹 Step 1: Create ACL

enable
configure terminal

access-list 10 deny host 192.168.1.2
access-list 10 permit any

🔹 Step 2: Apply ACL to Interface
interface serial 0/0
ip access-group 10 out
exit

end
