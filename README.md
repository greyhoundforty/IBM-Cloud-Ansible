# Overview
Using Ansible with the IBM Cloud

### Prerequisites

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

[**Part 1: Configure Environmental Variables and Test**](part1/README.md)
[**Part 2: Provision VPC and Networking resources**](part2/README.md)
[**Part 3: Deploy Compute instances and generate inventory**](part3/README.md)
[**Part 4: Install Consul. Vault, and Nomad on compute instances**](part4/README.md)
