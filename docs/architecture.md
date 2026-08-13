# System Architecture & Network Topology

## Network Topology Blueprint
```mermaid
flowchart TB
    subgraph Subnet ["Isolated Network (192.168.100.0/24)"]
        Target["Target Node: win7-target\n192.168.100.10\n(Sysmon + Wazuh Agent)"]
        SIEM["SIEM Core: kali-purple-siem\n192.168.100.50\n(Wazuh Manager + OpenSearch)"]
        Attacker["Attacker Node: kali-attacker\n192.168.100.100\n(Atomic Red Team)"]
    end

    Target -->|Log Pipeline: TCP 1514| SIEM
    Attacker ==>|Threat Emulation Traffic| Target