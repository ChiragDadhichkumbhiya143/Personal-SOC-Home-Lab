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