BEFORE


```hcl
variable "regions" {
  type = map(any)
  default = {
    "region1" = {
      draft = false
      latency = 50
    },
    "region2" = {
      draft = true
      latency = 100
    }
  }
}

resource "null_resource" "example" {
  for_each = var.regions

  provisioner "local-exec" {
    command = "echo Creating resource for region: ${each.key} with latency ${each.value.latency} and draft status ${each.value.draft}"
  }
}
```



AFTER

```hcl
variable "regions" {
  description = "Regions map"
  type        = map(any)
  default     = {
    "region1" = {
      draft   = false
      latency = 50
    },
    "region2" = {
      draft   = true
      latency = 100
    }
  }
}

variable "aws_services" {
  description = "AWS Services"
  type        = list(string)
  default     = ["EC2", "S3"]
}

locals {
  combined = { for pair in setproduct(keys(var.regions), var.aws_services) :
    "${pair[0]}-${pair[1]}" => {
      region  = pair[0]
      service = pair[1]
      draft   = var.regions[pair[0]].draft
      latency = var.regions[pair[0]].latency
    }
  }
}


resource "null_resource" "example" {
  for_each = local.combined

  provisioner "local-exec" {
    command = "echo Creating resource for region: ${each.value.region}, service: ${each.value.service}, with latency: ${each.value.latency} and draft status: ${each.value.draft}"
  }
}
```
