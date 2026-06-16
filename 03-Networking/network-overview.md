# Network Overview – Phase 2

## Status

🟡 In Progress

---

## Purpose

Provide secure private communication between all regions in the BlackPhoenix SOC Platform.

---

## Architecture

```
RG-BlackPhoenix
│
├── East US 2
│   └── vnet-identity (10.1.0.0/16)
│       └── victim-subnet (10.1.2.0/24)
│
├── West US 2
│   └── vnet-soc (10.2.0.0/16)
│       └── siem-subnet (10.2.3.0/24)
│
├── Central US
│   └── vnet-attack (10.3.0.0/16)
│       ├── attack-subnet (10.3.1.0/24)
│       └── victim-subnet (10.3.2.0/24)
│
└── South Central US
    └── vnet-automation (10.4.0.0/16)
        └── automation-subnet (10.4.3.0/24)
```

---

## Current State

All four VNets are completely isolated.

❌ Kali cannot reach Windows
❌ Wazuh cannot receive logs
❌ Splunk cannot receive forwarders
❌ DNS cannot work across regions
❌ TheHive and Shuffle cannot communicate

---

## Target State

All four VNets connected via Global VNet Peering using Hybrid Hub-and-Spoke model with vnet-soc as the hub.

✅ AD authentication across regions
✅ DNS resolution across regions
✅ SIEM log collection from all endpoints
✅ SOAR automation from SOC to Automation region
✅ Attack simulations from Central US to East US 2
✅ Incident response workflows end-to-end

---

## Objectives

Enable:

* AD authentication
* DNS
* SIEM communication
* SOAR communication
* Attack simulations
* Incident response

---

## Topology Model

Hybrid Hub-and-Spoke (SOC as Hub)

```
             vnet-automation
                     |
                     |
vnet-identity ---- vnet-soc ---- vnet-attack
```

---

## Key Decisions

* No VPN
* No ExpressRoute
* No NVAs
* No custom route tables
* Azure system routes only
* Global VNet Peering for all inter-region traffic
