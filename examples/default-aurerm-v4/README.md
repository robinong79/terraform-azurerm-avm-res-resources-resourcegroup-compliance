<!-- BEGIN_TF_DOCS -->
# terraform-azurerm-ecm-res-resource-resourcegroup for azurerm v4

This module is used to deploy an Azure Resource Group

```hcl
terraform {
  required_version = ">= 1.9, < 2.0"
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 4.0"
    }
    random = {
      source  = "hashicorp/random"
      version = ">= 3.5.0, < 4.0.0"
    }
  }
}

provider "azurerm" {
  skip_provider_registration = true
  features {
    resource_group {
      prevent_deletion_if_contains_resources = false
    }
  }
}

# Importing the Azure naming module to ensure resources have unique CAF compliant names.
module "naming" {
  source  = "Azure/naming/azurerm"
  version = " >= 0.4.0"
}

module "regions" {
  source  = "Azure/regions/azurerm"
  version = ">= 0.3.0"
}

# This allows us to randomize the region for the resource group.
resource "random_integer" "region_index" {
  max = length(module.regions.regions) - 1
  min = 0
}

module "resource_group" {
  source   = "../../"
  location = module.regions.regions[random_integer.region_index.result].name
  name     = module.naming.resource_group.name_unique
}

output "name" {
  description = "The name of the resource group"
  value       = module.resource_group.name
}

# Module owners should include the full resource via a 'resource' output
# https://confluence.ei.leidos.com/display/ECM/Terraform+ECM+Style+Guide#TerraformECMStyleGuide-TFFR2-Category:Outputs-AdditionalTerraformOutputs
output "resource" {
  description = "This is the full output for the resource group."
  value       = module.resource_group
}

output "resource_id" {
  description = "The resource Id of the resource group"
  value       = module.resource_group.resource_id
}
```

<!-- markdownlint-disable MD033 -->
## Requirements

The following requirements are needed by this module:

- <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) (>= 1.9, < 2.0)

- <a name="requirement_azurerm"></a> [azurerm](#requirement\_azurerm) (~> 4.0)

- <a name="requirement_random"></a> [random](#requirement\_random) (>= 3.5.0, < 4.0.0)

## Resources

The following resources are used by this module:

- [random_integer.region_index](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/integer) (resource)

<!-- markdownlint-disable MD013 -->
## Required Inputs

No required inputs.

## Optional Inputs

No optional inputs.

## Outputs

The following outputs are exported:

### <a name="output_name"></a> [name](#output\_name)

Description: The name of the resource group

### <a name="output_resource"></a> [resource](#output\_resource)

Description: This is the full output for the resource group.

### <a name="output_resource_id"></a> [resource\_id](#output\_resource\_id)

Description: The resource Id of the resource group

## Modules

The following Modules are called:

### <a name="module_naming"></a> [naming](#module\_naming)

Source: Azure/naming/azurerm

Version:  >= 0.4.0

### <a name="module_regions"></a> [regions](#module\_regions)

Source: Azure/regions/azurerm

Version: >= 0.3.0

### <a name="module_resource_group"></a> [resource\_group](#module\_resource\_group)

Source: ../../

Version:

<!-- markdownlint-disable-next-line MD041 -->
## Footer Data
<!-- END_TF_DOCS -->