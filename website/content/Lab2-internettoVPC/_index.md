+++
title = "Lab 2: Internet to VPC Firewall"
chapter = true
weight = 30
+++

## Lab 2 Internet to VPC Firewall

Let's protect our internet facing applications with the Palo Alto Firewalls.. Using **Gateway Load Balancer** not only provides availablity and scalability to our application, but we can use it to share our application to other VPCs via **AWS Privatelink**.

### VPC 1 Before and After
![VPC1 Diagrams](/images/lab2-before-after-VPC1.png)

### Final Architecture
![endpoint Diagram](/images/lab2-internetFW-design.png)

#### What you will build:

1. **Gateway Load Balancer** with an Auto Scaling group of EC2 Instance built from a template
2. **Gateway Load Balancer Endpoint Service**: making the application available for other VPCs to consume
3. **Gateway Load Balancer Endpoint**: Consume the application from another VPC, without going over the TGW
4. Modify some of our **VPC route tables**
