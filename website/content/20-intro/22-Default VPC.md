+++
title = "Intro to GWLB"
chapter = false
weight = 21
+++


## How Does GWLB send traffic to the right appliance

GWLB will pick a target base on a five-tuple hash, which means GWLB picks the backend target  based on these five components contained in every IP packet: 
- IP Protocol (for example TCP)
- Source IP address (for example 54.11.48.21)
- Source Port (for example 54001)
- Destination IP address (for example 10.1.2.113)
- Destination Port (for example 80 for HTTP)

GWLB will send the packet to same target for the return traffic as well. It would look like this:
- IP Protocol (for example TCP)
- Source IP address (for example 10.1.2.113)
- Source Port (for example 80 )
- Destination IP address (for example 54.11.48.21)
- Destination Port (for example 54001)

Its important to call out that if the same two hosts are talking on two different sets of ports, they could be routed through two different appliances. 

## Genenve Encapsulation
By encapsulating the traffic to the target, GWLB can provide seperation and some addition information about which GWLBe the traffic came from. This allows the return traffic to also go back to the correct GWLBe. 

For more details, read Milind Kularni's Blog, [Integrate your custom logic or appliance with AWS Gateway Load Balancer](https://aws.amazon.com/blogs/networking-and-content-delivery/integrate-your-custom-logic-or-appliance-with-aws-gateway-load-balancer/)