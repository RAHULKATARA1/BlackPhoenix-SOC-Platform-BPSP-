# VNet Peering Design – Phase 2

## Date

2026-06-16

## Status

LOCKED ✓

---

## Options Evaluated

### Option A – Hub and Spoke

```
vnet-identity
       |
       |
vnet-soc
   /      \
  /        \
attack   automation
```

Advantages:
* Less management
* Less peering cost

Disadvantages:
* Single point of failure

---

### Option B – Full Mesh

```
Identity ------- SOC
    |             |
    |             |
Attack ------- Automation
```

Advantages:
* Resilient
* Enterprise-grade
* Easier testing

Disadvantages:
* More peerings (6 total)
* Higher cost
* More complex troubleshooting

---

### Option C – Hybrid Hub-and-Spoke (SELECTED ✓)

```
             vnet-automation
                     |
                     |
vnet-identity ---- vnet-soc ---- vnet-attack
      |                                |
      +--------------------------------+
```

Advantages:
* Fewer peerings (3 total)
* Lower management overhead
* Lower peering traffic costs
* Easier troubleshooting
* Easier documentation
* More realistic SOC architecture
* All traffic flows through SOC (realistic monitoring)

Why SOC as the hub?
* Almost all traffic eventually goes to Wazuh, Splunk, TheHive, or Shuffle
* SOC needs visibility into all network segments
* Centralized monitoring is an enterprise best practice

---

## Selected Model

**Hybrid Hub-and-Spoke with vnet-soc as Hub**

---

## Peerings Required

| # | Peering Name | Source VNet | Destination VNet | Direction |
| --- | --- | --- | --- | --- |
| 1 | peer-soc-to-identity | vnet-soc | vnet-identity | Bidirectional |
| 2 | peer-identity-to-soc | vnet-identity | vnet-soc | Bidirectional |
| 3 | peer-soc-to-attack | vnet-soc | vnet-attack | Bidirectional |
| 4 | peer-attack-to-soc | vnet-attack | vnet-soc | Bidirectional |
| 5 | peer-soc-to-automation | vnet-soc | vnet-automation | Bidirectional |
| 6 | peer-automation-to-soc | vnet-automation | vnet-soc | Bidirectional |
| 7 | peer-identity-to-attack | vnet-identity | vnet-attack | Bidirectional |
| 8 | peer-attack-to-identity | vnet-attack | vnet-identity | Bidirectional |

Total: 8 peering entries (4 bidirectional pairs)

---

## Peering Settings (All Peerings)

```
Allow Virtual Network Access : Enabled
Allow Forwarded Traffic      : Enabled
Allow Gateway Transit        : Disabled
Use Remote Gateway           : Disabled
```

These settings are LOCKED.

---

## Azure CLI Commands (Reference)

### SOC ↔ Identity

```bash
az network vnet peering create \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-soc \
--name peer-soc-to-identity \
--remote-vnet vnet-identity \
--allow-vnet-access \
--allow-forwarded-traffic

az network vnet peering create \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-identity \
--name peer-identity-to-soc \
--remote-vnet vnet-soc \
--allow-vnet-access \
--allow-forwarded-traffic
```

### SOC ↔ Attack

```bash
az network vnet peering create \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-soc \
--name peer-soc-to-attack \
--remote-vnet vnet-attack \
--allow-vnet-access \
--allow-forwarded-traffic

az network vnet peering create \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-attack \
--name peer-attack-to-soc \
--remote-vnet vnet-soc \
--allow-vnet-access \
--allow-forwarded-traffic
```

### SOC ↔ Automation

```bash
az network vnet peering create \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-soc \
--name peer-soc-to-automation \
--remote-vnet vnet-automation \
--allow-vnet-access \
--allow-forwarded-traffic

az network vnet peering create \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-automation \
--name peer-automation-to-soc \
--remote-vnet vnet-soc \
--allow-vnet-access \
--allow-forwarded-traffic
```

### Identity ↔ Attack

```bash
az network vnet peering create \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-identity \
--name peer-identity-to-attack \
--remote-vnet vnet-attack \
--allow-vnet-access \
--allow-forwarded-traffic

az network vnet peering create \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-attack \
--name peer-attack-to-identity \
--remote-vnet vnet-identity \
--allow-vnet-access \
--allow-forwarded-traffic
```
