# Overview

In this session we will deploy the following IBM Cloud resources:

- A [VPC][vpc] where our compute resources will be deployed.
- A [Public Gateway][pubgw] that will allow our private only Compute instances to pull down packages and install updates.
- A [Subnet][subnet] that will define the IP addressing scheme used by the compute instances.  
- Two [Security Groups][sgs] that will define how our hosts can communicate.



























[vpc]: https://cloud.ibm.com/docs/vpc?topic=vpc-about-vpc
[pubgw]: https://cloud.ibm.com/docs/vpc?topic=vpc-about-networking-for-vpc#public-gateway-for-external-connectivity
[subnet]: https://cloud.ibm.com/docs/vpc?topic=vpc-about-networking-for-vpc#subnets-in-the-vpc
[sgs]: https://cloud.ibm.com/docs/vpc?topic=vpc-security-in-your-vpc#sgs-security