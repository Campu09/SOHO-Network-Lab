# 🛡️ Project: Secure SOHO Network Architecture

![Network Status](https://img.shields.io/badge/Status-Design_Completed-success)
![CompTIA](https://img.shields.io/badge/Aligned_with-CompTIA_Network+-blue)

## 🎯 Project Objective
Design and document a network architecture for a Small Office/Home Office (SOHO) by applying **Defense** and **Least Privilege** principles. This lab demonstrates proficiency in network segmentation (VLANs/802.1Q), secure administration (Bastion Host), and traffic control (ACLs/Firewall).

---

## 🏗️ Network Topology
<img src="SOHO_LAB1.png" width="100%" alt="Network Topology">

The network segments collision and broadcast domains into 4 distinct security zones:
1. **VLAN 10 (DMZ - Public Facing):** Hosts services exposed to the Internet (Web Server).
2. **VLAN 20 (STAFF - Internal):** Employee devices. Internet access allowed; access to DMZ is restricted.
3. **VLAN 30 (GUEST - Untrusted):** Isolated Wi-Fi network. Only outbound Internet access is permitted.
4. **VLAN 99 (MANAGEMENT - Restricted):** Critical zone. Contains the Bastion Host and the SIEM server.

---

## 📋 IP Addressing Plan (IPv4)

|      Device / Zone     | Interface | VLAN | Network (CIDR) | Assigned IP |         Role / Notes        |
| :--------------------- | :-------- | :--- | :------------- | :---------- | :-------------------------- |
| **Firewall (NGFW)**    | Gi0/0     | WAN  |     Public     | `ISP DHCP`  |      Internet Connection    |
| **Firewall (Gateway)** | Gi0/1.10  | 10   | `10.0.10.0/24` | `10.0.10.1` |         DMZ Gateway         |
| **Firewall (Gateway)** | Gi0/1.20  | 20   | `10.0.20.0/24` | `10.0.20.1` | Staff Gateway (DHCP Server) |
| **Firewall (Gateway)** | Gi0/1.30  | 30   | `10.0.30.0/24` | `10.0.30.1` | Guest Gateway (DHCP Server) |

| **Firewall (Gateway)** | Gi0/1.99  | 99   | `10.0.99.0/24` | `10.0.99.1` |         Mgmt Gateway        |
| **Web Server**         | eth0      | 10   | `10.0.10.0/24` | `10.0.10.50`|        Static IP            |
| **Bastion Host**       | eth0      | 99   | `10.0.99.0/24` | `10.0.99.10`|          Static IP          |
| **SIEM Server**        | eth0      | 99   | `10.0.99.0/24` | `10.0.99.20`| Receives Syslog (UDP 514)   |

---

## 🔒 Security Policies & Access Control Lists (ACLs)

To ensure isolation, the Firewall implements the following primary rules:

|     Source       |        Destination    |Port / Protocol|   Action  | Justification |
| :--------------- | :-------------------- | :------------ | :-------- | :------------ |
| Any (Internet)   | VLAN 10 (Web Server)  |TCP 443 (HTTPS)| **ALLOW** | Enable secure web traffic. |
| Any (Internet)   | Any (Internal Network)|      Any      | **DENY**  | Block all unsolicited inbound traffic (Default Deny). |
| VLAN 30 (Guest)  |     VLAN 10, 20, 99   |       Any     | **DENY**  | Total guest isolation. Only WAN egress allowed. |
|   VPN (Admin)    |   VLAN 99 (Bastion)   | TCP 22 (SSH)  | **ALLOW** | Sole entry point for infrastructure management. |
| VLAN 99 (Bastion)|  VLAN 10 (Web Server) | TCP 22 (SSH)  | **ALLOW** | Internal SSH management for the web server. |

---

## 🚀 Next Project Phases
- [ ] Virtual construction of the topology in **Cisco Packet Tracer**.
- [ ] Connectivity testing (ICMP) to validate VLAN isolation and security policy enforcement.

---
*This lab is part of my technical preparation.*