# Connectivity Matrix – Phase 2

## Date

2026-06-16

## Status

LOCKED ✓

---

## Required Communication Paths

| # | Source | Source IP | Destination | Dest IP | Purpose | Ports |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | bp-kali | 10.3.1.10 | bp-win10 | 10.1.2.20 | Attacks | 445, 3389, 5985, 5986 |
| 2 | bp-kali | 10.3.1.10 | bp-winserver | 10.1.2.30 | Attacks | 445, 3389, 5985, 5986 |
| 3 | bp-win10 | 10.1.2.20 | bp-wazuh | 10.2.3.50 | Logs | 1514, 1515 |
| 4 | bp-winserver | 10.1.2.30 | bp-wazuh | 10.2.3.50 | Logs | 1514, 1515 |
| 5 | bp-ubuntu | 10.3.2.40 | bp-wazuh | 10.2.3.50 | Logs | 1514, 1515 |
| 6 | bp-win10 | 10.1.2.20 | bp-splunk | 10.2.3.60 | Logs | 9997 |
| 7 | bp-winserver | 10.1.2.30 | bp-splunk | 10.2.3.60 | Logs | 9997 |
| 8 | bp-ubuntu | 10.3.2.40 | bp-splunk | 10.2.3.60 | Logs | 9997 |
| 9 | bp-wazuh | 10.2.3.50 | bp-thehive | 10.4.3.70 | Cases | 9000 |
| 10 | bp-thehive | 10.4.3.70 | bp-shuffle | 10.4.3.80 | Automation | 3001 |
| 11 | All VMs | * | bp-winserver | 10.1.2.30 | DNS | 53 |
| 12 | bp-win10 | 10.1.2.20 | bp-winserver | 10.1.2.30 | AD Auth | 88, 389, 445 |

---

## Traffic Flow Summary

```
Kali (Attacker)
 ↓ attacks
Windows (Victims)
 ↓ logs
Wazuh / Splunk (SIEM)
 ↓ alerts
TheHive (Case Management)
 ↓ triggers
Shuffle (SOAR Automation)
```

---

## VNet-to-VNet Communication Summary

| Source VNet | Destination VNet | Traffic Type |
| --- | --- | --- |
| vnet-attack | vnet-identity | Attack traffic |
| vnet-identity | vnet-soc | Log forwarding |
| vnet-attack | vnet-soc | Log forwarding |
| vnet-soc | vnet-automation | Alert/case forwarding |
| vnet-automation | vnet-automation | TheHive ↔ Shuffle (local) |
| All VNets | vnet-identity | DNS queries |
