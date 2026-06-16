# Validation Checklist – Phase 1

## Date

2026-06-16

---

## Checklist

| # | Check | Command | Status |
| --- | --- | --- | --- |
| 1 | Resource Group exists | `az group show --name RG-BlackPhoenix -o table` | □ |
| 2 | vnet-identity exists (10.1.0.0/16) | `az network vnet show --resource-group RG-BlackPhoenix --name vnet-identity -o table` | □ |
| 3 | vnet-soc exists (10.2.0.0/16) | `az network vnet show --resource-group RG-BlackPhoenix --name vnet-soc -o table` | □ |
| 4 | vnet-attack exists (10.3.0.0/16) | `az network vnet show --resource-group RG-BlackPhoenix --name vnet-attack -o table` | □ |
| 5 | vnet-automation exists (10.4.0.0/16) | `az network vnet show --resource-group RG-BlackPhoenix --name vnet-automation -o table` | □ |
| 6 | victim-subnet in vnet-identity (10.1.2.0/24) | `az network vnet subnet show --resource-group RG-BlackPhoenix --vnet-name vnet-identity --name victim-subnet -o table` | □ |
| 7 | siem-subnet in vnet-soc (10.2.3.0/24) | `az network vnet subnet show --resource-group RG-BlackPhoenix --vnet-name vnet-soc --name siem-subnet -o table` | □ |
| 8 | attack-subnet in vnet-attack (10.3.1.0/24) | `az network vnet subnet show --resource-group RG-BlackPhoenix --vnet-name vnet-attack --name attack-subnet -o table` | □ |
| 9 | victim-subnet in vnet-attack (10.3.2.0/24) | `az network vnet subnet show --resource-group RG-BlackPhoenix --vnet-name vnet-attack --name victim-subnet -o table` | □ |
| 10 | automation-subnet in vnet-automation (10.4.3.0/24) | `az network vnet subnet show --resource-group RG-BlackPhoenix --vnet-name vnet-automation --name automation-subnet -o table` | □ |
| 11 | nsg-identity exists | `az network nsg show --resource-group RG-BlackPhoenix --name nsg-identity -o table` | □ |
| 12 | nsg-soc exists | `az network nsg show --resource-group RG-BlackPhoenix --name nsg-soc -o table` | □ |
| 13 | nsg-attack exists | `az network nsg show --resource-group RG-BlackPhoenix --name nsg-attack -o table` | □ |
| 14 | nsg-automation exists | `az network nsg show --resource-group RG-BlackPhoenix --name nsg-automation -o table` | □ |
| 15 | NSGs attached to correct subnets | Verify via subnet show commands | □ |

---

## Validation Commands (Quick Run)

```bash
az group show --name RG-BlackPhoenix -o table
az network vnet list --resource-group RG-BlackPhoenix -o table
az network nsg list --resource-group RG-BlackPhoenix -o table
```
