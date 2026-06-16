# Resource Inventory – Phase 1

## Date

2026-06-16

---

## Resources

| Resource | Type | Region | Address Space |
| --- | --- | --- | --- |
| RG-BlackPhoenix | Resource Group | Global | N/A |
| vnet-identity | Virtual Network | East US 2 | 10.1.0.0/16 |
| vnet-soc | Virtual Network | West US 2 | 10.2.0.0/16 |
| vnet-attack | Virtual Network | Central US | 10.3.0.0/16 |
| vnet-automation | Virtual Network | South Central US | 10.4.0.0/16 |
| nsg-identity | Network Security Group | East US 2 | N/A |
| nsg-soc | Network Security Group | West US 2 | N/A |
| nsg-attack | Network Security Group | Central US | N/A |
| nsg-automation | Network Security Group | South Central US | N/A |

---

## Subnets

| Subnet | VNet | CIDR | Purpose |
| --- | --- | --- | --- |
| victim-subnet | vnet-identity | 10.1.2.0/24 | AD + DNS + Endpoint |
| siem-subnet | vnet-soc | 10.2.3.0/24 | Wazuh + Splunk |
| attack-subnet | vnet-attack | 10.3.1.0/24 | Kali (Attacker) |
| victim-subnet | vnet-attack | 10.3.2.0/24 | Ubuntu (Linux Victim) |
| automation-subnet | vnet-automation | 10.4.3.0/24 | TheHive + Shuffle |

---

## NSG Assignments

| NSG | Attached To |
| --- | --- |
| nsg-identity | vnet-identity / victim-subnet |
| nsg-soc | vnet-soc / siem-subnet |
| nsg-attack | vnet-attack / attack-subnet, victim-subnet |
| nsg-automation | vnet-automation / automation-subnet |

---

## Total Resource Count

| Type | Count |
| --- | --- |
| Resource Group | 1 |
| Virtual Networks | 4 |
| Subnets | 5 |
| NSGs | 4 |
| **Total** | **14** |
