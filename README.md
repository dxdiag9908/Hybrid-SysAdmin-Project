# 🖥️ Hybrid SysAdmin Project

---

## 🌐 Project Overview
This project demonstrates a hybrid sysadmin environment using **Windows Server 2022** and **RHEL 9**.  

Focus areas:

- Active Directory (AD DS)  
- DNS and DHCP  
- File sharing  
- Automation and monitoring  
- Cross-platform scripting  

---

## 🎯 Objectives

- Set up Windows Server 2022 with AD, DNS, DHCP, and PowerShell remoting  
- Configure RHEL 9 for cross-platform integration  
- Automate user management, backups, and monitoring  
- Showcase sysadmin skills with scripts and documentation  

---

## 📁 Repository Structure

| Folder | Description |
|--------|-------------|
| `windows-setup/` | Windows Server 2022 configuration scripts (AD DS, DNS, static IP, etc.) |
| `linux-setup/` | RHEL 9 configuration scripts |
| `automation-scripts/` | Cross-platform automation scripts |
| `docs/` | Documentation, diagrams, and lab notes |
| `README.md` | Project overview, objectives, step status, and troubleshooting notes |
| `.gitignore` | OS-specific files and VM artifacts excluded from repo |

---

## ✅ Completed: Step 2 — Windows Server 2022 VM Setup

- **IP Address:** 192.168.56.10  
- **Subnet Mask:** 255.255.255.0  
- **Default Gateway:** 192.168.56.1  
- **DNS Server:** 127.0.0.1 (post-AD DS configuration)  

**Verification:**

- Network Adapter: Ethernet 4  
- Status: Up, Link Speed: 1 Gbps  
- Default Route: 0.0.0.0/0 via 192.168.56.1  
- DNS Servers: 8.8.8.8, 8.8.4.4  
- Ping Test: Successful to 8.8.8.8  
- Traceroute: Reached 192.168.1.1  

---

## ✅ Completed: Step 3 — Active Directory Domain Services Lab

The AD DS lab was completed as a **continuous workflow**, starting with role installation and ending with verification of the domain:

1. **Install AD DS and DNS Roles**

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
Install-WindowsFeature DNS -IncludeManagementTools
Promote VM to Domain Controller

Used the script Promote-DC.ps1 (unattended dcpromo.exe)

Domain: corp.local

NetBIOS Name: CORP

DSRM Password: P@ssword123

PowerShell Remoting enabled: Enable-PSRemoting -Force

⚠️ Note: Default Users and Computers containers already exist.

Create Custom Organizational Units (OUs)

powershell
Copy code
New-ADOrganizationalUnit -Name "Staff" -Path "DC=corp,DC=local"
New-ADOrganizationalUnit -Name "Workstations" -Path "DC=corp,DC=local"
Create Test User

powershell
Copy code
New-ADUser -Name "TestUser1" -SamAccountName "TestUser1" -AccountPassword (ConvertTo-SecureString "P@ssword123" -AsPlainText -Force) -Enabled $true
Verify Domain

powershell
Copy code
Get-ADDomain
Domain exists

NetBIOS Name: CORP

DC roles assigned correctly

Troubleshooting Notes

Issue	Error / Observation	Cause	Resolution
Install-ADDSForest cmdlet errors	'DomainNetbiosName' was not recognized, 'InstallDNS' was not recognized, 'NewDomain' was not recognized	Limited/older ADDSDeployment module	Used unattended dcpromo.exe instead
No automatic reboot	Script completed but VM did not reboot automatically	N/A	Verified domain with Get-ADDomain and manually rebooted
Default Users and Computers containers exist	Attempt to create OUs failed (name already in use)	AD auto-creates default containers	Created custom OUs (Staff, Workstations) instead
