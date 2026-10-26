<div align="center">

# 🔒 CyberSecurity Lab Environment & Virtual Network Setup
### Practical Lab Deployment — NetworkWalks Cybersecurity Program (Batch B084)

![Author](https://img.shields.io/badge/Author-Manthan_Acharya-7B2CBF?style=for-the-badge&logo=github&logoColor=white)
![Hypervisor](https://img.shields.io/badge/Hypervisor-Oracle_VirtualBox-205C90?style=for-the-badge&logo=virtualbox&logoColor=white)
![OS](https://img.shields.io/badge/OS-Kali_Linux_2026.2-555555?style=for-the-badge&logo=kalilinux&logoColor=white)
![Subnet](https://img.shields.io/badge/Subnet-10.0.0.0%2F24-00B4D8?style=for-the-badge)
![Status](https://img.shields.io/badge/Lab_Status-Active_%26_Verified-00F0FF?style=for-the-badge)

</div>

---

## 📌 Technical Summary

This repository details the architecture, manual routing configuration, and operational verification of an isolated local penetration testing laboratory built on Oracle VM VirtualBox. Designed to support ethical hacking assessments, network traffic analysis, and tool orchestration in a strictly sandboxed environment.

---

## ⚙️ Environment Specs & Lab Parameters

| Setting Parameter | Implementation Value |
| :--- | :--- |
| **Target Workstation OS** | Kali Linux (`kali-linux-2026.2-virtualbox-amd64`) |
| **Hypervisor Software** | Oracle VM VirtualBox Manager |
| **Virtual Adapter Mode** | NAT Network (`NatNetwork`) |
| **Adapter Hardware Type** | Intel PRO/1000 MT Desktop (`82540EM`) |
| **Promiscuous Mode** | `Allow All` |
| **Subnet Addressing** | `10.0.0.0/24` (DHCP Enabled) |
| **Static Guest IP** | `10.0.0.2/24` |
| **Default Gateway** | `10.0.0.1` |
| **DNS Resolvers** | `8.8.8.8` / `10.0.0.1` |
| **System State Recovery** | VM Baseline Snapshot (`fresh setup`) |

---

## 🛠️ Infrastructure Buildout

### 1. Custom Global NAT Subnet Creation
Created an isolated global `NatNetwork` within VirtualBox operating on the `10.0.0.0/24` IPv4 space with active DHCP services enabled to manage local virtual host allocations.



### 2. Network Interface & Promiscuous Configuration
Bound the primary virtual NIC of the Kali Linux VM (`kali-linux-2026.2-virtualbox-amd64`) directly to `NatNetwork`. Set interface promiscuous mode to **Allow All** to permit raw packet inspection and promiscuous monitoring during future pentesting modules.



### 3. Workstation Deployment & Performance Inspection
Initialized the Kali Linux environment, setting up system processes and monitoring interface traffic across virtual sockets.



---

## ⚡ Real-World Troubleshooting: Terminal CLI Fixes

### NetworkManager Interface Timeout & DNS Resolution Fix
During static IP mapping, interface binding issues prevented external name resolution over the custom NAT bridge. The interface daemon was reconfigured directly via Kali terminal using `nmcli`:

```bash
# 1. Override NetworkManager Duplicate Address Detection (DAD) timeout
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0

# 2. Cycle connection profile to enforce parameters
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"

