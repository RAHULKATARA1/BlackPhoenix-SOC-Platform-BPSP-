# VNets

## Identity
vnet-identity
10.1.0.0/16

## SOC
vnet-soc
10.2.0.0/16

## Attack
vnet-attack
10.3.0.0/16

## Automation
vnet-automation
10.4.0.0/16

# Subnets

## Identity
10.1.2.0/24

## SOC
10.2.3.0/24

## Attack
10.3.1.0/24
10.3.2.0/24

## Automation
10.4.3.0/24

# Peering Model
SOC Hub-and-Spoke:
               vnet-automation
                       |
                       |
vnet-attack ----- vnet-soc ----- vnet-identity

# IP Plan

| VM | Private IP |
| --- | --- |
| bp-kali | 10.3.1.10 |
| bp-win10 | 10.1.2.20 |
| bp-winserver | 10.1.2.30 |
| bp-ubuntu | 10.3.2.40 |
| bp-wazuh | 10.2.3.50 |
| bp-splunk | 10.2.3.60 |
| bp-thehive | 10.4.3.70 |
| bp-shuffle | 10.4.3.80 |

DNS:
10.1.2.30

Domain:
blackphoenix.lab
