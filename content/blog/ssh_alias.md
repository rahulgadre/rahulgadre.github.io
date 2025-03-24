---
title: SSH access into an EC2 instance using an alias
date: 2025-01-09 19:24:27 +07:00
tags: [aws]
description: Accessing an EC2 instance using SSH alias 
---

Accessing an EC2 instance via SSH using a long SSH command (a general format is shown below) can be time consuming and not efficient. 

```
ssh -i <path to the pem file> default_user@ec2_ip
```

A quick way to access an EC2 instance is by using an alias. This alias can be created in the ssh config file located in the home directory (/Users/<username>/.ssh) of a user. The following can be added in the config file located at /Users/<username>/.ssh under the section "start of CNAME and SSH alias configs".

```
Host ec2
    Hostname        <IP_DNS_of_EC2_instance>
    User	            <default_user_of_AMI>
    IdentityFile       "<path to the pem file>"
```

Once the config file is updated with the above configuration, an EC2 instance can be accessed using the command ```ssh ec2``` where ec2 is an alias created for SSH access.

#### Note: update the alias name, hostname, user, and identity file details as per your usecase.


