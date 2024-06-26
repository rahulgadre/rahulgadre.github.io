---
title: Using Amazon EC2 Instance Connect for SSH access to EC2 instances 
date: 2023-08-19T07:25:25.364Z
type: page
tags: ["AWS"] 
description: Connecting to EC2 instances via SSH using Amazon EC2 Instance Connect Endpoints
---

Managing and rotating SSH keys may be cumbersome and often leads to security risks. With the launch of EC2 Instance Connect endpoints, it is now easier to connect to EC2 instances via SSH without worrying about SSH keys. The following is required to successfully configure SSH access to EC2 instances via EC2 Instance Connect endpoints.

#### EC2 Instance Connect Endpoints:

- Create an EC2 instance connect endpoint in the VPC where the EC2 instances are located. More information about creating an EC2 instance connect endpoint can be gathered from the following AWS document link.

=> Create an EC2 Instance Connect Endpoint - https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/create-ec2-instance-connect-endpoints.html

#### AWS CLI:

- As described in the [prerequisites](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-using-eice.html), ensure that the latest version of AWS CLI is installed and configured. 

=> Installing or updating the latest version of the AWS CLI - https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

#### EC2 Instance Connect: 

- For the instances for which SSH access needs to be configured via EC2 Instance Connect endpoints, ensure that the instance has the EC2 Instance Connect package installed for successful connectivity. For instances launched from the Amazon Linux 2 or later or Ubuntu 20.04 or later AMIs, the required EC2 Instance Connect package is already installed. For the AMIs which don't come with the required package, it needs to be installed manually by referring to the steps described in the AWS document link shown below:

=> Install EC2 Instance Connect on your EC2 instances - https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-connect-set-up.html

#### Connecting to EC2 Instances: 

Once the above requirements are met, an EC2 instance can be accessed via SSH using the EC2 instance connect endpoints with the following command:
```
$ aws ec2-instance-connect ssh --instance-id i-1234567890example
```
#### Troubleshooting:

- Check if the user/role attempting to access an EC2 instance via EC2 instance connect endpoints has the required IAM permissions
- Ensure that the AWS CLI version is at least 2.12 to run the aws ec2-instance-connect ssh command
- Verify that the EC2 instance contains the required EC2 Instance Connect package for successful connectivity to the instance via EIC endpoints.
