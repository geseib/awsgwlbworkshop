+++
menutitle = "Create GWLB Endpoints"
chapter = false
weight = 70
+++

## Create an Gateway Load balaner endpoint in VPC 1

Gateway load Balancer endpoints are the networking entrypoint to a Gateway Load Balancer. Since we want to protect our applications in VPC1 we will create endpoints in subnets named "***stackName*** **-VPC1-FW-x-Subnets**". These subnets will be sandwiched between the IGW and the public subnets when we modify VPC routing. We will force inbound and outbound traffic to flow through these endpoints and allow our Firewalls to inspect all internet traffic.


![GWLBe Diagram](/images/gwlbe-diagram.png)

#### We will complete the following steps:
1. Create a **VPC Endpoint Service** for the FWVPC GWLB

1. Create a **VPC Endpoint** in the VPC1 FW Subnets

1. Create a route table for edge routing pointing traffic destined for public subnets to the Endpoints, and associate it with VPC1's IGW.

1. Modify route tables for Public Subnets to point the default route to the endpoints.

1. Test connectivity and verify we are routing through the Firewalls.

