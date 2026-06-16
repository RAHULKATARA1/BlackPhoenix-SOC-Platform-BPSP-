# Architecture Notes – Phase 2

## Date

2026-06-16

---

## Key Decision: Hybrid Hub-and-Spoke (Not Full Mesh)

### Original Plan

Full Mesh peering (6 bidirectional peerings = 12 peering entries)

### Revised Plan

Hybrid Hub-and-Spoke with vnet-soc as hub, plus an additional link between Identity and Attack (4 bidirectional peerings = 8 peering entries)

### Rationale

* 8 VMs, 8-10 day lab — Full Mesh is overkill
* Fewer peerings = lower cost, less management
* SOC naturally receives all traffic (logs, alerts, cases)
* Easier troubleshooting with centralized hub
* More realistic enterprise SOC architecture
* No single-point-of-failure concern for a lab environment

---

## Topology

```
             vnet-automation
                     |
                     |
vnet-identity ---- vnet-soc ---- vnet-attack
      |                                |
      +--------------------------------+
```

---

## Peering Pairs

1. vnet-soc ↔ vnet-identity
2. vnet-soc ↔ vnet-attack
3. vnet-soc ↔ vnet-automation
4. vnet-identity ↔ vnet-attack

---

## DNS

* Primary DNS: 10.1.2.30 (bp-winserver)
* Domain: blackphoenix.lab
* Configured per-VNet after AD deployment

---

## Routing

* Azure system routes only
* No custom UDRs, VPN, ExpressRoute, or NVAs

---

## NSG Principle

* Deny all by default
* Allow only documented ports per subnet
* Management access (RDP/SSH) restricted to admin IP

---

## Screenshots Required

Save to this folder:
* network-design.png
* peering-design.png
* traffic-flow.png
