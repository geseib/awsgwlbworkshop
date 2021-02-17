+++
title = "Modify FW VPC Routing"
chapter = false
weight = 72
+++

## Modify VPC1 Routing

We will now have traffic route though out new GWLB Endpoints in **FWVPC** to connect to our **Endpoint Service** we just created. In order to have Availability Zone independence, we need to make sure traffic from TGW attachment in AZ1 uses the GWLB Endpoint in AZ1. This provides Availability Zone redundancy in a subnet of each Availability Zone we have subnets for this VPC.
_Remember the VPC Endpoints are created in the VPC that will send traffic to the appliances. In this case firewalls_.

![Endpoint Service](/images/internal-gwlbe-diagram.png)


### Step-by-step


#### Record each Endpoint ID Availability Zone


1. From the **Amazon VPC** console and from the left menu, near the bottom, select **Endpoint**. 

   ![GWLB Endpoint List](/images/gwlbe-list-gwlbe.png))

1. In the details panel below, get the Availability Zone and ID of each of the FWVPC GWLB **Endpoints**. If you named them, you can see it in the table, otherwise you can click the Subnets tab and note the Availability Zone to the right of the Subnet ID. _be sure to get both endpoints, one for each **Availability Zone**_.

#### FWVPC TGW Subnets Route tables

1. From the **Amazon VPC** console and from the left menu select **Route Tables**. Click the box next to the ***stackName*** **-FWVPC1-TGW-AZ1** route table.

    ![Route tables Console](/images/internal-fwvpc-tgw-subnet-routetable-list.png)


1. Add a new route for the 0.0.0.0/0 CIDR and point to the GWLB Endpoint in the same AZ. __recorded above from the **Endpoints** console_. Click **Save routes** button. 
     ![Add Routes](/images/internal-fwvpc-gwlbe-route.png)

1. Repeat the above steps for the other TGW Subnet in AZ2, the ***stackName*** **-FWVPC1-TGW-AZ2** route table.

### You have completed the Modify FWVPC Subnet routes to the GWLBE Endpoint.
