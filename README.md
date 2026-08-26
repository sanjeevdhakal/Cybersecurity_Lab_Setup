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
