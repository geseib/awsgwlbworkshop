+++
title = "Create GWLB Endpoint Service"
chapter = false
weight = 70
+++

## Overivew: Create Gateway Load Balancer Endpoint Service

We will create a Gateway Load Balancer Endpoint Service to make our GWLB availabile to accept endbound traffic for sources in the same region.  We could even share to VPCs in other AWS accounts. _Remember the GWLB **Endpoint Services** are created in the VPC that has the GWLB_.

![Endpoint Service](/images/gwlbe-diagram.png)

### Step-by-step
![Endpoint Service Console](/images/gwlbe-gwlbes-list.png)

1. From the **Amazon VPC** console and from the left menu select **Endpoint Service**. Click the **Create Endpoint Service** button.

    ![Create GWLBeS](/images/gwlbe-create-gwlbes.png)

1. Configure the **VPC Endpoint Service** with the following selections:
    - **Associate Load Balancer**: check to box next to the GWLB created earlier, named  ***stackName*** **-FWVPC-FWs**. _be sure not to pick the NLB_.
    - Notice the **Included** and **Excluded Availability Zones**. _When sharing to other accounts it is a good practice to place the GWLB in all Availability Zones to provide complete access no matter which Availiablity Zones the consumer VPCs are in. This may mean creating more subnets in the security VPC and using **cross zone load balancing**_.
    - Uncheck the **Require acceptance for endpoint**, so you do not have accept the VPC Endpoint when the consumer VPCs create them. 
    - Uncheck **Enable private DNS name**. _This is a great feature to automate creation of the DNS entry along with the Endpoint creation for the consumers. It requires proof of domain ownership. We dont have a real public DNS domain to use for the lab._
    - Click the **Add Tag**: button and give it a tag with the **Key** as **Name** and the **Value** as something like ***stackName*** **-FWVPC-GWLBes**
    - Click the **Create service** button to finish


    ![GWLBES Created](/images/gwlbe-created-gwlbes.png)

1. Click the **Close** button once the **Endpoint Service** has been created.

#### Verify and Record Endpoint Service Name

1. From the **Amazon VPC** console and from the left menu, near the bottom, select **Endpoint Services**. Check the box next to your **Endpoint Service** in the list.

   ![Select Endpoint Service](/images/gwlbe-gwlbes-details.png)

1. In the details panel below, get the DNS Name of the **Endpoint Service****. You can click the copy button to the right of the Service name.



### You have completed the Create VPC Endpoint Service.
