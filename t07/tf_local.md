Below are a few examples of Terraform main.tf files using null_resource, variable, local, count, and for_each which you can run locally to understand their behavior.

Example 1: Using variable, null_resource, and echo

main.tf
```bash
variable "message" {
    default = "Hello, Terraform!"
}

resource "null_resource" "hello" {
  provisioner "local-exec" {
    command = "echo ${var.message}"
  }
}
```
You can run this configuration with `terraform apply` and see the output of the echo command.

Example 2: Using local, null_resource, and echo

main.tf
```bash
locals {
  message = "Hello, again Terraform!"
}

resource "null_resource" "hello_local" {
  provisioner "local-exec" {
    command = "echo ${local.message}"
  }
}
```
Apply above configuration with `terraform apply` and see the message echoed.

Example 3: Using count, null_resource, and echo

main.tf
```bash
variable "message" {
    default = "Hello, multiple times from Terraform!"
}

resource "null_resource" "hello_count" {
  count = 3

  provisioner "local-exec" {
    command = "echo ${var.message} ${count.index}"
  }
}
```
This configuration will create 3 null resources. Run `terraform apply` and you will see three messages echoed each with a different index.

Example 4: Using for_each, null_resource, and echo

main.tf
```bash
variable "message_map" {
    default = {
        one = "First message"
        two = "Second message"
        three = "Third message"
    }
}

resource "null_resource" "hello_for_each" {
  for_each = var.message_map

  provisioner "local-exec" {
    command = "echo ${each.key} : ${each.value}"
  }
}
```
This configuration will create a null resource for each element in the map. Run `terraform apply` and you will see different messages for each element in the map.
