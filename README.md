# Cybersecurity--Home-Lab Setup 
This rep will contain all the lab practices in regards to learning curve of cyber security from scratch. 
# Cybersecurity Home Lab Project: Building an Enterprise SIEM

## Project Overview
The objective of this project is to build an isolated corporate-style security monitoring lab from scratch using VMware Workstation Pro. This lab will be used to simulate real-world cyber attacks mapped to the MITRE ATT&CK framework and monitor them using enterprise defensive tools.

As an IT Support Analyst transitioning into security, this lab allows me to bridge the gap between theoretical knowledge and hands-on detection engineering across both Windows and Linux environments.

---

## Current Lab Topology & Progress

### 🟩 Component 1: The Defender (SIEM) Server
*   **Operating System:** Ubuntu Server 24.04 LTS (64-bit)
*   **Resource Allocation:** 4 GB RAM | 2 CPU Cores | 30 GB Disk Space
*   **Network Configuration:** VMware Isolated NAT mode
*   **Static IP Address:** 192.168.42.128
*   **Software Installed:** Wazuh SIEM All-in-One Deployment (v4.9)

### 🟨 Component 2: The Attacker (In Progress)
*   **Operating System:** Kali Linux (Debian-based)
*   **Resource Allocation:** 2 GB RAM | 2 CPU Cores | 25 GB Disk Space
*   **Status:** Configuring container parameters

### 🟥 Component 3: The Victim Endpoint (Pending)
*   **Operating System:** Lubuntu (Lightweight Linux client)
*   **Resource Allocation:** 1.5 GB RAM | 1 CPU Core | 20 GB Disk Space
*   **Status:** Planning phase

---

## Phase 1 Milestones & Troubleshooting Log

### 1. OS Installation
Successfully initialized a lean Ubuntu Server environment. Purposely deselected LVM logical partitioning during setup to keep disk modifications simple and easy to track for laboratory storage sizing.

### 2. SIEM Deployment & Bypassing Hardware Restrictions
Attempted to deploy the automated Wazuh installer script. The script initially threw an error because my virtual container was allocated 4 GB of RAM, which triggered a built-in safety check expecting enterprise-level resources.

*   **The Fix:** Appended the `-i` (ignore) installation flag to the script command to force execution.
*   **The Command Used:** 
    ```bash
    curl -sO https://wazuh.com && sudo bash wazuh-install.sh -a -i
    ```

### 3. Verification & Web Accessibility
The server successfully compiled the background security databases. I verified accessibility by moving to my host Windows 11 machine, opening a browser, navigating through the self-signed certificate warning, and logging directly into the active dashboard at `https://192.168.42.128`.

---

## Concepts Learned So Far
*   **Thin Provisioning:** Understanding that allocating 30GB to a VM doesn't take 30GB off my C: drive instantly; it grows dynamically.
*   **Backend/Frontend Decoupling:** How an enterprise tool can run efficiently as a dark text-based background service ("kitchen") while presenting data via a clean web UI ("dining room").
*   **The Sudo Command:** Restricting core administrative file changes behind explicit verification layers for Linux system hardening.

Day 2 

## Update: Phase 2 - Victim Endpoint Deployment & Attacker Pivot

### 🟥 Component 2: The Victim Endpoint (Completed)
*   **Operating System:** Lubuntu 24.04 LTS (Lightweight Ubuntu Core)
*   **Resource Allocation:** 1.5 GB RAM | 1 CPU Core | 20 GB Disk Space
*   **Network Configuration:** VMware Isolated NAT mode
*   **Status:** Fully installed and operational on native hardware container.
*   **Purpose:** Active target endpoint to generate system logs, user behavior, and security event telemetry.

---

## Technical Troubleshooting & Architecture Evolution

### 1. The Pre-Built Kali Linux Image Bottleneck
Attempted to launch a preconfigured, unzipped Kali Linux VMware image (`.vmx`). The virtual machine consistently failed to initialize, resulting in a generic platform execution crash. 
*   **Root Cause Analysis:** Investigation of the VMware settings revealed that Windows 11 host-level hypervisor security mitigations (Core Isolation / Hyper-V) were conflicting with the hardware virtualization signatures hardcoded into the third-party pre-built image. 
*   **The Mitigation Strategy:** Rather than altering underlying Windows 11 operating system architecture, a tactical pivot was made to prioritize the defensive infrastructure. The pre-built image loop was abandoned to preserve system stability and focus on the deployment of the Lubuntu victim node. A clean Kali Installer ISO will be compiled natively at a later phase to sidestep the virtualization engine mismatch.

### 2. Streamlined Client OS Selection (Lubuntu Optimization)
To stay strictly within the 16 GB host RAM limitation (with ~7.4 GB already consumed by background Windows services), standard heavy desktop clients like corporate Windows 10/11 or stock Ubuntu GNOME were passed over due to their high baseline idling demands (2-4 GB RAM).
*   **The Solution:** Selected **Lubuntu**, an official flavor of Ubuntu utilizing the ultra-lean LXQt desktop environment. 
*   **Resource Conservation:** By configuring the container to run on just 1.5 GB RAM and 1 CPU core, the machine functions flawlessly as a target while keeping total laboratory memory draw under 6 GB—leaving an adequate safety buffer on the host machine.

---

