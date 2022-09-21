<p align="center">
    <a href="https://github.com/tomarv2/terraform-azure-container-instance/actions/workflows/pre-commit.yml" alt="Pre Commit">
        <img src="https://github.com/tomarv2/terraform-azure-container-instance/actions/workflows/pre-commit.yml/badge.svg?branch=main" /></a>
    <a href="https://www.apache.org/licenses/LICENSE-2.0" alt="license">
        <img src="https://img.shields.io/github/license/tomarv2/terraform-azure-container-instance" /></a>
    <a href="https://github.com/tomarv2/terraform-azure-container-instance/tags" alt="GitHub tag">
        <img src="https://img.shields.io/github/v/tag/tomarv2/terraform-azure-container-instance" /></a>
    <a href="https://github.com/tomarv2/terraform-azure-container-instance/pulse" alt="Activity">
        <img src="https://img.shields.io/github/commit-activity/m/tomarv2/terraform-azure-container-instance" /></a>
    <a href="https://stackoverflow.com/users/6679867/tomarv2" alt="Stack Exchange reputation">
        <img src="https://img.shields.io/stackexchange/stackoverflow/r/6679867"></a>
    <a href="https://twitter.com/intent/follow?screen_name=varuntomar2019" alt="follow on Twitter">
        <img src="https://img.shields.io/twitter/follow/varuntomar2019?style=social&logo=twitter"></a>
</p>

## Terraform module for Azure Container Instance (ACI)

