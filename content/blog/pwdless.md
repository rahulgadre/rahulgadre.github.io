---
title: Setting Up Passwordless SSH Access for Ansible
date: 2025-06-01T05:25:25.364Z
type: page
tags: ["AWS", "Linux", "Ansible"] 
description: This blog provides steps for setting up passwordless SSH access for Ansible.
---

Ansible requires passwordless SSH login to function properly. Without this setup, even a simple ping command will result in an error. Follow these steps to configure passwordless SSH access between a server and client.

### Environment Setup

```
SSH server IP: 10.0.0.2 
SSH client IP: 10.0.0.3
```

### Generate public/private rsa key pair.

1. Log into the client machine and generate a key using the following command.

```
ssh-keygen -t rsa
```

2. After generating the key, locate the line that says "Your public key has been saved in" and open the indicated .pub file. Copy the entire contents of this file to your clipboard.

### Configure the server

1. Log into the server and navigate to the /home/user/.ssh directory (where "user" is your actual username on the server).

2. Create a file named authorized_keys and paste the contents from your clipboard.

3. Set the proper permissions:
    - Directory permissions: chmod 700 /home/user/.ssh
    - File permissions: chmod 600 /home/user/.ssh/authorized_keys

### Test passwordless login

1. Log into the client machine and attempt to connect via SSH:

```
ssh user@10.0.0.2
```

***Note: Replace "user" in the command above with the username that owns the .ssh directory containing the authorized_keys file on the server.*** 

You should now be able to access the server without entering a password, and the connection should succeed. This setup will allow Ansible to properly communicate with your managed nodes.
