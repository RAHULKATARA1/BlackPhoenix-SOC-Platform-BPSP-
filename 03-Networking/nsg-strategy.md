# NSG Strategy – Phase 2

## Date

2026-06-16

## Status

LOCKED ✓

---

## Principle

```
Deny by default
Allow only required traffic
```

---

## Identity Ports (vnet-identity / nsg-identity)

| Port | Protocol | Purpose |
| --- | --- | --- |
| 53 | TCP/UDP | DNS |
| 88 | TCP/UDP | Kerberos |
| 135 | TCP | RPC |
| 389 | TCP/UDP | LDAP |
| 445 | TCP | SMB |
| 3389 | TCP | RDP |

---

## SOC Ports (vnet-soc / nsg-soc)

| Port | Protocol | Purpose |
| --- | --- | --- |
| 1514 | TCP | Wazuh Agent |
| 1515 | TCP | Wazuh Registration |
| 55000 | TCP | Wazuh API |
| 9997 | TCP | Splunk Forwarder |
| 8000 | TCP | Splunk Web |
| 8089 | TCP | Splunk Management |

---

## Automation Ports (vnet-automation / nsg-automation)

| Port | Protocol | Purpose |
| --- | --- | --- |
| 9000 | TCP | TheHive |
| 3001 | TCP | Shuffle |
| 443 | TCP | HTTPS |

---

## Attack Ports (vnet-attack / nsg-attack)

| Port | Protocol | Purpose |
| --- | --- | --- |
| 22 | TCP | SSH |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 445 | TCP | SMB |
| 3389 | TCP | RDP |
| 5985 | TCP | WinRM |
| 5986 | TCP | WinRM HTTPS |

---

## NSG Rule Design Principles

1. **Deny All Inbound** as the last rule (lowest priority)
2. **Allow specific ports** from specific source VNets only
3. **Management access** (RDP/SSH) restricted to admin IP only
4. **Inter-VNet traffic** flows through peering, filtered by NSGs
5. **No public exposure** except for management access

---

## Implementation Order

1. Create NSG rules for Identity subnet
2. Create NSG rules for SOC subnet
3. Create NSG rules for Attack subnets
4. Create NSG rules for Automation subnet
5. Validate with connectivity tests
