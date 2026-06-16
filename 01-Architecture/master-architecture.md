# Resource Group
RG-BlackPhoenix

# Region Assignment

## East US 2
Purpose: Identity Region
VMs:
* bp-winserver
* bp-win10

## West US 2
Purpose: SOC Region
VMs:
* bp-wazuh
* bp-splunk

## Central US
Purpose: Attack Region
VMs:
* bp-kali
* bp-ubuntu

## South Central US
Purpose: Automation Region
VMs:
* bp-thehive
* bp-shuffle

# VM Matrix

| VM | Region | Size | Role |
| --- | --- | --- | --- |
| bp-winserver | East US 2 | Standard_DC2ds_v3 | AD + DNS |
| bp-win10 | East US 2 | B2s | Endpoint |
| bp-wazuh | West US 2 | B2s | SIEM |
| bp-splunk | West US 2 | B2s | SIEM |
| bp-kali | Central US | B1ms | Attacker |
| bp-ubuntu | Central US | B1ms | Linux Victim |
| bp-thehive | South Central US | B2s | Case Management |
| bp-shuffle | South Central US | B1ms | SOAR |
