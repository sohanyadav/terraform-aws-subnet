<!-- This file was automatically generated by the `geine`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->

<p align="center"> <img src="https://user-images.githubusercontent.com/50652676/62349836-882fef80-b51e-11e9-99e3-7b974309c7e3.png" width="100" height="100"></p>


<h1 align="center">
    Terraform AWS Subnet
</h1>

<p align="center" style="font-size: 1.2rem;">
    Terraform module to create public, private and public-private subnet with network acl, route table, Elastic IP, nat gateway, flow log.
     </p>

<p align="center">

<a href="https://www.terraform.io">
  <img src="https://img.shields.io/badge/Terraform-v0.12-green" alt="Terraform">
</a>
<a href="LICENSE.md">
  <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="Licence">
</a>


</p>
<p align="center">

<a href='https://facebook.com/sharer/sharer.php?u=https://github.com/clouddrove/terraform-aws-subnet'>
  <img title="Share on Facebook" src="https://user-images.githubusercontent.com/50652676/62817743-4f64cb80-bb59-11e9-90c7-b057252ded50.png" />
</a>
<a href='https://www.linkedin.com/shareArticle?mini=true&title=Terraform+AWS+Subnet&url=https://github.com/clouddrove/terraform-aws-subnet'>
  <img title="Share on LinkedIn" src="https://user-images.githubusercontent.com/50652676/62817742-4e339e80-bb59-11e9-87b9-a1f68cae1049.png" />
</a>
<a href='https://twitter.com/intent/tweet/?text=Terraform+AWS+Subnet&url=https://github.com/clouddrove/terraform-aws-subnet'>
  <img title="Share on Twitter" src="https://user-images.githubusercontent.com/50652676/62817740-4c69db00-bb59-11e9-8a79-3580fbbf6d5c.png" />
</a>

</p>
<hr>


We eat, drink, sleep and most importantly love **DevOps**. We are working towards stratergies for standardizing architecture while ensuring security for the infrastructure. We are strong believer of the philosophy <b>Bigger problems are always solved by breaking them into smaller manageable problems</b>. Resonating with microservices architecture, it is considered best-practice to run database, cluster, storage in smaller <b>connected yet manageable pieces</b> within the infrastructure.

