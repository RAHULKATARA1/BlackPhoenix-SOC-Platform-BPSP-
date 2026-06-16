# Validation Checklist – Phase 2 Networking

## Date

2026-06-16

---

## Pre-Implementation Checks

| # | Check | Status |
| --- | --- | --- |
| 1 | Network overview documented | ✓ |
| 2 | Peering model selected (Hybrid Hub-and-Spoke) | ✓ |
| 3 | DNS strategy documented | ✓ |
| 4 | Routing policy documented | ✓ |
| 5 | NSG strategy documented | ✓ |
| 6 | Connectivity matrix completed | ✓ |
| 7 | Diagrams created | ✓ |

---

## Post-Implementation Checks (After Phase 2.2)

| # | Check | Command | Status |
| --- | --- | --- | --- |
| 1 | Peerings connected | `az network vnet peering list --resource-group RG-BlackPhoenix --vnet-name vnet-soc -o table` | □ |
| 2 | Peering state = Connected | Check PeeringState column | □ |
| 3 | DNS resolves across regions | `nslookup bp-wazuh.blackphoenix.lab 10.1.2.30` | □ |
| 4 | Windows can ping Wazuh | `ping 10.2.3.50` from bp-win10 | □ |
| 5 | Ubuntu can reach Splunk | `ping 10.2.3.60` from bp-ubuntu | □ |
| 6 | Kali reaches Win10 | `ping 10.1.2.20` from bp-kali | □ |
| 7 | TheHive reaches Shuffle | `ping 10.4.3.80` from bp-thehive | □ |
| 8 | NSGs permit required traffic | Test each connectivity path | □ |
| 9 | No public exposure except management | `az network nsg rule list` review | □ |

---

## Validation Commands (Quick Run)

```bash
# List all peerings from SOC hub
az network vnet peering list --resource-group RG-BlackPhoenix --vnet-name vnet-soc -o table

# Verify peering states
az network vnet peering show --resource-group RG-BlackPhoenix --vnet-name vnet-soc --name peer-soc-to-identity -o table
az network vnet peering show --resource-group RG-BlackPhoenix --vnet-name vnet-soc --name peer-soc-to-attack -o table
az network vnet peering show --resource-group RG-BlackPhoenix --vnet-name vnet-soc --name peer-soc-to-automation -o table
```
