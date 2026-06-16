# Deployment Order – Phase 1

## Date

2026-06-16

---

## Order of Operations

```
1. Resource Group
2. VNets
3. Subnets
4. NSGs
5. VNet Peering (deferred to Phase 2)
6. Validation
7. Evidence
```

---

## Detailed Steps

### Step 1 – Resource Group

```
az group create --name RG-BlackPhoenix --location eastus2
```

### Step 2 – Virtual Networks

```
1. vnet-identity  (East US 2)       10.1.0.0/16
2. vnet-soc       (West US 2)       10.2.0.0/16
3. vnet-attack    (Central US)      10.3.0.0/16
4. vnet-automation (South Central US) 10.4.0.0/16
```

### Step 3 – Subnets

```
1. victim-subnet     in vnet-identity    10.1.2.0/24
2. siem-subnet       in vnet-soc         10.2.3.0/24
3. attack-subnet     in vnet-attack      10.3.1.0/24
4. victim-subnet     in vnet-attack      10.3.2.0/24
5. automation-subnet in vnet-automation  10.4.3.0/24
```

### Step 4 – NSGs

```
1. nsg-identity   (East US 2)
2. nsg-soc        (West US 2)
3. nsg-attack     (Central US)
4. nsg-automation (South Central US)
```

### Step 5 – Attach NSGs to Subnets

```
1. nsg-identity   → vnet-identity / victim-subnet
2. nsg-soc        → vnet-soc / siem-subnet
3. nsg-attack     → vnet-attack / attack-subnet
4. nsg-attack     → vnet-attack / victim-subnet
5. nsg-automation → vnet-automation / automation-subnet
```

### Step 6 – Validation

```
az group show --name RG-BlackPhoenix -o table
az network vnet list --resource-group RG-BlackPhoenix -o table
az network nsg list --resource-group RG-BlackPhoenix -o table
```

### Step 7 – Evidence

Capture screenshots and save to:
```
14-Logs-and-Evidence/Phase-01/
```
