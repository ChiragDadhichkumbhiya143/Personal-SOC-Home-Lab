# System Requirements & Specifications

## Hardware Resource Allocation
- **Total Host RAM Needed:** 16 GB
- **Host CPU:** 8 Cores recommended
- **Subnet:** 192.168.100.0/24 (Isolated / Host-Only)

## VM Allocation Matrix
| VM Hostname | Static IP | vCPU | Allocated RAM | Primary Function |
| :--- | :--- | :--- | :--- | :--- |
| `win7-target` | 192.168.100.10 | 2 | 2 GB | Endpoint Telemetry Target (Sysmon) |
| `kali-purple-siem` | 192.168.100.50 | 4 | 6 GB | SIEM Core (Wazuh & OpenSearch Stack) |
| `kali-attacker` | 192.168.100.100 | 2 | 2 GB | Adversary Emulation Node |

## Port Matrix
- **TCP 1514:** Wazuh Agent Data Traffic
- **TCP 1515:** Wazuh Agent Registration
- **TCP 443/5601:** SIEM Dashboard Management