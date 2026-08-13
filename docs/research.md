# Day 3: SOC Telemetry, Log Architecture & Ingestion Protocols

## 1. Enterprise SOC Pipeline Architecture
Raw Events (Sysmon/Suricata) -> Agent Shipper (Logstash/Wazuh Agent) -> SIEM Rule Engine (Wazuh/OpenSearch) -> Dashboard / Alerts

## 2. Core Telemetry Mechanics
### Endpoint Telemetry: Windows Sysmon
- Event ID 1: Process Creation (Tracks Command Line arguments & Parent PIDs)
- Event ID 3: Network Connection (Maps Process Name to Destination IP)
- Event ID 11: File Creation (Detects Malware drops)

### Network Telemetry: Suricata vs Zeek
- Suricata: Signature-based NIDS for payload pattern matching.
- Zeek: Network metadata extraction (DNS, HTTP, TLS certificate logs).

## 3. Communication Protocols
- Agent-Manager Tunnel: TCP Ports 1514 / 1515 (AES Encrypted Transport)
- Syslog Transport: UDP 514 / TCP 601

```mermaid
flowchart TB
    subgraph Isolated_Subnet ["Isolated Lab Subnet (192.168.100.0/24)"]
        
        subgraph Target_Zone ["Target Endpoint (192.168.100.10)"]
            WinOS["Windows 7 OS Kernel"]
            Sysmon["Sysmon Driver (Ring-0)"]
            WAgent["Wazuh Agent Service"]
            
            WinOS -->|Process/Net Events| Sysmon
            Sysmon -->|EVTX Operational Log| WAgent
        end

        subgraph SIEM_Zone ["SIEM Core (192.168.100.50)"]
            WManager["Wazuh Manager (TCP 1514)"]
            Decoders["Decoder & Parser Engine"]
            Ruleset["Detection Rules Engine"]
            Indexer["OpenSearch Indexer"]
            Dashboard["Wazuh Dashboard UI"]

            WManager --> Decoders
            Decoders -->|Structured JSON| Ruleset
            Ruleset -->|Alert Trigger| Indexer
            Indexer <--> Dashboard
        end

        subgraph Attack_Zone ["Attacker Node (192.168.100.100)"]
            ART["Atomic Red Team / Metasploit"]
        end

    end

    %% Communication Flows
    WAgent -->|Encrypted TCP 1514| WManager
    ART ==>|Threat Emulation / Attacks| Target_Zone