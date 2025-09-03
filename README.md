🖥️ Hybrid SysAdmin Project
🌐 Project Overview
This project demonstrates a hybrid sysadmin environment using Windows Server 2022 and RHEL 9 virtual machines. The focus is on setting up and managing a multi-platform lab, with a strong emphasis on automation and troubleshooting.

Objectives
Set up Windows Server 2022 with Active Directory, DNS, DHCP, and PowerShell remoting.

Configure RHEL 9 for seamless cross-platform integration.

Automate key sysadmin tasks like user management, backups, and monitoring.

Document the entire process with scripts and notes.

📁 Repository Structure
Folder

Description

windows-setup/

Windows Server 2022 configuration scripts (AD DS, DNS, static IP, etc.).

linux-setup/

RHEL 9 configuration scripts.

automation-scripts/

Cross-platform automation scripts.

docs/

Documentation, diagrams, and lab notes.

README.md

Project overview, objectives, step status, and troubleshooting notes.

.gitignore

OS-specific files and VM artifacts are excluded from the repository.

✅ Completed Steps
Step 2: Windows Server 2022 VM Setup
This step involved configuring the foundational network settings for the Windows Server VM.

IP Address: 192.168.56.10

Subnet Mask: 255.255.255.0

Default Gateway: 192.168.56.1

DNS Server: 127.0.0.1 (post-AD DS configuration)

Verification:

Network Adapter: Ethernet 4

Status: Up, Link Speed: 1 Gbps

Default Route: 0.0.0.0/0 via 192.168.56.1

DNS Servers: 8.8.8.8, 8.8.4.4

Ping Test: Successful to 8.8.8.8

Traceroute: Reached 192.168.1.1

Step 3: Active Directory Domain Services Lab
The AD DS lab was a continuous workflow, starting with role installation and ending with verification of the domain.

Install AD DS and DNS Roles

Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
Install-WindowsFeature DNS -IncludeManagementTools

Promote VM to Domain Controller

Used the script Promote-DC.ps1 (unattended dcpromo.exe).

Domain: corp.local

NetBIOS Name: CORP

DSRM Password: P@ssword123

PowerShell Remoting enabled: Enable-PSRemoting -Force

⚠️ Note: The default Users and Computers Containers already exist.

Create Custom Organizational Units (OUs)

New-ADOrganizationalUnit -Name "Staff" -Path "DC=corp,DC=local"
New-ADOrganizationalUnit -Name "Workstations" -Path "DC=corp,DC=local"

Create Test User

New-ADUser -Name "TestUser1" -SamAccountName "TestUser1" -AccountPassword (ConvertTo-SecureString "P@ssword123" -AsPlainText -Force) -Enabled $true

Verify Domain

Get-ADDomain

Domain exists.

NetBIOS Name: CORP

DC roles are assigned correctly.

Troubleshooting Notes
Issue

Cause

Resolution

Install-ADDSForest cmdlet errors.

Limited/older ADDSDeployment module.

Used unattended dcpromo.exe instead.

No automatic reboot after script.

N/A

Verified domain with Get-ADDomain and manually rebooted.

Attempt to create OUs failed (name already in use).

AD automatically creates default containers.

Created custom OUs (Staff, Workstations) instead.

⚠️ Troubleshooting: VM Disk Space
After setting up your virtual machines, it's common to run into disk space issues on the host machine. This is often caused by virtual machine snapshots and suspended states that create large, temporary files.

The Problem: Fragmented Virtual Disks
Virtualization software like VMware Workstation Pro creates large files to store a VM's hard drive and memory.

.vmdk files: These are your virtual hard disks. When you take a snapshot, the software creates a new, temporary .vmdk file to store all subsequent changes. Over time, these files can multiply and consume a huge amount of disk space.

.vmem files: This file stores your VM's memory when you suspend it. It's often the same size as the VM's allocated RAM.

A simple PowerShell script cannot be used to safely delete these files, as it will corrupt your VMs. The only safe way to clean this up is to use your virtualization software's built-in tools.

The Solution: VMware's Built-in Tools
The most reliable way to reclaim this space is to use VMware's Disk Manager to consolidate and shrink the virtual disks.

Step 1: Delete Snapshots
If the VMware Disk Cleanup option is not available, it means your virtual disk is part of a snapshot chain. You must delete the snapshot first.

Shut down your VM completely (do not suspend).

In VMware Workstation Pro, go to VM > Snapshots > Snapshot Manager.

Select the snapshot you want to delete and click Delete. This will automatically begin a consolidation process to merge the snapshot files into your main virtual disk.

Step 2: Use VMware's Command-Line Tool to Shrink the Disk
Once the snapshot has been deleted, you can use the command-line tool to shrink the virtual disk and reclaim the remaining space.

Open Command Prompt as an administrator.

Navigate to the VMware installation directory:

cd "C:\Program Files (x86)\VMware\VMware Workstation"

Run the shrink command, targeting your VM's primary virtual disk file:

vmware-vdiskmanager.exe -k "C:\VMS\RHEL9-VM\Rhel 9\Red Hat Enterprise Linux 9 64-bit.vmdk"

Step 3: Verify the Cleanup
After the shrink process completes, you can verify the amount of free space you've reclaimed with this PowerShell command:

Get-PSDrive C | Select-Object Name, Used, Free, @{Name="Free (GB)";Expression={[math]::Round($_.Free / 1GB, 2)}}

This process should free up a significant amount of disk space, allowing you to run both of your VMs simultaneously.
