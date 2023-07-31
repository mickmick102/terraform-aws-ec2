<!-- This file was automatically generated by the `geine`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->

<p align="center"> <img src="https://user-images.githubusercontent.com/50652676/62349836-882fef80-b51e-11e9-99e3-7b974309c7e3.png" width="100" height="100"></p>


<h1 align="center">
    Terraform AWS EC2
</h1>

<p align="center" style="font-size: 1.2rem;"> 
    Terraform module to create an EC2 resource on AWS with ElasticC IP Addresses and Elastic Block Store.
     </p>

<p align="center">

<a href="https://www.terraform.io">
  <img src="https://img.shields.io/badge/Terraform-v1.1.7-green" alt="Terraform">
</a>
<a href="LICENSE.md">
  <img src="https://img.shields.io/badge/License-APACHE-blue.svg" alt="Licence">
</a>
<a href="https://github.com/clouddrove/terraform-aws-ec2/actions/workflows/tfsec.yml">
  <img src="https://github.com/clouddrove/terraform-aws-ec2/actions/workflows/tfsec.yml/badge.svg" alt="tfsec">
</a>
<a href="https://github.com/clouddrove/terraform-aws-ec2/actions/workflows/terraform.yml">
  <img src="https://github.com/clouddrove/terraform-aws-ec2/actions/workflows/terraform.yml/badge.svg" alt="static-checks">
</a>


</p>
<p align="center">

<a href='https://facebook.com/sharer/sharer.php?u=https://github.com/clouddrove/terraform-aws-ec2'>
  <img title="Share on Facebook" src="https://user-images.githubusercontent.com/50652676/62817743-4f64cb80-bb59-11e9-90c7-b057252ded50.png" />
</a>
<a href='https://www.linkedin.com/shareArticle?mini=true&title=Terraform+AWS+EC2&url=https://github.com/clouddrove/terraform-aws-ec2'>
  <img title="Share on LinkedIn" src="https://user-images.githubusercontent.com/50652676/62817742-4e339e80-bb59-11e9-87b9-a1f68cae1049.png" />
</a>
<a href='https://twitter.com/intent/tweet/?text=Terraform+AWS+EC2&url=https://github.com/clouddrove/terraform-aws-ec2'>
  <img title="Share on Twitter" src="https://user-images.githubusercontent.com/50652676/62817740-4c69db00-bb59-11e9-8a79-3580fbbf6d5c.png" />
</a>

</p>
<hr>


We eat, drink, sleep and most importantly love **DevOps**. We are working towards strategies for standardizing architecture while ensuring security for the infrastructure. We are strong believer of the philosophy <b>Bigger problems are always solved by breaking them into smaller manageable problems</b>. Resonating with microservices architecture, it is considered best-practice to run database, cluster, storage in smaller <b>connected yet manageable pieces</b> within the infrastructure. 

