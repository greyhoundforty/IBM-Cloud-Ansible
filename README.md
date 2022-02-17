# Overview
This repository is meant for anyone getting started with Ansible. While the code examples are mainly geared towards IBM Cloud, the lessons are applicable across Ansible providers. 

## Ansible 101: What is Ansible?

Ansible is an open source infrastructure automation tool initially operated by Redhat. Ansible enables you to tackle deployment, configuration, maintenance, etc. It allows you to quickly run a set of commands against a wide array of infrastructure, push database configuration changes based on system hostname or OS, and even provision additional compute resources. 

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

## Main Components of Ansible

### Inventory  
In Ansible, an [inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#intro-inventory) is a set of hosts that Ansible can work against to run commands and playbooks. Once you've defined your inventory, you can use [patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#intro-patterns) to further refine the hosts or groups you want Ansible to run against.

Here is an example `INI` style inventory file from my homelab environment:

```ini
[homelab]
neptune ansible_host=192.168.4.100
ares ansible_host=192.168.4.189 
dione ansible_host=192.168.4.185
phobos ansible_host=192.168.4.192 
io ansible_host=192.168.4.194

[vaultnodes]
phobos ansible_host=192.168.4.192 
ares ansible_host=192.168.4.189
dione ansible_host=192.168.4.185

[consulnodes]
neptune ansible_host=192.168.4.100
ares ansible_host=192.168.4.189 
io ansible_host=192.168.4.194
```

The headings in brackets are group names, which are used in classifying sets of hosts that you can then interact with using Ansible.  You can and likely will have hosts that exist in more than one group. You can think of groups as a way to track:

 - The `application` or `service` you are interacting with (web servers, dbs, etc)
 - The `location` of the resources you are dealing with (datacenter, MZR, etc)
 - The `development` or `deployment` target for the resources (Prod, Dev, etc)


You can also store variables that relate to a specific host or group of hosts in your inventory. Group variables are a convenient way to apply variables to multiple hosts at once. 

```ini

[monitoring]
titan ansible_host=192.168.4.199 ansible_user=deploy ansible_port=6759

[homelab:vars]
ansible_become=yes
ansible_ssh_extra_args='-o "StrictHostKeyChecking=no" -o ProxyCommand="ssh -o StrictHostKeyChecking=no -W %h:%p ryan@192.168.122.4"'
ansible_port=22
ansible_user=ryan

[control]
local ansible_connection=local
```


> If a host is a member of multiple groups, Ansible will read the variables for all groups and then determine which to use based on its own [merging logic](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#how-we-merge).
 

### Playbooks  
Ansible [playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html) are composed of one or more `plays` in an ordered list. Each play executes part of the overall goal of the playbook, running one or more tasks. Each task calls an Ansible module. You can use playbooks to push out new configuration, udpate your database servers, or run complex shell scripts. 

This is an example of a basic playbook that utilizes the built in Ansible [copy](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html) module. 

```yaml 
---
  - hosts: all:!control
    tasks:
      - name: Send default zsh/bash profile file
        copy:
          src: /Users/ryan/Code/host_templates/default-profile
          dest: /home/ryan/.profile
          owner: ryan
          group: ryan
          mode: 0644
          backup: yes
```

At a high level you can think of playbooks as outlining the following 3 things:

 - Which hosts to interact with (Where)
 - A set of tasks to run against the hosts (What) 
 - Logic to determine which tasks need to be run on which hosts (When)

### Variables  

Ansible uses [Variables](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html) to manage differences between systems. You can define these variables in your playbooks, in your inventory, in re-usable files or roles, or at the command line. They can then be used by playbooks, for loops, and conditional `when` statements. 

In this example we are providing Variables at the playbook level. We are explicitally setting 2 of the variables, and using Ansibles built in ability to lookup Environmental Variables for our API Key variable.

```yaml
---
  - hosts: all:!control
    vars:
      ibmcloud_api_key: "{{ lookup('env', 'IC_API_KEY') }}"
      region: "us-south"
      project: "thundercougarfalconbird"


```

### Facts 

Ansible [Facts](https://docs.ansible.com/ansible/latest/user_guide/playbooks_vars_facts.html) are data related to your remote systems, including operating systems, IP addresses, attached filesystems, and more. With facts, you can use the behavior or state of one system as configuration on other systems.

In this playbook we are utilizing the built in `os_family` fact to determine which update command to run based on the underlying Operating System of the hosts.

```yaml
---
  - hosts: all:!control
    become: true
    tasks:
      - name: Update yum packages
        yum: name=* state=latest
        when: ansible_facts['os_family'] == "RedHat"
      - name: Update apt packages
        apt: upgrade=yes update_cache=yes
        when: ansible_facts['os_family'] == "Debian"
      - name: Update Arch packages
        pacman: upgrade=yes update_cache=yes
        when: ansible_facts['os_family'] == "Archlinux"
```

You can also register facts from the output of one task to be used across other tasks and playbooks. You do this using the [set_fact](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/set_fact_module.html) module, but we'll dive more in to that in when we actually deploy an IBM Cloud VPC. 

### Roles  
[Roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html) let you automatically load related vars, files, tasks, handlers, and other Ansible artifacts based on a known file structure. After you group your content in roles, you can easily reuse them and share them with other users.

### Ad-hoc commands  
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
[**Part 2: Provision VPC and associated network resources**](02-Deploy-Vpc/README.md)    
[**Part 3: Deploy Compute instances and generate inventory**](04-Deploy-Compute/README.md)  
[**Part 4: Install Consul. Vault, and Nomad on compute instances**](05-Install-Hashistack/README.md)  
