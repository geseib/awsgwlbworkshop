+++
title = "Separate internal vs external traffic"
chapter = false
weight = 51
+++

### Central Inspection with sepration of traffic
This encapsulation allows us to create virtual interfaces on our appliances and apply different sets of policies for them.
See the Palo Alto [documentation](https://docs.paloaltonetworks.com/vm-series/10-0/vm-series-deployment/set-up-the-vm-series-firewall-on-aws/vm-series-integration-with-gateway-load-balancer/integrate-the-vm-series-with-an-aws-gateway-load-balancer/associate-a-vpc-endpoint-with-a-vm-series-interface.html) for more details on how this is done.

### Genenve Encapsulation
By encapsulating the traffic to the target, GWLB can provide seperation and some addition information about which GWLBe the traffic came from. This allows the return traffic to also go back to the correct GWLBe. 

For more details, read Milind Kularni's Blog, [Integrate your custom logic or appliance with AWS Gateway Load Balancer](https://aws.amazon.com/blogs/networking-and-content-delivery/integrate-your-custom-logic-or-appliance-with-aws-gateway-load-balancer/)

### Seperate policies for VPC-to-VPC and Internet-to-VPC1
In order to create these separate policies, we need to let the appliance know to use the information in the GENEVE encapsulation to associate the GWLBE where the traffic comes from with a virtual interface called **subinterface**

#### Subinterfaces in Palo Alto VM-Series
We will create subinterfaces on the security appliance. Think of this as a way to separate out traffic to use different security policies for each of our types of traffic. In our case we want:
  - one subinterface for VPC1 internet traffic
  - one interface for the VPC to VPC traffic. 
  All of this traffic will arrive on the same ENI on the EC2 instance of the Palo Alto Firewall, but we will use the GENEVE header to assoicate with the correct subinterfaces in a later step (**Associate GWLB Endpoints with a Subinterface**). 

**Step-by-step**
  1. From the Palo Alto VM_Series web interface, switch to the **Network** tab. Verify you are on  **Interfaces** on the left menu.
  **Create a subinterface for Internet to VPC1 traffic**
  1. Click on and highlight Interface **ethernet1/1**. Click the **+Add Subinterface** button at the bottom of the screen.
  1. In the **Layer3 Subinterface** dialog box change the subinterface number from (1-9999) to **100** and also set the **Tag** to **100**. *Note: the Tag is not actually used in this case, but is a required field)*. Set Virtual Router to **default** and set **Security Zone** to **DATA**.
  ![Subinterface 100](/images/pa-subinterface100.png)
  1. In order to push these changes to the running Policies, click the **Commit** button at the top right of the console page. 
  **Repeat for a second subitnerface for the VPC-to-VPC traffic**
  1. Click on and highlight Interface **ethernet1/1**. Click the **+Add Subinterface** button at the bottom of the screen.
  1. In the **Layer3 Subinterface** dialog box change the subinterface number from (1-9999) to **200** and, set the **Tag** to **200**. *Note: the Tag is not actually used in this case, but is a required field)* . Set Virtual Router to **default** and set **Security Zone** to **DATA**. *Note: likely these would be set to different Zones to allow for unique security policies to be applied to each. In our case we will keep them the same and just observe to subinterfaces on the monitor screen.
  ![Subinterface 200](/images/pa-subinterface200.png)
  #### Make sure you performed the configuration steps and committed the changes on both appliances


