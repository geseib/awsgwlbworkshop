+++
title = "AWS Gateway Load Balancer"
chapter = false
weight = 20
+++

## Introduction to AWS Gateway Load Balancer (ELB) ##

### What to expect ###

This workshop shows some of the basic networking aspects of using a Gateway Load Balancer to place Network/Security appliances inline transparently.

Within your own AWS account, you can explore how to direct traffic  through a fleet of Security Appliances.

We will explore two Architectural patterns: Distributed Ingress/Egress for Internet to VPC traffic and VPC to VPC internal traffic. Both with centralized security.

#### Distributed Internet Ingress/Egress
![GWLB for Internet traffic](/images/gwlb-welcome-distributed.png)

####  VPC to VPC
![GWLB for VPC to VPC traffic](/images/gwlb-welcome-vpc2vpc.png)


You will use the following services:

- **AWS Network Load Balancers** for a scalable application
- **AWS Gateway Load Balancers** for a scalable firewalls
- **AWS Privatelink** and **Transit Gateway** for cross VPC communication

