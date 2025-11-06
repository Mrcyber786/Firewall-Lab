#  pfSense Firewall Home Lab Project

This project demonstrates the setup, configuration, and verification of a **pfSense firewall** using **Kali Linux** as a client in a virtualized lab environment.  
The lab focuses on creating, applying, and testing firewall rules to control inbound and outbound network traffic while using **Wireshark** to verify the results.

All screenshots documenting the setup and testing process are included in the **/screenshots** folder of this repository.

---

##  Project Overview

The purpose of this lab is to showcase practical hands-on experience with:

- Installing and configuring **pfSense** as a firewall appliance  
- Managing firewall rules to allow and block specific traffic types  
- Testing network connectivity using **Kali Linux** tools  
- Capturing and analyzing traffic with **Wireshark**  
- Demonstrating real-world firewall administration and verification  

This project highlights foundational cybersecurity and network defense skills applicable to SOC, network security, and infrastructure roles.

---

##  Steps Completed

### 1. VirtualBox Setup
- Created two virtual machines: **pfSense Firewall** and **Kali Linux Client**.  
- Configured pfSense with two adapters:  
  - **Adapter 1:** NAT (WAN connection)  
  - **Adapter 2:** Internal Network (named `LAN-NET`)  
- Attached Kali Linux to the same Internal Network for LAN communication.

---

### 2. pfSense Installation and Configuration
- Installed pfSense from the official ISO.  
- Assigned network interfaces:
  - **WAN:** em0 (NAT)
  - **LAN:** em1 (Internal Network)
- Configured the LAN IP as `192.168.1.1/24`.  
- Enabled DHCP service for LAN to provide IPs to connected clients.  
- Accessed the pfSense Web UI via `https://192.168.1.1` using default credentials (admin / pfsense).

---

### 3. Firewall Rule Creation
Configured **Firewall → Rules → LAN** to apply least privilege access:

| # | Action | Protocol | Source | Destination | Port | Description |
|---|----------|-----------|----------|---------------|------|--------------|
| 1 | Pass | TCP | LAN net | any | 80 | Allow HTTP |
| 2 | Pass | TCP | LAN net | any | 443 | Allow HTTPS |
| 3 | Pass | UDP | LAN net | any | 53 | Allow DNS |
| 4 | Pass | ICMP | LAN net | any | * | Allow Ping |
| 5 | Block | * | LAN net | any | * | Block All Other Traffic |

After applying the rules, pfSense only allowed essential web and diagnostic traffic.

---

### 4. Traffic Testing from Kali Linux
Used command-line tools to verify rule enforcement:

**Allowed traffic tested:**
- ICMP → `ping -c 4 8.8.8.8`
- DNS → `dig google.com`
- HTTP → `curl -I http://example.com`
- HTTPS → `curl -I https://example.com`

**Blocked traffic tested:**
- FTP → `curl -I ftp://speedtest.tele2.net`
- Restricted ports → `nmap -p 21,22,23 8.8.8.8`

All allowed traffic succeeded, while blocked traffic failed as expected.

---

### 5. Wireshark Verification
Used Wireshark on Kali Linux to capture and analyze packets during testing.

**Filters applied:**
- `icmp` – view ping requests/replies  
- `dns` – observe DNS queries and responses  
- `tcp.port == 80` – monitor HTTP traffic  
- `tcp.port == 443` – monitor HTTPS traffic  
- `tcp.flags.syn == 1 && tcp.flags.ack == 0` – detect blocked SYN packets  

**Observed results:**
- Allowed traffic showed full TCP handshakes (SYN, SYN-ACK, ACK).
- Blocked traffic displayed repeated SYN attempts with no response — confirming the firewall dropped the packets.

Wireshark captures and screenshots of each test are provided in the **/screenshots** folder.

---

### 6. Automation Script
Created a Bash script (`tests.sh`) to automate testing and store results in a `/results` directory.

**Script functions:**
- Tests ICMP, DNS, HTTP, HTTPS, and blocked ports  
- Saves each output to a separate `.txt` file for documentation  
- Provides repeatable, consistent testing results  

---

##  Conclusion

This pfSense Firewall Home Lab demonstrates a complete implementation of a secure, rule-based network environment.  
It includes practical experience with:

- **pfSense installation and management**  
- **Firewall rule configuration**  
- **Network testing and packet analysis**  
- **Automation scripting for validation**  

This project highlights both **network security fundamentals** and **hands-on technical ability**, making it a strong addition to a cybersecurity portfolio or resume.

---

##  Repository Contents

| File/Folder | Description |
|--------------|-------------|
| `README.md` | Project documentation (this file) |
| `tests.sh` | Automated network testing script |
| `/results` | Folder containing text-based test outputs |
| `/screenshots` | Screenshots showing pfSense setup, rules, and Wireshark captures |
| `.gitignore` | Git ignore configuration for project |

---

###  Tools Used
- VirtualBox  
- pfSense Firewall  
- Kali Linux  
- Wireshark  
- Bash Shell  




