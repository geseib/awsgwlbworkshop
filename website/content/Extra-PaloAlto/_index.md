+++
menutitle = "Extra-Palo Alto"
chapter = false
weight = 50
+++

# Palo Alto?
While this workshop focus on the AWS infrastrucutre and traffic flow, its always good to have some basic understanding of whats going on in the middle. In real-world deployments cusotmers would use a management solution to provide a single pane of glass and control across multiple Palo Alto appliances. In this lab we will configure and view each of the two independently. This mean we will repeat the tasks for each firewall.

### Set Admin Password
- Launch **AWS CloudShell**:
  - From the console search for and select  **AWS CloudShell**. After a minute or so a new cloud based shell environment will launch, allowing you to use AWS cli tools and other bash based tools like dig
   ![Cloudshell](/images/gwlb-cloudshell-open.png)
- Upload pem file to cloudshell:
  - From the **Actions** menu in the upper right, select **Upload file**. ![Upload](/images/gwlb-cloudshell-upload-menu.png)
  - Click the **Select file** button.
  - Search for a select the **.pem** file you download during **Lab 1**. Click the **Open** button. ![Install](/images/gwlb-cloudshell-selectFile.png)
  - Move and secure the keym (replace with the name of your key file)
    - run `mkdir ~/.ssh`
    - run `mv key.pem ~/.ssh/`
    - run `chmod 400 ~.ssh/`
    ![Cloudshell](/images/gwlb-cloudshell-key-move.png)
- Install tools
  - run `sudo yum install -y bind-utils`
