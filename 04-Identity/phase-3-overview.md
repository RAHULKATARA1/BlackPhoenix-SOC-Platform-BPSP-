# Phase 3 – Identity Infrastructure Overview

## Status

🟡 In Progress

---

## Objective

Build the central Identity and DNS foundation for the BlackPhoenix SOC Platform. This phase establishes Active Directory, which will be used to authenticate Windows endpoints, provide DNS resolution for all regions, and serve as the core logging source for Windows events.

---

## Architecture

```text
East US 2 (vnet-identity)
├── bp-winserver (10.1.2.30) - Domain Controller & DNS
└── bp-win10 (10.1.2.40) - Domain Joined Endpoint
```

---

## Build Order

1. **Phase 3.1:** Deploy `bp-winserver` VM, NIC, and Public IP
2. **Phase 3.2:** Promote `bp-winserver` to Domain Controller (`blackphoenix.lab`) and configure DNS
3. **Phase 3.3:** Configure Active Directory (OUs, Users, Groups, GPOs)
4. **Phase 3.4:** Deploy `bp-win10` VM and join it to the domain
5. **Phase 3.5:** Update VNet DNS settings across all regions to point to `10.1.2.30`

---

## Exit Criteria

* Active Directory Domain Services (AD DS) installed and running.
* `blackphoenix.lab` domain functional.
* Forward and reverse DNS zones configured.
* Standard user and admin accounts created.
* Endpoints successfully joined to the domain.
* VNet DNS updated globally to use `10.1.2.30`.
