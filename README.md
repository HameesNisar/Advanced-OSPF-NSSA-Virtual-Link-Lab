# Advanced OSPF Multi-Area NSSA Lab (GNS3)

This project demonstrates an advanced OSPF deployment using **multiple area types**, **NSSA design**, **virtual links**, **authentication**, **route summarization**, and **multi-protocol redistribution** with **RIP** and **EIGRP**.  
The lab is built on GNS3 and designed to simulate real-world enterprise routing scenarios.

---

## 📌 Topology Diagram

<img width="1861" height="895" alt="area types new" src="https://github.com/user-attachments/assets/809ce08f-ed44-4546-aba2-fc30e01e3145" />


---

## 📘 Overview

This lab focuses on advanced concepts of **OSPF network design**, including:

- Multi-area OSPF deployment  
- NSSA (Not-So-Stubby Areas)  
- ABR and ASBR identification  
- MD5 authentication  
- Virtual links  
- Route summarization  
- Redistribution between OSPF ↔ EIGRP and OSPF ↔ RIP  
- DR/BDR election control  

Throughout the network, routers also run **loopback summarization** and **area-based behavior tuning** to optimize routing updates and LSA flooding.

---

## 🔧 Routing Protocols in Use

### **EIGRP**
- **ASN 166** — Authentication enabled (MD5)  
- **ASN 178** — Authentication enabled (MD5)  
- EIGRP is connected **only** to OSPF (no EIGRP-to-EIGRP redistribution)

### **RIP**
- Used in a small isolated region  
- Redistributed into OSPF where necessary

### **OSPF**
- Backbone Area **0**  
- Multiple NSSA areas  
- Area 180 connected via **Virtual Link**  
- MD5 authentication in **Area 123**

---

## 🧩 OSPF Area Design Summary

### **Area 0 – Backbone**
Area 0 acts as the **central transit area**.  
All other OSPF areas must connect **directly** or **indirectly via a virtual link** to Area 0.

### **NSSA Areas**
The following are **NSSA areas**:
- Area 123  
- Area 151  
- Area 156  
- Area 167  
- Area 190  

NSSAs allow:
- External routes to enter the area using type-7 LSAs  
- Prevent flooding of type-5 LSAs  
- Redistribution inside the area without turning it into a regular area

### **Area 180 – Virtual Link Requirement**
Area 180 is **not directly connected to Area 0**, which violates OSPF rules.  
Therefore, a **virtual link** is created between:

- **R22 (ABR)**  
- **R1 (Backbone router)**  

This allows Area 180 to participate properly in the OSPF backbone hierarchy.

---

## 🔁 Redistribution Summary

Redistribution in this lab occurs only where necessary:

- **RIP → OSPF**  
- **OSPF → RIP**  
- **EIGRP → OSPF**  
- **OSPF → EIGRP**

There is **no EIGRP-to-EIGRP redistribution**, since the design does not contain multiple connected EIGRP ASNs at the same point.

Redistribution points also act as **ASBRs** (Autonomous System Boundary Routers), such as:
- R8  
- R11  
- R15  
- R16  
(and others depending on their role in the design)

---

## 🔐 Authentication

### **EIGRP MD5 Authentication**
Enabled in:
- **ASN 166**
- **ASN 178**

Ensures neighbor integrity and prevents malicious route injection.

### **OSPF MD5 Authentication**
Enabled in:
- **Area 123**

Enhanced security prevents unauthorized routers from forming OSPF adjacencies.

---

## ➕ Summarization

Routers with multiple loopbacks (shown in red in the topology) use **manual summarization** to reduce routing table size and LSA flooding.  
Summaries were configured at the area boundaries or redistribution points depending on topology placement.

---

## 🏗 DR/BDR Election

OSPF DR/BDR was manually influenced using **OSPF interface priority**:

- **Router R6 → DR**  
- **Router R7 → BDR**

This is important on multi-access segments to:
- Control LSA flooding  
- Maintain predictable OSPF behavior  
- Ensure stable adjacencies

---

## 🔗 Virtual Link Explanation

A virtual link is required when:
- A non-backbone area (Area 180 here) does **not have a direct physical/logical path** to Area 0  
- OSPF rules require **all areas to connect to the backbone**  

In this lab:
- R22 and R1 form a **virtual link**  
- Area 180 becomes logically connected to Area 0  
- R22 becomes a valid ABR after this link

This restores the required OSPF hierarchy.

---

## 🔍 Troubleshooting & Validation Commands

### **Check Authentication Status**
```
show running-config | include authentication
show ip ospf interface
show ip eigrp neighbors detail
```

## Check Redistribution
```
show ip route
show ip protocols
show ip ospf database external
show ip eigrp topology
```

## Verify Virtual Link
```
show ip ospf virtual-links
```

## Verify DR/BDR Roles
```
show ip ospf interface
```

## 🏁 Conclusion
---
This lab demonstrates advanced OSPF design concepts including:
- Multi-area OSPF with NSSA regions
- Backbone behavior and area hierarchy
- Virtual link implementation
- Secure routing with MD5 authentication
- Controlled redistribution between dynamic routing protocols
- Route summarization efficiency
- DR/BDR control for predictable OSPF operation

---

This project serves as a strong example of enterprise-level routing design and advanced OSPF implementation using Cisco IOS and GNS3.

---
