# ⚔️ Attack Simulation & Detection Engineering Deep-Dive

## 📌 Executive Summary
This document serves as the technical record for attack simulations performed in an isolated sandbox environment and the corresponding telemetry ingestion and detection engineering workflows. 

The objective of this project phase is to simulate adversary tactics, techniques, and procedures (TTPs) originating from an offensive node (**Kali Linux**), observe endpoint telemetry on a legacy victim endpoint (**Windows 7 SP1 with Sysmon**), and validate detection alerts on a centralized SOC engine (**Kali Purple**).

---

## 🏗️ Lab Architecture & Network Variables

| Role | Host OS | IP Address | Subnet / Adapter | Primary Tooling / Service |
| :--- | :--- | :--- | :--- | :--- |
| **Attacker Node** | Kali Linux 2024.x | `192.168.56.10` | VirtualBox Host-Only (`192.168.56.0/24`) | Nmap, Metasploit Framework, Netcat |
| **Target Endpoint** | Windows 7 SP1 x64 | `192.168.56.20` | VirtualBox Host-Only (`192.168.56.0/24`) | Microsoft Sysmon v14+, Audit Policies |
| **SOC / SIEM Engine** | Kali Purple | `192.168.56.30` | VirtualBox Host-Only (`192.168.56.0/24`) | Suricata NIDS, Wazuh Manager / Indexer |

---

## 🧪 Phase 1: Reconnaissance & Network Discovery

### Scenario 1.1: Aggressive Port Scanning & Service Enumeration

#### 1. Attack Execution (Kali Linux)
The attacker initiates an aggressive TCP SYN scan targeting the top 1,000 common ports on the target host to map active services and OS version signatures.

```bash
# Executed from Kali Linux (192.168.56.10)
sudo nmap -sS -sV -sC -O -p 1-1000 192.168.56.20 -oA /root/logs/nmap_win7_recon