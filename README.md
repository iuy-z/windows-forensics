
# Practical Windows Forensics

## Introduction
The **Practical Windows Forensics** guide provides a DIY approach to performing digital forensic analysis on a Windows 10 system. 

## Steps TLDR:
1. Prepare a Windows target VM
2. Execute attack script (based on the AtomicRedTeam framework) on target VM
3. Acquire memory and disk images
4. Setup a Windows forensic VM
5. Get started with Windows forensic analysis

## Prerequisites:
- VirtualBox or VMware hypervisor
- Host system requirements:
  - **4GB+ RAM** for running Windows VMs
  - **Disk storage:** 2 x Windows VMs (20GB & 40GB) + ~30GB for forensic images

## Attack Scenario:
- The **ART-attack.ps1** script simulates a real-world compromise using Atomic Red Team techniques.
- The script installs **Invoke-AtomicRedTeam** and executes multiple attack techniques mapped to the **MITRE ATT&CK** framework.

## Preparation:
### 1. Prepare Target System
- Download and install a **Windows 10 Enterprise Evaluation VM** 
- Take a **snapshot** after setup.
- Pause **Windows Updates** to reduce noise.
- Install **Sysmon** for event logging:
  - Run: `.\Install-Sysmon.ps1` in an elevated PowerShell session.
- Disable **Windows Defender settings** temporarily before running attacks.

### 2. Execute the Attack Script
- Download and run: `.\ART-attack.ps1` as Administrator.
- Ensure **Internet access** is available for downloading dependencies.
- Verify **PowerShell logs** to confirm successful execution.

## Data Acquisition:
### 1. Capture Memory Image
- **VMware:** Copy `.vmem` and `.vmsn` snapshot files to the evidence folder.
- **VirtualBox:** Use `vboxmanage debugvm <VM_UUID> dumpvmcore --filename win10-mem.raw`

### 2. Capture Disk Image
- **VMware:** Locate and copy the latest `.vmdk` sequence files.
- **VirtualBox:** Export disk as RAW image:
  - `vboxmanage clonemedium disk <disk_UUID> --format raw win10-disk.raw`

### 3. Validate Integrity
- Generate **SHA1 hashes** for memory and disk images:
  - Windows: `Get-FileHash -Algorithm SHA1 <file>`
  - Linux/Mac: `shasum <file>`

## Forensic Analysis:
### 1. Set Up a Forensic Workstation
- Install a **Windows Server 2019 VM** as a forensic environment.
- Configure **Guest Additions**, enable **file sharing**, and install necessary forensic tools:
  - **Kali Linux Subsystem**, **Volatility**, **FTK Imager**, **RegRipper**, **KAPE**, **EventLog Explorer**.

### 2. Analyze Forensic Artifacts
- **User Accounts:** Identify active and created accounts.
- **Program Execution:** Detect PowerShell scripts, DLL injections.
- **Persistence Mechanisms:** Review run keys, scheduled tasks, startup items.
- **File System Analysis:** Investigate NTFS metadata, MFT entries, and timestomping.
- **Network & System Logs:** Extract logs from **Windows Event Viewer** and **Sysmon logs**.


