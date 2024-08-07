---
title: Getting started with Kubernetes with Minikube
date: 2024-05-09T13:39:25.364Z
type: page
tags: ["Kubernetes"] 
description: This blog contains the steps to create a single node cluster using Minikube
---

Deploying a full Kubernetes cluster can be challenging and time consuming. A managed Kubernetes cluster can also take hours to setup and may start to get expensive fairly quickly as the cluster setup involves multiple services. 
Minikube helps overcome the aforementioned challenges. Setting up a single node Minikube cluster to start experimenting or learning Kubernetes is relatively easier than setting up an entire cluster with a managed service provider. This blog covers the steps of deploying Minikube on an EC2 instance.

#### Setting up a virtual machine 

As described in the [link](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download#Service) to a Minikube documentation, ensure that the VM has the following resources to successfully run Minikube.

  1. 2 CPUs or more.
  2. 2 GB of free memory.
  3. 20 GB of free disk space.
  4. Internet connection.
  5. Container or virtual machine manager, such as Docker.

Note: A t2.medium instance with an EBS volume of 20G should meet the compute and storage requirements.

#### Installing and Logging into Docker

Once an EC2 instance with the aforementioned requirements is up, install docker on the instance as per the flavor of the OS. Start the docker service after installation and confirm the status of the service using a command such as ```systemctl status docker```. Set Docker to start on system boot using the command ```systemctl enable docker```. 
After installation, log into Docker using the ```docker login``` command. Proceed to the next step after successful login.

#### Installing Minikube

As described in the aforementioned link, install Minikube using the following commands:

```
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
$ sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```
Start Minkube using the command ```minikube start --driver=docker```.

#### Installing kubectl

kubectl is a command line tool used to run commands against Kubernetes clusters. This tool can be installed using the following commands. Refer to the [link](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/) for further information on installing kubectl.

```
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

Verify that kubectl is succesfully installed using the command ```kubectl version```. 

#### Verifiying the setup

Verify if the Minikube setup is working successfully using the following command:
```
$ kubectl get pods -n kube-system
```
Note: A successful command output shows a few pods running in the cluster.

Lastly, verify that docker login is working and the docker service is in running start to successfully start Minikube after a reboot or stop/start of the EC2 instance.