#### Record the GWLB Endspoint IDs
Since we want to associate the VPC1 GWLB Endpoints with the first subinterface (for VPC1's internet traffic) and and Security VPC's GWLB Endpoints with the second subinterface (for VPC to VPC traffic), we need to get the IDs.

**Step-by-step**
- From the **AWS VPC** console and from the left menu select **Endpoints**. 
- Filter the results by entering the the search term, **GatewayLoad** in the filter bar where it says **Add filter**. *Note: the search is not case sensitive*
- Copy these down, being sure to record and separate out by VPC ID. *We want to distinguish out the internet traffic going through the **VPC1** endpoints from the internal VPC to VPC traffic going through the **FWVPC** endpoints.* 
![GWLB Endpoint IDs](/images/pa-endpoint-ids.png)

#### Associate GWLB Endpoints with a Subinterface
You can associate multiple GWLB endpoints to a single subinterface on the VM-Series firewall. However, you must associate each GWLB endpoint individually. For example, to associate GWLB endpoint in **VPC1 AZ a** and GWLB endpoint in VPC1 AZ b with subinterface ethernet1/1.100, you must execute the association command twice, separately for each GWLB endpoint. We will need to do the assoication from CLI using SSH. lets go back to Cloudshell
- Launch **AWS CloudShell**:
  - From the console search for and select  **AWS CloudShell**. After a minute or so a new cloud based shell environment will launch, allowing you to use AWS cli tools and other bash based tools like dig
   ![Cloudshell](/images/gwlb-cloudshell-open.png)
- SSH into the first Palo Alto
  - the SSH command can be found in the **Cloudformation** console. Select the **stack** and change to the **outputs** tab. ***there will be two ssh commands, one for each appliance***
  - enter configuration mode by entering `configure`
  - We need to associatio each **Gateway LoadBalancer Endpoint** with the correct **subinterface** that we created. We will use the command format `request plugins vm_series aws gwlb associate vpc-endpoint <vpce-id> interface <subinterface>`. Use the information you recorded in the section above to complete this. You we need to run the command four times, once for each **GWLB Endpoint**. The CLI will verify the subinterface name but not the endpoint ids, so be sure to double check you have the correct IDs associated with the correct subinterface number.
    ```
    admin@ip-10-0-2-78> request plugins vm_series aws gwlb associate vpc-endpoint vpce-029230815a33bb5f8 interface ethernet1/1.100

    admin@ip-10-0-2-78> request plugins vm_series aws gwlb associate vpc-endpoint vpce-0a82e2729f61fa85a interface ethernet1/1.100

    admin@ip-10-0-2-78> request plugins vm_series aws gwlb associate vpc-endpoint vpce-0a8650a40b422072e interface ethernet1/1.200

    admin@ip-10-0-2-78> request plugins vm_series aws gwlb associate vpc-endpoint vpce-0a19b62753931e56b interface ethernet1/1.200
    ```
  - To verify that the association worked, run the command `show plugins vm_series aws gwlb ` if it doesn't return a list of the endpoint ids just entered verify that you ran the commit step after creating the subinterfaces. You will need to run the *request* commands again after the commit.

    ```
    admin@ip-10-0-2-78> show plugins vm_series aws gwlb

    GWLB enabled    :    True
    Overlay Routing :    False
    ================================================
    VPC endpoint              Interface      
    ================================================
    vpce-0a82e2729f61fa85a    ethernet1/1.100
    vpce-029230815a33bb5f8    ethernet1/1.100
    vpce-0a8650a40b422072e    ethernet1/1.200
    vpce-0a19b62753931e56b    ethernet1/1.200


  - **Repeat** the above steps for the second appliance. 
  #### Test that traffic is being associated with the subinterfaces
  - Initiate some traffic by using the public endpoint for the website in VPC1. Refresh the page a few times, pausing for 30 seconds between a few of them to get the . 
  - return to the Palo Alto Admin console
  - From the monitor tab enter a filter that matches the **remote IP** found on the webpage, such as ` (addr in 205.251.233.183)`
  - Click on the details icon to the left of the most recent entry and view the source and destination **Interfaces**. If everything is configured correctly you will now see the subinterface **ethernet1/1.100** being used for Internet to VPC1 traffic.
   ![verify subinterface](/images/pa-subinterface-monitor.png)


### You have completed the Isolation of the two types of traffic.
Its important to note that Palo Alto has a central management tool, **Panorama**, that can manage your fleet of appliances without having to reenter the commands on each instance independently, but we looked behind the scenes at what is configured on each appliance. Also the above *request* statements can be placed into the bootstrap files of the appliances. You will recall the bootstrap.xml that was placed in S3 at the beginning of the workshop. That file was used to configure the fleet of appliances; however at the time we didn't have the Endpoints created so didn't have IDs to place in them at that point in the lab. 
#### More from Palo Alto's website
[Panorama info](https://www.paloaltonetworks.com/network-security/panorama)
[Palo Alto and AWS GWLB Configuration](https://docs.paloaltonetworks.com/vm-series/10-0/vm-series-deployment/set-up-the-vm-series-firewall-on-aws/vm-series-integration-with-gateway-load-balancer/integrate-the-vm-series-with-an-aws-gateway-load-balancer/associate-a-vpc-endpoint-with-a-vm-series-interface.html)
