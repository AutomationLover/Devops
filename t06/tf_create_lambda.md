Here is some answer to https://github.com/JiangRenDevOps/DevOpsNotes/blob/master/WK6_Terraform/hands_on/4%20-%20Homework.md

### Step 1: Prerequisites
- AWS account setup with appropriate access and permissions.
- Local machine setup with Terraform and AWS CLI.

### Step 2: Create the Python Lambda Function

Create a file named `helloworld.py` with the following function:

```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Hello, World!'
    }
```

### Step 3: Package the Lambda Function

Zip the `helloworld.py` and name it `helloworld.zip`.

### Step 4: Terraform Files

#### `main.tf`:
The main.tf file is where all the resources are defined.

```hcl
provider "aws" {
  region  = "us-west-2" # use your preferred region
}

resource "aws_s3_bucket" "bucket" {
  bucket = "terraform-serverless-example" # your bucket name
  acl    = "private"
}

resource "aws_s3_bucket_object" "object" {
  bucket = aws_s3_bucket.bucket.bucket
  key    ="lambda_function_payload"
  source = "helloworld.zip" # Path to the ZIP file
  etag   = "${filemd5("helloworld.zip")}" # Hash of the file
}

module "lambda_function" {
  source = "./lambda"
  s3_bucket = aws_s3_bucket.bucket.id
  s3_key = aws_s3_bucket_object.object.id
}
```

#### `variables.tf`:

```hcl
variable "s3_bucket" {}

variable "s3_key" {}
```

#### `outputs.tf`:

```hcl
output "bucket_id" {
  description = "The name of the bucket"
  value       = aws_s3_bucket.bucket.id
}

output "object_id" {
  description = "The name of the object"
  value       = aws_s3_bucket_object.object.id
}
```

#### `./lambda/lambda.tf`:

```hcl
data "aws_iam_policy_document" "s3_policy" {
  statement {
    actions = ["s3:GetObject"]
    resources = [
      "arn:aws:s3:::${var.s3_bucket}/${var.s3_key}",
    ]
  }
}

resource "aws_iam_role" "iam_for_lambda"{
  name = "iam_for_lambda"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Principal = {
          Service = "lambda.amazonaws.com"
        }
        Effect = "Allow"
      },
    ]
  })

  inline_policy {
    name   = "s3_policy"
    policy = data.aws_iam_policy_document.s3_policy.json
  }
}

resource "aws_lambda_function" "example" {
  function_name = "serverless_example"

  s3_bucket = var.s3_bucket
  s3_key    = var.s3_key

  handler = "helloworld.lambda_handler"
  runtime = "python3.8"

  role = aws_iam_role.iam_for_lambda.arn
}

resource "aws_api_gateway_rest_api" "example" {
  name        = "serverless_example"
  description = "Hello world API Gateway"
}

resource "aws_api_gateway_resource" "example" {
  path_part   = "{proxy+}"
  parent_id   = aws_api_gateway_rest_api.example.root_resource_id
  rest_api_id = aws_api_gateway_rest_api.example.id
}

resource "aws_api_gateway_method" "example" {
  http_method   = "ANY"
  resource_id   = aws_api_gateway_resource.example.id
  rest_api_id   = aws_api_gateway_rest_api.example.id
  authorization = "NONE"
}

resource "aws_api_gateway_integration" "example" {
  http_method = aws_api_gateway_method.example.http_method
  resource_id = aws_api_gateway_resource.example.id
  rest_api_id = aws_api_gateway_rest_api.example.id
  type        = "AWS_PROXY"

  integration_http_method = "POST"
  uri                    = aws_lambda_function.example.invoke_arn
}

resource "aws_lambda_permission" "apigw" {
  statement_id  = "AllowExecutionFromAPIGateway"
  action        = "lambda:InvokeFunction"
  function_name = aws_lambda_function.example.function_name
  principal     = "apigateway.amazonaws.com"

  source_arn = "${aws_api_gateway_rest_api.example.execution_arn}/*/*"
}
```
Remember to replace placeholder values with your actual values.

### Step 5: Initialize and Deploy with Terraform

Run the following commands in your terminal:

```bash
$ terraform init
$ terraform apply
```

### Step 6: Test API GatewayTo test the API gateway, you can use the AWS Management Console, AWS CLI, or any HTTP client like CURL or Postman. 

To get the root URL of your deployed API, use the following Terraform output command:

```bash
terraform output
```

This will print the URL of your API Gateway to the console. You can call this URL with any HTTP method (GET, POST, PUT, DELETE, etc.) and it will trigger the Lambda function.

If you use CURL, the command would look like this:

```bash
curl -X POST https://<API-Gateway-URL>
```

### Step 2 auto with TF
you can use the `archive_file` data source in Terraform to perform the zip operation.

For example:

```hcl
data "archive_file" "lambda_zip" {
  type        = "zip"
  output_path = "helloworld.zip"
  source_dir  = "./path_to_your_python_files"
}

resource "aws_s3_bucket_object" "object" {
  bucket = aws_s3_bucket.bucket.bucket
  key    = "lambda_function_payload"
  source = data.archive_file.lambda_zip.output_path
  etag   = "${filemd5(data.archive_file.lambda_zip.output_path)}"
}
```

In this example, `archive_file` will zip the entire directory `./path_to_your_python_files` and output the zip file as `helloworld.zip`. This zip file can then be uploaded to S3 as part of the terraform apply.

Please replace `./path_to_your_python_files` with the actual path to your lambda function Python files. 

Remember to run `terraform apply` again after making these changes.
