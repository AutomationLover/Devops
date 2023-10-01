You can use the Terraform AWS provider's data sources to fetch the information about the default VPC and its default security group. 

Here's an example of how you can do it:

```hcl
# Fetch the default VPC
data "aws_vpc" "default" {
  default = true
}

# Fetch the default security group
data "aws_security_group" "default" {
  vpc_id = data.aws_vpc.default.id
  name   = "default"
}

output "default_vpc_id" {
  description = "The ID of the default VPC"
  value       = data.aws_vpc.default.id
}

output "default_sg_id" {
  description = "The ID of the default security group"
  value       = data.aws_security_group.default.id
}
```

In this script, two data sources are used: `aws_vpc` and `aws_security_group`. The `aws_vpc` source has an argument `default` set to true to ensure that we're fetching the default VPC.

The `aws_security_group` data source is configured to fetch the default security group for the fetched VPC by specifying the VPC's ID for the `vpc_id` argument and "default" for the `name` argument.

Please note that the above script assumes that the access key, secret access key, and region are configured in your AWS CLI or through environment variables. If not, you will need to add a provider block to your script to configure AWS:

```hcl
provider "aws" {
  region  = "us-west-2"
  access_key = "my-access-key"
  secret_access_key = "my-secret-key"
}
```
