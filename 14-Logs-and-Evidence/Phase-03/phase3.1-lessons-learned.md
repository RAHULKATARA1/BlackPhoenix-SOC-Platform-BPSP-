# Phase 3.1 Lessons Learned

## Date

2026-06-16

---

## 1. Azure Quotas ≠ Azure Capacity

During the initial deployment attempts for `bp-winserver`, multiple VM SKUs (`Standard_B2s`, `Standard_B1ms`, `Standard_DS1_v2`) failed with a `SkuNotAvailable` error.

**Finding:** Having a sufficient vCPU quota (e.g., 4 vCPUs per region) does **not** guarantee that the underlying Azure datacenter has the physical capacity to provision that specific hardware SKU at that moment. Regional capacity is dynamic.

---

## 2. Failed Deployments Leave Resources Behind

When a `az vm create` command fails mid-deployment (e.g., due to `SkuNotAvailable`), the Azure Resource Manager may successfully provision prerequisite resources before failing on the compute instance.

**Finding:** In our case, the NIC (`nic-bp-winserver`) was provisioned and held the private IP `10.1.2.30`. When the VM deployment finally succeeded via the Azure Portal, it generated a new NIC and received the IP `10.1.2.4`, leaving an orphaned NIC behind.

**Resolution:** Cloud engineers must proactively inspect the Resource Group for orphaned NICs, Public IPs, Disks, and NSGs after a failed deployment. We deleted the orphaned NIC and reassigned `10.1.2.30` to the active NIC using `az network nic ip-config update`.

---

## 3. Portal vs. CLI Utility

**Finding:** While the Azure CLI is excellent for automation, troubleshooting, cleanup, and specific configurations, the Azure Portal is vastly superior for **discovering available capacity**. When the CLI failed with `SkuNotAvailable`, the Portal explicitly listed which SKUs (like `Standard_DC2ds_v3`) had available capacity in `East US 2`.

**Lesson:** Use the Portal to survey the environment and constraints, and use the CLI to execute and standardize the deployment.
