+++
title = "Modify TGW Routing"
chapter = false
weight = 73
+++

## Modify Transit Gateway Routing

Currently the TGW Route table for the Apps VPCs (VPC1 and VPC2) routes traffic directly between the VPCs. While there is a route that sends all 10.0.0.0/8 traffic to the FWVPC, there are more specific routes that send 10.1.0.0/16 to VPC1 and 10.2.0.0/16 to VPC2. We will now have traffic route though out new GWLB Endpoints in **FWVPC** by removing those two more specific routes. 

### Old TGW route table for Apps VPCs
_The CIDR is the Destination the traffic matches and the Attachment is where to send that traffic_
| CIDR | Attachment |
|------|------------------------|
|10.0.0.0/8 | FWVPC TGW Attach ID |
|10.1.0.0/16| VPC1 TGW Attach ID |
|10.2.0.0/16| VPC2 TGW Attach ID |



### New TGW route table for Apps VPCs
_after removing the Direct VPC1 and VPC2 routes, traffic sent from VPC1 or VPC2 will be directed to the FWVPC._
| CIDR | Attachment |
|------|------------------------|
|10.0.0.0/8 | FWVPC TGW Attach ID |

### Exisiting TGW route table for the Firewall VPC
_Notice that traffic coming from FWVPC has routes to VPC1 and VPC2._
| CIDR | Attachment |
|------|------------------------|
|10.1.0.0/16| VPC1 TGW Attach ID |
|10.2.0.0/16| VPC2 TGW Attach ID |

### Step-by-step


#### TGW App Route table

1. From the **Amazon VPC** console and from the left menu select **Transit Gateway Route Tables**. Select the ***stackName*** **-Apps** route table.

    ![Route tables Console](/images/internal-tgw-routetable-list.png)

1. Change to the **Routes** tab
1. To delete the direct route to VPC 1: Click the box next to the **CIDR** **10.1.0.0/16** and click the **Delete static route** button above the table. 
1. To delete the direct route to VPC 2: Click the box next to the **CIDR** **10.2.0.0/16** and click the **Delete static route** button above the table. 





### You have completed the TGW Route table modification.
