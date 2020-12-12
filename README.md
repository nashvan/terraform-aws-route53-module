<!-- This file was automatically generated by the `geine`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->

<p align="center"> <img src="https://user-images.githubusercontent.com/50652676/62349836-882fef80-b51e-11e9-99e3-7b974309c7e3.png" width="100" height="100"></p>


<h1 align="center">
    Terraform AWS Route53
</h1>

<p align="center" style="font-size: 1.2rem;"> 
    Terraform module to create Route53 resource on AWS for zone and record set.
     </p>

<p align="center">

<a href="https://www.terraform.io">
  <img src="https://img.shields.io/badge/terraform-v0.13-green" alt="Terraform">
</a>
<a href="LICENSE.md">
  <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="Licence">
</a>


</p>
<p align="center">

<a href='https://facebook.com/sharer/sharer.php?u=https://github.com/nashvan/terraform-aws-route53-module'>
  <img title="Share on Facebook" src="https://user-images.githubusercontent.com/50652676/62817743-4f64cb80-bb59-11e9-90c7-b057252ded50.png" />
</a>
<a href='https://www.linkedin.com/shareArticle?mini=true&title=Terraform+AWS+Route53&url=https://github.com/nashvan/terraform-aws-route53-module'>
  <img title="Share on LinkedIn" src="https://user-images.githubusercontent.com/50652676/62817742-4e339e80-bb59-11e9-87b9-a1f68cae1049.png" />
</a>
<a href='https://twitter.com/intent/tweet/?text=Terraform+AWS+Route53&url=https://github.com/nashvan/terraform-aws-route53-module'>
  <img title="Share on Twitter" src="https://user-images.githubusercontent.com/50652676/62817740-4c69db00-bb59-11e9-8a79-3580fbbf6d5c.png" />
</a>

</p>
<hr>


**DevSecOps and Cloud Engineering** standardizing architecture while ensuring the architecture is security complaint.  

This module consits of [Terraform open source](https://www.terraform.io/) and includes automatation tests and examples. It also helps to create and improve your infrastructure with minimalistic code instead of repeating and maintaining the whole infrastructure code.


## Prerequisites

This module has a few dependencies: 

- [Terraform 0.13](https://learn.hashicorp.com/terraform/getting-started/install.html)
- [Go](https://golang.org/doc/install)
- [github.com/stretchr/testify/assert](https://github.com/stretchr/testify)
- [github.com/gruntwork-io/terratest/modules/terraform](https://github.com/gruntwork-io/terratest)


# Route53 Records

This module creates DNS records in Route53 zone.

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Requirements

| Name | Version |
|------|---------|
| terraform | >= 0.12.6 |
| aws | >= 2.49 |

## Providers

| Name | Version |
|------|---------|
| aws | >= 2.49 |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| records | List of maps of DNS records | `any` | `[]` | no |
| hosted_zone | Name of DNS zone | `string` | `null` | no |

## Outputs

| Name | Description |
|------|-------------|
| this\_route53\_record\_fqdn | FQDN built using the zone domain and name |
| this\_route53\_record\_name | The name of the record |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

## ## Authors

Module maintained by [Nashwan Mustafa](https://www.linkedin.com/in/nashwan-mustafa/).


## Examples

## 1. A record with alias
- this example will create an A record with Alias pointing to a resource name for example alb, s3, cloudfront, etc. 

```
module "service_dns_a_record" {
  source  = "nashvan/route53-module/aws"
  version = "1.2.0"

  hosted_zone = local.account_config["hosted_zone"]
  
  records = [
    {
      name  = "${var.environment_name}.${local.app_name}"
      type  = "A"
      alias = {
        name    = "abc-1830827336.xyz.elb.amazonaws.com"
        zone_id = "Z35SXDOTRQ7X7K"
      }
    }
  ]

}
```


## 2. A record with no alias
- this example will create an A record pointing to the IP address

```
module "service_dns" {
  source  = "nashvan/route53-module/aws"
  version = "1.2.0"

  hosted_zone = local.account_config["hosted_zone"]
  
  records = [
    {
      name  = "abc"
      type  = "A"
      ttl   = 3600
      records = ["10.10.10.10",]
    }
  ]

}

```


## 3. CNAME record
- this example will create a CNAME record (poiting an fqdn to another fqdn for example)

```
module "service_dns_cname" {
  source  = "nashvan/route53-module/aws"
  version = "1.2.0"

  hosted_zone = local.account_config["hosted_zone"]
  
  records = [
    {
      name    = "xyz"
      type    = "CNAME"
      ttl     = "5"
      records = ["develop.abc.test.info.",]
    }
  ]
}
```
// Will create the following record for you
// xyz.cmcloudlab912.info	CNAME	Simple	-	No	develop.abc.cmcloudlab912.info. 5

## 4. CNAME wighted routing policy 

```
module "service_dns_cname" {
  source  = "nashvan/route53-module/aws"
  version = "1.2.0"

  hosted_zone = local.account_config["hosted_zone"]
  
  records = [
    {
      name    = "test"
      type    = "CNAME"
      ttl     = "5"
      records = ["develop.abc.test.info.",]

      weighted_routing_policy = {
        weight         = 90
      }
      set_identifier = "develop"

    }
  ]
}
```
// Will create the following record for you
// test.test.info	CNAME	Weighted	90	No	develop.abc.test.info.


test.cmcloudlab912.info	CNAME	Simple	-	No	develop.abc.cmcloudlab912.info. 5


## About us

Experience in Security, DevOps, Cloud Archetecture, and infrastructure automation.

<p align="center"> <b> Kurdish by blood and values!</b></p>
<hr />
<p align="center">We ❤️  DevSecOps and Cloud Engineering </p>

[linkedin]: https://www.linkedin.com/in/nashwan-mustafa/

