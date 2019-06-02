# Storage Space Direct

## Introduction

This template deploys a Microsaoft Storage Space Direct infrastructure that can be used, for example, in combination with the RDS Gateway module.

## Security Controls

The following security controls can be met through configuration of this template:

* None documented yet

## Dependancies

* [Resource Groups](https://github.com/canada-ca/accelerators_accelerateurs-azure/blob/master/Templates/arm/resourcegroups/latest/readme.md)
* [Keyvault](https://github.com/canada-ca/accelerators_accelerateurs-azure/blob/master/Templates/arm/keyvaults/latest/readme.md)
* [VNET-Subnet](https://github.com/canada-ca/accelerators_accelerateurs-azure/blob/master/Templates/arm/vnet-subnet/latest/readme.md)

## Parameter format

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "namePrefix": {
            "value": "vals2d"
        },
        "storageAccountType": {
            "value": "Premium_LRS"
        },
        "vmSize": {
            "value": "Standard_DS2_v2"
        },
        "vmCount": {
            "value": 2
        },
        "vmDiskSize": {
            "value": 256
        },
        "vmDiskCount": {
            "value": 2
        },
        "existingDomainName": {
            "value": "validate.gc.ca.local"
        },
        "keyVaultResourceGroupName": {
            "value": "PwS2-validate-s2d-RG"
        },
        "keyVaultName": {
            "value": "PwS2-validate-[unique]"
        },
        "adminUsername": {
            "value": "azureadmin"
        },
        "adminPasswordSecret": {
            "value": "server2016DefaultPassword"
        },
        "existingVirtualNetworkRGName": {
            "value": "PwS2-validate-s2d-RG"
        },
        "existingVirtualNetworkName": {
            "value": "PwS2-validate-s2d-VNET"
        },
        "existingSubnetName": {
            "value": "APP"
        },
        "sofsName": {
            "value": "fs01"
        },
        "shareName": {
            "value": "data"
        },
        "tagValues": {
            "value": {
                "Owner": "build.pipeline@tpsgc-pwgsc.gc.ca",
                "CostCenter": "PSPC-EA",
                "Enviroment": "Validate",
                "Classification": "Unclassified",
                "Organizations": "PSPC-CCC-E&O"
            }
        }
    }
}
```

## Parameter Values

### Main Template

| Name                         | Type    | Required | Value                                                                                                                                                                                                                                   |
| ---------------------------- | ------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| containerSasToken            | string  | No       | SAS Token received as a parameter                                                                                                                                                                                                       |
| _artifactsLocation           | string  | No       | Dynamically derived from the deployment template link uri                                                                                                                                                                               |
| namePrefix                   | string  | Yes      | Naming prefix for each new resource created. 3-char min, 8-char max, lowercase alphanumeric                                                                                                                                             |
| storageAccountType           | string  | Yes      | Type of new Storage Accounts (Standard_LRS, Standard_GRS, Standard_RAGRS or Premium_LRS) to be created to store VM disks                                                                                                                |
| vmSize                       | string  | Yes      | Size of the S2D VMs to be created. Eg: Standard_DS2_v2                                                                                                                                                                                  |
| vmCount                      | integer | Yes      | Number of S2D VMs to be created in cluster (Min=2, Max=3)                                                                                                                                                                               |
| vmDiskSize                   | integer | Yes      | Size of each data disk in GB on each S2D VM (Min=128, Max=1023)                                                                                                                                                                         |
| vmDiskCount                  | integer | Yes      | Number of data disks on each S2D VM (Min=2, Max=32). Ensure that the VM size you've selected will support this number of data disks.                                                                                                    |
| existingDomainName           | string  | Yes      | DNS domain name for existing Active Directory domain                                                                                                                                                                                    |
| keyVaultResourceGroupName    | string  | Yes      | The name of the resource group containing the keyvault.                                                                                                                                                                                 |
| keyVaultName                 | string  | Yes      | The name of the keyvault containing the secret.                                                                                                                                                                                         |
| adminUsername                | string  | Yes      | Name of the Administrator of the existing Active Directory Domain                                                                                                                                                                       |
| adminPasswordSecret          | string  | Yes      | The name of the keyvault secret containing the password for the Administrator account of the existing Active Directory Domain                                                                                                           |
| existingVirtualNetworkRGName | string  | Yes      | Resource Group Name for the existing VNET.                                                                                                                                                                                              |
| existingVirtualNetworkName   | string  | Yes      | Name of the existing VNET.                                                                                                                                                                                                              |
| existingSubnetName           | string  | Yes      | Name of the existing subnet in the existing VNET to which the S2D VMs should be deployed                                                                                                                                                |
| ClusterIp                    | string  | Yes      | The IP to use for the SQL cluster. Make sure this IP is unique in the Active Directory hosting the cluster vms. If it is a duplicate the other cluster that had this IP will fail. IP should be between 169.254.1.1 and 169.254.255.255 |
| sofsName                     | string  | Yes      | Name of clustered Scale-Out File Server role                                                                                                                                                                                            |
| shareName                    | string  | Yes      | Name of shared data folder on clustered Scale-Out File Server role                                                                                                                                                                      |
| tagValues                    | object  | Yes      | Object containing [tags pairs](#tag-object)                                                                                                                                                                                             |

### Tag object

| Name     | Type   | Required | Value      |
| -------- | ------ | -------- | ---------- |
| tagname1 | string | No       | tag1 value |
| ...      | ...    | ...      | ...        |
| tagnameX | string | No       | tagX value |

## History

| Date     | Release                                                                    | Change                                                        |
| -------- | -------------------------------------------------------------------------- | ------------------------------------------------------------- |
| 20190506 | [20190506](https://github.com/canada-ca-azure-templates/s2d/tree/20190506) | Move to new github structure and add validation               |
| 20190602 | [20190531](https://github.com/canada-ca-azure-templates/s2d/tree/20190602) | Add support for custom cluster IP through template parameter. |
|          |                                                                            | Fix broken link for release 20190506                          |
