---
title: Ansible Ad Hoc Commands
date: 2025-04-27 23:30:27 +07:00
tags: ["AWS", "Linux", "Ansible"]
description: Getting started with Ansible ad hoc commands
---

Retrieving system information from managed nodes is much easier with Ansible ad hoc commands. Although Ansible ad hoc commands are not ideal for long-term maintenance and do not scale well, they are useful for quickly gathering system information from remote servers. This blog post demonstrates a few common Ansible ad hoc commands:

| Command      									| Usage 	  									  		| 
| :---        									|    :----:   									  		| 
| `ansible -i /path/to/inventory/file nodes -m ping`      				    | Ping remote servers using the Ansible ping module     |
| `ansible -i /path/to/inventory/file nodes -m command -a 'uname -r'`        | Get kernel version information                        |
| `ansible -i /path/to/inventory/file nodes -a "free -h"`                    | Check memory usage on remote nodes				        |
| `ansible -i /path/to/inventory/file nodes -a "df -h"`                      | Retrieve available disk space details 			        |
| `ansible -i /path/to/inventory/file nodes -a "date"`                       | Get date and time information from remote nodes 	    |

 **Note:** In the commands above, `nodes` refers to a group defined in the inventory file that contains a list of IP addresses or DNS names of remote nodes along with login credentials. If no Ansible module is specified in a command, Ansible defaults to using the `command` module.
