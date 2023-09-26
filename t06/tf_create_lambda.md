Here is some answer to https://github.com/JiangRenDevOps/DevOpsNotes/blob/master/WK6_Terraform/hands_on/4%20-%20Homework.md

## Step 1: Setting up your Terraform file

Firstly, you need to define your AWS provider. Create a file named `main.tf` and add the following code:

```hcl
provider "aws" {
  region = "us-west-2" # or your preferred AWS region
}
```

## Step 2: Creating an S3 Bucket

Next, you need to define a resource for your S3 bucket:

```hcl
resource "aws_s3_bucket" "b" {
  bucket = "bucket-name" # replace with your desired bucket name
  acl    = "private"
}
```

## Step 3: Uploading a ZIP file to the S3 Bucket

For this, you need to use a Terraform's local-exec provisioner that calls the AWS CLI.

```hcl
resource "null_resource" "upload_zip" {
  depends_on = [aws_s3_bucket.b]
  
  provisioner "local-exec" {
    command = "aws s3 cp ./lambda_function.zip s3://${aws_s3_bucket.b.id}/"
  }
}
```

You'll need to replace `./lambda_function.zip` with the path to your lambda function's ZIP file.

## Step 4: Creating Lambda Function

Use the `aws_lambda_function` resource. The S3 bucket and key parameters point to the uploaded ZIP file:

```hcl
resource "aws_lambda_function" "test_lambda" {
  function_name    = "test_lambda"
  filename         = "lambda_function_payload.zip"
  source_code_hash = filebase64sha256("lambda_function_payload.zip")
  role             = aws_iam_role.iam_for_lambda.arn
  handler          = "exports.test"
  runtime          = "nodejs12.x"

  source_code_hash = "${filebase64sha256("lambda_function_payload.zip")}"
  s3_bucket        = "${aws_s3_bucket.b.id}"
  s3_key           = "lambda_function.zip"

  depends_on = [null_resource.upload_zip]
}
```

Please note that a lambda function requires an IAM role with necessary permissions.

## Step 5: Defining IAM Role for Lambda

The following example creates an IAM role that Lambda function can assume.

```hcl
resource "aws_iam_role""iam_for_lambda" {
  name = "iam_for_lambda"
  
  assume_role_policy = <<EOF
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Action": "sts:AssumeRole",
        "Principal": {
          "Service": "lambda.amazonaws.com"
        },
        "Effect": "Allow",
        "Sid": ""
      }
    ]
  }
EOF
}
```

Please add necessary permissions to this role based on your Lambda function's requirements.

## Final Note:

Make sure to replace all the placeholders with your actual values. Also, ensure your AWS CLI is properly configured to use the same region as specified in the provider block.

Before applying the changes, use the command `terraform init` to initialize your Terraform configuration. After that, use `terraform plan` to see the changes that will be made. If everything looks good, use `terraform apply` to apply the changes.

