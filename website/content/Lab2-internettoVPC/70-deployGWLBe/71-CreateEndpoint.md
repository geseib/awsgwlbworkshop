+++
title = "Create GWLB Endpoint"
chapter = false
weight = 71
+++

## Create GWLB Endpoint

We will now create a GWLB Endpoint in **VPC1** to connect to our **Endpoint Service** we just created. This will create Elastic Network Interfaces in a subnet of each Availability Zone we have subnets for this VPC.
_Remember the VPC Endpoints are created in the VPC that will send traffic to the appliances. In this case firewalls_.

![Endpoint Service](/images/lab2-gwlbe-diagram.png)

### Step-by-step


#### Record Endpoint Service Name

_(if not completed previously)_

1. From the **Amazon VPC** console and from the left menu, near the bottom, select **Endpoint Services**. Check the box next to your **Endpoint Service** in the list.

   ![Select Endpoint Service](/images/gwlbe-gwlbes-details.png)

1. In the details panel below, get the Service Name of the **Endpoint Service**. You can click the copy button to the right of the Service name.

#### Endpoint creation


1. From the **Amazon VPC** console and from the left menu select **Endpoint**. Click the **Create Endpoint** button.

    ![PL Console](/images/gwlbe-gwlbe-list.png)

1. Configure the **VPC Endpoint** with the following selections:

   - For **Service Catagory** select **Find service by name**.
   - For the **Service Name** Paste in the **Endpoint Service** name recorded above _(or in previous task)_ It will look something like this: **com.amazonaws.vpce.us-east-1.vpce-svc-0a37cf83d4fd5876c**.
   - Click the **Verify** button. You should see **Service name found.** Otherwise, you back and make sure you recorded the correct name.

     ![Create Endpoint](/images/gwlbe-create-gwlbe.png)

1. Scroll down and Configure the **VPC Endpoint** with the following selections:

   - For **VPC** select the VPC1 from the dropdown list.
   - This should bring up the two Availability zones already checked. Change the first **Availability Zone** **Subnet ID** to the FW subnets _unlike other VPC Endpoints, We can only configure 1 Availability ZOne at a time, so we will uncheck one of the subnets. We will repeat this step for the other AZ_.
   - Uncheck the Second **Availability Zone**
   - Click **new tag** button
     - **Key**: Type **Name** _use upper case 'N', as its case sensitive_
     - **Vaule**: Name your VPC Endpoint something like **VPC1-GWLBEtoFWVPC-GWLB**
   - Click the **Create endpoint** in the bottom right.


1. Click the **Close** button once the **Endpoint ** has been created.

1. Repeat the above steps only this time uncheck the first Availability Zone, and select the FW subnet in the second Availability zone for VPC1.
_you would repeat this step for every availability zone you wanted to provide endpoints_.

     ![Create Endpoint](/images/gwlbe-create-gwlbe2.png)

1. You will have two **GWLB Endpoints** in VPC1. _The other two endpoints are S3 Gateway endpoints deployed in the security VPC for the firewalls to get their bootstrap files you put into S3_).
     ![GWLB Endpoint List](/images/gwlbe-list-gwlbe.png)

### You have completed the Create GWLBE Endpoint .