## Next Operational Step
The lab now consists of an active defensive server (Wazuh SIEM) and a live target node (Lubuntu Client). The immediate next milestone is to deploy the Wazuh Endpoint Monitoring Agent onto the Lubuntu client via the terminal and route its system syslog telemetry directly back to the SIEM dashboard at `192.168.42.128`.


Day 3

### Phase 3: Telemetry Ingestion & Live Validation

*   Successfully initialized the Wazuh Endpoint Agent deployment pipeline via the `DEB/amd64` compilation channel.
*   Routed log parameters directly to the centralized SIEM node over the internal private architecture interface (`192.168.42.128`).
*   Validated the deployment by verifying a status change from `0` to `1 Active Agent` inside the master visual dashboard interface.
*   Executed a manual authentication degradation simulation (Brute Force simulation) to verify the data ingestion loop and trace alert triggers directly back to the MITRE ATT&CK database.

#### Detailed Agent Deployment Walkthrough:
1.  **Dashboard Configuration:** Accessed the visual Wazuh Dashboard via the host Windows environment and navigated to `Server Management` -> `Endpoints Summary` -> `Deploy new agent`.
2.  **Script Generation:** Configured the deployment wizard parameters to select `DEB` package format, `amd64` system architecture, and pointed explicitly to the SIEM master IP address (`192.168.42.128`).
3.  **Endpoint Execution:** Opened a Linux terminal session inside the isolated `Lubuntu-Victim` client VM, executed the custom compilation string via root permissions, and let the installer pull the official software bundles.
4.  **Service Activation:** Initialized and hardened the tracking service engine inside the client terminal using system commands:
    ```bash
    sudo systemctl daemon-reload
    sudo systemctl enable wazuh-agent
    sudo systemctl start wazuh-agent
    ```
5.  **Telemetry Verification:** Monitored the web UI landing hub to confirm the `Lubuntu-Victim` machine registered as a live asset successfully pushing local event logs.

### Phase 2 Update: Hypervisor Hardening & Successful Kali Deployment
*   **Status:** Kali-Attacker VM fully operational (2 GB RAM | 2 CPU Cores | 25 GB Disk Space).
*   **Resolution Note:** The persistent `Exception 0xc0000005` kernel crash during OS compilation was successfully resolved by migrating the virtualization layer from VMware Workstation Pro v16 to v17. 
*   **Technical Justification:** v17 natively integrates with Windows 11 Virtualization-Based Security (VBS) and Core Isolation. This updates processing execution threads, allowing the virtual CPU engines to scale without triggering memory access violations on the host system.

**SUMMARIZATION**

```text
======================================================================================
                  CYBERSECURITY HOME LAB NETWORKING & ARCHITECTURE
======================================================================================

                   ┌──────────────────────────────────┐
                   │       WINDOWS 11 (HOST)          │
                   │  - Web Browser (Chrome/Edge)     │◄─────────┐
                   │  - System Monitoring Console     │          │
                   └─────────────────┬────────────────┘          │
                                     │                           │
  ===================================│===========================│====================
  VMWARE WORKSTATION PRO 17 VIRTUAL  │ NET SUBNET (192.168.42.x) │
  ===================================▼===========================│====================
                                                                 │
      ┌────────────────┐           ┌────────────────┐            │
      │ KALI LINUX     │           │ LUBUNTU        │            │
      │ (Attacker Node)│           │ (Victim Client)│            │
      ├────────────────┤           ├────────────────┤            │
      │ RAM: 2 GB      │           │ RAM: 1.5 GB    │            │
      │ CPU: 2 Cores   │           │ CPU: 1 Core    │            │
      │ Disk: 25 GB    │           │ Disk: 20 GB    │            │
      └───────┬────────┘           └───────┬────────┘            │
              │                            │                     │
              │                            │ (Ships Logs)        │ (Visual Dashboard)
              │ (Launches Attack)          │                     │
              │                            ▼                     │
              │                    ┌────────────────┐            │
              └───────────────────►│ UBUNTU SERVER  ├────────────┘
                                   │ (Wazuh SIEM)   │
                                   ├────────────────┤
                                   │ RAM: 4 GB      │
                                   │ CPU: 2 Cores   │
                                   │ Disk: 30 GB    │
                                   └────────────────┘

======================================================================================
                        COMPONENT ROLES & OPERATIONS LOG
======================================================================================

1. THE ATTACKER (Kali Linux)
   - Task: Simulates real-world adversary behavior.
   - Purpose: Executes techniques directly from the MITRE ATT&CK Matrix (such as network
     scanning, brute-force attempts, and malicious script execution) targeting the
     victim machine.

2. THE VICTIM (Lubuntu Client)
   - Task: Represents a standard user endpoint in a corporate network.
   - Core Software: Runs the silent background "Wazuh Agent" tool.
   - Purpose: Generates raw system behavior logs and tracking data, immediately 
     shipping them across the private network network when it detects suspicious changes.

3. THE DEFENDER (Ubuntu Server / Wazuh SIEM)
   - Task: Acts as the central Security Operations Center (SOC) brain.
   - Core Software: Runs the active Wazuh Manager engine and data indexing databases.
   - Purpose: Collects all incoming telemetry data, flags high-severity alerts, logs
     malware footprints, and hosts the visual dashboard interface for the analyst.
======================================================================================
```

