This module is basically combination of [Terraform open source](https://www.terraform.io/) and includes automatation tests and examples. It also helps to create and improve your infrastructure with minimalistic code instead of maintaining the whole infrastructure code yourself.

We have [*fifty plus terraform modules*][terraform_modules]. A few of them are comepleted and are available for open source usage while a few others are in progress.




## Prerequisites

This module has a few dependencies:

- [Terraform 0.12](https://learn.hashicorp.com/terraform/getting-started/install.html)
- [Go](https://golang.org/doc/install)
- [github.com/stretchr/testify/assert](https://github.com/stretchr/testify)
- [github.com/gruntwork-io/terratest/modules/terraform](https://github.com/gruntwork-io/terratest)






## Examples

**IMPORTANT:** Since the `master` branch used in `source` varies based on new modifications, we suggest that you use the release versions [here](https://github.com/clouddrove/terraform-aws-subnet/releases).


Here are some examples of how you can use this module in your inventory structure:
### Private Subnet
```hcl
  module "subnets" {
    source              = "git::https://github.com/clouddrove/terraform-aws-subnet.git"
    name                = "subnets"
    application         = "clouddrove"
    environment         = "test"
    label_order         = ["application", "environment", "name"]
    availability_zones  = ["eu-west-1a", "eu-west-1b", "eu-west-1c"]
    vpc_id              = "vpc-xxxxxxxxx"
    type                = "private"
    nat_gateway_enabled = true
    cidr_block          = "10.0.0.0/16"
    public_subnet_ids   = ["subnet-XXXXXXXXXXXXX", "subnet-XXXXXXXXXXXXX"]
  }
```

### Public-Private Subnet
```hcl
  module "subnets" {
    source              = "git::https://github.com/clouddrove/terraform-aws-subnet.git"
    name                = "subnets"
    application         = "clouddrove"
    environment         = "test"
    label_order         = ["application", "environment", "name"]
    availability_zones  = ["eu-west-1a", "eu-west-1b", "eu-west-1c"]
    vpc_id              = "vpc-xxxxxxxxx"
    type                = "public-private"
    igw_id              = "ig-xxxxxxxxx"
    nat_gateway_enabled = true
    cidr_block          = "10.0.0.0/16"
  }
```

### Public Subnet
```hcl
  module "subnets" {
    source              = "git::https://github.com/clouddrove/terraform-aws-subnet.git"
    name                = "subnets"
    application         = "clouddrove"
    environment         = "test"
    label_order         = ["application", "environment", "name"]
    availability_zones  = ["us-east-1a", "us-east-1b", "us-east-1c"]
    vpc_id              = "vpc-xxxxxxxxx"
    type                = "public"
    igw_id              = "ig-xxxxxxxxx"
    cidr_block          = "10.0.0.0/16"
  }
```



## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| application | Application (e.g. `cd` or `clouddrove`). | string | `` | no |
| attributes | Additional attributes (e.g. `1`). | list | `<list>` | no |
| availability_zones | List of Availability Zones (e.g. `['us-east-1a', 'us-east-1b', 'us-east-1c']`). | list(string) | `<list>` | no |
| az_ngw_count | Count of items in the `az_ngw_ids` map. Needs to be explicitly provided since Terraform currently can't use dynamic count on computed resources from different modules. https://github.com/hashicorp/terraform/issues/10857. | number | `0` | no |
| az_ngw_ids | Only for private subnets. Map of AZ names to NAT Gateway IDs that are used as default routes when creating private subnets. | map(string) | `<map>` | no |
| cidr_block | Base CIDR block which is divided into subnet CIDR blocks (e.g. `10.0.0.0/16`). | string | - | yes |
| delimiter | Delimiter to be used between `organization`, `environment`, `name` and `attributes`. | string | `-` | no |
| enable_acl | Set to false to prevent the module from creating any resources. | bool | `true` | no |
| enable_flow_log | Enable subnet_flow_log logs. | bool | `false` | no |
| enabled | Set to false to prevent the module from creating any resources. | bool | `true` | no |
| environment | Environment (e.g. `prod`, `dev`, `staging`). | string | `` | no |
| igw_id | Internet Gateway ID that is used as a default route when creating public subnets (e.g. `igw-9c26a123`). | string | `` | no |
| label_order | Label order, e.g. `name`,`application`. | list | `<list>` | no |
| map_public_ip_on_launch | Specify true to indicate that instances launched into the subnet should be assigned a public IP address. | bool | `true` | no |
| max_subnets | Maximum number of subnets that can be created. The variable is used for CIDR blocks calculation. | number | `6` | no |
| name | Name  (e.g. `app` or `cluster`). | string | `` | no |
| nat_gateway_enabled | Flag to enable/disable NAT Gateways creation in public subnets. | bool | `false` | no |
| private_network_acl_id | Network ACL ID that is added to the private subnets. If empty, a new ACL will be created. | string | `` | no |
| public_network_acl_id | Network ACL ID that is added to the public subnets. If empty, a new ACL will be created. | string | `` | no |
| public_subnet_ids | A list of public subnet ids. | list(string) | `<list>` | no |
| s3_bucket_arn | S3 ARN for vpc logs. | string | `` | no |
| tags | Additional tags (e.g. map(`BusinessUnit`,`XYZ`). | map | `<map>` | no |
| traffic_type | Type of traffic to capture. Valid values: ACCEPT,REJECT, ALL. | string | `ALL` | no |
| type | Type of subnets to create (`private` or `public`). | string | `` | no |
| vpc_id | VPC ID. | string | - | yes |

## Outputs

| Name | Description |
|------|-------------|
| private_route_tables_id | The ID of the routing table. |
| private_subnet_cidrs | CIDR blocks of the created private subnets. |
| private_subnet_id | The ID of the private subnet. |
| private_tags | A mapping of private tags to assign to the resource. |
| public_route_tables_id | The ID of the routing table. |
| public_subnet_cidrs | CIDR blocks of the created public subnets. |
| public_subnet_id | The ID of the subnet. |
| public_tags | A mapping of public tags to assign to the resource. |



## Testing

In this module testing is performed with [terratest](https://github.com/gruntwork-io/terratest) and it creates a small piece of infrastructure, matches the output like ARN, ID and Tags name etc and destroy infrastructure in your AWS account. This testing is written in GO, so you need a [GO environment](https://golang.org/doc/install) in your system.

You need to run the following command in the testing folder:
```hcl
  go test -run Test
```



## Feedback
If you come accross a bug or have any feedback, please log it in our [issue tracker](https://github.com/clouddrove/terraform-aws-subnet/issues), or feel free to drop us an email at [hello@clouddrove.com](mailto:hello@clouddrove.com).

If you have found it worth your time, go ahead and give us a ★ on [our GitHub](https://github.com/clouddrove/terraform-aws-subnet)!

## About us

At [CloudDrove][website], we offer expert guidance, implementation support and services to help organisations accelerate their journey to the cloud. Our services include docker and container orchestration, cloud migration and adoption, infrastructure automation, application modernisation and remediation, and performance engineering.

<p align="center">We are <b> The Cloud Experts!</b></p>
<hr />
<p align="center">We ❤️  <a href="https://github.com/clouddrove">Open Source</a> and you can check out <a href="https://github.com/clouddrove">our other modules</a> to get help with your new Cloud ideas.</p>

  [website]: https://clouddrove.com
  [github]: https://github.com/clouddrove
  [linkedin]: https://cpco.io/linkedin
  [twitter]: https://twitter.com/clouddrove/
  [email]: https://clouddrove.com/contact-us.html
  [terraform_modules]: https://github.com/clouddrove?utf8=%E2%9C%93&q=terraform-&type=&language=
