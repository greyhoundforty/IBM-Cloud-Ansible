# Overview
Using Ansible with the IBM Cloud

## Ansible 101: What is Ansible?

Ansible is an open source infrastructure automation tool initially operated by Redhat. Ansible enables you to tackle deployment, configuration, maintenance, etc. It allows you to quickly run a set of commands against a wide array of infrastructure, or push database configuration changes based on system hostname or OS. 

Ansible is agentless unlike a lot of other infrastructure automation tools, which means you don't actually need to install an agent on the hosts you want to maintain and manage. It uses SSH to communication with the hosts in your environment and is written in Python.  

Ansible has three major kind of uses: 

 - **Provisioning**: Deployment and provisioning of physical/virtual infrastructure either on-prem or in the cloud.
 - **Configuration**: System patches, package installations, routine maintenance. 
 - **Code Deployment**: Push our node.js or python applications directly to our infrastructure

**Similar Tools**

 - [Chef](https://www.chef.io/)  
 - [Puppet](https://puppet.com/)  
 - [Terraform](https://www.terraform.io/)  
 - [SaltStack](https://saltproject.io/)  
 - [Nomad](https://www.nomadproject.io/)

### Main Components of Ansible

**Inventory**  
In Ansible, an [inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#intro-inventory) is a set of hosts that Ansible can work against to run commands and playbooks. Once you've defined your inventory, you can use [patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#intro-patterns) to further refine the hosts or groups you want Ansible to run against.


**Playbooks**  
Ansible [playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html) are composed of one or more `plays` in an ordered list. Each play executes part of the overall goal of the playbook, running one or more tasks. Each task calls an Ansible module. You can use the playbook to push out new configuration or confirm the configuration of remote systems.


**Variables/Facts**  
With Ansible you can create, retrieve, or discover certain variables containing information about your remote systems or about Ansible itself. 

 - [Variables](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html): Ansible uses variables to manage differences between systems. You can define these variables in your playbooks, in your inventory, in re-usable files or roles, or at the command line.

 - [Facts](https://docs.ansible.com/ansible/latest/user_guide/playbooks_vars_facts.html): Ansible facts are data related to your remote systems, including operating systems, IP addresses, attached filesystems, and more. With facts, you can use the behavior or state of one system as configuration on other systems.

**Roles**  
[Roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html) let you automatically load related vars, files, tasks, handlers, and other Ansible artifacts based on a known file structure. After you group your content in roles, you can easily reuse them and share them with other users.

**Ad-hoc commands**  
Ansible [ad hoc](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html) commands are great for tasks you repeat rarely. For example, if you want to power off all the machines in your lab for Christmas vacation, you could execute a quick one-liner in Ansible without writing a playbook

## Prerequisites
In order to use the examples in this repository you will need to have the following pre-reqs completed:

 - IBM Cloud API Key. See this [guide](https://cloud.ibm.com/docs/account?topic=account-userapikey&interface=ui#create_user_key) if you need to generate one to use for this project. 
 - [Python 3](https://www.python.org/downloads/) installed. 
 - [Ansible](https://www.ansible.com/) installed. The IBM collection requires Ansible `2.9.2+`
 
    ```shell
    $ pip install "ansible>=2.9.2"
    ```

 - [IBM Collection](https://galaxy.ansible.com/ibm/cloudcollection) installed. 

    ```
    $ ansible-galaxy collection install ibm.cloudcollection
    ```

Once those are done you can move on to [Part 1](01-Configure/README.md) where you will configure your Ansible environment for IBM Cloud and run some test playbooks. 

## Guides

[**Part 1: Configure Environmental Variables and Test**](01-Configure/README.md)  
[**Part 2: Provision VPC and Networking resources**](02-Deploy-Vpc/README.md)  
[**Part 3: Deploy Compute instances and generate inventory**](03-Deploy-Compute/README.md)  
[**Part 4: Install Consul. Vault, and Nomad on compute instances**](04-Install-Hashistack/README.md)  
