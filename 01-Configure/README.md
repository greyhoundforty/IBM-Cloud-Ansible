# Overview

In this session we will configure Ansible to work with our IBM Cloud account and then run a test playbook to ensure that everything is configured properly. 

The Ansible collection uses the [IBM Cloud Terraform Provider](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs) configuration and as such we can set some Environmental Variables for use with our Playbooks.

## Set Environmental Variables

In order to authenticate with the IBM Cloud and target the right region and resource group we need to declare 3 variables:

```shell
export IC_API_KEY="YOUR_IBMCLOUD_API_KEY"
export IC_REGION="IBMCLOUD_VPC_REGION"
export IC_RESOURCE_GROUP="YOUR_RESOURCE_GROUP"
```

> If you need to test with different variables, such as targeting a different region or resource group you can use the `-e "variable=updated_value"` syntax. For example to target a different region you would use: `-e "region=OTHER_VPC_REGION"`

## Clone repo with example code

```shell
git clone https://github.com/greyhoundforty/IBM-Cloud-Ansible.git
cd IBM-Cloud-Ansible/01-Configure
```

## Testing Configuration

Now that we've got our environment set up we can test that everything is correctly configured and ready to deploy resources in the nest lesson.

### Listing available compute images in VPC

```shell
ansible-playbook list-images.yml
ansible-playbook list-images.yml -e "region=OTHER_VPC_REGION"
```

### Listing available compute profiles in VPC

```shell
ansible-playbook list-profiles.yml
ansible-playbook list-profiles.yml -e "region=OTHER_VPC_REGION"
```

 > This command may fail due to a known issue with the Ansible collections python conversion script. The fix for me is outlined in [here](https://github.com/IBM-Cloud/ansible-collection-ibm/issues/82#issuecomment-1026995864).

## Next steps

Once you have verified that you can (at least) list the available compute images you can move on to [Part 2](../02-Deploy-Vpc/README.md) where we will deploy some VPC resources and look at our gathered **facts** about the deployment.
