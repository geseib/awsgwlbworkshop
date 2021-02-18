+++
title = "Modify VPC Routing"
chapter = false
weight = 72
+++

## Modify VPC1 Routing

We will now have traffic route though out new GWLB Endpoints in **VPC1** to connect to our **Endpoint Service** we just created. In order to have Availability Zone independce, we need to make sure traffic destined for Public Subnet in AZ1 use the GWLB Endpoint in AZ1. This provides workloads  Availability Zone redunandy in a subnet of each Availability Zone we have subnets for this VPC.
_Remember the VPC Endpoints are created in the VPC that will send traffic to the appliances. In this case firewalls_.

![Endpoint Service](/images/lab2-vpc1-routes-diagram.png)

Two things to note:
- A new route table will be **Edge Associated** to the **IGW**. For the CIDR of each **Public** subnet, we have a route entry pointing to a GWLB Endpoint in the same AZ.
- Each **Public** Subnet has a route table that points the default route (0.0.0.0/0) to a GWLB Endpoint in the same AZ). _normally public subnets point their defualt routes to the IGW_.

### Step-by-step


#### Record each Endpoint ID Availability Zone


1. From the **Amazon VPC** console and from the left menu, near the bottom, select **Endpoint**. 

   ![GWLB Endpoint List](/images/gwlbe-list-gwlbe.png))

1. In the details panel below, get the Availability Zone and ID of each of the GWLB **Endpoints**. If you named them, you can see it in the table, otherwise you can click the Subnets tab and note the Availability ZOne to the right of the Subnet ID. _be sure to get both endpoints, one for each **Availability Zone**_.

#### Edge Route table

1. From the **Amazon VPC** console and from the left menu select **Route Tables**. Click the **Create route table** button.

    ![Route tables Console](/images/gwlbe-routetable-list.png)

1. Configure the **Route table** with the following selections:

   - For **Name tag** enter ***stackName*** **-VPC1-Edge**.
   - For the **VPC** Select **VPC1** from the list.
   - Click the **Create** button. 

     ![Create Endpoint](/images/gwlbe-create-edgert.png)

1. Select the Edge route table from the **Route Tables** console. Check the box next to the Edge route table. _you can search for it by typing 'edge' into the filter by tags and attributes bar_.

     ![Edit Routes](/images/gwlbe-select-edgert.png)
1. Switch to the **Routes** tab for the Edge route table and click **Edit routes** button.

     ![Routes tab](/images/gwlbe-edgert-routes.png)

1. Add two new routes for each Public Subnet's CIDR and point to the GWLB Endpoint in the same AZ. __recorded above from the **Endpoints** console_. Click **Save routes** button. 
     ![Add Routes](/images/gwlbe-edgert-addroutes.png)

1. Switch to the **Edge Associations** tab for the Edge route table and click **Edit edge associations** button.

![Edge Associations](/images/gwlbe-edgert-associations.png)

1. Check the igw ID for **VPC1** in the **Associate gateways** box. Click the **Save** button.
![Edge Associations](/images/gwlbe-edgert-associate.png)    

#### Public route table modification

1. From the **Amazon VPC** console and from the left menu select **Route Tables**. FIlter using the term "VPC1-Public" and check the box next to the **Public A route table**.

1. Switch to the **Routes** tab for the first Public route table and click **Edit routes** button
     ![Routes tab](/images/gwlbe-pubArt-routes.png)

1. Delete the igw **Target** for the defualt (0.0.0.0/0) route and select the GWLB Endpoint in the same AZ. __recorded above from the **Endpoints** console_. Click the **Save routes** button. 
     ![Create Endpoint](/images/gwlbe-pubA-addroutes.png)

1. Repeat the above 3 steps with **Public B Route Table**
  - Be sure to modify the default route (0.0.0.0/0) to the GWLB Endpoint in AZ B and Click the **Save routes** button. 
     ![Create Endpoint](/images/gwlbe-pubBrt-routes.png)


### You have completed the Create GWLBE Endpoint.
