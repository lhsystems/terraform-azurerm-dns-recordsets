# Usage

<!--- BEGIN_TF_DOCS --->
This is a Fork as the original repo looked dead.

# Azure DNS Recordsets Module

This module manages DNS recordsets in a given Azure DNS zone. It is part of
[the `terraformdns` project](https://terraformdns.github.io/).

## Example Usage

```hcl
resource "azurerm_resource_group" "example" {
  name     = "terraformdns-example"
  location = "West US"
}

resource "azurerm_dns_zone" "example" {
  resource_group_name = azurerm_resource_group.example.name

  name = "example.com"
}

module "dns_records" {
  source = "terraformdns/dns-recordsets/azurerm"

  resource_group_name = azurerm_dns_zone.example.resource_group_name
  dns_zone_name       = azurerm_dns_zone.example.name
  recordsets = [
    {
      name    = "www"
      type    = "A"
      ttl     = 3600
      records = [
        "192.0.2.56",
      ]
    },
    {
      name    = ""
      type    = "MX"
      ttl     = 3600
      records = [
        "1 mail1",
        "5 mail2",
        "5 mail3",
      ]
    },
    {
      name    = ""
      type    = "TXT"
      ttl     = 3600
      records = [
        "\"v=spf1 ip4:192.0.2.3 include:backoff.${azurerm_dns_zone.example.name} -all\"",
      ]
    },
    {
      name    = "_sip._tcp"
      type    = "SRV"
      ttl     = 3600
      records = [
        "10 60 5060 sip1",
        "10 20 5060 sip2",
        "10 20 5060 sip3",
        "20  0 5060 sip4",
      ]
    },
  ]
}
```

## Compatibility

When using this module, always use a version constraint that constraints to at
least a single major version. Future major versions may have new or different
required arguments, and may use a different internal structure that could
cause recordsets to be removed and replaced by the next plan.

## Arguments

- `resource_group_name` is the name of the resource group that contains the
  DNS zone where the records will be added.
- `dns_zone_name` is the name of the DNS zone within the given resource group
  where the records will be added.
- `recordsets` is a list of DNS recordsets in the standard `terraformdns`
  recordset format.

This module requires the `azurerm` provider.

Due to current limitations of the Terraform language, recordsets in Azure DNS
are correlated to `recordsets` elements using the index into the `recordsets`
list. Adding or removing records from the list will therefore cause this
module to also update all records with indices greater than where the
addition or removal was made.

## Limitations

This module supports only the following DNS record types, due to limitations
of the underlying Terraform provider:

- `A` and `AAAA`
- `CNAME`
- `MX`
- `NS`
- `PTR`
- `SRV`
- `TXT`

## Changelog

* Braking Changes

  The Resource name is changed from `this` to `name` as it better reflects the output of the module.

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement_terraform) | >= 0.12.0 |
| <a name="requirement_azurerm"></a> [azurerm](#requirement_azurerm) | >= 1.7.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_azurerm"></a> [azurerm](#provider_azurerm) | >= 1.7.0 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [azurerm_dns_a_record.name](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dns_a_record) | resource |
| [azurerm_dns_aaaa_record.name](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dns_aaaa_record) | resource |
| [azurerm_dns_cname_record.name](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dns_cname_record) | resource |
| [azurerm_dns_mx_record.name](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dns_mx_record) | resource |
| [azurerm_dns_ns_record.name](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dns_ns_record) | resource |
| [azurerm_dns_ptr_record.name](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dns_ptr_record) | resource |
| [azurerm_dns_srv_record.name](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dns_srv_record) | resource |
| [azurerm_dns_txt_record.name](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dns_txt_record) | resource |

## Inputs

| Name | Description | Type |
|------|-------------|------|
| <a name="input_dns_zone_name"></a> [dns_zone_name](#input_dns_zone_name) | The name of the DNS zone within the given resource group where the records will be added. | `string` |
| <a name="input_recordsets"></a> [recordsets](#input_recordsets) | List of DNS record objects to manage, in the standard terraformdns structure. | <pre>list(object({<br>    name    = string<br>    type    = string<br>    ttl     = number<br>    records = list(string)<br>  }))</pre> |
| <a name="input_resource_group_name"></a> [resource_group_name](#input_resource_group_name) | The name of the resource group that contains the DNS zone where the records will be added. | `string` |

## Outputs

No outputs.

<!--- END_TF_DOCS --->

