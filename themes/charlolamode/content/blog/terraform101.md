---
title: Terraform 101 
date: 2024-01-09T12:12:25.364Z
topic: Terraform
description: Understanding the basics of Terraform
---

Terraform is a widely used infrastructure management tool in the world of DevOps. 
It offers several benefits including its idempotent, declarative, and cloud agnostic. 
A cloud infrastructure can be managed using Terraform scripts to avoid human errors and to follow security best practices. 
It takes away the need of deploying Cloud architectures from the management consoles. In this blog, we will review the basics of Terraform:- 

### Terraform Commands:

- Terraform init - this command initializes a working directory containing configuration files and installs plugins for required providers.
- Terraform validate - this command validates the configuration files in a directory.
- Terraform plan - this command creates an execution plan and lets you validate the Terraform configuration.
- Terraform apply - this  command executes the actions proposed in a Terraform plan to create, update, or destroy infrastructure.
- Terraform destroy - this command is a convenient way to destroy all remote objects managed by a particular Terraform configuration.

Note: 

- The -auto-approve is used in the apply command to skip the yes word while doing terraform apply.
- Sensitive - In the script, if sensitive is set to true means the data won’t be outputted on CLI or terminal but will be saved in the state file. 

Apart from the above commands, there are several Terraform components which get used in Terraform scripts. 

### Terraform Components:

- Provider - A provider is responsible for understanding API interactions and exposing resources.
- Resource - A resource that needs to be launched.Variables - Define variables in other files and call them in the main.tf
- Variables - Define variables in other files and call them in the main.tf
- Provisioner - Provisioners can be used to model specific actions on the local machine or on a remote machine to prepare servers or other infrastructure objects for service.
- Modules - A module is a simple directory that contains other .tf files. Using modules we can make the code reusable. Modules are local or remote.

### Configuration Files used in Terraform:

- main.tf - this is a main terraform configuration file which is used to create resources, call modules, locals, and data sources. The name can be anything and doesn't have to be main.tf.
- variables.tf - this file is used to define variables. The variables defined in this file can then be called from main.tf. Only the variable blocks are required for different variables and the variables values can be defined in the terraform.vars file.
- outputs.tf - the outputs.tf file contains the outputs blocks where the outputs which want to be displayed are defined.
- terraform.vars - the file terraform.vars contains the values assigned to variables which are defined in the variables.tf file.
- versions.tf - this file is used to define the version requirements for terraform and providers.

###  Important tidbits related to Terraform:
-  .terraform.lock.hcl - specifies the exact provider versions used so that you can update the providers used for your project.
- Local values - using the same word as a variable at different places then use Locals
	- Can’t pass anything into local and needs to be defined in the main block.
	- Can be constant or referenced values.
	- Use - ${local.setup_name} or local.setup_name.
- Outputs - used to find what data is returned.can also display it in the terminal. 
- Data sources - lets you dynamically fetch data from APIs or other Terraform state backends.
- Resource provider meta arguments - The provider meta-argument specifies which provider configuration to use for a resource. It is used to change the behavior of resources.
- Local_exec - run commands locally where Terraform is installed.
- State - stores info about infra in a state file such as terraform.tfstate. 
	- Used to determine what changes to make to infra such as add or remove resources. 
	- It's a single source of truth and the file is refreshed automatically before any operation is applied to check for drift.
	- If  deleted, resources are orphaned and need to be deleted manually.
- Remote state - store state file at a remote place such as S3, TF enterprise instead of saving it locally.
- Import - Import resources not launched by Terraform to manage by Terraform ```terraform import aws_instance.importec2 i-123456789```
- Workspaces - By default all resources or work gets created in the default workspace.
	- Can create new workspaces - ```terraform workspaces add dev```
	- Switch to a different workspace - ```terraform workspaces select prod```
	- Creating a new workspace creates a new folder structure under terraform.tfstate.d.
	- Can have different vars for different workspaces: ```terraform apply -var-file dev.tfvars```
- Modules - Modules empowers the use of reusable config and private or public modules can be used.
	- Private modules in Terraform cloud.
	- The main dir where we run tf init uses the root module.
- Null resource - used to run a script after creating a resource.
	- If there is a trigger present, then the null resource gets executed just once.
	- If there is a change in trigger, the null resource will get executed.
