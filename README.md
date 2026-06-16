# BlackPhoenix SOC Platform (BPSP)

## Multi-Region Cloud Security Operations Center

An enterprise-style Security Operations Center built entirely on Microsoft Azure, spanning multiple regions with distributed networks, centralized SIEM/SOAR monitoring, attack simulation, and automated incident response.

---

## Project Structure

```
BlackPhoenix-SOC-Platform-BPSP-/
│
├── 00-Project-Charter          # Project charter and scope definition
├── 01-Architecture             # Master architecture, IP plan
├── 02-Azure-Build              # Deployment tracker and build docs
├── 03-Networking               # NSG policies and network config
├── 04-Identity                 # Active Directory and DNS
├── 05-SIEM                     # Wazuh and Splunk setup
├── 06-EDR                      # Endpoint Detection & Response
├── 07-SOAR                     # TheHive and Shuffle automation
├── 08-Detections               # Detection engineering rules
├── 09-Threat-Hunting           # Threat hunting queries and procedures
├── 10-Incident-Response        # IR playbooks and case documentation
├── 11-Attack-Simulations       # Attack scenarios and red team ops
├── 12-Automation-Scripts       # Utility and automation scripts
├── 13-Diagrams                 # Architecture and network diagrams
├── 14-Logs-and-Evidence        # Evidence collection per phase
├── 15-Final-Report             # Final project report
└── README.md
```

---

## Architecture Overview

| Region | Purpose | VMs |
| --- | --- | --- |
| East US 2 | Identity | bp-winserver (AD+DNS), bp-win10 (Endpoint) |
| West US 2 | SOC | bp-wazuh (SIEM), bp-splunk (SIEM) |
| Central US | Attack | bp-kali (Attacker), bp-ubuntu (Linux Victim) |
| South Central US | Automation | bp-thehive (Case Mgmt), bp-shuffle (SOAR) |

### Network Topology (Hub-and-Spoke)

```
               vnet-automation (10.4.0.0/16)
                       |
                       |
vnet-attack ---- vnet-soc ---- vnet-identity
(10.3.0.0/16)  (10.2.0.0/16)  (10.1.0.0/16)
```

### Domain

```
blackphoenix.lab
DNS: 10.1.2.30 (bp-winserver)
```

---

## Phases

| Phase | Description | Status |
| --- | --- | --- |
| Phase 0 | Project Initiation & Architecture Freeze | ✅ Complete |
| Phase 1 | Azure Foundation | 🔲 Not Started |
| Phase 2 | Networking | 🔲 Not Started |
| Phase 3 | Identity | 🔲 Not Started |
| Phase 4 | SOC | 🔲 Not Started |
| Phase 5 | Attack | 🔲 Not Started |
| Phase 6 | Automation | 🔲 Not Started |
| Phase 7 | Telemetry | 🔲 Not Started |
| Phase 8 | Detections | 🔲 Not Started |
| Phase 9 | Threat Hunting | 🔲 Not Started |
| Phase 10 | Incident Response | 🔲 Not Started |
| Phase 11 | Attack Simulation | 🔲 Not Started |
| Phase 12 | Final Report | 🔲 Not Started |

---

## Platform

Microsoft Azure

## Duration

8-10 Days

## Resource Group

RG-BlackPhoenix
