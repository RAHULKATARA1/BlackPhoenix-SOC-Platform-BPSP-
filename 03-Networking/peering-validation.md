# Peering Validation – Phase 2.2

## Date

2026-06-16

## Status

✅ All Peerings Validated

---

## Validation Results – vnet-soc (Hub)

```
az network vnet peering list --resource-group RG-BlackPhoenix --vnet-name vnet-soc -o table
```

| Name | PeeringState | PeeringSyncLevel | ProvisioningState | AllowForwardedTraffic | AllowGatewayTransit | AllowVirtualNetworkAccess | UseRemoteGateways |
| --- | --- | --- | --- | --- | --- | --- | --- |
| soc-to-identity | Connected | FullyInSync | Succeeded | True | False | True | False |
| soc-to-attack | Connected | FullyInSync | Succeeded | True | False | True | False |
| soc-to-automation | Connected | FullyInSync | Succeeded | True | False | True | False |

---

## Validation Results – vnet-identity (Spoke)

```
az network vnet peering list --resource-group RG-BlackPhoenix --vnet-name vnet-identity -o table
```

| Name | PeeringState | PeeringSyncLevel | ProvisioningState |
| --- | --- | --- | --- |
| identity-to-soc | Connected | FullyInSync | Succeeded |
| identity-to-attack | Connected | FullyInSync | Succeeded |

---

## Validation Results – vnet-attack (Spoke)

```
az network vnet peering list --resource-group RG-BlackPhoenix --vnet-name vnet-attack -o table
```

| Name | PeeringState | PeeringSyncLevel | ProvisioningState |
| --- | --- | --- | --- |
| attack-to-soc | Connected | FullyInSync | Succeeded |
| attack-to-identity | Connected | FullyInSync | Succeeded |

---

## Validation Results – vnet-automation (Spoke)

```
az network vnet peering list --resource-group RG-BlackPhoenix --vnet-name vnet-automation -o table
```

| Name | PeeringState | PeeringSyncLevel | ProvisioningState |
| --- | --- | --- | --- |
| automation-to-soc | Connected | FullyInSync | Succeeded |

---

## Summary

| Check | Result |
| --- | --- |
| Identity ↔ SOC Connected | ✅ |
| SOC ↔ Attack Connected | ✅ |
| SOC ↔ Automation Connected | ✅ |
| Identity ↔ Attack Connected | ✅ |
| All PeeringState = Connected | ✅ |
| All PeeringSyncLevel = FullyInSync | ✅ |
| All ProvisioningState = Succeeded | ✅ |
| AllowForwardedTraffic = True (all) | ✅ |
| AllowVirtualNetworkAccess = True (all) | ✅ |
| AllowGatewayTransit = False (all) | ✅ |
| UseRemoteGateways = False (all) | ✅ |

---

## Resource GUIDs

| Peering Pair | Resource GUID |
| --- | --- |
| Identity ↔ SOC | 090bd094-64be-0c30-3dcc-194b3690d389 |
| Attack ↔ SOC | 38ea6e34-2dc4-004b-0a2c-09dd78058a68 |
| Automation ↔ SOC | 364dec44-84d3-0fec-2203-ce789636b96c |
| Identity ↔ Attack | 31e1bea0-497a-0c7b-37e0-10964e9559e1 |
