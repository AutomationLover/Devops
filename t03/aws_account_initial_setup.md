# initial setup

Using AWS root account for day-to-day interactions is not recommended due to the unrestricted access it has to all AWS services and resources. Here are the best practices to handle AWS accounts:

1. **Enable MFA for Root Account**: Multi-Factor Authentication (MFA) adds an extra layer of protection on your AWS account. It's highly recommended to enable MFA for your AWS root account.

2. **Create IAM Users**: Instead of directly using the root account, create Identity and Access Management (IAM) users under your AWS account. These IAM users can be assigned granular permissions based on their role within your organization.

3. **Use Groups to Assign Permissions**: Rather than assigning permissions to individual IAM users, it's better to create groups that reflect job functions (Admins, Developers, Testers etc.), define the relevant permissions for each group, and assign IAM users to those groups.

4. **Grant Least Privilege**: Only grant the permissions necessary to perform a task. If a user only needs to read specific data, don't give them write access.

5. **Use AWS Organizations for Multiple AWS Accounts**: If you are working with multiple AWS accounts, using AWS Organizations can help manage those accounts and consolidate billing.

6. **Rotate Security Credentials Regularly**: Regular rotation of security credentials such as access keys for IAM users minimizes the risk of keys being compromised.

7. **Enable AWS CloudTrail**: CloudTrail records AWS API calls for your account. The recorded information includes the identity of the API caller, the time of the API call, the source IP address of the API caller, and so on. This helps to trace any changes made to the AWS resources.

8. **Regularly Review Activity in Your AWS Account**: Regularly audit your AWS account with tools like AWS Trusted Advisor, AWS Config, and AWS CloudTrail to keep tabs on what actions users are taking and what resources are being used.

9. **Use Strong Unique Passwords**: Ensure that all IAM users are using strong, unique passwords to prevent unauthorized access.

10. **Use Temporary Security Credentials**: Temporary security credentials are preferable to long-term access keys. Temporary credentials can be configured to expire after a certain period of time.

Remember, securing your AWS account is a continuous process that involves regular review and applying the necessary updates and patches.



A step-by-step guide on how to set up IAM users and groups using the AWS Management Console:

**To Enable MFA for Root Account:**

1. Sign in to the AWS Management Console with your root account.
2. Open the IAM console at https://console.aws.amazon.com/iam/.
3. In the navigation pane, choose "Dashboard" and then under "Security Status" choose "Activate MFA on your root account".
4. Follow the prompts to complete the process.

**To Create IAM Users:**

1. Open the IAM console at https://console.aws.amazon.com/iam/.
2. In the navigation pane, choose "Users" then "Add User".
3. Type the user name for the new user. Check "Programmatic access" if the users need to access API, CLI, SDK, and other development tools. Check "AWS Management Console access" if the user needs to access AWS Management Console. Click "Next: Permissions".
4. You can add the user to a group (instructions for creating a group are below), copy permissions from an existing user, or attach policy directly. After setting the permissions, click "Next: Tags".
5. (Optional) Add metadata to the user by attaching tags as key-value pairs. Then, click "Next: Review".
6. Review the user and permissions, then click "Create user". 

**To Create IAM Groups and Assign Permissions:**

1. In the IAM console, in the navigation pane, choose "User groups" then "Create group".
2. Enter a name for the group. Then, click "Next Step".
3. In the Attach Policy page, select the policies that you want to attach to the group. For example, if the group is for administrators, you might select the "AdministratorAccess" policy. After selecting the policies, click "Next Step".
4. Review the group and its policies. If everything is correct, click "Create Group".
5. Now you can add users to this group by selecting the group, then choosing "Add Users to Group". 

**To Enable AWS CloudTrail:**

1. Open the CloudTrail console at https://console.aws.amazon.com/cloudtrail/.
2. Choose "Get Started Now" if CloudTrail is not enabled. If it is, choose "Dashboard", then "Create trail".
3. Follow the prompts to complete the setup.

Remember, it's good practice to only give users the access they need to do their jobs (principle of least privilege), regularly review and update permissions, and enable logging and monitoring services to keep track of activity in your AWS account. Always follow the AWS recommended best practices for managing access keys, including regular key rotation and immediate revocation of keys if they are exposed or no longer needed.



## How to check balance
Sure, here's how you can monitor your AWS usage:

**1. AWS Cost Explorer** 

AWS Cost Explorer is a tool that enables you to view and analyze your costs and usage over the past 12 months. 

To use AWS Cost Explorer, follow these steps:

1. Sign in to the AWS Management Console and open the AWS Billing and Cost Management console at https://console.aws.amazon.com/billing/.
2. In the navigation pane, choose “Cost Explorer”.
3. To start, you need to enable Cost Explorer. Once enabled, it might take 24 hours to get available data.

**2. AWS Budgets**

AWS Budgets help you plan service usage, costs, and instance reservations. You can create custom cost and usage budgets to track your AWS cost and usage. 

You can create an AWS Budget by following these steps:

1. Sign in to the AWS Management Console and open the AWS Budgets at https://console.aws.amazon.com/billing/home#/budgets.
2. Choose "Create budget".
3. Choose the type of budget you want to create (Cost budget, Usage budget, Reservation budget), and then configure your budget.

**3. AWS Trusted Advisor** 

Trusted Advisor provides you real time guidance to help you provision your resources following AWS best practices. 

To use AWS Trusted Advisor:

1. Sign in to AWS Management Console and open the AWS Trusted Advisor console at https://console.aws.amazon.com/trustedadvisor/.
2. Browse through the dashboard for Cost Optimization checks.

**4. AWS Cost and Usage Report**

The AWS Cost and Usage Report contains the most comprehensive set of AWS cost and usage data available, including additional metadata about AWS services, pricing, and reservations (e.g., Amazon EC2 Reserved Instances).

To enable AWS Cost and Usage Report:

1. Open the Billing and Cost Management console at https://console.aws.amazon.com/billing/.
2. In the navigation pane, choose "Cost & Usage Reports".
3. Choose "Create report".
4. Specify the required information, and choose "Next".
5. In the "S3 bucket" section, choose "configure", and specify the required information. Choose "Next".
6. Review the settings, and choose "Review and Complete".

Each of these tools can help you manage your AWS costs and keep track of your usage. Remember to regularly check these tools to ensure you're not exceeding your expected AWS costs.

**5. check your AWS account balance**
To check your AWS account balance, follow these steps:

1. Sign in to the AWS Management Console.
2. Open the Billing and Cost Management console at https://console.aws.amazon.com/billing/.
3. In the navigation pane, choose "Billing Dashboard".
4. Your account balance is displayed in the "Credits" section.

Please note that the balance displayed here includes only AWS promotional credits and not other forms of payment. For a detailed breakdown of your charges by service, you can check the "Cost Explorer" in the same Billing and Cost Management console.
