# Phase 1 – Azure Foundation Overview

## Status

🟡 In Progress

## Purpose

Create the cloud foundation for BlackPhoenix SOC Platform.

## Scope

Create:

* Resource Group
* VNets
* Subnets
* NSGs
* Global VNet Peering

No compute resources in this phase.

## Target State

```
Azure Subscription
└── RG-BlackPhoenix
    │
    ├── East US 2
    │   └── vnet-identity (10.1.0.0/16)
    │       └── victim-subnet (10.1.2.0/24)
    │       └── nsg-identity
    │
    ├── West US 2
    │   └── vnet-soc (10.2.0.0/16)
    │       └── siem-subnet (10.2.3.0/24)
    │       └── nsg-soc
    │
    ├── Central US
    │   └── vnet-attack (10.3.0.0/16)
    │       ├── attack-subnet (10.3.1.0/24)
    │       └── victim-subnet (10.3.2.0/24)
    │       └── nsg-attack
    │
    └── South Central US
        └── vnet-automation (10.4.0.0/16)
            └── automation-subnet (10.4.3.0/24)
            └── nsg-automation
```

## Dependencies

* Azure Subscription (active)
* Azure CLI installed
* Phase 0 completed and locked

## Exit Criteria

```
✓ Resource Group created
✓ 4 VNets created
✓ 5 Subnets created
✓ 4 NSGs created
✓ NSGs attached to subnets
✓ Evidence captured
✓ Documentation updated
```
