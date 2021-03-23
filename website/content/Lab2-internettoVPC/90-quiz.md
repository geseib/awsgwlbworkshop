+++
title = "Quiz"
page = true
weight = 90
+++

{{< quizdown >}}
# GWLB 1

For Resiliency we deploy two Gateway Load Balancers

> This changes what any firewall inbetween would see


- [ ] At the Subnet where the resource (ENI) is located.
- [x] The Internet Gateway (IGW)
- [ ] The Elastic Network Interface of the resource
- [ ] It doesn't. Elastic IPs are assigned as Public IPs to the ENI along with the Private IP.

# GWLB 2

When you share a Gateway Loadbalancer using Endpoint Services, how can you assure when deploying GWLB Endpoints in multiple AWS accounts, that your Endpoints can be created in the Subnets that each accounts uses.

> Availability Zone and Availability Zone ID are different.

- [ ] Be sure to only create Subnets in the same Availability Zone IDs across all accounts
- [x] Create subnets and Gateway Loadbalancer across all Availability Zones that are availabile in a Region. Either distribute the traget appliances in every availability zone or enable cross zone laod balancing and place them in two or more Availaiblity Zones.
- [ ] You do not need to do anything. Gateway Load Balancers and GWLB Endpoints can be deployed across Availability Zones.

{{< /quizdown >}}