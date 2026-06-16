# BP-WINSERVER Design & Deployment (Phase 3.1)

## Date

2026-06-16

## Status

🟡 Ready for Deployment

---

## Architecture

```text
bp-winserver
├── Computer Name: BP-DC01
├── Region: East US 2
├── Size: Standard_DC2ds_v3
├── OS: Windows Server 2025 Datacenter Azure Edition
├── Private IP: 10.1.2.30 (Static)
├── Public IP: 172.200.164.250
├── NSG: nsg-identity
├── Future Role: AD DS + DNS
└── Domain: blackphoenix.lab
```

---

## Deployment Steps

### Step 1 — Create Public IP

```bash
az network public-ip create \
--resource-group RG-BlackPhoenix \
--name pip-bp-winserver \
--location eastus2 \
--sku Standard \
--version IPv4 \
--allocation-method Static
```

### Step 2 — Create NIC with Static Private IP

```bash
az network nic create \
--resource-group RG-BlackPhoenix \
--name nic-bp-winserver \
--location eastus2 \
--vnet-name vnet-identity \
--subnet victim-subnet \
--network-security-group nsg-identity \
--private-ip-address 10.1.2.30 \
--public-ip-address pip-bp-winserver
```

### Step 3 — Create Windows Server VM

**Important:** Make sure to replace `'YOUR_PASSWORD'` with a secure 12+ character password before executing.

```bash
az vm create \
--resource-group RG-BlackPhoenix \
--name bp-winserver \
--location eastus2 \
--nics nic-bp-winserver \
--image MicrosoftWindowsServer:WindowsServer:2022-datacenter-azure-edition:latest \
--size Standard_B2s \
--admin-username azureadmin \
--admin-password 'YOUR_PASSWORD'
```

---

## Verification Commands

### Verify VM

```bash
az vm show \
--resource-group RG-BlackPhoenix \
--name bp-winserver \
-d \
-o table
```

### Verify NIC

```bash
az network nic show \
--resource-group RG-BlackPhoenix \
--name nic-bp-winserver \
--query ipConfigurations \
-o table
```

### Get Public IP for RDP

```bash
az network public-ip show \
--resource-group RG-BlackPhoenix \
--name pip-bp-winserver \
--query ipAddress \
-o tsv
```

---

## Validation Checklist

- [x] Public IP `pip-bp-winserver` created successfully (172.200.164.250)
- [x] NIC `nic-bp-winserver` created successfully with Private IP `10.1.2.30`
- [x] VM `bp-winserver` deployed and running (Standard_DC2ds_v3)
- [x] Successful RDP connection to `bp-winserver` over Public IP using `azureadmin` credentials
