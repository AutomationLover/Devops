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

Some update
```
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
      region_var   = var.regions[pair[0]]
    }
  }
}


resource "null_resource" "example" {
  for_each = local.combined

  provisioner "local-exec" {
    command = "echo Creating resource for region: ${each.value.region}, service: ${each.value.service}, with latency: ${each.value.region_var.latency} and draft status: ${each.value.region_var.draft}"
  }
}

```
In the context of Terraform, `pair[0]` and `var.regions[pair[0]]` differently reference elements of a data structure. Here's an explanation:

- `pair[0]`: This syntax is used when you have a list of items, and you want to access an item in the list by its index. In Terraform, list indices start at 0, so `pair[0]` gives you the first item in the list named `pair`. 

- `var.regions[pair[0]]`: This syntax is used to access a map's values using its keys. Here, `var.regions` is a map, and `pair[0]` is being used as a key to this map. `var.regions[pair[0]]` will return the value that is associated with the key `pair[0]` in the `regions` map.

So, if `pair[0]` is 'region1', `var.regions[pair[0]]` will return the value associated with the key 'region1' in the `regions` map. This value is itself another map with keys like 'draft' and 'latency'. To access these inner values, you would use something like `var.regions[pair[0]].latency`, which will return the value associated with the 'latency' key in the map that is associated with the 'region1' key in the `regions` map.

