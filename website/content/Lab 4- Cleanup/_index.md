+++
menutitle = "Cleanup"
chapter = false
weight = 60
+++

# Clean Up
Once you are done exploring the aspects of Gateway Load Balancers, Endpoints, GENEVE encapsulations, and the Palo Alto security appliances you should minimize costs by removing everything we built in the workshop. Don't worry, you can always come back and build it again!

### Step-by-step

#### Remove the Endpoint routes
- Remove the VPC1 Edge associated route table. **recall, this route table directed all incoming traffic from the internet to the vpce. We had two routes to make sure that traffic destined for Availiablity Zone A went to through the Endpoint in Availability Zone A and traffic destined for Availiablity Zone B went to through the Endpoint in Availability Zone B. This creates Availailbity Zone independence, which mean an issue  in one AZ does not afftect the other AZs*
  - From the **VPC** console select **Route Tables** from the left menu.
  - Select the ***stack_name*-VPC1-Edge** route table, and from the **Actions** menu above select **Edit edge associations**.
  - Uncheck the box next to the VPC1 IGW and click the **Save** button.
  - Select the ***stack_name*-VPC1-Edge** route table, and from the **Actions** menu above select **Delete Route Table**. Confirm deletion by clicking the **Delete route table** button. 
- Remove the edpoints from the two VPC1 **Public** route tables. *One for each Availability Zone.* *Recall, this forced outbound internet traffic to flow back through the same endpoint that sent the traffic.*
  - Select the ***stack_name*-VPC1-Public A Route Table** route table, and from the **Actions** menu above select **Edit routes**.
  - To remove the Endpoint routes, click the **x** next to the *0.0.0.0/0* route pointing the the endpoint, **vpce-*xxxxxxxxxxx***. Click the **Save routes** button at the bottom right.
  - Select the ***stack_name*-VPC1-Public B Route Table** route table, and from the **Actions** menu above select **Edit routes**.
  - To remove the Endpoint routes, click the **x** next to the *0.0.0.0/0* route pointing the the endpoint, **vpce-*xxxxxxxxxxx***. Click the **Save routes** button at the bottom right.
- Remove the edpoints from the two FWVPC **TGW** route tables. *One for each Availability Zone.* *recall, this route table directed all incoming traffic from the TGW to the vpce in the FW VPC. this is for VPC to VPC traffic, directed through the TGW*
  - Select the ***stack_name*-TGW A Route Table** route table, and from the **Actions** menu above select **Edit routes**.
  - To remove the Endpoint routes, click the **x** next to the *0.0.0.0/0* route pointing the the endpoint, **vpce-*xxxxxxxxxxx***. Click the **Save routes** button at the bottom right.
  - Select the ***stack_name*-TGW B Route Table** route table, and from the **Actions** menu above select **Edit routes**.
  - To remove the Endpoint routes, click the **x** next to the *0.0.0.0/0* route pointing the the endpoint, **vpce-*xxxxxxxxxxx***. Click the **Save routes** button at the bottom right.

#### Remove the Endpoints
- Remove the VPC1 Endpoints
  - From the **VPC** console select **Endpoints** from the left menu.
  - Select the ***stack_name*-VPC1toFWVPC-GWLBe-AZA** Endpoint from the list and from the **Action** menu select **Delete endpoint**.
  - Select the ***stack_name*-VPC1toFWVPC-GWLBe-AZB** Endpoint from the list and from the **Action** menu select **Delete endpoint**.
- Remove the FW VPC Endpoints
  - From the **VPC** console select **Endpoints** from the left menu.
  - Select the ***stack_name*-FWVPC-GWLBe-AZA** Endpoint from the list and from the **Action** menu select **Delete endpoint**.
  - Select the ***stack_name*-FWVPC-GWLBe-AZB** Endpoint from the list and from the **Action** menu select **Delete endpoint**.
#### Remove the Endpoint Service
- Remove the Endpoint Service for the Gateway Load Balancer
  - From the **VPC** console select **Endpoint Services** from the left menu.
  - Select the Endpoint Service, type **GatewayLoadBalancer** *(should he the only one)* from the list and from the **Action** menu select **Delete**.

#### Remove the Gateway Load Balancer
- From the **EC2** console select **Load Balancers** from the left menu *(you may need to scroll down)*
- Select the Gateway Load Balancerfrom the list *(It will be of type **gateway**)* and from the **Action** menu select **Delete**.

#### Remove the rest of the resource that were created by Cloudfromation
- From the **Cloudformation** console select stack that you used to deploy the workshop and click the **Delete** button in the upper right.
- Confirm by clicking the **Delete stack** button. **This process will take several minutes.**
- If the stack deletion errors, then go back and verify that you have removed the items above, as well as any other items that you may have created in addition as part of your experimenting. Once you have deleted everything, go back and delete the cloudformation stack again.


### You Have cleaned up the resource used in the workshop. Way to be Cost Optimized!







