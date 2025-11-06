#  pfSense Firewall Lab Project 

##  About the Project
This project demonstrates my ability to deploy, configure, and validate a **pfSense firewall** using a **Kali Linux client** in a virtualized lab environment.  
I created and tested inbound/outbound rules, used **Wireshark** to capture and analyze network traffic, and documented results to verify correct rule enforcement.  
This project highlights my understanding of **network security, packet analysis, and firewall administration** — all essential cybersecurity fundamentals.

---

##  Lab Overview

| Component | Description |
|------------|-------------|
| **Firewall** | pfSense (latest release) |
| **Client** | Kali Linux (VirtualBox) |
| **Network Type** | NAT + Internal Network |
| **Tools Used** | pfSense, Kali Terminal, UFW, Wireshark, curl, ping |

---

##  Setup Summary

###  Virtual Machines
- **pfSense VM**
  - 2 Network Interfaces:  
    - `WAN` → NAT (for internet access)  
    - `LAN` → Internal Network (`LAN-NET`)
- **Kali Linux VM**
  - 1 Network Interface: Internal Network (`LAN-NET`)
  - Default gateway set to pfSense LAN IP (`192.168.1.1`)

###  pfSense Configuration
- **LAN IP:** `192.168.1.1/24`
- **DHCP:** Enabled (for Kali)
- **Default Rule:** Allow all (initially)
- **New Rules Created:**
  | Rule | Protocol | Port | Action | Description |
  |------|-----------|------|---------|-------------|
  | 1 | TCP | 80 (HTTP) | Allow | Allow web traffic |
  | 2 | TCP | 443 (HTTPS) | Allow | Allow secure web |
  | 3 | UDP | 53 (DNS) | Allow | Allow name resolution |
  | 4 | ICMP | — | Allow | Allow pings |
  | 5 | ALL | ANY | Deny | Block everything else |

---

##  Firewall Rules (Example)

```bash
# Inside pfSense Web GUI → Firewall → Rules → LAN
# Create rules from top to bottom:

1. Pass | Protocol: TCP | Destination Port: 80 | Description: Allow HTTP
2. Pass | Protocol: TCP | Destination Port: 443 | Description: Allow HTTPS
3. Pass | Protocol: UDP | Destination Port: 53 | Description: Allow DNS
4. Pass | Protocol: ICMP | Description: Allow Ping
5. Block | Protocol: Any | Description: Block All Other Traffic

