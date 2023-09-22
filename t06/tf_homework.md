**Terraform Locals Homework**

Objective: 
This assignment aims to help you understand the usage of Terraform locals and how to use the `null_resource` with the `local-exec` provisioner. 

Tasks:
1. Install Terraform on your local machine if you haven't done so already.

2. Create a new directory named `terraform-local` and navigate into it.

3. Create a new Terraform configuration file named `main.tf`.

4. In `main.tf`, define two variables: `regions` and `aws_services`. 

    - `regions`: a map variable that contains different regions with properties like 'draft' (boolean) and 'latency' (integer).
    - `aws_services`: a list variable that contains different AWS service names as strings.

5. Define a `locals` block that creates a combination of regions and services using the `setproduct` function. This will be a map where each key is a combination of a region and a service, and the value is another map with details about the region and the service.

6. Define a `null_resource` with a `local-exec` provisioner that uses the `locals` block you created earlier.
   
   - The `null_resource` should use the `for_each` argument to iterate over each item in the `locals` block.
   - The `local-exec` provisioner should echo a message containing the region, service, latency, and draft status.

7. Initialize your Terraform working directory by running `terraform init`.

8. Apply your Terraform configuration by running `terraform apply`.

   - Observe the echo messages for each combination of region and service.

9. Remember to destroy the resources when you're done testing by running `terraform destroy`.

Deliverables:
- Submit your `main.tf` file.
- Provide screenshots of the terminal output from running `terraform apply` and `terraform destroy`.

Note: 
- Be sure to understand how the `setproduct` function and `for_each` argument work.
- Familiarize yourself with the structure and usage of Terraform `locals`.
- Understand the purpose and usage of the `null_resource` and `local-exec` provisioner.
- This assignment does not require any cloud resources. All tasks are performed locally.



