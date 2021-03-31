+++
title = "Quiz"
page = true
weight = 13
+++

{{< quizdown >}}
# VPCs 1

At what point does the address translation take place between a resource, such as an EC2 instance's Elastic IP (EIP) and its private IP address

> This changes what any firewall in between would see


- [ ] At the Subnet where the resource (ENI) is located.
- [x] The Internet Gateway (IGW).
- [ ] The Elastic Network Interface of the resource
- [ ] it doesn't. Elastic IPs are assigned as Public IPs to the ENI along with the Private IP

# VPC 2

The NAT Gateways is deployed in a single Public Subnet with routing for all Private subnets, across Availability Zones, having default routes (0.0.0.0/0) pointing to the NAT Gateway. Is this best practice?

> Availability ZOne Independence is well architected

- [ ] Yes, NAT Gateway is a managed service and is highly available.
- [x] No, for production environments a NAT Gateway should be deployed in each Availability Zone, and routes should  point to a NAT Gateway in the Same Availability Zone. NAT Gateway is highly available but is per Availability Zone.

{{< /quizdown >}}
