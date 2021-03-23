+++
title = "Quiz"
page = true
weight = 52
+++

{{< quizdown >}}
# Palo Alto

How does the appliance know which GWLB Endpoint sent a packet?

> Headers can contain a lot of information


- [ ] Palo Alto receives event from Cloudwatch
- [ ] Palo Alto talkes directly to AWS VPC API
- [x] The GENEVE encapsulation includes the GWLB Endpoint ID
- [ ] NAT is used by the GWLB endpoint to make the ientify the source

# Palo Alto 2

In the workshop we seperated out the traffic between the VPC1 endpoints and the FWVPC endpoints using Subinterfaces, but the same security policy was applied. This can work fine in many cases, but what if the the different VPCs had the same IP CIDRs. how could you assure the rules were specific to each VPC.

> Virtualization is everywhere

- [ ] Use dedicated fleets of appliances for each environment
- [x] Place the subinterfaces in different zones and apply unique security policys to each zone
- [ ] Be Sure to NAT the addresses in each VPC or at the GWLB to unqie IP addresses to identify them to a specific security policy
- [ ] You cant. You have to use a new deployment of GWLB and GWLB Endpoints for overlapping CIDRs
{{< /quizdown >}}