# Peering Implementation – Phase 2.2

## Date

2026-06-16

## Status

✅ Completed

---

## Objective

Enable private communication between all project regions using Global VNet Peering.

---

## Peering Model

Hybrid Hub-and-Spoke (SOC as Hub)

```
             vnet-automation
                     |
                     |
vnet-identity ---- vnet-soc ---- vnet-attack
      |                                |
      +--------------------------------+
```

---

## Why SOC Hub

* Wazuh lives here
* Splunk lives here
* Most telemetry terminates here
* Easier troubleshooting
* Lower complexity
* Lower peering costs

---

## Build Order

```
1. Identity ↔ SOC
2. SOC ↔ Attack
3. SOC ↔ Automation
4. Identity ↔ Attack
5. Validate
6. Document
```

---

## Peering Settings (Locked)

| Setting | Value |
| --- | --- |
| Allow Virtual Network Access | Enabled |
| Allow Forwarded Traffic | Enabled |
| Allow Gateway Transit | Disabled |
| Use Remote Gateway | Disabled |

---

## Peerings Created

| # | Peering Name | Source VNet | Remote VNet | Peering State | Provisioning State |
| --- | --- | --- | --- | --- | --- |
| 1 | identity-to-soc | vnet-identity | vnet-soc | Connected | Succeeded |
| 2 | soc-to-identity | vnet-soc | vnet-identity | Connected | Succeeded |
| 3 | soc-to-attack | vnet-soc | vnet-attack | Connected | Succeeded |
| 4 | attack-to-soc | vnet-attack | vnet-soc | Connected | Succeeded |
| 5 | soc-to-automation | vnet-soc | vnet-automation | Connected | Succeeded |
| 6 | automation-to-soc | vnet-automation | vnet-soc | Connected | Succeeded |
| 7 | identity-to-attack | vnet-identity | vnet-attack | Connected | Succeeded |
| 8 | attack-to-identity | vnet-attack | vnet-identity | Connected | Succeeded |

Total: 8 peering entries (4 bidirectional pairs)

---

## Azure CLI Commands Used

### Step 1 – Identity → SOC

```bash
az network vnet peering create \
--name identity-to-soc \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-identity \
--remote-vnet \
$(az network vnet show \
--resource-group RG-BlackPhoenix \
--name vnet-soc \
--query id -o tsv) \
--allow-vnet-access \
--allow-forwarded-traffic
```

### Step 2 – SOC → Identity

```bash
az network vnet peering create \
--name soc-to-identity \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-soc \
--remote-vnet \
$(az network vnet show \
--resource-group RG-BlackPhoenix \
--name vnet-identity \
--query id -o tsv) \
--allow-vnet-access \
--allow-forwarded-traffic
```

### Step 3 – SOC → Attack

```bash
az network vnet peering create \
--name soc-to-attack \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-soc \
--remote-vnet \
$(az network vnet show \
--resource-group RG-BlackPhoenix \
--name vnet-attack \
--query id -o tsv) \
--allow-vnet-access \
--allow-forwarded-traffic
```

### Step 4 – Attack → SOC

```bash
az network vnet peering create \
--name attack-to-soc \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-attack \
--remote-vnet \
$(az network vnet show \
--resource-group RG-BlackPhoenix \
--name vnet-soc \
--query id -o tsv) \
--allow-vnet-access \
--allow-forwarded-traffic
```

### Step 5 – SOC → Automation

```bash
az network vnet peering create \
--name soc-to-automation \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-soc \
--remote-vnet \
$(az network vnet show \
--resource-group RG-BlackPhoenix \
--name vnet-automation \
--query id -o tsv) \
--allow-vnet-access \
--allow-forwarded-traffic
```

### Step 6 – Automation → SOC

```bash
az network vnet peering create \
--name automation-to-soc \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-automation \
--remote-vnet \
$(az network vnet show \
--resource-group RG-BlackPhoenix \
--name vnet-soc \
--query id -o tsv) \
--allow-vnet-access \
--allow-forwarded-traffic
```

### Step 7 – Identity → Attack

```bash
az network vnet peering create \
--name identity-to-attack \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-identity \
--remote-vnet \
$(az network vnet show \
--resource-group RG-BlackPhoenix \
--name vnet-attack \
--query id -o tsv) \
--allow-vnet-access \
--allow-forwarded-traffic
```

### Step 8 – Attack → Identity

```bash
az network vnet peering create \
--name attack-to-identity \
--resource-group RG-BlackPhoenix \
--vnet-name vnet-attack \
--remote-vnet \
$(az network vnet show \
--resource-group RG-BlackPhoenix \
--name vnet-identity \
--query id -o tsv) \
--allow-vnet-access \
--allow-forwarded-traffic
```

---

## Subscription

```
ID: 093aef2f-6411-4dc3-9214-0120f87d298b
```
