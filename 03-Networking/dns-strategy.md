# DNS Strategy – Phase 2

## Date

2026-06-16

## Status

LOCKED ✓

---

## DNS Server

```
VM:     bp-winserver
IP:     10.1.2.30
Region: East US 2
Role:   AD Domain Controller + DNS Server
```

---

## Domain

```
blackphoenix.lab
```

---

## All Machines Use

```
Primary DNS:   10.1.2.30
Secondary DNS: 168.63.129.16 (Azure DNS fallback)
```

---

## Why bp-winserver as DNS?

* Active Directory requires DNS
* Kerberos authentication requires DNS
* Domain joins require DNS
* Centralized name resolution simplifies management
* All VMs can resolve each other by hostname

---

## DNS Records (Post AD Deployment)

| Hostname | IP | Type |
| --- | --- | --- |
| bp-winserver.blackphoenix.lab | 10.1.2.30 | A |
| bp-win10.blackphoenix.lab | 10.1.2.20 | A |
| bp-wazuh.blackphoenix.lab | 10.2.3.50 | A |
| bp-splunk.blackphoenix.lab | 10.2.3.60 | A |
| bp-kali.blackphoenix.lab | 10.3.1.10 | A |
| bp-ubuntu.blackphoenix.lab | 10.3.2.40 | A |
| bp-thehive.blackphoenix.lab | 10.4.3.70 | A |
| bp-shuffle.blackphoenix.lab | 10.4.3.80 | A |

---

## VNet DNS Configuration

Each VNet must be configured to use 10.1.2.30 as its DNS server:

```bash
az network vnet update \
--resource-group RG-BlackPhoenix \
--name vnet-identity \
--dns-servers 10.1.2.30

az network vnet update \
--resource-group RG-BlackPhoenix \
--name vnet-soc \
--dns-servers 10.1.2.30

az network vnet update \
--resource-group RG-BlackPhoenix \
--name vnet-attack \
--dns-servers 10.1.2.30

az network vnet update \
--resource-group RG-BlackPhoenix \
--name vnet-automation \
--dns-servers 10.1.2.30
```

---

## Notes

* DNS configuration happens AFTER bp-winserver is deployed and AD is configured (Phase 3)
* Until then, VNets use Azure default DNS (168.63.129.16)
* VNet peering must be established BEFORE DNS can work cross-region
