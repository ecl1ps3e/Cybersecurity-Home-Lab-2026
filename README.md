# 🛡️ Enterprise Security Simulation Lab (2026)

![Badge](https://img.shields.io/badge/Security-Red%20%26%20Blue%20Team-red) ![Badge](https://img.shields.io/badge/Python-3.11-blue) ![Badge](https://img.shields.io/badge/Platform-Linux-grey)

## 📌 Executive Summary
This project documents the construction of an isolated, virtualized corporate network (`CyberLab`) to simulate real-world attack vectors and defensive monitoring. The objective was to engineer a complete kill chain—from infrastructure deployment and custom tool development to vulnerability exploitation and SIEM detection.

**Tech Stack:**
* **Infrastructure:** Oracle VirtualBox, NAT Network Segmentation
* **Red Team (Offensive):** Kali Linux, Metasploit Framework, Custom Python Tooling
* **Blue Team (Defensive):** Wazuh SIEM/XDR, Auditd, Syslog

---

## 🏗️ Phase 1: Network Architecture
*Status: Deployed*

I architected a segregated NAT Network to simulate an internal corporate environment, ensuring traffic isolation from the host machine while allowing inter-VM communication.

**Topology Map:**
| Node Type | OS | IP Address | Role |
| :--- | :--- | :--- | :--- |
| **Attacker** | Kali Linux (2025.4) | `10.0.2.3` | Command & Control (C2) |
| **Defender** | Wazuh Server | `10.0.2.4` | SIEM & Log Aggregator |
| **Victim** | Metasploitable 2 | `10.0.2.5` | Vulnerable Production Server |

---

## ⚔️ Phase 2: Offensive Tool Development (Python)
Instead of relying solely on pre-made tools like Nmap, I developed a custom **Multi-Threaded Port Scanner** in Python to demonstrate understanding of the TCP 3-Way Handshake and socket programming.

* **Logic:** Implements `threading` and `queue` to scan 1,000 ports in <0.5 seconds.
* **Result:** Successfully identified open services on the target (`10.0.2.5`).

> **[View Source Code](scripts/scanner.py)**

![Python Scan Result](images/network_scan.png)
*Figure 1: Custom Python script identifying open ports (21, 22, 80) on the victim machine.*

---

## 🛡️ Phase 3: Threat Detection & Analysis (Blue Team)
Configured the **Wazuh SIEM** to ingest telemetry from network agents. The system successfully detected the scanning activity from Phase 2.

**Detection Logic:**
* **Alert Level:** 12 (High Severity)
* **Rule ID:** 100002
* **Trigger:** "Network scan detected from internal host 10.0.2.3"

![Wazuh Dashboard](images/wazuh_dashboard.png)
*Figure 2: Wazuh Dashboard visualizing active agents and security alerts.*

---

## 🧨 Phase 4: Vulnerability Exploitation
*Objective: Obtain Root Access on the Victim Node.*

Following the reconnaissance phase which identified **vsftpd 2.3.4** running on Port 21, I executed a targeted exploit to bypass authentication.

**Attack Chain:**
1.  **Vulnerability ID:** Backdoor Command Execution in `vsftpd v2.3.4`.
2.  **Exploit Framework:** Metasploit (`exploit/unix/ftp/vsftpd_234_backdoor`).
3.  **Payload:** Remote Shell (Reverse TCP).

**Proof of Compromise:**
The exploit successfully opened a backdoor connection on port 6200, granting `UID=0 (Root)` privileges.

```bash
# Proof of Root Access
whoami
> root

hostname
> metasploitable

id
> uid=0(root) gid=0(root)
