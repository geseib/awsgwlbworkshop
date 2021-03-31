+++
title = "Quiz"
page = true
weight = 90
+++

{{< quizdown >}}
# GWLB 1

For Resiliency, we deploy two Gateway Load Balancers that point to the same target appliances.

> Endpoints and Gateway load balancers deploy across Availability Zones differently.


- [x] No, you deploy just one Gateway Load Balancer, but you must specify subnets from at least two Availability Zones to increase the availability of your load balancer. 
- [ ] No, you deploy just one Gateway Load balancer as it is a Regional service and Availability ZOne independent. 
- [ ] No. Every Availability Zone that has targets in it should have a Gateway Load Balancer deployed in it.
- [ ] Yes. For resiliency, deploy two Gateway Load Balancers.

# GWLB 2

When you share a Gateway Loadbalancer using Endpoint Services, how can you assure when deploying GWLB Endpoints in multiple AWS accounts, that your Endpoints can be created in the Subnets that each accounts uses.

> Availability Zone and Availability Zone ID are different.

- [ ] Be sure to only create Subnets in the same Availability Zone IDs across all accounts
- [x] Create subnets and Gateway Loadbalancer across all Availability Zones that are availabile in a Region. Either distribute the traget appliances in every availability zone or enable cross zone laod balancing and place them in two or more Availaiblity Zones.
- [ ] You do not need to do anything. Gateway Load Balancers and GWLB Endpoints can be deployed across Availability Zones.

{{< /quizdown >}}