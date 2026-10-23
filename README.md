# Industrial IT/OT Network Architecture & Cybersecurity Simulation

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco%20Packet%20Tracer-8.x-005691?logo=cisco)
![Networking](https://img.shields.io/badge/Domain-Industrial%20Networking%20%26%20OT-blue)
![Security](https://img.shields.io/badge/Security-Access%20Control%20Lists%20(ACLs)-red)

## 📌 Overview
This project simulates a segmented, multi-VLAN industrial network architecture designed to safely bridge Information Technology (IT) and Operational Technology (OT) environments. Built in Cisco Packet Tracer, it demonstrates inter-VLAN routing using a Router-on-a-Stick setup and enforces perimeter security between administrative corporate subnets and shop-floor manufacturing cells using Extended Access Control Lists (ACLs).

---

## 🏗 Network Architecture & Segmentation

The topology isolates critical manufacturing equipment (PLCs, SCADA servers) from non-critical administrative corporate traffic to mitigate cyber risks and traffic congestion.

| VLAN ID | Subnet Name | IP Range | Description / Assignment |
| :--- | :--- | :--- | :--- |
| **VLAN 10** | `IT_Admin` | `192.168.10.0/24` | Engineering & IT Administration Workstations |
| **VLAN 20** | `Finance` | `192.168.20.0/24` | Corporate & Administrative Workstations |
| **VLAN 30** | `OT_Plant` | `192.168.30.0/24` | Shop-floor PLCs, SCADA Servers, IoT Controllers |
| **VLAN 99** | `Management` | `192.168.99.0/24` | Network Devices In-Band Management |

---

## 🔒 Security Policy & ACL Implementation

To preserve OT integrity, strict communication policies are implemented at the router interface layer:
1. **Deny General Corporate Access to OT:** All traffic originating from corporate/finance subnets (VLAN 20) targeting the manufacturing plant cell (VLAN 30) is dropped.
2. **Permit Authorized IT Access:** IT Engineering (VLAN 10) retains administrative access to VLAN 30 for maintenance, monitoring, and debugging.

### Core Router ACL Configuration
```text
ip access-list extended OT_SECURITY
 deny ip 192.168.20.0 0.0.0.255 192.168.30.0 0.0.0.255
 permit ip any any
exit

interface GigabitEthernet0/0.30
 ip access-group OT_SECURITY out
exit
