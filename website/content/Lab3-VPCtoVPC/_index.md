+++
title = "Lab 3: VPC to VPC Firewall"
chapter = true
weight = 30
+++

## Lab 3 VPC to VPC Firewall

Now, what if we want to have a firewall inspect traffic between VPCs. By modifying the Route tables in the transit gateway to send traffic to our FW VPC which has out **Gateway Load Balancer** traffic can be inspected.

### Starting Config
The Traffic flow for cross VPC traffic is direct through the Transit Gateway (TGW).
![Before](/images/internal-start-diagram.png)

### Final Config
The traffic still flows over the Transit Gateway (TGW); however, now the TGW route table for the APP VPC attachments directs all (0.0.0.0/0) traffic to the Firewall VPC. In the Firewall VPC, the trafic from the transit gateway uses the TGW Attach subnet route table and is sent to the Gateway Load balanacer Endpoints. From there the Gateway Load balancer encapsulates the traffic to the firewall (via GENEVE encap). Traffic returns back through the GWLB and the GWLBe. The GWLBe subnet has a 10.0.0.0/8 route which points back to the TGW. Once at the TGW, traffic will be sent to the destination VPC using the TGW route table for the firewall VPC attachment. 
![After](/images/internal-final-diagram.png)

#### What you will build:

1. **Gateway Load Balancer Endpoints** just like the previous lab except in the same VPC (Firewall VPC) as the GWLB that was created in the last lab.
1. Modify the **VPC route table** for the TGW Subnets in the Firewall VPC. to point all traffic to the GWLB endpoints. 
1. On the **Transit Gateway**, modify the **TGW App route table**: removing the direct VP1 and VPC 2 routes and  instead a new default route sending all traffic to the Firewall VPC.
