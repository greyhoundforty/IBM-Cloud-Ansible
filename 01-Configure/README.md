# Overview
In this session we will configure Ansible to work with our IBM Cloud account and then run a test playbook to ensure that everything is configured properly. 

The Ansible collection uses the [IBM Cloud Terraform Provider](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs) configuration and as such we can set some Environmental Variables for use with our Playbooks. 

## Set Environmental Variables 

In order to authenticate with the IBM Cloud and target the right region we need to declare 2 variables:

```shell
export IC_API_KEY="YOUR_IBMCLOUD_API_KEY"
export IC_REGION="IBMCLOUD_VPC_REGION"
```

> If you need to test with a different Region but don't want to unset your Environmental Variable, you can supply the Region by appending your command with `-e region="OTHER_VPC_REGION"`. 

## Clone repo with example code

```shell
git clone https://github.com/greyhoundforty/IBM-Cloud-Ansible.git
cd IBM-Cloud-Ansible/01-Configure
```

## Testing Configuration 
Now that we've got our environment set up we can test that everything is correctly configured and ready to deploy resources in the nest lesson.

**Listing available compute images in VPC**

```shell
$ ansible-playbook list-images.yml
$ ansible-playbook list-images.yml -e "region=OTHER_VPC_REGION"
```

**Listing available compute profiles in VPC**

```shell
$ ansible-playbook list-profiles.yml
$ ansible-playbook list-profiles.yml -e "region=OTHER_VPC_REGION"
```
