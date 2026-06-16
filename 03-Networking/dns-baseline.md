# DNS Baseline – Phase 2.2

## Date

2026-06-16

## Status

🟡 Prepared (Active after Phase 3 – AD deployment)

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

## DNS Configuration Per VNet

| VNet | Custom DNS | Status |
| --- | --- | --- |
| vnet-identity | 10.1.2.30 | 🟡 Apply after AD setup |
| vnet-soc | 10.1.2.30 | 🟡 Apply after AD setup |
| vnet-attack | 10.1.2.30 | 🟡 Apply after AD setup |
| vnet-automation | 10.1.2.30 | 🟡 Apply after AD setup |

Currently all VNets use Azure default DNS (168.63.129.16).

---

## Planned DNS Records

| Hostname | FQDN | IP | Type |
| --- | --- | --- | --- |
| bp-winserver | bp-winserver.blackphoenix.lab | 10.1.2.30 | A |
| bp-win10 | bp-win10.blackphoenix.lab | 10.1.2.20 | A |
| bp-wazuh | bp-wazuh.blackphoenix.lab | 10.2.3.50 | A |
| bp-splunk | bp-splunk.blackphoenix.lab | 10.2.3.60 | A |
| bp-kali | bp-kali.blackphoenix.lab | 10.3.1.10 | A |
| bp-ubuntu | bp-ubuntu.blackphoenix.lab | 10.3.2.40 | A |
| bp-thehive | bp-thehive.blackphoenix.lab | 10.4.3.70 | A |
| bp-shuffle | bp-shuffle.blackphoenix.lab | 10.4.3.80 | A |

---

## DNS Dependencies

```
1. bp-winserver must be deployed first (Phase 3)
2. AD DS role must be installed
3. DNS role installed automatically with AD DS
4. Forward lookup zone: blackphoenix.lab
5. Reverse lookup zones: 10.1.x.x, 10.2.x.x, 10.3.x.x, 10.4.x.x
6. VNet DNS settings updated to point to 10.1.2.30
7. All VMs restarted to pick up new DNS
```

---

## DNS Resolution Flow

```
Any VM
  ↓
VNet Custom DNS (10.1.2.30)
  ↓
bp-winserver (AD DNS)
  ↓
blackphoenix.lab zone
  ↓
A record → Private IP
```

For external resolution:

```
Any VM
  ↓
bp-winserver (AD DNS)
  ↓
Forwarder → Azure DNS (168.63.129.16)
  ↓
Internet resolution
```

---

## CLI Commands (Apply After Phase 3)

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

* Do NOT set custom DNS before bp-winserver is deployed and AD is configured
* Premature DNS change will break Azure agent communication
* Apply DNS changes only after AD DS is fully functional
