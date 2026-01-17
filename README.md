# Enterprise Security Simulation Lab (2026)

## 📌 Project Overview
This repository documents a self-contained security laboratory designed to simulate real-world attack vectors and defensive monitoring. The environment was architected to mimic a corporate network, featuring segmented subnets, automated threat detection, and custom Python-based security tools.

**Tech Stack:**
* **Infrastructure:** Oracle VirtualBox, pfSense (Firewall), NAT Networks
* **Red Team (Offensive):** Kali Linux, Custom Python Scripts, Nmap, Metasploit
* **Blue Team (Defensive):** Wazuh SIEM/EDR, Splunk, Suricata

## 🏗️ Phase 1: Network Architecture
*Current Status: Deployed*

**Topology:**
* **Attacker Node:** Kali Linux (Rolling 2026) - IP: `10.0.0.5`
* **Victim Node:** Metasploitable 2 (Linux) - IP: `10.0.0.10`
* **SIEM Node:** Wazuh Server (OVA) - IP: `10.0.0.20`
* **Gateway:** Virtualized Router/Firewall

*[Place Network Diagram Screenshot Here]*

## ⚔️ Phase 2: Offensive Tool Development (Python)
I developed a custom, multi-threaded port scanner to understand the TCP handshake process at a packet level.
* **Features:** Multi-threading for speed, Banner Grabbing, Timeout handling.
* **Code:** [Link to /scripts/scanner.py]

## 🛡️ Phase 3: Threat Detection & Analysis
Using Wazuh EDR, I configured custom rules to detect the signature of my custom Python scanner.

**Alert Log Sample:**
> **Rule ID:** 100002
> **Level:** 12 (High)
> **Description:** "Nmap-style scan detected from internal host 10.0.0.5"

*[Place Screenshot of Wazuh Dashboard Here]*
