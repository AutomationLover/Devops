The roles 'aws-elasticbeanstalk-ec2-role' and 'aws-elasticbeanstalk-service-role' are created when you launch an environment in AWS Elastic Beanstalk for the first time and choose to create default roles. If these roles are not present in your AWS account, it's likely because either:

1. You haven't launched an AWS Elastic Beanstalk environment in your account before, or
2. You may have manually deleted these roles, or
3. These roles were not created due to insufficient IAM permissions.

Here's how you can create them:

1. **aws-elasticbeanstalk-ec2-role**

    This is the instance profile role that gives permissions to your Elastic Beanstalk environment to access other AWS services.

    - Open the AWS IAM console.
    - In the navigation pane, click 'Roles' and then 'Create role'.
    - Choose 'EC2' service,  Click 'Next: Permissions'.
    - Attach the 'AWSElasticBeanstalkWebTier' and 'AWSElasticBeanstalkMulticontainerDocker' managed policies, and then click 'Next: Tags'.
    - (Optional) Add metadata to the role by attaching tags.
    - Click 'Next: Review'.
    - For 'Role name', enter 'aws-elasticbeanstalk-ec2-role'. Review the role and then click 'Create role'.

2. **aws-elasticbeanstalk-service-role**

    This is the service role that gives AWS Elastic Beanstalk the required permissions to call other AWS services on your behalf.

    - In the AWS IAM console, in the navigation pane, click 'Roles' and then 'Create role'.
    - Choose 'Elastic Beanstalk' service and 'Elastic Beanstalk Customizable' use case. Click 'Next: Permissions'.
    - Attach the 'AWSElasticBeanstalkService' managed policy, and then click 'Next: Tags'.
    - (Optional) Add metadata to the role by attaching tags.
    - Click 'Next: Review'.
    - For 'Role name', enter 'aws-elasticbeanstalk-service-role'. Review the role and then click 'Create role'.