> :arrow_right:  Terraform module for [AWS ECS](https://registry.terraform.io/modules/tomarv2/ecs/aws/latest)

## ACI cluster requires

:point_right: An existing Resource Group

:point_right: Existing MSI with role assignment

### Versions

- Module tested for Terraform 1.0.1.
- Azure provider version [3.21.1](https://registry.terraform.io/providers/hashicorp/azurerm/latest)
- `main` branch: Provider versions not pinned to keep up with Terraform releases
- `tags` releases: Tags are pinned with versions (use <a href="https://github.com/tomarv2/terraform-azure-container-instance/tags" alt="GitHub tag">
        <img src="https://img.shields.io/github/v/tag/tomarv2/terraform-azure-container-instance" /></a> in your releases)

### Usage

#### Option 1:

```
terrafrom init
terraform plan -var='teamid=tryme' -var='prjid=project'
terraform apply -var='teamid=tryme' -var='prjid=project'
terraform destroy -var='teamid=tryme' -var='prjid=project'
```
**Note:** With this option please take care of remote state storage

#### Option 2:

#### Recommended method (stores remote state in storage using `prjid` and `teamid` to create directory structure):

- Create python 3.8+ virtual environment
```
python3 -m venv <venv name>
```

- Install package:
```
pip install tfremote --upgrade
```

- Set below environment variables:
```
export TF_AZURE_STORAGE_ACCOUNT=tfstatexxxxx # Output of remote_state.sh
export TF_AZURE_CONTAINER=tfstate # Output of remote_state.sh
export ARM_ACCESS_KEY=xxxxxxxxxx # Output of remote_state.sh
```

- Updated `examples` directory to required values

- Run and verify the output before deploying:
```
tf -c=azure plan -var='teamid=foo' -var='prjid=bar'
```

- Run below to deploy:
```
tf -c=azure apply -var='teamid=foo' -var='prjid=bar'
```

- Run below to destroy:
```
tf -c=azure destroy -var='teamid=foo' -var='prjid=bar'
```
**Note:** Read more on [tfremote](https://github.com/tomarv2/tfremote)
##### ACI

```
terraform {
  required_version = ">= 1.0.1"
  required_providers {
    azurerm = {
      version = "~> 2.98"
    }
  }
}

provider "azurerm" {
  features {}
}

module "aci" {
  source = "../../"

  resource_group_name = "demo-resource_group"
  docker_image        = "nginx"
  container_port      = "80"
  # ---------------------------------------------
  # Note: Do not change teamid and prjid once set.
  teamid = var.teamid
  prjid  = var.prjid
}

```

Please refer to examples directory [link](examples) for references.

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.0.1 |
| <a name="requirement_azurerm"></a> [azurerm](#requirement\_azurerm) | ~> 3.21.1 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_azurerm"></a> [azurerm](#provider\_azurerm) | ~> 3.21.1 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [azurerm_container_group.container_group](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/container_group) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_container_port"></a> [container\_port](#input\_container\_port) | Container port | `string` | `"443"` | no |
| <a name="input_container_protocol"></a> [container\_protocol](#input\_container\_protocol) | Container protocol | `string` | `"TCP"` | no |
| <a name="input_containers_config"></a> [containers\_config](#input\_containers\_config) | Containers configurations, defined by this type:<pre>map(<br>  container-name (string) : object({<br>    image                        = string<br>    cpu                          = number<br>    memory                       = number<br>    environment_variables        = optional(map)<br>    secure_environment_variables = optional(map)<br>    commands                     = optional(list)<br>    ports = list(object({<br>      port     = number<br>      protocol = string<br>    }))<br>  })<br>)</pre> | `map(any)` | n/a | yes |
| <a name="input_cpu_allocation"></a> [cpu\_allocation](#input\_cpu\_allocation) | CPU allocation | `string` | `"0.5"` | no |
| <a name="input_dns_name_label"></a> [dns\_name\_label](#input\_dns\_name\_label) | The DNS label/name for the container group's IP. Changing this forces a new resource to be created. | `string` | `null` | no |
| <a name="input_env_variables"></a> [env\_variables](#input\_env\_variables) | Environment variables | `map(any)` | `{}` | no |
| <a name="input_exposed_port"></a> [exposed\_port](#input\_exposed\_port) | n/a | `list(map(any))` | `[]` | no |
| <a name="input_extra_tags"></a> [extra\_tags](#input\_extra\_tags) | Additional tags to associate | `map(string)` | `{}` | no |
| <a name="input_identity"></a> [identity](#input\_identity) | MSI information | `map(any)` | n/a | yes |
| <a name="input_identity_type"></a> [identity\_type](#input\_identity\_type) | n/a | `string` | `"UserAssigned"` | no |
| <a name="input_ip_address_type"></a> [ip\_address\_type](#input\_ip\_address\_type) | IP address type | `string` | `"Public"` | no |
| <a name="input_location"></a> [location](#input\_location) | The location/region where the resource is created. Changing this forces a new resource to be created | `string` | `"westus2"` | no |
| <a name="input_mem_allocation"></a> [mem\_allocation](#input\_mem\_allocation) | Memory allocation | `string` | `"1.0"` | no |
| <a name="input_msi_config"></a> [msi\_config](#input\_msi\_config) | n/a | `map` | `{}` | no |
| <a name="input_name"></a> [name](#input\_name) | Azure container group name | `string` | `null` | no |
| <a name="input_num_of_containers"></a> [num\_of\_containers](#input\_num\_of\_containers) | Number of containers | `number` | `1` | no |
| <a name="input_os_type"></a> [os\_type](#input\_os\_type) | OS type | `string` | `"Linux"` | no |
| <a name="input_prjid"></a> [prjid](#input\_prjid) | Name of the project/stack e.g: mystack, nifieks, demoaci. Should not be changed after running 'tf apply' | `string` | n/a | yes |
| <a name="input_resource_group"></a> [resource\_group](#input\_resource\_group) | Name of the resource group | `string` | `null` | no |
| <a name="input_resource_group_settings"></a> [resource\_group\_settings](#input\_resource\_group\_settings) | n/a | `map` | `{}` | no |
| <a name="input_restart_policy"></a> [restart\_policy](#input\_restart\_policy) | Restart policy | `string` | `"OnFailure"` | no |
| <a name="input_teamid"></a> [teamid](#input\_teamid) | Name of the team/group e.g. devops, dataengineering. Should not be changed after running 'tf apply' | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_container_group_fqdn"></a> [container\_group\_fqdn](#output\_container\_group\_fqdn) | The FQDN of the created container group |
| <a name="output_container_group_id"></a> [container\_group\_id](#output\_container\_group\_id) | The ID of the created container group |
| <a name="output_container_group_ip_address"></a> [container\_group\_ip\_address](#output\_container\_group\_ip\_address) | The IP address of the created container group |
<!-- END_TF_DOCS -->
