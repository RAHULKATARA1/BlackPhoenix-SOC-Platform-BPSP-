# VM Quota Audit Plan (Pre-Phase 3)

## Objective

Before deploying any VMs in the BlackPhoenix SOC lab (which utilizes a $200 Azure Free Trial), we must audit the compute quotas in our target regions to ensure we don't hit the `OperationNotAllowed` (Quota Exceeded) error mid-deployment.

Free Trial subscriptions often have a strict **4 vCPU limit per region**.

## Our Target Regions

1. **East US 2** (vnet-identity)
2. **West US 2** (vnet-soc)
3. **Central US** (vnet-attack)
4. **South Central US** (vnet-automation)

## Quota Checking Commands

Run these commands to verify how many vCPUs you are allowed to provision in each region. We are looking for the `Standard FSv2 Family vCPUs` or `Standard Dsv3 Family vCPUs` limit.

### 1. East US 2
```bash
az vm list-usage --location eastus2 --query "[?name.value=='cores'].{Name:name.localizedValue, CurrentValue:currentValue, Limit:limit}" -o table
```

### 2. West US 2
```bash
az vm list-usage --location westus2 --query "[?name.value=='cores'].{Name:name.localizedValue, CurrentValue:currentValue, Limit:limit}" -o table
```

### 3. Central US
```bash
az vm list-usage --location centralus --query "[?name.value=='cores'].{Name:name.localizedValue, CurrentValue:currentValue, Limit:limit}" -o table
```

### 4. South Central US
```bash
az vm list-usage --location southcentralus --query "[?name.value=='cores'].{Name:name.localizedValue, CurrentValue:currentValue, Limit:limit}" -o table
```

## Review Criteria

Look at the **Limit** column. If your limit is `4` per region, you must carefully select VM sizes that fit within 4 vCPUs per region.

Example of what to look for in your planned architecture:
* **East US 2:** WinServer (2 vCPUs) + Win10 (2 vCPUs) = 4 vCPUs total (Fits in a 4-vCPU quota)
* **West US 2:** Wazuh (2 vCPUs) + Splunk (2 vCPUs) = 4 vCPUs total (Fits in a 4-vCPU quota)

### Recommended Free-Tier Friendly VM Sizes:
* `Standard_B2s` (2 vCPUs, 4GB RAM) -> Good for AD, Win10, Wazuh, Splunk, Ubuntu.
* `Standard_B2ms` (2 vCPUs, 8GB RAM) -> If more RAM is needed.

**Action Item:** Execute the quota checks above and verify the limits before proceeding to Phase 3.
