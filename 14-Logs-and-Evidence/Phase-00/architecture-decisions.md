# Architecture Decisions – Phase 0

## Date

2026-06-16

## Status

LOCKED ✓

---

## Decision 1: Resource Group

**Decision:** Single resource group `RG-BlackPhoenix`
**Rationale:** Simplifies management, billing, and cleanup for a lab/portfolio project.

---

## Decision 2: Multi-Region Layout

| Region | Purpose | VMs |
| --- | --- | --- |
| East US 2 | Identity | bp-winserver, bp-win10 |
| West US 2 | SOC | bp-wazuh, bp-splunk |
| Central US | Attack | bp-kali, bp-ubuntu |
| South Central US | Automation | bp-thehive, bp-shuffle |

**Rationale:** Separating concerns by region simulates an enterprise multi-region deployment and demonstrates cross-region networking skills.

---

## Decision 3: VM Sizing

| Size | Used For |
| --- | --- |
| B2s (2 vCPU, 4 GB) | Workloads needing more memory: AD, SIEM, case management |
| B1ms (1 vCPU, 2 GB) | Lightweight workloads: attacker, victim, SOAR |

**Rationale:** B-series burstable VMs minimize Azure spend while providing enough resources for lab workloads.

---

## Decision 4: Network Topology

**Decision:** Hub-and-Spoke with `vnet-soc` as hub
**Rationale:** All telemetry and logs flow to the SOC region. Hub-and-spoke ensures SOC has visibility into all spokes while spokes don't directly communicate (attack isolation).

---

## Decision 5: IP Addressing

**Decision:** /16 per VNet, /24 per subnet, static private IPs
**Rationale:** Large address spaces prevent future conflicts. Static IPs ensure consistent firewall rules and DNS resolution.

---

## Decision 6: DNS & Domain

**Decision:** `blackphoenix.lab` domain, DNS at `10.1.2.30` (bp-winserver)
**Rationale:** Active Directory requires DNS. Using a `.lab` TLD avoids real-domain conflicts.

---

## Decision 7: SIEM Stack

**Decision:** Dual SIEM — Wazuh + Splunk
**Rationale:** Demonstrates proficiency with both open-source (Wazuh) and commercial (Splunk Free) SIEM platforms. Wazuh handles agent-based EDR/HIDS; Splunk handles log aggregation and search.

---

## Decision 8: SOAR Stack

**Decision:** TheHive + Shuffle
**Rationale:** TheHive for case management, Shuffle for playbook automation. Both are open-source and integrate well with Wazuh and Splunk via webhooks/APIs.
