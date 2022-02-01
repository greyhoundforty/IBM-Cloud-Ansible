# Overview
In this session we will configure Ansible to work with our IBM Cloud account and then run a test playbook to ensure that everything is configured properly. 

The Ansible collection uses the [IBM Cloud Terraform Provider]() configuration and as such we can set some Environmental Variables for use with our Playbooks. 

## Environmental Variables 


    - IC_RESOURCE_GROUP
    - IC_REGION

> If you need to test with a different Resource Group but don't want to unset your Environmental Variable, you can supply a Resource Group by appending your command with `-e resource_group=YOUR_RESOURCE_GROUP"`. 

## Testing Configuration 

**Listing available compute images in VPC**
```
$ ansible-playbook list-images.yml
$ ansible-playbook list-images.yml -e "resource_group=SOME_OTHER_RESOURCE_GROUP"
```

