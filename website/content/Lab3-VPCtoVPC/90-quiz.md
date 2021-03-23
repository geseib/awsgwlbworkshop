+++
title = "Quiz"
page = true
weight = 90
+++

{{< quizdown >}}
# TGW 1

Can you send traffic directly to the Gateway Load Balancer, or do you always need to send traffic through the Gateway Load Balancer Endpoints?

> What information is provided by the GENEVE Header. See this [Blog](https://aws.amazon.com/blogs/networking-and-content-delivery/integrate-your-custom-logic-or-appliance-with-aws-gateway-load-balancer/)


- [ ] Gateway Load Balancer Endpoints are only needed when deploying without a Transit Gateway.
- [x] Always use Gateway Load Balancer Endpoints. As This provides the key information 
- [ ] If you have only local VPC trafic you can use the Gateway Load Balancer without provisioning Gateway Load Balancer Endpoints.

# TGW 2

Why do we need to modify the VPC attachment on the TGW to use appliance mode? 

> Think stateful inspection and symmetric flow

- [x] To assure that the same gateway Load balancer Availabilty Zone gets the entire flow and send traffic symmterically through the same appliance instance.
- [ ] Appliance mode is only needed if you devices are not Gateway Loadbalancer aware.
- [ ] To ensure that the GENEVE protocol is included across the Transit Gatway.
- [ ] Increase the largest frame size supported across the Transit Gateway

{{< /quizdown >}}