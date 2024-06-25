---
title: How do I delete an ENI from a VPC?
date: 2024-06-24 17:22:47 +07:00
tags: [AWS]
description: This blog contains the steps to delete an unused ENI
---

An Elastic Network Interface (ENI)is a logical networking component in a VPC that represents a virtual network card. An ENI is associated a resource. When an ENI can't be deleted, it mainly indicates a resource is still using the ENI which is preventing the ENI from being deleted. In this case, the resource using the ENI can be identified and deleted if it's no longer required. Once the resource using the ENI is deleted, the ENI can then be deleted. A requester-managed network interface is a network interface that an AWS service creates in your VPC on your behalf. When the resource associated with a requester-managed network interface is deleted, the AWS service detaches the network interface and deletes it. If the service detached a network interface but didn't delete it, you can delete the detached network interface. The resource using the ENI can be identified using the following methods:

### Method 1

- Note down the ENI ID to identify the resource using the elastic network interface.
- Execute the following command via Cloudshell or AWS CLI for further information on the resource.

```
$ aws ec2 describe-network-interfaces --filters Name=network-interface-id,Values=<ENI_ID> --region <region_name>
```
***Note: Replace the ENI_ID and <region_name> from the aforementioned command with the ENI ID starting with eni-xxxxxxxxxxxxxxxxx and the region (for e.g. us-west-2) where the ENI is located***

- Review the Description to find which resource the elastic network interface is attached to.
- If the resource associated with the ENI is no longer needed, then first delete the resource and then attempt to delete the ENI.

### Method 2

1.    Open the Amazon EC2 console in the region of the ENI.

2.    In the navigation pane, choose Network Interfaces.

3.    Search for the ID of the elastic network interface that you're detaching or deleting.

4.    Select the elastic network interface, and then choose the Details tab.

5.    Important: Review the Description to find which resource the elastic network interface is attached to.

6.    If you're no longer using the corresponding AWS service, then first delete the service.

***Note: If the resource is a Lambda function, Lambda deletes the network interface automatically within 24 hours after deleting the function.***

### Related information:

- Requester-managed network interfaces - https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/requester-managed-eni.html  