- SSH into the first Palo Alto
  - the SSH command can be found in the **Cloudformation** console. Select the **stack** and change to the **outputs** tab. ***there will be two ssh commands, one for each appliance***
  - run the copied command, `ssh -i .ssh/***your_key*** admin@***Public_IP_of_the_Palo***
  - ***Note: you may see that someone attempted to login into your appliance if you did not lock down the CIDR for management when deploying the stack. If you want to change this, you can edit the Secuirty group, from the EC2 console, that contains **xVMSeriesManagementSecurityGroup** in the name.***
- Enter configuration mode
  - run  `configure`
- Set admin password
  - run `set mgt-config users admin password`
  - at the **enter password** prompt enter a new password (don t forget it, or you will have to do this step again). Requires 
  - enter the password again at the **Confirm Password** prompt to confirm

### Login to the Palo Alto Console
- the Palo Alto URL can be found in the **Cloudformation** console. Select the **stack** and change to the **outputs** tab. ***there will be two URLs, one for each appliance***
- From your browser go to the copied URL, and login with user **admin** and the password you just set.
![PaloAlto Console](/images/gwlb-palo-login.png)
  - ***Note: since we have not loaded a certificate with a varifyable name, the session will have to proceed without Validation. With Google Chrome this means clicking the **Advanced** button and click the **proceed to ***your_Firewalls_IP_Address*** link.

### Repeat the above steps for the second appliance listed in the **Cloudformation** Stack's **Output**

## What to Look at in the Palo Alto
### Monitoring
We can take a look at the traffic that reaches the firewall and understand what action (allow/deny), what rule caused the action
- Find out your IP address, by browsing to the VPC1 Web Application. Remember its behind a Load balancer. It will indicate your IP address as the **Remote ip**. Record that, 
![VPC 1 Website](/images/test-PA-header.png)
- Switch to the **Monitor** tab at the top
- In the Queury dialog bar, enter the following: `(addr in 205.251.233.178)` and press the arrow to the right. ***Note: since the traffic will be distributed across both Palo Alto, you should perform the query on both appliances.***

## Modifying the Header from the Palo Alto
- From the Palo ALto admin Browser console, let's look at the **Policies** by selecting the **Policies** tab and looking at the **Security** policy rules.
![Policies](/images/PA-policies.png)
- From the list of policies click on the **Allow All** rule and switch over to the **Actions** tab. Notice the **Profile Setting** has a **URL Filtering** object attached called **gwlb**.
- Cancel out of the **Security Policy Rule** dialog box, and switch to the **Objects** tab at the top of the console. From the left menu select **URL Filtering** from the **Secuirty Profiles** section.
- From the list of **URL Filtering** profiles, in the **NAME** column click on **gwlb**
![Filter](/images/PA-filter.png)
- In the **URL Filtering Profile** dialog box, switch to the **HTTP Header Insertion** tab.

- In the **NAME** column click on **gwlb** to bring up the **HTTP Header Insertion** dialog box. 
![Header Insertion](/images/PA-header-insertion.png)
- In the **VALUE** column, click **YES** change this to the IP address of this Firewall. 
![Header](/images/PA-header.png)
- Edit the **Value** to send back a unique identifier such as the IP address of the firewall and click the **OK** button. 
- At the **HTTP Header Insertion** dialog box click **OK** button again to save changes.
- At the **URL Filtering Profile** dialog box click **OK** button again to save changes.
- In order to push these changes to the running Policies, click the **Commit** button at the top right of the console page. 
- Repeat these steps for the second appliance using a unique identifier for its header insertion. ***Note: be sure to Commit the changes for them to take effect***






## Central Inspection with sepration of traffic
This encapsulation allows us to create virtual interfaces on our appliances and apply different sets of policies for them.
See the Palo Alto [documentation](https://docs.paloaltonetworks.com/vm-series/10-0/vm-series-deployment/set-up-the-vm-series-firewall-on-aws/vm-series-integration-with-gateway-load-balancer/integrate-the-vm-series-with-an-aws-gateway-load-balancer/associate-a-vpc-endpoint-with-a-vm-series-interface.html) for more details on how this is done.

### Genenve Encapsulation
By encapsulating the traffic to the target, GWLB can provide seperation and some addition information about which GWLBe the traffic came from. This allows the return traffic to also go back to the correct GWLBe. 

For more details, read Milind Kularni's Blog, [Integrate your custom logic or appliance with AWS Gateway Load Balancer](https://aws.amazon.com/blogs/networking-and-content-delivery/integrate-your-custom-logic-or-appliance-with-aws-gateway-load-balancer/)

### Seperate policies for VPC-to-VPC and Internet-to-VPC1
In order to create these sperate policies, we need to let the appliance know to use the information in the GENEVE encapsultation to associate the GWLBE where the traffic comes from with a virtual interface called **subinterface**

#### Subinterfaces in Palo Alto VM-Series
We will create subinterfaces on the security appliance. Think of this as a way to seperate out traffic to use different security policies for each of our types of traffic. In our case we want:
  1. one subinterface for VPC1 internet traffic
  1. one interface for the VPC to VPC traffic. 
All of this traffic will arrive on the same ENI on the EC2 instance of the Palo Alto Firewall, but we will use the GENEVE header to assoicate with the correct subinterfaces in a later step (**Associate GWLB Endpoints with a Subinterface**). 
- From the Palo Alto VM_Series web interface, switch to the **Network** tab. Verify you are on  **Interfaces** on the left menu.
- Click on and highlight Interface **ethernet1/1**. Click the **+Add Subinterface** button at the bottom of the screen
- In the Layer3 Subinterface** dialog box change the subinterface number from (1-9999) to **100** and also for the **Tag** ***Note: the Tag is not actually used in this case, but is a required field)

#### Record the GWLB Endspoint IDs
Since we want to associate the VPC1 GWLB Endpoints with the first subinterface (for VPC1's internet traffic) and and Security VPC's GWLB Endpoints with the second subinterface (for VPC to VPC traffic), we need to get the IDs.
- From the VPC Console, 
- 

#### Associate GWLB Endpoints with a Subinterface
You can associate multiple GWLB endpoints to a single subinterface on the VM-Series firewall. However, you must associate each GWLB endpoint individually. For example, to associate GWLB endpoint in **VPC1 AZ a** and GWLB endpoint in VPC1 AZ b with subinterface ethernet1/1.100, you must execute the association command twice, separately for each GWLB endpoint. We will need to do the assoication from CLI using SSH. lets go back to Cloudshell
- Launch **AWS CloudShell**:
  - From the console search for and select  **AWS CloudShell**. After a minute or so a new cloud based shell environment will launch, allowing you to use AWS cli tools and other bash based tools like dig
   ![Cloudshell](/images/gwlb-cloudshell-open.png)
- SSH into the first Palo Alto
  - the SSH command can be found in the **Cloudformation** console. Select the **stack** and change to the **outputs** tab. ***there will be two ssh commands, one for each appliance***







