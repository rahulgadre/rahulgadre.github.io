---
title: Using Ansible playbook to install httpd
date: 2025-07-04T15:08:25.364Z
type: page
tags: ["AWS", "Linux", "Ansible"] 
description: This blog contains the steps to install httpd using Ansible playbook
---

Once Ansible is up and running on the master node, one of the first playbooks that can be written is to install httpd on a remote node. Here are a few prerequisites before getting started.

#### Prerequisites:

- Ensure that a new instance is running with Python 3.8 installed.
- Set up passwordless access between the Ansible master node and the remote server.
- Update the inventory file with the login credentials.

#### Ansible Playbook:

Here is a snippet of an Ansible playbook written to install httpd:

```
tasks:
  - name: Install httpd on a web server.
    dnf:
      name: httpd
      state: present    
 
  - name: Make sure httpd is started now and at boot.
    service:
      name: httpd
      state: started
      enabled: yes

  - name: Checking the status of httpd service.
    command: systemctl status httpd
    register: status_httpd

  - name: Displaying the status of httpd service.
    debug:
      var: status_httpd.stdout

```
The above playbook is made for an Ansible remote host deployed as a web server, and the host section of the playbook contains ```become: true```. The last 2 tasks check the status of the httpd service and store the output in a register named ```status_httpd```. The last task then displays the value stored in the register on the terminal.
 
The playbook can be run using the command: ```ansible-playbook installhttpd.yml```

#### Issue encountered:

Here is an issue that I encountered during the httpd installation process on a remote server:

- Ensure that the Ansible fact ```ansible_pkg_mgr``` doesn't return ```unknown``` using the command ```ansible -i inventory web -m setup | grep ansible_pkg_mgr```

***Note: ```web``` in the above command is an Ansible group in the inventory file which contains the login details of the web server.***

The first web server that I launched using the Amazon Linux 2 OS resulted in the Ansible fact ```ansible_pkg_mgr``` returning ```unknown```, which failed the yum or dnf module on the remote host. Next, a new web server based on Amazon Linux 2023 OS was launched, which resulted in the fact returning as ```"ansible_pkg_mgr": "dnf"```. Hence, the dnf module is used in the playbook to install httpd.
