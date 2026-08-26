# Cybersecurity--Home-Lab
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