This module is basically combination of [Terraform open source](https://www.terraform.io/) and includes automatation tests and examples. It also helps to create and improve your infrastructure with minimalistic code instead of maintaining the whole infrastructure code yourself.

We have [*fifty plus terraform modules*][terraform_modules]. A few of them are comepleted and are available for open source usage while a few others are in progress.




## Prerequisites

This module has a few dependencies: 






## Examples


**IMPORTANT:** Since the `master` branch used in `source` varies based on new modifications, we suggest that you use the release versions [here](https://github.com/clouddrove/terraform-aws-ec2/releases).


Here is examples of how you can use this module in your inventory structure:
### Basic Example
```hcl
    module "ec2" {
    source      = "clouddrove/ec2/aws"
    version     = "1.3.1"

    name        = "ec2"
    environment = "test"
    label_order = ["name", "environment"]

    ## security-group
    vpc_id            = module.vpc.vpc_id
    ssh_allowed_ip    = ["0.0.0.0/0"]
    ssh_allowed_ports = [22]
  
    #instance
    instance_count = 1
    ami            = "ami-08d658f84a6d84a80"
    instance_type  = "c4.xlarge"
  
    #Networking
    subnet_ids = tolist(module.public_subnets.public_subnet_id)
  
    #Keypair
    public_key = "ssh-rsa ArJh5/gxz7sbSSseLd+ldHEOM3+lajUSGqWk3Bw/NgygEf1Kgw7gyK3jsTVVcokhK3TDuR3pi4u2QDR2tvLXddPKd37a2S7rjeqecw+XRW9559zKaR7RJJfjO1u1Onc2tgA3y0btdju2bcYBtFkRVOLwpog8CvslYEDV1Vf9HNeh9A3yOS6Pkjq6gDMrsUVF89ps3zuLmdVBIlCOnJDkwHK71lKihGKdkeXEtAj0aOQzAJsIpDFXz7vob9OiA/fb2T3t4R1EwEsPEnYVczKMsqUyqa+EE36bItcZHQyCPVN7+bRJyJpPcrfrsAa4yMtiHUUiecPdL/6HYwGHxxl2UQR5NE4NR35NI86Q+q1kNOc5VctxxQOTHBwKHaGvKLk4c5gHXaEl8yyYL0wVkL00KYx3GCh1LvRdQ"
  
    #IAM
    iam_instance_profile = module.iam-role.name
  
    #Root Volume
    root_block_device = [
      {
        volume_type           = "gp2"
        volume_size           = 15
        delete_on_termination = true
      }
    ]
  
    #EBS Volume
    ebs_volume_enabled = false
    ebs_volume_type    = "gp2"
    ebs_volume_size    = 30
  
    #Tags
    instance_tags = { "snapshot" = true }

  }
```

### ebs_mount
```hcl
    module "ec2" {
    source        = "clouddrove/ec2/aws"
    version       = "1.3.1"

    name          = "ec2"
    environment   = "test"
    label_order   = ["name", "environment"]

    ## security group 
    vpc_id            = module.vpc.vpc_id
    ssh_allowed_ip    = ["0.0.0.0/0"]
    ssh_allowed_ports = [22]
  
    #Instance
    instance_count = 1
    ami            = "ami-08d658f84a6d84a80"
    instance_type  = "t2.nano"
  
    #Keypair
    public_key = "HEOM3+lajUSGqWk3Bw/NgygEf1Kgw7gyK3jsTVVcokhK3TDuR3pi4u2QDR2tvLXddPKd37a2S7rjeqecw+XRW9559zKaR7RJJfjO1u1Onc2tgA3y0btdju2bcYBtFkRVOLwpog8CvslYEDLmdVBIlCOnJDkwHK71lKihGKdkeXEtAj0aOQzAJsIpDFXz7vob9OiA/fb2T3t4R1EwEsPEnYVczKMsqUyqa+EE36bItcZHQyCPVN7+bRJyJpPcrfrsAa4yMtiHUUiecPdL/6HYwGHxA5rUX3uD2UBm6sbGBH00ZCj6yUxl2UQR5NE4NR35NI86Q+q1kNOc5VctxxQOTHBwKHaGvKLk4c5gHXaEl8yyYL0wVkL00KYx3GCh1"
  
    #Networking
    subnet_ids = tolist(module.public_subnets.public_subnet_id)
  
    #IAM
    iam_instance_profile = module.iam-role.name
  
    #Root Volume
    root_block_device = [
      {
        volume_type           = "gp2"
        volume_size           = 15
        delete_on_termination = true
      }
    ]
  
    #EBS Volume
    ebs_volume_enabled = true
    ebs_volume_type    = "gp2"
    ebs_volume_size    = 30
  
    #Tags
    instance_tags = { "snapshot" = true }
  
    #Mount EBS With User Data
    user_data = file("user-data.sh")
  }
```

### spot-instance
```hcl
    module "spot-ec2" {
    source      = "clouddrove/ec2/aws"
    version     = "1.3.1"

    name        = "ec2"
    environment = "test"
    label_order = ["name", "environment"]
  
    ## security-group
    vpc_id            = module.vpc.vpc_id
    ssh_allowed_ip    = ["0.0.0.0/0"]
    ssh_allowed_ports = [22]
  
    #Keypair
    public_key = "h5/gxz7sbSSseLd+ldHEOM3+lajUSGqWk3Bw/NgygEf1Kgw7gyK3jsTVVcokhK3TDuR3pi4u2QDR2tvLXddPKd37a2S7rjeqecw+XRW9559zKaR7RJJfjO1u1Onc2tgA3y0btdju2bcYBtFkRVOLwpog8CvslYEDV1Vf9HNeh9A3yOS6Pkjq6gDMrsUVF89ps3zuLmdVBIlCOnJDkwHK71lKihGKdkeXEtAj0aOQzAJsIpDFXz7vob9OiA/fb2T3t4R1EwEsPEnYVczKMsqUyqa+EE36bItcZHQyCPVN7+bRJyJpPcrfrsAa4yMtiHUUiecPdL/6HYwGHxA5rUX3uD2UBm6sbGBHxQOTHBwKHaGvKLk4c5gHXaEl8yyYL0wVkL00KYx3GCh1LvRdQL8fvzImBCNg"
  
    # Spot-instance
    spot_price                          = "0.3"
    spot_wait_for_fulfillment           = true
    spot_type                           = "persistent"
    spot_instance_interruption_behavior = "terminate"
    spot_instance_enabled               = true
    spot_instance_count                 = 1
    spot_ami                            = "ami-08d658f84a6d84a80"
    instance_type                       = "c4.xlarge"
  
    #Networking
    subnet_ids = tolist(module.public_subnets.public_subnet_id)
  
    #IAM
    iam_instance_profile = module.iam-role.name
  
    #Root Volume
    root_block_device = [
    {
    volume_type           = "gp2"
    volume_size           = 15
    delete_on_termination = true
    }
    ]
  
    #EBS Volume
    ebs_volume_enabled = true
    ebs_volume_type    = "gp2"
    ebs_volume_size    = 30
  
    #Tags
    spot_instance_tags = { "snapshot" = true }
  
  }
```






## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| alias | The display name of the alias. The name must start with the word `alias` followed by a forward slash. | `string` | `"alias/ec2222"` | no |
| allowed\_ip | List of allowed ip. | `list(any)` | <pre>[<br>  "0.0.0.0/0"<br>]</pre> | no |
| allowed\_ports | List of allowed ingress ports | `list(any)` | <pre>[<br>  80,<br>  443<br>]</pre> | no |
| ami | The AMI to use for the instance. | `string` | `""` | no |
| assign\_eip\_address | Assign an Elastic IP address to the instance. | `bool` | `true` | no |
| associate\_public\_ip\_address | Associate a public IP address with the instance. | `bool` | `true` | no |
| attributes | Additional attributes (e.g. `1`). | `list(any)` | `[]` | no |
| availability\_zone | Availability Zone the instance is launched in. If not set, will be launched in the first AZ of the region. | `list(any)` | `[]` | no |
| cpu\_core\_count | Sets the number of CPU cores for an instance. | `string` | `null` | no |
| cpu\_credits | The credit option for CPU usage. Can be `standard` or `unlimited`. T3 instances are launched as unlimited by default. T2 instances are launched as standard by default. | `string` | `"standard"` | no |
| customer\_master\_key\_spec | Specifies whether the key contains a symmetric key or an asymmetric key pair and the encryption algorithms or signing algorithms that the key supports. Valid values: SYMMETRIC\_DEFAULT, RSA\_2048, RSA\_3072, RSA\_4096, ECC\_NIST\_P256, ECC\_NIST\_P384, ECC\_NIST\_P521, or ECC\_SECG\_P256K1. Defaults to SYMMETRIC\_DEFAULT. | `string` | `"SYMMETRIC_DEFAULT"` | no |
| deletion\_window\_in\_days | Duration in days after which the key is deleted after destruction of the resource. | `number` | `7` | no |
| delimiter | Delimiter to be used between `organization`, `environment`, `name` and `attributes`. | `string` | `"-"` | no |
| disable\_api\_termination | If true, enables EC2 Instance Termination Protection. | `bool` | `false` | no |
| dns\_enabled | Flag to control the dns\_enable. | `bool` | `false` | no |
| dns\_zone\_id | The Zone ID of Route53. | `string` | `"Z1XJD7SSBKXLC1"` | no |
| ebs\_block\_device | Additional EBS block devices to attach to the instance. | `list(any)` | `[]` | no |
| ebs\_device\_name | Name of the EBS device to mount. | `list(string)` | <pre>[<br>  "/dev/xvdb",<br>  "/dev/xvdc",<br>  "/dev/xvdd",<br>  "/dev/xvde",<br>  "/dev/xvdf",<br>  "/dev/xvdg",<br>  "/dev/xvdh",<br>  "/dev/xvdi",<br>  "/dev/xvdj",<br>  "/dev/xvdk",<br>  "/dev/xvdl",<br>  "/dev/xvdm",<br>  "/dev/xvdn",<br>  "/dev/xvdo",<br>  "/dev/xvdp",<br>  "/dev/xvdq",<br>  "/dev/xvdr",<br>  "/dev/xvds",<br>  "/dev/xvdt",<br>  "/dev/xvdu",<br>  "/dev/xvdv",<br>  "/dev/xvdw",<br>  "/dev/xvdx",<br>  "/dev/xvdy",<br>  "/dev/xvdz"<br>]</pre> | no |
| ebs\_iops | Amount of provisioned IOPS. This must be set with a volume\_type of io1. | `number` | `0` | no |
| ebs\_optimized | If true, the launched EC2 instance will be EBS-optimized. | `bool` | `false` | no |
| ebs\_volume\_enabled | Flag to control the ebs creation. | `bool` | `false` | no |
| ebs\_volume\_size | Size of the EBS volume in gigabytes. | `number` | `30` | no |
| ebs\_volume\_type | The type of EBS volume. Can be standard, gp2 or io1. | `string` | `"gp2"` | no |
| egress\_rule | Enable to create egress rule | `bool` | `true` | no |
| enable\_key\_pair | A boolean flag to enable/disable key pair. | `bool` | `true` | no |
| enable\_key\_rotation | Specifies whether key rotation is enabled. | `string` | `true` | no |
| enable\_security\_group | Enable default Security Group with only Egress traffic allowed. | `bool` | `true` | no |
| environment | Environment (e.g. `prod`, `dev`, `staging`). | `string` | `""` | no |
| ephemeral\_block\_device | Customize Ephemeral (also known as Instance Store) volumes on the instance. | `list(any)` | `[]` | no |
| existing\_sg\_id | Provide existing security group id for updating existing rule | `string` | `null` | no |
| hibernation | hibernate an instance, Amazon EC2 signals the operating system to perform hibernation. | `bool` | `false` | no |
| host\_id | The Id of a dedicated host that the instance will be assigned to. Use when an instance is to be launched on a specific dedicated host. | `string` | `null` | no |
| hostname | DNS records to create. | `string` | `"ec2"` | no |
| iam\_instance\_profile | The IAM Instance Profile to launch the instance with. Specified as the name of the Instance Profile. | `string` | `""` | no |
| instance\_count | Number of instances to launch. | `number` | `0` | no |
| instance\_enabled | Flag to control the instance creation. | `bool` | `true` | no |
| instance\_initiated\_shutdown\_behavior | n/a | `string` | `"terminate"` | no |
| instance\_profile\_enabled | Flag to control the instance profile creation. | `bool` | `true` | no |
| instance\_tags | Instance tags. | `map(any)` | `{}` | no |
| instance\_type | The type of instance to start. Updates to this field will trigger a stop/start of the EC2 instance. | `string` | n/a | yes |
| ipv6\_address\_count | Number of IPv6 addresses to associate with the primary network interface. Amazon EC2 chooses the IPv6 addresses from the range of your subnet. | `number` | `null` | no |
| ipv6\_addresses | List of IPv6 addresses from the range of the subnet to associate with the primary network interface. | `list(any)` | `null` | no |
| is\_enabled | Specifies whether the key is enabled. | `bool` | `true` | no |
| is\_external | enable to udated existing security Group | `bool` | `false` | no |
| key\_name | The key name to use for the instance. | `string` | `""` | no |
| key\_path | Name  (e.g. `~/.ssh/id_rsa.pub`). | `string` | `""` | no |
| key\_usage | Specifies the intended use of the key. Defaults to ENCRYPT\_DECRYPT, and only symmetric encryption and decryption are supported. | `string` | `"ENCRYPT_DECRYPT"` | no |
| kms\_description | The description of the key as viewed in AWS console. | `string` | `"Parameter Store KMS master key"` | no |
| kms\_key\_enabled | Specifies whether the kms is enabled or disabled. | `bool` | `true` | no |
| kms\_key\_id | The ARN of the key that you wish to use if encrypting at rest. If not supplied, uses service managed encryption. Can be specified only if at\_rest\_encryption\_enabled = true. | `string` | `""` | no |
| kms\_multi\_region | Indicates whether the KMS key is a multi-Region (true) or regional (false) key. | `bool` | `false` | no |
| label\_order | Label order, e.g. `name`,`application`. | `list(any)` | `[]` | no |
| managedby | ManagedBy, eg 'CloudDrove'. | `string` | `"hello@clouddrove.com"` | no |
| metadata\_http\_endpoint\_enabled | Whether the metadata service is available. Valid values include enabled or disabled. Defaults to enabled. | `string` | `"enabled"` | no |
| metadata\_http\_put\_response\_hop\_limit | The desired HTTP PUT response hop limit (between 1 and 64) for instance metadata requests. | `number` | `2` | no |
| metadata\_http\_tokens\_required | Whether or not the metadata service requires session tokens, also referred to as Instance Metadata Service Version 2 (IMDSv2). Valid values include optional or required. Defaults to optional. | `string` | `"optional"` | no |
| monitoring | If true, the launched EC2 instance will have detailed monitoring enabled. (Available since v0.6.0). | `bool` | `false` | no |
| multi\_attach\_enabled | Specifies whether to enable Amazon EBS Multi-Attach. Multi-Attach is supported on io1 and io2 volumes. | `bool` | `false` | no |
| name | Name  (e.g. `app` or `cluster`). | `string` | `""` | no |
| network\_interface | Customize network interfaces to be attached at instance boot time | `list(map(string))` | `[]` | no |
| placement\_group | The Placement Group to start the instance in. | `string` | `""` | no |
| protocol | The protocol. If not icmp, tcp, udp, or all use the. | `string` | `"tcp"` | no |
| public\_key | Name  (e.g. `ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD3F6tyPEFEzV0LX3X8BsXdMsQ`). | `string` | `""` | no |
| repository | Terraform current module repo | `string` | `"https://github.com/clouddrove/terraform-aws-ec2"` | no |
| root\_block\_device | Customize details about the root block device of the instance. See Block Devices below for details. | `list(any)` | `[]` | no |
| sg\_description | The security group description. | `string` | `"Instance default security group (only egress access is allowed)."` | no |
| sg\_egress\_description | Description of the egress and ingress rule | `string` | `"Description of the rule."` | no |
| sg\_egress\_ipv6\_description | Description of the egress\_ipv6 rule | `string` | `"Description of the rule."` | no |
| sg\_ids | of the security group id. | `list(any)` | `[]` | no |
| sg\_ingress\_description | Description of the ingress rule | `string` | `"Description of the ingress rule use elasticache."` | no |
| source\_dest\_check | Controls if traffic is routed to the instance when the destination address does not match the instance. Used for NAT or VPNs. | `bool` | `true` | no |
| spot\_ami | The AMI to use for the instance. | `string` | `""` | no |
| spot\_block\_duration\_minutes | The required duration for the Spot instances, in minutes. This value must be a multiple of 60 (60, 120, 180, 240, 300, or 360) | `number` | `null` | no |
| spot\_instance\_count | Number of instances to launch. | `number` | `0` | no |
| spot\_instance\_enabled | Flag to control the instance creation. | `bool` | `true` | no |
| spot\_instance\_interruption\_behavior | Indicates Spot instance behavior when it is interrupted. Valid values are `terminate`, `stop`, or `hibernate` | `string` | `null` | no |
| spot\_instance\_tags | Instance tags. | `map(any)` | `{}` | no |
| spot\_launch\_group | A launch group is a group of spot instances that launch together and terminate together. If left empty instances are launched and terminated individually | `string` | `null` | no |
| spot\_price | The maximum price to request on the spot market. Defaults to on-demand price | `string` | `null` | no |
| spot\_type | If set to one-time, after the instance is terminated, the spot request will be closed. Default `persistent` | `string` | `null` | no |
| spot\_valid\_from | The start date and time of the request, in UTC RFC3339 format(for example, YYYY-MM-DDTHH:MM:SSZ) | `string` | `null` | no |
| spot\_valid\_until | The end date and time of the request, in UTC RFC3339 format(for example, YYYY-MM-DDTHH:MM:SSZ) | `string` | `null` | no |
| spot\_wait\_for\_fulfillment | If set, Terraform will wait for the Spot Request to be fulfilled, and will throw an error if the timeout of 10m is reached | `bool` | `false` | no |
| ssh\_allowed\_ip | List of allowed ip. | `list(any)` | `[]` | no |
| ssh\_allowed\_ports | List of allowed ingress ports | `list(any)` | `[]` | no |
| ssh\_protocol | The protocol. If not icmp, tcp, udp, or all use the. | `string` | `"tcp"` | no |
| ssh\_sg\_ingress\_description | Description of the ingress rule | `string` | `"Description of the ingress rule use elasticache."` | no |
| subnet | VPC Subnet ID the instance is launched in. | `string` | `null` | no |
| subnet\_ids | A list of VPC Subnet IDs to launch in. | `list(string)` | `[]` | no |
| tags | Additional tags (e.g. map(`BusinessUnit`,`XYZ`). | `map(any)` | `{}` | no |
| tenancy | The tenancy of the instance (if the instance is running in a VPC). An instance with a tenancy of dedicated runs on single-tenant hardware. The host tenancy is not supported for the import-instance command. | `string` | `"default"` | no |
| ttl | The TTL of the record to add to the DNS zone to complete certificate validation. | `string` | `"300"` | no |
| type | Type of DNS records to create. | `string` | `"CNAME"` | no |
| user\_data | (Optional) A string of the desired User Data for the ec2. | `string` | `""` | no |
| vpc\_id | The ID of the VPC that the instance security group belongs to. | `string` | `""` | no |
| vpc\_security\_group\_ids\_list | A list of security group IDs to associate with. | `list(string)` | `[]` | no |

## Outputs

| Name | Description |
|------|-------------|
| arn | The ARN of the instance. |
| az | The availability zone of the instance. |
| instance\_count | The count of instances. |
| instance\_id | The instance ID. |
| ipv6\_addresses | A list of assigned IPv6 addresses. |
| key\_name | The key name of the instance. |
| name | Name of SSH key. |
| placement\_group | The placement group of the instance. |
| private\_ip | Private IP of instance. |
| public\_ip | Public IP of instance (or EIP). |
| spot\_bid\_status | The current bid status of the Spot Instance Request |
| spot\_instance\_id | The instance ID. |
| subnet\_id | The EC2 subnet ID. |
| tags | The instance ID. |
| vpc\_security\_group\_ids | The associated security groups in non-default VPC. |




## Testing
In this module testing is performed with [terratest](https://github.com/gruntwork-io/terratest) and it creates a small piece of infrastructure, matches the output like ARN, ID and Tags name etc and destroy infrastructure in your AWS account. This testing is written in GO, so you need a [GO environment](https://golang.org/doc/install) in your system. 

You need to run the following command in the testing folder:
```hcl
  go test -run Test
```



## Feedback 
If you come accross a bug or have any feedback, please log it in our [issue tracker](https://github.com/clouddrove/terraform-aws-ec2/issues), or feel free to drop us an email at [hello@clouddrove.com](mailto:hello@clouddrove.com).

If you have found it worth your time, go ahead and give us a ★ on [our GitHub](https://github.com/clouddrove/terraform-aws-ec2)!

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
