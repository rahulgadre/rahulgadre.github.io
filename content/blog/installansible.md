---
title: Installing Ansible on Mac
date: 2025-05-09 13:30:27 +07:00
tags: ["AWS", "Linux", "Ansible"] 
description: Installing Ansible on Mac 
---

Installing Ansible on Mac can be a straightforward process. However, it requires a few tweaks once it is installed to get it working. The following steps assist with installing Ansible on Mac.

### Installation

1. Check if pip is installed using the `which pip` command, and install it using the `sudo yum install pip3` command.  
2. Once pip is installed, install Ansible using the `sudo pip3 install ansible` command.  
3. Ensure that Ansible is installed successfully using the `ansible --version` command.

### Installing boto3 dependency

1. Install boto3, which is a prerequisite, using the command `sudo pip3 install boto3`.

### Configure passwordless access between the master node and the managed nodes

As Ansible requires passwordless SSH login, refer to this [blog](https://rahulgadre.com/blog/pwdless/) to successfully set up passwordless access between the master node and the managed nodes.

### Testing Ansible

Once Ansible and its dependencies are installed, along with passwordless access between the master node and the managed nodes, Confirm that the master node can communicate successfully with the managed nodes by using the Ansible ping module with the command: ```ansible <managed_node_DNS_IP> -m ping.```

```
ansible <managed_node_DNS_IP> -m ping

<managed_node_DNS_IP> | SUCCESS => {
"changed": false,
"ping": "pong"
}
```

***Note that `<managed_node_DNS_IP>` refers to a managed node's IP address or DNS name.***
