Here is some answer to https://github.com/JiangRenDevOps/DevOpsNotes/blob/master/WK6_Terraform/hands_on/4%20-%20Homework.md


**Please note:** Before starting, make sure you have installed Terraform and AWS CLI on your local machine. 

**Step 1:** Firstly, we will create the project structure. Your folder structure might look something like this:

```
/terraform-lambda-example
  /lambda-src
    - hello_world.py
  - main.tf
  - variables.tf
  - outputs.tf
  - lambda.tf
  - api_gateway.tf
  - s3.tf
```

**Step 2:** Create your Python Lambda function. In `lambda-src/hello_world.py`:

```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Hello, World!'
    }
```

**Step 3:** Create your Terraform configuration files.

**a.** Variables file `variables.tf`

```hcl
variable "region" {
  description = "AWS region"
  default     = "us-west-2"
}

variable "bucket_name" {
  description = "S3 bucket name"
  default = "terraform-lambda-bucket"
}

variable "lambda_function_name" {
  description = "Lambda function name"
  default     = "terraform_lambda_hello_world"
}
```

**b.** Lambda file `lambda.tf`

```hcl
data "archive_file" "lambda_zip" {
  type        = "zip"
  source_file = "${path.module}/lambda-src/hello_world.py"
  output_path = "${path.module}/lambda-src/hello_world.zip"
}

resource "aws_lambda_function" "lambda_function" {
  filename      = "lambda-src/hello_world.zip"
  function_name = var.lambda_function_name
  role          = aws_iam_role.lambda_exec_role.arn
  handler       = "hello_world.lambda_handler"

  source_code_hash = data.archive_file.lambda_zip.output_base64sha256

  runtime = "python3.7"
  
  depends_on = [
    aws_s3_bucket_object.lambda_zip
  ]
}
```

**c.** API Gateway file `api_gateway.tf`

```hcl
resource "aws_api_gateway_rest_api" "api" {
  name        = "HelloWorldAPI"
  description = "Hello World Rest API"
}

resource "aws_api_gateway_integration" "lambda" {
  rest_api_id = aws_api_gateway_rest_api.api.id
  resource_id = aws_api_gateway_resource.proxy.id
  http_method = aws_api_gateway_method.proxy.http_method

  integration_http_method = "POST"
  type                    = "AWS_PROXY"
  uri                     = aws_lambda_function.lambda_function.invoke_arn
}

resource "aws_api_gateway_deployment" "api_gateway_deployment" {
  rest_api_id = aws_api_gateway_rest_api.api.id
  stage_name  = "test"

  depends_on = [
    aws_api_gateway_integration.lambda,
  ]
}
```
**d.** S3 file `s3.tf`
```hcl
resource "aws_s3_bucket" "lambda_bucket" {
  bucket = var.bucket_name
  acl    = "private"

  versioning {
    enabled = true
  }
}

resource "aws_s3_bucket_object" "lambda_zip" {
  bucket = aws_s3_bucket.lambda_bucket.id
  key    = "lambda-src/hello_world.zip"
  source = "lambda-src/hello_world.zip"
  etag   = filemd5("lambda-src/hello_world.zip")
}
```
**Step 4:** Run your Terraform code
```bash# Initialize your Terraform workspace
terraform init

# Plan and review your changes
terraform plan

# Apply your changes
terraform apply
```


**Step 5:** Test your function via the API Gateway that was created. First, you need to get the URL of your API Gateway. You can output this in `outputs.tf` file:

```hcl
output "invoke_url" {
  value = "https://${aws_api_gateway_rest_api.api.id}.execute-api.${var.region}.amazonaws.com/test"
}
```
Run `terraform apply` to get the `invoke_url`.

Then, you can test the function using curl:

```bash
curl -X POST <Your API Gateway Invoke URL>
```

Replace `<Your API Gateway Invoke URL>` with your API Gateway URL.

This test should return: `{"statusCode":200, "body":"Hello, World!"}`.

**Note:** Please replace all the `<placeholders>` with your actual values. Make sure to replace the `region`, `bucket_name`, and `lambda_function_name` in the `variables.tf` file as per your requirements. Also, remember to zip your lambda function code and upload it to the S3 bucket.
