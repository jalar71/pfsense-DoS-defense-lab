# pfSense DoS Attack Simulation & Defense Lab

This project simulates a Denial-of-Service (DoS) attack in a controlled home lab environment using **pfSense** as a perimeter firewall, **Kali Linux** as the attacker, and **Ubuntu** as the victim. The setup is built on **VMware Workstation**, showcasing how to detect and block malicious traffic using firewall rules.

---

## 🧠 Objectives

- Deploy a virtual firewall using pfSense
- Simulate an external attacker with Kali Linux
- Set up an internal victim machine using Ubuntu
- Demonstrate a DoS attack using `hping3`
- Capture and analyze network traffic in Wireshark
- Block the attack via pfSense firewall rules

---

## 📡 Networking Overview

### 🔧 pfSense Firewall

- **WAN Interface**: `192.168.1.193/24` (Bridged to home network)
- **LAN Interface**: `10.109.25.1/24` (Host-Only Network)
- **Roles**: DHCP server, default gateway for LAN, firewall

### 🐍 Kali Linux (Attacker)

- **IP Address**: `192.168.1.56/24`
- **Adapter Type**: Bridged
- **Role**: External attacker
- **Routing**: Static route to reach 10.109.25.0/24 via pfSense WAN

```bash
sudo ip route add 10.109.25.0/24 via 192.168.1.193
```
### 🟠 Ubuntu Linux (Victim)
- **IP Address**: `10.109.25.11/24`
- **Adapter Type**: Host-Only
- **DHCP**: IP and gateway assigned by pfSense
- **Role**: Internal system under attack

## 🔐Firewall Rules
### ✔️Allow Initial Access (for DoS simulation)
- **Interface**: WAN
- **Source**: Kali IP (`192.168.1.56`)
- **Destination**: Ubuntu IP (`10.109.25.11`)
### ❌Block DoS Traffic
- **Interface**: WAN
- **Source**: Kali IP
- **Destination**: Ubuntu IP
- **Action**: Block & Log
### 🚀Attack & Defense Workflow
1. Kali sends a flood using:
   ```bash
   sudo hping3 -1 --flood 10.109.25.11
   ```
2. Ubuntu (victim) captures the packet spike in Wireshark.
3. A pfSense rule is applied to block further incoming traffic from Kali.
4. Wireshark confirms the flood has stopped
5. pfSense logs show blocked traffic entries for Kali's IP.
  
