# Network Baseline – Phase 2.2

## Date

2026-06-16

## Status

✅ Baseline Established

---

## VNet Matrix

| VNet | Region | Address Space | Status |
| --- | --- | --- | --- |
| vnet-identity | East US 2 | 10.1.0.0/16 | ✅ Active |
| vnet-soc | West US 2 | 10.2.0.0/16 | ✅ Active |
| vnet-attack | Central US | 10.3.0.0/16 | ✅ Active |
| vnet-automation | South Central US | 10.4.0.0/16 | ✅ Active |

---

## Subnet Matrix

| Subnet | VNet | CIDR | NSG | Status |
| --- | --- | --- | --- | --- |
| victim-subnet | vnet-identity | 10.1.2.0/24 | nsg-identity | ✅ Active |
| siem-subnet | vnet-soc | 10.2.3.0/24 | nsg-soc | ✅ Active |
| attack-subnet | vnet-attack | 10.3.1.0/24 | nsg-attack | ✅ Active |
| victim-subnet | vnet-attack | 10.3.2.0/24 | nsg-attack | ✅ Active |
| automation-subnet | vnet-automation | 10.4.3.0/24 | nsg-automation | ✅ Active |

---

## Peering Matrix

| Source | Destination | State | Sync Level |
| --- | --- | --- | --- |
| vnet-identity | vnet-soc | Connected | FullyInSync |
| vnet-soc | vnet-identity | Connected | FullyInSync |
| vnet-soc | vnet-attack | Connected | FullyInSync |
| vnet-attack | vnet-soc | Connected | FullyInSync |
| vnet-soc | vnet-automation | Connected | FullyInSync |
| vnet-automation | vnet-soc | Connected | FullyInSync |
| vnet-identity | vnet-attack | Connected | FullyInSync |
| vnet-attack | vnet-identity | Connected | FullyInSync |

---

## NSG Matrix

| NSG | Region | Attached To | Status |
| --- | --- | --- | --- |
| nsg-identity | East US 2 | vnet-identity / victim-subnet | ✅ Active |
| nsg-soc | West US 2 | vnet-soc / siem-subnet | ✅ Active |
| nsg-attack | Central US | vnet-attack / attack-subnet, victim-subnet | ✅ Active |
| nsg-automation | South Central US | vnet-automation / automation-subnet | ✅ Active |

---

## Network Topology (Current State)

```
             vnet-automation (10.4.0.0/16)
                     |
                     | peered
                     |
vnet-identity ---- vnet-soc ---- vnet-attack
(10.1.0.0/16)    (10.2.0.0/16)  (10.3.0.0/16)
  peered           (HUB)          peered
      |                                |
      +--------------------------------+
                  peered
```

---

## Resource Counts

| Resource Type | Count |
| --- | --- |
| Resource Group | 1 |
| Virtual Networks | 4 |
| Subnets | 5 |
| NSGs | 4 |
| VNet Peerings | 8 |
| VMs | 0 (not yet deployed) |
| **Total Azure Resources** | **22** |
