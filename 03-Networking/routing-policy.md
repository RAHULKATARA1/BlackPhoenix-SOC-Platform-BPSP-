# Routing Policy – Phase 2

## Date

2026-06-16

## Status

LOCKED ✓

---

## Policy

No custom route tables.

Azure system routes are sufficient for this deployment.

---

## Traffic Flow

```
VM
 ↓
VNet
 ↓
Peering
 ↓
Destination VNet
 ↓
Destination VM
```

---

## What We Are NOT Using

* No VPN Gateway
* No ExpressRoute
* No Network Virtual Appliances (NVAs)
* No User-Defined Routes (UDRs)
* No Azure Firewall
* No Azure Route Server

---

## Rationale

* Lab environment does not require complex routing
* Azure system routes handle VNet-to-VNet traffic via peering automatically
* NSGs provide sufficient traffic control at the subnet level
* Simpler architecture means easier troubleshooting
* Cost-effective for student/lab environment

---

## Route Behavior

| Traffic Type | Route |
| --- | --- |
| Intra-VNet | Azure system route (direct) |
| Inter-VNet (peered) | Azure system route (via peering) |
| Internet outbound | Azure default route |
| On-premises | Not applicable |

---

## Notes

* If future phases require traffic inspection, Azure Firewall or NVA can be added
* For now, NSGs at subnet level provide all necessary traffic filtering
