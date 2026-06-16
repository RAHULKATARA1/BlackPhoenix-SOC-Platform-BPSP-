# NSG Policy – BlackPhoenix SOC Platform

## Objective

Define Network Security Group rules for all subnets across all regions.

---

## VNet / Subnet Reference

| VNet | Subnet CIDR | Region | Purpose |
| --- | --- | --- | --- |
| vnet-identity | 10.1.2.0/24 | East US 2 | Identity (AD + DNS + Endpoint) |
| vnet-soc | 10.2.3.0/24 | West US 2 | SOC (Wazuh + Splunk) |
| vnet-attack | 10.3.1.0/24 | Central US | Attack (Kali) |
| vnet-attack | 10.3.2.0/24 | Central US | Attack (Ubuntu Victim) |
| vnet-automation | 10.4.3.0/24 | South Central US | Automation (TheHive + Shuffle) |

---

## Default NSG Rules (All Subnets)

### Inbound

| Priority | Name | Source | Destination | Port | Protocol | Action |
| --- | --- | --- | --- | --- | --- | --- |
| 100 | Allow-SSH-RDP-Admin | Admin IP | Any | 22, 3389 | TCP | Allow |
| 200 | Allow-Internal-VNet | VirtualNetwork | VirtualNetwork | * | Any | Allow |
| 4096 | Deny-All-Inbound | Any | Any | * | Any | Deny |

### Outbound

| Priority | Name | Source | Destination | Port | Protocol | Action |
| --- | --- | --- | --- | --- | --- | --- |
| 100 | Allow-Internal-VNet | VirtualNetwork | VirtualNetwork | * | Any | Allow |
| 200 | Allow-Internet-Out | Any | Internet | 443, 80 | TCP | Allow |
| 4096 | Deny-All-Outbound | Any | Any | * | Any | Deny |

---

## SOC Subnet Specific Rules (10.2.3.0/24)

### Inbound

| Priority | Name | Source | Destination | Port | Protocol | Action |
| --- | --- | --- | --- | --- | --- | --- |
| 110 | Allow-Wazuh-Agent | 10.1.0.0/16, 10.3.0.0/16, 10.4.0.0/16 | 10.2.3.50 | 1514, 1515 | TCP | Allow |
| 120 | Allow-Splunk-HEC | 10.1.0.0/16, 10.3.0.0/16, 10.4.0.0/16 | 10.2.3.60 | 8088 | TCP | Allow |
| 130 | Allow-Splunk-Web | Admin IP | 10.2.3.60 | 8000 | TCP | Allow |
| 140 | Allow-Wazuh-Web | Admin IP | 10.2.3.50 | 443 | TCP | Allow |

---

## Automation Subnet Specific Rules (10.4.3.0/24)

### Inbound

| Priority | Name | Source | Destination | Port | Protocol | Action |
| --- | --- | --- | --- | --- | --- | --- |
| 110 | Allow-TheHive-API | 10.2.3.0/24 | 10.4.3.70 | 9000 | TCP | Allow |
| 120 | Allow-Shuffle-Web | Admin IP | 10.4.3.80 | 3443 | TCP | Allow |
| 130 | Allow-Shuffle-Webhook | 10.2.3.0/24 | 10.4.3.80 | 3443 | TCP | Allow |

---

## Identity Subnet Specific Rules (10.1.2.0/24)

### Inbound

| Priority | Name | Source | Destination | Port | Protocol | Action |
| --- | --- | --- | --- | --- | --- | --- |
| 110 | Allow-DNS | 10.0.0.0/8 | 10.1.2.30 | 53 | TCP/UDP | Allow |
| 120 | Allow-AD-Auth | 10.0.0.0/8 | 10.1.2.30 | 389, 636, 88, 445 | TCP | Allow |

---

## Notes

- Replace "Admin IP" with your actual public IP before deployment
- NSG rules will be refined during Phase 2 (Networking)
- All inter-VNet traffic flows through VNet Peering
