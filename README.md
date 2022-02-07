# Overview
Using Ansible with the IBM Cloud

## Ansible 101 
**What is Ansible?**

 Ansible is an open source infrastructure automation tool initially operated by Redhat. Ansible enables you to tackle deployment, configuration, maintenance, etc. It allows you to quickly run a set of commands against a wide array of infrastructure, or push database configuration changes based on system hostname or OS. 

Ansible is agentless unlike a lot of other infrastructure automation tools, which means you don't actually need to install an agent on the hosts you want to maintain and manage. It uses SSH to communication with the hosts in your environment and is written in Python.  

Ansible has three major kind of uses: 

 - Provisioning: Deployment and provisioning of physical/virtual infrastructure either on-prem or in the cloud.
 - Configuration: System patches, package installations, routine maintenance. 
 - Code Deployment: Push our node.js or python applications directly to our infrastructure

**Similar Tools**

 - [Chef](https://www.chef.io/)  
 - [Puppet](https://puppet.com/)  
 - [Terraform](https://www.terraform.io/)  
 - [SaltStack](https://saltproject.io/)  
 - [Nomad](https://www.nomadproject.io/)

**Inventory**

In Ansible, an `inventory` is a set of hosts that Ansible can work against to run commands and playbooks. Once you've defined your inventory, you can use [patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#intro-patterns) to further refine the hosts or groups you want Ansible to run against.


**Playbooks**

Ansible Playbooks offer repeatable, re-usable tasks that can be run against your inventory. They define what actions need to be taken as well as the machines to target and variables or facts to use. 

**Variables/Facts**

*Variables*

*Facts*

**Roles**

**Ad-hoc commands**


## Prerequisites
In order to use the examples in this repository you will need to have the following pre-reqs completed:

 - IBM Cloud API Key. See this [guide]() if you need to generate one to use for this project. 
 - [Python 3]() installed. 
 - [Ansible] installed. The IBM collection requires Ansible `2.9.2+`
 
    ```shell
    $ pip install "ansible>=2.9.2"
    ```

 - [IBM Collection][] installed. 

    ```
    $ ansible-galaxy collection install ibm.cloudcollection
    ```

Once those are done you can move on to [Part 1](01-Configure/README.md) where you will configure your Ansible environment for IBM Cloud and run some test playbooks. 

## Guides

[**Part 1: Configure Environmental Variables and Test**](01-Configure/README.md)  
[**Part 2: Provision VPC and Networking resources**](02-Deploy-Vpc/README.md)  
[**Part 3: Deploy Compute instances and generate inventory**](03-Deploy-Compute/README.md)  
[**Part 4: Install Consul. Vault, and Nomad on compute instances**](04-Install-Hashistack/README.md)  
