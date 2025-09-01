# Hybrid SysAdmin Project

## Project Overview

This project demonstrates a hybrid sysadmin environment using Windows Server 2022 and RHEL 9. It focuses on Active Directory, DNS, DHCP, file sharing, automation, monitoring, and cross-platform scripting.

## Objectives

* Set up Windows Server 2022 with AD, DNS, DHCP, and PowerShell remoting.
* Configure RHEL 9 for cross-platform integration.
* Automate user management, backups, and monitoring.
* Showcase sysadmin skills with scripts and documentation.

## Repository Structure

* windows-setup/
* linux-setup/
* automation-scripts/
* docs/
* README.md

## Current Repository Status

* **windows-setup/**: Configuration scripts for Windows Server 2022 (Step 2 completed: static IP assigned and verified).  
* **linux-setup/**: Placeholder for RHEL 9 configuration scripts.  
* **automation-scripts/**: Placeholder for cross-platform automation scripts.  
* **docs/**: Placeholder for documentation and diagrams.  
* **README.md**: Project overview, objectives, and step status.  
* **.gitignore**: Excludes OS-specific files and VM artifacts.

---

## ✅ Step 2: Windows Server 2022 VM Setup (Day 1–2)

### 1. Assign Static IP

- **IP Address:** 192.168.56.10  
- **Subnet Mask:** 255.255.255.0  
- **Default Gateway:** 192.168.56.1  
- **DNS Server:** 127.0.0.1 (to be configured post-AD DS installation)  

### 2. Verify Configuration

- **Network Adapter:** Ethernet 4  
- **Status:** Up  
- **Link Speed:** 1 Gbps  
- **Default Route:** 0.0.0.0/0 via 192.168.56.1  
- **DNS Servers:** 8.8.8.8, 8.8.4.4  
- **Ping Test:** Successful to external IP 8.8.8.8, indicating internet connectivity  
- **Traceroute:** Reached external IP 192.168.1.1, confirming routing functionality

---

## Next Steps

1. Configure Windows Server 2022 Active Directory, DNS, and DHCP.  
2. Develop automation scripts for user/group management, backups, and monitoring.  
3. Document configurations and push updates to GitHub regularly.  
4. Expand Linux/RHEL 9 configuration scripts for cross-platform integration.

## Getting Started

* Clone the repository: git clone https://github.com/dxdiag9908/Hybrid-SysAdmin-Project.git  
* Follow instructions in subfolders as the project evolves.
