+++
menutitle = "Create GWLB"
chapter = false
weight = 60
+++

## Create a Gateway Load Balancer and Target Group for Palo Altos

Now lets depoly a Gateway Load Balancer and a Target Group with the two Palo Alto EC2 instances as targets. These will be created in the Security VPC. We will create the GWLB and place it in the two **Data** Subnets

![GLB Diagram](/images/gwlb-creategwlb.png)

#### Steps

1. From the **Amazon EC2** console and from the left menu, near the bottom, select **Load Balancers**. Click the **Create Load Balancer** button.
   ![GWLBs](/images/gwlb-gwlbs.png)

1. Click the Center **Create** button for the **Network Load Balancer**.

   ![Select NLB](/images/gwlb-select.png)
1. Create an **Gateway Load Balancer** with the following:
   - **Name**: name it, like **gwlblab-gwlb-FWs**.
   - **VPC**: select the Security VPC, ***stackName*** **-FWVPC** from the dropdown list.
   - **Subnet**: Check both **Availability Zones** and Select both of the data subnets **FWVPC-Data-AZ1** and **FWVPC-Data-AZ2**. *make sure you selected the Data Subnets.* 
   - Click the **Next Confgiure Routing**
   ![create GWLB](/images/gwlb-config.png)
1. **Configure Routing** with the following:
   - For **Target Group** select **New target group**. Give it a **Name** like ***stackName*** **-FWVPC-Firewall-TG** 
   - Leave **Target type** set to **Instance**.
   - Notice the **Protocol** is fixed as **GENEVE: 6081** *as info, this is port 6081 over UDP.*
   - Leave the **Health checks** setting at **TCP** and the advanced health check setting at their defaults as well. Note the health check is not inline (in that it uses a different port, TCP 80).
   - Click the **Next: Register Targets** button.
   ![Routing](/images/gwlb-routing.png)

1. **Register Targets** with the following:
   - Check the box next to both **Instances** and click the **Add to registered** button. Make sure both instances appear in the top section, **Registered Instances** before proceeding.
   - Click the **Next: Review** button.
   ![Targets](/images/gwlb-targets.png)

1. Review and verify the setting are correct and click the **Create** button at the lower right.

1. Enable **Cross-zone load balancing** on the Gateway load balancer.
  - From the **Amazon EC2** console and from the left menu, near the bottom, select **Load Balancers**.
  - Check the box next to the Gateway load balancer just created. 
  - Scroll down to the bottom of the page and click **Edit attributes** under the **Attributes** section of the **Description** tab.
  - Check the **Enable** box next to **Cross-zone load balancing**** *note the potential charges that may be incured for cross zone traffic*. Click the **Save** button.
   ![Cross zone](/images/gwlb-crosszone.png)
  - Select **Create** to deploy content.


1. On the **Review** screen, review the configuration, and click **Create**. After a few seconds the Gateway Load Balancer should start provisioning. This will take ~30-90 seconds. 

### Troubleshooting
1. Verify that the Target Group is connected to the EC2 Instance.
  - From the **Amazon EC2** console and from the left menu, near the bottom, select **Target Groups**. Check the box next to the Target group you created above. Click the **Targets** tab.
  - The two instances should be listed here (if not click the register targets button and add them). 
  - The two instances will cycle through initial, unhealthy, and healthy. It may take 3-4 minutes before it is in the healthy state.

     ![Targets](/images/gwlb-verify-tg.png)


   ## You have created the Gateway Load Balancer.