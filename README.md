# soc-home-lab-siem
# SOC Analyst Home Lab: SIEM & Threat Detection

## Objective
Build a virtualized Security Operations Center (SOC) home lab to practice log ingestion, alert creation, network traffic analysis, and incident investigation using Splunk and Linux.

## Host & Virtual Architecture
* **Host Machine:** Apple Mac Mini (M2 Max, 16GB RAM, 1TB SSD)
* **Hypervisor:** VMware Fusion Pro (Personal Use)
* **Network Design:** Isolated Host-Only / Private virtual network

### Virtual Machines

| VM Name | OS & Architecture | Specs | Role |
| :--- | :--- | :--- | :--- |
| `Ubuntu-SIEM-Server` | Ubuntu Server 24.04 LTS (ARM64) | 2 vCPU, 6GB RAM, 60GB NVMe | Monitored Linux endpoint & Splunk SIEM server |
| `Kali-Attacker` | Kali Linux (ARM64) | 2 vCPU, 4GB RAM, 40GB NVMe | Attack simulation & traffic generation (Nmap, Hydra) |

## Build Log & Troubleshooting

### Phase 1: Hypervisor & Base VM Setup
* Installed VMware Fusion on macOS.
* Created a custom ARM64 Linux virtual machine (`Ubuntu-SIEM-Server`) configured with 6GB RAM and a 60GB virtual disk.

#### Troubleshooting Note: VMware UEFI Boot Manager Loop
* **Issue:** When starting the newly created virtual machine for the first time, it booted into the UEFI Boot Manager screen (`Boot normally` failed) instead of launching an operating system installer.
* **Root Cause:** VMware Fusion creates an empty virtual hardware shell without a pre-installed OS, and no installation `.iso` image had been downloaded or attached to the virtual CD/DVD drive yet.
* **Resolution:** Downloaded the official **Ubuntu Server ARM64** `.iso` image, attached it via **Virtual Machine > CD/DVD (SATA) > Choose a disc or disc image**, and selected `EFI VMware Virtual SATA CDROM Drive` from the Boot Manager menu.
