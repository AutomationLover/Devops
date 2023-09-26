Here is your step-by-step guide in Markdown format:

```markdown
# Setting up AWS CLI and Creating a CLI Account in AWS on Ubuntu

## Prerequisites:
- Ubuntu 16.04 or higher
- Basic understanding of the Linux command line interface

## Step 1: Installing AWS CLI

1. Update your Ubuntu package list.

```bash
sudo apt-get update
```

2. Install the AWS CLI using `apt-get`.

```bash
sudo apt-get install awscli
```

3. Check the AWS CLI version to confirm that it was installed correctly.

```bash
aws --version
```

## Step 2: Creating a CLI User in AWS

1. Sign into the AWS Management Console.

2. Navigate to the IAM service.

3. Select "Users" from the navigation menu and click "Add User".

4. Enter a name for the user, select "Programmatic Access" as the access type, and click "Next".

5. Select "Attach existing policies directly" and choose the policy that matches the permissions you want to grant to the user. For full access, select "AdministratorAccess".

6. Click "Next: Tags" to add metadata to the user (optional), then "Next: Review" to review the ```markdown
details.

7. Click "Create user" to finalize the creation. 

8. On the final screen, you'll see your access key ID and secret access key. Save these in a secure place. You won't be able to see these again.

**Note:** Never share your secret access key. Anyone who has your access key has the same level of access to your AWS resources as you do.

## Step 3: Configuring AWS CLI

1. Open your terminal and run the following command to start the configuration process:

```bash
aws configure
```
2. Enter your AWS Access Key ID and Secret Access Key that you saved from Step 2.

3. Specify `json` as the default output format (or another if preferred).

4. Specify your preferred AWS region. You can find region codes in the [AWS Documentation](https://docs.aws.amazon.com/general/latest/gr/rande.html).

```bash
AWS Access Key ID [****************]: YOUR_ACCESS_KEY
AWS Secret Access Key [****************]: YOUR_SECRET_KEY
Default region name [us-east-1]: YOUR_PREFERRED_REGION
Default output format [json]: json
```

Congratulations! You have now set up AWS CLI and created a CLI account in AWS. You can now ```markdown
use AWS services from your Ubuntu terminal.

## Step 4: Test Your Configuration

To confirm that your AWS CLI is configured correctly, you can try running a command using the AWS CLI.

For example, to list all of your S3 buckets, you can use the following command:

```bash
aws s3 ls
```

If your setup is correct, you'll see a list of your S3 buckets. If you haven't created any, the output will be empty.

**Note:** The AWS CLI sends requests to AWS services on your behalf, so you will be charged for any services that the AWS CLI command line tools interact with, such as Amazon EC2 or Amazon S3.
```
Remember to replace `YOUR_ACCESS_KEY`, `YOUR_SECRET_KEY`, and `YOUR_PREFERRED_REGION` with your actual access key, secret key, and region. 

