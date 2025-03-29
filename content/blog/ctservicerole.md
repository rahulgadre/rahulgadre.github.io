---
title: Creating an IAM service role to deploy Landing Zone  
date: 2025-03-24 08:42:27 +07:00
tags: [aws]
description: Creating an IAM service role to deploy Landing Zone 
---

AWS [Control Tower](https://docs.aws.amazon.com/controltower/latest/userguide/what-is-control-tower.html) is an AWS sevice that is used to set up and govern an AWS multi-account environment, following prescriptive best practices. AWS Control Tower integrates with other AWS Services such as AWS Organizations, Service Catalog, and IAM identity center to build a landing zone.

Landing Zone is a multi account environment that is based on security and compliance best practices. It holds organizational units (OUs), accounts, users, and other resources that you want to be subject to compliance regulation. A landing zone can scale to fit the needs of an enterprise of any size.

The error specifiying that the service role AWSControlTowerAdmin doesn't exist may be encountered when creating a landing zone in a new management account for Control Tower.  The error can be resolved by creating a file servicerole.json in CloudShell with the following content:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "controltower.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
Once the file is created in the CloudShell environment, execute the following command to create a service role for Control Tower via CLI:

```
aws iam create-role --role-name AWSControlTowerAdmin --path /service-role/ --assume-role-policy-document file://servicerole.json
```
Navigate to the IAM console and select Roles to review the service role AWSControlTowerAdmin. Ensure that a test EC2 instance is running and then attempt to create Landing Zone after creating the AWSControlTowerAdmin service role. 
