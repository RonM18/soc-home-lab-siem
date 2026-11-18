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
### Phase 2: Splunk SIEM & Universal Forwarder Deployment
* **Architecture Adjustment:** Because Splunk Enterprise provides native ARM64 support for macOS while the standard Linux `.deb` package targets `x86_64`, I deployed **Splunk Enterprise** natively on the macOS host (`port 8000` web UI, `port 9997` receiver) and installed the **Splunk Universal Forwarder (ARM64)** on `Ubuntu-SIEM-Server`.
* **Log Ingestion Configured:** Added `/var/log/auth.log` as a monitored data input on the Ubuntu forwarder and enabled `boot-start` persistence.
* **Verification:** Confirmed real-time ingestion of Linux `sudo` and authentication events inside Splunk (`index=main host=soc-siem-home-lab`).
<img width="1297" height="846" alt="Screenshot 2026-11-17 at 11 02 51 PM" src="https://github.com/user-attachments/assets/d9c01573-4820-4350-8c20-6c2999b14047" />
<img width="1911" height="1077" alt="Screenshot 2026-11-17 at 10 46 13 PM" src="https://github.com/user-attachments/assets/bb8b40ea-122a-4040-ae4a-d248616363a4" />
